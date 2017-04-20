<properties
    pageTitle="Vernieuwing van de richtlijnen voor gebruikers van Office 365 en Azure Active Directory het certificaat | Microsoft Azure"
    description="In dit artikel wordt uitgelegd voor Office 365 gebruikers het oplossen van problemen met e-mails die mee over het vernieuwen van een certificaat."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Federation-certificaten voor Office 365 en Azure Active Directory vernieuwen

##<a name="overview"></a>Overzicht

Geslaagde federatie tussen Azure Active Directory (AD Azure) en Active Directory Federation Services (AD FS), de certificaten die door AD FS-beveiligingstokens naar Azure AD ondertekenen gebruikt moeten overeenkomen met wat in Azure AD is geconfigureerd. Een probleem kan leiden tot verbroken vertrouwen. Azure AD zorgt ervoor dat deze gegevens synchroon worden gehouden bij het implementeren van AD FS en Web Application Proxy (voor toegang tot het extranet).

Dit artikel vindt u meer informatie voor het beheren van uw token handtekeningcertificaten en houd deze synchroon met Azure AD, in de volgende gevallen:

* U implementeert niet de toepassing Web Proxy en de federation-metagegevens is daarom niet beschikbaar in het extranet.
* U gebruikt de standaardconfiguratie van AD FS niet voor token-ondertekening van certificaten.
* U gebruikt een identiteitsprovider van derden.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>De standaardconfiguratie van AD FS voor token-ondertekening van certificaten

De token-ondertekening token decoderen van certificaten zijn meestal zelf-ondertekende certificaten en zijn goed voor een jaar. AD FS bevat standaard een automatische vernieuwingsproces **AutoCertificateRollover**genoemd. Als u AD FS 2.0 of hoger, bijwerken Office 365 en Azure AD automatisch uw certificaat voordat deze verloopt.

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>Vernieuwing van de melding van de Office 365 portal of een e-mailbericht

