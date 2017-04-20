<properties 
    pageTitle="Service Bus brokered messaging REST zelfstudie | Microsoft Azure"
    description="Brokered messaging REST zelfstudie."
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

# <a name="service-bus-brokered-messaging-rest-tutorial"></a>Service Bus brokered messaging REST zelfstudie

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Deze zelfstudie laat zien hoe een eenvoudige REST gebaseerde Azure Service Bus wachtrij en onderwerp/abonnement maken.

## <a name="create-a-namespace"></a>Een naamruimte maken

De eerste stap is een Servicenaamruimte maken en een [Gedeelde Access-handtekening](service-bus-sas-overview.md) (SAS) sleutel ophalen. Een naamruimte biedt een toepassingsgrenzen voor elke toepassing die toegankelijk zijn via de Service Bus. Een SAS-sleutel wordt automatisch gegenereerd door het systeem als een Servicenaamruimte wordt gemaakt. De combinatie van Servicenaamruimte en SAS sleutel bevat een referentie voor Service Bus voor het verifiëren van een toepassing.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-console-client"></a>Maak een consoleclient

Service Bus wachtrijen kunnen u berichten in een wachtrij eerst-in, first-out opslaan. Implementeren van een patroon publiceren/abonneren, onderwerpen en abonnementen u maakt een onderwerp en vervolgens een of meer abonnementen die is gekoppeld aan dat onderwerp te maken. Wanneer berichten worden verzonden naar het onderwerp, worden ze onmiddellijk verzonden naar de abonnees van dat onderwerp.

De code in deze zelfstudie wordt het volgende.

- De naamruimte en de sleutel [Handtekening voor gedeelde toegang](service-bus-sas-overview.md) (SAS) toegang te krijgen tot uw Service Bus naamruimte resources gebruikt.

- Hiermee maakt u een wachtrij en stuurt een bericht naar de wachtrij leest het bericht in de wachtrij.

- Hiermee maakt u een onderwerp, een abonnement op dat onderwerp, en stuurt en leest het bericht in het abonnement.

- De wachtrij, onderwerp en abonnementsgegevens, inclusief de abonnementsregels voor het van Service Bus opgehaald.

- Hiermee verwijdert u de bronnen van de wachtrij, onderwerp en abonnement.

Omdat de service een REST-stijl webservice is, zijn er geen speciale typen betrokken, zoals de hele uitwisseling tekenreeksen omvat. Dit betekent dat de Visual Studio-project mag niet verwijzen naar een Service Bus-bibliotheken.

Na het verkrijgen van de naamruimte en de referenties in de eerste stap, moet u vervolgens een basistoepassing Visual Studio console maken.

### <a name="create-a-console-application"></a>Een consoletoepassing maken

1. Visual Studio starten als beheerder met de rechtermuisknop op het programma in het menu **Start** en klik vervolgens op **Als administrator uitvoeren**.

1. Maak een nieuwe console application-project. Klik in het menu **bestand** en klik op **Nieuw**en klik op **Project**. Klik in het dialoogvenster **Nieuw Project** **Visual C#** (als **Visual C#** niet, kijk onder **Andere talen**), selecteert u de sjabloon **Consoletoepassing** en geef deze de naam **Microsoft.ServiceBus.Samples**. Gebruik de standaard locatie. Klik op **OK** om het project te maken.

1. Program.cs, zorg ervoor dat uw `using` instructies er als volgt uitzien:

    ```
    using System;
    using System.Globalization;
    using System.IO;
    using System.Net;
    using System.Security.Cryptography;
    using System.Text;
    using System.Xml;
    ```

1. Indien nodig, wijzig de naam van de naamruimte voor het programma in de Visual Studio-standaard `Microsoft.ServiceBus.Samples`.

1. In de `Program` klasse, voeg de volgende globale variabelen:
    
    ```
    static string serviceNamespace;
    static string baseAddress;
    static string token;
    const string sbHostName = "servicebus.windows.net";
    ```

