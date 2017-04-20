<properties
    pageTitle="Azure AD Domain Services: Wachtwoordsynchronisatie inschakelen | Microsoft Azure"
    description="Aan de slag met Azure Active Directory Domain Services"
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
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Wachtwoordsynchronisatie Azure AD Domain Services inschakelen
In de voorafgaande taken u Azure AD Domain Services voor uw huurder Azure AD ingeschakeld. De volgende taak is het inschakelen van de synchronisatie van wachtwoorden naar Azure AD Domain Services. Zodra de synchronisatie van de referentie is ingesteld, zich gebruikers aanmelden bij de beheerde domein met hun zakelijke referenties.

De stappen zijn verschillende op basis van of uw organisatie een alleen-wolk Azure AD is pachters of is ingesteld om te synchroniseren met uw op ruimten directory Azure AD verbinden met.

<br>

> [AZURE.SELECTOR]
- [Alleen cloud Azure AD pachters](active-directory-ds-getting-started-password-sync.md)
- [Azure AD huurder gesynchroniseerd](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Taak 5: Wachtwoordsynchronisatie AAD Domain Services voor een gesynchroniseerde Azure inschakelen AD huurder
A gesynchroniseerd Azure AD huurder is ingesteld om te synchroniseren met een van uw organisatie op ruimten directory Azure AD verbinden met. Azure AD verbinden niet wordt gesynchroniseerd NTLM en Kerberos-referenties hashes naar Azure AD standaard. Azure AD-domein om Services te gebruiken, moet u configureren Azure AD verbinden om te synchroniseren van referentie-hashes voor NTLM en Kerberos-verificatie is vereist. De volgende stappen kunnen synchronisatie van de vereiste referenties-hashes aan de huurder Azure AD.


### <a name="install-or-update-azure-ad-connect"></a>Installeren of bijwerken van Azure AD verbinden
Installeer de meest recente aanbevolen release van Azure AD verbinden op een domein gekoppelde computer. Als u een bestaand exemplaar van Azure AD Connect setup, moet u bijwerken voor het gebruik van de nieuwste versie van Azure AD verbinden. Om te voorkomen dat bekende problemen/bugs die mogelijk al zijn gecorrigeerd, te zorgen dat u altijd de nieuwste versie van Azure AD verbinden.

**[Azure AD downloaden verbinding](http://www.microsoft.com/download/details.aspx?id=47594)**

Aanbevolen versie: **1.1.281.0** - gepubliceerd op 7 September 2016.

  > [AZURE.WARNING] U moet de meest recente aanbevolen Azure AD verbinding maken met het oude wachtwoordreferenties (vereist voor NTLM en Kerberos-verificatie) inschakelen om te synchroniseren met uw huurder Azure AD-versie installeren. Deze functionaliteit is niet beschikbaar in eerdere versies van Azure AD verbinden of met het oudere DirSync gereedschap.

Installatie-instructies voor de Azure AD Connect zijn beschikbaar in het volgende artikel - [aan de slag met Azure AD verbinden](../active-directory/active-directory-aadconnect.md)


### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Synchronisatie van NTLM en Kerberos-referenties-hashes naar Azure AD
De volgende PowerShell script uitvoeren op elke AD forest, volledige afdwingen en inschakelen van alle op het bedrijf gebruikers referenties hashes wilt synchroniseren met uw huurder Azure AD. Met dit script kunt de referentie-hashes voor NTLM of Kerberos-verificatie moeten worden gesynchroniseerd naar uw huurder Azure AD vereist.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Afhankelijk van de grootte van de map (van gebruikers, groepen, enz.), synchronisatie van hashes referenties naar AD Azure duurt. De wachtwoorden, worden gebruikt voor het AD-Domain Azure Services beheerde domein kort nadat de referentie-hashes hebt gesynchroniseerd naar Azure AD.


<br>

## <a name="related-content"></a>Verwante inhoud

- [Inschakelen van Wachtwoordsynchronisatie AAD Domain Services voor een Azure cloud alleen-lezen AD-directory](active-directory-ds-getting-started-password-sync.md)

- [Een beheerde Azure AD Domain Services-domein beheren](active-directory-ds-admin-guide-administer-domain.md)

- [Een virtuele Windows-computer aan een beheerde Azure AD Domain Services-domein toevoegen](active-directory-ds-admin-guide-join-windows-vm.md)

- [Een Red Hat Enterprise Linux virtuele machine aan een beheerde Azure AD Domain Services-domein toevoegen](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
