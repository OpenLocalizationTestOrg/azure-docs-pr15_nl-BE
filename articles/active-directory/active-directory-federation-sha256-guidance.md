<properties
    pageTitle="Wijziging handtekening hash-algoritme voor Office 365 beantwoorden partij vertrouwen | Microsoft Azure"
    description="Deze pagina bevat richtlijnen voor het wijzigen van SHA-algoritme voor de federation-vertrouwensrelatie met Office 365"
    keywords="SHA1, SHA256, O365, Federatie, aadconnect, adfs, ad fs, sha wijzigen, federation-vertrouwensrelatie vertrouwen fabrikanten"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="samueld"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="change-signature-hash-algorithm-for-office-365-replying-party-trust"></a>Hash-algoritme voor handtekening wijzigen voor Office 365 fabrikanten beantwoorden

## <a name="overview"></a>Overzicht

Azure Active Directory Federation Services (AD FS) ondertekent de tokens naar Microsoft Azure Active Directory om ervoor te zorgen dat ze niet kunnen worden gewijzigd. Deze handtekening kan worden gebaseerd op SHA1 of SHA256. Azure Active Directory ondersteunt nu ondertekend met een algoritme SHA256 tokens en wordt aangeraden als u de algoritme voor token-ondertekening op SHA256 voor het hoogste niveau van beveiliging. In dit artikel beschrijft de stappen die nodig zijn voor het algoritme voor token-ondertekening op de veiliger SHA256 niveau ingesteld.

## <a name="change-the-token-signing-algorithm"></a>Het algoritme voor token-ondertekening wijzigen

Nadat u de algoritme voor handtekening met een van de onderstaande processen hebt ingesteld, wordt in AD FS de tokens voor Office 365 SHA256 partij vertrouwensrelatie vertrouwen ondertekent. U hoeft niet extra configuratiewijzigingen aanbrengen en deze wijziging heeft geen invloed op de mogelijkheid voor toegang tot Office 365 of andere toepassingen Azure AD.

### <a name="ad-fs-management-console"></a>AD FS-beheerconsole

1. Open de beheerconsole van AD FS op de primaire server voor AD FS.
2. Vouw het knooppunt voor AD FS en op **Vertrouwen partij vertrouwt**.
3. Klik met de rechtermuisknop op uw Office 365/Azure gebruikmakende partij vertrouwen en selecteer **Eigenschappen**.
4. Selecteer het tabblad **Geavanceerd** en selecteer de secure hash-algoritme SHA256.
5. Klik op **OK**.

![SHA256 ondertekenen algoritme--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS PowerShell-cmdlets

1. Open PowerShell onder administrator-bevoegdheden op de AD FS-server.
2. Secure hash-algoritme instellen via de cmdlet **Set-AdfsRelyingPartyTrust** .

 <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Ook lezen

* [Herstellen van vertrouwen met Azure AD verbinding maken met Office 365](./active-directory-aadconnect-federation-management.md#repairing-the-trust)
