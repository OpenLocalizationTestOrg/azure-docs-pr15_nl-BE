<properties
    pageTitle="App Service API Apps - wat er veranderd | Microsoft Azure"
    description="Informatie over nieuwe functies voor API Apps in Azure App-Service."
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps---whats-changed"></a>App Service API Apps - wat er veranderd

Bij de gebeurtenis Connect() in November 2015 zijn een aantal verbeteringen in Azure App Service [aangekondigd](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Deze verbeteringen omvatten wijzigingen van onderliggende API Apps beter uitlijnen met mobiele en Web Apps, concept telling en implementatie en runtime-prestaties te verbeteren. Vanaf 30 November 2015 nieuwe API apps u maken met behulp van de portal Azure management of de meest recente tooling, worden deze wijzigingen doorgevoerd. In dit artikel wordt beschreven, deze wijzigingen bij het implementeren van bestaande apps om te profiteren van de mogelijkheden.

## <a name="feature-changes"></a>Functionele wijzigingen
De belangrijkste functies van de API Apps – verificatie, metagegevens van CORS en API – hebt rechtstreeks in App-Service verplaatst. Met deze wijziging, de functies zijn beschikbaar via het Web, mobiele en API Apps. In feite delen alle drie dezelfde **Microsoft.Web/sites** brontype in Resource Manager. De gateway API Apps is niet langer nodig hebt of met API Apps aangeboden. Zo ook makkelijker Azure API beheer gebruiken, omdat er slechts de één API Management gateway.

![API Apps-overzicht](./media/app-service-api-whats-changed/api-apps-overview.png)

Een belangrijke beginsel met de API Apps update is zodat u kunt doen om uw API is in uw taal van keuze.  Als uw API is al geïmplementeerd als een Web App of mobiele App, hoeft u niet naar uw app om te profiteren van de nieuwe functies te implementeren. Als u op dit moment API Apps preview, worden richtlijnen voor migratie wordt hieronder beschreven.

### <a name="authentication"></a>Verificatie
De bestaande Sleutelklaar API Apps, mobiele Services/Apps en Web Apps verificatiefuncties hebt is unified en zijn beschikbaar in een enkele Azure App Service verificatie blade in de portal beheren. Zie voor een inleiding tot verificatieservices in App-Service, [verificatie van App-Service uit te breiden / vergunning](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

Er zijn een aantal relevante nieuwe mogelijkheden voor API-scenario's:

- **Ondersteuning voor het gebruik van Azure Active Directory rechtstreeks**zonder clientcode hoeft te wisselen de AAD token voor een sessietoken: uw client kan alleen de AAD tokens in de kop van de vergunning bevatten volgens de token aan toonder-specificatie. Dit betekent ook dat geen App servicespecifieke SDK op de client of server-kant nodig is. 
- **Service-service- of access "Internal"**: hebt u een proces of een andere client die toegang tot API's zonder een interface, u kunt een token met behulp van een principal AAD service aanvragen en doorgeven aan de App Service voor verificatie met uw toepassing.
- **Uitgestelde vergunning**: veel toepassingen kennen verschillende beperkingen voor verschillende gedeelten van de toepassing. Misschien wilt u sommige API's zijn algemeen beschikbaar, terwijl andere-in. De oorspronkelijke functie voor verificatie is ofwel volledig, ofwel, met de hele site waarvoor aanmelding is vereist. Deze optie bestaat nog steeds, maar kunt u ook uw toepassingscode toegang besluiten wordt weergegeven nadat de gebruiker is geverifieerd door App-Service.
 
Voor meer informatie over de nieuwe verificatiefuncties Zie [verificatie en machtiging van API Apps in Azure App-Service](app-service-api-authentication.md). Voor meer informatie over het migreren van Zie bestaande API apps uit het vorige model van API apps naar de nieuwe [migreren bestaande API apps](#migrating-existing-api-apps) verderop in dit artikel.
 
### <a name="cors"></a>CORS
In plaats van een door komma's gescheiden **MS_CrossDomainOrigins** app is er nu een blade in de portal Azure management voor het configureren van CORS. Ook kan worden geconfigureerd met behulp van bronbeheer gereedschap zoals Azure PowerShell, CLI of [Resource Explorer](https://resources.azure.com/). Stel de eigenschap **cors** voor het brontype van de **Microsoft.Web/sites/config** voor uw ** &lt;sitenaam&gt;/web** bron. Bijvoorbeeld:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>API-metagegevens
De definitie van API blade is nu beschikbaar via het Web, mobiele en API Apps. In de portal beheren kunt u een relatieve url of een absolute url die verwijst naar een eindpunt die hosts een 2.0 Swagger representatie van de API. Ook kan worden geconfigureerd met behulp van bronbeheer tooling. De eigenschap **apiDefinition** ingesteld op het type **Microsoft.Web/sites/config** voor uw ** &lt;site-naam&gt;/web** bron. Bijvoorbeeld:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

Op dit moment moet het eindpunt metagegevens openbaar toegankelijk zonder verificatie voor vele downstream-clients (bijvoorbeeld Visual Studio REST API-client genereren en PowerApps "API toevoegen" stroom) van kunnen maken. Dit betekent dat als u App Service verificatie gebruikt en de definitie van de API van binnen uw app zelf beschikbaar maken, moet u de eerder beschreven, zodat de route naar uw metagegevens Swagger openbare uitgestelde verificatie-optie te gebruiken.

## <a name="management-portal"></a>Beheerportal
Selecteren **Nieuw > Web + Mobile > API App** maken in de portal API apps die overeenkomen met de nieuwe mogelijkheden in het artikel beschreven. **Bladeren > API Apps** wordt alleen deze nieuwe API apps. Wanneer u naar een API-app bladert, deelt het blad dezelfde indeling en dezelfde mogelijkheden als die van het Web en mobiele Apps. De enige verschillen zijn quickstart inhoud en instellingen te bestellen.

Bestaande API apps (of marktplaats API apps gemaakt op basis van logica Apps) met het vorige voorbeeld mogelijkheden nog steeds zichtbaar in de ontwerpfunctie voor Apps logica en tijdens het bladeren door alle resources in een resourcegroep.

## <a name="visual-studio"></a>Visual Studio

De meeste Web Apps gereedschap werkt samen met de nieuwe API apps omdat ze hetzelfde onderliggende **Microsoft.Web/sites** resourcetype delen. De Azure Visual Studio, gereedschap, echter moet worden bijgewerkt naar versie van punt 2.8.1 of hoger omdat een aantal mogelijkheden die specifiek zijn voor API's worden getoond. De SDK downloaden vanaf de [pagina downloads Azure](https://azure.microsoft.com/downloads/).

Publiceren met de rationalisatie van het type App Service is ook onder uniforme **publiceren > Microsoft Azure App Service**:

![API Apps publiceren](./media/app-service-api-whats-changed/api-apps-publish.png)

Lees meer informatie over de SDK van punt 2.8.1, de aankondiging [van blogberichten](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).

U kunt ook handmatig het publicatieprofiel importeren vanaf de portal management publiceren inschakelen. Echter Cloud Explorer, codegeneratie en API app selectie maken of moet de SDK van punt 2.8.1 of hoger.

## <a name="migrating-existing-api-apps"></a>Migratie van bestaande API apps
Als uw aangepaste API is geïmplementeerd op de vorige versie van het voorbeeld van API Apps, vragen we te migreren naar het nieuwe model voor API Apps door 31 December 2015. Aangezien zowel het oude als het nieuwe model zijn gebaseerd op Web-API's die worden gehost in App-Service, het merendeel van de bestaande code opnieuw kan worden gebruikt.

### <a name="hosting-and-redeployment"></a>Hosting en hergebruik
De stappen voor het opnieuw distribueren van zijn hetzelfde als een bestaande Web-API implementeert op App-Service. Stappen:

1. Maak een lege API app. Dit kan gebeuren in de portal met New > in Visual Studio van publiceren, of bronnenbeheerder tooling API App. Als Resource Manager tooling of sjablonen gebruikt, stelt u de waarde van het **type** **API** op het type **Microsoft.Web/sites** hebben de QuickStart en instellingen in de portal management is gericht op de API-scenario's.
2. Verbinding maken en implementeren van uw project lege API App met behulp van de mechanismen voor implementatie wordt ondersteund door de App Service. Lees de [documentatie over de implementatie van Azure App-Service](../app-service-web/web-sites-deploy.md) voor meer informatie. 
  
### <a name="authentication"></a>Verificatie
De verificatieservices van App-Service ondersteunen dezelfde mogelijkheden die met het vorige model van API Apps beschikbaar waren. Als u sessie-tokens en SDK's vereisen, gebruikt u de volgende client- en SDK's:

- Client: [Azure mobiele Client SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- Server: [Microsoft Azure mobiele App .NET verificatie-extensie](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Als u in plaats daarvan het alpha App Service SDK's, worden deze nu vervangen:

- Client: [Microsoft Azure AppService SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

In het bijzonder met Azure Active Directory is echter geen App servicespecifieke vereist als u van het token AAD rechtstreeks gebruikmaakt.

### <a name="internal-access"></a>Interne toegang
Het vorige model van API Apps een ingebouwde interne toegangsniveau opgenomen. Dit vereist het gebruik van de SDK voor het ondertekenen van aanvragen. Zoals eerder beschreven, met de nieuwe API Apps model kunnen AAD service beveiligings-principals worden gebruikt als een alternatief voor de verificatie van services zonder een App servicespecifieke SDK. Lees meer in [Service principal verificatie voor API Apps in Azure App-Service](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Discovery
Het vorige model van API Apps hadden API's voor andere API apps in runtime in dezelfde bronnengroep achter de gateway dezelfde ontdekken. Dit is vooral handig voor architecturen die patronen microservice implementeren. Terwijl dit niet direct wordt ondersteund, wordt een aantal opties zijn beschikbaar:

1. De Azure Resource Manager-API gebruiken voor discovery.
2. Azure API beheer voor uw gehoste App Service-API's plaatsen. Azure API beheer dient als een gevel en een stabiele gerichte externe url kan bieden, zelfs als u interne topologie wordt gewijzigd.
3. Bouw uw eigen discovery API app en hebben andere API apps registreren met de discovery-app bij het opstarten.
4. Bij de implementatie, de app instellingen van alle API apps (en clients) te vullen met de eindpunten van de andere API apps. Dit is haalbaar in implementaties van de sjabloon en de API Apps kunt u nu de controle van de url.

## <a name="using-api-apps-with-logic-apps"></a>Met behulp van API Apps met logica Apps

Het nieuwe model van API apps werkt goed samen met [Logica Apps schemaversie 08-01-2015](../app-service-logic/app-service-logic-schema-2015-08-01.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie leest u de artikelen in de [sectie van de API-documentatie voor Apps](https://azure.microsoft.com/documentation/services/app-service/api/). Ze zijn bijgewerkt om aan te geven van het nieuwe model voor API Apps. Bovendien bereiken in de forums voor aanvullende informatie of advies over migratie:

- [MSDN-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-api-apps)
