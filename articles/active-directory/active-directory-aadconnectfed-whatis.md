<properties
    pageTitle="Verbinding Azure AD en Federatie | Microsoft Azure"
    description="Deze pagina is de centrale locatie voor alle documentatie betreffende Azure AD verbinden met AD FS"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="anandy"/>


# <a name="azure-ad-connect-and-federation"></a>Azure AD verbinden en Federatie

Azure AD Connect kunt u federation configureren met de ruimten in AD FS en de AD Azure. Met het teken van de Federatie op kunt u gebruikers aan te melden op Azure AD gebaseerd services met hun wachtwoorden op gebouwen en, terwijl op het bedrijfsnetwerk, zonder hun wachtwoord opnieuw invoeren. De optie federation met AD FS kunt u implementeert een nieuwe of een bestaande AD FS in de farm Windows Server 2012 R2 opgeven.

In dit onderwerp wordt de introductiepagina voor meer informatie over Federation gerelateerde functies voor Azure AD lijsten en verbinding maken met koppelingen naar andere onderwerpen die zijn gerelateerd aan het. Azure AD verbinden, Zie voor integratie van identiteiten in ruimten met Azure Active Directory.

## <a name="azure-ad-connect---federation-topics"></a>Azure AD Connect - federation onderwerpen

| Onderwerp | Wat dekt en het lezen |
|:------|:-----------|
| **Azure verbinden met AD-in gebruikersopties** ||
| [Wat zijn gebruikers-in opties](active-directory-aadconnect-user-signin.md) | Beschrijving van diverse aanmelden gebruikersopties en invloed op Azure-in-gebruikerservaring |
| **AD FS-installatie Azure AD verbinden**||
| [Minimumvereisten](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | De vereisten voor een geslaagde installatie van AD FS via Azure AD verbinden|
| [AD FS-farm configureren](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | Het installeren van een nieuwe AD FS-farm met Azure AD verbinden |
| **De AD FS-configuratie wijzigen** | |
| [Het vertrouwen herstellen](active-directory-aadconnect-federation-management.md#reparing-the-trust) | Het repareren van huidige vertrouwensrelatie tussen op-lokalen AD FS en Office 365 / Azure |
| [Toevoegen van een nieuwe AD FS-server](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | AD FS-serverfarm met aanvullende AD FS-server na installatie uitbreiden |
| [Een nieuwe AD FS-WAP-server toevoegen](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | AD FS-farm met extra WAP-server na installatie uitbreiden |
| [Een nieuwe federatieve domein toevoegen](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | Een ander domein worden federatieve met Azure Active Directory toevoegen |
|**Installatietaken boeken**||
| [Toevoegen van aangepaste bedrijfslogo / illustratie](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| Het zich wijzigen door op te geven van het aangepaste logo dat wordt weergegeven op de aanmeldingspagina van AD FS |
| [-In de beschrijving toevoegen](active-directory-aadconnect-federation-management.md#add-sign-in-description) | Beschrijving-in op de AD FS-in pagina wijzigen | 
| [Wijzigen van AD FS claimen regels](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | Wijzigen / regels claim in AD FS overeenkomt met Azure AD verbinden sync configuratie toevoegen |


## <a name="additional-resources"></a>Aanvullende bronnen

* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
* [AD FS-implementatie in Azure](active-directory-aadconnect-azure-adfs.md)
* [Hoge beschikbaarheid cross-geografische AD FS-implementatie in Azure Azure verkeer Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)


