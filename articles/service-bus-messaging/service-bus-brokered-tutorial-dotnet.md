<properties 
    pageTitle="Service Bus brokered messaging .NET-zelfstudie | Microsoft Azure"
    description="Brokered messaging .NET zelfstudie."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-brokered-messaging-net-tutorial"></a>Zelfstudie voor messaging .NET Service Bus brokered

Azure Service Bus biedt twee uitgebreide messaging-toepassingen – een via een centrale 'relay'-service die wordt uitgevoerd in de cloud die een groot aantal verschillende transportprotocollen en Web ondersteunt services-standaards, waaronder SOAP, WS-*, en de REST. De client hoeft niet een directe verbinding met de service in de lokalen, noch het nodig heeft om te weten waar de service is geïnstalleerd en de service op locatie hoeft niet alle poorten voor inkomend verkeer geopend in de firewall.

De tweede oplossing voor expresberichten kunt 'brokered' messaging-mogelijkheden. Deze kunnen worden beschouwd als asynchroon of ontkoppeld van de messaging-functies die ondersteuning publicatie abonnementen tijdelijke ontkoppeling en load-balancing scenario's met behulp van de messaging Service Bus-infrastructuur. Losgekoppelde communicatie heeft veel voordelen; clients en servers kunnen bijvoorbeeld verbinding maken als nodig is en bewerkingen op asynchrone wijze.

Deze zelfstudie is bedoeld om u te bieden een overzicht en praktische ervaring met wachtrijen, een van de belangrijkste onderdelen van de Service Bus brokered messaging. Nadat u de volgorde van de onderwerpen in deze zelfstudie doorloopt, hebt u een toepassing die een lijst met berichten wordt gevuld, een wachtrij maakt en verzendt berichten naar die wachtrij. Ten slotte de toepassing ontvangt, worden de berichten uit de wachtrij weergegeven en vervolgens ruimt bronnen- en uitgangen. Zie voor een overeenkomstige zelfstudie waarin wordt beschreven hoe u een toepassing die gebruikmaakt van de Relay Service Bus, de [Bus Service messaging zelfstudie doorgegeven](../service-bus-relay/service-bus-relay-tutorial.md).

## <a name="introduction-and-prerequisites"></a>Inleiding en vereisten

Wachtrijen bieden First In, eerste Out (FIFO) berichtlevering aan een of meer concurrerende consumenten. FIFO betekent dat berichten meestal naar verwachting worden ontvangen en verwerkt door de ontvangers in de tijd volgorde waarin ze werden in wachtrij en elk bericht worden ontvangen en verwerkt door de consument slechts één bericht. Een belangrijk voordeel van het gebruik van wachtrijen te verwezenlijken *tijdelijke ontkoppeling* van componenten van toepassing is: met andere woorden, de producenten en consumenten hoeft niet te worden berichten verzenden en ontvangen op hetzelfde moment, omdat de berichten in de wachtrij blijvend worden opgeslagen. Een verwante vergoeding is *herverdeling laden*, waardoor de producenten en consumenten te verzenden en ontvangen van berichten met verschillende snelheden.

Hier volgen enkele administratieve en vereiste stappen die u volgen moet voordat u begint met de zelfstudie. De eerste is een Servicenaamruimte maken en voor informatie over een gedeelde access-handtekeningsleutel (SAS). Een naamruimte biedt een toepassingsgrenzen voor elke toepassing die toegankelijk zijn via de Service Bus. Een SAS-sleutel wordt automatisch gegenereerd door het systeem als een Servicenaamruimte wordt gemaakt. De combinatie van Servicenaamruimte en SAS sleutel bevat een referentie die Bus-Service wordt geverifieerd toegang krijgen tot een toepassing.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Een Servicenaamruimte maken en krijgen een sleutel SAS

De eerste stap is een Servicenaamruimte maken en een [Gedeelde Access-handtekening](service-bus-sas-overview.md) (SAS) sleutel ophalen. Een naamruimte biedt een toepassingsgrenzen voor elke toepassing die toegankelijk zijn via de Service Bus. Een SAS-sleutel wordt automatisch gegenereerd door het systeem als een Servicenaamruimte wordt gemaakt. De combinatie van Servicenaamruimte en SAS sleutel bevat een referentie voor Service Bus voor het verifiëren van een toepassing.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

De volgende stap is een Visual Studio-project maken en twee hulpfuncties die een door komma's gescheiden lijst met berichten worden geladen in een sterk getypeerde [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .NET [lijst](https://msdn.microsoft.com/library/6sh2ey19.aspx) object schrijven.

