<properties 
    pageTitle="Meldingen en e-mailsjablonen in Azure API beheer configureren" 
    description="Informatie over het configureren van kennisgevingen en e-mailsjablonen in Azure API beheer." 
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

# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Meldingen en e-mailsjablonen in Azure API beheer configureren

Beheer-API biedt de mogelijkheid voor het configureren van meldingen voor specifieke gebeurtenissen en voor het configureren van de e-mailsjablonen die worden gebruikt om te communiceren met de beheerders en ontwikkelaars van een exemplaar van de beheer-API. In dit onderwerp ziet u hoe het configureren van kennisgevingen voor de beschikbare gebeurtenissen en biedt een overzicht van het configureren van de e-mailsjablonen voor deze gebeurtenissen worden gebruikt.

## <a name="publisher-notifications"> </a>Uitgever meldingen configureren

Meldingen configureren, klikt u op **beheren** in de klassieke Azure-Portal voor uw service Management API. Hiermee gaat u naar de portal Management API publisher.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de zelfstudie [aan de slag met Azure API beheer][] .

Klik op **meldingen** in het menu **API beheer** aan de linkerkant om de beschikbare meldingen weergeven.

![Meldingen van Publisher][api-management-publisher-notifications]

De volgende lijst met gebeurtenissen kan worden geconfigureerd voor meldingen.

-   **Abonnement aanvragen (vereisen van goedkeuring)** - de opgegeven geadresseerden en gebruikers e-meldingen ontvangt over het abonnement aanvragen voor API producten vereisen van goedkeuring.
-   **Nieuwe abonnementen** - de opgegeven geadresseerden en de gebruikers ontvangen e-mailberichten over nieuwe API product abonnementen.
-   **Aanvragen galerij** - de opgegeven geadresseerden en gebruikers e-mail meldingen ontvangt wanneer nieuwe toepassingen worden ingediend bij de galerie toepassing.
-   **BCC** - geadresseerden opgegeven e-mailadres en gebruikers ontvangt e-mail blind carbon copy van alle e-mails verzonden aan ontwikkelaars.
-   **Nieuwe kwestie of commentaar** - de opgegeven geadresseerden en gebruikers e-mail meldingen ontvangt wanneer een nieuw probleem of opmerking wordt ingediend op de developer-portal.
-   **Account sluiten bericht** - de opgegeven geadresseerden en gebruikers e-mail meldingen ontvangt wanneer een account wordt gesloten.
-   **Quotumlimiet Approaching-abonnement** - de volgende geadresseerden en gebruikers ontvangen e-mailberichten gebruik abonnement haalt bijna gebruiksgegevens.

Voor elke gebeurtenis, kunt u de geadresseerden met behulp van het tekstvak e-mailadres opgeven of u kunt gebruikers selecteren uit een lijst.

Als u de e-mailadressen op de hoogte worden gesteld, moet u deze in het tekstvak e-mailadres invoeren. Als u meerdere e-mailadressen, gescheiden met komma's.

![Ontvangers van meldingen][api-management-email-addresses]

Als u de gebruikers op de hoogte worden gesteld, klikt u op **geadresseerde toevoegen**, schakel het selectievakje in naast de gebruikers moeten worden geïnformeerd en klikt u op **OK**.

>Houd er rekening mee dat alleen beheerders in de lijst worden weergegeven.

Na het configureren van de geadresseerden van het bericht, klikt u op **Opslaan** om de ontvangers van bijgewerkte meldingen van toepassing.

>Als u het tabblad **Meldingen Publisher** verlaat de uitgever portal wordt u gewaarschuwd als er niet-opgeslagen wijzigingen.

## <a name="email-templates"> </a>E-mailsjablonen voor configureren

Beheer-API biedt e-mailsjablonen voor de e-mailberichten die worden verzonden in het kader van beheer en gebruik van de service. De volgende e-mailsjablonen worden geleverd.

-   Toepassing galerie inzending goedgekeurd
-   Ontwikkelaar afscheidstekst letter
-   Ontwikkelaar quotumlimiet nadert melding
-   Gebruiker uitnodigen
-   Nieuwe opmerking toegevoegd aan een probleem
-   Nieuwe kwestie ontvangen
-   Nieuw abonnement op geactiveerd
-   Bevestiging abonnement is verlengd
-   Abonnement op verzoek weigert
-   Aanvraag voor een abonnement ontvangen

Deze sjablonen kunnen worden aangepast naar wens.

Als u wilt weergeven en configureren van de e-mailsjablonen voor uw exemplaar van de beheer-API, **meldingen** op in het menu **API beheer** aan de linkerkant en klik op het tabblad **E-mailsjablonen** .

![E-mailsjablonen][api-management-email-templates]

Als u wilt bekijken of wijzigen van een specifieke sjabloon, selecteert u deze in de lijst **sjablonen** .

![E-mail templates lijst][api-management-email-templates-list]

Elke e-mailsjabloon heeft een onderwerp als tekst zonder opmaak en een definitie van de hoofdtekst in HTML-indeling. Elk item kan worden aangepast naar wens.

![E-mail sjablooneditor][api-management-email-template]

**De lijst** bevat een overzicht van de parameters die tijdens zijn ingevoegd in het onderwerp of de hoofdtekst, de aangewezen waarde vervangen wanneer het e-mailbericht wordt verzonden. Als een parameter wilt invoegen, plaatst u de cursor waar u wilt dat de parameter om te gaan en klik op de pijl links van de parameternaam van de.

Klik op **voorbeeld** of **verzenden van een test** om te zien hoe het e-mailbericht wordt gezocht of een test per e-mail.

>Houd er rekening mee dat de parameters niet worden vervangen door werkelijke waarden bij het weergeven of verzenden van een test.
>
>Sla de wijzigingen in de e-mailsjabloon, klikt u op **Opslaan**of Annuleren de wijzigingen klikt u op **Annuleren**.



[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Aan de slag met Azure API beheer]: api-management-get-started.md
[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance