<properties
    pageTitle="Azure Active Directory PowerShell-cmdlets van voorbeeld voor groepsbeheer in Azure AD | Microsoft Azure"
    description="Op deze pagina vindt voorbeelden om u te helpen bij het beheren van uw groepen in Azure Active Directory PowerShell"
    keywords="Azure PowerShell AD, Azure Active Directory, beheer van groepen, groep"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="curtand"/>

# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>Azure Active Directory voorbeeld cmdlets voor het groepsbeheer

> [AZURE.SELECTOR]
- [Azure portal](active-directory-groups-create-azure-portal.md)
- [Azure klassieke portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Het volgende document biedt u voorbeelden van hoe de groepen in Azure Active Directory (AD Azure) beheren met PowerShell.  Ook bevat informatie over hoe u instellen met de module Azure AD PowerShell voorbeeld. Eerst moet u [de module Azure AD PowerShell downloaden](http://go.microsoft.com/fwlink/p/?LinkId=828627).

## <a name="installing-the-azure-ad-powershell-module"></a>De module Azure AD PowerShell installeren

Als u wilt installeren in de module AzureAD PowerShell voorbeeld, kunt u de volgende opdrachten gebruiken:

    PS C:\Windows\system32> install-module azureadpreview

Om te controleren dat de module preview is geïnstalleerd, gebruikt u de volgende opdracht:

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

U kunt nu starten met de cmdlets in de module. Raadpleeg de [on line documentatie](https://msdn.microsoft.com/library/azure/mt757216.aspx)voor een volledige beschrijving van de cmdlets in de module AzureAD voorbeeld.

## <a name="connecting-to-the-directory"></a>Verbinding maken met de map
Voordat u kunt groepen met Azure AD PowerShell voorbeeld-cmdlets beheren, moet u de PowerShell-sessie naar de map die u wilt beheren. Gebruik hiervoor de volgende opdracht:

    PS C:\Windows\system32> Connect-AzureAD -Force

De cmdlet wordt u gevraagd om de referenties die u gebruiken wilt voor toegang tot de map. In dit voorbeeld gebruiken we karen@drumkit.onmicrosoft.com voor toegang tot de map demonstratie. De cmdlet retourneert een bevestiging om de sessie met succes is aangesloten op de map weer te geven:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

U kunt nu starten met behulp van het voorbeeld AzureAD cmdlets voor het beheren van groepen in de directory.

## <a name="retrieving-groups"></a>Ophalen van groepen
Bestaande groepen worden opgehaald uit de map kunt u de cmdlet Get-AzureADGroups. Voor het ophalen van alle groepen in Active directory, gebruikt u de cmdlet zonder parameters:

    PS C:\Windows\system32> get-azureadgroup

De cmdlet retourneert alle groepen in de directory verbonden.

U kunt de object-id - parameter op te halen van een specifieke groep waarvoor u de id van de groep opgeven:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Nu retourneert de cmdlet de groep waarvan de id overeenkomt met de waarde van de parameter die u hebt ingevoerd:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

U kunt zoeken naar een specifieke groep met behulp van de - filterparameter. Deze parameter wordt een filtervoorwaarde ODATA en alle groepen die overeenkomen met het filter, zoals in het volgende voorbeeld geeft als resultaat:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Houd er rekening mee dat de AzureAD PowerShell-cmdlets de OData-query standaard implementeren, meer informatie vindt u [hier](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Groepen maken
Maakt u een nieuwe groep in de map met de cmdlet New-AzureADGroup. Deze cmdlet maakt u een nieuwe beveiligingsgroep met de naam 'Marketing':

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Bijwerken van groepen
Om te werken in een bestaande groep, gebruikt u de cmdlet Set-AzureADGroup. In dit voorbeeld wijzigt we de eigenschap DisplayName van de groep 'Beheerders Intune'. Eerst wij de groep met de cmdlet Get-AzureADGroup wilt zoeken en filteren met behulp van het kenmerk DisplayName:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

We vervolgens wijzigt de eigenschap Description op de nieuwe waarde 'Intune apparaat beheerders':

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Nu als wij de groep opnieuw vinden, we dat de eigenschap Description wordt bijgewerkt met de nieuwe waarde zien:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Groepen verwijderen
Groepen uit de map verwijderen, gebruikt u de cmdlet verwijderen AzureADGroup als volgt:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Leden van groepen beheren
Als u nieuwe leden toevoegen aan een groep, gebruikt u de cmdlet Add-AzureADGroupMember. Deze opdracht voegt u een lid toe aan de beheerdersgroep van Intune die we hebben gebruikt in het vorige voorbeeld:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

De parameter - object-id is de object-id van de groep waaraan wij willen een lid toevoegen en de RefObjectId - id van de gebruiker die u wilt toevoegen als lid aan de groep is.

Als u de bestaande leden van een groep, gebruikt u de cmdlet Get-AzureADGroupMember, zoals in dit voorbeeld:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                        8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Met de cmdlet verwijderen AzureADGroupMember, zoals hier wordt weergegeven, verwijdert u het lid dat we eerder toegevoegd aan de groep:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Om te controleren of de membership(s) van de groep van een gebruiker, gebruikt u de cmdlet selecteren AzureADGroupIdsUserIsMemberOf. Deze cmdlet wordt als de parameters de object-id van de gebruiker waarvoor u het groepslidmaatschap te controleren en een lijst met groepen waarvan het lidmaatschap te controleren. De lijst van groepen moet worden verstrekt in de vorm van een complexe variabele van het type 'Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck', zodat we eerst moet een variabele met dat type maken:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Wij bieden vervolgens waarden voor de groupIds om te controleren in het kenmerk 'GroupIds' van deze complexe variabele:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Nu, als we het groepslidmaatschap van een gebruiker met de object-id 72cd4bbd-2594-40a2-935c-016f3cfeeeea ten opzichte van de groepen in $g, dat we moeten gebruiken:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                               Value
    -------------                                                                                               -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


De geretourneerde waarde is een lijst met groepen die deze gebruiker lid is. U kunt ook deze methode als u wilt controleren, contactpersonen, groepen of beveiligings-Principals Service lidmaatschap voor een bepaalde lijst met groepen, selecteer AzureADGroupIdsContactIsMemberOf, selecteert u AzureADGroupIdsGroupIsMemberOf of selecteer AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="managing-owners-of-groups"></a>Eigenaren van groepen beheren
Eigenaars toevoegen aan een groep, gebruikt u de cmdlet Add-AzureADGroupOwner:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

De parameter - object-id is de object-id van de groep waaraan we wilt toevoegen van een eigenaar en de RefObjectId - id van de gebruiker die u wilt toevoegen als eigenaar van de groep is.

U haalt de eigenaars van een groep met de Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

De cmdlet retourneert de lijst met voorkeurseigenaars voor de opgegeven groep:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Als u een eigenaar verwijderen uit een groep wilt, gebruikt u verwijderen AzureADGroupOwner:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>Volgende stappen

U kunt meer Azure Active Directory PowerShell documentatie vinden op [Azure Active Directory Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260).

* [Toegang tot bronnen te beheren met Azure Active Directory-groepen](active-directory-manage-groups.md)

* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
