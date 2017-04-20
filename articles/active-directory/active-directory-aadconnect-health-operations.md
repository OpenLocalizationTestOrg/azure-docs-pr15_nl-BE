<properties
    pageTitle="Azure AD verbinding gezondheid bewerkingen."
    description="Dit artikel beschrijft extra bewerkingen die kunnen worden uitgevoerd nadat u gezondheid Azure AD verbinding hebt geïmplementeerd."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-operations"></a>Azure AD verbinding gezondheid bewerkingen

Dit onderwerp beschrijft de verschillende bewerkingen die kunnen worden uitgevoerd met Azure AD verbinding maken met de gezondheid.

## <a name="enable-email-notifications"></a>E-mailberichten inschakelen
U kunt de Azure AD verbinding gezondheid Service configureren voor het verzenden van e-mailberichten wanneer waarschuwingen worden gegenereerd die aangeeft van dat de infrastructuur van uw identiteit niet in orde is. Dit gebeurt wanneer een waarschuwing wordt gegenereerd, alsmede dat deze wordt gemarkeerd als opgelost. Volg de onderstaande instructies voor het configureren van e-mailberichten.

![Azure AD verbinding gezondheid E-mail melding ontdekken](./media/active-directory-aadconnect-health/email_noti_discover.png)

>[AZURE.NOTE] E-mailmeldingen zijn standaard uitgeschakeld.


### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Azure AD verbinding maken met de gezondheid van e-mailberichten inschakelen

1. De Blade waarschuwingen voor de service waarvoor u ontvangen e-mailbericht wilt openen.
2. Klik op de knop 'Instellingen voor meldingen' uit de actiebalk.
3. Schakel op de e-mailwaarschuwing-switch.
4. Schakel het selectievakje globale beheerders voor het ontvangen van e-mailmeldingen configureren.
5. Als u ontvangen van e-mailberichten op de e-mailadres wilt, kunt u deze in het vak Extra e-mailgeadresseerde. Als een e-mailadres uit deze lijst verwijderen, klik met de rechtermuisknop op het item en selecteer verwijderen.
6. Afwerken van de wijzigingen-Klik op 'Opslaan'. Alle wijzigingen worden effecten pas nadat u op 'Opslaan'.

## <a name="delete-a-server-or-service-instance"></a>Een server of service-exemplaar verwijderen

### <a name="delete-a-server-from-azure-ad-connect-health-service"></a>Een server verwijderen uit de Azure AD verbinding Health Service
In sommige gevallen kunt u een server verwijderen uit de gecontroleerde. Volg de onderstaande instructies om een server verwijderen uit de Azure AD verbinding Health Service.

Wanneer u een server verwijdert, worden op de hoogte van de volgende opties:

- Deze actie zal stoppen met het verzamelen van verdere gegevens van die server. Deze server wordt verwijderd uit de monitoring service. Na deze actie, u niet mogelijk om nieuwe waarschuwingen weer te controleren of het gebruik van analytics-gegevens voor deze server.
- Deze actie wordt niet verwijderen of de Agent voor de gezondheid van uw server te verwijderen. Als u niet de Health Agent hebt verwijderd voordat u deze stap uitvoert, ziet u fouten op de server die betrekking hebben op de gezondheid Agent.
- Deze actie wordt niet verwijderd van de gegevens van deze server al verzameld. Aan de hand van het beleid van Microsoft Azure gegevens bewaren worden die gegevens verwijderd.
- Na het uitvoeren van deze actie als u een controle van dezelfde server opnieuw starten wilt moet u verwijderen en opnieuw installeren van de agent van de gezondheid op deze server.


#### <a name="to-delete-a-server-from-azure-ad-connect-health-service"></a>Een server verwijderen uit de Azure AD verbinding Health Service

Azure AD gezondheid verbinding voor AD FS & Azure AD (synchronisatie):

1. De Blade-Server van de Blade-Server lijst openen door de naam van de server moet worden verwijderd.
2. Klik op de knop 'Verwijderen' van de actiebalk in de Blade-Server.
3. Bevestig de actie aan de server verwijderen door de naam van de server te typen in het vak bevestigen.
4. Klik op de knop 'Verwijderen'.

Azure AD gezondheid verbinding voor AD DS:

