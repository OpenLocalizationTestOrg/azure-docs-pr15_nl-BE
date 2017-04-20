<properties
    pageTitle="Messaging service Bus REST zelfstudie met behulp van doorgegeven | Microsoft Azure"
    description="Bouwen van een eenvoudige Service Bus relay hosttoepassing die een REST gebaseerde interface beschrijft."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-rest-tutorial"></a>Service Bus Relay REST zelfstudie

In deze zelfstudie wordt beschreven hoe u een eenvoudige Service Bus hosttoepassing die een REST gebaseerde interface beschrijft. Overige kunt web-client, zoals een webbrowser toegang tot de Service Bus-API's via HTTP-aanvragen.

In deze zelfstudie wordt de overige Windows Communication Foundation (WCF) programmeermodel om te bouwen van een andere service op Service Bus. Zie [WCF REST Programming Model](https://msdn.microsoft.com/library/bb412169.aspx) en het [ontwerpen en implementeren van diensten](https://msdn.microsoft.com/library/ms729746.aspx) in de WCF-documentatie voor meer informatie.

## <a name="step-1-create-a-service-namespace"></a>Stap 1: Een Servicenaamruimte maken

De eerste stap is een naamruimte maken en vervolgens een gedeeld Access handtekening (SAS) sleutel. Een naamruimte biedt een toepassingsgrenzen voor elke toepassing die toegankelijk zijn via de Service Bus. Een SAS-sleutel wordt automatisch gegenereerd door het systeem als een Servicenaamruimte wordt gemaakt. De combinatie van Servicenaamruimte en SAS-toets kunt u de referenties voor een Service Bus voor het verifiëren van een toepassing.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-service-bus"></a>Stap 2: Een servicecontract op basis van REST WCF met Bus Service definiëren

Als met andere services Service Bus moet bij het maken van een service REST stijl, u het contract. Het contract bepaalt welke bewerkingen de host ondersteunt. Een bewerking kan worden beschouwd als een methode voor de webservice. Contracten worden gemaakt door het definiëren van een interface voor C++, C# of Visual Basic. Elke methode in de interface komt overeen met een specifieke bewerking. Het kenmerk [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) moet worden toegepast op elke interface en het kenmerk [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) moet worden toegepast op elke bewerking. Als een methode in een interface die de [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) heeft niet de [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), is deze methode niet beschikbaar. De code die wordt gebruikt voor deze taken wordt weergegeven in het voorbeeld van de procedure te volgen.

Het belangrijkste verschil tussen een gewone Bus Service contract en een contract voor de REST stijl is de toevoeging van een eigenschap aan de [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Deze eigenschap kunt u een methode in de interface zijn toegewezen aan een methode aan de andere kant van de interface. We gebruiken in dit geval [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) een methode om HTTP-koppeling. Hierdoor kan de Service Bus nauwkeurig ophalen en interpreteren van de opdrachten die zijn verzonden aan de interface.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Voor het maken van een contract Service Bus met een interface

1. Visual Studio als administrator openen: klik met de rechtermuisknop op het programma in het menu **Start** en klik vervolgens op **Als administrator uitvoeren**.

2. Maak een nieuwe console application-project. Klik in het menu **bestand** en selecteer **Nieuw**en selecteer vervolgens **Project**. In het dialoogvenster **Nieuw Project** klikt u op **Visual C#**, selecteert u de sjabloon **Consoletoepassing** en geef deze de naam **ImageListener**. Gebruik de standaard **locatie**. Klik op **OK** om het project te maken.

3. Voor een C#-project, maakt Visual Studio een `Program.cs` bestand. Deze klasse bevat een lege `Main()` methode, die nodig zijn voor een console application-project op juiste wijze is samengesteld.

4. Verwijzingen naar Service Bus en **System.ServiceModel.dll** toevoegen aan het project door de installatie van het pakket Service Bus NuGet. Dit pakket automatisch verwijzingen naar de Service Bus bibliotheken, alsmede de WCF- **traceerbron**toegevoegd. In de Solution Explorer met de rechtermuisknop op het **ImageListener** -project en klik vervolgens op **Beheren NuGet pakketten**. Klik op het tabblad **Bladeren** en zoeken naar `Microsoft Azure Service Bus`. Klik op **installeren**en accepteer de gebruiksvoorwaarden.

5. U moet een verwijzing naar de **System.ServiceModel.Web.dll** expliciet toevoegen aan het project:

    een. In Solution Explorer met de rechtermuisknop op de map **References** in de projectmap en klik vervolgens op **Add Reference**.

    b. Klik op het tabblad **kader** aan de linkerkant en in het vak **Zoeken** in het dialoogvenster **Verwijzing toevoegen** , typt u **System.ServiceModel.Web**. Schakel het selectievakje **System.ServiceModel.Web** in en klik op **OK**.

6. Voeg de volgende `using` instructies boven aan het bestand Program.cs.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [Traceerbron](https://msdn.microsoft.com/library/system.servicemodel.aspx) is de naamruimte waarmee u programmatische toegang tot de basisfuncties van WCF. Bus service gebruikt veel van de objecten en kenmerken van WCF-contracten. U gebruikt deze naamruimte in de meeste toepassingen relay Service Bus. Ook helpt [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) bij het definiëren van het kanaal, is het object waarmee u wilt met de Bus-Service en de web-browser van de client communiceren. Ten slotte bevat [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) de typen waarmee u webtoepassingen maken.

7. Wijzig de naam van de `ImageListener` **Microsoft.ServiceBus.Samples**-naamruimte.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Direct na de accolade van opening van de declaratie van de naamruimte definieert een nieuwe interface met de naam **IImageContract** en het kenmerk **ServiceContractAttribute** van toepassing op de interface met een waarde van `http://samples.microsoft.com/ServiceModel/Relay/`. De naamruimtewaarde verschilt van de naamruimte waarmee u binnen het bereik van uw code. De naamruimtewaarde wordt gebruikt als unieke identificatie voor dit contract en versie-informatie moet hebben. Zie [Service versiebeheer](http://go.microsoft.com/fwlink/?LinkID=180498)voor meer informatie. Expliciet opgeven van de naamruimte, voorkomt u dat de standaardwaarde voor de naamruimte worden toegevoegd aan de naam van het contract.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. Binnen de `IImageContract` interface, declareert u een methode voor één bewerking de `IImageContract` contract beschrijft in de interface en de toepassing de `OperationContractAttribute` kenmerken van de methode die u wilt weergeven als onderdeel van de overheidsopdracht Service Bus.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. Voeg het kenmerk **OperationContract** de waarde **WebGet** .

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    Service Bus route HTTP GET aanvragen doet kunt `GetImage`, en vertalen de retourwaarden van `GetImage` in antwoord op een HTTP-GETRESPONSE. Verderop in de zelfstudie gebruikt u een webbrowser toegang krijgen tot deze methode en de afbeelding weergeven in de browser.

11. Direct na de `IImageContract` definitie, declareert u een kanaal dat gegevens van zowel overneemt de `IImageContract` en `IClientChannel` interfaces.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Een kanaal is de WCF-object waarmee de client-service en informatie aan elkaar doorgeven. Later, maakt u het kanaal in de hosttoepassing. Dit kanaal Bus-service gebruikt het HTTP GET-aanvragen van de browser om aan te geven de implementatie van **GetImage** . Het kanaal service Bus ook gebruikt om de geretourneerde waarde van de **GetImage** en het vertalen in een HTTP-GETRESPONSE voor de clientbrowser.

12. Klik in het menu **Build** **Oplossing bouwen** om te bevestigen de juistheid van uw werk tot nu toe.

### <a name="example"></a>Voorbeeld

De volgende code toont een eenvoudige interface die u een contract Service Bus definieert.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Stap 3: Implementatie van een servicecontract op basis van REST WCF Service Bus gebruiken

Een andere stijl Service Bus service maken, moet u eerst de overeenkomst, die is gedefinieerd met behulp van een interface. De volgende stap is voor het implementeren van de interface. Dit omvat het maken van een klasse met de naam **ImageService** die door de gebruiker gedefinieerde **IImageContract** -interface implementeert. Nadat u het contract hebt geïmplementeerd, wordt de interface met een App.config bestand vervolgens configureren. Het configuratiebestand bevat de benodigde informatie voor de toepassing, zoals de naam van de service, de naam van het contract en het type protocol dat wordt gebruikt om te communiceren met de Service Bus. De code die wordt gebruikt voor deze taken is beschikbaar in het voorbeeld van de procedure te volgen.

Net als bij de vorige stappen, is er zeer weinig verschil tussen de uitvoering van een contract voor de REST stijl en een eenvoudige Bus Service contract.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>Voor de uitvoering van een contract voor de REST stijl Service Bus

1. Een nieuwe klasse met de naam **ImageService** rechtstreeks de definitie van de **IImageContract** -interface maken. De klasse **ImageService** implementeert de **IImageContract** -interface.

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Net als bij andere implementaties interface, kunt u de definitie implementeren in een ander bestand. Echter voor deze zelfstudie, de uitvoering wordt weergegeven in hetzelfde bestand als de interfacedefinitie van de en `Main()` methode.

2. Het kenmerk [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) van toepassing op de klasse **IImageService** om aan te geven dat de klasse een implementatie van een WCF-contract is.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Zoals eerder vermeld, is deze naamruimte geen traditionele naamruimte. In plaats daarvan uitmaakt deel van de WCF-architectuur waarmee het contract. Zie het onderwerp [Gegevens contractnamen](https://msdn.microsoft.com/library/ms731045.aspx) in de WCF-documentatie voor meer informatie.

3. Een JPG-afbeelding toevoegen aan het project.  

    Dit is een afbeelding die de service in de browser ontvangen weergeeft. Klik met de rechtermuisknop op het project en klik vervolgens op **toevoegen**. Klik vervolgens op een **Bestaand Item**. Het dialoogvenster **Add Existing Item** gebruiken om te bladeren naar een juiste .jpg en klik vervolgens op **toevoegen**.

    Als u het bestand toevoegt, zorg ervoor dat **Alle bestanden** is geselecteerd in de vervolgkeuzelijst naast de **naam:** veld. De rest van deze handleiding wordt ervan uitgegaan dat de naam van de afbeelding "image.jpg". Als u een ander bestand hebt, moet u de naam van de afbeelding wijzigen of uw code wijzigen om te compenseren.

4. Zorg ervoor dat de actieve service het afbeeldingsbestand kunt vinden, in de **Solution Explorer** met de rechtermuisknop op het afbeeldingsbestand en klik vervolgens op **Eigenschappen**. Stel in het deelvenster **Eigenschappen** **kopiëren naar de uitvoermap** kopiëren **als nieuwere**.

5. Een verwijzing naar de assembly **System.Drawing.dll** toevoegen aan het project, en ook toevoegen de volgende gekoppelde `using` instructies.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. Voeg de volgende constructor die de bitmap wordt geladen en wordt voorbereid om te verzenden naar de browser van de client in de klasse **ImageService** .

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Direct na de vorige code, de volgende **GetImage** methode toevoegen in de klasse **ImageService** aan een HTTP-bericht met de afbeelding terug.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    **MemoryStream** deze implementatie gebruikt voor het ophalen van de afbeelding en voorbereiden voor streaming naar de browser. Deze positie van de stroom bij nul begint, verklaart de inhoud stroomsgewijs verzenden als jpeg en streams van de informatie.

8. Klik in het menu **Build** **Oplossing bouwen**.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>De configuratie voor het uitvoeren van de webservice op de Bus Service definiëren

1. Dubbelklik op **App.config** om deze te openen in de editor van Visual Studio **Solution Explorer**.

    Het bestand **App.config** lijkt op een WCF-configuratiebestand en bevat de servicenaam, het eindpunt (dat wil zeggen, de locatie Service Bus beschrijft voor clients en hosts met elkaar te communiceren) en de binding (het type protocol dat wordt gebruikt om te communiceren). Het belangrijkste verschil hier is dat de geconfigureerde service-eindpunt verwijst naar een binding [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) , die geen deel uitmaakt van .NET Framework.

2. De `<system.serviceModel>` XML-element is een WCF-element dat een of meer services definieert. Hier wordt gebruikt voor het definiëren van de servicenaam en een eindpunt. Aan de onderkant van de `<system.serviceModel>` element (maar nog steeds binnen `<system.serviceModel>`), toevoegen een `<bindings>` -element met de volgende inhoud. Hiermee definieert u de bindingen in de toepassing gebruikt. U kunt bindingen met meerdere definiëren, maar voor deze zelfstudie definieert u slechts één.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Deze stap definieert een Service Bus [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) binding met **relayClientAuthenticationType** ingesteld op **geen**. Deze instelling geeft aan dat een eindpunt deze binding met de clientreferenties van een niet vereist.

3. Na de `<bindings>` -element toevoegen een `<services>` element. Net als bij de bindingen, kunt u meerdere services definiëren in een enkel configuratiebestand. Echter, voor deze zelfstudie definieert u slechts één.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    In deze stap configureert u een service die gebruikmaakt van de eerder gedefinieerde standaard- **webHttpRelayBinding**. Ook de standaard **sbTokenProvider**, die is gedefinieerd in de volgende stap wordt gebruikt.

4. Na de `<services>` -element, maken een `<behaviors>` -element met de volgende inhoud, "SAS_KEY" vervangen door de *Handtekening van gedeelde toegang* (SAS) sleutel eerder uit de [Azure portal verkregen][].

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. App.config, nog de `<appSettings>` -element, de volledige verbinding met de verbindingstekenreeks die u eerder hebt verkregen via de portal tekenreekswaarde vervangen. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. Klik op **Oplossing bouwen** voor het opbouwen van de gehele oplossing uit het menu **Build** .

### <a name="example"></a>Voorbeeld

De volgende code ziet u de uitvoering van het contract en de service voor REST-service die wordt uitgevoerd op de Bus Service met behulp van de **WebHttpRelayBinding** -binding.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

In het volgende voorbeeld wordt het bestand App.config die is gekoppeld aan de service.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-service-bus"></a>Stap 4: De REST gebaseerde WCF-service voor het gebruik van Bus-Service Host

Deze stap wordt beschreven hoe u een webservice met behulp van een consoletoepassing op Bus-Service wordt uitgevoerd. Een volledig overzicht van de code die is geschreven in deze stap vindt u in het voorbeeld van de procedure te volgen.

### <a name="to-create-a-base-address-for-the-service"></a>Voor het maken van een basisadres voor de service

1. In de `Main()` aangifte te werken, maakt u een variabele voor het opslaan van de naamruimte van uw project Service Bus. Vervang `yourNamespace` met de naam van de naamruimte van de service die u eerder hebt gemaakt.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Bus-service gebruikt de naam van de naamruimte voor het maken van een unieke URI.

2. Maak een `Uri` instantie voor het basisadres van de service op basis van de naamruimte.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Maken en configureren van de service webhost

- Maken van de web ServiceHost, de URI-adres dat eerder in deze sectie hebt gemaakt.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    De ServiceHost is het WCF-object dat een exemplaar maakt van de hosttoepassing. In dit voorbeeld wordt het type de host die u wilt maken (een **ImageService**) en het adres waarop u wilt tonen van de hosttoepassing.

### <a name="to-run-the-web-service-host"></a>De web ServiceHost uitvoeren

1. Open de service.

    ```
    host.Open();
    ```
    De service is nu actief.

2. Een bericht dat aangeeft dat de service wordt uitgevoerd en het beëindigen van de service weergegeven.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Wanneer u klaar bent, sluit u de ServiceHost.

    ```
    host.Close();
    ```

## <a name="example"></a>Voorbeeld

In het volgende voorbeeld bevat het servicecontract en de implementatie van de vorige stappen in de zelfstudie en fungeert als host voor de service in een console-programma. De volgende code compileren in een uitvoerbaar bestand met de naam ImageListener.exe.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>De code compileren

Na het opbouwen van de oplossing als volgt de toepassing wordt uitgevoerd:

1. Druk op **F5**of blader naar de locatie van het uitvoerbare bestand (ImageListener\bin\Debug\ImageListener.exe), de service uit te voeren. Houd de app actief is, zoals deze is vereist voor het uitvoeren van de volgende stap.

2. Kopieer en plak het adres van de MS-DOS-prompt in een browser voor een overzicht van de afbeelding.

3. Wanneer u klaar bent, drukt u op **Enter** in het opdrachtpromptvenster te sluiten van de app.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een toepassing die gebruikmaakt van de Service Bus relay-service hebt gemaakt, Zie de volgende artikelen voor meer informatie over indirecte messaging:

- [Overzicht van Azure Service Bus-architectuur](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)

- [Het gebruik van de Service Bus relayservice](service-bus-dotnet-how-to-use-relay.md)

[Azure portal]: https://portal.azure.com