<properties
    pageTitle="OAUTH-beveiliging in SaaS-Connectors en API Apps | Azure"
    description="Meer informatie over beveiliging OAUTH in de connectoren en API Apps in Azure App-Service. microservices architectuur; SaaS"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="mandia"/>


# <a name="learn-about-oauth-security-in-saas-connectors"></a>Informatie over de beveiliging van OAUTH in SaaS-connectors

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2014-12-01-voorbeeld.

Veel van de Software als een Service (SaaS)-connectors zoals Facebook, Twitter en DropBox moeten gebruikers verifiëren met behulp van het protocol OAUTH.  Wanneer u deze SaaS-connectoren van Apps logica, bieden we een vereenvoudigde gebruikersinterface waar u klikt u op "Machtigen" in de ontwerpfunctie voor Apps logica. Als u **machtigen**, u wordt gevraagd toestemming en log in (indien niet reeds) verbinding maken met de SaaS-service voor jou. Nadat u toestemming geven en autoriseren, kan uw logica Apps toegang krijgen tot deze services SaaS.

## <a name="create-your-own-saas-app"></a>Maak uw eigen SaaS-app
Deze vereenvoudigde ervaring is mogelijk omdat we eerder hebben gemaakt en de toepassing in deze services SaaS geregistreerd.  U kunt in bepaalde gevallen, registreren en gebruiken van uw eigen toepassing.  Dit is bijvoorbeeld nodig als u wilt deze SaaS verbindingslijnen gebruiken in aangepaste toepassingen. In dit voorbeeld wordt de verbindingslijn DropBox, maar het proces is hetzelfde voor alle verbindingslijnen die gebaseerd op OAUTH zijn.

Zelfs in de context van de logica Apps, kunt u uw eigen toepassing in plaats van de standaard-toepassing die we aanbieden. Als de knop 'Machtigen' geen verbinding maakt, kunt u proberen het maken van uw eigen app. Hieronder vindt u deze stappen voor de Twitter-connector:

1. Open uw Twitter-connector in de portal Azure voorbeeld. **Ga**naar > **API Apps**. Selecteer uw Twitter-connector:  
    ![][1]

2. Selecteer **Instellingen** > **verificatie**:  
    ![][2]

3. Kopieer de waarde van de **URI omgeleid** :  
    ![][3]

4. Ga naar [Twitter](http://apps.twitter.com) en **een nieuwe toepassing maken**. Plak de **Redirect URI** -waarde opgehaald uit uw Twitter-connector in de eigenschap **Callback-URL** : ![][4]  
5. Wanneer uw Twitter app is gemaakt, selecteert u **sleutel en Access Tokens**. Deze waarden kopiëren.
6. Plak deze waarden in de eigenschappen van **De Client-ID** en het **Geheim van de Client** in uw Twitter-connector verificatie-instellingen:   
    ![][5]  
7. Sla uw connectorinstellingen.  

Nu, moet u gebruikmaken van de connector van de logica Apps. Als u deze verbindingslijn logica Apps gebruikt, wordt uw toepassing gebruikt in plaats van de standaard-toepassing.  

> [AZURE.NOTE] Als u een app eerder hebt gemachtigd, moet u wellicht de app autoriseren.


<!--Image references-->
[1]: ./media/app-service-logic-oauth-security/TwitterConnector.png
[2]: ./media/app-service-logic-oauth-security/Authentication.png
[3]: ./media/app-service-logic-oauth-security/RedirectURI.png
[4]: ./media/app-service-logic-oauth-security/TwitterApp.png
[5]: ./media/app-service-logic-oauth-security/TwitterKeys.png
