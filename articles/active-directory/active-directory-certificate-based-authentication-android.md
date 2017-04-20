<properties 
    pageTitle="Aan de slag met verificatie certificaat dat is gebaseerd op Android | Microsoft Azure" 
    description="Informatie over het configureren van verificatie op basis van certificaten in oplossingen met Android apparaten" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-android---public-preview"></a>Aan de slag met verificatie certificaat dat is gebaseerd op Android - Public Preview  

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android](active-directory-certificate-based-authentication-android.md)


In dit onderwerp wordt beschreven hoe u kunt configureren en gebruiken op basis van certificaatverificatie (CBA) op een Android apparaat voor gebruikers van huurders in Office 365 Enterprise en Business onderwijs plannen. 

CBA kunt u door Azure Active Directory worden geverifieerd met een clientcertificaat op een Android of iOS-apparaat verbinding te maken met uw Exchange online-account aan: 

- Office mobile-toepassingen zoals Microsoft Outlook en Microsoft Word   
- Exchange ActiveSync (EAS) clients 

Configuratie van deze functie niet nodig een combinatie van gebruikersnaam en wachtwoord invoeren in bepaalde e-mailberichten en Microsoft Office-toepassingen op uw mobiele apparaat. 
 

## <a name="supported-scenarios-and-requirements"></a>Ondersteunde scenario's en vereisten  



### <a name="general-requirements"></a>Algemene voorschriften 


De volgende taken zijn vereist voor alle scenario's in dit onderwerp:  

- Toegang tot authority(s) client om certificaten te verlenen certificaat.  

