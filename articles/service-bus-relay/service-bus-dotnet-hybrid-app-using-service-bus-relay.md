<properties
    pageTitle="Hybride op-ruimten/cloud toepassing (.NET) | Microsoft Azure"
    description="Informatie over het .NET op-ruimten/cloud hybride een toepassing maken met behulp van de relay Azure Service Bus."
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
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="net-on-premisescloud-hybrid-application-using-azure-service-bus-relay"></a>.NET-op-ruimten/cloud hybride toepassing met Azure Service Bus Relay

## <a name="introduction"></a>Inleiding

In dit artikel wordt beschreven hoe u een toepassing van hybride cloud met Microsoft Azure en Visual Studio. De zelfstudie wordt ervan uitgegaan dat u geen eerdere ervaring hebt met Azure. In minder dan 30 minuten hebt u een toepassing die meerdere Azure resources en uitgevoerd in de cloud.

U leert:

-   Het maken of een bestaand webservice voor consumptie door de oplossing van een webpagina aan te passen.
-   De Azure Service Bus Relay-service gebruiken voor het delen van gegevens tussen een Azure toepassing en een webservice die elders worden gehost.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-the-service-bus-relay-helps-with-hybrid-solutions"></a>Hoe de relay Service Bus helpt met hybride oplossingen

Zakelijke oplossingen bestaan meestal uit een combinatie van aangepaste code die is geschreven om te werken aan nieuwe en unieke zakelijke vereisten en de bestaande functionaliteit van oplossingen en systemen die al aanwezig zijn.

Architecten oplossing begint te gebruiken de cloud voor eenvoudiger verwerken van schaal eisen en lagere operationele kosten. Daarbij vinden ze bestaande service activa die ze gebruikmaken willen als bouwstenen voor hun oplossingen binnen de firewall van het bedrijf en gemakkelijk zijn bereiken voor toegang door de cloud-oplossing. Veel interne services zijn niet ingebouwd of die worden gehost op een manier dat zij gemakkelijk kunnen worden blootgesteld aan de rand van het bedrijfsnetwerk.

De relay Service Bus is ontworpen voor het gebruiksvoorbeeld bestaande webservices van Windows Communication Foundation (WCF) te nemen en deze services veilig toegankelijk te maken oplossingen die zich bevinden buiten het bedrijfsnetwerk zonder opdringerige wijzigingen in de infrastructuur van het bedrijfsnetwerk. Deze relay Service Bus diensten nog steeds worden gehost in hun bestaande omgeving, maar ze overdragen, luistert naar binnenkomende sessies en aanvragen naar de cloud gehost Service Bus. Deze services beschermt Service Bus ook tegen ongeoorloofde toegang met behulp van verificatie van [Handtekening voor gedeelde toegang](../service-bus-messaging/service-bus-sas-overview.md) (SAS).

## <a name="solution-scenario"></a>Oplossing-scenario

In deze zelfstudie maakt u een ASP.NET-website waarmee u een overzicht van de producten op de productpagina van de voorraad.

![][0]

De zelfstudie wordt verondersteld hebben productgegevens in een bestaand systeem voor op gebouwen en maakt gebruik van de relay Service Bus te bereiken op dat systeem. Dit wordt gesimuleerd door een webservice die wordt uitgevoerd in een consoletoepassing eenvoudig en wordt ondersteund door een set in het geheugen van de producten. Kun je deze consoletoepassing uitvoeren op uw eigen computer en het implementeren van de Webrol in Azure. Op deze manier ziet u hoe de Webrol van uitgevoerd in het datacenter Azure inderdaad bellen op uw computer, zelfs als uw computer wordt vrijwel zeker bevinden zich achter een firewall voor ten minste één en een laag netwerk adres netwerkadresomzetting (NAT).

Hieronder ziet u een schermafdruk van de startpagina van de voltooide webtoepassing.

![][1]

## <a name="set-up-the-development-environment"></a>De ontwikkelomgeving instellen

Voordat u met het ontwikkelen van toepassingen met Azure beginnen kunt, hulpprogramma's voor het ophalen en instellen van uw ontwikkelomgeving.

