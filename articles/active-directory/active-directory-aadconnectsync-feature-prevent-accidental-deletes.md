<properties
   pageTitle="Azure AD verbinden sync: niet per ongeluk verwijderen | Microsoft Azure"
   description="Dit onderwerp beschrijft de functie voorkomen dat per ongeluk worden verwijderd (voorkomen van onbedoelde verwijderingen) in Azure AD verbinding maken."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD verbinden sync: niet per ongeluk verwijderen
Dit onderwerp beschrijft de functie voorkomen dat per ongeluk worden verwijderd (voorkomen van onbedoelde verwijderingen) in Azure AD verbinding maken.

Bij het installeren van Azure AD verbinden, te voorkomen dat per ongeluk verwijderen is standaard ingeschakeld en geconfigureerd voor uitvoer met meer dan 500 verwijderen niet is toegestaan. Deze functie is ontworpen om u te beschermen van wijzigingen en wijzigingen in de configuratie per ongeluk naar de map op de ruimten die van invloed zou zijn op veel gebruikers en andere objecten.

Veelvoorkomende scenario's beschreven wanneer er veel verwijderd zijn:

- Wijzigingen in de [filters](active-directory-aadconnectsync-configure-filtering.md) waarbij een gehele [organisatie-eenheid](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) of [domein](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) niet geselecteerd is.
- Alle objecten in een organisatie-eenheid worden verwijderd.
- Een organisatie-eenheid is gewijzigd, zodat alle objecten in deze worden beschouwd als buiten bereik voor synchronisatie.

De standaardwaarde van 500 objecten met PowerShell kan worden gewijzigd met behulp van `Enable-ADSyncExportDeletionThreshold`. U moet deze waarde aan de grootte van uw organisatie. Aangezien de sync-scheduler wordt uitgevoerd voor elke 30 minuten, is de waarde het aantal verwijderingen gezien binnen 30 minuten.

Als er te veel worden verwijderd om te worden geëxporteerd naar Azure AD klaargezet, stopt het exporteren en u ontvangt een e-mail als volgt:

![Voorkomen dat per ongeluk verwijderde e-mail](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Hallo (technische contactpersoon). (Tijd) de identiteit tijdsynchronisatie-service gedetecteerd dat het aantal verwijderingen de verwijdering geconfigureerde drempel (organisatienaam) overschreden. Een totaal van (getal) objecten zijn verzonden voor verwijdering in deze identiteit synchronisatie uitvoeren. Dit bereikt of overschreden de drempelwaarde voor de geconfigureerde verwijdering van objecten (getal). We moeten bevestigen dat deze verwijderde items moeten worden verwerkt voordat het zal worden voortgezet. Zie het voorkomen van onbedoelde verwijderingen voor meer informatie over de fout in dit e-mailbericht.*

U kunt ook de status zien `stopped-deletion-threshold-exceeded` wanneer u zoeken in de **Service Synchronisatiebeheer** UI voor het profiel exporteren.
![Niet per ongeluk verwijderen UI Sync Service Manager](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Als dit onverwacht was, onderzoeken en corrigerende maatregelen treffen. Als u wilt weten welke objecten zijn verwijderd, het volgende doen:

1. **Synchronisatie** starten vanuit het startmenu.
2. Ga naar de **verbindingslijnen**.
3. Selecteer de verbindingslijn met het type **Azure Active Directory**.
4. Selecteer **Search Connector ruimte**onder **Acties** aan de rechterkant.
5. In het pop-upmenu onder het **bereik** **Verbroken omdat** selecteert en kies een tijdstip in het verleden. Klik op **Zoeken**. Deze pagina geeft een overzicht van alle objecten verwijderd. Op elk item klikt, krijgt u meer informatie wilt over het object. U kunt ook klikken op de **Kolom instellen** als u wilt toevoegen van extra kenmerken worden weergegeven in het raster.

![Search-Connector ruimte](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Als u deze verwijdert u gewenst zijn, vervolgens het volgende doen:

1. Tijdelijk uitschakelen van deze bescherming en kunt deze verwijderen via, de PowerShell-cmdlet uitvoert: `Disable-ADSyncExportDeletionThreshold`. Een Azure AD globale Administrator-account en wachtwoord opgeven.
![Referenties](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
2. Met de Azure Active Directory Connector nog steeds is geselecteerd, selecteert u de actie **uitvoert** en selecteer **exporteren**.
3. Als u opnieuw de bescherming, voert u de PowerShell-cmdlet: `Enable-ADSyncExportDeletionThreshold`.

## <a name="next-steps"></a>Volgende stappen

**Van overzichtsonderwerpen**

- [Azure AD verbinden sync: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