- De authority(s) van de certificaten moet worden geconfigureerd in Azure Active Directory. Hier vindt u gedetailleerde stappen voor het voltooien van de configuratie in de sectie [Aan de slag](#getting-started) .  

- De basiscertificeringsinstantie en tussenliggende certificeringsinstanties moeten worden geconfigureerd in Azure Active Directory.  

- Elke certificeringsinstantie moet een certificaatintrekkingslijst (CRL) kan worden verwezen via de URL van een internetverbinding hebben.  

- Dit certificaat moet worden uitgegeven voor clientverificatie.  


- Voor Exchange ActiveSync-clients alleen moet het clientcertificaat routeerbaar e-mailadres van de gebruiker in Exchange online in de Principal-naam of de naam RFC822-waarde van het veld alternatieve naam voor onderwerp. Azure Active Directory wijst de RFC822-waarde met het kenmerk Proxy-adres in de directory.  



### <a name="office-mobile-applications-support"></a>Ondersteuning voor mobiele toepassingen van Office 

| Apps                      | Ondersteuning      |
| ---                       | ---          |
| Word / Excel / PowerPoint | ![Selectievakje][1]  |
| OneNote                   | Binnenkort beschikbaar  |
| OneDrive                  | ![Selectievakje][1]  |
| Outlook                   | ![Selectievakje][1]  |
| Yammer                    | ![Selectievakje][1]  |
| Skype voor bedrijven        | ![Selectievakje][1]  |


### <a name="requirements"></a>Vereisten  

De versie van het besturingssysteem van apparaat moet Android 5.0 (Lollipop) en hoger. 

Een federation-server moet worden geconfigureerd.  


Azure Active Directory een certificaat intrekken, de AD FS-token moet beschikken over de volgende vorderingen:  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
(Het serienummer van het certificaat) 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
(De tekenreeks voor de uitgever van het clientcertificaat) 

Azure Active Directory toegevoegd deze claims op de token vernieuwen als deze beschikbaar in de AD FS-token (of andere SAML-token zijn). Wanneer het token vernieuwd worden gevalideerd moet, wordt deze informatie gebruikt de intrekkingscontrole. 

Beste, moet u het AD FS-foutpagina's bijwerken met instructies voor het verkrijgen van een gebruikerscertificaat. 

Zie [de AD FS Sign-in pagina's aanpassen](https://technet.microsoft.com/library/dn280950.aspx)voor meer informatie.  



### <a name="exchange-activesync-clients-support"></a>Exchange ActiveSync-clients ondersteunen 


Bepaalde toepassingen van Exchange ActiveSync in Android 5.0 (Lollipop) of hoger worden ondersteund. Neem contact op met de ontwikkelaar van de toepassing om te bepalen als deze functie wordt ondersteund door uw e-mailtoepassing. 



## <a name="getting-started"></a>Aan de slag 


Om te beginnen, moet u de certificeringsinstanties configureren in Azure Active Directory. Voor elke certificeringsinstantie uploaden van het volgende: 

- Het openbare gedeelte van het certificaat, in de indeling van *CER* 

- De internetverbinding URL's waarop de lijsten met ingetrokken certificaten (CRL's) staan
 

Hieronder vindt u het schema voor een certificeringsinstantie: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


Om de gegevens te uploaden, kunt u de module AD Azure via Windows PowerShell.  
Hieronder vindt u voorbeelden voor het toevoegen, verwijderen of wijzigen van een certificeringsinstantie. 



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>Verificatie op basis van de vruchtgebruiker Azure AD certificaat configureren 

1. Start Windows PowerShell met administrator-bevoegdheden. 

2. Installeer de module AD Azure. U moet installeren, versie [1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) of hoger.  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Verbinding maken met uw huurder doel: 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>Een nieuwe certificate authority toevoegt

1. Verschillende eigenschappen van de certificeringsinstantie instellen en Azure Active Directory toe te voegen: 

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. Haal de certificeringsinstanties: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="retrieving-the-list-certificate-authorities"></a>Ophalen van de lijst met certificeringsinstanties

De certificeringsinstanties die momenteel zijn opgeslagen in Azure Active Directory voor de huurder ophalen: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="removing-a-certificate-authority"></a>Verwijderen van een certificeringsinstantie

1.  De certificeringsinstanties ophalen: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Het certificaat voor de certificeringsinstantie verwijderen: 

        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiying-a-certificate-authority"></a>Modfiying, een certificate authority 

1.  De certificeringsinstanties ophalen: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Eigenschappen van de certificeringsinstantie te wijzigen: 

        $c[0].AuthorityType=1 

3. Stel de **certificeringsinstantie**: 

        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## <a name="testing-office-mobile-applications"></a>Office mobile-toepassingen testen  

Certificaatverificatie op uw mobiele Office-toepassing testen: 

1.  Installeren op uw testapparaat een mobiele Office-toepassing (bijvoorbeeld OneDrive) uit de Google Play Store.

2.  Controleer of het certificaat is ingericht op het testapparaat. 

3.  De toepassing wordt gestart. 

4.  Voer uw gebruikersnaam in en kies vervolgens het gewenste gebruikerscertificaat. 

U moet zijn aangemeld. 





## <a name="testing-exchange-activesync-client-applications"></a>Testen van clienttoepassingen Exchange ActiveSync

Voor toegang tot Exchange ActiveSync via verificatie op basis van certificaten, moet een EAS-profiel met het clientcertificaat beschikbaar zijn voor toepassing. Het EAS-profiel, moet de volgende gegevens bevatten:

- Het certificaat moet worden gebruikt voor verificatie 

- Het eindpunt van de EA's moet outlook.office365.com worden (als deze functie wordt momenteel alleen ondersteund in de Exchange online met meerdere huurder-omgeving)

Een EAS-profiel kan worden geconfigureerd en op het apparaat via het gebruik van een MDM zoals Intune of door het certificaat handmatig plaatsen in het profiel van de EA's op het apparaat geplaatst.  


### <a name="testing-eas-client-applications-on-android"></a>Testen van EAS-clienttoepassingen op Android 

Testen van verificatie met een toepassing op Android 5.0 (Lollipop) of hoger, voeren de volgende stappen uit:  

1. Een EAS-profiel configureren in de toepassing die voldoet aan bovenstaande voorwaarden.  


2. Wanneer het profiel correct is geconfigureerd, opent u de toepassing en controleer dat mail wordt gesynchroniseerd. 




## <a name="revocation"></a>Intrekking

Als u wilt een certificaat intrekken, Azure Active Directory haalt de certificaatintrekkingslijst (CRL) van de URL's geüpload als onderdeel van de informatie over certificeringsinstantie en het in de cache opgeslagen. Publiceren van de laatste tijdstempel (**Datum** eigenschap) in de CRL wordt gebruikt om ervoor te zorgen de CRL is nog steeds geldig. De CRL wordt regelmatig verwezen om in te trekken, de toegang tot de certificaten die deel van de lijst uitmaken.

Als een meer directe intrekking vereist is (bijvoorbeeld als een gebruiker een apparaat wordt verbroken), kan het token van de vergunning van de gebruiker ongeldig worden gemaakt. Als u wilt het token van de vergunning vervalt, stelt u het veld **StsRefreshTokenValidFrom** voor deze bepaalde gebruiker met Windows PowerShell. U moet het veld **StsRefreshTokenValidFrom** voor elke gebruiker die u wilt intrekken toegang voor bijwerken.
 
Om ervoor te zorgen dat de intrekking zich blijft voordoen, stelt u de **Ingangsdatum** van de CRL op een datum na de waarde ingesteld door de **StsRefreshTokenValidFrom** en ervoor te zorgen het certificaat in kwestie is in de Certificaatintrekkingslijst.
 
De volgende stappen geven een overzicht van het proces voor het bijwerken en de Autorisatietoken ongeldig kunnen worden gemaakt door het instellen van het veld **StsRefreshTokenValidFrom** . 

1. Verbinding maken met de referenties voor de service MSOL admin: 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  De huidige waarde van StsRefreshTokensValidFrom voor een gebruiker ophalen: 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  Een nieuwe waarde voor StsRefreshTokensValidFrom configureren voor de gebruiker gelijk is aan het huidige tijdstip: 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


De datum die u instelt moet in de toekomst zijn. Als de datum niet in de toekomst, wordt de eigenschap **StsRefreshTokensValidFrom** is niet ingesteld. Als de datum in de toekomst, is **StsRefreshTokensValidFrom** ingesteld op de huidige tijd (niet de datum aangegeven door de opdracht Set-MsolUser). 


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png