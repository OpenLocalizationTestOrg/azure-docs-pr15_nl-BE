<properties 
    pageTitle="Foutopsporing uitvoeren op basis van SAML eenmalige aanmelding voor toepassingen in Azure Active Directory | Microsoft Azure" 
    description="Meer informatie over foutopsporing uitvoeren op basis van SAML eenmalige aanmelding voor toepassingen in Azure Active Directory " 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Foutopsporing uitvoeren op basis van SAML eenmalige aanmelding voor toepassingen in Azure Active Directory

Bij het opsporen van fouten in een toepassing op basis van SAML-integratie, is het vaak handig om een hulpprogramma als [Fiddler](http://www.telerik.com/fiddler) via de SAML-aanvraag, de SAML-reactie en het werkelijke SAML-token die wordt uitgegeven aan de toepassing. Door het onderzoeken van het SAML-token, kunt u ervoor zorgen dat alle vereiste kenmerken, de gebruikersnaam in de SAML-onderwerp en de uitgever van de URI komen uit zoals verwacht.

![][1]

De reactie van Azure AD met het SAML-token is meestal degene die deze gebeurtenis vindt plaats nadat een HTTP 302 redirect vanaf https://login.windows.net en wordt verzonden naar de geconfigureerde **Antwoord URL** van de toepassing. 
 
U kunt het SAML-token weergeven door deze regel te selecteren en vervolgens te klikken op de **inspecteurs > webformulieren** tabblad in het rechterdeelvenster. Van daaruit met de rechtermuisknop op de waarde **SAMLResponse** en selecteer **verzenden naar TextWizard**. Selecteer **Base64 uit** in het menu **Transformeren** voor het decoderen van het token en de inhoud ervan te zien.
 
**Opmerking**: overzicht van de inhoud van dit HTTP-verzoek Fiddler mogelijk gevraagd of u wilt configureren, het decoderen van HTTPS-verkeer, moet u doen.

## <a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Eenmalige aanmelding voor toepassingen die niet in de galerie met Azure Active Directory-toepassing configureren](active-directory-saas-custom-apps.md)
- [Claims die worden uitgegeven in het SAML-Token voor vooraf geïntegreerde toepassingen aanpassen](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png
