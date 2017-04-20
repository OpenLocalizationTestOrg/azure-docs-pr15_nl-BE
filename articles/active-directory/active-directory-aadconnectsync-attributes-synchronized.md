<properties
    pageTitle="Azure AD verbinden sync: kenmerken gesynchroniseerd met Azure Active Directory | Microsoft Azure"
    description="Dit zijn de kenmerken die zijn gesynchroniseerd met Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD verbinden sync: kenmerken gesynchroniseerd met Azure Active Directory
In dit onderwerp worden de kenmerken die worden gesynchroniseerd door sync Azure AD verbinden.  
De kenmerken worden gegroepeerd op de verwante Azure AD app.

## <a name="attributes-to-synchronize"></a>Kenmerken te synchroniseren
Een veelgestelde vraag is *Wat is de lijst van kenmerken te synchroniseren*. De standaard en de aanbevolen werkwijze is om de standaardkenmerken behouden, zodat een volledige GAL (Global Address List) in de cloud kan worden samengesteld om alle functies in Office 365 werkbelasting. In sommige gevallen zijn er sommige kenmerken die uw organisatie wil niet gesynchroniseerd met de cloud omdat deze kenmerken bevatten gevoelige of persoonlijke gegevens (persoonsgegevens) gegevens, zoals in het volgende voorbeeld:  
![ongeldige kenmerken](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

In dit geval beginnen met de lijst met kenmerken die in dit onderwerp en identificatie van de kenmerken die gevoelige of persoonlijke gegevens gegevens bevatten zou en kunnen niet worden gesynchroniseerd. Deselecteer deze kenmerken tijdens de installatie met [Azure AD app en kenmerk filteren](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering).

>[AZURE.WARNING] Wanneer kenmerken uitschakelt, moet u voorzichtig zijn en schakelt alleen die kenmerken absoluut niet mogelijk om te synchroniseren. Selectie opheffen van andere kenmerken kan een negatieve invloed op de functies hebben.

## <a name="office-365-proplus"></a>Office 365 ProPlus

| Naam van kenmerk| Gebruiker| Opmerking |
| --- | :-: | --- |
| accountEnabled| X| Wordt gedefinieerd als een account is ingeschakeld.|
| CN| X|  |
| displayName| X|  |
| objectSID| X| mechanische eigenschap. AD gebruikers-id gebruikt voor het synchroniseren tussen Azure AD en AD.|
| pwdLastSet| X| mechanische eigenschap. Om te weten wanneer reeds gepubliceerde tokens wordt ongeldig gebruikt. Gebruikt door zowel wachtwoord synchronisatie en federation.|
| sourceAnchor| X| mechanische eigenschap. Onveranderbare id relatie tussen ADDS en Azure AD onderhouden.|
| usageLocation| X| mechanische eigenschap. Het land van de gebruiker. Gebruikt voor toewijzing van de licentie.|
| userPrincipalName| X| UPN is de aanmeldings-ID voor de gebruiker. In de meeste gevallen als [e] dezelfde waarde.|

## <a name="exchange-online"></a>Exchange Online

| Naam van kenmerk| Gebruiker| Contact| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Wordt gedefinieerd als een account is ingeschakeld.|
| Office-assistent| X| X|  |  |
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| CO| X| X|  |  |
| bedrijf| X| X|  |  |
| countryCode| X| X|  |  |
| afdeling| X| X|  |  |
| Beschrijving| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| TelefoonPrivé| X| X|  |  |
| Info| X| X| X| Dit kenmerk wordt op dit moment niet worden gebruikt voor groepen.|
| Initialen| X| X|  |  |
| l| X| X|  |  |
| legacyExchangeDN| X| X| X|  |
| mailNickname| X| X| X|  |
| mangedBy|  |  | X|  |
| Manager| X| X|  |  |
| lid|  |  | X|  |
| mobiel| X| X|  |  |
| msDS-HABSeniorityIndex| X| X| X|  |
| msDS-PhoneticDisplayName| X| X| X|  |
| msExchArchiveGUID| X|  |  |  |
| msExchArchiveName| X|  |  |  |
| msExchAssistantName| X| X|  |  |
| msExchAuditAdmin| X|  |  |  |
| msExchAuditDelegate| X|  |  |  |
| msExchAuditDelegateAdmin| X|  |  |  |
| msExchAuditOwner| X|  |  |  |
| msExchBlockedSendersHash| X| X|  |  |
| msExchBypassAudit| X|  |  |  |
| msExchCoManagedByLink|  |  | X|  |
| msExchDelegateListLink| X|  |  |  |
| msExchELCExpirySuspensionEnd| X|  |  |  |
| msExchELCExpirySuspensionStart| X|  |  |  |
| msExchELCMailboxFlags| X|  |  |  |
| msExchEnableModeration| X|  | X|  |
| msExchExtensionCustomAttribute1| X| X| X| Dit kenmerk wordt op dit moment niet worden gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute2| X| X| X| Dit kenmerk wordt op dit moment niet worden gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute3| X| X| X| Dit kenmerk wordt op dit moment niet worden gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute4| X| X| X| Dit kenmerk wordt op dit moment niet worden gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute5| X| X| X| Dit kenmerk wordt op dit moment niet worden gebruikt door Exchange Online. |
| msExchHideFromAddressLists| X| X| X|  |
| msExchImmutableID| X|  |  |  |
| msExchLitigationHoldDate| X| X| X|  |
| msExchLitigationHoldOwner| X| X| X|  |
| msExchMailboxAuditEnable| X|  |  |  |
| msExchMailboxAuditLogAgeLimit| X|  |  |  |
| msExchMailboxGuid| X|  |  |  |
| msExchModeratedByLink| X| X| X|  |
| msExchModerationFlags| X| X| X|  |
| msExchRecipientDisplayType| X| X| X|  |
| msExchRecipientTypeDetails| X| X| X|  |
| msExchRemoteRecipientType| X|  |  |  |
| msExchRequireAuthToSendTo| X| X| X|  |
| msExchResourceCapacity| X|  |  |  |
| msExchResourceDisplay| X|  |  |  |
| msExchResourceMetaData| X|  |  |  |
| msExchResourceSearchProperties| X|  |  |  |
| msExchRetentionComment| X| X| X|  |
| msExchRetentionURL| X| X| X|  |
| msExchSafeRecipientsHash| X| X|  |  |
| msExchSafeSendersHash| X| X|  |  |
| msExchSenderHintTranslations| X| X| X|  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| msExchUserHoldPolicies| X|  |  |  |
| msOrg IsOrganizational|  |  | X|  |
| objectSID| X|  | X| mechanische eigenschap. AD gebruikers-id gebruikt voor het synchroniseren tussen Azure AD en AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherTelephone| X| X|  |  |
| pager| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postcode| X| X|  |  |
| proxyAddresses| X| X| X|  |
| publicDelegates| X| X| X|  |
| pwdLastSet| X|  |  | mechanische eigenschap. Om te weten wanneer reeds gepubliceerde tokens wordt ongeldig gebruikt. Gebruikt door zowel wachtwoord synchronisatie en federation.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Afgeleid van groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mechanische eigenschap. Onveranderbare id relatie tussen ADDS en Azure AD onderhouden.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| titel| X| X|  |  |
| unauthOrig| X| X| X|  |
| usageLocation| X|  |  | mechanische eigenschap. Het land van de gebruiker. Gebruikt voor toewijzing van de licentie.|
| userCertificate| X| X|  |  |
| userPrincipalName| X|  |  | UPN is de aanmeldings-ID voor de gebruiker. In de meeste gevallen als [e] dezelfde waarde.|
| userSMIMECertificates| X| X|  |  |
| wWWHomePage| X| X|  |  |

## <a name="sharepoint-online"></a>SharePoint Online

| Naam van kenmerk| Gebruiker| Contact| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Wordt gedefinieerd als een account is ingeschakeld.|
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| CO| X| X|  |  |
| bedrijf| X| X|  |  |
| countryCode| X| X|  |  |
| afdeling| X| X|  |  |
| Beschrijving| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| hideDLMembership|  |  | X|  |
| TelefoonPrivé| X| X|  |  |
| Info| X| X| X|  |
| initialen| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| e-mail| X| X| X|  |
| mailnickname| X| X| X|  |
| managedBy|  |  | X|  |
| Manager| X| X|  |  |
| lid|  |  | X|  |
| middleName| X| X|  |  |
| mobiel| X| X|  |  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointLinkedBy| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| objectSID| X|  | X| mechanische eigenschap. AD gebruikers-id gebruikt voor het synchroniseren tussen Azure AD en AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherIpPhone| X| X|  |  |
| otherMobile| X| X|  |  |
| otherPager| X| X|  |  |
| otherTelephone| X| X|  |  |
| pager| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postcode| X| X|  |  |
| postOfficeBox| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mechanische eigenschap. Om te weten wanneer reeds gepubliceerde tokens wordt ongeldig gebruikt. Gebruikt door zowel wachtwoord synchronisatie en federation.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Afgeleid van groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mechanische eigenschap. Onveranderbare id relatie tussen ADDS en Azure AD onderhouden.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| titel| X| X|  |  |
| unauthOrig| X| X| X|  |
| URL| X| X|  |  |
| usageLocation| X|  |  | mechanische eigenschap. Het land van de gebruiker. Gebruikt voor toewijzing van de licentie.|
| userPrincipalName| X|  |  | UPN is de aanmeldings-ID voor de gebruiker. In de meeste gevallen als [e] dezelfde waarde.|
| wWWHomePage| X| X|  |  |

## <a name="lync-online"></a>Lync Online

| Naam van kenmerk| Gebruiker| Contact| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Wordt gedefinieerd als een account is ingeschakeld.|
| c| X| X|  |  |
| CN| X|  | X|  |
| CO| X| X|  |  |
| bedrijf| X| X|  |  |
| afdeling| X| X|  |  |
| Beschrijving| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X| X|  |
| givenName| X| X|  |  |
| TelefoonPrivé| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| e-mail| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| Manager| X| X|  |  |
| lid|  |  | X|  |
| mobiel| X| X|  |  |
| msExchHideFromAddressLists| X| X| X|  |
| msRTCSIP ApplicationOptions| X|  |  |  |
| msRTCSIP DeploymentLocator| X| X|  |  |
| msRTCSIP-lijn| X| X|  |  |
| msRTCSIP OptionFlags| X| X|  |  |
| msRTCSIP OwnerUrn| X|  |  |  |
| msRTCSIP PrimaryUserAddress| X| X|  |  |
| msRTCSIP UserEnabled| X| X|  |  |
| objectSID| X|  | X| mechanische eigenschap. AD gebruikers-id gebruikt voor het synchroniseren tussen Azure AD en AD.|
| otherTelephone| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postcode| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mechanische eigenschap. Om te weten wanneer reeds gepubliceerde tokens wordt ongeldig gebruikt. Gebruikt door zowel wachtwoord synchronisatie en federation.|
| securityEnabled|  |  | X| Afgeleid van groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mechanische eigenschap. Onveranderbare id relatie tussen ADDS en Azure AD onderhouden.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| titel| X| X|  |  |
| usageLocation| X|  |  | mechanische eigenschap. Het land van de gebruiker. Gebruikt voor toewijzing van de licentie.|
| userPrincipalName| X|  |  | UPN is de aanmeldings-ID voor de gebruiker. In de meeste gevallen als [e] dezelfde waarde.|
| wWWHomePage| X| X|  |  |

## <a name="azure-rms"></a>Azure RMS

| Naam van kenmerk| Gebruiker| Contact| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Wordt gedefinieerd als een account is ingeschakeld.|
| CN| X|  | X| Naam of alias. In de meeste gevallen het voorvoegsel van de waarde van de [e].|
| displayName| X| X| X| Een tekenreeks met de naam vaak weergegeven als de beschrijvende naam (voornaam achternaam).|
| e-mail| X| X| X| volledige e-mailadres.|
| lid|  |  | X|  |
| objectSID| X|  | X| mechanische eigenschap. AD gebruikers-id gebruikt voor het synchroniseren tussen Azure AD en AD.|
| proxyAddresses| X| X| X| mechanische eigenschap. Door AD Azure gebruikt. Alle secundaire e-mailadressen voor de gebruiker bevat.|
| pwdLastSet| X|  |  | mechanische eigenschap. Om te weten wanneer reeds gepubliceerde tokens wordt ongeldig gebruikt.|
| securityEnabled|  |  | X| Afgeleid van groupType.|
| sourceAnchor| X| X| X| mechanische eigenschap. Onveranderbare id relatie tussen ADDS en Azure AD onderhouden.|
| usageLocation| X|  |  | mechanische eigenschap. Het land van de gebruiker. Gebruikt voor toewijzing van de licentie.|
| userPrincipalName| X|  |  | De UPN is de aanmeldings-ID voor de gebruiker. In de meeste gevallen als [e] dezelfde waarde.|

## <a name="intune"></a>Intune

| Naam van kenmerk| Gebruiker| Contact| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Wordt gedefinieerd als een account is ingeschakeld.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Beschrijving| X| X| X|  |
| displayName| X| X| X|  |
| e-mail| X| X| X|  |
| mailnickname| X| X| X|  |
| lid|  |  | X|  |
| objectSID| X|  | X| mechanische eigenschap. AD gebruikers-id gebruikt voor het synchroniseren tussen Azure AD en AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mechanische eigenschap. Om te weten wanneer reeds gepubliceerde tokens wordt ongeldig gebruikt. Gebruikt door zowel wachtwoord synchronisatie en federation.|
| securityEnabled|  |  | X| Afgeleid van groupType|
| sourceAnchor| X| X| X| mechanische eigenschap. Onveranderbare id relatie tussen ADDS en Azure AD onderhouden.|
| usageLocation| X|  |  | mechanische eigenschap. Het land van de gebruiker. Gebruikt voor toewijzing van de licentie.|
| userPrincipalName| X|  |  | UPN is de aanmeldings-ID voor de gebruiker. In de meeste gevallen als [e] dezelfde waarde.|

## <a name="dynamics-crm"></a>Dynamics CRM

| Naam van kenmerk| Gebruiker| Contact| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Wordt gedefinieerd als een account is ingeschakeld.|
| c| X| X|  |  |
| CN| X|  | X|  |
| CO| X| X|  |  |
| bedrijf| X| X|  |  |
| countryCode| X| X|  |  |
| Beschrijving| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| l| X| X|  |  |
| managedBy|  |  | X|  |
| Manager| X| X|  |  |
| lid|  |  | X|  |
| mobiel| X| X|  |  |
| objectSID| X|  | X| mechanische eigenschap. AD gebruikers-id gebruikt voor het synchroniseren tussen Azure AD en AD.|
| physicalDeliveryOfficeName| X| X|  |  |
| Postcode| X| X|  |  |
| preferredLanguage| X|  |  |  |
| pwdLastSet| X|  |  | mechanische eigenschap. Om te weten wanneer reeds gepubliceerde tokens wordt ongeldig gebruikt. Gebruikt door zowel wachtwoord synchronisatie en federation.|
| securityEnabled|  |  | X| Afgeleid van groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mechanische eigenschap. Onveranderbare id relatie tussen ADDS en Azure AD onderhouden.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| titel| X| X|  |  |
| usageLocation| X|  |  | mechanische eigenschap. Het land van de gebruiker. Gebruikt voor toewijzing van de licentie.|
| userPrincipalName| X|  |  | UPN is de aanmeldings-ID voor de gebruiker. In de meeste gevallen als [e] dezelfde waarde.|

## <a name="3rd-party-applications"></a>3e partij toepassingen
Deze groep is een verzameling kenmerken die worden gebruikt als de minimale kenmerken die nodig zijn voor een algemene belasting of toepassing. Worden kan gebruikt voor een belasting die niet voorkomt in een andere sectie of voor een niet-Microsoft-toepassing. Expliciet wordt deze gebruikt voor het volgende:

- Yammer (alleen gebruiker wordt gebruikt)
- [Hybride Business-to-Business (B2B) cross org samenwerkingsscenario's mogelijk aangeboden door bronnen zoals SharePoint](http://go.microsoft.com/fwlink/?LinkId=747036)

Deze groep is een verzameling kenmerken die kunnen worden gebruikt als de map Azure AD niet wordt gebruikt voor de ondersteuning van Office 365, Dynamics of Intune. Er is een kleine set van belangrijke kenmerken.

| Naam van kenmerk| Gebruiker| Contact| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Wordt gedefinieerd als een account is ingeschakeld.|
| CN| X|  | X|  |
| displayName| X| X| X|  |
| givenName| X| X|  |  |
| e-mail| X|  | X|  |
| managedBy|  |  | X|  |
| mailNickName| X| X| X|  |
| lid|  |  | X|  |
| objectSID| X|  |  | mechanische eigenschap. AD gebruikers-id gebruikt voor het synchroniseren tussen Azure AD en AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mechanische eigenschap. Om te weten wanneer reeds gepubliceerde tokens wordt ongeldig gebruikt. Gebruikt door zowel wachtwoord synchronisatie en federation.|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mechanische eigenschap. Onveranderbare id relatie tussen ADDS en Azure AD onderhouden.|
| usageLocation| X|  |  | mechanische eigenschap. Het land van de gebruiker. Gebruikt voor toewijzing van de licentie.|
| userPrincipalName| X|  |  | UPN is de aanmeldings-ID voor de gebruiker. In de meeste gevallen als [e] dezelfde waarde.|

## <a name="windows-10"></a>Windows 10
Een computer(device) Windows 10 domein behoren synchroniseert bepaalde kenmerken naar Azure AD. Zie [verbinding maken met een domein behoren apparaten Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)voor meer informatie op de scenario's. Deze kenmerken altijd synchroniseren en Windows 10 wordt niet als een app die u kunt deselecteren. Een computer deel uitmaakt van een domein met Windows 10 wordt aangeduid met het kenmerk userCertificate gevuld hebben.

| Naam van kenmerk| Apparaat| Opmerking |
| --- | :-: | --- |
| accountEnabled| X| |
| deviceTrustType| X| Vastgelegde waarde voor computers die deel uitmaakt van een domein. |
| displayName | X| |
| MS-DS-CreatorSID | X| Ook wel registeredOwnerReference genoemd.|
| objectGUID | X| DeviceID ook genoemd.|
| objectSID | X| Ook wel onPremisesSecurityIdentifier genoemd.|
| operatingSystem | X| Ook wel deviceOSType genoemd.|
| operatingSystemVersion | X| Ook wel deviceOSVersion genoemd.|
| userCertificate | X| |

Deze kenmerken voor de **gebruiker** zijn een aanvulling op de andere toepassingen die u hebt geselecteerd.  

| Naam van kenmerk| Gebruiker| Opmerking |
| --- | :-: | --- |
| domainFQDN| X| DNS-domeinnaam ook genoemd. Bijvoorbeeld contoso.com.|
| domainNetBios| X| NetBIOS-naam ook genoemd. Bijvoorbeeld CONTOSO.|

## <a name="exchange-hybrid-writeback"></a>Exchange hybride Write-back
Deze kenmerken worden opgeslagen in van Azure AD voor bedrijfsruimten Active Directory wanneer u deze optie selecteren om **Exchange-hybride**. Afhankelijk van uw versie van Exchange, kunnen minder kenmerken worden gesynchroniseerd.

| Naam van kenmerk| Gebruiker| Contact| Groep| Opmerking |
| --- | :-: | :-: | :-: | --- |
| msDS-ExternalDirectoryObjectID| X|  |  | Afgeleid van cloudAnchor in Azure AD. Dit kenmerk is nieuw in 2016.|
| msExchArchiveStatus| X|  |  | Online archief: Kunnen klanten voor het archiveren van e-mailberichten.|
| msExchBlockedSendersHash| X|  |  | Filters: Schrijft terug filteren op gebouwen en online veilige en geblokkeerde afzender-gegevens van clients.|
| msExchSafeRecipientsHash| X|  |  | Filters: Schrijft terug filteren op gebouwen en online veilige en geblokkeerde afzender-gegevens van clients.|
| msExchSafeSendersHash| X|  |  | Filters: Schrijft terug filteren op gebouwen en online veilige en geblokkeerde afzender-gegevens van clients.|
| msExchUCVoiceMailSettings| X|  |  | Unified Messaging (UM) - Online voicemail inschakelen: gebruikt door Microsoft Lync Server integratie om Lync Server aan te geven op-ruimten die de gebruiker voicemail in de on line services is.|
| msExchUserHoldPolicies| X|  |  | Wachtruimte rechtszaak: Hiermee cloud services om te bepalen welke gebruikers zijn onder de rechtszaak houdt.|
| proxyAddresses| X| X| X| Alleen de x500 is het adres van Exchange Online geplaatst.|

## <a name="device-writeback"></a>Apparaat Write-back
Apparaatobjecten gemaakt in Active Directory. Deze objecten kunnen worden toegevoegd aan AD Azure apparaten of computers met Windows 10 domein behoren.

| Naam van kenmerk| Apparaat| Opmerking |
| --- | :-: | --- |
| altSecurityIdentities | X| |
| displayName | X| |
| DN-naam | X| |
| msDS-CloudAnchor | X| |
| msDS-DeviceID | X| |
| msDS-DeviceObjectVersion | X| |
| msDS-DeviceOSType | X| |
| msDS-DeviceOSVersion | X| |
| msDS-DevicePhysicalIDs | X| |
| msDS-KeyCredentialLink | X| Alleen met Windows Server 2016 AD schema |
| msDS-IsCompliant | X| |
| msDS IsEnabled | X| |
| msDS-IsManaged | X| |
| msDS RegisteredOwner | X| |


## <a name="notes"></a>Notities

- Wanneer u een alternatieve ID, is het in lokalen kenmerk userPrincipalName gesynchroniseerd met het kenmerk Azure AD onPremisesUserPrincipalName. Het kenmerk alternatieve ID voor een voorbeeld van de afdruk, wordt gesynchroniseerd met de Azure AD kenmerk userPrincipalName.
- In de lijst hierboven is het objecttype **gebruiker** ook van toepassing op het object type **iNetOrgPerson**.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het configureren van [Azure AD Connect worden gesynchroniseerd](active-directory-aadconnectsync-whatis.md) .

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
