<properties
    pageTitle="Conceptueel overzicht van aangepaste domeinnamen in Azure Active Directory | Microsoft Azure"
    description="Het conceptueel kader voor het gebruik van aangepaste domeinnamen in Azure Active directory, met inbegrip van de Federatie voor eenmalige aanmelding wordt uitgelegd"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Conceptueel overzicht van aangepaste domeinnamen in Azure Active Directory

Een domeinnaam is een belangrijk onderdeel van de vele bronnen van directory-ID: deze deel uitmaakt van een gebruikersnaam of e-mailadres voor een deel van het adres van een groep, gebruiker en kan deel uitmaken van de app-ID-URI voor een toepassing. Een resource in Azure Active Directory (AD Azure) kan een domeinnaam die reeds is gecontroleerd en het eigendom zijn van de map waarin u de bron bevatten. Alleen de beheerder van een globale kunt domein beheertaken uitvoeren in Azure AD.

Domeinnamen in Azure AD zijn uniek. Een domeinnaam kan worden gebruikt door een enkele Azure AD. Als een bepaalde map Azure AD heeft een domeinnaam gecontroleerd, kunt vervolgens geen andere Azure AD-map controleren of die dezelfde domeinnaam gebruiken.

## <a name="initial-and-custom-domain-names"></a>Oorspronkelijke en aangepaste domeinnamen

Elke domeinnaam in Azure AD is een eerste domeinnaam of de naam van een aangepaste domein.

Elke AD Azure is voorzien van een eerste domeinnaam in het formulier contoso.onmicrosoft.com. Deze derde niveau domeinnaam, in dit voorbeeld is 'contoso.onmicrosoft.com', werd vastgesteld als de map is gemaakt, meestal door de beheerder die de map gemaakt. De eerste naam van een map kan niet worden gewijzigd of verwijderd. De eerste domeinnaam, is terwijl volledig functioneel voornamelijk bedoeld voor als een bootstrapping mechanisme worden gebruikt totdat de naam van een aangepaste domein is geverifieerd.

Een directory is ten minste één gecontroleerde aangepaste domein, zoals 'contoso.com', in de meeste productieomgevingen en is dat aangepaste domein die zichtbaar is voor eindgebruikers. Een aangepaste domeinnaam is een domeinnaam die eigendom is van en wordt gebruikt door deze organisatie, zoals 'contoso.com', voor gebruik zoals het hosten van de website. Deze domeinnaam is bekend aan werknemers omdat deze deel uitmaakt van de naam van de gebruiker die ze gebruiken voor aanmelding bij het bedrijfsnetwerk of verzenden en ophalen van e-mail.

Voordat deze kan worden gebruikt door AD Azure, moet u de naam van het aangepaste domein toevoegt aan de directory en gecontroleerd.

## <a name="verified-and-unverified-domain-names"></a>Geverifieerde en niet-geverifieerde domeinnamen

De eerste naam van een map wordt impliciet als geverifieerd door AD Azure geëvalueerd. Wanneer een beheerder een aangepaste domeinnaam aan een advertentie Azure voegt, is in eerste instantie in een niet-geverifieerde staat. Azure AD, kan geen directory-bronnen een niet-geverifieerde domeinnaam. Dit zorgt ervoor dat slechts één map de naam van een bepaald domein gebruiken kunt en dat de organisatie worden gebruikt de domeinnaam daadwerkelijk eigenaar is van die domeinnaam.

Azure AD wordt gecontroleerd of de eigenaar van een domeinnaam door te zoeken naar een bepaalde post in het zonebestand domain name service (DNS) voor de domeinnaam. Om te controleren of de eigenaar van een domeinnaam, wordt een beheerder de DNS-vermelding in Azure AD Azure AD zoekt en wordt die vermelding toegevoegd aan het bestand van de DNS-zone voor de domeinnaam. Het DNS-zonebestand wordt onderhouden door het domeinnamenregister voor dat domein. De stappen om te controleren of een domein worden weergegeven in het artikel voor het [toevoegen van een aangepaste domein naar de map Azure AD](active-directory-add-domain.md).

Een DNS-vermelding toe te voegen aan het zonebestand voor de naam van het domein heeft geen invloed op andere domeinservices zoals e-mail of webhosting.

## <a name="federated-and-managed-domain-names"></a>Federatieve en beheerde domeinnamen

Een aangepaste domeinnaam in Azure AD kan worden geconfigureerd zodat gebruikers op een bord federatieve in beleving tussen uw Active Directory op gebouwen en Azure AD. Configureren van een domein voor federation vereist-updates tot beschermde bronnen in Azure AD en aan Windows Server Active Directory. Configureren van die een federatieve domein Azure AD Connect moet worden voltooid of met PowerShell. Federeren van een aangepaste domein kan niet worden gestart vanuit de klassieke Azure portal. [Bekijk deze video voor meer informatie over het configureren van AD FS voor gebruiker meld u aan met Azure AD verbinden](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Domeinen die niet zijn federatieve worden soms beheerde domeinen genoemd. Het eerste domein voor een map Azure AD wordt impliciet geëvalueerd als een beheerde domein.

## <a name="primary-domain-names"></a>Primaire domeinnamen

De primaire domeinnaam voor een map is de naam van het domein die vooraf is geselecteerd als de standaardwaarde voor het 'domein' gedeelte van de naam van de gebruiker wanneer een beheerder een nieuwe gebruiker in de [Azure klassieke portal](https://manage.windowsazure.com/) of een andere portal zoals de admin Office 365 portal maakt. Een map kan slechts één primaire domeinnaam hebben. De primaire domeinnaam voor een aangepaste domein dat niet wordt federatieve, gecontroleerd worden door een beheerder kan wijzigen of het oorspronkelijke domein.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Domeinnamen in Azure AD en andere Microsoft Online Services

Een domeinnaam moet worden geverifieerd in Azure AD voordat deze kan worden gebruikt door een andere Microsoft Online Service als Exchange Online, SharePoint Online en Intune. De volgende services vereisen doorgaans een beheerder een of meer DNS-vermeldingen die specifiek voor de service zijn toevoegen.

Een Azure web app gebruikt een eigen mechanisme om te controleren of de eigendom van een domein. Een domein moet worden geverifieerd voor gebruik met Azure Active Directory, zelfs als deze is eerder gecontroleerd voor gebruik door een Azure web app in een abonnement dat gebaseerd op die Azure AD is. Een Azure web app kunt gebruiken een domeinnaam die is vastgesteld in een andere map in de map die de web app beveiligt.

## <a name="managing-domain-names"></a>Beheer van domeinnamen

Domein beheertaken kunnen worden uitgevoerd van de klassieke Azure portal en van PowerShell. Veel taken kunnen worden uitgevoerd met de Azure AD Graph API (public preview).

-   [Toe te voegen en een aangepaste domeinnaam controleren](active-directory-add-domain.md)

-   [Domeinen in de klassieke Azure portal beheren](active-directory-add-manage-domain-names.md)

-   [PowerShell gebruiken voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Azure AD Graph API gebruiken voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)
