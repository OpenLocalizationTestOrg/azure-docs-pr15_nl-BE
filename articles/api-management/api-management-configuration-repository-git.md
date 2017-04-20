<properties 
    pageTitle="Het opslaan en de API-Management service-configuratie met Git configureren" 
    description="Informatie over het opslaan en de API-Management service-configuratie met Git configureren." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Het opslaan en de API-Management service-configuratie met Git configureren

>[AZURE.IMPORTANT] GIT configuratie voor de beheer-API is momenteel in de voorvertoning. Functioneel voltooid is, maar is in voorbeeld omdat we zijn actief op zoek naar feedback over deze functie. Het is mogelijk dat wij een recente wijzigen in reactie op feedback van klanten, dus het is raadzaam niet afhankelijk van de functie voor gebruik in een productieomgeving kan maken. Als u reacties of vragen hebt, laat het ons weten op `apimgmt@microsoft.com`.

Elk exemplaar van de service Management API onderhoudt een configuratiedatabase met met informatie over de configuratie en de metagegevens voor de service-exemplaar. Wijzigingen kunnen worden aangebracht op het exemplaar van de door het wijzigen van een instelling in de publisher-portal, een PowerShell-cmdlet gebruikt of een REST API-aanroep. Naast deze methoden kunt u de serviceconfiguratie exemplaar met behulp van Git, service management-scenario's, zoals inschakelen beheren:

-   Configuratie van versiebeheer - downloaden en opslaan van verschillende versies van de configuratie van de service
-   Wijzigingen in de configuratie in bulk - wijzigingen aanbrengen in meerdere delen van de configuratie van de service in uw lokale bibliotheek en de wijzigingen naar de server worden geïntegreerd met een enkele bewerking
-   Bekende Git toolchain en workflow - gebruik van het Git tooling en werkstromen die u al bekend met bent

In het volgende diagram ziet u een overzicht van de verschillende manieren om uw API Management service-exemplaar configureren.

![GIT configureren][api-management-git-configure]

Wanneer u wijzigingen in uw service met behulp van de publisher-portal, PowerShell-cmdlets of de REST API aanbrengt, u beheert uw service configuration database met behulp van de `https://{name}.management.azure-api.net` eindpunt, zoals wordt weergegeven aan de rechterkant van het diagram. Aan de linkerkant van het diagram ziet u hoe u de configuratie van de service met behulp van Git kunt beheren en Git repository voor uw service te vinden op `https://{name}.scm.azure-api.net`.

De volgende stappen geven een overzicht van het beheren van uw API Management service-exemplaar met behulp van Git.

1.  Git toegang in uw service inschakelen
2.  De service configuratiedatabase opslaan in de Git repository
3.  Clone de Git repo naar de computer.
4.  De meest recente repo naar uw lokale computer en wijzigingen doorvoeren en push pull-terug naar uw repo
5.  De wijzigingen van de repo in de configuratiedatabase service implementeren

In dit artikel wordt beschreven hoe Git gebruikt voor het beheren van de configuratie van de service en biedt een verwijzing voor de bestanden en mappen in de Git repository.

## <a name="to-enable-git-access"></a>Git-toegang inschakelen

U kunt snel de status van de Git configuratie weergeven door het Git-pictogram in de rechterbovenhoek van de portal voor publisher weer. In dit voorbeeld is nog niet Git toegang ingeschakeld.

![GIT status][api-management-git-icon-enable]

Als u wilt weergeven en configureren van de Git configuratie-instellingen, kunt u op het pictogram Git of klikt u op het menu **beveiliging** en Ga naar het tabblad **configuratie opslagplaats** .

![GIT inschakelen][api-management-enable-git]

Schakel het selectievakje in **access Git inschakelen** zodat Git toegang.

Na een moment dat de wijziging wordt opgeslagen en wordt een bevestigingsbericht weergegeven. Houd er rekening mee dat Git pictogram kleur om aan te geven dat Git toegang is ingeschakeld en het statusbericht geeft nu aan dat er niet-opgeslagen wijzigingen in de opslagplaats is gewijzigd. Dit komt omdat de configuratiedatabase API Management-service nog niet aan de bibliotheek zijn opgeslagen.

![GIT is ingeschakeld][api-management-git-enabled]

>[AZURE.IMPORTANT] Geen geheimen die niet zijn gedefinieerd als de eigenschappen worden opgeslagen in de opslagplaats en in haar Geschiedenis bewaard totdat u uitschakelen en opnieuw inschakelen Git. Eigenschappen bieden een veilige plaats voor het beheren van constante string-waarden, met inbegrip van geheimen, voor alle API-configuratie en -beleid, zodat u niet hoeft op te slaan ze rechtstreeks in uw beleidsverklaringen. Zie [Eigenschappen van een beleid voor Azure API gebruiken](api-management-howto-properties.md)voor meer informatie.