### <a name="create-a-visual-studio-project"></a>Maak een Visual Studio-project

1. Visual Studio openen als beheerder met de rechtermuisknop op het programma in het menu Start te klikken op **Als administrator uitvoeren**.

1. Maak een nieuwe console application-project. Klik in het menu **bestand** en selecteert u **Nieuw**aan en klik vervolgens op **Project**. Klik in het dialoogvenster **Nieuw Project** **Visual C#** (als **Visual C#** niet, kijk onder **Andere talen**), klikt u op de sjabloon **Consoletoepassing** en geef deze de naam **QueueSample**. Gebruik de standaard **locatie**. Klik op **OK** om het project te maken.

1. De NuGet package manager gebruikt de Service Bus bibliotheken toevoegen aan uw project:
    1. Klik met de rechtermuisknop op het **QueueSample** -project in de Solution Explorer en klik vervolgens op **Beheren NuGet pakketten**.
    2. In het dialoogvenster **Nuget pakketten beheren** , klikt u op het tabblad **Bladeren** en zoeken naar **Azure Service Bus**en klik vervolgens op **installeren**.
<br />
1. Dubbelklik op het bestand Program.cs om deze te openen in de editor van Visual Studio Solution Explorer. Wijzig de naamruimtenaam van de standaardnaam van de `QueueSample` op `Microsoft.ServiceBus.Samples`.

    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Wijzig de `using` overzichten zoals in de volgende code.

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Maak een tekstbestand met de naam Data.csv en de kopie in een door komma's gescheiden tekst.

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Opslaan en sluit het bestand Data.csv en vergeet niet de locatie waarin u het hebt opgeslagen.

1. In de Solution Explorer met de rechtermuisknop op de naam van het project (in dit voorbeeld **QueueSample**), klikt u op **toevoegen**en klik op **Bestaand Item**.

1. Blader naar het bestand Data.csv dat u in stap 6 hebt gemaakt. Klik op het bestand en klik vervolgens op **toevoegen**. Ervoor te zorgen dat * *alle bestanden (*.*) ** is geselecteerd in de lijst met bestandstypen.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>Een methode waarbij een lijst met berichten wordt geparseerd maken

1. In de `Program` klasse voor de `Main()` methode, twee variabelen declareren: een of meer **DataTable**bevat de lijst met berichten in Data.csv. De andere moet van het type lijstobject, sterk getypeerd aan [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx). Deze laatste is de lijst van brokered berichten die in opeenvolgende stappen in de zelfstudie wordt gebruikt.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. Buiten `Main()`, definieert een `ParseCSV()` methode die de lijst met berichten in Data.csv wordt geparseerd en laadt de berichten in een tabel [DataTable](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) , zoals hier wordt weergegeven. De methode retourneert een **DataTable** -object.

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. In de `Main()` -methode een instructie toevoegt die roept de `ParseCSVFile()` methode:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>Een methode maken die in de lijst met berichten wordt geladen

1. Buiten `Main()`, definieert een `GenerateMessages()` methode waarmee het **DataTable** -object geretourneerd door `ParseCSVFile()` en laadt u de tabel in een sterk getypeerde lijst met brokered berichten. De methode retourneert vervolgens het **List** -object. 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. In `Main()`, direct na de aanroep van `ParseCSVFile()`, een instructie toevoegt die roept de `GenerateMessages()` methode met de retourwaarde van `ParseCSVFile()` als argument:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Referenties van de gebruiker downloaden

1. Maak eerst drie algemene tekenreeksvariabelen deze waarden kan bevatten. Deze variabelen declareren direct na de vorige declaraties van variabelen; bijvoorbeeld:

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 

            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```

1. Vervolgens maakt u een functie die accepteert en de naamruimte van service en SAS-sleutel worden opgeslagen. Deze methode buiten toevoegen `Main()`. Bijvoorbeeld: 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. In `Main()`, direct na de aanroep van `GenerateMessages()`, een instructie toevoegt die roept de `CollectUserInput()` methode:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>De oplossing bouwen

U kunt in het menu **samenstellen** in Visual Studio klikt u op **Oplossing bouwen** of druk op **Ctrl + Shift + B** om de nauwkeurigheid van uw werk tot nu toe.

## <a name="create-management-credentials"></a>Beheer referenties maken

In deze stap definieert u de beheertaken uit te voeren met u maakt gedeeld gebruik handtekening (SAS) referenties waarmee uw toepassing kan worden toegestaan.

1. In deze zelfstudie wordt voor de duidelijkheid, alle bewerkingen in de wachtrij geplaatst in een afzonderlijke methode. Maak een asynchrone `Queue()` methode in de `Program` klasse na de `Main()` methode. Bijvoorbeeld:
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```

