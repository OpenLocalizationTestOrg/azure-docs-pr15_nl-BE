<properties
    pageTitle="Intensieve Java-toepassing op een VM | Microsoft Azure"
    description="Informatie over het maken van een Azure virtuele machine die een intensieve Java-toepassing die kan worden gecontroleerd door een andere Java-toepassing wordt uitgevoerd."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Een intensieve taak uitvoeren in Java op een virtuele machine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Met Azure, kunt u een virtuele machine voor het verwerken van computer-intensieve taken. Bijvoorbeeld, een virtuele machine verwerken van taken en resultaten leveren aan clientcomputers of mobiele toepassingen. Na het lezen van dit artikel hebt u een goed begrip van het maken van een virtuele machine die een intensieve Java-toepassing die kan worden gecontroleerd door een andere Java-toepassing wordt uitgevoerd.

In deze zelfstudie wordt ervan uitgegaan dat weet hoe Java consoletoepassingen maken, bibliotheken kunt importeren in uw Java-toepassing en een archief voor Java (JAR) kunt genereren. Zonder enige kennis van Microsoft Azure wordt aangenomen.

U leert:

* Het maken van een virtuele machine met een Java Development Kit (JDK) al is geïnstalleerd.
* Informatie over het extern aan te melden uw virtuele machine.
* Het maken van een naamruimte service bus.
* Het maken van een Java-toepassing waarmee een intensieve taak wordt uitgevoerd.
* Het maken van een Java-toepassing die de voortgang van de intensieve taak controleert.
* Het uitvoeren van Java-toepassingen.
* Het beëindigen van de Java-toepassingen.

In deze zelfstudie wordt het probleem Traveling verkoper voor de intensieve taak gebruiken. Hier volgt een voorbeeld van de Java-toepassing met de intensieve taak.

![Traveling verkoper probleem Oplosser][solver_output]

Hier volgt een voorbeeld van de Java-toepassing die de taak van intensieve bewaking.

![Traveling verkoper probleem client][client_output]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Een virtuele machine maken

