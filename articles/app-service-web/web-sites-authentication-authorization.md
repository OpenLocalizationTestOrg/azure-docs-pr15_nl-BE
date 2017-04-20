<properties 
    pageTitle="Geverifieerd door Active Directory in uw Azure app on-premises | Microsoft Azure" 
    description="Klik hier voor informatie over de verschillende opties voor lijn-of-business toepassingen in Azure App Service voor verificatie met Active Directory voor gebouwen" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="08/31/2016" 
    ms.author="cephalin"/>

# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Geverifieerd door Active Directory in uw Azure app on-premises #

In dit artikel wordt beschreven hoe u voor verificatie met Active Directory (AD) in [Azure App Service](../app-service/app-service-value-prop-what-is.md)ruimten. Een Azure app wordt gehost in de wolk, maar er zijn manieren om te verifiëren op-premises AD-gebruikers veilig. 

## <a name="authenticate-through-azure-active-directory"></a>Verifiëren via Azure Active Directory
Een huurder Azure Active Directory kan worden directory wordt gesynchroniseerd met een ruimten op AD. Deze aanpak kan AD gebruikers vanaf internet toegang krijgen tot uw App en verifiëren met behulp van hun referenties op gebouwen. Azure App Service biedt bovendien een [turn-key oplossing voor deze methode](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Met een paar klikken van de knop kunt u voor uw app Azure-verificatie met een huurder directory worden gesynchroniseerd. Deze aanpak heeft de volgende voordelen:

-   Geen verificatiecode in uw toepassing vereist niet. Laat de App-Service kan de verificatie voor u en uw tijd besteden aan bieden functionaliteit in uw app.
-   [Azure AD Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx) maakt toegang tot Active directory-gegevens van uw app in Azure.
-   SSO biedt voor [alle toepassingen die worden ondersteund door Active Directory Azure](/marketplace/active-directory/), Office 365, Dynamics CRM Online, Microsoft Intune en duizenden wolk van niet-Microsoft-toepassingen. 
-   Azure Active Directory ondersteunt toegang op basis van rollen beheren. U kunt het patroon [Authorize(Roles="X")] met minimale wijzigingen aan uw code.

Zie informatie over het schrijven van een bedrijfs Azure app waarmee wordt geverifieerd met Active Directory Azure, [maken een bedrijfs Azure app met Azure Active Directory-verificatie](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Verifiëren via een on-premises STS
Als er een op ruimten beveiligde token service (STS) als Active Directory Federation Services (AD FS), kunt u die verificatie van uw app Azure communiceren. Deze methode werkt het beste als het bedrijfsbeleid AD-gegevens worden opgeslagen in Azure verbiedt. Echter, Let op het volgende:

-   STS-topologie moet worden geïmplementeerd op-ruimten, met kosten- en overhead.
-   Alleen beheerders van AD FS kunnen configureren [gebruikmakende partij vertrouwensrelaties en claim regels](http://technet.microsoft.com/library/dd807108.aspx), die van de ontwikkelaar opties kunnen beperken. Aan de andere kant is het mogelijk om te beheren en aanpassen van [vorderingen](http://technet.microsoft.com/library/ee913571.aspx) op basis van per toepassing.
-   Toegang tot op gebouwen AD gegevens vereist een afzonderlijke oplossing via de firewall van het bedrijf.

Zie als u wilt zien hoe u een lijn-of-business Azure app die worden geverifieerd met een STS op lokalen, [maken een bedrijfs Azure app met AD FS-verificatie](web-sites-dotnet-lob-application-adfs.md).
 