1. De volgende stap is voor het maken van een SAS-referenties met een [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) -object. Het maken wordt de naam van de SA's en waarde verkregen de `CollectUserInput()` methode. Voeg de volgende code aan de `Queue()` methode:

    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```

2. Maak een nieuwe naamruimte management object met een URI die de naamruimtenaam van de en het beheer van referenties die zijn verkregen in de vorige stap als argumenten. Voeg deze code direct na de code die u in de vorige stap. Vervang `<yourNamespace>` met de naam van de naamruimte van uw service:
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Voorbeeld

Op dit moment ziet uw code er ongeveer als volgt:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>Berichten naar de wachtrij verzenden.

In deze stap kunt u een wachtrij maakt en vervolgens de berichten in de lijst met brokered berichten naar die wachtrij verzenden.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Wachtrij maken en verzenden van berichten naar de wachtrij

1. Maak eerst de wachtrij. Noem bijvoorbeeld het `myQueue`, en declareert direct na de management bewerkingen die u hebt toegevoegd in het `Queue()` -methode in de laatste stap:

    ```
    QueueDescription myQueue;

    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. In de `Queue()` -methode een messaging factory-object maken met een nieuwe Service Bus URI als argument. Voeg de volgende code direct na het management bewerkingen die u hebt toegevoegd in de laatste stap. Vervang `<yourNamespace>` met de naam van de naamruimte van uw service:

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

1. Vervolgens maakt u de wachtrij-object met de klasse [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) . Voeg de volgende code direct na de code die u hebt toegevoegd in de laatste stap:

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. Voeg vervolgens de code die de lijst met brokered berichten die u hebt gemaakt doorloopt, elk naar de wachtrij verzenden. Voeg de volgende code direct na de `CreateQueueClient()` -instructie in de vorige stap:
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>Berichten uit de wachtrij

In deze stap geeft u de lijst met berichten uit de wachtrij die u in de vorige stap hebt gemaakt.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Maak een ontvanger en ontvangen van berichten uit de wachtrij

In de `Queue()` methode, de wachtrij doorlopen en worden de berichten die met de methode [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) , afdrukken van elk bericht dat naar de console. Voeg de volgende code direct na de code die u in de vorige stap hebt toegevoegd:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();
    
        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Houd er rekening mee dat de `Thread.Sleep` wordt alleen gebruikt voor het simuleren van het bericht en u wouldn't moet waarschijnlijk het in een echte messaging-toepassing.

### <a name="end-the-queue-method-and-clean-up-resources"></a>De methode wachtrij beëindigen en resources opruimen

Voeg de volgende code voor het opschonen van de bronnen van fabrieks- en bericht direct na de voorgaande code:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>Roep de methode wachtrij

De laatste stap is het toevoegen van een instructie die de `Queue()` methode van `Main()`. De volgende regel gemarkeerde code toevoegen aan het einde van de Main():
    
```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();
    
    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);
    
    // Add this call
    Queue();
}
```

### <a name="example"></a>Voorbeeld

De volgende code bevat de volledige toepassing van de **QueueSample** .

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }
            
            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
            
            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>Bouwen en uitvoeren van de toepassing van QueueSample

Nu u de voorgaande stappen hebt voltooid, kunt u bouwen en uitvoeren van de toepassing van de **QueueSample** .

### <a name="build-the-queuesample-application"></a>De QueueSample-toepassing maken

Klik op **Oplossing bouwen**in Visual Studio, uit het menu **Build** of druk op **Ctrl + Shift + B**. Als er fouten optreden, Controleer of de code juist is gebaseerd op het volledige voorbeeld gepresenteerd aan het einde van de vorige stap.

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie bleek het bouwen van een client Service Bus toepassing en service met behulp van de messaging Service Bus brokered-mogelijkheden. Zie voor een soortgelijke zelfstudie die Service Bus [Relay](service-bus-messaging-overview.md#Relayed-messaging)wordt gebruikt, de [Service Bus berichten zelfstudie doorgegeven](../service-bus-relay/service-bus-relay-tutorial.md).

Zie de volgende onderwerpen voor meer informatie over [Service Bus](https://azure.microsoft.com/services/service-bus/).

- [Messaging service Bus-overzicht](service-bus-messaging-overview.md)
- [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
- [Service Bus-architectuur](service-bus-architecture.md)

