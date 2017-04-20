<properties
    pageTitle="Azure functies: referentie voor ontwikkelaars | Microsoft Azure"
    description="Inzicht in concepten Azure functies en onderdelen die gemeenschappelijk voor alle talen en bindingen zijn."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure functies, functies, verwerking van de gebeurtenis, webhooks, dynamische compute, zonder server-architectuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-developer-reference"></a>Azure functies: referentie voor ontwikkelaars

Azure functies delen enkele technische kernconcepten en onderdelen, ongeacht de taal of de binding met u. Voordat u in het leren werken met details die specifiek zijn voor een bepaalde taal of een binding, moet u Lees dit overzicht die voor alle afbeeldingen geldt.

In dit artikel wordt ervan uitgegaan dat u het [overzicht van Azure functies](functions-overview.md) al heeft gelezen en bekend met [concepten als triggers, bindingen, en de runtime JobHost WebJobs SDK bent](../app-service-web/websites-dotnet-webjobs-sdk.md). Azure functies is gebaseerd op de WebJobs SDK. 


## <a name="function-code"></a>De functiecode

Een *functie* is het primaire concept in Azure functies. U schrijft code voor een functie in een taal van uw keuze en de bestanden die code en een configuratiebestand opslaan in dezelfde map. Configuratie in JSON en de naam van het bestand `function.json`. Verschillende talen worden ondersteund en elk een heeft een iets andere ervaring afgestemd op voor die taal. 

De `function.json` -bestand bevat de configuratie van een functie, met inbegrip van de bindingen. De runtime leest dit bestand om te bepalen welke gebeurtenissen voor het starten van af van welke gegevens u wilt opnemen bij het aanroepen van de functie en waar u voor het verzenden van gegevens van de functie zelf doorgegeven. 

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

U kunt voorkomen dat de runtime waarop de functie wordt uitgevoerd door de `disabled` eigenschap `true`.

De `bindings` eigenschap is waar u triggers en bindingen configureren. Elke binding deelt een aantal gebruikelijke instellingen en bepaalde instellingen die specifiek voor een bepaald type binding zijn. Elke binding moet de volgende instellingen:

|Eigenschap|Waarden/typen|Opmerkingen|
|---|-----|------|
|`type`|tekenreeks|Bindingstype. Bijvoorbeeld `queueTrigger`.
|`direction`|in, 'naar buiten'| Geeft aan of de binding voor het ontvangen van gegevens in de functie of het verzenden van gegevens van de functie.
| `name` | tekenreeks | De naam die wordt gebruikt voor de gekoppelde gegevens in de functie. Voor C# is dit de naam van een argument; Als u JavaScript, worden de sleutel in een sleutel/waarde-lijst.

## <a name="function-app"></a>Functie app

Een functie app bestaat uit een of meer afzonderlijke functies die samen worden beheerd door Azure App-Service. Alle functies in een functie app delen dezelfde prijzen plan, continuous deployment en runtime-versie. Functies in meerdere talen is geschreven kunnen alle de app met dezelfde functie hebben. Een functie app beschouwen als een manier om te organiseren en beheren van uw functies samen. 

## <a name="runtime-script-host-and-web-host"></a>Runtime (scripthost en webhost)

De runtime of scripthost, is de onderliggende WebJobs SDK host luistert naar gebeurtenissen worden verzameld en verzendt gegevens en uiteindelijk wordt de code uitgevoerd. 

Ter vergemakkelijking van HTTP-triggers, is er ook een host die is ontworpen om te passen voor de scripthost in productie scenario's. Dit helpt de scripthost uit beheerd door de webhost-front-end verkeer te isoleren.

## <a name="folder-structure"></a>Mapstructuur

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Bij het instellen van een project voor de implementatie van functies voor een functie in Azure App Service app, kunt u deze mapstructuur behandelen als de sitecode van uw. U kunt bestaande hulpprogramma's continue integratie en implementatie of implementatie van aangepaste scripts voor installatie-pakket implementeren of transpilation-code.

>[AZURE.NOTE] De `wwwroot` map hier is waar uw bestanden naar get gedistribueerd. Echter, moet u niet opnemen die map in de bestanden die u implementeert, en waardoor uiteindelijk een met `wwwroot\wwwroot`. In plaats daarvan de `host.json` en de functie mappen moeten worden rechtstreeks aan de basis van wat u hebt geïmplementeerd.

## <a id="fileupdate"></a>Functie app bestanden bijwerken

