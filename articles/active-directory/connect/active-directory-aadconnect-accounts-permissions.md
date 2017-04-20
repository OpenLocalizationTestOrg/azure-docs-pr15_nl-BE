<properties
   pageTitle="Azure AD verbinden: De Accounts en machtigingen | Microsoft Azure"
   description="Dit onderwerp beschrijft de rekeningen gebruikt en wordt gemaakt en de vereiste machtigingen."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/04/2016"
   ms.author="billmath"/>


# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD verbinden: De Accounts en machtigingen
De installatiewizard Azure AD Connect biedt twee verschillende paden:

- De wizard vereist meer bevoegdheden in de Express instellingen zodat deze de configuratie eenvoudig instellen kunt zonder dat u gebruikers maken en machtigingen configureren apart.

- De wizard biedt meer mogelijkheden en opties in aangepaste instellingen, maar er zijn situaties waarin u moet ervoor zorgen dat u over de juiste machtigingen beschikt.

## <a name="related-documentation"></a>Gerelateerde documentatie
Als u de documentatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](../active-directory-aadconnect.md)niet heeft gelezen, vindt de volgende tabel u koppelingen naar verwante onderwerpen.

Onderwerp |  
--------- | ---------
Met behulp van instellingen voor Express installeren | [Snelle installatie van Azure AD verbinden](active-directory-aadconnect-get-started-express.md)
Installeren met behulp van aangepaste instellingen | [Aangepaste installatie van Azure AD verbinden](active-directory-aadconnect-get-started-custom.md)
DirSync upgraden | [Een upgrade van Azure AD synchronisatietool (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)


## <a name="express-settings-installation"></a>Snelle installatie van de instellingen
Express-instellingen de installatiewizard wordt gevraagd om referenties voor ondernemingsadministrator voor AD DS zodat uw Active Directory op gebouwen kan worden geconfigureerd met de vereiste machtigingen voor het verbinden van Azure AD. Als u een upgrade van DirSync uitvoert, worden de referenties van de AD DS-ondernemingsadministrators opnieuw instellen van het wachtwoord voor de account die wordt gebruikt door DirSync gebruikt. U moet ook Azure AD globale Administrator-referenties.

Pagina van de wizard  | Referenties verzameld | Vereiste machtigingen| Gebruikt voor
------------- | ------------- |------------- |-------------
N.V.T.|Met de installatiewizard gebruiker| De beheerder van de lokale server| <li>De lokale account die wordt gebruikt als de [serviceaccount voor synchronisatie-engine](#azure-ad-connect-sync-service-account)maakt.
Verbinding maken met Azure AD| Azure AD directory-referenties | Globale beheerdersrol in Azure AD | <li>Synchronisatie in AD Azure Active directory inschakelen.</li>  <li>Het maken van de [Azure AD-account](#azure-ad-service-account) die wordt gebruikt voor synchronisatie lopende bewerkingen in Azure AD.</li>
Verbinding maken met AD DS | Active Directory-referenties gebouwen | Lid van de groep Enterprise Admins (EA) in Active Directory| <li>Hiermee maakt u een [account](#active-directory-account) in Active Directory en machtigingen verleent. Deze account gemaakt wordt gebruikt om te lezen en schrijven van directory-informatie tijdens de synchronisatie.</li>

### <a name="enterprise-admin-credentials"></a>Referenties voor ondernemingsadministrator
Deze referenties worden alleen tijdens de installatie gebruikt en worden gebruikt nadat de installatie is voltooid. Is het ondernemings- en niet-domein Admin om ervoor te zorgen dat de machtigingen in Active Directory kunnen worden ingesteld in alle domeinen.

### <a name="global-admin-credentials"></a>Globale Administrator-referenties
Deze referenties worden alleen gebruikt tijdens de installatie en niet worden gebruikt nadat de installatie is voltooid. Wordt gebruikt voor het maken van de [Azure AD-account](#azure-ad-service-account) die wordt gebruikt voor het synchroniseren van wijzigingen naar Azure AD. De account kunt ook synchroniseren als een functie in Azure AD.

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>Machtigingen voor de gemaakte AD DS rekening voor express-instellingen
De [account](#active-directory-account) gemaakt voor lezen en schrijven naar AD DS hebt de volgende machtigingen wanneer gemaakt met express-instellingen:

Machtiging | Gebruikt voor
---- | ----
<li>Directory-wijzigingen repliceren</li><li>Alle Directory repliceren gewijzigd | Wachtwoord synchronisatie
Lezen/schrijven alle eigenschappen van gebruiker | Import- en hybride
Lezen/schrijven alle eigenschappen iNetOrgPerson | Import- en hybride
Lezen/schrijven, die alle eigenschappen groeperen | Import- en hybride
Neem contact op met lezen/schrijven alle eigenschappen | Import- en hybride
Wachtwoord opnieuw instellen | Voorbereiding voor het terugschrijven wachtwoord inschakelen

## <a name="custom-settings-installation"></a>Instellingen voor aangepaste installatie
Als u aangepaste instellingen gebruikt, moet de account waarmee verbinding met Active Directory worden gemaakt vóór de installatie. De machtigingen die u moet deze account toewijzen kunnen u vinden in [de AD DS-account maken](#create-the-ad-ds-account).

Pagina van de wizard  | Referenties verzameld | Vereiste machtigingen| Gebruikt voor
------------- | ------------- |------------- |-------------
N.V.T. | Met de installatiewizard gebruiker|<li>De beheerder van de lokale server</li><li>Als u een volledige SQL Server gebruikt, moet de gebruiker zijn systeem Administrator (SA) in SQL</li>| Standaard maakt de lokale account die wordt gebruikt als de [serviceaccount voor synchronisatie-engine](#azure-ad-connect-sync-service-account). De account wordt alleen gemaakt als admin niet een bepaalde account geeft.
Synchronisatieservices installeren, Service accountoptie | AD of referenties van de lokale gebruikersaccount | Gebruiker machtigingen worden verleend door de installatiewizard | Als de beheerder een account opgeeft, wordt deze rekening gebruikt als de serviceaccount voor de synchronisatieservice.
Verbinding maken met Azure AD | Azure AD directory-referenties| Globale beheerdersrol in Azure AD| <li>Synchronisatie in AD Azure Active directory inschakelen.</li>  <li>Het maken van de [Azure AD-account](#azure-ad-service-account) die wordt gebruikt voor synchronisatie lopende bewerkingen in Azure AD.</li>
Verbinding maken met uw mappen | Active Directory-referenties voor elk forest dat is verbonden met Azure AD ruimten | De machtigingen zijn afhankelijk van welke functies u inschakelen en kan worden gevonden in het [maken van de AD DS-account](#create-the-ad-ds-account) |Deze rekening wordt gebruikt voor het lezen en schrijven van directory-informatie tijdens de synchronisatie.
AD FS-Servers | Voor elke server in de lijst verzamelen het van referenties wanneer de aanmeldingsreferenties van de gebruiker uitvoeren van de wizard zijn onvoldoende om verbinding te maken | Domeinbeheerder | Installatie en configuratie van de functie van AD FS-server.
Web application proxyservers |Voor elke server in de lijst verzamelen het van referenties wanneer de aanmeldingsreferenties van de gebruiker uitvoeren van de wizard zijn onvoldoende om verbinding te maken | Lokale beheerder op de doelcomputer | Installatie en configuratie van de functie van de WAP-server.
Proxy vertrouwen referenties |Federation-service referenties (de referenties de proxy wordt gebruikt om in te schrijven voor een certificaat vertrouwen van de FS vertrouwen |Domeinaccount is een lokale beheerder van de AD FS-server | Eerste inschrijving van FS WAP vertrouwen certificaat.
AD FS-serviceaccount pagina "Optie rekening een domein gebruiken" | AD-gebruikersaccountreferenties | Domeingebruiker | De AD-gebruikersaccount waarvan de referenties zijn beschikbaar wordt als de aanmeldingsaccount van de AD FS-service gebruikt.

### <a name="create-the-ad-ds-account"></a>De AD DS-account maken
Tijdens de installatie van Azure AD verbinding maken met de account die u opgeeft op de pagina **verbinding maken met uw mappen** moet aanwezig zijn in Active Directory en machtigingen zijn vereist. De installatiewizard controleert niet of dat de machtigingen en eventuele problemen worden alleen gevonden tijdens de synchronisatie.

De machtigingen die u nodig hebt is afhankelijk van de optionele functies inschakelen. Als u meerdere domeinen hebt, moeten de machtigingen worden verleend voor alle domeinen in het forest. Als u deze functies niet inschakelt, worden de standaardmachtigingen voor de **Domeingebruiker** voldoende.

Functie | Machtigingen
------ | ------
Wachtwoord synchronisatie | <li>Directory-wijzigingen repliceren</li>  <li>Alle Directory repliceren gewijzigd
Implementatie van Exchange-hybride | Schrijfmachtigingen voor de kenmerken die zijn gedocumenteerd in [Exchange hybride writeback](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) voor gebruikers, groepen en contactpersonen.
Wachtwoord Write-back | Schrijfmachtigingen voor de kenmerken beschreven in [aan de slag met wachtwoordbeheer](../active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions) voor gebruikers.
Apparaat Write-back | Machtigingen die zijn toegekend met een PowerShell script zoals beschreven in het [apparaat Write-back](../active-directory-aadconnect-feature-device-writeback.md).
Groep Write-back | Lezen, maken, bijwerken en verwijderen van objecten groeperen in de organisatie-eenheid waar de verdelingen groepen gevonden worden moeten.

## <a name="upgrade"></a>Upgrade
Wanneer u een upgrade van een versie van Azure AD verbinden naar een nieuwe versie uitvoert, moet u de volgende machtigingen:

Principal | Vereiste machtigingen | Gebruikt voor
---- | ---- | ----
Met de installatiewizard gebruiker | De beheerder van de lokale server | Binaire bestanden worden bijgewerkt.
Met de installatiewizard gebruiker | Lid van ADSyncAdmins | Wijzigingen aanbrengen in de Sync-regels en andere configuratie.
Met de installatiewizard gebruiker | Als u een volledige SQL server: DBO (of soortgelijke) van de synchronisatie-engine-database | Controleer op wijzigingen in de database, zoals tabellen bijwerken met nieuwe kolommen.

## <a name="more-about-the-created-accounts"></a>Meer informatie over de gemaakte accounts

### <a name="active-directory-account"></a>Active Directory-account
Als u met express-instellingen, wordt een account gemaakt in Active Directory die wordt gebruikt voor synchronisatie. De account bevindt zich in het forest-hoofddomein in de container Users en heeft de naam voorafgegaan door **MSOL_**. De account wordt gemaakt met een lange complex wachtwoord dat niet verloopt. Als u een wachtwoordbeleid in uw domein hebt, zorg ervoor dat de lange en complexe wachtwoorden mogen voor deze account.

![AD-account](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

### <a name="azure-ad-connect-sync-service-accounts"></a>Azure AD verbinden sync-serviceaccounts
Een lokale serviceaccount wordt gemaakt door de wizard installeren (tenzij u de account die u wilt gebruiken in aangepaste instellingen opgeven). De rekening is **AAD_** voorafgegaan en worden gebruikt voor de werkelijke synchronisatieservice uitvoeren als. Als u Azure AD verbinden op een domeincontroller installeert, wordt de account in het domein gemaakt. Als u een externe server met SQL server of als u een proxyserver waarvoor verificatie vereist is, kan de **AAD_** -account moet zich bevinden in het domein.

![Sync-serviceaccount](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

De account wordt gemaakt met een lange complex wachtwoord dat niet verloopt.

Deze rekening wordt gebruikt voor het opslaan van de wachtwoorden voor de andere accounts op een veilige manier. Deze andere rekeningen wachtwoorden worden opgeslagen in de database is versleuteld. De persoonlijke sleutels van de coderingssleutels worden beveiligd met de services voor cryptografie geheim-codering met sleutels met Windows Data Protection API (DPAPI). U moet het wachtwoord voor de serviceaccount sinds Windows vervolgens de coderingssleutels om veiligheidsredenen vernietigt niet opnieuw instellen.

Als u een volledige SQL Server gebruikt, is de serviceaccount de DBO van de gemaakte database voor de synchronisatie-engine. De service werkt niet zoals bedoeld met andere machtigingen. Een SQL-aanmelding wordt ook gemaakt.

De account is ook machtigingen voor bestanden, registersleutels en andere objecten die betrekking hebben op de synchronisatie-Engine.

### <a name="azure-ad-service-account"></a>Azure AD-serviceaccount
Een account in Azure AD wordt voor de synchronisatieservice gemaakt. Deze account kan worden geïdentificeerd door de weergavenaam.

![AD-account](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

De naam van de server die op de account wordt gebruikt, kan worden geïdentificeerd in het tweede deel van de naam van de gebruiker. In de afbeelding is de naam van de FABRIKAMCON. Als u servers in staging, heeft elke server een eigen account. Er is een limiet van 10 sync-serviceaccounts in Azure AD.

Het serviceaccount wordt gemaakt met een lange complex wachtwoord dat niet verloopt. Een speciale rol **Directory synchronisatie-Accounts** die alleen machtigingen voor directory-synchronisatietaken heeft verleend. Deze speciale ingebouwde functie buiten de Azure AD verbinding maken met wizard kan niet worden toegekend en de Azure portal voor deze account de rol van **gebruiker**wordt weergegeven.

![Rol van AD-account](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccountrole.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](../active-directory-aadconnect.md).