1. In `Main()`, plak de volgende code:

    ```
    Console.Write("Enter your service namespace: ");
    serviceNamespace = Console.ReadLine();
    
    Console.Write("Enter your SAS key: ");
    string SASKey = Console.ReadLine();
    
    baseAddress = "https://" + serviceNamespace + "." + sbHostName + "/";
    try
    {
        token = GetSASToken("RootManageSharedAccessKey", SASKey);
    
        string queueName = "Queue" + Guid.NewGuid().ToString();
    
        // Create and put a message in the queue
        CreateQueue(queueName, token);
        SendMessage(queueName, "msg1");
        string msg = ReceiveAndDeleteMessage(queueName);
    
        string topicName = "Topic" + Guid.NewGuid().ToString();
        string subscriptionName = "Subscription" + Guid.NewGuid().ToString();
        CreateTopic(topicName);
        CreateSubscription(topicName, subscriptionName);
        SendMessage(topicName, "msg2");
    
        Console.WriteLine(ReceiveAndDeleteMessage(topicName + "/Subscriptions/" + subscriptionName));
    
        // Get an Atom feed with all the queues in the namespace
        Console.WriteLine(GetResources("$Resources/Queues"));
    
        // Get an Atom feed with all the topics in the namespace
        Console.WriteLine(GetResources("$Resources/Topics"));
    
        // Get an Atom feed with all the subscriptions for the topic we just created
        Console.WriteLine(GetResources(topicName + "/Subscriptions"));
    
        // Get an Atom feed with all the rules for the topic and subscription we just created
        Console.WriteLine(GetResources(topicName + "/Subscriptions/" + subscriptionName + "/Rules"));
    
        // Delete the queue we created
        DeleteResource(queueName);
    
        // Delete the topic we created
        DeleteResource(topicName);
    
        // Get an Atom feed with all the topics in the namespace, it shouldn't have the one we created now
        Console.WriteLine(GetResources("$Resources/Topics"));
    
        // Get an Atom feed with all the queues in the namespace, it shouldn't have the one we created now
        Console.WriteLine(GetResources("$Resources/Queues"));
    }
    catch (WebException we)
    {
        using (HttpWebResponse response = we.Response as HttpWebResponse)
        {
            if (response != null)
            {
                Console.WriteLine(new StreamReader(response.GetResponseStream()).ReadToEnd());
            }
            else
            {
                Console.WriteLine(we.ToString());
            }
        }
    }
    
    Console.WriteLine("\nPress ENTER to exit.");
    Console.ReadLine();
    ```

## <a name="create-management-credentials"></a>Beheer referenties maken

De volgende stap is het schrijven van een methode waarmee de naamruimte en SAS-toets worden verwerkt in de vorige stap hebt opgegeven en geeft als resultaat een SAS-token. In dit voorbeeld wordt een SAS-token die geldig is voor één uur.

### <a name="create-a-getsastoken-method"></a>Maak een GetSASToken()

Plak de volgende code na de `Main()` methode in de `Program` klasse:

```
private static string GetSASToken(string SASKeyName, string SASKeyValue)
{
  TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
  string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + 3600);
  string stringToSign = WebUtility.UrlEncode(baseAddress) + "\n" + expiry;
  HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(SASKeyValue));

  string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
  string sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
      WebUtility.UrlEncode(baseAddress), WebUtility.UrlEncode(signature), expiry, SASKeyName);
  return sasToken;
}
```
## <a name="create-the-queue"></a>De wachtrij maken

De volgende stap is het schrijven van een methode die de opdracht REST stijl HTTP PUT gebruikt om een wachtrij te maken.

De volgende direct na het code plakken de `GetSASToken()` code die u in de vorige stap hebt toegevoegd:

