<properties 
    pageTitle="Aan de slag met Azure Search Management REST API | Microsoft Azure | De zoekservice hosted cloud" 
    description="Beheer uw gehoste wolk Azure Search-service met een API voor beheer van REST" 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="get-started-with-azure-search-management-rest-api"></a>Aan de slag met Azure Search Management REST API
> [AZURE.SELECTOR]
- [Portal](search-manage.md)
- [PowerShell](search-manage-powershell.md)
- [REST-API](search-get-started-management-api.md)

De REST van Azure Search API voor beheer is een programmatische alternatief voor het uitvoeren van beheertaken in de portal. Service management-activiteiten omvatten het maken of verwijderen van de service, de service schalen en sleutels beheren. Deze zelfstudie bevat een voorbeeldtoepassing client waarop u de-API voor servicebeheer. Daarnaast configuratiestappen vereist voor het uitvoeren van het monster in uw omgeving voor plaatselijke ontwikkeling.

Deze zelfstudie hebt u het volgende nodig:

- Visual Studio 2012 of 2013
- het monster client toepassing downloaden

In het kader van de zelfstudie is voltooid, twee services ingericht: Azure Search en Azure Active Directory (AD). Bovendien maakt u een AD-toepassing die de vertrouwensrelatie tussen de clienttoepassing en de resource manager eindpunt in Azure te worden ingesteld.

U moet een account Azure voor het voltooien van deze zelfstudie.


##<a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

Deze zelfstudie is gebaseerd op een Windows-consoletoepassing die is geschreven in C#, die u kunt bewerken en uitvoeren in Visual Studio 2012 of 2013

