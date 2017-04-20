<properties
    pageTitle="Azure Active Directory Domain Services: Troubleshooting Guide | Microsoft Azure"
    description="Problemen oplossen met Azure AD Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD Domain Services - handleiding voor het oplossen van problemen
Dit artikel bevat tips voor het oplossen van problemen voor problemen die optreden kunnen bij het instellen of beheren van domeinservices Azure Active Directory (AD).


## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>U kunt geen Azure AD Domain Services voor uw map Azure AD inschakelen
In deze sectie kunt u fouten oplossen wanneer u probeert in te schakelen Azure AD Domain Services voor de map en deze mislukt of weer op 'Uitgeschakeld' wordt ingeschakeld.

Kies de stappen voor probleemoplossing die overeenkomen met het foutbericht dat u ondervindt.

|**Foutbericht**|**Resolutie**|
|---|:---|
|*De naam contoso100.com is al in gebruik is op dit netwerk. Geef een naam die niet in gebruik is.*|[Domein naamconflict in het virtuele netwerk](active-directory-ds-troubleshooting.md#domain-name-conflict)
|*Domain Services kunnen niet worden ingeschakeld in deze huurder Azure AD. De service heeft niet voldoende machtigingen om de toepassing met de naam 'Sync Azure AD Domain Services'. Verwijderen van de toepassing met de naam 'Sync Azure AD Domain Services' en probeer het vervolgens voor het domein inschakelen voor uw huurder Azure AD.*|[Domain Services geen voldoende machtigingen voor de toepassing van Sync van Azure AD Domain Services](active-directory-ds-troubleshooting.md#inadequate-permissions)|
|*Domain Services kunnen niet worden ingeschakeld in deze huurder Azure AD. De toepassing Domain Services in uw huurder Azure AD heeft niet de vereiste machtigingen voor het domein inschakelen. Verwijderen van de toepassing met de aanvraag-id d87dcbc6-a371-462e-88e3-28ad15ec4e64 en probeer het vervolgens voor het domein inschakelen voor uw huurder Azure AD.*|[De toepassing Domain Services is niet correct geconfigureerd in uw huurder](active-directory-ds-troubleshooting.md#invalid-configuration)|
|*Domain Services kunnen niet worden ingeschakeld in deze huurder Azure AD. De toepassing Microsoft Azure AD is uitgeschakeld in uw huurder Azure AD. De toepassing met de aanvraag-id-00000002-0000-0000-c000-000000000000 inschakelen en probeer het vervolgens voor het domein inschakelen voor uw huurder Azure AD.*|[De toepassing Microsoft Graph is uitgeschakeld in uw huurder Azure AD](active-directory-ds-troubleshooting.md#microsoft-graph-disabled)|


### <a name="domain-name-conflict"></a>Conflicten met domein
**Foutbericht:**

*De naam contoso100.com is al in gebruik is op dit netwerk. Geef een naam die niet in gebruik is.*

**Herstel:**

Zorg ervoor dat er geen een bestaand domein met dezelfde naam van het domein op dat virtuele netwerk beschikbaar. Bijvoorbeeld, gaan we uit van een domein met de naam 'contoso.com' al beschikbaar op de geselecteerde virtuele netwerk. Later, probeert u een beheerde Azure AD Domain Services-domein met dezelfde domeinnaam (dat wil zeggen ' contoso.com') op die virtuele netwerk inschakelen. U kunt een fout optreden als u Azure AD Domain Services inschakelen.

Deze fout is het gevolg van naamconflicten voor de domeinnaam in die virtuele netwerk. In dit geval moet u een andere naam voor het instellen van uw beheerde Azure AD Domain Services-domein. U kunt ook uit het bestaande domein inrichten en gaat u verder met het AD-Domain Azure Services inschakelen.


### <a name="inadequate-permissions"></a>Onvoldoende machtigingen
**Foutbericht:**

*Domain Services kunnen niet worden ingeschakeld in deze huurder Azure AD. De service heeft niet voldoende machtigingen om de toepassing met de naam 'Sync Azure AD Domain Services'. Verwijderen van de toepassing met de naam 'Sync Azure AD Domain Services' en probeer het vervolgens voor het domein inschakelen voor uw huurder Azure AD.*

**Herstel:**

Controleer of er een toepassing met de naam 'Azure AD Domain Services Sync' in de map Azure AD is. Als deze toepassing bestaat, het verwijderen en opnieuw Azure AD Domain Services inschakelen.

Voer de volgende stappen uit om te controleren op de aanwezigheid van de toepassing en verwijderen, als de toepassing bestaat:

  1. Ga naar de **Azure klassieke portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
  2. Selecteer het knooppunt **Active Directory** in het linkerdeelvenster.
  3. Selecteer de huurder Azure AD (map) dat u wilt inschakelen, Azure AD Domain Services.
  4. Ga naar het tabblad **toepassingen** .
  5. **Mijn bedrijf eigenaar is van toepassingen** de optie in de vervolgkeuzelijst.
  6. Controleer of een toepassing genoemd **Sync van Azure AD Domain Services**. Als de toepassing bestaat, gaat u verder om deze te verwijderen.
  7. Zodra u de toepassing hebt verwijderd, probeer nogmaals Azure AD Domain Services inschakelen.


### <a name="invalid-configuration"></a>Ongeldige configuratie
**Foutbericht:**

*Domain Services kunnen niet worden ingeschakeld in deze huurder Azure AD. De toepassing Domain Services in uw huurder Azure AD heeft niet de vereiste machtigingen voor het domein inschakelen. Verwijderen van de toepassing met de aanvraag-id d87dcbc6-a371-462e-88e3-28ad15ec4e64 en probeer het vervolgens voor het domein inschakelen voor uw huurder Azure AD.*

**Herstel:**

Controleer of er een toepassing met de naam 'AzureActiveDirectoryDomainControllerServices' (met een aanvraag-id van de d87dcbc6-a371-462e-88e3-28ad15ec4e64) in uw map Azure AD. Als deze toepassing bestaat, moet u deze verwijderen en opnieuw Azure AD Domain Services inschakelen.

De volgende PowerShell-script gebruiken om de toepassing te vinden en te verwijderen.

> [AZURE.NOTE] Dit script maakt gebruik van cmdlets **Azure AD PowerShell versie 2** . Raadpleeg de [documentatie AzureAD PowerShell](https://msdn.microsoft.com/library/azure/mt757189.aspx)voor een volledige lijst van alle beschikbare cmdlets en het downloaden van de module.

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graph uitgeschakeld
**Foutbericht:**

Domain Services kunnen niet worden ingeschakeld in deze huurder Azure AD. De toepassing Microsoft Azure AD is uitgeschakeld in uw huurder Azure AD. De toepassing met de aanvraag-id-00000002-0000-0000-c000-000000000000 inschakelen en probeer het vervolgens voor het domein inschakelen voor uw huurder Azure AD.

**Herstel:**

Controleer als u een toepassing met de id 00000002-0000-0000-c000-000000000000 hebt uitgeschakeld. Deze toepassing wordt de toepassing Microsoft Azure AD en Graph API toegang biedt tot de huurder Azure AD. Azure AD Domain Services, moet deze toepassing worden ingeschakeld voor het synchroniseren van uw huurder Azure AD aan uw domein beheerd.

Fout wilt verhelpen, deze toepassing inschakelen in en probeer het vervolgens voor het domein inschakelen voor uw huurder Azure AD.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Gebruikers kunnen zich aanmelden bij het AD-Domain Azure Services beheerde domein geen
Als een of meer gebruikers in uw huurder Azure AD zich niet aanmelden bij het nieuwe beheerde domein, voert u de volgende stappen:

- **Aanmelden met UPN-notatie:** Probeer aan te melden in de UPN-notatie (bijvoorbeeld, 'joeuser@contoso.com') in plaats van de indeling SAMAccountName (CONTOSO\joeuser). De SAMAccountName kan automatisch worden gegenereerd voor gebruikers waarvoor UPN-voorvoegsel is te lang of is hetzelfde als een andere gebruiker in het domein beheerd. De UPN-indeling is gegarandeerd uniek binnen een huurder Azure AD.

> [AZURE.NOTE] Wij raden u aan de UPN-indeling gebruikt voor aanmelding bij het AD-Domain Azure Services beheerde domein.

- Zorg ervoor dat [Wachtwoordsynchronisatie ingeschakeld](active-directory-ds-getting-started-password-sync.md) volgens de stappen in de handleiding aan de slag.

- **Externe accounts:** Zorg ervoor dat de gebruikersaccount waarin dit probleem optreedt niet een externe account in de huurder Azure AD. Voorbeelden van externe accounts zijn Microsoft-accounts (bijvoorbeeld, 'joe@live.com') of gebruikersaccounts van een externe directory Azure AD. Aangezien Azure AD Domain Services geen referenties voor deze gebruikersaccounts, aanmelden deze gebruikers niet bij het domein beheerd.

- **Accounts gesynchroniseerd:** Als de desbetreffende gebruikersaccounts uit een directory op de ruimten worden gesynchroniseerd, moet u de volgende punten controleren:
    - U hebt geïnstalleerd of bijgewerkt naar de [meest recente aanbevolen versie van Azure AD verbinden](active-directory-ds-getting-started-password-sync.md#install-or-update-azure-ad-connect).

    - U hebt geconfigureerd Azure AD verbinding maken met [een volledige synchronisatie uitvoeren](active-directory-ds-getting-started-password-sync.md).

    - Afhankelijk van de grootte van de map mogelijk duren voor gebruikersaccounts en referentie-hashes in Azure AD Domain Services beschikbaar zijn. Zorg ervoor dat u wachten lang genoeg voordat opnieuw wordt geprobeerd het verificatie (afhankelijk van de grootte van de map - enkele uren in een dag of twee voor grote mappen).

    - Als het probleem zich blijft voordoen nadat de voorgaande stappen gecontroleerd, probeer het opnieuw starten van de Service Microsoft Azure AD Sync. Vanaf uw computer synchroniseren vanaf de opdrachtprompt starten en uitvoeren van de volgende opdrachten:
      1. net stop 'Microsoft Azure AD Sync'
      2. net start 'Microsoft Azure AD Sync'

- **Alleen cloud - accounts**: als de gebruikersaccount waarin dit probleem optreedt een alleen-cloud account is, ervoor te zorgen dat de gebruiker het wachtwoord is gewijzigd nadat u het AD-Domain Azure Services ingeschakeld. Deze stap zorgt ervoor dat de referentie hashes die zijn vereist voor AD-domein Azure Services moet worden gegenereerd.


## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Gebruikers verwijderd uit uw huurder Azure AD worden niet verwijderd uit uw domein beheerd
Azure AD beschermt u tegen onopzettelijk verwijderen van gebruikersobjecten. Wanneer u een gebruikersaccount uit uw huurder Azure AD verwijdert, wordt het bijbehorende gebruikersobject verplaatst naar de Prullenbak. Wanneer deze bewerking is gesynchroniseerd met uw beheerde domein, worden de bijbehorende gebruikersaccount uitgeschakeld worden gemarkeerd. Deze functie helpt u bij het herstellen of ongedaan maken van de account later.

Om volledig de gebruikersaccount verwijderen uit uw domein beheerd, de gebruiker verwijderen definitief uit uw huurder Azure AD. Gebruik de cmdlet PowerShell verwijderen MsolUser met de '-RemoveFromRecycleBin' optie, zoals beschreven in dit [MSDN-artikel](https://msdn.microsoft.com/library/azure/dn194132.aspx).


## <a name="contact-us"></a>Neem contact op met ons
Neem contact op met het productteam Azure Active Directory Domain Services op [feedback te delen of voor ondersteuning] (active-directory-ds-contact-us.md).
