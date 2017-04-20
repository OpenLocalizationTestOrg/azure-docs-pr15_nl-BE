<properties
   pageTitle="Maken van een web-front-end voor uw toepassing met behulp van ASP.NET Core | Microsoft Azure"
   description="Uw toepassing Fabric-Service op het web met behulp van een project voor ASP.NET-Core Web-API en onderling service communicatie via ServiceProxy blootstellen."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/11/2016"
   ms.author="seanmck"/>


# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Bouwen van een web service-front-end voor uw toepassing met behulp van ASP.NET-Core

Standaard bieden configuratieservices Azure Service een openbare interface naar het web. Als u wilt weergeven in de functionaliteit van uw toepassing voor HTTP-clients, moet u een webproject om te fungeren als een toegangspunt en vervolgens communiceren van daaruit naar de afzonderlijke services maken.

In deze zelfstudie, we oppakken waar we in de zelfstudie voor het [maken van de eerste toepassing in Visual Studio gestopt](service-fabric-create-your-first-application-in-visual-studio.md) en een webservice voor de service stateful item toevoegen. Als u dit nog niet hebt gedaan, moet u teruggaan en eerst deze zelfstudie doorloopt.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Een ASP.NET-Core service aan uw toepassing toevoegen

ASP.NET-Core is een lichtgewicht, cross-platform raamwerk voor webontwikkeling dat u gebruiken kunt voor het maken van moderne web UI en web-API's. We gaan een ASP.NET Web API-project toevoegen aan onze bestaande toepassing.

>[AZURE.NOTE] Als u deze zelfstudie hebt voltooid, moet u [.NET Core 1.0 installeren][dotnetcore-install].

1. In Solution Explorer met de rechtermuisknop op **Services** in het toepassingsproject en kies **toevoegen > nieuwe Service Fabric Service**.

    ![Een nieuwe service toe te voegen aan een bestaande toepassing][vs-add-new-service]

2. Op de pagina **maken een Service** **ASP.NET Core** kiest en een naam geven.

    ![ASP.NET webservice kiezen in het dialoogvenster nieuwe service][vs-new-service-dialog]

3. De volgende pagina bevat een set van ASP.NET Core projectsjablonen. Houd er rekening mee dat die dezelfde templates die u zien zou als u een project voor ASP.NET-Core buiten een Service Fabric-toepassing hebt gemaakt. Voor deze zelfstudie kiezen wij **Web API**. U kunt echter dezelfde concepten toepassen op een volledige webtoepassing opbouwt.

    ![ASP.NET-projecttype kiezen][vs-new-aspnet-project-dialog]

    Als uw project Web-API is gemaakt, hebt u twee services in uw toepassing. Als u doorgaat met uw toepassing maken, voegt u meer services op dezelfde manier. Elk kan afzonderlijk versienummer en een upgrade is uitgevoerd worden.

