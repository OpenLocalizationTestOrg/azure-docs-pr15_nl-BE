<properties 
   pageTitle="Een Azure Active Directory toe te voegen met behulp van Connected Services in Visual Studio | Microsoft Azure"
   description="Een Azure Active Directory toevoegen met behulp van het dialoogvenster Visual Studio verbonden Services toevoegen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="active-directory"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Een Azure Active Directory toe te voegen met behulp van Connected Services in Visual Studio 

##<a name="overview"></a>Overzicht
Azure Active Directory (AD Azure) gebruikt, kunt u Single Sign On (SSO) voor ASP.NET MVC webtoepassingen of verificatie van AD in services Web-API ondersteunen. Uw gebruikers kunnen hun accounts van Azure AD met Azure AD-verificatie verbinding maken met uw webtoepassingen gebruiken. De voordelen van Azure AD-verificatie met Web API opnemen verbeterde gegevensbeveiliging bij dat een API van een webtoepassing. Met Azure AD hoeft u niet te beheren een aparte verificatiesysteem met een eigen account- en management.

## <a name="supported-project-types"></a>Ondersteunde projecttypen

Het dialoogvenster verbonden Services kunt u verbinding maken met Azure AD in de volgende projecttypen.

- ASP.NET MVC projecten

- ASP.NET Web API-projecten


### <a name="connect-to-azure-ad-using-the-connected-services-dialog"></a>Verbinding maken met Azure AD met behulp van het dialoogvenster verbonden Services

1. Zorg ervoor dat u een Azure-account hebt. Als u niet een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefperiode](http://go.microsoft.com/fwlink/?LinkId=518146).

1. Open het snelmenu van het knooppunt met **verwijzingen** in uw project in Visual Studio en kiest u **Verbonden Services toevoegen**.
1. Selecteer **Azure AD-verificatie** en klik op **configureren**.

    ![Kies toevoegen Azure AD-verificatie](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Controleer op de eerste pagina van de **AD-verificatie configureren Azure** **configureren van eenmalige aanmelding met Azure AD**.

    Als uw project met een andere configuratie voor verificatie is geconfigureerd, de wizard wordt u gewaarschuwd dat het voortgezette vorige configuratie wordt uitgeschakeld.

    ![Azure AD configureren in de wizard](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1.  Selecteer een domein uit de vervolgkeuzelijst **domein** op de tweede pagina. De lijst met domeinen bevat alle domeinen toegankelijk door de rekeningen in het dialoogvenster Accountinstellingen. Als alternatief kunt u een domeinnaam als u niet kunt vinden die u, zoals mydomain.onmicrosoft.com zoekt. U kunt de optie voor het maken van een nieuwe Azure AD app of de instellingen van een bestaande Azure AD app gebruiken. 

    ![Azure AD configureren in de wizard](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)


1. Zorg ervoor dat **Active directory-gegevens te lezen** is ingeschakeld op de derde pagina van de wizard. De wizard wordt ingevuld in het **geheim van de Client**. 

    ![Azure AD configureren in de wizard](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Klik op de knop **Voltooien** . Het dialoogvenster wordt de vereiste configuratiecode en verwijzingen naar het project voor Azure AD-verificatie inschakelen. Het AD-domein kunt u bekijken op de [portal Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Bekijk de pagina aan de slag die wordt weergegeven in uw browser voor de ideeën over de volgende stappen en op de pagina Wat is er gebeurd om te zien hoe het project is gewijzigd. Als u wilt controleren of alles heeft gewerkt, een van de gewijzigde configuratiebestanden openen en controleer zijn de instellingen bedoeld in wat is er gebeurd er. Bijvoorbeeld, hebben de belangrijkste web.config in een ASP.NET-MVC-project deze instellingen toegevoegd:

        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## <a name="how-your-project-is-modified"></a>Hoe het project is gewijzigd

Wanneer u de wizard uitvoert, wordt Visual Studio voegt Azure AD en verwijzingen naar het project is gekoppeld. Configuratiebestanden en bestanden in uw project ook aangepast ondersteuning voor Azure advertentie toevoegen. De specifieke wijzigingen die Visual Studio is afhankelijk van het projecttype. Zie voor gedetailleerde informatie over hoe u ASP.NET MVC projecten worden gewijzigd, [Wat is er gebeurd – MVC projecten](http://go.microsoft.com/fwlink/p/?LinkID=513809). Zie [Wat is er gebeurd – API webprojecten](http://go.microsoft.com/fwlink/p/?LinkId=513810)voor projecten met een Web-API.

##<a name="next-steps"></a>Volgende stappen

Vragen stellen en Help-informatie opvragen.

 - [Forum MSDN: Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)

 - [Azure AD-documentatie](https://azure.microsoft.com/documentation/services/active-directory/)

 - [Blogbericht: Inleiding tot AD Azure](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

