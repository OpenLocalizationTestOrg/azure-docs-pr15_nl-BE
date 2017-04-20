<properties
    pageTitle="Azure AD verbinden: Introductie met express instellingen | Microsoft Azure"
    description="Informatie over het downloaden, installeren en uitvoeren van de wizard setup voor Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Aan de slag met Azure AD verbinden via een express-instellingen
Azure AD verbinden **Express instellingen** wordt gebruikt wanneer u een topologie met één forest en [synchronisatie van wachtwoorden](../active-directory-aadconnectsync-implement-password-synchronization.md) voor verificatie hebt. **Express-instellingen** is de standaardoptie en wordt gebruikt voor de meest gebruikte scenario. U bent slechts een paar korte muisklikken weg om uit te breiden van de map op het bedrijf naar de cloud.

Voordat u begint met het installeren van Azure AD verbinden, zorg ervoor dat [Azure AD verbinden](http://go.microsoft.com/fwlink/?LinkId=615771) downloaden en volledig de voorafgaande vereiste stappen in [Azure AD verbinden: Hardware en vereisten](../active-directory-aadconnect-prerequisites.md).

Als express-instellingen komt niet overeen met de topologie, ziet u de [bijbehorende documentatie](#related-documentation) voor andere scenario's.

## <a name="express-installation-of-azure-ad-connect"></a>Snelle installatie van Azure AD verbinden
U kunt deze stappen in actie in de sectie [video's](#videos) bekijken.

1. Meld je aan als lokale beheerder op de server die u verbinden met Azure AD wilt op installeren. Doe dit op de server die u wilt worden van de server synchroniseren.
2. Navigeer naar en dubbelklik op **AzureADConnect.msi**.
3. Schakel het akkoord met de licentievoorwaarden en klik op **Doorgaan**in het welkomstscherm.  
4. Klik op het scherm Express instellingen **express instellingen gebruiken**.  
![Welkom bij AD Azure verbinding](./media/active-directory-aadconnect-get-started-express/express.png)
5. Voer de gebruikersnaam en het wachtwoord van een beheerder van een globale voor uw advertentie Azure op de verbinding naar Azure AD scherm. Klik op **volgende**.  
![Verbinding maken met Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) als u een foutbericht en problemen met verbindingen, klik Zie [problemen met verbindingen oplossen](../active-directory-aadconnect-troubleshoot-connectivity.md).
6. Voer op de verbinding met het AD DS-scherm, de gebruikersnaam en het wachtwoord voor een enterprise-beheerdersaccount. U kunt het domeingedeelte in NetBios of FQDN-indeling, dat wil zeggen, FABRIKAM\administrator of fabrikam.com\administrator. Klik op **volgende**.  
![Verbinding maken met AD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. De pagina [**Azure AD - in de configuratie**](../active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) wordt alleen weergegeven als u niet [controleren of uw domeinen](../active-directory-add-domain.md) in de [voorwaarden](../active-directory-aadconnect-prerequisites.md)is voltooid.
![Niet-geverifieerde domeinen](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
Als u deze pagina ziet, controleert u elk domein is gemarkeerd als **Niet toegevoegd** en wordt **Niet gecontroleerd**. Zorg ervoor dat deze domeinen die u gebruikt in Azure AD zijn geverifieerd. Wanneer u uw domeinen hebt gecontroleerd, klikt u op het symbool voor vernieuwing.
8. Klik op de toepassing scherm configureren, klikt u op **installeren**.
    - U kunt desgewenst het selectievakje **Start het synchronisatieproces zodra configuratie is voltooid** deselecteren op de gereed voor het configureren van de pagina. U moet dit selectievakje selecties opheffen als u aanvullende instellingen, zoals het [filteren](../active-directory-aadconnectsync-configure-filtering.md). Als u de selectie van deze optie ongedaan maken, de wizard configureert synchronisatie, maar blijft de scheduler is uitgeschakeld. Totdat u deze handmatig door [de installatiewizard opnieuw inschakelen](../active-directory-aadconnectsync-installation-wizard.md)niet worden uitgevoerd.
    - Als u Exchange in uw Active Directory op lokalen, hebt u ook een optie voor [**Exchange hybride implementatie**](https://technet.microsoft.com/library/jj200581.aspx)mogelijk te maken. Schakel deze optie in als u van plan bent om Exchange-postbussen, zowel in de cloud en op gebouwen op hetzelfde moment.
![Azure AD-verbinding configureren](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Wanneer de installatie is voltooid, klikt u op **Afsluiten**.
10. Nadat de installatie is voltooid, afmelden en opnieuw aanmelden voordat u servicebeheer synchronisatie of synchronisatie regeleditor gebruiken.

## <a name="videos"></a>Video 's

Zie voor een video over het gebruik van de aangepaste installatie:

>[AZURE.VIDEO azure-active-directory-connect-express-settings]

## <a name="next-steps"></a>Volgende stappen
Nu u hebt die Azure AD Connect geïnstalleerd kunt u [controleren of de installatie en het toewijzen van licenties](../active-directory-aadconnect-whats-next.md).

Meer informatie over deze functies zijn ingeschakeld bij de installatie: [Automatische upgrade](../active-directory-aadconnect-feature-automatic-upgrade.md), [niet per ongeluk verwijderen](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)en [Azure AD verbinding](../active-directory-aadconnect-health-sync.md).

Meer informatie over deze onderwerpen gemeenschappelijke: [Taakplanner en het synchroniseren starten](../active-directory-aadconnectsync-feature-scheduler.md).

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Gerelateerde documentatie

Onderwerp |  
--------- | ---------
Azure AD-verbinding-overzicht | [Integratie van uw identiteiten op ruimten met Azure Active Directory](../active-directory-aadconnect.md)
Installeren met behulp van aangepaste instellingen | [Aangepaste installatie van Azure AD verbinden](active-directory-aadconnect-get-started-custom.md)
DirSync upgraden | [Een upgrade van Azure AD synchronisatietool (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Accounts die worden gebruikt voor de installatie | [Meer informatie over het verbinden van Azure AD accounts en machtigingen](active-directory-aadconnect-accounts-permissions.md)
