<properties
    pageTitle="Azure Active Directory preview explainer | Microsoft Azure"
    description="Een onderwerp met informatie over de verschillen tussen Azure Active Directory in de klassieke portal en de voorvertoning Azure Active Directory in de portal Azure."
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
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="preview-of-the-azure-active-directory-management-experience-in-the-azure-portal"></a>Voorbeeld van de Azure Active Directory management ervaring in de portal voor Azure

De Azure Active Directory (AD Azure) management ervaring is in het voorbeeld in de portal Azure. U kunt proberen dit uit door het aanmelden bij [de Azure portal](https://portal.azure.com) als globale beheerder van de map. Vervolgens Azure Active Directory selecteert in de lijst met services als deze zichtbaar is, of selecteer **meer services** voor de lijst met alle services. U hoeft niet een Azure-abonnement te gebruiken de Azure AD management ervaring in de portal Azure.


## <a name="capabilities-of-the-preview-experience"></a>Mogelijkheden van de voorbeeld-ervaring

De ervaring van het voorbeeld kunt u veel directory-bronnen zoals gebruikers, groepen, toepassingen en instellingen, in de Azure portal beheren. Deze ervaring om op te nemen van de mogelijkheden die bestaan in de Azure verbeteren AD management ervaring in [Azure klassieke portal](https://manage.windowsazure.com). Tot die tijd zijn er enkele taken die u nog moet in de klassieke portal voltooien directory-beheer.

## <a name="manage-the-same-azure-ad-tenants"></a>De dezelfde Azure AD Pachters beheren

De ervaring van voorbeeld leest en schrijft naar de dezelfde huurder Azure Active Directory als de klassieke portal en het Office 365 Admin center. Wijzigingen in een van deze portals zijn doorgevoerd in alle andere.

## <a name="use-the-same-authorization-logic"></a>Gebruik dezelfde logica vergunning

De ervaring van het voorbeeld gebruikt dezelfde logica vergunning als bestaande Active Directory-clients. Gebruikers mogen wijzigingen aanbrengen in de map resources op basis van hun functie directory, zoals globale beheerder, Gebruikersbeheerder, administrator wachtwoord. Met een rol op Azure resources of een abonnement op Azure staat niet toe dat een gebruiker voor het beheren van de directory-bronnen. Azure AD management rollen Zie [rollen van beheerder toewijzen in Azure Active Directory](active-directory-assign-admin-roles.md)voor meer informatie. 

De ervaring van het voorbeeld is geoptimaliseerd voor globale beheerders. Als u de voorvertoning ervaring terwijl aangemeld als een gebruiker die geen beheerder van een globale, wellicht een slechtere ervaring. Bijvoorbeeld, u mogelijk op een knop waarmee u een taak die in de map kan niet worden voltooid. We zijn deze ervaring snel te verbeteren.
 
## <a name="tell-us-what-you-think"></a>Laat ons weten wat u denkt

U kunt feedback geven over de ervaring die in de admin portal-sectie van het [Feedbackforum Azure AD](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&filter=alltypes&sort=lastpostdesc)preview.