>[AZURE.NOTE] Als u een e-mailbericht of een portal melding waarin u wordt gevraagd een certificaat te vernieuwen voor Office ontvangen, Zie [beheren verandert in token handtekeningcertificaten](#managecerts) controleren als u geen actie te ondernemen. Microsoft is zich bewust van een mogelijk probleem dat tot berichten voor het vernieuwen van een certificaat wordt verzonden leiden kan, zelfs als er is geen actie vereist.

Azure AD probeert de federation-metagegevens bewaken en bijwerken van de handtekeningcertificaten die wordt aangegeven door deze metagegevens token. 30 dagen vóór het verstrijken van de handtekeningcertificaten, token controleert Azure AD of er nieuwe certificaten door de federation-metagegevens polling beschikbaar zijn.

* Als het kan met succes poll de federation-metagegevens en de nieuwe certificaten ophalen, wordt geen e-mailbericht of een waarschuwing in de Office 365 portal verleend aan de gebruiker.
* Als het token van de nieuwe certificaten te ondertekenen kan niet ophalen, verzendt hetzij omdat de metagegevens van de Federatie niet bereikbaar is of automatische certificaataanvragen rollover niet is ingeschakeld, Azure AD een e-mailbericht en een waarschuwing in de Office 365 portal.

![Office 365 portal melding](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] Als u AD FS, zodat de bedrijfscontinuïteit, Controleer of uw servers de volgende updates zodat verificatiefouten voor bekende problemen zich niet voor. Dit vermindert het probleem van bekende problemen met AD FS proxy server voor deze vernieuwing en de van de toekomstige vernieuwingsperioden:
>
>Server 2012 R2 - [Windows Server mei 2014 updatepakket](http://support.microsoft.com/kb/2955164)
>
>Server 2008 R2 en 2012 - [verificatie via proxy mislukt in Windows Server 2012 of Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446)

## Controleer als de certificaten moeten worden bijgewerkt<a name="managecerts"></a>

### <a name="step-1-check-the-autocertificaterollover-state"></a>Stap 1: Controleer de status van AutoCertificateRollover

Open PowerShell op de AD FS-server. Controleer of de waarde AutoCertificateRollover is ingesteld op True.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] Als u AD FS 2.0 gebruikt, voert u eerst toevoegen Pssnapin Microsoft.Adfs.Powershell.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Stap 2: Controleer of AD FS en Azure AD synchroon zijn

Op de AD FS-server de Azure AD PowerShell-prompt te openen en verbinding maken met Azure AD.

>[AZURE.NOTE] U kunt de Azure AD PowerShell downloaden [hier](https://technet.microsoft.com/library/jj151815.aspx).

    Connect-MsolService

Controleer de certificaten die zijn geconfigureerd in AD FS en Azure AD eigenschappen voor het opgegeven domein vertrouwt.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Als de vingerafdrukken in de beide uitgangen overeenkomen, worden de certificaten synchroon met Azure AD.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>Stap 3: Controleren of het certificaat verloopt

Controleer in de uitvoer van de Get-MsolFederationProperty of Get-AdfsCertificate, de datum onder "Niet na." Als de datum waarop minder dan 30 dagen afwezig is, moet u actie ondernemen.

| AutoCertificateRollover | Synchroon met Azure AD certificaten | Federation-metagegevens is toegankelijk voor het publiek | Geldigheid | Actie |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| Ja | Ja | Ja | - | Geen actie nodig. Zie [handtekeningcertificaat automatisch vernieuwen-token](#autorenew). |
| Ja | Nee  | - | Minder dan 15 dagen | Onmiddellijk vernieuwen. Zie [certificaat handmatig vernieuwen-token](#manualrenew). |
| Nee | - | - | Minder dan 30 dagen | Onmiddellijk vernieuwen. Zie [certificaat handmatig vernieuwen-token](#manualrenew). |

\[-] Maakt niet uit.

## De token handtekeningcertificaat automatisch vernieuwen (aanbevolen)<a name="autorenew"></a>

U hoeft niet alle handmatige stappen moet uitvoeren als de volgende voorwaarden voldaan wordt:
- U hebt geïmplementeerd Web Application Proxy die toegang in de metagegevens van de Federatie van het extranet inschakelen kunt.
- U gebruikt de standaardconfiguratie van AD FS (AutoCertificateRollover is ingeschakeld).

Controleer het volgende om te bevestigen dat het certificaat automatisch kan worden bijgewerkt.

**1. de AD FS-eigenschap AutoCertificateRollover moet worden ingesteld op True.** Dit betekent dat nieuwe tokenhandtekeningen en certificaten voor token decodering wordt automatisch genereren van AD FS, voordat de oude zijn verlopen.

**2. de AD FS-federation-metagegevens is toegankelijk voor het publiek.** Controleer of de federation-metagegevens openbaar toegankelijk is door te gaan naar de volgende URL van een computer op het openbare internet (buiten het bedrijfsnetwerk):


/federationmetadata/2007-06/federationmetadata.xml https:// (your_FS_name)

waar `(your_FS_name) `wordt vervangen door de federation-service host-naam uw organisatie, zoals fs.contoso.com gebruikt.  Als u kunt controleren of beide instellingen is er geen te doen.  

Voorbeeld: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Het token is geverifieerd certificaat handmatig vernieuwen<a name="manualrenew"></a>

U kunt de token handtekeningcertificaten handmatig vernieuwen. Bijvoorbeeld kunnen de volgende scenario's werken beter voor het handmatig vernieuwen:
* Token handtekeningcertificaten zijn geen zelfondertekende certificaten. De meest voorkomende reden hiervoor is dat uw organisatie certificaten van een certificeringsinstantie organisatie AD FS beheert.
* Netwerkbeveiliging is niet toegestaan de federation-metagegevens zijn openbaar.

In deze scenario's, telkens wanneer u de token handtekeningcertificaten, werken moet u ook bijwerken uw Office 365-domein met behulp van de opdracht PowerShell-Update MsolFederatedDomain.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Stap 1: Zorg ervoor dat AD FS heeft nieuwe token handtekeningcertificaten

**Niet-standaard configuratie**

Als u een niet-standaard configuratie van AD FS (waarbij de **AutoCertificateRollover** is ingesteld op **False**), gebruik u waarschijnlijk van aangepaste certificaten (geen zelf-ondertekend). Zie [richtlijnen voor klanten die niet gebruikmaken van AD FS zelfondertekend certificaten](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert)voor meer informatie over het vernieuwen van de AD FS-beveiligingstoken handtekeningcertificaten.

**Federation-metagegevens zijn niet openbaar**

Aan de andere kant als **AutoCertificateRollover** is ingesteld op **True**, maar de federation-metagegevens niet toegankelijk voor het publiek zijn, eerst voor zorgen dat nieuwe token handtekeningcertificaten zijn gegenereerd door AD FS. Bevestigen dat u inderdaad nieuwe token handtekeningcertificaten door de volgende stappen:

1. Controleer of u bent aangemeld met de primaire server voor AD FS.
2. De huidige handtekeningcertificaten in AD FS controleren door een opdrachtvenster PowerShell openen en de volgende opdracht:

    PS C:\>Get-ADFSCertificate – CertificateType token-ondertekening

    >[AZURE.NOTE] Als u AD FS 2.0 gebruikt, moet u eerst toevoegen Pssnapin Microsoft.Adfs.Powershell uitvoeren.

3. Bekijk de uitvoer van de opdracht op alle certificaten die worden vermeld. Als AD FS is een nieuw certificaat gegenereerd, moet u twee certificaten in de uitvoer zien: een waarvan de waarde **IsPrimary** is **waar** en de datum **niet na** binnen 5 dagen, en één die **IsPrimary** is **False** en **niet na** ongeveer een jaar in de toekomst.

4. Als er slechts één certificaat en de datum **niet na** binnen 5 dagen, moet u een nieuw certificaat te genereren.

5. Als u wilt een nieuw certificaat genereert, worden uitgevoerd met de volgende opdracht achter de PowerShell-opdrachtprompt: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

6. Controleren of de update opnieuw door de volgende opdracht: PS C:\>Get-ADFSCertificate – CertificateType token-ondertekening

Twee certificaten moeten nu worden weergegeven, waarvan één een datum **niet na** ongeveer één jaar in de toekomst heeft en waarvoor **IsPrimary** is **ingesteld op False**.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>Stap 2: Het nieuwe token handtekeningcertificaten voor de Office 365-vertrouwensrelatie bijwerken

Office 365 bijwerken met het nieuwe token handtekeningcertificaten moet als volgt worden gebruikt voor de vertrouwensrelatie.

1.  Open de Microsoft Azure Active Directory-Module voor Windows PowerShell.
2.  Uitvoeren van $cred = Get-Credential. Als deze cmdlet wordt u gevraagd om referenties, typt u uw referenties cloud service-account.
3.  Verbinding maken met MsolService worden uitgevoerd – referentie-$cred. Deze cmdlet wordt u verbonden met de service cloud. Voordat u een van de extra cmdlets geïnstalleerd door het hulpprogramma is voor het maken van een context die u maakt verbinding met de cloud-service vereist.
4.  Als u deze opdrachten op een computer die niet de primaire federatieserver van AD FS uitvoert, voert u Set MSOLAdfscontext-Computer <AD FS primary server>, waarbij <AD FS primary server> is de interne FQDN-naam van de primaire server voor AD FS. Deze cmdlet wordt gemaakt van een context die u maakt verbinding met AD FS.
5.  Uitvoeren van Update MSOLFederatedDomain domeinnaam: <domain>. Deze cmdlet instellingen in de cloud-service van AD FS worden bijgewerkt en configureert u de vertrouwensrelatie tussen de twee.


>[AZURE.NOTE] Als u ondersteuning voor meerdere domeinen op het hoogste niveau, zoals contoso.com en fabrikam.com, moet u de schakeloptie **SupportMultipleDomain** met alle cmdlets. Zie [ondersteuning voor meerdere domeinen van het hoogste niveau](active-directory-aadconnect-multiple-domains.md)voor meer informatie.

## Azure AD vertrouwensrelatie met Azure AD verbinding herstellen<a name="connectrenew"></a>

Als u uw AD FS-farm en Azure AD vertrouwensrelatie met Azure AD verbinding geconfigureerd, kunt u Azure AD verbinden als u niets te doen voor uw token handtekeningcertificaten moet detecteren. Als u de certificaten vernieuwen, kunt u Azure AD verbinden om dit te doen.

Zie voor meer informatie [de vertrouwensrelatie te herstellen](./active-directory-aadconnect-federation-management.md#repairing-the-trust).
