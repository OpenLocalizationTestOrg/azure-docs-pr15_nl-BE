<properties
   pageTitle="Topologieën met Visual Studio en C# Apache Storm | Microsoft Azure"
   description="Informatie over Storm topologieën in C# maken door het maken van een topologie met eenvoudige word count in Visual Studio met behulp van de HDInsight's voor Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/27/2016"
   ms.author="larryfr"/>

# <a name="develop-c-topologies-for-apache-storm-on-hdinsight-using-hadoop-tools-for-visual-studio"></a>Ontwikkelen van C# topologieën voor Apache Storm op HDInsight met Hadoop tools voor Visual Studio

Informatie over het maken van een topologie Storm C# met behulp van de HDInsight's voor Visual Studio. Deze zelfstudie begeleidt bij het proces van het maken van een nieuwe Storm-project in Visual Studio en implementatie daarvan op een Storm Apache op HDInsight cluster lokaal te testen.

U leert ook hoe hybride topologieën met C# en Java-onderdelen maken.

> [AZURE.IMPORTANT] Terwijl u de stappen in dit document zijn gebaseerd op een Windows-ontwikkelomgeving met Visual Studio, kan de gecompileerde project naar een Linux- of HDInsight op basis van het Windows-cluster worden verzonden. Alleen Linux gebaseerde clusters gemaakt na 28-10/2016 ondersteuning SCP.NET topologieën.
>
> Als u wilt een C#-topologie met een Linux-gebaseerde cluster gebruikt, moet u het pakket Microsoft.SCP.Net.SDK NuGet gebruikt door uw project naar versie 0.10.0.6 of hoger bijwerken. De versie van het pakket moet ook overeenkomen met de primaire versie van Storm geïnstalleerd op HDInsight. Gebruik bijvoorbeeld Storm op HDInsight versie 3.3 en 3.4 Storm versie 0.10.x, terwijl de HDInsight 3.5 met Storm 1.0.x.
> 
> C#-topologieën op Linux gebaseerde clusters moeten gebruik .NET 4.5, en Mono uit te voeren op de cluster HDInsight. De meeste dingen werken, echter u moet de [Compatibiliteit met Mono-](http://www.mono-project.com/docs/about-mono/compatibility/) document controleren op potentiële compatibiliteitsproblemen.

## <a name="prerequisites"></a>Vereisten

