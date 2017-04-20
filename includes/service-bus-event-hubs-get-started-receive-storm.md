## <a name="receive-messages-with-apache-storm"></a>Ontvangen berichten met Apache Storm

[**Apache Storm**](https://storm.incubator.apache.org) is een gedistribueerde real-time berekening systeem dat betrouwbare verwerking van niet-gebonden gegevensstreams vereenvoudigt. In dit gedeelte ziet u hoe met behulp van een gebeurtenis Hubs Storm spout ontvangen van gebeurtenissen van gebeurtenis Hubs. Apache Storm kunt u gebeurtenissen opsplitsen in meerdere processen die zijn ondergebracht in verschillende knooppunten. De gebeurtenis Hubs integratie met Storm vereenvoudigt de gebeurtenis verbruik door transparant controlepuntbeheer de voortgang van Storm Zookeeper-installatie, het beheren van permanente controlepunten en parallelle ontvangt van de gebeurtenis Hubs.

Voor meer informatie over de gebeurtenis Hubs patronen ontvangt, Zie het [overzicht van de gebeurtenis Hubs][].

In deze zelfstudie wordt een [HDInsight Storm][] -installatie wordt geleverd met de gebeurtenis Hubs spout al beschikbaar.

1. Volg de procedure [HDInsight Storm - aan de slag](../articles/hdinsight/hdinsight-storm-overview.md) te maken van een cluster met nieuwe HDInsight en een verbinding maken via Extern bureaublad.

2. Kopie de `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` bestand op uw lokale ontwikkelomgeving. Deze bevat gebeurtenissen-storm-spout.

3. Gebruik de volgende opdracht als u het pakket installeren in het archief van lokale Maven. Hierdoor kunt u deze toevoegen als een verwijzing in de Storm-project in een latere stap.

        mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar

4. Maak een nieuw project van Maven in Eclips, (Klik op **bestand**, **Nieuw**, vervolgens de **Project**).

    ![][12]

5. **Gebruik werkruimte standaardlocatie**selecteren en klik op **volgende**

6. Het archetype **maven-archetype-quickstart** selecteren en klik op **volgende**

7. Een **groeps-id** en **ArtifactId**invoegen en klik vervolgens op **Voltooien**

8. **Pom.xml**, voeg de volgende afhankelijkheden in de `<dependency>` knooppunt.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-core</artifactId>
            <version>0.9.2-incubating</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.microsoft.eventhubs</groupId>
            <artifactId>eventhubs-storm-spout</artifactId>
            <version>0.9</version>
        </dependency>
        <dependency>
            <groupId>com.netflix.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>1.3.3</version>
            <exclusions>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
            <scope>provided</scope>
        </dependency>

9. Maak een bestand genaamd **Config.properties** in de map **src** en kopieer de volgende inhoud, vervangen door de volgende waarden:

        eventhubspout.username = ReceiveRule

        eventhubspout.password = {receive rule key}

        eventhubspout.namespace = ioteventhub-ns

        eventhubspout.entitypath = {event hub name}

        eventhubspout.partitions.count = 16

        # if not provided, will use storm's zookeeper settings
        # zookeeper.connectionstring=localhost:2181

        eventhubspout.checkpoint.interval = 10

        eventhub.receiver.credits = 10

    De waarde voor **eventhub.receiver.credits** bepaalt het aantal gebeurtenissen batch worden verwerkt voordat ze op de pijpleiding Storm. Omwille van de eenvoud wordt in dit voorbeeld deze waarde ingesteld op 10. In de productie, moet het gewoonlijk worden ingesteld op hogere waarden; bijvoorbeeld: 1024.

10. Maak een nieuwe klasse **LoggerBolt** aangeroepen met de volgende code:

        import java.util.Map;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import backtype.storm.task.OutputCollector;
        import backtype.storm.task.TopologyContext;
        import backtype.storm.topology.OutputFieldsDeclarer;
        import backtype.storm.topology.base.BaseRichBolt;
        import backtype.storm.tuple.Tuple;

        public class LoggerBolt extends BaseRichBolt {
            private OutputCollector collector;
            private static final Logger logger = LoggerFactory
                      .getLogger(LoggerBolt.class);

            @Override
            public void execute(Tuple tuple) {
                String value = tuple.getString(0);
                logger.info("Tuple value: " + value);

                collector.ack(tuple);
            }

            @Override
            public void prepare(Map map, TopologyContext context, OutputCollector collector) {
                this.collector = collector;
                this.count = 0;
            }

            @Override
            public void declareOutputFields(OutputFieldsDeclarer declarer) {
                // no output fields
            }

        }

    Deze bout Storm logboekregistratie van de inhoud van de gebeurtenissen ontvangen. Dit kan eenvoudig worden uitgebreid voor tuples opslaan in een storage-service. De [HDInsight sensor analyse zelfstudie] gebruikt deze dezelfde aanpak voor het opslaan van gegevens in HBase.

11. Maak een klasse met de naam **LogTopology** met de volgende code:

        import java.io.FileReader;
        import java.util.Properties;
        import backtype.storm.Config;
        import backtype.storm.LocalCluster;
        import backtype.storm.StormSubmitter;
        import backtype.storm.generated.StormTopology;
        import backtype.storm.topology.TopologyBuilder;
        import com.microsoft.eventhubs.samples.EventCount;
        import com.microsoft.eventhubs.spout.EventHubSpout;
        import com.microsoft.eventhubs.spout.EventHubSpoutConfig;

        public class LogTopology {
            protected EventHubSpoutConfig spoutConfig;
            protected int numWorkers;

            protected void readEHConfig(String[] args) throws Exception {
                Properties properties = new Properties();
                if (args.length > 1) {
                    properties.load(new FileReader(args[1]));
                } else {
                    properties.load(EventCount.class.getClassLoader()
                            .getResourceAsStream("Config.properties"));
                }

                String username = properties.getProperty("eventhubspout.username");
                String password = properties.getProperty("eventhubspout.password");
                String namespaceName = properties
                        .getProperty("eventhubspout.namespace");
                String entityPath = properties.getProperty("eventhubspout.entitypath");
                String zkEndpointAddress = properties
                        .getProperty("zookeeper.connectionstring"); // opt
                int partitionCount = Integer.parseInt(properties
                        .getProperty("eventhubspout.partitions.count"));
                int checkpointIntervalInSeconds = Integer.parseInt(properties
                        .getProperty("eventhubspout.checkpoint.interval"));
                int receiverCredits = Integer.parseInt(properties
                        .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                    // (opt)
                System.out.println("Eventhub spout config: ");
                System.out.println("  partition count: " + partitionCount);
                System.out.println("  checkpoint interval: "
                        + checkpointIntervalInSeconds);
                System.out.println("  receiver credits: " + receiverCredits);

                spoutConfig = new EventHubSpoutConfig(username, password,
                        namespaceName, entityPath, partitionCount, zkEndpointAddress,
                        checkpointIntervalInSeconds, receiverCredits);

                // set the number of workers to be the same as partition number.
                // the idea is to have a spout and a logger bolt co-exist in one
                // worker to avoid shuffling messages across workers in storm cluster.
                numWorkers = spoutConfig.getPartitionCount();

                if (args.length > 0) {
                    // set topology name so that sample Trident topology can use it as
                    // stream name.
                    spoutConfig.setTopologyName(args[0]);
                }
            }

            protected StormTopology buildTopology() {
                TopologyBuilder topologyBuilder = new TopologyBuilder();

                EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
                topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                        spoutConfig.getPartitionCount()).setNumTasks(
                        spoutConfig.getPartitionCount());
                topologyBuilder
                        .setBolt("LoggerBolt", new LoggerBolt(),
                                spoutConfig.getPartitionCount())
                        .localOrShuffleGrouping("EventHubsSpout")
                        .setNumTasks(spoutConfig.getPartitionCount());
                return topologyBuilder.createTopology();
            }

            protected void runScenario(String[] args) throws Exception {
                boolean runLocal = true;
                readEHConfig(args);
                StormTopology topology = buildTopology();
                Config config = new Config();
                config.setDebug(false);

                if (runLocal) {
                    config.setMaxTaskParallelism(2);
                    LocalCluster localCluster = new LocalCluster();
                    localCluster.submitTopology("test", config, topology);
                    Thread.sleep(5000000);
                    localCluster.shutdown();
                } else {
                    config.setNumWorkers(numWorkers);
                    StormSubmitter.submitTopology(args[0], config, topology);
                }
            }

            public static void main(String[] args) throws Exception {
                LogTopology topology = new LogTopology();
                topology.runScenario(args);
            }
        }


    Deze klasse wordt gemaakt van een nieuwe gebeurtenis Hubs spout, met behulp van de eigenschappen in de configuratie van het bestand in instatiate het. Het is belangrijk te weten in dit voorbeeld zoveel spouts taken als het aantal partities in de Hub gebeurtenis wordt de maximale parallellisme toegestaan door deze gebeurtenis Hub gebruiken.

<!-- Links -->
[Overzicht van de gebeurtenissen Hubs]: ../articles/event-hubs/event-hubs-overview.md
[HDInsight-Storm]: ../articles/hdinsight/hdinsight-storm-overview.md
[HDInsight sensor analyse zelfstudie]: ../articles/hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/service-bus-event-hubs-get-started-receive-storm/create-storm1.png