Zie voor informatie over het in- of uitschakelen van Git toegang met behulp van de REST-API's [in- of uitschakelen van Git toegang met behulp van de API van de REST](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>Configuratie van de service op de Git repository opslaan

De eerste stap voor het klonen van de opslagplaats is de huidige status van de configuratie van de service op de bibliotheek opslaan. Klik op **configuratie opslaan naar de bibliotheek**.

![Configuratie opslaan][api-management-save-configuration]

Breng de gewenste wijzigingen in het bevestigingsvenster en klik op **Ok** om op te slaan.

![Configuratie opslaan][api-management-save-configuration-confirm]

Na enkele seconden duren voordat de configuratie wordt opgeslagen en de status van de configuratie van de bibliotheek wordt weergegeven, inclusief de datum en tijd van de laatste wijziging in de configuratie en de laatste synchronisatie tussen de configuratie van de service en de opslagplaats.

![, Configuratiestatus][api-management-configuration-status]

Zodra de configuratie wordt opgeslagen in de bibliotheek, kunnen worden gekloond.

Voor meer informatie over het uitvoeren van deze bewerking met de REST API Zie [configuratie momentopname met de REST API doorvoeren](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>De bibliotheek op uw lokale computer kopiëren

Klonen van een bibliotheek, moet u de URL van de opslagplaats, een gebruikersnaam en een wachtwoord. De gebruikersnaam en de URL worden weergegeven boven aan het tabblad **configuratie opslagplaats** .

![GIT clone][api-management-configuration-git-clone]

Het wachtwoord wordt onder aan het tabblad **configuratie opslagplaats** gegenereerd.

![Wachtwoord genereren][api-management-generate-password]

Eerst zorgen dat de **vervaldatum** is ingesteld op de gewenste datum en tijd voor het genereren van een wachtwoord, en klik op **Token genereren**.

![Wachtwoord][api-management-password]

>[AZURE.IMPORTANT] Noteer dit wachtwoord. Als u deze pagina verlaat het wachtwoord niet opnieuw weergegeven.

In de volgende voorbeelden gebruiken Git Bash van [Git voor Windows](http://www.git-scm.com/downloads) , maar u kunt Git-hulpprogramma dat u bekend met bent.

De Git tool openen in de gewenste map en voer de volgende opdracht om de clone de git repository naar uw lokale computer, met behulp van de opdracht is verstrekt door de uitgever portal.

    git clone https://bugbashdev4.scm.azure-api.net/ 

Geef de gebruikersnaam en het wachtwoord wanneer daarom wordt gevraagd.

Als u fouten ontvangt, probeer het wijzigen van de `git clone` opdracht op te nemen van de gebruikersnaam en wachtwoord, zoals in het volgende voorbeeld.

    git clone https://username:password@bugbashdev4.scm.azure-api.net/

Als dit een fout bevat, kunt u URL-codering van het wachtwoord-gedeelte van de opdracht. Een snelle manier om dit te doen is open Visual Studio en geef de volgende opdracht in het **Venster Direct**. Open het **Venster Direct**, een oplossing of project openen in Visual Studio (of maak een nieuwe lege consoletoepassing), en kies **Windows**, **direct** in het menu **Foutopsporing** .

    ?System.NetWebUtility.UrlEncode("password from publisher portal")

Het gecodeerde wachtwoord en de locatie van de naam en locatie gebruiken om de opdracht git samen te stellen.

    git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/

U kunt weergeven en ermee werken in uw lokale bestandssysteem zodra de opslagplaats is gekloond. Zie voor meer informatie, [bestanden en mappen structuur verwijzing naar lokale Git repository](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>Uw lokale bibliotheek bijwerken met de meest recente configuratie van de service-exemplaar

Als u wijzigingen in uw exemplaar van de service Management API in de publisher-portal aanbrengt of met de REST API, moet u deze wijzigingen opslaan naar de opslagplaats voordat u uw lokale bibliotheek met de laatste wijzigingen bijwerken kunt. Hiertoe klikt u op het tabblad **configuratie opslagplaats** in de publisher-portal op **configuratie opslaan naar de bibliotheek** en geef vervolgens de volgende opdracht in uw lokale bibliotheek.

    git pull

Voordat u `git pull` zorgen ervoor dat u in de map voor uw lokale bibliotheek. Als u zojuist hebt voltooid de `git clone` en vervolgens moet u de map op uw repo door het uitvoeren van een opdracht de volgende opdracht.

    cd bugbashdev4.scm.azure-api.net/

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Om wijzigingen naar de server repo van uw lokale repo

Om push wijzigingen uit uw lokale bibliotheek naar de bibliotheek van de server, moet u uw wijzigingen en ze vervolgens naar de opslagplaats server push. Als u wilt uw wijzigingen doorvoeren, uw opdracht Git programma openen, Ga naar de map van uw lokale bibliotheek en de volgende opdrachten.

    git add --all
    git commit -m "Description of your changes"

Om alle de doorvoercoördinatie pushen naar de server, moet u de volgende opdracht uitvoeren.

    git push

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>Eventuele wijzigingen in de serviceconfiguratie naar het exemplaar van de beheer-API implementeren

Zodra de lokale wijzigingen zijn doorgevoerd en geduwd aan de opslagplaats van de server, kunt u deze kunt implementeren in uw exemplaar van de service Management API.

![Implementeren][api-management-configuration-deploy]

Zie voor informatie over het uitvoeren van deze bewerking met de REST API's [implementeren Git wijzigingen in de configuratiedatabase de REST API](https://msdn.microsoft.com/library/dn781420.aspx#DeployChanges).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Verwijzing naar een bestand en de map structuur van lokale Git repository

De bestanden en mappen in de opslagplaats lokale git bevatten de configuratie-informatie over het service-exemplaar.

| Artikel                       | Beschrijving                                                                                |
|-------------------------   |--------------------------------------------------------------------------------------------|
| hoofdmap van de api-management | Op het hoogste niveau van de configuratie voor de service-exemplaar bevat                                  |
| map met API 's                | Bevat de configuratie voor de API's van het exemplaar van de                            |
| de map groepen              | Bevat de configuratie voor de groepen op het exemplaar van de                          |
| map beleid            | Bevat het beleid in het exemplaar van de                                              |
| map portalStyles        | Bevat de configuratie voor de developer portal aanpassingen in het exemplaar van de |
| map Products            | Bevat de configuratie voor de producten in het exemplaar van de                        |
| map met sjablonen           | Bevat de configuratie voor de e-mailsjablonen in het exemplaar van de                 |

Elke map kan een of meer bestanden bevatten, en in sommige gevallen een of meer mappen, bijvoorbeeld een map voor elke API, product of groep. De bestanden in elke map zijn specifiek voor het entiteitstype dat wordt beschreven door de naam van de map.

| Bestandstype | Doel                                                                |
|-----------|------------------------------------------------------------------------|
| JSON      | Configuratie-informatie over de desbetreffende entiteit                  |
| HTML-code      | Beschrijvingen van de entiteit, vaak weergegeven in de developer-portal |
| XML       | Beleidslijnen                                                      |
| CSS       | Opmaakmodellen voor developer portal aanpassen                        |

Deze bestanden kunnen worden gemaakt, verwijderd, bewerkt en beheerd op het lokale bestandssysteem en de geïmplementeerd wijzigingen weer met de service-exemplaar van uw beheer-API.

>[AZURE.NOTE] De volgende entiteiten zijn niet opgenomen in de Git repository en kunnen niet worden geconfigureerd met behulp van Git.
>
>-    Gebruikers
>-    Abonnementen
>-    Eigenschappen
>-    Developer portal entiteiten dan stijlen

### <a name="root-api-management-folder"></a>Hoofdmap van de api-management

De hoofdmap `api-management` map bevat een `configuration.json` bestand dat op het hoogste niveau informatie over het exemplaar van de in de volgende indeling bevat.

    {
      "settings": {
        "RegistrationEnabled": "True",
        "UserRegistrationTerms": null,
        "UserRegistrationTermsEnabled": "False",
        "UserRegistrationTermsConsentRequired": "False",
        "DelegationEnabled": "False",
        "DelegationUrl": "",
        "DelegatedSubscriptionEnabled": "False",
        "DelegationValidationKey": ""
      },
      "$ref-policy": "api-management/policies/global.xml"
    }

De eerste vier instellingen (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, en `UserRegistrationTermsConsentRequired`) toewijzen aan de volgende instellingen op het tabblad **identiteit** in de sectie **beveiliging** .

| Identiteit                     | Toegewezen aan                                               |
|--------------------------------------|-------------------------------------------------------|
| RegistrationEnabled                  | **Anonieme gebruikers om te leiden - aanmeldingspagina** checkbox |
| UserRegistrationTerms                | **Gebruiksvoorwaarden van aanmelding gebruiker** textbox               |
| UserRegistrationTermsEnabled         | Checkbox **gebruiksvoorwaarden op de aanmeldingspagina weergeven**         |
| UserRegistrationTermsConsentRequired | Selectievakje voor **toestemming vereist**                          |

![Instellingen van identiteit][api-management-identity-settings]

De volgende vier instellingen (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, en `DelegationValidationKey`) toewijzen aan de volgende instellingen op het tabblad **overdracht** in de sectie **beveiliging** .

| Overdracht, instelling           | Toegewezen aan                                    |
|------------------------------|--------------------------------------------|
| DelegationEnabled            | Selectievakje **gemachtigde inloggen & aanmelden**    |
| DelegationUrl                | **Overdracht eindpunt-URL** textbox        |
| DelegatedSubscriptionEnabled | Selectievakje **gemachtigde product abonnement** |
| DelegationValidationKey      | **Gemachtigde validatiesleutel** textbox        |

![Overdrachtinstellingen][api-management-delegation-settings]

De instelling van de definitieve `$ref-policy`, worden toegewezen aan het bestand met de algemene instructies voor het exemplaar van de.

### <a name="apis-folder"></a>map met API 's

De `apis` een map voor elke API in het service-exemplaar bevat de volgende items bevat.

-   `apis\<api name>\configuration.json`-Dit is de configuratie van de API en bevat informatie over de bewerkingen en de URL van de back-end. Dit is dezelfde informatie die wordt geretourneerd als u belt [krijgt u een specifieke API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) met `export=true` in `application/json` indeling.
-   `apis\<api name>\api.description.html`-Dit is de beschrijving van de API en komt overeen met de `description` eigenschap van de [API-entiteit](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).
-   `apis\<api name>\operations\`-Deze map bevat `<operation name>.description.html` bestanden die zijn toegewezen aan de bewerkingen in de API. Elk bestand bevat de omschrijving van een enkele bewerking in de API die wordt toegewezen aan de `description` eigenschap van de [entiteit van de bewerking](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) in de REST API.

### <a name="groups-folder"></a>de map groepen

De `groups` een map voor elke groep is gedefinieerd in het service-exemplaar bevat.

-   `groups\<group name>\configuration.json`-Dit is de configuratie voor de groep. Dit is dezelfde informatie die wordt geretourneerd als u de bewerking voor het [ophalen van een bepaalde groep](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) aanroepen.
-   `groups\<group name>\description.html`-Dit is de omschrijving van de groep en komt overeen met de `description` eigenschap van de [entiteit van de groep](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>map beleid

De `policies` map bevat de beleidslijnen voor uw service-exemplaar.

-   `policies\global.xml`-beleid dat is gedefinieerd met globaal bereik voor uw service-exemplaar bevat.
-   `policies\apis\<api name>\`-Als er een beleid is gedefinieerd in het bereik van de API, bevinden zich in deze map.
-   `policies\apis\<api name>\<operation name>\`map - als u alle beleidsregels die zijn gedefinieerd in het bewerkingsbereik, zijn opgenomen in deze map in `<operation name>.xml` bestanden die zijn toegewezen aan de beleidslijnen voor elke bewerking.
-   `policies\products\`-Als u gedefinieerd op de productomvang beleid hebt, bevinden zich in deze map bevat `<product name>.xml` bestanden die zijn toegewezen aan de beleidslijnen voor elk product.

### <a name="portalstyles-folder"></a>map portalStyles

De `portalStyles` map bevat configuratie- en opmaakmodellen voor developer portal aanpassingen voor de service-exemplaar.

-   `portalStyles\configuration.json`-bevat de namen van de opmaakmodellen die worden gebruikt door de developer-portal
-   `portalStyles\<style name>.css`-elke `<style name>.css` -bestand bevat stijlen voor de developer-portal (`Preview.css` en `Production.css` standaard).

### <a name="products-folder"></a>map Products

De `products` een map voor elk product dat is gedefinieerd in het service-exemplaar bevat.

-   `products\<product name>\configuration.json`-Dit is de configuratie voor het product. Dit is dezelfde informatie die wordt geretourneerd als u belt, de werking van [een bepaald product](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) .
-   `products\<product name>\product.description.html`-Dit is de omschrijving van het product en komt overeen met de `description` eigenschap van het [product, entiteit](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) in de REST API.

### <a name="templates"></a>sjablonen

De `templates` map bevat de configuratie voor de [e-mailsjablonen](api-management-howto-configure-notifications.md) van de service-exemplaar.

-   `<template name>\configuration.json`-Dit is de configuratie voor de e-mailsjabloon.
-   `<template name>\body.html`-Dit is de hoofdtekst van het e-mailsjabloon.

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over andere manieren voor het beheren van uw service-exemplaar:

-   Beheren van uw service-exemplaar met de volgende PowerShell-cmdlets
    -   [Deployment service PowerShell-cmdlet referentie](https://msdn.microsoft.com/library/azure/mt619282.aspx)
    -   [Service management PowerShell-cmdlet referentie](https://msdn.microsoft.com/library/azure/mt613507.aspx)
-   Uw service-exemplaar in de publisher-portal beheren
    -   [Uw eerste API beheren](api-management-get-started.md)
-   Uw service-exemplaar met de REST API beheren
    -   [API beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Bekijk een video-overzicht

> [AZURE.VIDEO configuration-over-git]

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




