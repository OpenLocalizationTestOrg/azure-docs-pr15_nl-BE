<properties
    pageTitle="Enable onderneming staat Roaming in Azure Active Directory | Microsoft Azure"
    description="Veelgestelde vragen over onderneming staat Roaming-instellingen in Windows-apparaten. Enterprise staat zwervende gebruikers beschikken over een consistente ervaring via hun Windows-apparaten en vermindert de tijd die nodig is voor het configureren van een nieuw apparaat."
    services="active-directory"
    keywords="Enterprise staat roaming, cloud, windows, het inschakelen van de onderneming staat roaming"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>



# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Enable onderneming staat Roaming in Azure Active Directory

Enterprise staat Roaming is beschikbaar voor elke organisatie met een abonnement Premium Azure Active Directory (AD Azure). Zie voor meer informatie over het verkrijgen van een abonnement Azure AD de [Azure AD productpagina](https://azure.microsoft.com/services/active-directory).

Wanneer u Enterprise staat Roaming inschakelt, worden uw organisatie automatisch toegewezen licenties voor een abonnement gratis, gelimiteerde Azure Rights Management. Dit gratis abonnement is beperkt tot het coderen en decoderen van de instellingen van de onderneming en de toepassingsgegevens gesynchroniseerd door de service Enterprise staat Roaming; hebt u een betaalde licentie voor het gebruik van de volledige mogelijkheden van Azure Rights Management.

Na het verkrijgen van een Azure AD Premium-abonnement, volgt onderneming staat zwervende inschakelen:

1. Aanmelden bij de klassieke Azure portal.
2. Selecteer **ACTIVE DIRECTORY**aan de linkerkant en selecteer de map die u wilt inschakelen onderneming staat Roaming.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Ga naar het tabblad **configureren** op de bovenkant.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4.  Schuif omlaag op de pagina en selecteer **gebruikers kunnen instellingen en ENTERPRISE APP gegevens SYNCHRONISEREN**en klik vervolgens op **Opslaan**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Het apparaat moet voor een Windows 10-apparaat in staat met de service Enterprise staat zwervende instellingen verifiëren met behulp van een identiteit Azure AD. Primaire aanmelding van de gebruiker is de identiteit Azure AD voor apparaten die lid zijn van AD Azure, dus er is geen extra configuratie vereist. Voor apparaten die een traditionele op ruimten Active Directory gebruikt, moet IT-beheer [Sluit de apparaten een domein behoren naar Azure AD voor ervaringen met Windows 10](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Sync-gegevensopslag
Enterprise staat Roaming gegevens wordt gehost in een of meer [Azure regio's](https://azure.microsoft.com/regions/ ) die het best overeenstemt met het land waarde instelt in de instantie Azure Active Directory. Gegevens onderneming staat Roaming is gepartitioneerd op basis van drie belangrijke geografische regio's: Noord-Amerika, EMEA en APAC. Gegevens voor de huurder onderneming staat Roaming is lokaal opgeslagen met de geografische regio en wordt niet gerepliceerd tussen de regio's.  Bijvoorbeeld, klanten die hun land waarde worden ingesteld op een van de EMEA-landen, zoals "Frankrijk" of 'Zambia' hebben hun gegevens in een of van de Azure gebieden binnen Europa.  Klanten die hun land waarde ingesteld in Azure AD op een van de landen van Noord-Amerika zoals 'Verenigde Staten' of 'Canada' hun gegevens in een of meer van de Azure gebieden binnen de Verenigde Staten gehost hebben.  Klanten die hun land waarde in Azure AD op een van de landen van APAC instellen zoals "Australië" of "Nieuw-Zeeland" hun gegevens in een of meer van de Azure regio's in Azië hebben.  Zuid-Amerikaanse landen en Antarctica gegevens zal worden gehost in een of meer Azure gebieden binnen de Verenigde Staten.  De waarde voor land/regio is ingesteld als onderdeel van het ontwikkelingsproces van Azure AD directory en vervolgens niet worden gewijzigd. 

Als u meer informatie over de locatie voor opslag van gegevens, moet het bestand een ticket met [ondersteuning van Azure](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Roaming staat van de onderneming beheren
Azure AD globale beheerders kunnen inschakelen en uitschakelen van de onderneming staat Roaming in de klassieke Azure portal.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globale beheerders kunnen instellingen synchroniseren naar specifieke beveiligingsgroepen te beperken.

Globale beheerders kunnen een statusrapport per gebruiker apparaat synchroniseren ook weergeven door een bepaalde gebruiker selecteren in de lijst met Active Directory **gebruikers** en te klikken op het tabblad **apparaten** en **apparaten synchroniseren instellingen en gegevens van enterprise app**weergave te selecteren.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

##<a name="data-retention"></a>Gegevens bewaren
Gegevens gesynchroniseerd met Azure via Enterprise staat Roaming bewaard voor onbepaalde tijd tenzij een handmatige bewerking wordt uitgevoerd of wordt vastgesteld dat de betrokken gegevens verlopen. 

**Expliciete verwijderen:** De gegevens worden verwijderd wanneer een Azure admin Hiermee verwijdert u een gebruiker of een map of een beheerder om hulp uitdrukkelijk vraagt dat er gegevens moeten worden verwijderd.

- **Gebruiker verwijderen**: wanneer een gebruiker wordt verwijderd uit AD Azure, de zwervende gebruikersaccount worden gemarkeerd voor verwijdering en tussen 90-180 dagen worden verwijderd. 
- **Map verwijderen**: verwijderen van een hele map in Azure AD is een directe bewerking. Alle instellingsgegevens die zijn gekoppeld met die map worden gemarkeerd voor verwijdering en tussen 90-180 dagen worden verwijderd. 
- **Op verzoek verwijderen**: als de admin Azure AD wil handmatig verwijderen van een specifieke gebruiker gegevens of instellingen, de admin een ticket met [ondersteuning van Azure](https://azure.microsoft.com/support/)kunt indienen. 

**Verwijdering van verouderde gegevens**: gegevens die niet zijn geopend voor één jaar ("de bewaartermijn"), worden behandeld als een verlopen en kan worden verwijderd uit de Azure. De bewaartermijn wordt gewijzigd, maar worden niet minder dan 90 dagen. Verouderde gegevens is een reeks instellingen voor Windows-toepassing of alle instellingen voor een gebruiker mogelijk. Bijvoorbeeld:
 
- Als er geen apparaten toegang krijgen een collectie van bepaalde instellingen tot (bijvoorbeeld een toepassing van het apparaat wordt verwijderd, of een groep instellingen, zoals 'Thema' is uitgeschakeld voor alle apparaten van een gebruiker) en vervolgens die collectie na de bewaarperiode verouderd raken en kan worden verwijderd. 
- Een gebruiker heeft de instellingen voor synchronisatie op alle apparaten zijn uitgeschakeld, vervolgens de instellingen voor gegevens worden benaderd als de voor instellingsgegevens voor die gebruiker zullen verlopen en kan worden verwijderd na de bewaarperiode. 
- Als de Azure AD directory admin onderneming staat Roaming voor de gehele map, worden alle gebruikers uitschakelt in deze map wordt niet meer synchroniseren instellingen en instellingsgegevens van alle voor alle gebruikers zullen verlopen en kan worden verwijderd na de bewaarperiode. 

**Verwijderde gegevens te herstellen**: het bewaarbeleid gegevens kan niet worden geconfigureerd. Zodra de gegevens permanent is verwijderd, is deze niet worden hersteld. Het is echter belangrijk te weten dat de instellingsgegevens worden alleen verwijderd uit Azure, niet het apparaat van de eindgebruiker. Als een apparaat later wordt u opnieuw met de service Enterprise staat Roaming verbonden, worden de instellingen opnieuw gesynchroniseerd en opgeslagen in Azure.


## <a name="related-topics"></a>Verwante onderwerpen
- [Enterprise staat Roaming-overzicht](active-directory-windows-enterprise-state-roaming-overview.md)
- [Instellingen en gegevens zwervende Veelgestelde vragen](active-directory-windows-enterprise-state-roaming-faqs.md)
- [MDM beleid en de instellingen voor synchronisatie](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Windows 10 zwervende instellingen-naslag](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