>[AZURE.TIP] Voor meer informatie over het bouwen van ASP.NET Core services, raadpleegt u de [Documentatie van de ASP.NET-Core](https://docs.asp.net).

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

Als u een idee van wat we hebben gedaan, gaan we de nieuwe toepassing implementeren en bekijk het standaardgedrag dat de sjabloon ASP.NET Core Web API biedt.

1. Druk op F5 in Visual Studio voor foutopsporing van de app.

2. Wanneer de implementatie is voltooid, start u Visual Studio de browser naar de hoofdmap van de ASP.NET-Web-API service--bijvoorbeeld http://localhost:33003. Het poortnummer wordt willekeurig toegewezen en op uw computer anders zijn. De sjabloon ASP.NET Core Web API biedt niet standaardgedrag voor het toegangspunt, zodat u een foutmelding in de browser krijgt.

3. Voeg toe `/api/values` naar de locatie in de browser. Dit roept de `Get` methode van de ValuesController in de sjabloon voor Web-API. Het standaardantwoord dat wordt geleverd door de sjabloon--een JSON-matrix met twee tekenreeksen wordt geretourneerd:

    ![Standaardwaarden geretourneerd door ASP.NET Core Web API-sjabloon][browser-aspnet-template-values]

    Aan het einde van de zelfstudie wordt hebben we vervangen deze waarden met de meest recente waarde van de teller van onze stateful-service.


## <a name="connect-the-services"></a>Verbinding maken met de services

Fabric-service biedt volledige vrijheid in hoe u betrouwbare services communiceert. Binnen één toepassing mogelijk services die toegankelijk zijn via TCP, andere services die toegankelijk via een HTTP-REST API zijn en nog andere services die toegankelijk via het web-sockets zijn. Zie voor achtergrondinformatie over de beschikbare opties en de compromissen die erbij betrokken, [Communicating met services](service-fabric-connect-and-communicate-with-services.md). In deze zelfstudie wordt voert u een van de eenvoudigere benaderingen en gebruik de `ServiceProxy` / `ServiceRemotingListener` klassen die in de SDK worden geleverd.

In de `ServiceProxy` benadering (gebaseerd op RPC's of RPC's), definieert u een interface als het public contract voor de service. Gebruik vervolgens die interface voor het genereren van een proxy-klasse voor de interactie met de service.


### <a name="create-the-interface"></a>De interface maken

We zullen begint met het maken van de interface om te fungeren als het contract tussen de stateful-service en de klanten, met inbegrip van het project voor ASP.NET-Core.

1. Klik met de rechtermuisknop op uw oplossing in de Solution Explorer en kies **toevoegen** > **Nieuw Project**.

2. Kies de **Visual C#** -post in het linkernavigatievenster en selecteer vervolgens de sjabloon **Class Library** . Controleer of de versie van .NET Framework **4.5.2**is ingesteld.

    ![Een interface-project voor de stateful-service maken][vs-add-class-library-project]

3. Voor een interface worden gebruikt door `ServiceProxy`, moeten die afkomstig zijn uit de IService-interface. Deze interface is opgenomen in een van de Service Fabric NuGet pakketten. Het pakket toevoegen, klik met de rechtermuisknop op de nieuwe class library-project en kies **NuGet pakketten beheren**.

4. Het pakket **Microsoft.ServiceFabric.Services** zoeken en installeren.

    ![Het pakket NuGet Services toevoegen][vs-services-nuget-package]

5. Maak in de klassenbibliotheek, een interface met één methode `GetCountAsync`, en de interface van IService uitbreiden.

    ```c#
    namespace MyStatefulService.Interfaces
    {
        using Microsoft.ServiceFabric.Services.Remoting;

        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```


### <a name="implement-the-interface-in-your-stateful-service"></a>De interface in de stateful-service implementeren

Nu dat we de interface zijn gedefinieerd, moeten we dit in de stateful-service implementeert.

1. Voeg een verwijzing naar de class library-project waarin de interface in de stateful-service.

    ![Een verwijzing naar de class library-project toevoegen in de stateful-service][vs-add-class-library-reference]

2. Zoek de klasse dat gegevens van overneemt `StatefulService`, zoals `MyStatefulService`, en uit te breiden voor het implementeren van de `ICounter` interface.

    ```c#
    using MyStatefulService.Interfaces;

    ...

    public class MyStatefulService : StatefulService, ICounter
    {        
          // ...
    }
    ```

3. Nu implementeren de één methode die is gedefinieerd in de `ICounter` -interface, `GetCountAsync`.

    ```c#
    public async Task<long> GetCountAsync()
    {
      var myDictionary =
        await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```


### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>De stateful-service via een externe service listener blootstellen

Met de `ICounter` interface wordt geïmplementeerd, de laatste stap bij het inschakelen van de stateful-service worden callable van andere services is een communicatiekanaal te openen. Fabric-Service biedt een overschrijfbare methode aangeroepen voor stateful services, `CreateServiceReplicaListeners`. Met deze methode kunt u een of meer communicatie listeners, op basis van het type communicatie die u met uw service wilt inschakelen.

>[AZURE.NOTE] De gelijkwaardige methode voor het openen van een communicatiekanaal naar stateless services heet `CreateServiceInstanceListeners`.

In dit geval zullen we vervangen de bestaande `CreateServiceReplicaListeners` methode en een instantie van `ServiceRemotingListener`, waarbij automatisch een RPC-eindpunt dat is callable van clients via `ServiceProxy`.  

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>De klasse ServiceProxy gebruiken voor de interactie met de service

Onze stateful service is nu klaar om te ontvangen van andere services. Blijft toegevoegd zodat de code van de ASP.NET-webservice te communiceren.

1. In uw ASP.NET-project een verwijzing toevoegen aan de klassebibliotheek met de `ICounter` interface.

2. Open in het menu **Build** de **Configuration Manager**. Ziet er ongeveer als volgt:

    ![Configuration manager tonen class library als/platform][vs-configuration-manager]

    Houd er rekening mee dat de class library-project **MyStatefulService.Interface**, is geconfigureerd om samen te stellen voor elke CPU. Correct werkt met weefsel van de Service, moet het zijn expliciet gericht op x64. Klik op de vervolgkeuzelijst Platform en kies **Nieuw**en vervolgens een x64 maken platformconfiguratie.

    ![Nieuw platform voor class-bibliotheek maken][vs-create-platform]

3. Het pakket Microsoft.ServiceFabric.Services toevoegen aan het project voor ASP.NET, net als voor de class library-project eerder. Hierdoor krijgt de `ServiceProxy` klasse.

4. Open in de map met **domeincontrollers** , het `ValuesController` klasse. Houd er rekening mee dat de `Get` methode wordt momenteel slechts een hardcoded tekenreeksmatrix "waarde1" en "value2"--die overeenkomt met wat we eerder in de browser hebt gezien. Deze implementatie vervangen door de volgende code:

    ```c#
    using MyStatefulService.Interfaces;
    using Microsoft.ServiceFabric.Services.Remoting.Client;

    ...

    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));

        long count = await counter.GetCountAsync();

        return new string[] { count.ToString() };
    }
    ```

    De eerste regel van de code is de sleutel. De proxy ICounter naar de stateful-service maken, moet u twee soorten informatie: een partitie-ID en de naam van de service.

    U kunt partitioneren schaal stateful-services door het breken van de staat in verschillende categorieën, op basis van een sleutel die u, zoals een klant-ID of een postcode opgeeft. In onze eenvoudige toepassing heeft de stateful-service alleen één partitie, zodat de sleutel niet van belang. Elke sleutel die u opgeeft zal leiden tot dezelfde partitie. Meer informatie over het partitioneren van services, Zie [partitie betrouwbare configuratieservices Service](service-fabric-concepts-partitioning.md).

    De naam van een URI van het weefsel van het formulier is: /&lt;Naam_toepassing&gt;/&lt;service_name&gt;.

    Met deze twee stukjes informatie aanduiding Service Fabric unieke vormen van de machine die aanvragen moeten worden verzonden naar. De `ServiceProxy` klasse ook naadloos verwerkt wanneer de computer die fungeert als host voor de partitie stateful service mislukt en een andere computer moet worden bevorderd ter vervanging. Deze onttrekking maakt het schrijven van de clientcode voor de behandeling van andere diensten aanzienlijk eenvoudiger.

    Als we de proxy, roepen we gewoon de `GetCountAsync` methode en haar resultaat te retourneren.

5. Druk op F5 opnieuw aan de aangepaste toepassing uitvoert. Als wordt voorheen, Visual Studio automatisch gestart de browser naar de hoofdmap van het webproject. Het pad ' api/waarden' toevoegen en ziet u de huidige waarde van een teller geretourneerd.

    ![De waarde van de stateful teller weergegeven in de browser][browser-aspnet-counter-value]

    Vernieuw de browser regelmatig om te zien de waarde van de teller bijwerken.


>[AZURE.WARNING] De Core ASP.NET-webserver die in de sjabloon, bekend als Kestrel, is [momenteel niet ondersteund voor het afhandelen van directe internet-verkeer](https://docs.asp.net/en/latest/fundamentals/servers.html#kestrel). Voor productie scenario's, kunt u overwegen die als host fungeert voor uw ASP.NET-Core eindpunten achter [API Management] [ api-management-landing-page] of een andere internetverbinding gateway. Opmerking Fabric-Service wordt niet ondersteund voor de installatie van IIS.


## <a name="what-about-actors"></a>Hoe zit het met actoren?

Deze zelfstudie is gericht op het toevoegen van een web-front-end die met een stateful-service worden uitgewisseld. U kunt echter een vergelijkbaar model spreken om actoren volgen. In feite is het iets eenvoudiger.

Bij het maken van een actor-project Visual Studio een interface-project automatisch voor u genereert. U kunt die interface voor het genereren van een actor-proxy in het webproject om te communiceren met de actor. Het communicatiekanaal is automatisch beschikbaar. Dus u niet hoeft te ondernemen die gelijk is aan het tot stand brengen van een `ServiceRemotingListener` zoals voor de stateful-service in deze zelfstudie.

## <a name="how-web-services-work-on-your-local-cluster"></a>De werking van web-services op het lokale cluster

U kunt in het implementeren van exact dezelfde Service Fabric toepassing aan het cluster voor meerdere machines die u op het lokale cluster hebt geïmplementeerd en zeer zeker zal werken zoals verwacht. Dit komt omdat het lokale cluster is gewoon een 5-node configuratie op één computer is samengevouwen.

Wanneer het gaat om webservices, is er echter één belangrijk aspect. Als het cluster bevindt zich achter een taakverdeling als in Azure, moet u ervoor zorgen dat uw webservices op elke computer worden geïmplementeerd omdat de taakverdeling wordt gewoon round robin-verkeer tussen de computers. U kunt dit doen door de `InstanceCount` om de speciale waarde '-1'.

Daarentegen, wanneer u een webservice lokaal worden uitgevoerd, moet u ervoor zorgen dat slechts één exemplaar van loopt de service. Anders wordt u in conflicten uitvoeren van meerdere processen die op het pad en de poort luisteren. Daardoor moet het aantal web service-exemplaar worden ingesteld op "1" voor lokale implementaties.

Zie informatie over het configureren van verschillende waarden voor de andere omgeving, [toepassingsparameters beheren voor meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Volgende stappen

- [Een cluster maken in Azure voor de implementatie van uw toepassing naar de cloud](service-fabric-cluster-creation-via-portal.md)
- [Meer informatie over de communicatie met services](service-fabric-connect-and-communicate-with-services.md)
- [Meer informatie over het partitioneren van stateful services](service-fabric-concepts-partitioning.md)

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-configuration-manager]: ./media/service-fabric-add-a-web-frontend/vs-configuration-manager.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
[api-management-landing-page]: https://azure.microsoft.com/en-us/services/api-management/