De functie editor ingebouwd in de Azure portal kunt u het bestand *function.json* en de codebestand voor een functie bijwerken. U hebt om te uploaden of bijwerken van andere bestanden, zoals *package.json* of *project.json* of afhankelijkheden, gebruiken andere implementatiemethoden.

Functie apps zijn gebaseerd op App-Service, zodat alle [installatieopties beschikbaar zijn voor de standaard web-apps](../app-service-web/web-sites-deploy.md) beschikbaar voor de functie apps ook zijn. Hier volgen enkele methoden die u kunt uploaden of functie app bestanden bijwerken. 

#### <a name="to-use-app-service-editor"></a>App Service-Editor gebruiken

1. Klik op **functie app instellingen**in de portal Azure functies.

2. In de sectie **Geavanceerde instellingen** klikt u op **instellingen van App-Service**.

3. Klik op **Service-Editor App** in App Menu Nav onder **ONTWIKKELPROGRAMMA's**.

4.  Klik op **Zoeken**.

    Nadat de App Service Editor wordt geladen, ziet u de *host.json* en de functie mappen onder de *map wwwroot*. 

5. Open bestanden bewerken, of slepen en neerzetten van uw computer ontwikkeling om bestanden te uploaden.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>Gebruik de functie-app SCM (Kudu) eindpunt

1. Ga naar: `https://<function_app_name>.scm.azurewebsites.net`.

2. Klik op **Foutopsporingsconsole > CMD**.

3. Ga naar `D:\home\site\wwwroot\` voor het bijwerken van *host.json* of `D:\home\site\wwwroot\<function_name>` van de functie bestanden bij te werken.

4. Slepen en neerzetten een bestand dat u wilt uploaden naar de juiste map in het raster van het bestand. Er zijn twee gebieden in het bestand raster waar u een bestand kunt neerzetten. Voor *zip-* bestanden verschijnt een vak met het label "Sleep hier om te uploaden en unzip." Voor andere bestandstypen neerzetten in het raster van het bestand, maar buiten het vak 'unzip'.

#### <a name="to-use-ftp"></a>FTP gebruiken

1. Volg de instructies [hier](../app-service-web/web-sites-deploy.md#ftp) om FTP geconfigureerd.

2. Wanneer u met de functie app-site verbonden bent, de bijgewerkte *host.json* bestand kopiëren `/site/wwwroot` of de functie bestanden kopiëren naar `/site/wwwroot/<function_name>`.

#### <a name="to-use-continuous-deployment"></a>Continuous deployment gebruiken

Volg de instructies in het onderwerp [continue implementatie voor Azure functies](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Parallelle uitvoering

Bij meerdere die als trigger dient sneller gebeurtenissen dan ze kan worden verwerkt door een runtime single thread-functie, kan de runtime de functie meerdere keren parallel aanroepen.  Als een functie-app is de [Dynamische Service plannen](functions-scale.md#dynamic-service-plan), kan de functie app uit automatisch geschaald.  Elk exemplaar van de app functie of de toepassing wordt uitgevoerd op de dynamische Service planning of een gewone [App Service plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), mogelijk gelijktijdige functie aanroepen met meerdere threads gelijktijdig verwerken.  Het maximum aantal gelijktijdige functie aanroepen in elk exemplaar van de app functie is afhankelijk van de grootte van het geheugen van de functie app. 

## <a name="azure-functions-pulse"></a>Azure functies Pulse  

Pulse is een stream live-gebeurtenis die hoe vaak de functie wordt uitgevoerd aangeeft, maar ook de successen en mislukkingen. U kunt ook uw gemiddelde uitvoeringstijd controleren. Worden er nog meer functies en aanpassingen aan het na verloop van tijd. De pagina **puls** is toegankelijk via het tabblad **controle** .

## <a name="repositories"></a>Opslagplaatsen

De code voor de functies van Azure is open-source en opgeslagen in de opslagplaatsen van GitHub:

* [Azure functies runtime](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure functies portal](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure sjablonen met functies](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK-extensies](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bindingen

Hier volgt een lijst met alle ondersteunde bindingen.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Melden van problemen

[AZURE.INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)] 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Azure functies in C#: referentie voor ontwikkelaars](functions-reference-csharp.md)
* [Azure functies F #: referentie voor ontwikkelaars](functions-reference-fsharp.md)
* [Azure NodeJS functies: referentie voor ontwikkelaars](functions-reference-node.md)
* [Azure functies, triggers en bindingen](functions-triggers-bindings.md)
* [Functies azure: de rit](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) het Azure App Service team blog. Een overzicht van hoe de functies Azure is ontwikkeld.