- Een van de volgende versies van Visual Studio

    - Visual Studio 2012 met [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)

    - Visual Studio 2013 met [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) of [Visual Studio 2013 Gemeenschap](http://go.microsoft.com/fwlink/?LinkId=517284)

    - Visual Studio 2015 of [Visual Studio 2015 Gemeenschap](https://go.microsoft.com/fwlink/?LinkId=532606)

- Azure SDK 2.9.5 of hoger

- Extra HDInsight voor Visual Studio: [aan de slag met de HDInsight's voor Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) wilt installeren en configureren van de HDInsight's voor Visual Studio.

    > [AZURE.NOTE] Extra HDInsight voor Visual Studio worden niet ondersteund op Visual Studio Express

-   Storm op HDInsight cluster Apache: [aan de slag met Apache Storm op HDInsight](hdinsight-apache-storm-tutorial-get-started.md) voor stapsgewijze instructies voor het maken van een cluster.

## <a name="templates"></a>Sjablonen

De HDInsight's voor Visual Studio de volgende sjablonen bieden::

| Projecttype | Laat zien |
| ------------ | ------------- |
| Storm-toepassing | Een leeg project van de Storm-topologie |
| Storm Azure SQL Writer monster | Het schrijven van Azure SQL-Database |
| Storm DocumentDB Reader monster | Lezen van Azure DocumentDB |
| Storm schrijver DocumentDB monster | Het schrijven van Azure DocumentDB |
| Storm EventHub Reader monster | Lezen van Azure gebeurtenis Hubs |
| Storm schrijver EventHub monster | Het schrijven van Azure gebeurtenis Hubs |
| Storm HBase Reader monster | Lezen van HBase op HDInsight-clusters |
| Storm HBase schrijver monster | Het schrijven van HBase op HDInsight-clusters |
| Storm hybride monster | Het gebruik van een Java-onderdeel |
| Voorbeeld van de storm | Een topologie met eenvoudige word count |

> [AZURE.NOTE] De REST HBase API HBase lezer en schrijver monsters gebruiken om te communiceren met een HBase niet de HBase Java API-cluster HDInsight.

In de stappen in dit document wordt het type standaard Storm toepassing project maakt u een nieuwe topologie.

## <a name="create-a-c-topology"></a>Een C#-topologie maken

1. Als u de meest recente versie van de HDInsight's zijn niet voor Visual Studio hebt geïnstalleerd, ziet u [aan de slag met de HDInsight's voor Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Visual Studio openen, selecteert u **bestand** > **Nieuw**en klik vervolgens op **Project**.

3. Vouw in het **Nieuwe Project** -scherm **geïnstalleerde** > **sjablonen**en selecteer **HDInsight**. Selecteer in de lijst met sjablonen, **Storm-toepassing**. Voer onderaan het scherm **WordCount** als de naam van de toepassing.

    ![afbeelding](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4. Nadat u het project hebt gemaakt, hebt u de volgende bestanden:

    - **Program.cs**: Hiermee definieert u de topologie voor uw project. Houd er rekening mee dat een standaard topologie die uit één spout en een bout bestaat wordt standaard gemaakt.

    - **Spout.cs**: een voorbeeld van de spout die willekeurige getallen genereert.

    - **Bolt.cs**: een voorbeeld van de bout waarmee het aantal cijfers dat door de spout blijft.

    Als onderdeel van het maken van het project, wordt de meest recente [SCP.NET pakketten](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/) gedownload van NuGet.

    [AZURE.INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

In de volgende secties wijzigt u dit project in een basistoepassing WordCount.

### <a name="implement-the-spout"></a>De spout implementeren

1. Open **Spout.cs**. Spouts worden gebruikt om gegevens te lezen in een topologie van een externe bron. De belangrijkste onderdelen voor een spout zijn:

    - **NextTuple**: door Storm wordt aangeroepen wanneer de spout mag nieuwe tuples uitstralen.

    - **ACK** (alleen transactionele topologie): gestart door andere onderdelen in de topologie voor tuples verzonden vanuit deze spout bevestigingen worden verwerkt. Een tupel zijn bevestigd, kunt de spout weet dat deze is verwerkt door downstream-componenten.

    - **Mislukt** (alleen transactionele topologie): tupels die zijn mislukt-verwerking van andere onderdelen in de topologie worden verwerkt. Dit biedt de mogelijkheid de tupel opnieuw verzenden, zodat deze opnieuw kan worden verwerkt.

2. De inhoud van de klasse **Spout** vervangen door het volgende. Hiermee maakt u een spout die willekeurig een zin in de topologie genereert.

        private Context ctx;
        private Random r = new Random();
        string[] sentences = new string[] {
            "the cow jumped over the moon",
            "an apple a day keeps the doctor away",
            "four score and seven years ago",
            "snow white and the seven dwarfs",
            "i am at two with nature"
        };

        public Spout(Context ctx)
        {
            // Set the instance context
            this.ctx = ctx;

            Context.Logger.Info("Generator constructor called");

            // Declare Output schema
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // The schema for the default output stream is
            // a tuple that contains a string field
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        }

        // Get an instance of the spout
        public static Spout Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Spout(ctx);
        }

        public void NextTuple(Dictionary<string, Object> parms)
        {
            Context.Logger.Info("NextTuple enter");
            // The sentence to be emitted
            string sentence;

            // Get a random sentence
            sentence = sentences[r.Next(0, sentences.Length - 1)];
            Context.Logger.Info("Emit: {0}", sentence);
            // Emit it
            this.ctx.Emit(new Values(sentence));

            Context.Logger.Info("NextTuple exit");
        }

        public void Ack(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }

        public void Fail(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }
    
    Neem even de tijd om te lezen via de opmerkingen te begrijpen wat deze code doet.

### <a name="implement-the-bolts"></a>De bouten implementeren

1. Verwijder het bestaande **Bolt.cs** -bestand uit het project.

2. In de **Solution Explorer**met de rechtermuisknop op het project en selecteer **toevoegen** > **Nieuw item**. In de lijst Selecteer **Storm bout**en **Splitter.cs** als naam invoeren. Herhaal dit als u wilt maken van een tweede bolt met de naam **Counter.cs**.

    - **Splitter.cs**: implementeert een bout die splitsen in afzonderlijke woorden en zinnen en genereert een nieuwe stroom van woorden.

    - **Counter.cs**: een bout die elk woord telt en genereert een nieuwe stroom van woorden en het aantal voor elk woord wordt geïmplementeerd.

    > [AZURE.NOTE] Deze bouten gewoon lezen en schrijven naar stromen, maar u kunt ook een bout gebruiken om te communiceren met bronnen, zoals een database of een service.

3. Open **Splitter.cs**. Opmerking dat er slechts één methode standaard: **uitvoeren**. Dit wordt genoemd als de bout een tupel voor verwerking ontvangt. Hier kunt u lezen en verwerken tuples binnenkomende en uitgaande tuples uitstralen.

4. De inhoud van de klasse **Splitter** vervangen door de volgende code:

        private Context ctx;

        // Constructor
        public Splitter(Context ctx)
        {
            Context.Logger.Info("Splitter constructor called");
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // Input contains a tuple with a string field (the sentence)
            inputSchema.Add("default", new List<Type>() { typeof(string) });
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // Outbound contains a tuple with a string field (the word)
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance of the bolt
        public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Splitter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the sentence from the tuple
            string sentence = tuple.GetString(0);
            // Split at space characters
            foreach (string word in sentence.Split(' '))
            {
                Context.Logger.Info("Emit: {0}", word);
                //Emit each word
                this.ctx.Emit(new Values(word));
            }

            Context.Logger.Info("Execute exit");
        }

    Neem even de tijd om te lezen via de opmerkingen te begrijpen wat deze code doet.

5. Open **Counter.cs** en de inhoud van de klasse vervangen door het volgende:

        private Context ctx;

        // Dictionary for holding words and counts
        private Dictionary<string, int> counts = new Dictionary<string, int>();

        // Constructor
        public Counter(Context ctx)
        {
            Context.Logger.Info("Counter constructor called");
            // Set instance context
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string field - the word
            inputSchema.Add("default", new List<Type>() { typeof(string) });

            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string and integer field - the word and the word count
            outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance
        public static Counter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Counter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the word from the tuple
            string word = tuple.GetString(0);
            // Do we already have an entry for the word in the dictionary?
            // If no, create one with a count of 0
            int count = counts.ContainsKey(word) ? counts[word] : 0;
            // Increment the count
            count++;
            // Update the count in the dictionary
            counts[word] = count;

            Context.Logger.Info("Emit: {0}, count: {1}", word, count);
            // Emit the word and count information
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
            Context.Logger.Info("Execute exit");
        }

    Neem even de tijd om te lezen via de opmerkingen te begrijpen wat deze code doet.

### <a name="define-the-topology"></a>De topologie te definiëren

Spouts en bouten worden gerangschikt in een grafiek, die bepaalt hoe gegevens worden uitgewisseld tussen de onderdelen. Binnen deze topologie is de grafiek als volgt:

![afbeelding van de rangschikking van onderdelen](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Zinnen worden uitgestoten uit de spout, die worden gedistribueerd naar exemplaren van de bout Splitter. De splitser bout breekt de zinnen in woorden, die worden verdeeld over de teller bout.

Omdat word count is die lokaal zijn opgeslagen in de item-instantie, willen we om ervoor te zorgen dat bepaalde woorden op dezelfde teller bout instantie, vloeien dus we hebben slechts één exemplaar van het bijhouden van een bepaald woord. Maar voor de bout Splitter maakt het echt niet uit welke bout ontvangt welke zin, zodat we gewoon wilt laden zinnen evenwicht tussen de instanties.

Open **Program.cs**. De belangrijke methode is **GetTopologyBuilder**, die wordt gebruikt voor het definiëren van de topologie die is ingediend voor de Storm. De inhoud van de **GetTopologyBuilder** vervangen door de volgende code voor de uitvoering van de topologie die eerder zijn beschreven:

        // Create a new topology named 'WordCount'
        TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

        // Add the spout to the topology.
        // Name the component 'sentences'
        // Name the field that is emitted as 'sentence'
        topologyBuilder.SetSpout(
            "sentences",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
            },
            1);
        // Add the splitter bolt to the topology.
        // Name the component 'splitter'
        // Name the field that is emitted 'word'
        // Use suffleGrouping to distribute incoming tuples
        //   from the 'sentences' spout across instances
        //   of the splitter
        topologyBuilder.SetBolt(
            "splitter",
            Splitter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
            },
            1).shuffleGrouping("sentences");

        // Add the counter bolt to the topology.
        // Name the component 'counter'
        // Name the fields that are emitted 'word' and 'count'
        // Use fieldsGrouping to ensure that tuples are routed
        //   to counter instances based on the contents of field
        //   position 0 (the word). This could also have been
        //   List<string>(){"word"}.
        //   This ensures that the word 'jumped', for example, will always
        //   go to the same instance
        topologyBuilder.SetBolt(
            "counter",
            Counter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
            },
            1).fieldsGrouping("splitter", new List<int>() { 0 });

        // Add topology config
        topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
        {
            {"topology.kryo.register","[\"[B\"]"}
        });

        return topologyBuilder;

Neem even de tijd om te lezen via de opmerkingen te begrijpen wat deze code doet.

## <a name="submit-the-topology"></a>De topologie indienen

1. In de **Solution Explorer**met de rechtermuisknop op het project en selecteer **indienen bij Storm op HDInsight**.

    > [AZURE.NOTE] Voer desgevraagd de inloggegevens voor uw abonnement op Azure. Als er meer dan één abonnement, aanmelden bij de database met de Storm op de cluster HDInsight.

2. Uw Storm op HDInsight cluster selecteren in de vervolgkeuzelijst **Storm Cluster** en selecteer **verzenden**. U kunt controleren als het indienen gelukt is met behulp van het venster **uitvoer** .

3. Wanneer de topologie is ingediend, weergegeven de **Storm topologieën** voor het cluster. De topologie **WordCount** selecteren uit de lijst voor informatie over de lopende topologie.

    > [AZURE.NOTE] **Storm topologieën** van **Server Explorer**kunt u ook bekijken: vouw **Azure** > **HDInsight**, met de rechtermuisknop op een Storm op de cluster HDInsight en selecteer vervolgens **Weergave Storm topologieën**.

    Gebruik de koppelingen voor het spouts of bouten voor informatie over deze onderdelen. Voor elk geselecteerd item wordt een nieuw venster geopend.

4. Klik op **verwijderen** als u wilt stoppen, de topologie uit de weergave **Samenvatting topologie** .

    > [AZURE.NOTE] Storm topologieën blijven totdat zij worden gedeactiveerd of het cluster is verwijderd.

## <a name="transactional-topology"></a>Transactionele topologie

De topologie van de vorige is niet-transactioneel. De onderdelen binnen de topologie functionaliteit voor het afspelen van berichten als de verwerking mislukt door een onderdeel in de topologie niet geïmplementeerd. Voor een transactionele topologie, maak een nieuw project en selecteer **Monster Storm** als het projecttype.

Transactionele topologieën implementeren het volgende ter ondersteuning van replay van gegevens:

- **Metagegevens in cachegeheugen plaatsen**: de spout metagegevens over de uitgestoten, zodat de gegevens worden opgehaald en opnieuw gegenereerd als er een storing optreedt gegevens moet opslaan. Omdat de gegevens verstrekt door het monster te klein is, is de onbewerkte gegevens voor elke tupel in een woordenboek voor replay opgeslagen.

- **ACK**: elke bout in de topologie kunt bellen `this.ctx.Ack(tuple)` voor ack een tupel is met succes verwerkt. Wanneer alle bouten bevestigd de tupel zijn, de `Ack` methode van de spout wordt aangeroepen. Hierdoor kan de spout gegevens in de cache voor replay verwijderen omdat de gegevens volledig is verwerkt.

- **Mislukt**: elke bout kunt bellen `this.ctx.Fail(tuple)` om aan te geven dat de verwerking is mislukt voor een tupel. De fout wordt doorgegeven aan de `Fail` methode van de spout, waar de tupel kan worden opnieuw afgespeeld met behulp van metagegevens in de cache opgeslagen.

- **Reeks-ID**: een reeks-ID kunt opgeven bij het genereren van een tupel. Dit moet een waarde die de tupel voor verwerking van replay (Ack en mislukken aangeeft). De spout in het voorbeeldproject **Storm** gebruikt bijvoorbeeld het volgende bij het genereren van gegevens:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    Dit genereert een nieuwe tuple die een zin als de standaardstream met de reeks id-waarde in **lastSeqId bevat**. In dit voorbeeld wordt gewoon **lastSeqId** verhoogd voor elke tupel uitgestoten.

Zoals u in het voorbeeldproject **Storm** , kan of een onderdeel is transactionele worden ingesteld tijdens runtime, op basis van de configuratie.

## <a name="hybrid-topology"></a>Hybride topologie

HDInsight's voor Visual Studio kunnen ook worden gebruikt hybride topologieën, waarbij sommige onderdelen zijn C# en anderen Java maken.

Maak een nieuw project voor een hybride topologie, en selecteer **Storm hybride monster**. Hiermee maakt u een voorbeeld van een volledig commentaar dat verschillende topologieën die aantonen het volgende dat bevat:

- **Spout Java** en **C# bout**: gedefinieerd in **HybridTopology_javaSpout_csharpBolt**

    - Een transactionele versie is gedefinieerd in **HybridTopologyTx_javaSpout_csharpBolt**

- **C# spout** en **Java bout**: gedefinieerd in **HybridTopology_csharpSpout_javaBolt**

    - Een transactionele versie is gedefinieerd in **HybridTopologyTx_csharpSpout_javaBolt**

        > [AZURE.NOTE] Deze versie wordt ook beschreven hoe Clojure code uit een tekstbestand gebruiken als onderdeel van de Java.

Als u wilt schakelen tussen de topologie die wordt gebruikt wanneer het project wordt ingediend, verplaatst de `[Active(true)]` -instructie aan de topologie die u gebruiken wilt voordat deze worden ingediend met het cluster.

> [AZURE.NOTE] Alle Java-bestanden die nodig zijn worden geleverd als onderdeel van dit project in de map **JavaDependency** .

Overweeg het volgende bij het maken en indienen van een topologie hybride:

- **JavaComponentConstructor** moet worden gebruikt voor het maken van een nieuw exemplaar van de Java-klasse voor een spout of bout.

- **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** moet worden gebruikt voor het serialiseren van gegevens in de aan- of Java-onderdelen van Java-objecten naar JSON.

- Bij de indiening van de topologie van de server, moet u de optie **aanvullende configuraties** opgeven van de **Java-bestandspaden**. Het opgegeven pad moet de map waarin de JAR-bestanden met de Java-klassen.

### <a name="azure-event-hubs"></a>Azure gebeurtenis Hubs

SCP.Net versie 0.9.4.203 bevat een nieuwe klasse en een methode speciaal voor het werken met de gebeurtenis Hub Spout (een Java-spout die van Hub gebeurtenis.) Bij het maken van een topologie die deze spout wordt gebruikt, moet u de volgende methoden gebruiken:

- **EventHubSpoutConfig** -klasse: Hiermee maakt u een object met de configuratie van het onderdeel spout

- **TopologyBuilder.SetEventHubSpout** , methode: de gebeurtenis Hub Spout component toevoegen aan de topologie

> [AZURE.NOTE] Terwijl deze gemakkelijker werken met de gebeurtenis Hub Spout dan andere Java-onderdelen, moet u de CustomizedInteropJSONSerializer gegevens geproduceerd door de spout serialiseren.

## <a id="configurationmanager"></a>Met behulp van ConfigurationManager

Niet met ConfigurationManager waarden ophalen uit bout en spout onderdelen; Dit zal leiden tot een uitzondering null-aanwijzer. In plaats daarvan wordt de configuratie voor uw project in de Storm-topologie doorgegeven als een sleutel/waarde-paar in de context van de topologie. Elk onderdeel dat is gebaseerd op de waarden van de systeemconfiguratie moet terugzetten uit de context tijdens de initialisatie.

De volgende code toont hoe u deze waarden ophaalt:

    public class MyComponent : ISCPBolt
    {
        // To hold configuration information loaded from context
        Configuration configuration;
        ...
        public MyComponent(Context ctx, Dictionary<string, Object> parms)
        {
            // Save a copy of the context for this component instance
            this.ctx = ctx;
            // If it exists, load the configuration for the component
            if(parms.ContainsKey(Constants.USER_CONFIG))
            {
                this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
            }
            // Retrieve the value of "Foo" from configuration
            var foo = this.configuration.AppSettings.Settings["Foo"].Value;
        }
        ...
    }

Als u een `Get` methode om terug te keren een exemplaar van de component, moet u ervoor zorgen dat dit wordt doorgegeven zowel de `Context` en `Dictionary<string, Object>` parameters voor de constructor. In het volgende voorbeeld wordt een basic `Get` methode die deze waarden correct doorgegeven:

    public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new MyComponent(ctx, parms);
    }

