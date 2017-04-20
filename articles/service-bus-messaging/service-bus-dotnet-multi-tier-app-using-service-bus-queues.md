<properties
    pageTitle=".NET meerlagige toepassing | Microsoft Azure"
    description="Een zelfstudie .NET waarmee u een toepassing met meerdere niveaus in Azure die wachtrijen Service Bus gebruikt voor communicatie tussen de lagen te ontwikkelen."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>

# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>.NET meerlagige toepassing gebruik van wachtrijen Azure Service Bus

## <a name="introduction"></a>Inleiding

Ontwikkelen voor Microsoft Azure is eenvoudig met behulp van Visual Studio en de vrije Azure SDK voor .NET. Deze zelfstudie doorloopt u de stappen voor het maken van een toepassing die gebruikmaakt van meerdere Azure resources in uw lokale omgeving. De stappen wordt ervan uitgegaan dat u geen eerdere ervaring hebt met Azure.

U leert het volgende:

-   Het inschakelen van de computer voor de ontwikkeling van Azure met een enkel downloaden en installeren.
-   Het gebruik van Visual Studio te ontwikkelen voor Azure.
-   Het maken van een toepassing met meerdere niveaus in web- en werknemer rollen met Azure.
-   Hoe om te communiceren tussen lagen met wachtrijen Service Bus.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

