<properties 
    pageTitle="Een Web Application Firewall (WAF) configureren voor App-omgeving" 
    description="Informatie over het configureren van een web application firewall voor uw App-omgeving." 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="naziml"/>    

# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Een Web Application Firewall (WAF) configureren voor App-omgeving

## <a name="overview"></a>Overzicht ##
Web application firewalls zoals [Barracuda WAF voor Azure](https://www.barracuda.com/programs/azure) die beschikbaar is op de [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) kunt veilig dat uw webtoepassingen met een controle op binnenkomende Internet-verkeer blokkeren van SQL-injecties, Cross-Site Scripting, malware uploads & toepassing DDoS en andere aanvallen. Zij ook keurt de antwoorden van de back-end-webservers gegevens verlies te voorkomen (DLP). In combinatie met de isolatie en extra schalen geleverd door App serviceomgevingen, biedt dit een ideale omgeving voor business critical webtoepassingen die moeten bestand zijn tegen schadelijke aanvragen en grote hoeveelheden verkeer.

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Setup ##
Onze App-omgeving achter meerdere laden evenwicht exemplaren van Barracuda WAF voor dit document die we configureert zodat alleen verkeer van de WAF de App-omgeving bereiken kunt en deze is niet toegankelijk vanaf het DMZ. We hebben ook Azure verkeer Manager voor onze Barracuda WAF exemplaren te worden verdeeld over Azure datacenters en regio's. Een hoog niveau van de setup-diagram eruit zou zien wat wordt hieronder weergegeven.

![Architectuur][Architecture] 

> Opmerking: Met de introductie van het [ILB ondersteuning voor App-omgeving](app-service-environment-with-internal-load-balancer.md), kunt u de ASE zijn niet toegankelijk vanaf de DMZ en alleen beschikbaar zijn voor het particuliere netwerk. 

## <a name="configuring-your-app-service-environment"></a>Uw App-omgeving configureren ##
Als u wilt configureren verwijzen een App-omgeving naar [onze documentatie](app-service-web-how-to-create-an-app-service-environment.md) over dit onderwerp. Als u een App-omgeving gemaakt hebt, kunt u [Web Apps](app-service-web-overview.md), [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) en [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) in deze omgeving die alle beschermd achter de WAF wij in de volgende sectie configureren.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>De Barracuda WAF Cloud-Service configureren ##
Barracuda heeft een [gedetailleerd artikel](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) over het implementeren van de WAF op een virtuele machine in Azure. Maar omdat we redundantie wilt en geen potentieel risico voeren, u wilt implementeren van ten minste 2 WAF exemplaar VMs in dezelfde Cloud Service bij het volgen van deze instructies.

### <a name="adding-endpoints-to-cloud-service"></a>Eindpunten cloud Service toe te voegen ###
Als u eenmaal 2 of meer WAF VM exemplaren in de Cloud-Service hebt kunt u de [Azure portal](https://portal.azure.com/) toevoegen van HTTP- en HTTPS-eindpunten die door uw toepassing worden gebruikt, zoals in de onderstaande afbeelding.

![Eindpunt configureren][ConfigureEndpoint]

Als uw toepassingen gebruikmaken van andere eindpunten, moet u deze toevoegen aan deze lijst ook. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Barracuda WAF via de beheerportal configureren ###
Barracuda WAF gebruikt TCP-poort 8000 voor configuratie via de portal beheren. Omdat we er meerdere exemplaren van het VMs WAF moet u hier de stappen herhalen voor elk exemplaar van de VM. 


> Opmerking: Zodra u klaar bent met de configuratie van WAF, verwijderd het eindpunt TCP/8000 van de WAF VMs uw WAF te beveiligen.

Het eindpunt van het beheer toevoegen zoals in de onderstaande afbeelding voor het configureren van uw WAF Barracuda.

![Management eindpunt toevoegen][AddManagementEndpoint]
 
Een browser gebruiken om te bladeren naar het eindpunt van het beheer van de Cloud-Service. Als uw Service Cloud wordt test.cloudapp.net genoemd, zou u de toegang tot dit eindpunt door te surfen naar http://test.cloudapp.net:8000. U ziet nu een aanmeldingspagina hieronder graag dat u zich kunt aanmelden met behulp van referenties die u hebt opgegeven in de installatiefase WAF VM.

![Beheer Login pagina][ManagementLoginPage]

Eenmaal u login ziet u een dashboard als de in de onderstaande afbeelding Basisstatistieken over de bescherming van de WAF weergeven.

![Management Dashboard][ManagementDashboard]

Te klikken op het tabblad Services kunt u uw WAF voor deze beschermt services te configureren. U kunt [hun documentatie](https://techlib.barracuda.com/waf/getstarted1)raadplegen voor meer informatie over het configureren van uw WAF Barracuda. In het voorbeeld hieronder een Azure Web App is voor verkeer op HTTP en HTTPS geconfigureerd.

![Management Services toevoegen][ManagementAddServices]

> Opmerking: Afhankelijk van hoe uw toepassingen worden geconfigureerd en welke functies in de App-omgeving worden gebruikt, moet u doorsturen van verkeer voor TCP-poorten 80 en 443, bijvoorbeeld als u SSL IP-instelling voor een Web App. Raadpleeg [binnenkomende verkeer voor netwerkbesturing-documentatie](app-service-app-service-environment-control-inbound-traffic.md) netwerkpoorten sectie voor een lijst van netwerkpoorten die worden gebruikt in omgevingen met App-Service.

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Configureren van Microsoft Azure verkeer Manager (optioneel) ##
Als de toepassing beschikbaar in meerdere regio's, is en vervolgens te laden wilt stemmen deze achter [Azure verkeer Manager](../traffic-manager/traffic-manager-overview.md). Hiervoor kunt u een eindpunt in [Azure klassieke portal](https://manage.azure.com) met behulp van de Service Cloud-naam voor de WAF in het profiel van het beheer van verkeer zoals in de onderstaande afbeelding toevoegen. 

![Beheer van netwerkverkeer eindpunt][TrafficManagerEndpoint]

Als verificatie vereist is voor uw toepassing, ervoor te zorgen dat u een bron die de verificatie voor verkeer Manager ping voor de beschikbaarheid van uw toepassing niet nodig hebt. Zoals hieronder wordt weergegeven, kunt u de URL in de sectie configureren op de [Azure klassieke portal](https://manage.azure.com) .

![Verkeer Manager configureren][ConfigureTrafficManager]

Verder de pings Manager verkeer vanaf de WAF voor uw toepassing, moet u setup vertalingen van de Website op uw Barracuda WAF voor het doorsturen van verkeer naar uw toepassing zoals in het volgende voorbeeld.

![Website vertalingen][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Het beveiligen van verkeer naar Service App-omgeving met behulp van beveiligingsgroepen netwerk (NSG)##
Volg het [binnenkomende verkeer voor netwerkbesturing-documentatie](app-service-app-service-environment-control-inbound-traffic.md) voor meer informatie over het beperken van verkeer naar uw App-omgeving van de WAF alleen via het VIP-adres van uw Cloud-Service. Hier volgt een voorbeeld-Powershell-opdracht voor het uitvoeren van deze taak voor TCP-poort 80.


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

De SourceAddressPrefix vervangen door het virtuele IP-adres (VIP) van uw WAF Cloud-Service.

> Opmerking: De VIP van uw Cloud-Service wordt gewijzigd wanneer u verwijderen en opnieuw maken van de Cloud-Service. Zorg ervoor dat het IP-adres in het netwerk resourcegroep bijwerken zodra u dit doet. 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