1.  De Azure SDK voor .NET installeren vanaf de pagina [u hulpprogramma's en SDK][] .

2.  Klik op **de SDK installeren** voor de versie van Visual Studio u gebruikt. De stappen in deze zelfstudie gebruiken Visual Studio 2015.

4.  Als u wilt uitvoeren of opslaan van het installatieprogramma wordt gevraagd, klikt u op **uitvoeren**.

5.  In het **Web Platform Installer**, klikt u op **installeren** en doorgaan met de installatie.

6.  Zodra de installatie voltooid is, hebt u alles wat nodig is om te starten met de ontwikkeling van de app. De SDK bevat hulpprogramma's waarmee u eenvoudig Azure toepassingen ontwikkelen in Visual Studio. Als u nog geen Visual Studio is geïnstalleerd, installeert de SDK ook de vrije Visual Studio Express.

## <a name="create-a-namespace"></a>Een naamruimte maken

Om te beginnen met de Bus Service functies in Azure, moet u de naamruimte van een service te maken. Een naamruimte biedt een scope-container voor het adresseren van Service Bus bronnen binnen uw toepassing.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Maken van een server op locatie

Eerst, bouwt u een systeem (model) op ruimten product catalogus. Zijn vrij eenvoudig; Dit kunt u zien dat een werkelijke op ruimten product catalogus systeem met een complete service oppervlak dat hebben we geprobeerd te integreren.

Dit project is een consoletoepassing Visual Studio en de [Azure Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) gebruikt de Service Bus bibliotheken en configuratie-instellingen.

### <a name="create-the-project"></a>Het project maken

1.  Start Microsoft Visual Studio met behulp van beheerdersbevoegdheden. Visual Studio starten met administrator-bevoegdheden, met de rechtermuisknop op het programmapictogram van **Visual Studio** en klik vervolgens op **Als administrator uitvoeren**.

2.  In Visual Studio, klikt u in het menu **bestand** op **Nieuw**en klik vervolgens op **Project**.

3.  Klik op **Consoletoepassing**in **Visual C#**, **Geïnstalleerde sjablonen**. Typ in het vak **naam** de naam **ProductsServer**:

    ![][11]

4.  Klik op **OK** om het **ProductsServer** -project te maken.

7.  Als u al de NuGet package manager voor Visual Studio hebt geïnstalleerd, gaat u verder met de volgende stap. Anders gaat u naar [NuGet][] en [NuGet installeren](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)op. Volg de aanwijzingen om de NuGet package manager installeren en vervolgens opnieuw start Visual Studio.

7.  In Solution Explorer met de rechtermuisknop op het **ProductsServer** -project, klik op **Beheren NuGet pakketten**.

8.  Klik op het tabblad **Bladeren** en zoeken naar `Microsoft Azure Service Bus`. Klik op **installeren**en accepteer de gebruiksvoorwaarden.

    ![][13]

    Houd er rekening mee dat de client vereiste assembly's nu naar wordt verwezen.

9.  Voeg een nieuwe klasse voor het contract van uw product. In de Solution Explorer met de rechtermuisknop op het **ProductsServer** -project en klik op **toevoegen**en klik op **klasse**.

10. Typ in het vak **naam** de naam van de **ProductsContract.cs**. Klik vervolgens op **toevoegen**.

11. In **ProductsContract.cs**, de naamruimtedefinitie te vervangen door de volgende code waarin het contract voor de service.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. Vervang in Program.cs, de naamruimtedefinitie van de met de volgende code, waarmee de profielservice en de host worden toegevoegd.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. Dubbelklik op het bestand **App.config** om deze te openen in de editor van Visual Studio Solution Explorer. Onder aan de ** &lt;systeem. ServiceModel&gt; ** element (maar nog steeds binnen &lt;systeem. ServiceModel&gt;), voeg de volgende XML-code. Zorg ervoor dat u *yourServiceNamespace* vervangt door de naam van de naamruimte en *yourKey* SAS-sleutel die u eerder hebt opgehaald vanaf de portal:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. App.config, nog de ** &lt;appSettings&gt; ** -element, de verbinding met de verbindingstekenreeks die u eerder hebt verkregen via de portal tekenreeks vervangen. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Druk op **Ctrl + Shift + B** of vanuit het menu **Build** op **Oplossing bouwen** om te bouwen van de toepassing en de juistheid van uw werk tot nu toe.

## <a name="create-an-aspnet-application"></a>Een ASP.NET-toepassing maken

In deze sectie bouwt u een eenvoudige ASP.NET-toepassing waarin gegevens worden opgehaald uit de productservice voor uw.

### <a name="create-the-project"></a>Het project maken

1.  Ervoor te zorgen dat Visual Studio wordt uitgevoerd met beheerdersbevoegdheden.

2.  In Visual Studio, klikt u in het menu **bestand** op **Nieuw**en klik vervolgens op **Project**.

3.  Klik op **ASP.NET Web Application**in **Visual C#**, **Geïnstalleerde sjablonen**. Naam van het project **ProductsPortal**. Klik vervolgens op **OK**.

    ![][15]

4.  Klik in de lijst **Selecteer een sjabloon** **MVC**. 

6.  Schakel het selectievakje in voor **de Host in de cloud**.

    ![][16]

5. Klik op **Verificatie wijzigen** . Klik op **Geen verificatie**in het dialoogvenster **Verificatie wijzigen** en klik op **OK**. Voor deze zelfstudie hebt u een app die een gebruikersaanmelding niet hoeft te implementeren.

    ![][18]

6.  In de sectie **Microsoft Azure** van het dialoogvenster **Nieuw Project voor ASP.NET** moet **opnemen in de cloud** is geselecteerd en dat de **App-Service** is geselecteerd in de vervolgkeuzelijst.

    ![][19]

7. Klik op **OK**. 

8. U moet nu Azure resources voor een nieuwe web app configureren. Volg de stappen in de sectie [bronnen Azure configureren voor een nieuwe web app](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app). Vervolgens terug naar deze zelfstudie en gaat u verder met de volgende stap.

5.  In de Solution Explorer met de rechtermuisknop op de **modellen** en vervolgens klikt u op **toevoegen**en klik vervolgens op **klasse**. Typ in het vak **naam** de naam van de **Product.cs**. Klik vervolgens op **toevoegen**.

    ![][17]

### <a name="modify-the-web-application"></a>De webtoepassing wijzigen

1.  De volgende code in het bestand Product.cs in Visual Studio, vervangen de bestaande naamruimtedefinitie.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  Vouw de map met **domeincontrollers** in Solution Explorer en dubbelklik op het bestand **HomeController.cs** om deze te openen in Visual Studio.

3. De volgende code in **HomeController.cs**, vervangen de bestaande naamruimtedefinitie.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  Vouw de map Views\Shared in de Solution Explorer en dubbelklik op **_Layout.cshtml** om deze te openen in de Visual Studio-editor.

5.  Alle exemplaren van **Mijn ASP.NET-toepassing** op **producten van LITWARE**wijzigen.

6. De **introductiepagina**, **over**en **Contact** koppelingen verwijderen. In het volgende voorbeeld wordt de geselecteerde code te verwijderen.

    ![][41]

7.  Vouw de map Views\Home in de Solution Explorer en dubbelklik op **Index.cshtml** om deze te openen in de Visual Studio-editor.
    Vervang de gehele inhoud van het bestand met de volgende code.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  Om te controleren of de nauwkeurigheid van uw werk tot nu toe, kunt u ook op **Ctrl + Shift + B** om het project samenstellen.


### <a name="run-the-app-locally"></a>De app lokaal uitvoeren

Voer de toepassing om te controleren of het werkt.

1.  Controleer of **ProductsPortal** het actieve project is. Klik met de rechtermuisknop op de naam van het project in de Solution Explorer en selecteer **Instellen als Project opstarten**.
2.  Druk op F5 als u in Visual Studio.
3.  Uw toepassing wordt weergegeven in een browser wordt uitgevoerd.

    ![][21]

## <a name="put-the-pieces-together"></a>De stukken bij elkaar plaatsen

De volgende stap is het aansluiten van de server op locatie producten met de ASP.NET-toepassing.

1.  Als dit nog niet is geopend, Visual Studio opnieuw open in het **ProductsPortal** -project dat u hebt gemaakt in de sectie [een ASP.NET-toepassing maken](#create-an-aspnet-application) .

2.  Het pakket NuGet is vergelijkbaar met de stap in de sectie 'maken een On-Premises-Server toevoegen aan de projectverwijzingen. In Solution Explorer met de rechtermuisknop op het **ProductsPortal** -project, klik op **Beheren NuGet pakketten**.

3.  'Service Bus' zoekt en selecteert u het item **Microsoft Azure Service Bus** . Vervolgens de installatie voltooid en sluit het dialoogvenster.

4.  Klik met de rechtermuisknop op het **ProductsPortal** -project in de Solution Explorer, vervolgens klikt u op **toevoegen**, klik op **Bestaand Item**.

5.  Navigeer naar het bestand **ProductsContract.cs** vanuit de console **ProductsServer** project. Klik hierop om het ProductsContract.cs te markeren. Klik op de pijl-omlaag naast **toevoegen**en klik op **toevoegen als koppeling**.

    ![][24]

6.  Nu open het bestand **HomeController.cs** in de editor van Visual Studio en de naamruimtedefinitie vervangen door de volgende code. Zorg ervoor dat *yourServiceNamespace* vervangen door de naam van de naamruimte van service en *yourKey* met uw SAS-sleutel. Hierdoor kan de client de service op locatie, geeft het resultaat van de aanroep aanroepen.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  In de Solution Explorer met de rechtermuisknop op de **ProductsPortal** -oplossing (Zorg ervoor dat u met de rechtermuisknop op de oplossing niet in het project). Klik op **toevoegen**en klik vervolgens op **Bestaand Project**.

8.  Ga naar het project **ProductsServer** en dubbelklik op het bestand **ProductsServer.csproj** -oplossing toe te voegen.

9.  **ProductsServer** moet worden uitgevoerd om de gegevens op de **ProductsPortal**weer te geven. In Solution Explorer met de rechtermuisknop op de oplossing van **ProductsPortal** en klik op **Eigenschappen**. De **Eigenschappenpagina's** wordt weergegeven.

10. Klik op **Startup Project**aan de linkerkant. Aan de rechterkant, klikt u op **meerdere projecten voor opstarten**. Zorg ervoor dat **ProductsServer** en **ProductsPortal** worden weergegeven, in die volgorde, met **Start** als de actie voor beide.

      ![][25]

11. Nog steeds in het dialoogvenster **Eigenschappen** klikt u op **Projectafhankelijkheden** aan de linkerkant.

12. Klik in de lijst **projecten** op **ProductsServer**. Zorg ervoor dat de **ProductsPortal** is **niet** ingeschakeld.

14. Klik in de lijst **projecten** op **ProductsPortal**. Controleer of **ProductsServer** is geselecteerd. 

    ![][26]

15. Klik op **OK** in het dialoogvenster **Pagina's** .

## <a name="run-the-project-locally"></a>Het project lokaal uitvoeren

Test de toepassing lokaal, druk op **F5**in Visual Studio. De server op locatie (**ProductsServer**) eerste te starten en vervolgens de **ProductsPortal** -toepassing moet worden gestart in een browservenster. Deze tijd is, ziet u dat de productvoorraad gegevens uit het systeem service op locatie bevat.

![][10]

Drukt u op **vernieuwen** op de pagina **ProductsPortal** . Telkens wanneer u de pagina vernieuwt, ziet u de servertoepassing een bericht weergeven wanneer `GetProducts()` van **ProductsServer** wordt genoemd.

Sluit beide toepassingen voordat u verdergaat met de volgende stap.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Het ProductsPortal-project implementeren op een Azure web app.

De volgende stap is de frontend **ProductsPortal** converteren naar een Azure web app. Implementeer eerst het **ProductsPortal** -project, de stappen in de sectie van [het webproject App Azure web Deploy](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app). Nadat de implementatie is voltooid, terug naar deze zelfstudie en gaat u verder met de volgende stap.

> [AZURE.NOTE] Wordt er een foutbericht in het browservenster wanneer het webproject **ProductsPortal** automatisch na de implementatie gestart wordt. Dit is normaal en doet zich voor omdat de **ProductsServer** -toepassing nog niet wordt uitgevoerd.

Kopieer de URL van de geïmplementeerde web app, want u de URL in de volgende stap moet. Verder kunt u deze URL in het venster Azure App serviceactiviteit in Visual Studio:

![][9] 

### <a name="set-productsportal-as-web-app"></a>ProductsPortal instellen als web app.

Voordat u de toepassing in de cloud, moet u ervoor zorgen dat **ProductsPortal** is gestart vanuit Visual Studio als een web app.

1. Klik met de rechtermuisknop op het **ProjectsPortal** -project in Visual Studio en klik vervolgens op **Eigenschappen**.

3. Klik in de linkerkolom op **Web**.

5. In de sectie **Actie Start** , klik op de knop **Start URL** en voer in het tekstvak de URL voor uw eerder geïmplementeerde web app; bijvoorbeeld `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. Klik op **Alles opslaan**in het menu **bestand** in Visual Studio.

7. Klik in het menu Build in Visual Studio **Oplossing bouwen**.

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

2.  Druk op F5 om te bouwen en uitvoeren van de toepassing. De server op locatie ( **ProductsServer** consoletoepassing) eerste te starten en vervolgens de **ProductsPortal** -toepassing moet worden gestart in een browservenster zoals in de volgende schermafdruk. Zoals u ziet, opnieuw de productinventaris geeft een overzicht van gegevens uit het systeem service op locatie worden opgehaald en weergegeven in de web app. Controleer de URL om ervoor te zorgen dat de **ProductsPortal** wordt uitgevoerd in een wolk, als een Azure web app. 

    ![][1]

    > [AZURE.IMPORTANT] De console **ProductsServer** toepassing moet zijn gestart en kunnen de gegevens naar de toepassing **ProductsPortal** rijden. Als de browser een fout wordt weergegeven, wacht u een paar meer seconden voor **ProductsServer** om te laden en het volgende bericht weergegeven. Drukt u vervolgens op **vernieuwen** in de browser.

    ![][37]

3. Terug in de browser, drukt u op **vernieuwen** op de pagina **ProductsPortal** . Telkens wanneer u de pagina vernieuwt, ziet u de servertoepassing een bericht weergeven wanneer `GetProducts()` van **ProductsServer** wordt genoemd.

    ![][38]

## <a name="next-steps"></a>Volgende stappen  

Zie voor meer informatie over Service Bus, de volgende bronnen:  

* [Azure Service Bus][sbwacom]  
* [Het gebruik van wachtrijen Bus][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Download hulpprogramma's en SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

