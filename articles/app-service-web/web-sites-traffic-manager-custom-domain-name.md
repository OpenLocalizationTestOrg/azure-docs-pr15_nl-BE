<properties
    pageTitle="Een aangepaste domeinnaam voor een web app in Azure App-Service Manager verkeer voor taakverdeling gebruikt configureren."
    description="Gebruik een aangepaste domeinnaam voor een in Azure App Service Manager verkeer voor taakverdeling met een web-app."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Een aangepaste domeinnaam voor een web app configureren in Azure App-Service met behulp van beheer van netwerkverkeer

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Dit artikel bevat algemene instructies voor het gebruik van een aangepaste domeinnaam met Azure App-Service die verkeer Manager voor taakverdeling gebruiken.

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## <a name="understanding-dns-records"></a>Wat is DNS-records?

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## <a name="configure-your-web-apps-for-standard-mode"></a>Uw web apps voor de standaard-modus configureren

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## <a name="add-a-dns-record-for-your-custom-domain"></a>DNS-records voor uw aangepaste domein toevoegen

> [AZURE.NOTE] Als u domein via Azure App Service Web Apps hebt aangeschaft stappen overslaan en naar de laatste stap van het [Domein kopen voor Web Apps](custom-dns-web-site-buydomains-web-app.md) artikel verwijzen.

Om uw aangepaste domein koppelen aan een web app in Azure App-Service, moet u een nieuwe vermelding in de DNS-tabel voor uw aangepaste domein toevoegen met behulp van hulpprogramma's die door de domeinregistratieservice die u hebt gekocht van de domeinnaam van uw. Gebruik de volgende stappen uit om te zoeken en de DNS-hulpprogramma's gebruiken.

1. Aanmelden bij uw account op uw domeinregistratieservice en zoekt naar een pagina voor het beheren van DNS-records. Zoek naar koppelingen of gedeelten van de site aangeduid als **Domeinnaam**, **DNS**of **Name Server Management**. Vaak een link naar deze pagina vindt u uw accountgegevens bekijken en klik vervolgens op zoek naar een koppeling zoals **Mijn domeinen**.

1. Als u de beheerpagina hebt gevonden voor uw domeinnaam, zoekt u een koppeling waarmee u de DNS-records bewerken. Dit kan worden weergegeven als een **zonebestand**, **DNS-Records**, of als een koppeling **Geavanceerd** configuratie.

    * De pagina heeft waarschijnlijk een aantal records hebt gemaakt, zoals het koppelen van een vermelding '**@**'of'\*' met een 'domein parkeren' pagina. Het kan ook records voor gemeenschappelijke subdomeinen, zoals **www**bevatten.
    * De pagina **CNAME-records**worden vermeld of bieden een vervolgkeuzelijst een record te selecteren. Het kan ook andere records zoals **A-records** en **MX-records**worden vermeld. In sommige gevallen wordt door andere namen zoals een **Alias-Record**CNAME-records worden genoemd.
    * De pagina moet ook de velden waarmee u aan **toewijzing** van een **hostnaam** of **domeinnaam** aan een andere domeinnaam.

1. Terwijl de specifieke kenmerken van elke registrar variëren in het algemeen wijst u *uit* uw aangepaste domeinnaam (bijvoorbeeld **contoso.com**) *om* verkeer domeinnaam van de Manager (**contoso.trafficmanager.net**) die wordt gebruikt voor uw web app.

    > [AZURE.NOTE] Als een record al gebruikt wordt en u moet preemptively uw apps om hieraan te binden, kunt u ook een extra CNAME-record maken. Bijvoorbeeld om te preemptively **www.contoso.com** binden aan uw web app, maken een CNAME-record van **awverify.www** naar **contoso.trafficmanager.net**. Vervolgens kunt u 'www.contoso.com' toevoegen aan uw Web App zonder het 'www' CNAME-record. Zie voor meer informatie, [maken van DNS-records voor een web app in een aangepaste domein][CREATEDNS].

1. De wijzigingen opslaan als u klaar bent met het toevoegen of wijzigen van de DNS-records bij uw registratieservice.

<a name="enabledomain"></a>
## <a name="enable-traffic-manager"></a>Verkeer-beheer inschakelen

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Volgende stappen

Zie [Node.js Developer Center](/develop/nodejs/)voor meer informatie.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