## <a name="how-to-update-scpnet"></a>Het bijwerken van SCP.NET

Recente versies van SCP.NET ondersteunen upgrade pakket via NuGet. Wanneer een nieuwe update beschikbaar is, ontvangt u een melding van een upgrade. Handmatig om te controleren of een upgrade, moet u deze stappen uitvoeren:

1. In de **Solution Explorer**met de rechtermuisknop op het project en selecteer **NuGet pakketten beheren**.

2. Selecteer de **Updates**van het Pakketbeheer. Als een update beschikbaar is, wordt deze weergegeven. Klik op de knop **bijwerken** voor het pakket te installeren.

> [AZURE.IMPORTANT] Als het project is gemaakt met een van de eerdere versies van de SCP.NET die NuGet package updates niet hebt gebruikt, moet u de volgende stappen uit om bij te werken naar de nieuwe versie uitvoeren:
>
> 1. In de **Solution Explorer**met de rechtermuisknop op het project en selecteer **NuGet pakketten beheren**.
> 2. Met het veld **Zoeken** , zoeken en toevoegen, **Microsoft.SCP.Net.SDK** aan het project.

## <a name="troubleshooting"></a>Het oplossen van problemen

### <a name="null-pointer-exceptions"></a>Null-pointer uitzonderingen