```
// Uses HTTP PUT to create the queue
private static string CreateQueue(string queueName, string token)
{
    // Create the URI of the new queue, note that this uses the HTTPS scheme
    string queueAddress = baseAddress + queueName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating queue {0}", queueAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                          <title type=""text"">" + queueName + @"</title>
                          <content type=""application/xml"">
                            <QueueDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                          </content>
                        </entry>";

    byte[] response = webClient.UploadData(queueAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

## <a name="send-a-message-to-the-queue"></a>Een bericht naar de wachtrij verzenden.

In deze stap voegt u een methode die u de opdracht REST stijl HTTP POST gebruikt een bericht te verzenden naar de wachtrij die u in de vorige stap hebt gemaakt.

1. De volgende direct na het code plakken de `CreateQueue()` code die u in de vorige stap hebt toegevoegd:

    ```
    // Sends a message to the "queueName" queue, given the name and the value to enqueue
    // Uses an HTTP POST request.
    private static void SendMessage(string queueName, string body)
    {
        string fullAddress = baseAddress + queueName + "/messages" + "?timeout=60&api-version=2013-08 ";
        Console.WriteLine("\nSending message {0} - to address {1}", body, fullAddress);
        WebClient webClient = new WebClient();
        webClient.Headers[HttpRequestHeader.Authorization] = token;
    
        webClient.UploadData(fullAddress, "POST", Encoding.UTF8.GetBytes(body));
    }
    ```

1. Standaard brokered berichteigenschappen worden geplaatst een `BrokerProperties` HTTP-header. De broker-eigenschappen moeten worden geserialiseerd in JSON-indeling. De waarde van een **TimeToLive** van 30 seconden op te geven en een berichtlabel "M1" toevoegen aan het bericht, toe te voegen met de volgende code direct voor de `webClient.UploadData()` aanroepen in het vorige voorbeeld wordt weergegeven:

    ```
    // Add brokered message properties "TimeToLive" and "Label"
    webClient.Headers.Add("BrokerProperties", "{ \"TimeToLive\":30, \"Label\":\"M1\"}");
    ```

    Houd er rekening mee dat brokered berichteigenschappen zijn en dat wordt toegevoegd. Daarom moet de verzendaanvraag een API versie ondersteunt alle brokered berichteigenschappen die deel van de aanvraag uitmaken opgeven. Als een berichteigenschap brokered de opgegeven versie van de API niet ondersteunt, wordt deze eigenschap genegeerd.

1. Eigenschappen van aangepaste berichten worden gedefinieerd als een set sleutel-waardeparen. Elke aangepaste eigenschap wordt opgeslagen in de koptekst van een eigen TPPT. "Prioriteit" en "Customer" aangepaste eigenschappen toevoegen, voeg de volgende code direct voor de `webClient.UploadData()` aanroepen in het vorige voorbeeld wordt weergegeven:

    ```
    // Add custom properties "Priority" and "Customer".
    webClient.Headers.Add("Priority", "High");
    webClient.Headers.Add("Customer", "12345");
    ```

## <a name="receive-and-delete-a-message-from-the-queue"></a>Ontvangen en een bericht uit de wachtrij verwijderen

De volgende stap is het toevoegen van een methode die ontvangen en een bericht uit de wachtrij verwijderen met de opdracht REST stijl HTTP verwijderen.

De volgende direct na het code plakken de `SendMessage()` code die u in de vorige stap hebt toegevoegd:

```
// Receives and deletes the next message from the given resource (queue, topic, or subscription)
// using the resourceName and an HTTP DELETE request
private static string ReceiveAndDeleteMessage(string resourceName)
{
    string fullAddress = baseAddress + resourceName + "/messages/head" + "?timeout=60";
    Console.WriteLine("\nRetrieving message from {0}", fullAddress);
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
    string responseStr = Encoding.UTF8.GetString(response);

    Console.WriteLine(responseStr);
    return responseStr;
}
```

## <a name="create-a-topic-and-subscription"></a>Een onderwerp en een abonnement maken

De volgende stap is een methode die de opdracht REST stijl HTTP PUT gebruikt voor het maken van een onderwerp te schrijven. U schrijft vervolgens een methode die wordt gemaakt van een abonnement op dat onderwerp.

### <a name="create-a-topic"></a>Een onderwerp maken

De volgende direct na het code plakken de `ReceiveAndDeleteMessage()` code die u in de vorige stap hebt toegevoegd:

```
// Using an HTTP PUT request.
private static string CreateTopic(string topicName)
{
    var topicAddress = baseAddress + topicName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating topic {0}", topicAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + topicName + @"</title>
                                  <content type=""application/xml"">
                                    <TopicDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

    byte[] response = webClient.UploadData(topicAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

### <a name="create-a-subscription"></a>Een abonnement maken

De volgende code maakt een abonnement op het onderwerp dat u in de vorige stap hebt gemaakt. Voeg de volgende code direct na de `CreateTopic()` definitie:

```
private static string CreateSubscription(string topicName, string subscriptionName)
{
    var subscriptionAddress = baseAddress + topicName + "/Subscriptions/" + subscriptionName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating subscription {0}", subscriptionAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + subscriptionName + @"</title>
                                  <content type=""application/xml"">
                                    <SubscriptionDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

    byte[] response = webClient.UploadData(subscriptionAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

## <a name="retrieve-message-resources"></a>Bericht resources ophalen

In deze stap voegt u code die wordt opgehaald van de berichteigenschappen en vervolgens verwijdert de messaging resources die u in de vorige stappen hebt gemaakt.

### <a name="retrieve-an-atom-feed-with-the-specified-resources"></a>Een Atom-feed met de opgegeven resources ophalen

Voeg de volgende code direct na de `CreateSubscription()` methode die u in de vorige stap hebt toegevoegd:

```
private static string GetResources(string resourceAddress)
{
    string fullAddress = baseAddress + resourceAddress;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;
    Console.WriteLine("\nGetting resources from {0}", fullAddress);
    return FormatXml(webClient.DownloadString(fullAddress));
}
```

### <a name="delete-messaging-entities"></a>Messaging entiteiten verwijderen

Voeg de volgende code direct na de code die u in de vorige stap hebt toegevoegd:

```
private static string DeleteResource(string resourceName)
{
    string fullAddress = baseAddress + resourceName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nDeleting resource at {0}", fullAddress);
    byte[] response = webClient.UploadData(fullAddress, "DELETE", newbyte[0]);
    return Encoding.UTF8.GetString(response);
}
```

### <a name="format-the-atom-feed"></a>De Atom-feed opmaken

De `GetResources()` methode bevat een aanroep naar een `FormatXml()` methode die de opgehaalde Atom zet-feed beter leesbaar worden. Het volgende is de definitie van `FormatXml()`; Voeg deze code direct na de `DeleteResource()` code die u in de vorige sectie hebt toegevoegd:

```
// Formats the XML string to be more human-readable; intended for display purposes
private static string FormatXml(string inputXml)
{
    XmlDocument document = new XmlDocument();
    document.Load(new StringReader(inputXml));

    StringBuilder builder = new StringBuilder();
    using (XmlTextWriter writer = new XmlTextWriter(new StringWriter(builder)))
    {
        writer.Formatting = Formatting.Indented;
        document.Save(writer);
    }

    return builder.ToString();
}
```

## <a name="build-and-run-the-application"></a>Bouwen en uitvoeren van de toepassing

U kunt nu bouwen en uitvoeren van de toepassing. In het menu **samenstellen** in Visual Studio **Oplossing bouwen**op of druk op **Ctrl + Shift + B**.

### <a name="run-the-application"></a>De toepassing wordt uitgevoerd

Als er geen fouten zijn, drukt u op F5 om de toepassing te starten. Voer desgevraagd uw naamruimte, SAS sleutelnaam en SAS-sleutelwaarde die u hebt verkregen in de eerste stap.

### <a name="example"></a>Voorbeeld

In het volgende voorbeeld wordt de volledige code die moet worden weergegeven nadat u alle stappen in deze zelfstudie.

```
using System;
using System.Globalization;
using System.IO;
using System.Net;
using System.Security.Cryptography;
using System.Text;
using System.Xml;

namespace Microsoft.ServiceBus.Samples
{
    class Program
    {
        static string serviceNamespace;
        static string baseAddress;
        static string token;
        const string sbHostName = "servicebus.windows.net";

        static void Main(string[] args)
        {
            Console.Write("Enter your service namespace: ");
            serviceNamespace = Console.ReadLine();

            Console.Write("Enter your SAS key: ");
            string SASKey = Console.ReadLine();

            baseAddress = "https://" + serviceNamespace + "." + sbHostName + "/";
            try
            {
                token = GetSASToken("RootManageSharedAccessKey", SASKey);

                string queueName = "Queue" + Guid.NewGuid().ToString();

                // Create and put a message in the queue
                CreateQueue(queueName, token);
                SendMessage(queueName, "msg1");
                string msg = ReceiveAndDeleteMessage(queueName);

                string topicName = "Topic" + Guid.NewGuid().ToString();
                string subscriptionName = "Subscription" + Guid.NewGuid().ToString();
                CreateTopic(topicName);
                CreateSubscription(topicName, subscriptionName);
                SendMessage(topicName, "msg2");

                Console.WriteLine(ReceiveAndDeleteMessage(topicName + "/Subscriptions/" + subscriptionName));

                // Get an Atom feed with all the queues in the namespace
                Console.WriteLine(GetResources("$Resources/Queues"));

                // Get an Atom feed with all the topics in the namespace
                Console.WriteLine(GetResources("$Resources/Topics"));

                // Get an Atom feed with all the subscriptions for the topic we just created
                Console.WriteLine(GetResources(topicName + "/Subscriptions"));

                // Get an Atom feed with all the rules for the topic and subscription we just created
                Console.WriteLine(GetResources(topicName + "/Subscriptions/" + subscriptionName + "/Rules"));

                // Delete the queue we created
                DeleteResource(queueName);

                // Delete the topic we created
                DeleteResource(topicName);

                // Get an Atom feed with all the topics in the namespace, it shouldn't have the one we created now
                Console.WriteLine(GetResources("$Resources/Topics"));

                // Get an Atom feed with all the queues in the namespace, it shouldn't have the one we created now
                Console.WriteLine(GetResources("$Resources/Queues"));
            }
            catch (WebException we)
            {
                using (HttpWebResponse response = we.Response as HttpWebResponse)
                {
                    if (response != null)
                    {
                        Console.WriteLine(new StreamReader(response.GetResponseStream()).ReadToEnd());
                    }
                    else
                    {
                        Console.WriteLine(we.ToString());
                    }
                }
            }

            Console.WriteLine("\nPress ENTER to exit.");
            Console.ReadLine();
        }

        private static string GetSASToken(string SASKeyName, string SASKeyValue)
        {
            TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
            string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + 3600);
            string stringToSign = WebUtility.UrlEncode(baseAddress) + "\n" + expiry;
            HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(SASKeyValue));

            string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
            string sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
                WebUtility.UrlEncode(baseAddress), WebUtility.UrlEncode(signature), expiry, SASKeyName);
            return sasToken;
        }

        // Uses HTTP PUT to create the queue
        private static string CreateQueue(string queueName, string token)
        {
            // Create the URI of the new queue, note that this uses the HTTPS scheme
            string queueAddress = baseAddress + queueName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating queue {0}", queueAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + queueName + @"</title>
                                  <content type=""application/xml"">
                                    <QueueDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(queueAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        // Sends a message to the "queueName" queue, given the name and the value to enqueue
        // Uses an HTTP POST request.
        private static void SendMessage(string queueName, string body)
        {
            string fullAddress = baseAddress + queueName + "/messages" + "?timeout=60&api-version=2013-08 ";
            Console.WriteLine("\nSending message {0} - to address {1}", body, fullAddress);
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;
            // Add brokered message properties “TimeToLive” and “Label”.
            webClient.Headers.Add("BrokerProperties", "{ \"TimeToLive\":30, \"Label\":\"M1\"}");
            // Add custom properties “Priority” and “Customer”.
            webClient.Headers.Add("Priority", "High");
            webClient.Headers.Add("Customer", "12345");
            webClient.UploadData(fullAddress, "POST", Encoding.UTF8.GetBytes(body));

        }

        // Receives and deletes the next message from the given resource (queue, topic, or subscription)
        // using the resourceName and an HTTP DELETE request.
        private static string ReceiveAndDeleteMessage(string resourceName)
        {
            string fullAddress = baseAddress + resourceName + "/messages/head" + "?timeout=60";
            Console.WriteLine("\nRetrieving message from {0}", fullAddress);
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
            string responseStr = Encoding.UTF8.GetString(response);

            Console.WriteLine(responseStr);
            return responseStr;
        }

        // Using an HTTP PUT request.
        private static string CreateTopic(string topicName)
        {
            var topicAddress = baseAddress + topicName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating topic {0}", topicAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + topicName + @"</title>
                                  <content type=""application/xml"">
                                    <TopicDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(topicAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        private static string CreateSubscription(string topicName, string subscriptionName)
        {
            var subscriptionAddress = baseAddress + topicName + "/Subscriptions/" + subscriptionName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating subscription {0}", subscriptionAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + subscriptionName + @"</title>
                                  <content type=""application/xml"">
                                    <SubscriptionDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(subscriptionAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        private static string GetResources(string resourceAddress)
        {
            string fullAddress = baseAddress + resourceAddress;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;
            Console.WriteLine("\nGetting resources from {0}", fullAddress);
            return FormatXml(webClient.DownloadString(fullAddress));
        }

        private static string DeleteResource(string resourceName)
        {
            string fullAddress = baseAddress + resourceName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nDeleting resource at {0}", fullAddress);
            byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
            return Encoding.UTF8.GetString(response);
        }

        // Formats the XML string to be more human-readable; intended for display purposes
        private static string FormatXml(string inputXml)
        {
            XmlDocument document = new XmlDocument();
            document.Load(new StringReader(inputXml));

            StringBuilder builder = new StringBuilder();
            using (XmlTextWriter writer = new XmlTextWriter(new StringWriter(builder)))
            {
                writer.Formatting = Formatting.Indented;
                document.Save(writer);
            }

            return builder.ToString();
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie:

- [Messaging service Bus-overzicht](service-bus-messaging-overview.md)
- [Azure Service Bus fundamentals](service-bus-fundamentals-hybrid-solutions.md)
- [Service Bus relay REST zelfstudie](../service-bus-relay/service-bus-relay-rest-tutorial.md)