1. Log in op de [Azure klassieke portal](https://manage.windowsazure.com).
2. Klik op **Nieuw**, klikt u op **berekenen**, klikt u op de **virtuele machine**en klik op **Uit galerie**.
3. Selecteer in het dialoogvenster **virtuele machine afbeelding selecteren** **JDK 7 Windows Server 2012**.
Houd er rekening mee dat **JDK 6 Windows Server 2012** beschikbaar is in het geval u oudere toepassingen hebt die nog niet klaar om uit te voeren in JDK 7.
4. Klik op **volgende**.
4. Klik in het dialoogvenster **configuratie van de virtuele machine** :
    1. Geef een naam voor de virtuele machine.
    2. Geef de grootte moet worden gebruikt voor de virtuele machine.
    3. Voer een naam voor de beheerder in het veld **Gebruikersnaam** . Onthoud deze naam en het wachtwoord u vervolgens voert, u gebruikt deze wanneer u extern aanmelden bij de virtuele machine.
    4. Voer in het veld **Nieuw wachtwoord** een wachtwoord en voer deze opnieuw in het veld **bevestigen** . Dit is het wachtwoord voor de beheerdersaccount.
    5. Klik op **volgende**.
5. De volgende virtuele machine in het dialoogvenster **configuratie** :
    1. Voor **Cloud-service**, gebruikt de standaard **een nieuwe wolk service maken**.
    2. De waarde voor **Cloud service DNS-naam** moet uniek zijn in cloudapp.net. Indien nodig, deze waarde wijzigen zodat die Azure geeft aan dat deze uniek is.
    2. Geef een regio, affiniteit groep of virtueel netwerk. Geef een regio zoals **West VS**voor doeleinden van deze zelfstudie.
    2. Voor **Rekening van de opslag**, selecteert u **een automatisch gegenereerde opslag-account gebruiken**.
    3. Voor **Beschikbaarheid instellen**, selecteert u **(geen)**.
    4. Klik op **volgende**.
5. In het uiteindelijke dialoogvenster **virtuele machine configuratie** :
    1. Accepteer de standaardinstellingen voor het eindpunt.
    2. Klik op **Voltooien**.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>Op afstand aan te melden uw virtuele machine

1. Meld u op de [Azure klassieke portal](https://manage.windowsazure.com).
2. Klik op **virtuele machines**.
3. Klik op de naam van de virtuele machine die u wilt om in te loggen.
4. Klik op **verbinden**.
5. Reageren op de vragen zo nodig verbinding met de virtuele machine. Als u wordt gevraagd de naam administrator en het wachtwoord, de waarden die u hebt opgegeven bij het maken van de virtuele machine gebruiken.

Houd er rekening mee dat de Azure Service Bus functionaliteit vereist is voor het basiscertificaat Baltimore CyberTrust worden geïnstalleerd als onderdeel van de Java Runtime Environment **cacerts** winkel. Dit certificaat wordt automatisch opgenomen in de Java Runtime Environment (JRE) die wordt gebruikt door deze zelfstudie. Zie [Java CA certificaatarchief een certificaat toe te voegen] als er geen dit certificaat in uw winkel JRE- **cacerts** ,[ add_ca_cert] voor meer informatie over het toevoegen van deze (evenals informatie over het weergeven van de certificaten in het archief van cacerts).

## <a name="how-to-create-a-service-bus-namespace"></a>Het maken van een naamruimte service bus

Om te beginnen met behulp van Service Bus wachtrijen in Azure, moet u de naamruimte van een service te maken. Een Servicenaamruimte biedt een scope-container voor het adresseren van Service Bus bronnen binnen uw toepassing.

Voor het maken van een Servicenaamruimte:

1.  Meld u op de [Azure klassieke portal](https://manage.windowsazure.com).
2.  Klik in het navigatiedeelvenster linksonder van de portal voor Azure klassieke **Service Bus, Access Control & opslaan in cache**.
3.  Klik op het knooppunt **Bus Service** en klik vervolgens op de knop **Nieuw** in het deelvenster linksboven van het klassieke Azure portal.  
    ![Screenshot-Bus-knooppunt][svc_bus_node]
4.  Voer een **Namespace**en klik vervolgens op de knop **Beschikbaarheid controleren** om er zeker van te zijn dat deze uniek is, in het dialoogvenster **een nieuwe Service Namespace maken** .  
    ![Een nieuw Namespace screenshot maken][create_namespace]
5.  Nadat en zorg dat de naam beschikbaar is, kiest u het land of de regio waarin de naamruimte moet zich bevinden en klik vervolgens op de knop **Maken Namespace** .  

    De naamruimte die u hebt gemaakt, wordt vervolgens weergegeven in de klassieke Azure portal en duurt het even om te activeren. Wacht totdat de status **actief** is voordat u verdergaat met de volgende stap.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>De standaard beheer referenties voor de naamruimte ophalen

U moet management bewerkingen uitvoeren, zoals het maken van een wachtrij op de nieuwe naamruimte ophalen van de referenties voor de naamruimte.

1.  Klik op het knooppunt **Service Bus** voor de lijst met beschikbare naamruimten in het linkernavigatievenster.
    ![Screenshot van beschikbare naamruimten][avail_namespaces]
2.  Selecteer de naamruimte die u zojuist hebt gemaakt in de lijst weergegeven.
    ![Screenshot Namespace-lijst][namespace_list]
3.  Het rechter deelvenster **Eigenschappen** worden de eigenschappen voor de nieuwe naamruimte.
    ![Schermafbeelding van deelvenster Eigenschappen][properties_pane]
4.  De **Sleutel standaard** verborgen is. Klik op de knop **weergave** om de beveiligingsreferenties weer te geven.
    ![Standaard sleutel screenshot][default_key]
5.  Maak een notitie van de **Uitgevende instelling standaard** en de **Standaard sleutel** als u kunt deze gegevens hieronder bewerkingen uitvoeren met de naamruimte.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Het maken van een Java-toepassing waarmee een intensieve taak

1. Op de ontwikkelcomputer (dit hoeft niet te worden van de virtuele machine die u hebt gemaakt), downloadt u de [SDK voor Java Azure](https://azure.microsoft.com/develop/java/).
2. Java console een toepassing maken met de voorbeeldcode aan het einde van deze sectie. In deze zelfstudie gebruiken we **TSPSolver.java** als de naam van de Java-bestand. Wijzigen de **de\_service\_bus\_naamruimte**, **de\_service\_bus\_eigenaar**, en **de\_service\_bus\_sleutel** tijdelijke aanduidingen voor uw service bus **naamruimte**, **Standaard uitgever** en **Sleutel standaard** waarden, respectievelijk.
3. De toepassing exporteren naar een runnable Java archive (JAR) na codering, en vereiste bibliotheken in de gegenereerde JAR inpakken. In deze zelfstudie gebruiken we **TSPSolver.jar** als de naam van de gegenereerde JAR.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>Het maken van een Java-toepassing die de voortgang van de intensieve taak controleert

1. Op de ontwikkelcomputer Java console een toepassing maken met de voorbeeldcode aan het einde van deze sectie. In deze zelfstudie gebruiken we **TSPClient.java** als de naam van de Java-bestand. Zoals eerder besproken, wijzigen de **uw\_service\_bus\_naamruimte**, **de\_service\_bus\_eigenaar**, en **de\_service\_bus\_sleutel** tijdelijke aanduidingen voor uw service bus **naamruimte**, **Standaard uitgever** en **Sleutel standaard** waarden, respectievelijk.
2. De toepassing exporteren naar een runnable JAR en vereiste bibliotheken in de gegenereerde JAR inpakken. In deze zelfstudie gebruiken we **TSPClient.jar** als de naam van de gegenereerde JAR.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Het uitvoeren van Java-toepassingen
Voer de intensieve toepassing, eerst naar de wachtrij maken, vervolgens om op te lossen het probleem onderweg Saleseman die de huidige beste route wordt toegevoegd aan de wachtrij service bus. Terwijl de computer-intensieve toepassingen actief (of later), is het uitvoeren van de resultaten uit de wachtrij service bus.

### <a name="to-run-the-compute-intensive-application"></a>De computer-intensieve toepassingen kunnen uitvoeren

1. Meld u op uw virtuele machine.
2. Maak een map waarin u uw toepassing wordt uitgevoerd. Bijvoorbeeld **c:\TSP**.
3. Kopieer **TSPSolver.jar** naar **c:\TSP**,
4. Maak een bestand met de naam **c:\TSP\cities.txt** met de volgende inhoud.

        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33

5. Bij een opdrachtprompt wijzigen in mappen c:\TSP.
6. Controleer de map bin van de Java Runtime Environment in de omgevingsvariabele PATH.
7. U moet de service bus wachtrij voordat u het TSP Oplosser permutaties maken. De volgende opdracht uit te maken van de bus-mailwachtrij service.

        java -jar TSPSolver.jar createqueue

8. Nu dat de wachtrij wordt gemaakt, kunt u het TSP Oplosser permutaties uitvoeren. Bijvoorbeeld de volgende opdracht uit te voeren van de Oplosser voor 8 steden worden uitgevoerd.

        java -jar TSPSolver.jar 8

 Als u een getal opgeeft, wordt deze uitgevoerd voor 10 steden. Als Oplosser worden de huidige kortste routes gevonden, wordt deze aan de wachtrij toevoegen.

> [AZURE.NOTE]
> Hoe hoger het getal dat u, hoe langer opgeeft de Oplosser wordt uitgevoerd. Bijvoorbeeld actief 14 steden kunnen enkele minuten duren en uitgevoerd voor 15 steden kan enkele uren duren. 16 of meer steden worden verhoogd, kan dit leiden tot dagen van runtime (uiteindelijk weken, maanden en jaren). Dit komt door de snelle toename van het aantal permutaties geëvalueerd door Oplosser als het aantal steden toeneemt.

### <a name="how-to-run-the-monitoring-client-application"></a>Het uitvoeren van de clienttoepassing controleren
1. Meld u op de computer waar u de clienttoepassing wordt uitgevoerd. Dit hoeft niet dezelfde computer waarop de toepassing **TSPSolver** worden, hoewel het kan zijn.
2. Maak een map waarin u uw toepassing wordt uitgevoerd. Bijvoorbeeld **c:\TSP**.
3. Kopieer **TSPClient.jar** naar **c:\TSP**,
4. Controleer de map bin van de Java Runtime Environment in de omgevingsvariabele PATH.
5. Bij een opdrachtprompt wijzigen in mappen c:\TSP.
6. Voer de volgende opdracht.

        java -jar TSPClient.jar

    U kunt desgewenst het aantal minuten slaapstand bij het controleren van de wachtrij, door doorgeven in een opdrachtregelargument opgeven. De standaardperiode voor de slaapstand voor de controle van de wachtrij is 3 minuten, die wordt gebruikt als er geen opdrachtregelargument aan **TSPClient doorgegeven**. Als u gebruiken een andere waarde voor de slaap-interval wilt, bijvoorbeeld een minuut, met de volgende opdracht uitvoeren.

        java -jar TSPClient.jar 1

    De client wordt uitgevoerd totdat er een bericht wachtrij 'Volledig' ziet. Houd er rekening mee dat als u meerdere exemplaren van de Oplosser uitvoert zonder dat de client wordt uitgevoerd, wellicht moet u meerdere keren volledig leegmaken van de wachtrij voor de client uitvoeren. U kunt ook de wachtrij te verwijderen en opnieuw maken. De wachtrij wilt verwijderen, voert u de opdracht van **TSPSolver** (niet **TSPClient**).

        java -jar TSPSolver.jar deletequeue

    De Oplosser wordt uitgevoerd totdat het klaar is met het onderzoeken van alle routes.

## <a name="how-to-stop-the-java-applications"></a>De Java-toepassingen stoppen
Voor zowel het Oplosser-clienttoepassingen, kunt u druk op **Ctrl + C** om af te sluiten als u wilt beëindigen vóór de normale voltooiing.


[solver_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../java-add-certificate-ca-store.md