In deze zelfstudie u bouwen en de meerlagige toepassing in een Azure cloud-service uitgevoerd. De front-end is een functie van ASP.NET MVC web en de back-end is een werknemer-functie die gebruikmaakt van een wachtrij Service Bus. Als een webproject dat is geïmplementeerd in een Azure website in plaats van een cloud-service kunt u de meerlagige toepassing maken met de front-endtoepassing. Zie de sectie [volgende stappen](#nextsteps) voor instructies over wat anders te doen op een website Azure-front-end. U kunt ook proberen om de zelfstudie [op-ruimten/cloud hybride .NET-toepassing](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) .

De volgende schermafbeelding ziet u de voltooide toepassing.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Scenario-overzicht: communicatie tussen rol

In te dienen voor de verwerking van een order, moet de front-UI-component in de Webrol van het met communiceren met de logica van de middelste laag is uitgevoerd in functie van de werknemer. In dit voorbeeld wordt de Service Bus brokered berichten voor de communicatie tussen de lagen.

Met behulp van brokered berichten tussen het web en de middelste lagen decouples de twee onderdelen. In tegenstelling tot directe berichten (TCP of HTTP), de weblaag niet wordt aangesloten op de middelste laag direct; in plaats daarvan duwt het werkeenheden, als berichten, op de Bus Service, die betrouwbaar blijft totdat de middenlaag is klaar om te gebruiken en deze te verwerken.

Service Bus biedt twee entiteiten brokered messaging ondersteuning: wachtrijen en onderwerpen. Elk bericht dat wordt verzonden naar een wachtrij is met wachtrijen verbruikt door een enkele ontvanger. Onderwerpen ondersteuning voor het publiceren/abonneren patroon waar elke gepubliceerde bericht beschikbaar wordt gemaakt voor een abonnement met het onderwerp wordt geregistreerd. Elk abonnement houdt logischerwijze een eigen wachtrij voor berichten. Abonnementen kunnen ook worden geconfigureerd met de filterregels voor het beperken van de berichten die worden doorgegeven aan de wachtrij abonnement die overeenkomen met het filter. In het volgende voorbeeld wordt de Service Bus wachtrijen.

![][1]

Dit mechanisme heeft verschillende voordelen ten opzichte van directe messaging:

-   **Tijdelijke ontkoppeling.** Met de asynchrone berichtenafhandeling patroon, producenten en consumenten worden online op hetzelfde moment. Bus service betrouwbare berichten worden opgeslagen totdat de verbruikende partij is klaar om te ontvangen. Hierdoor kunnen de onderdelen van de gedistribueerde toepassing worden verbroken, hetzij vrijwillig, bijvoorbeeld voor onderhoud, of als gevolg van een crash onderdeel zonder invloed op het systeem als geheel. Bovendien hoeft de toepassing in beslag nemen alleen gedurende bepaalde periodes van de dag on line worden gebracht.

-   **Herverdeling worden geladen.** In veel toepassingen varieert systeembelasting na verloop van tijd, terwijl de verwerkingstijd voor elke eenheid van werk gewoonlijk constant is. Intermediating bericht producenten en consumenten met een wachtrij betekent dat het beslag toepassing (werknemer) alleen moet worden ingericht ten behoeve van de gemiddelde belasting in plaats van piekbelasting. De diepte van de wachtrij groeit en contracten van de binnenkomende belastingstoestand. Dit bespaart u direct geld in termen van het bedrag van de infrastructuur die nodig zijn voor de belasting van toepassing.

-   **Taakverdeling.** Zoals verhogingen worden geladen, kunnen meer werkprocessen te lezen uit de wachtrij worden toegevoegd. Elk bericht wordt verwerkt door slechts één van de werkprocessen. Bovendien kan deze pull-based taakverdeling optimaal gebruik van de machines van de werknemer zelfs als de werknemer machines in termen van verwerkingscapaciteit verschillen, als ze hun maximale snelheid berichten haalt. Dit patroon wordt vaak aangeduid als de *consument concurrerende* patroon.

    ![][2]

De volgende secties worden de code die deze architectuur implementeert.

## <a name="set-up-the-development-environment"></a>De ontwikkelomgeving instellen

Voordat u met het ontwikkelen van toepassingen met Azure beginnen kunt, hulpprogramma's voor het ophalen en instellen van uw ontwikkelomgeving.

1.  De Azure SDK voor .NET op [hulpprogramma's en SDK][]installeren.

2.  Klik op **de SDK installeren** voor de versie van Visual Studio u gebruikt. De stappen in deze zelfstudie gebruiken Visual Studio 2015.

4.  Als u wilt uitvoeren of opslaan van het installatieprogramma wordt gevraagd, klikt u op **uitvoeren**.

5.  In het **Web Platform Installer**, klikt u op **installeren** en doorgaan met de installatie.

6.  Zodra de installatie voltooid is, hebt u alles wat nodig is om te starten met de ontwikkeling van de app. De SDK bevat hulpprogramma's waarmee u eenvoudig Azure toepassingen ontwikkelen in Visual Studio. Als u nog geen Visual Studio is geïnstalleerd, installeert de SDK ook de vrije Visual Studio Express.

## <a name="create-a-namespace"></a>Een naamruimte maken

De volgende stap is een Servicenaamruimte maken en krijgen een sleutel voor gedeelde toegang handtekening (SAS). Een naamruimte biedt een toepassingsgrenzen voor elke toepassing die toegankelijk zijn via de Service Bus. Een SAS-sleutel wordt gegenereerd door het systeem wanneer een naamruimte wordt gemaakt. De combinatie van de naamruimte en SAS-toets kunt u de referenties voor een Service Bus voor het verifiëren van een toepassing.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Een webpagina maken

In deze sectie kunt maken u de front-end van uw toepassing. Maak eerst de pagina's die uw toepassing wordt weergegeven.
Daarna code toevoegen die artikelen naar een wachtrij Service Bus indient en de wordt statusinformatie weergegeven over de wachtrij.

### <a name="create-the-project"></a>Het project maken

1.  Start Microsoft Visual Studio met behulp van beheerdersbevoegdheden. Visual Studio starten met administrator-bevoegdheden, met de rechtermuisknop op het programmapictogram van **Visual Studio** en klik vervolgens op **Als administrator uitvoeren**. De emulator Azure compute, verderop in dit artikel wordt besproken, moet Visual Studio worden gestart met administrator-bevoegdheden.

    In Visual Studio, klikt u in het menu **bestand** op **Nieuw**en klik vervolgens op **Project**.

2.  **Wolk** van **Geïnstalleerde sjablonen**, **Visual C#**, en klik op **Azure Cloud-Service**. Naam van het project **MultiTierApp**. Klik vervolgens op **OK**.

    ![][9]

3.  Dubbelklik op **ASP.NET Webrol**van **.NET Framework 4.5** -rollen.

    ![][10]

4.  Plaats de muisaanwijzer op de **WebRole1** onder **Azure Cloud Service-oplossing**, klikt u op het potloodpictogram en wijzig de naam van de rol van de website **FrontendWebRole**. Klik vervolgens op **OK**. (Zorg ervoor dat u 'Frontend' met een kleine letters 'e' niet 'FrontEnd'.)

    ![][11]

5.  Klik in de lijst **Selecteer een sjabloon** in het dialoogvenster **Nieuw Project voor ASP.NET** **MVC**.

    ![][12]

6. Klik op de knop **Verificatie wijzigen** in het dialoogvenster **Nieuw Project voor ASP.NET** . Klik op **Geen verificatie**in het dialoogvenster **Verificatie wijzigen** en klik op **OK**. Voor deze zelfstudie hebt u een app die een gebruikersaanmelding niet hoeft te implementeren.

    ![][16]

7. Klik op **OK** om het project te maken in het dialoogvenster **Nieuw Project voor ASP.NET** .

6.  In **Solution Explorer**in het **FrontendWebRole** -project, met de rechtermuisknop op **References**, klik op **Beheren NuGet pakketten**.

7.  Klik op het tabblad **Bladeren** en zoeken naar `Microsoft Azure Service Bus`. Klik op **installeren**en accepteer de gebruiksvoorwaarden.

    ![][13]

    Houd er rekening mee dat nu wordt verwezen naar de client vereiste assembly's en sommige nieuwe codebestanden zijn toegevoegd.

9.  In de **Solution Explorer**met de rechtermuisknop op **modellen** en klik op **toevoegen**en klik vervolgens op **klasse**. Typ in het vak **naam** de naam van de **OnlineOrder.cs**. Klik vervolgens op **toevoegen**.

### <a name="write-the-code-for-your-web-role"></a>Schrijf de code voor uw web-functie

In deze sectie kunt u de verschillende pagina's die uw toepassing wordt weergegeven.

1.  Vervang de naamruimtedefinitie van de bestaande met de volgende code in het bestand OnlineOrder.cs in Visual Studio:

    ```
    namespace FrontendWebRole.Models
    {
        public class OnlineOrder
        {
            public string Customer { get; set; }
            public string Product { get; set; }
        }
    }
    ```

2.  Dubbelklik op **Controllers\HomeController.cs**in de **Solution Explorer**. De volgende instructies voor het **gebruik van** toevoegen boven aan het bestand op te nemen van de naamruimten voor het model dat u zojuist hebt gemaakt, maar ook Service Bus.

    ```
    using FrontendWebRole.Models;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    ```

3.  Vervangt ook de naamruimtedefinitie van de bestaande met de volgende code in het bestand HomeController.cs in Visual Studio. Deze code bevat methoden voor het verwerken van de indiening van de items naar de wachtrij.

    ```
    namespace FrontendWebRole.Controllers
    {
        public class HomeController : Controller
        {
            public ActionResult Index()
            {
                // Simply redirect to Submit, since Submit will serve as the
                // front page of this application.
                return RedirectToAction("Submit");
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            // GET: /Home/Submit.
            // Controller method for a view you will create for the submission
            // form.
            public ActionResult Submit()
            {
                // Will put code for displaying queue message count here.
    
                return View();
            }
    
            // POST: /Home/Submit.
            // Controller method for handling submissions from the submission
            // form.
            [HttpPost]
            // Attribute to help prevent cross-site scripting attacks and
            // cross-site request forgery.  
            [ValidateAntiForgeryToken]
            public ActionResult Submit(OnlineOrder order)
            {
                if (ModelState.IsValid)
                {
                    // Will put code for submitting to queue here.
    
                    return RedirectToAction("Submit");
                }
                else
                {
                    return View(order);
                }
            }
        }
    }
    ```

4.  Klik op het menu **Build** **Oplossing bouwen** om te testen van de nauwkeurigheid van uw werk tot nu toe.

5.  Maak nu de weergave voor de `Submit()` methode die u eerder hebt gemaakt. Klik met de rechtermuisknop in de `Submit()` methode (de overbelasting van de `Submit()` die geen parameters), en kies vervolgens de **Weergave toevoegen**.

    ![][14]

6.  Er verschijnt een dialoogvenster voor het maken van de weergave. Kies in de lijst met **sjablonen** **maken**. Klik in de lijst **Model klasse** op de klasse **OnlineOrder** .

    ![][15]

7.  Klik op **toevoegen**.

8.  Wijzig nu de naam van uw toepassing wordt weergegeven. Dubbelklik in de **Solution Explorer**, op de **Views\Shared\\_Layout.cshtml** bestand om het te openen in de Visual Studio-editor.

9.  Vervangen van **Mijn ASP.NET-toepassing** met de **producten van LITWARE**.

10. De **introductiepagina**, **over**en **Contact** koppelingen verwijderen. Verwijder de geselecteerde code:

    ![][28]

11. Ten slotte, de pagina indienen om bepaalde informatie bevatten over de wachtrij wijzigen. Dubbelklik op het bestand **Views\Home\Submit.cshtml** om deze te openen in de editor van Visual Studio **Solution Explorer**. Voeg de volgende regel na `<h2>Submit</h2>`. Nu de `ViewBag.MessageCount` is leeg. U vult het later.

    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```

12. U hebt nu uw UI geïmplementeerd. U kunt druk op **F5** voor het uitvoeren van uw toepassing en bevestigen dat het er uitziet zoals u verwacht.

    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Schrijf de code voor het indienen van items naar een wachtrij Service Bus

Nu de code voor het indienen van items naar een wachtrij toevoegen. U maakt eerst een klasse met de verbindingsgegevens van uw Service Bus wachtrij. De verbinding van de Global.aspx.cs vervolgens initialiseren. Tot slot werken de indiening code die u eerder hebt gemaakt in HomeController.cs daadwerkelijk indienen van items naar een wachtrij Service Bus.

1.  In de **Solution Explorer**met de rechtermuisknop op **FrontendWebRole** (met de rechtermuisknop op het project, niet de rol). Klik op **toevoegen**en klik vervolgens op **klasse**.

2.  Naam van de klasse **QueueConnector.cs**. Klik op **toevoegen** om de klasse te maken.

3.  Voeg nu code ingekapseld gegevens voor de verbinding en de verbinding naar een wachtrij Bus-Service wordt geïnitialiseerd. De volledige inhoud van de QueueConnector.cs vervangen door de volgende code en voer waarden in voor `your Service Bus namespace` (de naamruimtenaam) en `yourKey`, is de **primaire sleutel** eerder via de portal Azure verkregen.

    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    
    namespace FrontendWebRole
    {
        public static class QueueConnector
        {
            // Thread-safe. Recommended that you cache rather than recreating it
            // on every request.
            public static QueueClient OrdersQueueClient;
    
            // Obtain these values from the portal.
            public const string Namespace = "your Service Bus namespace";
    
            // The name of your queue.
            public const string QueueName = "OrdersQueue";
    
            public static NamespaceManager CreateNamespaceManager()
            {
                // Create the namespace manager which gives you access to
                // management operations.
                var uri = ServiceBusEnvironment.CreateServiceUri(
                    "sb", Namespace, String.Empty);
                var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                    "RootManageSharedAccessKey", "yourKey");
                return new NamespaceManager(uri, tP);
            }
    
            public static void Initialize()
            {
                // Using Http to be friendly with outbound firewalls.
                ServiceBusEnvironment.SystemConnectivity.Mode =
                    ConnectivityMode.Http;
    
                // Create the namespace manager which gives you access to
                // management operations.
                var namespaceManager = CreateNamespaceManager();
    
                // Create the queue if it does not exist already.
                if (!namespaceManager.QueueExists(QueueName))
                {
                    namespaceManager.CreateQueue(QueueName);
                }
    
                // Get a client to the queue.
                var messagingFactory = MessagingFactory.Create(
                    namespaceManager.Address,
                    namespaceManager.Settings.TokenProvider);
                OrdersQueueClient = messagingFactory.CreateQueueClient(
                    "OrdersQueue");
            }
        }
    }
    ```

4.  Nu, ervoor te zorgen dat uw **initialisatie** -methode wordt aangeroepen. Dubbelklik op **Global.asax\Global.asax.cs**in de **Solution Explorer**.

5.  Voeg de volgende regel code aan het einde van de methode **Application_Start** .

    ```
    FrontendWebRole.QueueConnector.Initialize();
    ```

6.  Tot slot werk the web-code die u eerder hebt gemaakt, om de artikelen naar de wachtrij verzenden. Dubbelklik op **Controllers\HomeController.cs**in de **Solution Explorer**.

7.  Update de `Submit()` (de overbelasting die geen parameters) methode als volgt voor het aantal berichten voor de wachtrij.

    ```
    public ActionResult Submit()
    {
        // Get a NamespaceManager which allows you to perform management and
        // diagnostic operations on your Service Bus queues.
        var namespaceManager = QueueConnector.CreateNamespaceManager();
    
        // Get the queue, and obtain the message count.
        var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
        ViewBag.MessageCount = queue.MessageCount;
    
        return View();
    }
    ```

8.  Update de `Submit(OnlineOrder order)` (de overbelasting die één parameter) methode als volgt in te dienen gegevens naar de wachtrij.

    ```
    public ActionResult Submit(OnlineOrder order)
    {
        if (ModelState.IsValid)
        {
            // Create a message from the order.
            var message = new BrokeredMessage(order);
    
            // Submit the order.
            QueueConnector.OrdersQueueClient.Send(message);
            return RedirectToAction("Submit");
        }
        else
        {
            return View(order);
        }
    }
    ```

9.  Nu kunt u de toepassing opnieuw uitvoeren. Telkens wanneer die u een order verzendt het aantal berichten neemt toe.

    ![][18]

## <a name="create-the-worker-role"></a>De rol van werknemer maken

Nu maakt u de rol van de werknemer die de order ingediende items verwerkt. In dit voorbeeld wordt de **Rol van de werknemer met Bus servicewachtrij** Visual Studio project-sjabloon. U ontvangen al de vereiste referenties van de portal.

1. Zorg ervoor dat u Visual Studio hebt aangesloten op uw account Azure.

2.  In Visual Studio **Solution Explorer** met de rechtermuisknop op de map **rollen** onder het **MultiTierApp** -project.

3.  Klik op **toevoegen**en klik op **Nieuwe rol werknemer-Project**. Het dialoogvenster **Nieuwe rol Project toevoegen** wordt weergegeven.

    ![][26]

4.  Klik op **De rol van de werknemer met Bus-mailwachtrij Service**in het dialoogvenster **Nieuwe rol Project toevoegen** .

    ![][23]

5.  Geef in het vak **naam van** het project **OrderProcessingRole**. Klik vervolgens op **toevoegen**.

6.  De tekenreeks die u hebt verkregen in stap 9 van de sectie 'Een naamruimte Bus Service maken' naar het Klembord kopiëren.

7.  In de **Solution Explorer**met de rechtermuisknop op de **OrderProcessingRole** die u in stap 5 hebt gemaakt (Zorg ervoor dat u met de rechtermuisknop op de **OrderProcessingRole** onder de **rollen**en niet in de klasse). Klik vervolgens op **Eigenschappen**.

8.  Klik in het vak **waarde** voor **Microsoft.ServiceBus.ConnectionString**op het tabblad **Instellingen** van het dialoogvenster **Eigenschappen** en plak de endpoint-waarde die u in stap 6 hebt gekopieerd.

    ![][25]

9.  Maak een klasse **OnlineOrder** om de orders te vertegenwoordigen tijdens het verwerken van de wachtrij. U kunt een klasse die u al hebt gemaakt opnieuw gebruiken. In de **Solution Explorer**met de rechtermuisknop op de klasse **OrderProcessingRole** (met de rechtermuisknop op het pictogram niet de rol van de klasse). Klik op **toevoegen**en klik vervolgens op **Bestaand Item**.

10. Ga naar de submap voor **FrontendWebRole\Models**en dubbelklik op **OnlineOrder.cs** toe te voegen aan dit project.

11. In **WorkerRole.cs**, wijzigt u de waarde van de variabele **wachtrijnaam** van `"ProcessingQueue"` voor `"OrdersQueue"` zoals in de volgende code.

    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```

12. Voeg de volgende instructie gebruiken aan het begin van het bestand WorkerRole.cs.

    ```
    using FrontendWebRole.Models;
    ```

13. In de `Run()` functioneren binnen de `OnMessage()` bel, vervangen door de inhoud van de `try` -component met de volgende code.

    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```

14. U kunt de toepassing hebt voltooid. U kunt de volledige toepassing testen door met de rechtermuisknop op het MultiTierApp-project in de Solution Explorer, **als Project opstarten**selecteren en vervolgens op F5 te drukken. Houd er rekening mee dat het aantal berichten wordt niet verhoogd, omdat de rol van werknemer items uit de wachtrij, verwerkt en gemarkeerd als voltooid. De trace-uitvoer van uw werknemer kunt u bekijken door de Azure berekenen Emulator gebruikersinterface weer te geven. U kunt dit doen door met de rechtermuisknop op het emulator-pictogram in het systeemvak van de taakbalk en **Berekenen Emulator gebruikersinterface weergeven**te selecteren.

    ![][19]

    ![][20]

## <a name="next-steps"></a>Volgende stappen  

Zie voor meer informatie over Service Bus, de volgende bronnen:  

* [Azure Service Bus][sbmsdn]  
* [Bus service servicepagina][sbwacom]  
* [Het gebruik van wachtrijen Bus][sbwacomqhowto]  

Voor meer informatie over scenario's met meerdere niveaus, Zie:  

* [.NET meerlagige toepassing met behulp van tabellen voor opslag, wachtrijen en BLOB 's][mutitierstorage]  

  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Download hulpprogramma's en SDK]: http://go.microsoft.com/fwlink/?LinkId=271920


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
  [mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  