1. Open het dashboard voor domeincontrollers.
2. Selecteer de domeincontroller te verwijderen.
3. Klik op de knop 'Verwijder de geselecteerde' uit de actiebalk.
4. Bevestig de actie aan de server te verwijderen.
5. Klik op de knop 'Verwijderen'.

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Een service-exemplaar van Azure AD verbinding Health Service verwijderen

In sommige gevallen wilt u mogelijk een service-exemplaar verwijderen. Volg de instructies hieronder een service-exemplaar van Azure AD verbinding Health Service verwijderen.

Wanneer u een service-exemplaar verwijdert, worden op de hoogte van de volgende opties:

- Deze actie wordt het huidige exemplaar van de service verwijderen uit de monitoring service.
- Deze actie wordt niet verwijderen of de Health Agent verwijderen uit een van de servers die als onderdeel van deze service-instantie werden gecontroleerd. Als u niet de Health Agent hebt verwijderd voordat u deze stap uitvoert, ziet u mogelijk fouten op de server (s) die verband houden met het Health Agent.
- Alle gegevens van deze service-instantie worden aan de hand van het bewaarbeleid voor Microsoft Azure gegevens verwijderd.
- Na het uitvoeren van deze actie, als u beginnen met de service te controleren wilt, verwijder de installatie en opnieuw installeren van de agent van de gezondheid op alle servers die zullen worden gecontroleerd. Na het uitvoeren van deze actie als u een controle van dezelfde server opnieuw starten wilt moet u verwijderen en opnieuw installeren van de agent van de gezondheid op deze server.


#### <a name="to-delete-a-service-instance-from-azure-ad-connect-health-service"></a>Een service-exemplaar van Azure AD verbinding Health Service verwijderen

1. De Blade-Service uit de lijst Service Blade openen door het selecteren van de service-id (naam bedrijf) die u wilt verwijderen.
2. Klik op de knop 'Verwijderen' van de actiebalk in de Blade-Server.
3. Bevestig de servicenaam van de door deze te typen in het vak bevestigen. (bijvoorbeeld: sts.contoso.com)
4. Klik op de knop 'Verwijderen'.
<br><br>