U vindt de clienttoepassing op Github op [Azure Search.NET Management API Demo](https://github.com/Azure-Samples/search-dotnet-management-api/).


##<a name="configure-the-application"></a>De toepassing configureren

Voordat u de voorbeeldtoepassing uitvoeren kunt, moet u verificatie inschakelen zodat aanvragen die worden verzonden van de clienttoepassing naar het eindpunt resource manager kunnen worden geaccepteerd. De verificatievereiste voortkomt uit de [Azure Resource Manager](https://msdn.microsoft.com/library/azure/dn790568.aspx), die de basis voor alle portal-gerelateerde bewerkingen aangevraagd via een API vormt, met inbegrip van die welke betrekking hebben op de zoekservice beheren. De API voor servicebeheer voor Azure Search is gewoon een uitbreiding van de bronnenbeheerder Azure en dus neemt de afhankelijkheden.  

Azure Resource Manager vereist Azure Active Directory-service als de id-provider. 

Als u een toegangstoken waarmee aanvragen voor het bereiken van de bronnenbeheerder, bevat de clienttoepassing een codesegment waarin Active Directory wordt aangeroepen. Het codesegment, plus de vereiste stappen voor het gebruik van het codesegment zijn geleend uit dit artikel: [Azure Resource Manager verificatie aanvragen](http://msdn.microsoft.com/library/azure/dn790557.aspx).

U kunt de instructies in de bovenstaande link of de stappen in dit document gebruiken als u liever de handleiding stap voor stap doorlopen.

In deze sectie, wordt u de volgende taken uitvoeren:

1. Maak een AD-service
1. Een AD-toepassing maken
1. De AD-toepassing configureren door te registreren gegevens over de monster-clienttoepassing die u hebt gedownload
1. Laden van de voorbeeldtoepassing client met waarden die te krijgen van de vergunning voor de aanvragen wordt gebruikt

> [AZURE.NOTE] Deze koppelingen leiden naar achtergrond op Azure Active Directory gebruikt voor het verifiëren van aanvragen van clients naar de bronnenbeheerder: [Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790568.aspx), [Azure Resource Manager verificatie aanvragen](http://msdn.microsoft.com/library/azure/dn790557.aspx)en [Azure Active Directory](http://msdn.microsoft.com/library/azure/jj673460.aspx).

###<a name="create-an-active-directory-service"></a>Maak een Active Directory-Service

1. Aanmelden bij de [Azure Portal](https://manage.windowsazure.com).

2. Blader in het linkernavigatievenster en klik op **Active Directory**.

4. Klik op **Nieuw** om te openen **App Services** | **Active Directory**. In deze stap maakt u een nieuwe Active Directory-service. Deze service host fungeert voor de AD-toepassing zult u enkele stappen vanaf nu definiëren. Een nieuwe service maken Hiermee kunt u de zelfstudie van andere toepassingen die u al als host in Azure fungeren mogelijk.

5. Klik op de **map** | **aangepaste maken**.

6. Voer een naam, domein en geo-locatie. Het domein moet uniek zijn. Klik op het selectievakje om de service te maken.

     ![][5]

###<a name="create-a-new-ad-application-for-this-service"></a>Maak een nieuwe toepassing AD voor deze service

1. Selecteer de 'SearchTutorial' Active Directory service die u zojuist hebt gemaakt.

2. Klik in het bovenste menu op **toepassingen**. 
 
3. Klik op **een toepassing toevoegen**. Een AD-toepassing wordt informatie opgeslagen over de clienttoepassingen die zal worden gebruikt als een id-provider.  
 
4. Kies **een toepassing die het ontwikkelen van mijn organisatie toevoegen**. Deze optie biedt de registratie-instellingen voor toepassingen die niet zijn gepubliceerd in de galerie van toepassing. Aangezien de clienttoepassing geen deel uit van de galerie toepassing maakt, is dit de juiste keuze voor deze zelfstudie.

     ![][6]
 
5. Voer een naam, bijvoorbeeld 'Azure Search Manager'.

6. **Native clienttoepassing** voor toepassingstype kiezen. Dit is juist voor de voorbeeldtoepassing; Dit gebeurt als een Windows-client (console)-toepassing niet in een webtoepassing.

     ![][7]
 
7. Geef in URI omleiden, 'http://localhost/Azure-Search-Manager-App'. Dit een URI die Azure Active Directory user-agent in reactie op een aanvraag van de vergunning OAuth 2.0 worden omgeleid. De waarde hoeft niet te worden het fysieke eindpunt aan, maar u moet een geldige URI. 

    Voor de toepassing van deze zelfstudie, de waarde kan van alles zijn, maar wat u invoert, wordt een vereiste invoer voor de administratieve verbinding in de voorbeeldtoepassing. 
 
7. Klik op het selectievakje om de Active Directory-toepassing te maken. 'Azure-zoeken-Manager-App' ziet u in het linker navigatievenster.

###<a name="configure-the-ad-application"></a>Configureer de AD-toepassing
 
9. Klik op de toepassing AD "Azure-zoeken-Manager-App", die u zojuist hebt gemaakt. U ziet dat deze vermeld in het linkernavigatievenster.

10. Klik op **configureren** in het bovenste menu.
 
11. Ga naar machtigingen en selecteer **Azure Management API**. In deze stap maakt u de API (in dit geval de bronnenbeheerder Azure API) opgeven dat de clienttoepassing moet toegang tot, en het niveau van toegang nodig is.

12. Klik op de vervolgkeuzelijst lijst en selecteer overgedragen machtigingen **toegangsbeheer Azure Service (voorbeeld**).
 
     ![][8]
 
13. De wijzigingen worden opgeslagen. 

Houd de configuratiepagina van de toepassing geopend. In de volgende stap u waarden van deze pagina kopiëren en deze invoeren in de voorbeeldtoepassing.

###<a name="load-the-sample-application-program-with-registration-and-subscription-values"></a>Het monster toepassing laden met waarden voor registratie en abonnement

In dit gedeelte vindt gaat u de oplossing in Visual Studio, vervangen door geldige waarden, verkregen uit de portal bewerken.
De waarden die u wilt toevoegen aan de bovenkant van de Program.cs wordt weergegeven:

        private const string TenantId = "<your tenant id>";
        private const string ClientId = "<your client id>";
        private const string SubscriptionId = "<your subscription id>";
        private static readonly Uri RedirectUrl = new Uri("<your redirect url>");

Als u nog niet [gedownload van de voorbeeldtoepassing van Github hebt](https://github.com/Azure-Samples/search-dotnet-management-api/), moet u deze voor deze stap.

1. Open het **ManagementAPI.sln** in Visual Studio.

2. Open Program.cs.

3. Geef `ClientId`. Vanuit de toepassing AD configuratie pagina links openen uit de vorige stap, de Client-ID van de pagina configuratie AD in de portal te kopiëren en plakken Program.cs.

4. Geef `RedirectUrl`. Redirect URI van dezelfde portal pagina kopiëren en plakken in de Program.cs.

    ![][9]

5. Bieden`TenantID.` 
    - Ga terug naar Active Directory | SearchTutorial (service). 
    - Klik op **toepassingen** van de bovenste balk. 
    - Klik op de **Eindpunten van de weergave** onder aan de pagina. 
    - Kopieer het eindpunt OAUTH 2.0 vergunning onder aan de lijst. 
    - Plak het eindpunt in TenantID, bijsnijden van de waarde van alle URI parameters, met uitzondering van de huurder-ID.

    Bepaalde 'https://login.windows.net/55e324c7-1656-4afe-8dc3-43efcd4ffa50/oauth2/authorize?api-version=1.0', Verwijder alles behalve de '55e324c7-1656-4afe-8dc3-43efcd4ffa50'.

    ![][10]

6. Geef `SubscriptionID`.
    - Ga naar de portal-pagina.
    - Klik op **Instellingen** onder in het linkernavigatievenster.
    - Op het tabblad abonnementen de abonnement-ID kopiëren en plakken in de Program.cs.

7. Opslaan en vervolgens de oplossing bouwen.


##<a name="explore-the-application"></a>Verken de toepassing

Een onderbrekingspunt toevoegen aan de eerste aanroep van de methode die wordt doorlopen. Druk op **F5** om de toepassing te starten en druk op **F11** om de code te doorlopen.

De voorbeeldtoepassing maakt een gratis Azure Search-service voor een abonnement Azure. Als er al een gratis service voor uw abonnement bestaat, mislukt de voorbeeldtoepassing. Gratis zoekservice per abonnement slechts één is toegestaan.

1. Open Program.cs in de Solution Explorer en Ga naar de functie Main (string [] void). 
 
3. U ziet dat de **ExecuteArmRequest** wordt gebruikt om te verzoeken het eindpunt Azure Resource Manager `https://management.azure.com/subscriptions` voor een opgegeven `subscriptionID`. Deze methode wordt gebruikt in het programma met behulp van de zoek-API voor beheer of Azure Resource Manager API bewerkingen uit te voeren.

3. Aanvragen Azure Resource Manager moeten worden geverifieerd en gemachtigd. Dit wordt bereikt met behulp van de methode **GetAuthorizationHeader** wordt aangeroepen door de methode **ExecuteArmRequest** , geleend van [Azure Resource Manager verificatie aanvragen](http://msdn.microsoft.com/library/azure/dn790557.aspx). Merk op dat **GetAuthorizationHeader** roept `https://management.core.windows.net` om een toegangstoken.

4. U wordt gevraagd aan te melden met een gebruikersnaam en wachtwoord die geldig is voor uw abonnement.

5. Vervolgens wordt is een nieuwe Azure Search-service geregistreerd bij de provider Azure Resource Manager. Nogmaals, dit is de methode **ExecuteArmRequest** , deze keer gebruikt voor het maken van de zoekservice op Azure voor uw abonnement via `providers/Microsoft.Search/register`. 

6. De rest van het programma gebruikt de [Azure Search Management REST API](http://msdn.microsoft.com/library/dn832684.aspx). Merk op dat de `api-version` voor deze API van de bronnenbeheerder Azure api-versie verschilt. Bijvoorbeeld `/listAdminKeys?api-version=2014-07-31-Preview` geeft de `api-version` van de REST Azure Search beheer-API.

    De volgende reeks bewerkingen ophalen de servicedefinitie net gemaakt, de beheer-api-sleutels, worden opnieuw gegenereerd en sleutels worden opgehaald, de replica en de partitie en ten slotte de service verwijderen.

    Wanneer u de service replica of partitie aantal wijzigt, wordt verwacht dat deze actie niet als u de gratis versie. Alleen de standard edition kunt u het gebruik van extra partities en replica's.

    Als u de service verwijdert, is de laatste bewerking.

##<a name="next-steps"></a>Volgende stappen

Na nadat deze zelfstudie is voltooid, kunt u meer informatie over service management of verificatie met Active Directory-service:

- Meer informatie over een clienttoepassing integreren met Active Directory. Zie [integratie van toepassingen in Azure Active Directory](http://msdn.microsoft.com/library/azure/dn151122.aspx).
- Meer informatie over andere service management-activiteiten in Azure. Zie [uw Services beheren](http://msdn.microsoft.com/library/azure/dn578292.aspx).

<!--Anchors-->
[Download the sample application]: #Download
[Configure the application]: #config
[Explore the application]: #explore
[Next Steps]: #next-steps

<!--Image references-->
[5]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-Service.PNG
[6]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App.PNG
[7]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App-prop.PNG
[8]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPermissions.PNG
[9]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPage.PNG
[10]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-OAuthEndpoint.PNG

<!--Link references-->
[Manage your search solution in Microsoft Azure]: search-manage.md
[Azure Search development workflow]: search-workflow.md
[Create your first azure search solution]: search-create-first-solution.md
[Create a geospatial search app using Azure Search]: search-create-geospatial.md


 