Als u een C#-topologie met een HDInsight op basis van Linux-cluster, bout en spout onderdelen die ConfigurationManager gebruiken voor het lezen van configuratie-instellingen in runtime mogelijk resultaat null-pointer uitzonderingen. Dit gebeurt omdat de configuratie voor het domein geladen is niet uit de assembly met uw project.

De configuratie voor uw project in de Storm-topologie wordt doorgegeven als een sleutel/waarde-paar in de context van de topologie en kan worden opgehaald uit het dictionary-object dat wordt doorgegeven aan de onderdelen wanneer deze worden geïnitialiseerd.

Het volgende voorbeeld worden de waarden van de systeemconfiguratie wordt geladen vanuit de context van de topologie, raadpleegt u de sectie [ConfigurationManager](#configurationmanager) van dit document.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Als u een C#-topologie met een HDInsight op basis van Linux-cluster, kan de volgende fout optreden:

    System.TypeLoadException: Failure has occurred while loading a type.

Dit meestal occurrs wanneer u een binair bestand dat is niet compatibel met de versie van .NET die mono ondersteunt.

Voor HDInsight op basis van Linux-clusters, moet u ervoor zorgen dat uw project .NET 4.5 gecompileerde binaire bestanden gebruikt.


### <a name="test-a-topology-locally"></a>Een topologie lokaal testen

Hoewel het is eenvoudig te implementeren een topologie met een cluster, in sommige gevallen wellicht moet u een topologie lokaal te testen. Gebruik de volgende stappen uit te voeren en de topologie van het voorbeeld in deze zelfstudie lokaal in uw ontwikkelomgeving te testen.

> [AZURE.WARNING] Lokale test werkt alleen voor basic, C# alleen topologieën. Gebruik niet lokaal testen voor hybride topologieën of topologieën met meerdere streams als er fouten optreden.

1. In de **Solution Explorer**met de rechtermuisknop op het project en selecteer **Eigenschappen**. Wijzig het **type uitvoer** **Consoletoepassing**in de Projecteigenschappen van het.

    ![het uitvoertype](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

    > [AZURE.NOTE] Vergeet niet om te wijzigen in het **type uitvoer** **Class-bibliotheek** voordat u de topologie op een cluster implementeren.

2. In de **Solution Explorer**met de rechtermuisknop op het project en selecteer vervolgens **toevoegen** > **Nieuw Item**. **Klasse** inschakelt en **LocalTest.cs** als de naam van de klasse. Ten slotte klikt u op **toevoegen**.

3. **LocalTest.cs** openen en de volgende instructie met **behulp van** boven toevoegen:

        using Microsoft.SCP;

4. Als de inhoud van de klasse **LocalTest** , gebruikt u het volgende:

        // Drives the topology components
        public void RunTestCase()
        {
            // An empty dictionary for use when creating components
            Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

            #region Test the spout
            {
                Console.WriteLine("Starting spout");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext spoutCtx = LocalContext.Get();
                // Get a new instance of the spout, using the local context
                Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

                // Emit 10 tuples
                for (int i = 0; i < 10; i++)
                {
                    sentences.NextTuple(emptyDictionary);
                }
                // Use LocalContext to persist the data stream to file
                spoutCtx.WriteMsgQueueToFile("sentences.txt");
                Console.WriteLine("Spout finished");
            }
            #endregion

            #region Test the splitter bolt
            {
                Console.WriteLine("Starting splitter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext splitterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);


                // Set the data stream to the data created by the spout
                splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    splitter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                splitterCtx.WriteMsgQueueToFile("splitter.txt");
                Console.WriteLine("Splitter bolt finished");
            }
            #endregion

            #region Test the counter bolt
            {
                Console.WriteLine("Starting counter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext counterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Counter counter = Counter.Get(counterCtx, emptyDictionary);

                // Set the data stream to the data created by splitter bolt
                counterCtx.ReadFromFileToMsgQueue("splitter.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    counter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                counterCtx.WriteMsgQueueToFile("counter.txt");
                Console.WriteLine("Counter bolt finished");
            }
            #endregion
        }

    Neem even de tijd om te lezen via de commentaar bij de code. Deze code **LocalContext** gebruikt voor het uitvoeren van de onderdelen in de ontwikkelomgeving en de gegevensstroom tussen de onderdelen van de bestanden op de lokale schijf blijft bestaan.

5. **Program.cs** Open en voeg het volgende toe aan de **Main** -methode:

        Console.WriteLine("Starting tests");
        System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
        // Initialize the runtime
        SCPRuntime.Initialize();

        //If we are not running under the local context, throw an error
        if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
        {
            throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
        }
        // Create test instance
        LocalTest tests = new LocalTest();
        // Run tests
        tests.RunTestCase();
        Console.WriteLine("Tests finished");
        Console.ReadKey();

6. Sla de wijzigingen op, en vervolgens klikt u op **F5** of selecteer **Debug** > **Foutopsporing starten** om het project te starten. Een consolevenster moet worden weergegeven en status in logboek registreren als de voortgang van de tests. Wanneer de **dat tests voltooid** wordt weergegeven, drukt u op een willekeurige toets om het venster te sluiten.

7. Zoek de map met het project, bijvoorbeeld via **Windows Verkenner** **C:\Users\<gebruikersnaam > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. In deze map, open **opslaglocatie**en klik vervolgens op **fouten opsporen**. Ziet u de bestanden die zijn gemaakt bij de tests worden uitgevoerd: sentences.txt, counter.txt en splitter.txt. Elk tekstbestand openen en controleren van de gegevens.

    > [AZURE.NOTE] Gegevens wordt als een matrix van decimale waarden in deze bestanden behouden. Bijvoorbeeld: \[[97,103,111]] in het **splitter.txt** bestand is het woord "en".

Hoewel een eenvoudige word testen tellen is toepassing lokaal heel eenvoudig, de reële waarde wordt geleverd wanneer u een complexe topologie die communiceert met externe gegevensbronnen of complexe analyses uitvoert. Wanneer u aan een dergelijk project werkt, moet u mogelijk onderbrekingspunten kunnen instellen en stap voor stap de code in uw onderdelen voor het isoleren van problemen.

> [AZURE.NOTE] Zorg ervoor dat het **type Project** weer instellen op **Class Library** voordat een Storm op de cluster HDInsight.

### <a name="log-information"></a>Logboekgegevens

U kunt gemakkelijk informatie vanaf uw topologie onderdelen registreren met behulp van `Context.Logger`. Bijvoorbeeld maakt de volgende een logboek van informatie:

    Context.Logger.Info("Component started");

Geregistreerde gegevens kan worden weergegeven in het **Logboek van Hadoop-Service**, dat is gevonden in **Server Explorer**. Vouw de vermelding voor de Storm op de cluster HDInsight uit en vouw vervolgens **Hadoop-logboek**. Ten slotte selecteert u het logboekbestand weer te geven.

> [AZURE.NOTE] De logboekbestanden worden opgeslagen in de opslag Azure-account die wordt gebruikt door het cluster. Als dit een ander abonnement dan die u bent aangemeld bij met Visual Studio, moet u zich aanmelden bij het abonnement met de opslag-account om deze gegevens weer te geven.

### <a name="view-error-information"></a>Foutgegevens weergeven

Om fouten te bekijken die hebben plaatsgevonden in een topologie uitgevoerd, gebruik de volgende stappen uit:

1. In **Server Explorer**met de rechtermuisknop op de Storm op de cluster HDInsight en selecteer **View Storm topologieën**.

2. De **Laatste fout** kolom hebt voor **Spout** en **Bolts**, informatie over de laatste fout die is opgetreden.

3. Selecteer de **Id Spout** **Bout-Id** voor het onderdeel dat er een fout weergegeven. Op de pagina die wordt weergegeven, worden aanvullende foutgegevens worden vermeld in de sectie **fouten** onder aan de pagina.

4. Als u meer informatie, een **poort** te selecteren in de sectie **Executors** van de pagina voor een overzicht van het logboek Storm werknemer voor de laatste paar minuten.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe te ontwikkelen en implementeren van topologieën Storm uit de HDInsight's voor Visual Studio, leren hoe om [gebeurtenissen](hdinsight-storm-develop-csharp-event-hub-topology.md)te verwerken van Azure gebeurtenis Hub met Storm op HDInsight.

Zie voor een voorbeeld van een C#-topologie die stream-gegevens worden gesplitst in meerdere streams [Storm C#-voorbeeld](https://github.com/Blackmist/csharp-storm-example).

Meer informatie over het maken van C# topologieën ontdekt, gaat u naar [SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Meer manieren om te werken met HDInsight en meer Storm op monsters van HDinsight, Zie het volgende:

**Microsoft SCP.NET**

* [SCP programming guide](hdinsight-storm-scp-programming-guide.md)

**Apache Storm op HDInsight**

- [Implementeren en topologieën met Apache Storm op HDInsight controleren](hdinsight-storm-deploy-monitor-topology.md)

- [Voorbeeld van de topologieën voor Storm op HDInsight](hdinsight-storm-example-topology.md)

**Apache Hadoop op HDInsight**

- [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

- [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)

- [MapReduce gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

**HBase Apache op HDInsight**

- [Aan de slag met HBase op HDInsight](hdinsight-hbase-tutorial-get-started.md)
