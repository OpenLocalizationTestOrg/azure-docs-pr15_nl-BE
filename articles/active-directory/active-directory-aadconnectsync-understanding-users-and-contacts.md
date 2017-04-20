<properties
    pageTitle="Azure AD verbinden sync: wat zijn gebruikers en contactpersonen | Microsoft Azure"
    description="Gebruikers en contactpersonen in Azure AD verbinden sync uitgelegd."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Azure AD verbinden sync: wat zijn gebruikers en contactpersonen

Er zijn verschillende andere redenen waarom u meerdere Active Directory-forest hebben en er zijn diverse verschillende topologieën. Gebruikte modellen zijn een implementatie rekening resource en GAL sync'ed bossen na een fusie & overname. Maar zelfs als er puur modellen, hybride modellen zijn ook. De standaardconfiguratie in Azure verbinden met AD-synchronisatie komt niet wordt ervan uitgegaan dat een bepaald model, maar afhankelijk van hoe gebruiker die is geselecteerd in de installatiehandleiding, verschillende gedragingen kunnen worden waargenomen.

In dit onderwerp wordt doorloopt u de werking van de standaardconfiguratie in bepaalde topologieën. We gaan via de configuratie en de regeleditor synchronisatie kan worden gebruikt om de configuratie te bekijken.

Er zijn een paar algemene regels de configuratie wordt ervan uitgegaan dat:

- Ongeacht de volgorde waarin wij vanuit Active Directory's van de bron importeren, moet het eindresultaat altijd hetzelfde zijn.
- Een actieve account zal altijd bijdragen-in informatie, inclusief **userPrincipalName** en **sourceAnchor**.
- Een uitgeschakelde account bijdragen userPrincipalName en sourceAnchor, tenzij het een gekoppelde postbus als er geen actieve rekening te vinden.
- Een account met een gekoppelde postbus wordt nooit gebruikt voor userPrincipalName en sourceAnchor. Er wordt aangenomen dat een actieve account later worden gevonden.
- Contact-object kan worden ingericht naar Azure AD als een contactpersoon of als een gebruiker. U weet niet echt totdat alle bron Active Directory-forests zijn verwerkt.

## <a name="contacts"></a>Contactpersonen

Contactpersonen die een gebruiker in een ander forest vertegenwoordigt dat is gebruikelijk na een fusie & overname waar twee of meer Exchange-forests is overbruggen van een GALSync-oplossing. Contact-object wordt altijd vanuit de ruimte connector koppelen aan de metaverse met behulp van het e-mailkenmerk. Als er al een contactpersoon of gebruikersobject met hetzelfde e-mailadres, worden de objecten samengevoegd. Dit is geconfigureerd in de regel **In uit Active Directory – Contact deelnemen aan**. Er is ook een regel met de naam **In uit AD – algemeen Contact** kenmerk **Bronobjecttype** met constant **Contact**met de stroom van een kenmerk aan de metaverse. Deze regel heeft een zeer lage prioriteit dus als een user-object is gekoppeld aan hetzelfde object metaverse, wordt de regel **In uit Active Directory-gebruiker gemeenschappelijk** gebruiker waarde aan dit kenmerk zal bijdragen. Met deze regel hebben dit kenmerk Contact als er geen gebruiker is lid geworden van de waarde en de waarde van gebruiker als ten minste één gebruiker is gevonden.

Voor het inrichten van een object naar Azure AD maakt de uitgaande regel **Out aan AAD: contact opnemen met lid** contact-object als de metaverse kenmerk **Bronobjecttype** **contact**is ingesteld. Als dit kenmerk is ingesteld op **gebruiker**, de regel **Out naar AAD – gebruiker toevoegen** maakt een gebruikersobject in plaats daarvan.
Het is mogelijk dat een object wordt verschoven van contactpersoon aan de gebruiker wanneer meer bron Active Directory's worden geïmporteerd en gesynchroniseerd.

Bijvoorbeeld in een topologie GALSync vindt we contactpersoonobjecten voor iedereen in het tweede forest als importeert u het eerste forest. Dit zal nieuwe contactpersoon objecten in de Connector AAD fase. Wanneer we later importeren en het tweede forest synchroniseren, we zoeken echte gebruikers en voeg ze toe aan de bestaande objecten in de metaverse. We vervolgens het contact AAD-object te verwijderen en in plaats daarvan een nieuw object maken.

Als u een topologie hebt waarbij gebruikers en contactpersonen wordt weergegeven, moet u selecteren aan gebruikers op het e-mailkenmerk in de installatiehandleiding. Als u een andere optie selecteert, moet u afhankelijke configuratie van een order. Contact-objecten worden altijd toegevoegd aan op het e-mailkenmerk maar gebruikersobjecten wordt alleen deelnemen aan op het e-mailkenmerk als deze optie is geselecteerd in de installatiehandleiding. U kan vervolgens uiteindelijk twee verschillende objecten in de metaverse met hetzelfde mailkenmerk als de contact-object voor het gebruikersobject is geïmporteerd. Tijdens het exporteren naar Azure AD zal een fout wordt gegenereerd. Dit gedrag is inherent aan het ontwerp en beschadigde gegevens of de topologie is niet correct geïdentificeerd tijdens de installatie zou geven.

## <a name="disabled-accounts"></a>Uitgeschakelde accounts

Uitgeschakelde accounts worden ook gesynchroniseerd met Azure AD. Uitgeschakelde accounts zijn gemeenschappelijk voor bronnen in Exchange, zoals vergaderruimten. Geldt niet voor gebruikers met een gekoppelde postbus; zoals eerder vermeld, zal deze nooit een Azure AD-account inrichten.

De veronderstelling is dat als een uitgeschakelde gebruikersaccount wordt gevonden, wordt er een andere actieve account later wordt gevonden en het object is ingericht naar Azure AD met de userPrincipalName en sourceAnchor gevonden. In het geval een andere actieve account lid van hetzelfde object metaverse worden wordt, wordt de userPrincipalName en sourceAnchor gebruikt.

## <a name="changing-sourceanchor"></a>SourceAnchor wijzigen

Wanneer een object is geëxporteerd naar Azure AD en het is niet toegestaan om de sourceAnchor niet meer wijzigen. Bij van het object exporteren wordt met de waarde van de **sourceAnchor** geaccepteerd door Azure AD de metaverse kenmerk **cloudSourceAnchor** ingesteld. Als **sourceAnchor** is gewijzigd en niet overeen met de **cloudSourceAnchor**, de regel **Out naar AAD – gebruiker toevoegen** in de weergave van de fout **sourceAnchor-kenmerk is gewijzigd**. In dit geval moeten de configuratie of de gegevens worden gecorrigeerd zodat het dezelfde sourceAnchor aanwezig in de metaverse opnieuw is voordat u het object opnieuw kan worden gesynchroniseerd.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Azure AD verbinding Sync: Synchronisatie-opties aanpassen](active-directory-aadconnectsync-whatis.md)
* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
