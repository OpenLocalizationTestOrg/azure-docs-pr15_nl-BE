<properties
    pageTitle="Azure API beheer Veelgestelde vragen | Microsoft Azure"
    description="Lees de antwoorden op veelgestelde vragen, patronen en aanbevolen procedures in Azure API beheer."
    services="api-management"
    documentationCenter=""
    authors="miaojiang"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="mijiang"/>

# <a name="azure-api-management-faqs"></a>Azure API beheer Veelgestelde vragen

De antwoorden op veelgestelde vragen, patronen en aanbevolen procedures voor het beheer van Azure API.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

-   [Hoe kan ik het managementteam Microsoft Azure API een vraag?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)
-   [Wat betekent het wanneer een functie in het voorbeeld wordt?](#what-does-it-mean-when-a-feature-is-in-preview)
-   [Hoe kan ik de verbinding tussen de gateway API Management en Mijn back-end services beveiligen?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
-   [Hoe kopieer ik mijn API Management service-exemplaar naar een nieuw exemplaar?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
-   [Kan ik mijn exemplaar Management API programmatisch beheren?](#can-i-manage-my-api-management-instance-programmatically)
-   [Hoe voeg ik een gebruiker aan de groep Administrators](#how-do-i-add-a-user-to-the-administrators-group)
-   [Waarom is het beleid dat ik wil toevoegen niet beschikbaar in de Groepsbeleid-editor?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
-   [Hoe gebruik API versiebeheer in de beheer-API?](#how-do-i-use-api-versioning-in-api-management)
-   [Hoe stel ik meerdere omgevingen in één API](#how-do-i-set-up-multiple-environments-in-a-single-api)
-   [Kan ik SOAP met de beheer-API gebruiken?](#can-i-use-soap-with-api-management)
-   [De constante API Management gateway-IP-adres is? Kan ik deze in de firewall-regels gebruiken?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-   [Kan ik een vergunning OAuth 2.0 server met AD FS-beveiliging configureren?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-   [Welke methode gebruikt de beheer-API in implementaties voor meerdere geografische locaties?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
-   [Kan ik een sjabloon Azure Resource Manager gebruiken voor het maken van een exemplaar van de service Management API?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
-   [Kan ik een zelfondertekend certificaat voor SSL gebruiken voor back-end](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
-   [Waarom krijg ik een verificatiefout wanneer ik probeer een GIT repository klonen?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
-   [Werkt de beheer-API met Azure ExpressRoute?](#does-api-management-work-with-azure-expressroute)
-   [Kan ik een API-Management-service van een abonnement naar de andere verplaatsen?](#can-i-move-an-api-management-service-from-one-subscription-to-another)


### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Hoe kan ik het managementteam Microsoft Azure API een vraag?

U kunt contact met ons opnemen via een van de volgende opties:

-   Vragen in onze [API beheer MSDN forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt)posten.
-   Een e-mail sturen naar <apimgmt@microsoft.com>.
-   Stuur ons een aanvraag voor een functie in het [Feedbackforum Azure](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Wat betekent het wanneer een functie in het voorbeeld wordt?

Als voorbeeld een functie is, betekent dit dat we zijn actief op zoek naar feedback over hoe de functie voor u werkt. Een functie in het voorbeeld is functioneel voltooid, maar het is mogelijk dat maken we een recente wijziging in reactie op feedback van klanten. Wij raden aan dat u niet afhankelijk van een functie die in het voorbeeld in de productieomgeving. Als u alle feedback op voorbeeld functies, laat het ons weten via een van de opties voor contactpersonen in [hoe kan ik vraag het managementteam van Microsoft Azure API?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>Hoe kan ik de verbinding tussen de gateway API Management en Mijn back-end services beveiligen?

U hebt verschillende opties voor het beveiligen van de verbinding tussen de gateway API beheer en uw back-end services. U kunt:

-   HTTP-basisverificatie gebruiken. Zie [Instellingen voor API configureren](api-management-howto-create-apis.md#configure-api-settings)voor meer informatie.
- Wederzijdse verificatie SSL gebruiken zoals beschreven in [het beveiligen van back-end services met behulp van verificatie van clientcertificaten in Azure API beheer](api-management-howto-mutual-certificates.md).
- Gebruik IP-whitelisting op uw back-end-service. Als u een exemplaar Standard of Premium laag API beheer, blijft het IP-adres van de gateway constant. U kunt de "witte" lijst zodat dit IP-adres instellen. Het IP-adres van uw exemplaar van de beheer-API kunt u krijgen op het Dashboard in Azure portal.
- De beheer-API-instantie verbinden met een virtueel netwerk Azure. Zie [VPN-verbindingen in Azure API beheer instellen](api-management-howto-setup-vpn.md)voor meer informatie.

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Hoe kopieer ik mijn API Management service-exemplaar naar een nieuw exemplaar?

U hebt verschillende opties als u een exemplaar van de beheer-API te kopiëren naar een nieuw exemplaar wilt maken. U kunt:

-   Met de back-up en herstel van de functie in de beheer-API. Zie [Noodherstel met back-up service implementeren en herstellen in Azure API-beheer](api-management-howto-disaster-recovery-backup-restore.md)voor meer informatie.
-   Uw eigen back-up maken en terugzetten functie met behulp van de [API beheer REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx). Gebruik de API van de REST op te slaan en de diensten van het service-exemplaar dat u wilt terugzetten.
-   Configuratie van de service met behulp van Git downloaden en vervolgens uploaden naar een nieuw exemplaar. Zie [Opslaan en de configuratie van de service Management API met behulp van Git configureren](api-management-configuration-repository-git.md)voor meer informatie.

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Kan ik mijn exemplaar Management API programmatisch beheren?

Ja, kunt u de API-beheer via programmacode beheren met behulp van:

-   Het [Management API REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx).
-   [ApiManagement Service Management Library SDK van Microsoft Azure](http://aka.ms/apimsdk).
-   De [implementatie van Service](https://msdn.microsoft.com/library/mt619282.aspx) en [Service management](https://msdn.microsoft.com/library/mt613507.aspx) PowerShell-cmdlets.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Hoe voeg ik een gebruiker aan de groep Administrators

Hier ziet u hoe u een gebruiker kunt toevoegen aan de groep Administrators:

1. Log in om de [Azure portal](https://portal.azure.com).
2. Ga naar de bronnengroep met de beheer-API-exemplaar dat u wilt bijwerken.
3. API in beheer van de rol van **Inzender voor Api beheer** te wijzen aan de gebruiker.

Nu kunt de nieuw toegevoegde Inzender Azure PowerShell- [cmdlets](https://msdn.microsoft.com/library/mt613507.aspx)gebruiken. Hier ziet u hoe aan te melden als beheerder:

1. Gebruik de `Login-AzureRmAccount` cmdlet aan te melden.
2. Stel de context aan het abonnement waarvoor u de service met behulp van `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Een URL voor één ophalen met behulp van `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. De URL voor toegang tot de admin portal gebruiken.


### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Waarom is het beleid dat ik wil toevoegen niet beschikbaar in de Groepsbeleid-editor?

Als het beleid dat u wilt toevoegen wordt weergegeven, grijs of grijs weergegeven in de Groepsbeleid-editor, zorg ervoor dat u in het juiste bereik voor het beleid. Elke beleidsverklaring is ontworpen voor gebruik in specifieke scopes en beleid. De beleid-secties en bereiken voor een beleid, Zie sectie voor het gebruik in [API beleid](https://msdn.microsoft.com/library/azure/dn894080.aspx)van het beleid.


### <a name="how-do-i-use-api-versioning-in-api-management"></a>Hoe gebruik API versiebeheer in de beheer-API?

U hebt een paar opties API versiebeheer gebruiken in de beheer-API:

-   U kunt beheer-API API's voor verschillende versies. Zo mogelijk twee verschillende API's, MyAPIv1 en MyAPIv2. Een ontwikkelaar kan kiezen de versie die door de ontwikkelaar wil gebruiken.
-   Ook kunt u de API met een URL die geen versiesegment van een, bijvoorbeeld https://my.api. Configureer vervolgens een versiesegment op van elke bewerking [URL herschrijven](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) sjabloon. U kunt bijvoorbeeld een bewerking met een /resource en een [URL herschrijven](api-management-howto-add-operations.md#rewrite-url-template) de sjabloon /v1/Resource [URL van sjabloon](api-management-howto-add-operations.md#url-template) . U kunt de waarde bij version segment afzonderlijk voor elke bewerking kunt wijzigen.
-   Als u een segment "default" versie behouden in de URL van de API's, klikt u op geselecteerde bewerkingen instellen een beleid dat gebruikmaakt van het beleid [backend-service zo instellen dat](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) het pad van de back-end-aanvraag wijzigen.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Hoe stel ik meerdere omgevingen in één API

U hebt twee opties om in te stellen meerdere omgevingen, bijvoorbeeld een testomgeving en een productie-omgeving één API. U kunt:

-   Host verschillende API's op de dezelfde huurder.
-   Host de dezelfde API's op verschillende huurders.

### <a name="can-i-use-soap-with-api-management"></a>Kan ik SOAP met de beheer-API gebruiken?

Ondersteuning voor [SOAP Pass Through-query](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) is nu beschikbaar. Beheerders kunnen importeren van de WSDL van de SOAP-service en Azure API beheer maakt een SOAP-front-end. Portal documentatie voor ontwikkelaars, test console, beleid en analytics zijn beschikbaar voor SOAP-services.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>De constante API Management gateway-IP-adres is? Kan ik deze in de firewall-regels gebruiken?

Op de onderlaag Standard en Premium is het openbare IP-adres (VIP) van de huurder Management API statisch gedurende de levensduur van de pachter, met enkele uitzonderingen. Het IP-adres wijzigingen in deze omstandigheden:

-   De service is verwijderd en opnieuw gemaakt.
-   Het serviceabonnement is onderbroken (bijvoorbeeld nonpayment) en vervolgens hersteld.
-   U toevoegen of verwijderen van Azure Virtual Network (kunt u virtueel netwerk alleen in de Premium-laag).

Voor implementaties met meerdere regio, het regionale adres verandert als de regio wordt leeggemaakt en vervolgens hersteld (u kunt implementeren in meerdere landen/regio alleen op de premie laag).

Premium-tier huurders die zijn geconfigureerd voor de implementatie van meerdere regio toegewezen één openbaar IP-adres per regio.

Op de pagina van de huurder in Azure portal kunt u uw IP-adres (of de adressen in een distributie met meerdere regio) krijgen.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Kan ik een vergunning OAuth 2.0 server met AD FS-beveiliging configureren?

Zie meer informatie over het configureren van een server 2.0 OAuth vergunning met beveiliging van Active Directory Federation Services (AD FS), [Met behulp van AD FS in Management API](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Welke methode gebruikt de beheer-API in implementaties voor meerdere geografische locaties?

Beheer-API wordt de [methode verkeer prestaties](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method) implementaties voor meerdere geografische locaties. Binnenkomend verkeer wordt doorgestuurd naar de dichtstbijzijnde API-gateway. Als er één regio off line gaat, wordt binnenkomend verkeer automatisch doorgestuurd naar de volgende dichtstbijzijnde gateway. Meer informatie over methoden voor routering in [beheer van verkeer methoden bewerkingsplan](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Kan ik een sjabloon Azure Resource Manager gebruiken voor het maken van een exemplaar van de service Management API?

Ja. Zie sjablonen QuickStart [Azure API Management-Service](http://aka.ms/apimtemplate) .

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Kan ik een zelfondertekend certificaat voor SSL gebruiken voor back-end

Ja. Hier ziet u een zelfondertekend certificaat voor Secure Sockets Layer (SSL) gebruiken voor back-end:

1. Een [back-end](https://msdn.microsoft.com/library/azure/dn935030.aspx) entiteit maken met behulp van beheer-API.
2. De eigenschap **skipCertificateChainValidation** ingesteld op **true**.
3. Als u niet langer toestaan dat niet-geverifieerde certificaten wilt, verwijdert u de back-end-entiteit of de eigenschap **skipCertificateChainValidation** ingesteld op **false**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Waarom krijg ik een verificatiefout wanneer ik probeer een Git repository klonen?

Als u de Git Referentiebeheer gebruikt of als u probeert een Git repository kopiëren met behulp van Visual Studio, die u kunt uitvoeren in een bekend probleem met het dialoogvenster Windows-referenties. In het dialoogvenster wachtwoordlengte 127 tekens beperkt en het wachtwoord voor Microsoft gegenereerd worden afgekapt. Wij werken aan het verkorten van het wachtwoord. Gebruik Git Bash om clone de Git repository op dit moment.

### <a name="does-api-management-work-with-azure-expressroute"></a>Werkt de beheer-API met Azure ExpressRoute?

Ja. Beheer-API werkt met Azure ExpressRoute.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Kan ik een API-Management-service van een abonnement naar de andere verplaatsen?

Ja. Voor meer informatie Zie [bronnen naar een nieuwe resourcegroep of abonnement verplaatsen](../resource-group-move-resources.md).
