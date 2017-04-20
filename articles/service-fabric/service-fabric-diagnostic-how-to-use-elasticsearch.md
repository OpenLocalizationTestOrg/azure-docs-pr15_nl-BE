<properties
   pageTitle="Met Elasticsearch als een Service Fabric toepassing trace winkel | Microsoft Azure"
   description="Beschrijft hoe toepassingen Service Fabric Elasticsearch en Kibana gebruiken kunnen voor het opslaan, index, en doorzoeken toepassing sporen (Logboeken)"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/09/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Gebruik Elasticsearch als een trace Service Fabric toepassing opslaan
## <a name="introduction"></a>Inleiding
In dit artikel wordt beschreven hoe [Azure Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric/) toepassingen **Elasticsearch** en **Kibana** voor toepassingen traceren opslag gebruiken kunnen, indexering en zoeken. [Elasticsearch](https://www.elastic.co/guide/index.html) is een open source, gedistribueerde en schaalbare realtime search en analytics motor die goed geschikt is voor deze taak. Het kan worden geïnstalleerd op Windows en Linux virtuele machines die worden uitgevoerd in Microsoft Azure. Elasticsearch kan efficiënt verwerken van *gestructureerde* sporen geproduceerd met behulp van technologieën zoals **Gebeurtenis Tracing voor Windows (ETW)**.

ETW wordt gebruikt door de Service Fabric-runtime diagnostische gegevens (sporen). Dit is de aanbevolen methode voor Service Fabric toepassingen naar de bron van de diagnostische gegevens te. Via de methode maakt correlatie tussen runtime geleverd en toepassing opgegeven sporen, en wordt het oplossen van problemen gemakkelijker. Sjablonen voor het project in Visual Studio Service Fabric bevatten een registratie API (op basis van de klasse .NET **gebeurtenisbron** ) ETW-sporen standaard zendt. Zie [controle en diagnose van services in de instellingen voor een lokale computer ontwikkeling](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)voor een algemeen overzicht van Service Fabric toepassing tracering ETW.

Voor de sporen in Elasticsearch wilt laten, moeten ze worden vastgelegd op de knooppunten van het cluster Service Fabric in real-time (terwijl de toepassing wordt uitgevoerd) en naar een eindpunt Elasticsearch verzonden. Er zijn twee belangrijke opties voor het vastleggen van trace:

+ **Proces traceren vastleggen**  
De toepassing of preciezer, serviceproces is verantwoordelijk voor het verzenden van de diagnostische gegevens naar de winkel trace (Elasticsearch).

+ **Out-of-process-trace vastleggen**  
Een aparte agent is sporen van het serviceproces of processen vastleggen en deze te verzenden naar de winkel traceren.

Hieronder, het instellen van Elasticsearch in Azure, bespreken de professionals worden beschreven en nadelen voor beide opties voor vastleggen en het configureren van een service Service Fabric om gegevens te verzenden naar Elasticsearch.


## <a name="set-up-elasticsearch-on-azure"></a>Elasticsearch op Azure instellen
De eenvoudigste manier voor het instellen van de service Elasticsearch in Azure is via [**Azure Resource Manager-sjablonen**](../azure-resource-manager/resource-group-overview.md). Een uitgebreide [sjabloon voor Elasticsearch Quickstart Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) is beschikbaar via Azure Quickstart sjablonen opslagplaats. Deze sjabloon gebruikt aparte opslag rekeningen voor schaaleenheden (groepen van knooppunten). Het kan ook gegevens leveren voor afzonderlijke client en serverknooppunten op verschillende configuraties en diverse aantallen gegevensschijven die zijn aangesloten.

We gebruiken hier, een andere sjabloon, de zogeheten **ES MultiNode** uit de [opslagplaats Azure diagnostische hulpprogramma's](https://github.com/Azure/azure-diagnostics-tools). Deze sjabloon is gemakkelijker te gebruiken en een Elasticsearch cluster beschermd door HTTP-basisverificatie wordt gemaakt. Voordat u verdergaat, downloaden de opslagplaats van GitHub naar uw computer (de klonen van de opslagplaats of een zip-bestand downloaden). De ES MultiNode-sjabloon bevindt zich in de map met dezelfde naam.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>Voorbereiden van een machine Elasticsearch installatiescripts uitvoeren
De eenvoudigste manier om de ES MultiNode-sjabloon gebruiken is via een meegeleverde Azure PowerShell script genaamd `CreateElasticSearchCluster`. Als u dit script gebruikt, moet u installeren PowerShell-modules en hulpprogramma **openssl**. Deze laatste is nodig voor het maken van een SSH-sleutel die kan worden gebruikt voor het extern beheren van het cluster Elasticsearch.

`CreateElasticSearchCluster`script is ontworpen voor gebruiksgemak met de ES-MultiNode-sjabloon van een Windows-computer. Het is mogelijk de sjabloon wilt gebruiken op een niet-Windows-machine, maar dat scenario valt buiten het bestek van dit artikel.

1. Als u deze al hebt geïnstalleerd, installeert u [**Azure PowerShell-modules**](http://aka.ms/webpi-azps). Klik op **uitvoeren**, te **installeren**. Azure PowerShell 1.3 of hoger is vereist.

2. De **openssl** -hulpprogramma is opgenomen in de distributie van [**Git voor Windows**](http://www.git-scm.com/downloads). Als u dit nog niet hebt gedaan, nu [Git voor Windows](http://www.git-scm.com/downloads) installeren. (De standaard installatie-opties zijn OK.)

3. Ervan uitgaande dat Git is geïnstalleerd maar niet in het systeempad wordt opgenomen, opent u een venster van Microsoft Azure PowerShell en voer de volgende opdrachten:

    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```

    Vervang de `<Git installation folder>` met de Git locatie op uw computer; de standaardinstelling is **' C:\Program Files\Git '**. Let op de puntkomma aan het begin van het eerste pad.

4. Zorg ervoor dat u bent aangemeld met Azure (via [`Add-AzureRmAccount`](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet) en dat u het abonnement die moet worden gebruikt voor het maken van het cluster elastische zoeken hebt geselecteerd. U kunt controleren of dat het juiste abonnement is geselecteerd met behulp van `Get-AzureRmContext` en `Get-AzureRmSubscription` cmdlets.

5. Als u nog niet hebt gedaan, wordt de huidige map wijzigen in de map ES MultiNode.

### <a name="run-the-createelasticsearchcluster-script"></a>Voer het script CreateElasticSearchCluster
Voordat u het script wilt uitvoeren, opent u de `azuredeploy-parameters.json` het bestand en controleer of waarden opgeven voor de scriptparameters. De volgende parameters zijn beschikbaar:

|Naam van de parameter           |Beschrijving|
|-----------------------  |--------------------------|
|dnsNameForLoadBalancerIP |De naam die wordt gebruikt voor het maken van het openbaar zichtbaar DNS-naam voor het cluster elastische zoeken (door het domein Azure regio toe te voegen aan de naam van de opgegeven). Als de waarde voor deze parameter is 'myBigCluster' en de gekozen Azure regio West VS is, is de resulterende DNS-naam voor het cluster myBigCluster.westus.cloudapp.azure.com. <br /><br />Deze naam wordt ook gebruikt als de naam van een hoofdmap voor veel artefacten die zich bij het cluster elastische zoeken, zoals de namen van de nodes gegevens.|
|adminUsername           |De naam van de administrator-account voor het beheer van het cluster elastische zoeken (bijbehorende SSH-sleutels automatisch gegenereerd).|
|dataNodeCount           |Het aantal knooppunten in het cluster elastische zoeken. De huidige versie van het script wordt geen onderscheid gemaakt tussen gegevens- en knooppunten; alle knooppunten spelen beide rollen. De standaardwaarde is 3 knooppunten.|
|dataDiskSize            |De grootte van de gegevensschijven (in GB) dat is toegewezen voor elk gegevensknooppunt. Elk knooppunt ontvangt 4 gegevensschijven, uitsluitend gewijd aan elastische Search-service.|
|regio                  |De naam van Azure gebied waar het cluster elastische zoeken geplaatst worden moet.|
|esUserName              |De gebruikersnaam van de gebruiker die is geconfigureerd voor toegang tot het cluster ES (afhankelijk van de HTTP-basisverificatie). Het wachtwoord is geen onderdeel van het bestand en moet worden voorzien wanneer `CreateElasticSearchCluster` script wordt aangeroepen.|
|vmSizeDataNodes         |De grootte van Azure virtual machine voor clusterknooppunten elastische zoeken. De standaardwaarde is Standard_D2.|

U bent nu gereed voor het uitvoeren van het script. Geef de volgende opdracht:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

waar 

|Parameternaam    |Beschrijving|
|-----------------------  |--------------------------|
|`<es-group-name>`        |De naam van de Azure resourcegroep met alle elastische zoeken clusterbronnen.|
|`<azure-region>`         |De naam van de Azure regio waar het cluster elastische zoeken moet worden gemaakt.|         
|`<es-password>`          |Het wachtwoord voor de gebruiker elastische zoeken.|

>[AZURE.NOTE] Als u een NullReferenceException van de Test-AzureResourceGroup-cmdlet krijgt, u bent vergeten aan te melden op Azure (`Add-AzureRmAccount`).

Als u een foutmelding krijgt het script wordt uitgevoerd en u hebt vastgesteld dat de fout is veroorzaakt door een verkeerde sjabloon parameterwaarde, corrigeer het parameterbestand en het script opnieuw uitvoert met de naam van een andere resource. U kunt ook dezelfde naam gebruiken en het script opschonen van de oude versie door toe te voegen de `-RemoveExistingResourceGroup` het script aanroepen met de parameter.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>Resultaat van het uitvoeren van het script CreateElasticSearchCluster
Na uitvoering van de `CreateElasticSearchCluster` -script, de volgende belangrijkste artefacten worden gemaakt. In dit voorbeeld nemen we aan dat u 'myBigCluster' hebt gebruikt als de waarde van de `dnsNameForLoadBalancerIP` parameter is en dat de regio waar u het cluster hebt gemaakt West VS.

|Artefact|Naam, locatie en Opmerking|
|----------------------------------|----------------------------------|
|SSH-sleutel voor extern beheer |het bestand myBigCluster.key (in de map van waaruit de CreateElasticSearchCluster wordt uitgevoerd). <br /><br />Dit bestand kan worden gebruikt om verbinding met het knooppunt admin en (via het knooppunt admin) gegevens knooppunten in het cluster.|
|Knooppunt Admin                        |myBigCluster admin.westus.cloudapp.azure.com <br /><br />Een speciale VM voor extern beheer van het cluster voor Elasticsearch--de enige is waarmee externe SSH-verbindingen. Deze wordt uitgevoerd op dezelfde virtuele netwerk als alle clusterknooppunten van Elasticsearch maar Elasticsearch services niet worden uitgevoerd.|
|Gegevensknooppunten                        |myBigCluster1... myBigCluster*N* <br /><br />Gegevensknooppunten met Elasticsearch en Kibana services worden uitgevoerd. U kunt verbinding maken via SSH op elk knooppunt, maar alleen via het knooppunt admin.|
|Elasticsearch cluster             |http://myBigCluster.westus.cloudapp.Azure.com/ES/ <br /><br />Het primaire eindpunt voor het cluster Elasticsearch (Opmerking: het achtervoegsel /es). Het wordt beschermd door de standaard HTTP-verificatie (de referenties werden opgegeven parameters van de sjabloon ES MultiNode esUserName/esPassword). Het cluster heeft ook het hoofd invoegtoepassing geïnstalleerd (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) voor eenvoudige Clusterbeheer.|
|Kibana service                    |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />De Kibana-service is ingesteld op gegevens uit de gemaakte Elasticsearch cluster. Het is beveiligd door de verificatiereferenties hetzelfde als het cluster zelf.|

## <a name="in-process-versus-out-of-process-trace-capturing"></a>In-process versus out-of-process-trace vastleggen
In de inleiding gezegd twee fundamentele manieren om het verzamelen van diagnostische gegevens: in-process- en out-of-process. Elk heeft sterke en zwakke punten.

Het **proces traceren vastleggen van** voordelen:

1. *Eenvoudige configuratie en implementatie*

    * De configuratie van diagnostische gegevens verzamelen is slechts een deel van de configuratie van de toepassing. Het is eenvoudig altijd het "synchroon houden" met de rest van de toepassing.

    * Configuratie per toepassing of per service is gemakkelijk haalbaar.

    * Vastleggen van out-of-process-trace vereist gewoonlijk een afzonderlijke installatie en configuratie van de diagnostische agent, een extra administratieve taken en een potentiële bron van fouten is. De bijzondere agent-technologie kunnen vaak slechts één exemplaar van de agent per virtuele machine (knooppunt). Dit betekent dat de configuratie voor het verzamelen van diagnostische configuratie wordt gedeeld door alle toepassingen en services die worden uitgevoerd op dat knooppunt.

2. *Flexibiliteit*

    * De toepassing kan gegevens verzenden waar nodig om te gaan, zolang de clientbibliotheek van een die ondersteuning bieden voor het beoogde systeem voor gegevensopslag. Nieuwe sinks kunnen worden toegevoegd naar wens.

    * Complexe regels voor opname, filteren en samenvoegen van gegevens kunnen worden geïmplementeerd.

    * Vastleggen van een out-of-process-trace is vaak beperkt door de put van gegevens die door de agent worden ondersteund. Sommige stoffen worden uitgebreid.

3. *Toegang tot interne toepassingsgegevens en context*

    * De diagnostische subsysteem uitvoering binnen het proces van het application-service kan de sporen met contextuele informatie gemakkelijk uitbreiden.

    * In de out-of-process-methode, moeten de gegevens worden verzonden aan een agent via een mechanisme voor communicatie tussen processen, zoals Event Tracing voor Windows. Dit mechanisme kan extra beperkingen opleggen.

Het **vastleggen van out of process - trace** voordelen:

1. *De mogelijkheid om te houden op de toepassing en het verzamelen van crash dumpen*

    * Vastleggen van trace in het proces kan worden voltooid als de toepassing niet kan worden gestart of loopt vast. Onafhankelijke agent heeft een veel grotere kans vastleggen van essentiële informatie over probleemoplossing.<br /><br />

2. *Looptijd, robuustheid en de beproefde prestaties*

    * Een agent die is ontwikkeld door een leverancier platform (bijvoorbeeld een Microsoft Azure Diagnostics agent) is onderworpen aan strenge tests en strijd hardening.

    * Met process-trace vastleggen, Wees voorzichtig om ervoor te zorgen dat de activiteit van het verzenden van diagnostische gegevens van een toepassingsproces niet met de belangrijkste taken van de toepassing interfereren of problemen timing of prestaties veroorzaken. Een onafhankelijk van elkaar uitgevoerd agent minder gevoelig is voor deze problemen en is speciaal ontworpen voor het beperken van de gevolgen voor het systeem.

Het is mogelijk om te combineren en te profiteren van beide benaderingen. Inderdaad, kan het zijn de beste oplossing voor veel toepassingen.

Hier gebruiken we de **Microsoft.Diagnostic.Listeners-bibliotheek** en de in-process-tracering om gegevens te verzenden vanuit een toepassing Fabric-Service aan een cluster Elasticsearch vastleggen.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Gebruik de bibliotheek Listeners diagnostische gegevens verzenden naar Elasticsearch
De Microsoft.Diagnostic.Listeners-bibliotheek maakt deel uit van de voorbeeldtoepassing Fabric Service PartyCluster. Om deze te gebruiken:

1. [Het monster PartyCluster](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) downloaden van GitHub.

2. De Microsoft.Diagnostics.Listeners en Microsoft.Diagnostics.Listeners.Fabric projecten (hele mappen) uit de directory PartyCluster monster kopiëren naar de map van de toepassing die moet worden de gegevens verzenden naar Elasticsearch.

3. De doel-oplossing opent, klik met de rechtermuisknop op het knooppunt van de oplossing in de Solution Explorer en kies **Bestaand Project toevoegen**. Het Microsoft.Diagnostics.Listeners-project toevoegen aan de oplossing. Herhaal dezelfde voor het project Microsoft.Diagnostics.Listeners.Fabric.

4. Project een verwijzing van uw projecten service toevoegen aan de twee projecten toegevoegd. (Elke service die u stopt met het verzenden van gegevens naar Elasticsearch moet verwijzen naar Microsoft.Diagnostics.EventListeners en Microsoft.Diagnostics.EventListeners.Fabric).

    ![Projectverwijzingen naar bibliotheken Microsoft.Diagnostics.EventListeners en Microsoft.Diagnostics.EventListeners.Fabric][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Release van service Fabric algemene beschikbaarheid en Microsoft.Diagnostics.Tracing Nuget package
Toepassingen die zijn ontwikkeld met beschikbaarheid Fabric-algemene Service release (2.0.135, 31 maart 2016 vrijgegeven) doel **.NET Framework 4.5.2**. Deze versie is de laatste versie van .NET Framework ondersteund door Azure ten tijde van de release van NH. Deze versie van het framework ontbreekt helaas bepaalde EventListener APIs die de Microsoft.Diagnostics.Listeners bibliotheek nodig heeft. Omdat gebeurtenisbron (de component die de basis vormt van API's vastleggen in weefsels toepassingen) en EventListener nauw verbonden zijn, kan elk project dat gebruikmaakt van de bibliotheek Microsoft.Diagnostics.Listeners een alternatieve implementatie van gebeurtenisbron moet gebruiken. Deze implementatie wordt geleverd door het **pakket Microsoft.Diagnostics.Tracing Nuget**, geschreven door Microsoft. Het pakket is volledig achterwaarts compatibel met gebeurtenisbron opgenomen in het kader, zodat er geen codewijzigingen nodig dan naamruimte waarnaar wordt verwezen moeten worden.

Om te starten met behulp van de uitvoering van de Microsoft.Diagnostics.Tracing van de klasse gebeurtenisbron, volg deze stappen voor elk project service die nodig zijn om gegevens te verzenden naar Elasticsearch:

1. Klik met de rechtermuisknop op het project service en kies **Nuget pakketten beheren**.

2. Ga naar de bron van nuget.org pakket (als deze nog niet is geselecteerd) en zoek naar '**Microsoft.Diagnostics.Tracing**'.

3. Installeer de `Microsoft.Diagnostics.Tracing.EventSource` -pakket (en de bijbehorende afhankelijkheden).

4. Open het bestand **ServiceEventSource.cs** of **ActorEventSource.cs** in uw project service en vervangt u de `using System.Diagnostics.Tracing` richtlijn in het bestand met de `using Microsoft.Diagnostics.Tracing` richtlijn.

Deze stappen meer niet nodig nadat het **.NET Framework 4.6** wordt ondersteund door Microsoft Azure.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Elasticsearch listener starten en configureren
De laatste stap voor het verzenden van diagnostische gegevens naar Elasticsearch is het maken van een exemplaar van `ElasticSearchListener` en configureer deze met de Elasticsearch verbindingsgegevens. De listener worden automatisch vastgelegd voor alle gebeurtenissen via gebeurtenisbron klassen die zijn gedefinieerd in de project service. Het moet leven gedurende de levensduur van de service, zodat u de beste plaats om deze te maken in de code van de initialisatie. De initialisatiecode voor een stateless service kan uitzien na de nodige wijzigingen (toevoegingen die zijn uiteengezet in de opmerkingen beginnen met `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider);
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Elasticsearch verbindingsgegevens worden opgeslagen in een afzonderlijke sectie in het configuratiebestand service (**PackageRoot\Config\Settings.xml**). De naam van de sectie moet overeenkomen met de waarde die is doorgegeven aan de `FabricConfigurationProvider` constructor, bijvoorbeeld:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
De waarden van `serviceUri`, `userName` en `password` parameters komen overeen met het adres van eindpunt cluster Elasticsearch, Elasticsearch gebruikersnaam en wachtwoord, respectievelijk. `indexNamePrefix`is het voorvoegsel voor indexen Elasticsearch; de Microsoft.Diagnostics.Listeners-bibliotheek maakt elke dag een nieuwe index voor de gegevens.

### <a name="verification"></a>Verificatie
Dat is alles. Nu, wanneer de service wordt uitgevoerd, wordt het traces verzenden naar de Elasticsearch-service die is opgegeven in de configuratie. U kunt controleren of dit door de Kibana UI te openen dat is gekoppeld aan de doelinstantie Elasticsearch. In ons voorbeeld is het adres van de pagina http://myBigCluster.westus.cloudapp.azure.com/. Controleren of de indexen met het voorvoegsel van de gekozen voor de `ElasticSearchListener` instantie inderdaad gemaakt en gevuld met gegevens.

![Kibana tonen PartyCluster application-gebeurtenissen][2]

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over het opsporen en een Service Fabric-service controleren](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