[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Toegangsbeheer met rol op basis van toegangsbeheer
### <a name="overview"></a>Overzicht
[Rol gebaseerde toegangscontrole](role-based-access-control-configure.md) voor de gezondheid verbinding Azure AD biedt toegang Azure AD verbinding gezondheidszorg voor gebruikers als groepen buiten globale beheerders. Dit wordt bereikt door rollen toewijzen aan gebruikers en/of groepen en een mechanisme voor het beperken van de globale beheerders in uw directory.

#### <a name="roles"></a>Rollen
Azure AD verbinding maken met de gezondheid van ondersteunt de volgende ingebouwde functies.

| Rol | Machtigingen |
| ----------- | ---------- |
| Eigenaar | Eigenaren can ***access beheren*** (bijvoorbeeld toewijzen rol aan een gebruiker of groep), ***alle informatie*** (bv. waarschuwingen bekijken) van de portal en de ***instellingen wijzigen*** (bijvoorbeeld e-mailberichten) in Azure AD verbinding maken met de gezondheid van. <br>Standaard Azure AD globale beheerders deze rol zijn toegewezen en dit kan niet worden gewijzigd.  |
|Inzender|  Medewerkers kunnen ***bekijken van alle informatie*** (bv. waarschuwingen bekijken) van de portal en de ***instellingen wijzigen*** (bijvoorbeeld e-mailberichten) in Azure AD verbinding maken met de gezondheid.|
|Reader| Lezers kunnen ***bekijken van alle informatie*** (bv. waarschuwingen bekijken) vanaf de portal in Azure AD verbinding maken met de gezondheid.|

Alle andere functies (zoals 'Beheerders toegang' of 'DevTest Labs Users'), zelfs als deze beschikbaar zijn in de portal ervaring hebben geen invloed op toegang in Azure AD verbinding maken met de gezondheid.

#### <a name="access-scope"></a>Toegang tot bereik

Azure AD Connect ondersteunt toegangsbeheer op twee niveaus:

- ***Alle exemplaren van de service***: dit is de aanbevolen procedure voor de meeste klanten en beheert de toegang voor alle exemplaren van de service (bijvoorbeeld een bedrijf AD FS) in alle typen van rol die door gezondheid verbinding Azure AD zijn worden gecontroleerd.

- ***Service-exemplaar***: In sommige gevallen moet u mogelijk toegang op basis van de functie typen of door een service-exemplaar te scheiden. In dit geval kunt u de toegang op het niveau van service-exemplaar beheren.  

Machtiging wordt verleend als de gebruiker heeft toegang tot op het niveau van de map of het Service-exemplaar.


### <a name="how-to-allow-users-or-groups-access-to-azure-ad-connect-health"></a>Het toestaan van gebruikers of groepen toegang tot gezondheid verbinding Azure AD
#### <a name="steps-1-select-the-appropriate-access-scope"></a>Stap 1: Selecteer het juiste bereik
Als u wilt toestaan dat een gebruikerstoegang op het niveau van *alle exemplaren van de service* in Azure AD verbinding maken met de gezondheid, de belangrijkste blade in Azure AD verbinding maken met de gezondheid te openen.<br>
#### <a name="step-2-add-users-groups-and-assign-roles"></a>Stap 2: Toevoegen van gebruikers, groepen en rollen toewijzen
1. Klik in het gedeelte 'Gebruikers' in de sectie configureren.<br>
![Azure AD verbinding gezondheid RBAC Main Blade](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Selecteer "Add"
3. Selecteer de "rol" zoals "Eigenaar"<br>
![Azure AD verbinding gezondheid RBAC gebruiker toevoegen](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Typ de naam of de id van de desbetreffende gebruiker of groep. U kunt een of meer gebruikers of groepen selecteren op hetzelfde moment. Klik op 'selecteren'.
![Azure AD gezondheid RBAC Selecteer gebruiker verbinding](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Selecteer "Ok".<br>

6. Als de toewijzing voltooid is, worden de gebruikers en/of groepen weergegeven in de lijst.<br>
![Azure AD verbinding gezondheid RBAC-gebruikerslijst](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Deze stappen kunt de gebruikers en de groepstoegang aan de hand van hun rollen.
>[AZURE.NOTE]
- Globale beheerders altijd volledige toegang hebben tot alle bewerkingen, maar globale administrator-accounts worden niet in de bovenstaande lijst.
- "Gebruikers uitnodigen"-functie wordt niet ondersteund in Azure AD verbinding maken met de gezondheid.

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Stap 3: De blade-locatie delen met gebruikers of groepen
1. Na het toewijzen van machtigingen van een gebruiker toegang tot gezondheid verbinding Azure AD door naar [http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth)te gaan.
2. Eenmaal op het blad, de gebruiker kunt vastmaken de blade of verschillende onderdelen aan het dashboard door te klikken op "Aan het dashboard vastmaken"<br>
![Azure AD verbinding gezondheid RBAC pin blade](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)


>[AZURE.NOTE] Een gebruiker met de rol 'Lezer' is niet mogelijk om de bewerking 'maken' als u de extensie Azure AD verbinding maken met de gezondheid van Azure Marketplace. Deze gebruiker nog u het blad door te gaan naar de bovenstaande koppeling. Voor later gebruik, kan de gebruiker het blad aan het dashboard vastmaken.

### <a name="remove-users-andor-groups"></a>Gebruikers en/of groepen verwijderen
U kunt een gebruiker of een groep toegevoegd aan deel Azure AD verbinding gezondheid rollen gebaseerd toegangsbeheer met de rechtermuisknop te klikken en verwijderen te selecteren.<br>
![Azure AD gezondheid RBAC verwijderen gebruiker verbinding](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="related-links"></a>Verwante koppelingen

* [Azure AD verbinding gezondheid](active-directory-aadconnect-health.md)
* [Azure AD Health Agent-installatie aansluiten](active-directory-aadconnect-health-agent-install.md)
* [Met behulp van Azure AD gezondheid verbinding met AD FS](active-directory-aadconnect-health-adfs.md)
* [Met behulp van gezondheid Azure AD verbinden voor synchronisatie](active-directory-aadconnect-health-sync.md)
* [Met behulp van Azure AD gezondheid verbinding met AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD gezondheid Veelgestelde vragen over verbinding](active-directory-aadconnect-health-faq.md)
* [Azure AD verbinding gezondheid versiegeschiedenis](active-directory-aadconnect-health-version-history.md)
