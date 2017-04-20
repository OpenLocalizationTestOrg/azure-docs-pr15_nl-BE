<properties
    pageTitle="Aan de slag met de SDK Azure CDN voor Node.js | Microsoft Azure"
    description="Informatie over het schrijven van toepassingen voor het beheren van Azure CDN Node.js."
    services="cdn"
    documentationCenter="nodejs"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Aan de slag met Azure CDN-ontwikkeling

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

U kunt de [Azure CDN SDK voor Node.js](https://www.npmjs.com/package/azure-arm-cdn) maken en beheren van CDN profielen en eindpunten te automatiseren.  Deze handleiding helpt bij het maken van een eenvoudige Node.js consoletoepassing die meer van de beschikbare bewerkingen toont.  Deze zelfstudie is niet bedoeld om alle aspecten van de SDK Azure CDN voor Node.js in detail te beschrijven.

Deze zelfstudie moet al [Node.js](http://www.nodejs.org) **4.x.x** of hoger is geïnstalleerd en geconfigureerd.  U kunt een willekeurige teksteditor die u wilt maken van uw toepassing Node.js.  Deze zelfstudie schrijven dat ik [Visual Studio-Code](https://code.visualstudio.com)gebruikt.  

> [AZURE.TIP] Het [project van deze zelfstudie voltooid](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) is beschikbaar voor download op MSDN.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Het project maken en toevoegen van NPM afhankelijkheden

Nu dat we hebt gemaakt van een resourcegroep voor onze CDN-profielen en toestemming van onze Azure AD toepassing voor het beheren van CDN profielen en eindpunten binnen die groep, kan begin onze toepassing maken.

Maak een map voor uw toepassing.  Stel uw huidige locatie naar deze nieuwe map vanuit een console met de Node.js 's in het huidige pad en initialiseren van uw project door te voeren:
    
    npm init
    
U wordt weergegeven een reeks vragen om uw project te initialiseren.  In deze zelfstudie wordt voor het **ingangspunt**, *app.js*.  U kunt mijn andere opties in het volgende voorbeeld kunt zien.

![NPM init-uitvoer](./media/cdn-app-dev-node/cdn-npm-init.png)

Onze project wordt nu geïnitialiseerd met een *packages.json* -bestand.  Onze project gaat gebruiken sommige Azure bibliotheken opgenomen in NPM-pakketten.  We gebruiken de Azure Client Runtime voor Node.js (ms rest azure) en de Client-bibliotheek van Azure CDN voor Node.js (azure arm cd).  Laten we deze toevoegen aan het project als afhankelijkheden.
 
    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Nadat u klaar bent met de pakketten moet installeert, wordt het *package.json* bestand uitzien in het volgende voorbeeld (versie nummers kan verschillen):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Ten slotte met uw teksteditor, maak een leeg tekstbestand en sla het op in de hoofdmap van de projectmap als *app.js*.  We zijn nu klaar om te beginnen met het schrijven van code.

## <a name="requires-constants-authentication-and-structure"></a>Vereist, constanten, verificatie en structuur

Met *app.js* in onze editor openen, we krijgen de basisstructuur van ons programma geschreven.

1. Voeg de "vereist" voor onze pakketten NPM boven aan het volgende:

    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```

2. We moeten sommige constanten onze methoden gebruiken definiëren.  Voeg het volgende toe.  U in plaats van tijdelijke aanduidingen, met inbegrip van de ** &lt;punthaken&gt;**, met uw eigen waarden.

    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";

    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Vervolgens we instantiëren de CDN management-client en geven onze referenties.

    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
    
    Als u van verificatie van de individuele gebruiker gebruikmaakt, er deze twee lijnen enigszins anders.

    >[AZURE.IMPORTANT] In deze voorbeeldcode alleen gebruiken als u ervoor kiest om afzonderlijke gebruikersverificatie in plaats van een service principal.  Zorg ervoor dat uw referenties voor afzonderlijke gebruikers te beschermen en deze geheim houdt.

    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```

    Zorg ervoor dat u de vervanging van de artikelen in ** &lt;punthaken&gt; ** met de juiste gegevens.  Voor `<redirect URI>`, de URI die u hebt ingevoerd toen u de toepassing geregistreerd in Azure AD-omleiding te gebruiken.
    

4.  Onze Node.js consoletoepassing gaat nemen enkele parameters voor de opdrachtregel.  We controleren ten minste één parameter is doorgegeven.

    ```javascript
    //Collect command-line parameters
    var parms = process.argv.slice(2);

    //Do we have parameters?
    if(parms == null || parms.length == 0)
    {
        console.log("Not enough parameters!");
        console.log("Valid commands are list, delete, create, and purge.");
        process.exit(1);
    }
    ```

5. Dat brengt ons tot het belangrijkste onderdeel van ons programma, waar we aftakking naar andere functies op basis van welke parameters zijn doorgegeven.

    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;

        case "create":
            cdnCreate();
            break;
        
        case "delete":
            cdnDelete();
            break;

        case "purge":
            cdnPurge();
            break;

        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```

6.  Op verschillende plaatsen in ons programma moet u controleren of het juiste aantal parameters zijn doorgegeven en sommige Help-informatie weer te geven als ze niet bevallen.  We functies maken om dat te doen.

    ```javascript
    function requireParms(parmCount) {
        if(parms.length < parmCount) {
            usageHelp(parms[0].toLowerCase());
            process.exit(1);
        }
    }

    function usageHelp(cmd) {
        console.log("Usage for " + cmd + ":");
        switch(cmd)
        {
            case "list":
                console.log("list profiles");
                console.log("list endpoints <profile name>");
                break;

            case "create":
                console.log("create profile <profile name>");
                console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
                break;
            
            case "delete":
                console.log("delete profile <profile name>");
                console.log("delete endpoint <profile name> <endpoint name>");
                break;

            case "purge":
                console.log("purge <profile name> <endpoint name> <path>");
                break;

            default:
                console.log("Invalid command.");
        }
    }
    ```

7. Ten slotte zijn de functies die we op de client CDN management gebruikt asynchroon, zodat ze nodig hebben een methode aan te roepen wanneer ze klaar.  We maken die u kunt weergeven van de uitvoer van de CDN management-client (indien aanwezig) en het programma afgesloten.

    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Nu de basisstructuur van het programma is geschreven, moeten we de functies die worden aangeroepen op basis van onze parameters maken.

## <a name="list-cdn-profiles-and-endpoints"></a>Lijst CDN profielen en eindpunten

Laten we beginnen met de code voor een overzicht van onze bestaande profielen en eindpunten.  Mijn commentaar bij de code bieden de verwachte syntaxis, zodat we weten waar elke parameter gaat.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN profielen en eindpunten maken

Vervolgens zullen we schrijven de functies om profielen en eindpunten te maken.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Een eindpunt verwijderen

Ervan uitgaande dat het eindpunt is gemaakt, is een gemeenschappelijke taak die wij in ons programma uit te voeren kunt inhoud in onze eindpunt verwijderen.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN profielen en eindpunten verwijderen

De laatste functie we nemen verwijderd eindpunten en profielen.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>Het programma wordt uitgevoerd

We ons Node.js programma met behulp van onze favoriete debugger kan nu worden uitgevoerd of op de console.

> [AZURE.TIP] Als u Visual Studio-Code als in de debugger gebruikt, moet u uw omgeving instellen om aan te geven in de parameters voor de opdrachtregel.  Visual Studio Code wordt in het bestand **lanuch.json** .  Zoeken naar een eigenschap met de naam **args** en het toevoegen van een matrix van string-waarden voor de parameters, zodat er ongeveer zo uitziet: `"args": ["list", "profiles"]`.

Laten we beginnen met een overzicht van onze profielen.

![Lijst met profielen](./media/cdn-app-dev-node/cdn-list-profiles.png)

Wij weer een lege matrix.  Aangezien wij geen profielen in onze groep hebt, die wordt verwacht.  We gaan nu een profiel maken.

![Profiel maken](./media/cdn-app-dev-node/cdn-create-profile.png)

Nu voegen we een eindpunt.

![Eindpunt maken](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Tot slot laten we ons profiel verwijderen.

![Profiel verwijderen](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Volgende stappen

Om te zien het voltooide project van deze procedure, [downloaden van het monster](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

Overzicht van de verwijzing voor de Azure CDN SDK voor Node.js, de [verwijzing](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/)te bekijken.

Ga voor aanvullende documentatie op de Azure-SDK voor Node.js, de [volledige verwijzing](http://azure.github.io/azure-sdk-for-node/)te bekijken.

Beheer uw resources CDN met [PowerShell](./cdn-manage-powershell.md).

