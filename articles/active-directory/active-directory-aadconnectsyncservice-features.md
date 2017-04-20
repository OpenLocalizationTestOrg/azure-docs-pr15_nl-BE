<properties
    pageTitle="Azure AD verbinden sync servicefuncties en configuratie | Microsoft Azure"
    description="Beschrijving van service naast functies voor Azure AD verbinden sync-service."
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
    ms.date="08/22/2016"
    ms.author="andkjell;markvi"/>

# <a name="azure-ad-connect-sync-service-features"></a>Azure AD verbinden sync servicefuncties

De synchronisatiefunctie van Azure AD verbinding bestaat uit twee onderdelen:

- Het onderdeel op het bedrijf met de naam **Azure AD verbinden sync**, **sync engine**ook genoemd.
- De service die woonachtig zijn in Azure AD ook bekend als de **synchronisatieservice Azure AD verbinden**

In dit onderwerp wordt uitgelegd hoe de volgende functies van de **synchronisatieservice Azure AD Connect** werken en hoe u kunt deze configureren met behulp van Windows PowerShell.

Deze instellingen worden geconfigureerd door de [Azure Active Directory Module voor Windows PowerShell](http://aka.ms/aadposh). Download en installeer het afzonderlijk Azure AD Connect. De cmdlets beschreven in dit onderwerp zijn geïntroduceerd in [maart 2016-versie (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Als u beschikt niet over de cmdlets beschreven in dit onderwerp of ze niet hetzelfde resultaat produceert, Controleer of dat u de meest recente versie uitvoert.

Als u wilt zien van de configuratie in uw directory AD Azure, `Get-MsolDirSyncFeatures`.  
![Resultaat van Get-MsolDirSyncFeatures](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Veel van deze instellingen kunnen alleen worden gewijzigd door Azure AD verbinden.

De volgende instellingen kunnen worden geconfigureerd door `Set-MsolDirSyncFeature`:

DirSyncFeature | Opmerking
--- | ---
[DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | Kunt u een kenmerk in quarantaine worden geplaatst wanneer het een duplicaat van een ander object in plaats van het gehele object mislukt tijdens het exporteren.
[EnableSoftMatchOnUpn](#userprincipalname-soft-match) | Kunnen objecten userPrincipalName naast het primaire SMTP-adres koppelen.
[SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | Hiermee kunt de synchronisatie-engine voor het bijwerken van het kenmerk userPrincipalName (niet-federatieve) gebruikers beheerde/in licentie gegeven.

Nadat u een functie hebt ingeschakeld, kunnen deze niet worden uitgeschakeld.

>[AZURE.NOTE] Vanaf 24 augustus 2016 *Dubbel kenmerk tolerantie* van de functie is standaard ingeschakeld voor nieuwe Azure AD-mappen. Deze functie wordt ook uitgerold en ingeschakeld op de mappen die worden gemaakt vóór deze datum. U ontvangt een e-mailbericht wanneer uw map ophalen van deze functie is ingeschakeld.

De volgende instellingen worden geconfigureerd door het verbinden van Azure AD en kan niet worden gewijzigd door `Set-MsolDirSyncFeature`:

DirSyncFeature | Opmerking
--- | ---
DeviceWriteback | [Azure AD verbinden: Writeback apparaat inschakelen](active-directory-aadconnect-feature-device-writeback.md)
DirectoryExtensions | [Azure AD verbinden sync: map extensies](active-directory-aadconnectsync-feature-directory-extensions.md)
PasswordSync | [Implementeren van Wachtwoordsynchronisatie met Azure verbinden met AD-synchronisatie](active-directory-aadconnectsync-implement-password-synchronization.md)
UnifiedGroupWriteback | [Voorbeeld: Groep Write-back](active-directory-aadconnect-feature-preview.md#group-writeback)
UserWriteback | Op dit moment niet ondersteund.

## <a name="duplicate-attribute-resiliency"></a>Dubbel kenmerk tolerantie
Gehouden om in te richten en niet de objecten met dubbele UPN / proxyAddresses, het gedupliceerde kenmerk "quarantaine" en een tijdelijke waarde wordt toegewezen. Wanneer het conflict is opgelost, wordt de tijdelijke UPN automatisch gewijzigd op de juiste waarde. Dit probleem kan afzonderlijk worden ingeschakeld voor UPN- en proxyAddress. Zie [identiteit synchronisatie en dubbel kenmerk tolerantie](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)voor meer informatie.

## <a name="userprincipalname-soft-match"></a>UserPrincipalName zachte match
Wanneer deze functie is ingeschakeld, is voor UPN soft-match ingeschakeld naast het [primaire SMTP-adres](https://support.microsoft.com/kb/2641663), die altijd is ingeschakeld. Soft-match wordt gebruikt om bestaande gebruikers van de wolk in Azure AD overeenkomen met gebruikers voor gebouwen.

Als u wilt afstemmen op-premises AD accounts met bestaande accounts die zijn gemaakt in de cloud en u geen Exchange Online, is deze functie is handig. In dit scenario wordt in het algemeen er geen reden is om het kenmerk SMTP instellen in de cloud.

Deze functie is op standaard voor nieuw gemaakte mappen Azure AD. U kunt zien als deze functie is ingeschakeld voor u door te voeren:  
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Als deze functie is niet ingeschakeld voor de map Azure AD, kunt vervolgens u deze inschakelen door te voeren:  
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>UserPrincipalName updates synchroniseren
In het verleden, updates van het kenmerk UserPrincipalName is via de synchronisatieservice voor op ruimten is geblokkeerd, tenzij deze voorwaarden van toepassing zijn:

- De gebruiker wordt beheerd (niet-federatieve).
- De gebruiker heeft niet een licentie is toegewezen.

Zie voor meer informatie de [gebruikersnamen in Office 365, Azure, of Intune komen niet overeen met de UPN in de lokalen of andere aanmeldings-ID](https://support.microsoft.com/kb/2523192).

Deze functie inschakelt, kunt de synchronisatie-engine de userPrincipalName bijwerken wanneer het gewijzigde op ruimten is en u wachtwoord synchronisatie gebruikt. Als u federation, is deze functie niet ondersteund.

Deze functie is op standaard voor nieuw gemaakte mappen Azure AD. U kunt zien als deze functie is ingeschakeld voor u door te voeren:  
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Als deze functie is niet ingeschakeld voor de map Azure AD, kunt vervolgens u deze inschakelen door te voeren:  
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Nadat u deze functie inschakelt, blijven bestaande userPrincipalName waarden als-is. De normale delta synchronisatie van gebruikers werkt op de volgende wijziging van de userPrincipalName kenmerk op-ruimten, de UPN.  

## <a name="see-also"></a>Zie ook

- [Azure AD verbinden sync](active-directory-aadconnectsync-whatis.md)
- [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
