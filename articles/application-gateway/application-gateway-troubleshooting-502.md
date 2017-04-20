<properties
   pageTitle="Application Gateway ongeldige Gateway (502) fouten | Microsoft Azure"
   description="Meer informatie over het toepassing Gateway 502 fouten oplossen"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Het oplossen van fouten in een toepassingsgateway Ongeldige gateway

## <a name="overview"></a>Overzicht

Na het configureren van een toepassingsgateway Azure is een van de fouten die gebruikers kunnen ondervinden ' Serverfout: 502 - webserver ontving een ongeldige reactie die tijdelijk als gateway of proxy server ". Dit kan gebeuren door de volgende belangrijkste redenen:

- Groep met back-end Azure toepassing van de standaardgateway is niet geconfigureerd of leeg.
- Geen van de VMs of exemplaren in VM schaal ingesteld in orde zijn.
- Reageert niet op de standaard gezondheid sonde back-end-VMs of exemplaren van VM schaal ingesteld.
- Ongeldige of onjuiste configuratie van aangepaste gezondheid sondes.
- Request time-out of verbindingsproblemen met aanvragen van gebruikers.

## <a name="empty-backendaddresspool"></a>Lege BackendAddressPool

### <a name="cause"></a>Oorzaak

Als de toepassingsgateway heeft geen VMs of VM schaal instellen in de back-end-adresgroep is geconfigureerd, kan niet elk klantverzoek sturen en genereert een foutbericht Ongeldige gateway.

### <a name="solution"></a>Oplossing

Zorg ervoor dat de back-end-adresgroep niet leeg is. U kunt hiervoor ofwel via PowerShell, CLI of portal.

    Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"

De uitvoer van de voorgaande cmdlet bevatten niet-lege back-end-adresgroep. Hieronder volgt een voorbeeld waarin twee groepen worden geretourneerd die zijn geconfigureerd met een FQDN-naam of IP-adressen voor backend VMs. De inrichting staat van de BackendAddressPool moet worden 'geslaagd'.
    
    BackendAddressPoolsText: 
            [{
                "BackendAddresses": [{
                    "ipAddress": "10.0.0.10",
                    "ipAddress": "10.0.0.11"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool01",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
            }, {
                "BackendAddresses": [{
                    "Fqdn": "xyx.cloudapp.net",
                    "Fqdn": "abc.cloudapp.net"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool02",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
            }]


## <a name="unhealthy-instances-in-backendaddresspool"></a>Beschadigde exemplaren in BackendAddressPool

### <a name="cause"></a>Oorzaak

Als u alle exemplaren van de BackendAddressPool zijn beschadigd, vervolgens Application Gateway geen aan gebruikersaanvraag route naar de backend. Dit kan ook het geval zijn wanneer de back-end exemplaren zijn in orde, maar beschikt niet over de vereiste toepassing is geïmplementeerd.

### <a name="solution"></a>Oplossing

Controleer of de exemplaren in orde zijn en de toepassing correct is geconfigureerd. Controleren of de back-end-exemplaren kunnen reageren op een ping-opdracht uit een andere VM in de VNet hetzelfde zijn. Als geconfigureerd met een openbaar eindpunt, ervoor te zorgen dat de aanvraag van een browser om de webtoepassing te onderhouden is.

## <a name="problems-with-default-health-probe"></a>Problemen met standaard gezondheid sonde

### <a name="cause"></a>Oorzaak

502 fouten kunnen ook worden vaak indicatoren is de standaard gezondheid sonde niet kunnen bereiken van VMs back-end. Wanneer een toepassingsgateway exemplaar is ingericht, het automatisch geconfigureerd door een sonde standaard gezondheid aan elke BackendAddressPool met behulp van de eigenschappen van de BackendHttpSetting. Geen invoer van de gebruiker is vereist voor het instellen van deze sonde. Speciaal, wanneer een regel voor taakverdeling is geconfigureerd, wordt een koppeling gemaakt tussen een BackendHttpSetting en BackendAddressPool. Een sonde standaard is geconfigureerd voor elk van deze koppelingen en Application Gateway verbinding periodieke health selectievakje voor elke instantie in de BackendAddressPool op de poort die is opgegeven in het element BackendHttpSetting. Volgende tabel geeft een overzicht van de waarden die zijn gekoppeld aan de sonde standaard gezondheid.


|Sonde, eigenschap | Waarde | Beschrijving|
|---|---|---|
| URL van de sonde| http://127.0.0.1/ | URL-pad |
| Interval | 30 | Sonde-interval in seconden |
| Time-out  | 30 | Sonde time-out in seconden |
| Beschadigde drempel | 3 | Aantal nieuwe pogingen Probe. De back-end-server wordt gemarkeerd af nadat u het aantal opeenvolgende sonde fouten de beschadigde drempel bereikt. |

### <a name="solution"></a>Oplossing

- Zorg ervoor dat een standaard-site is geconfigureerd en naar de 127.0.0.1 luistert.
- Als BackendHttpSetting in een andere poort dan 80, moet de standaardsite worden geconfigureerd om te luisteren op die poort.
- De aanroep van http://127.0.0.1:port moet een HTTP-resultaatcode van 200 retourneren. Dit moet worden geretourneerd binnen de time-outperiode van 30 seconden.
- Zorg ervoor dat de poort geconfigureerd geopend is en dat er geen firewall-regels of Azure netwerk beveiligingsgroepen die het blokkeren van inkomend en uitgaand verkeer op de poort die is geconfigureerd.
- Als Azure klassieke VMs of Cloud-Service wordt gebruikt met de FQDN-naam of IP-openbare, ervoor te zorgen dat de bijbehorende is [eindpunt](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md) geopend.
- Als de VM is geconfigureerd via Azure Resource Manager en buiten de VNet waar Application Gateway wordt geïmplementeerd, moet [Beveiligingsgroep netwerk](../virtual-network/virtual-networks-nsg.md) worden geconfigureerd voor toegang op de gewenste poort.


## <a name="problems-with-custom-health-probe"></a>Problemen met aangepaste gezondheid sonde

### <a name="cause"></a>Oorzaak

Aangepaste gezondheid sondes kunnen extra flexibiliteit om de standaard gedrag scannen. Wanneer u aangepaste sondes, kunnen gebruikers de sonde interval, de URL en pad om te testen en hoeveel mislukte antwoorden te accepteren voordat het exemplaar van de back-end-toepassingen als beschadigd gemarkeerd. De volgende aanvullende eigenschappen worden toegevoegd.

|Sonde, eigenschap| Beschrijving|
|---|---|
| Naam | De naam van de sonde. Deze naam wordt gebruikt om te verwijzen naar de sonde in de backend-HTTP-instellingen. |
| Protocol | Protocol dat wordt gebruikt voor het verzenden van de sonde. De sonde wordt gedefinieerd in de instellingen van de back-end-HTTP-protocol gebruiken |
| Host |  De hostnaam voor het verzenden van de sonde. Alleen van toepassing wanneer meerdere site is geconfigureerd op de Gateway van de toepassing. Dit verschilt van de hostnaam VM.  |
| Pad | Het relatieve pad van de sonde. Het geldige pad begint met '/'. De sonde wordt verzonden naar \<protocol\>://\<host\>:\<poort\>\<pad\> |
| Interval | Probe interval in seconden. Dit is het interval tussen twee opeenvolgende sondes.|
| Time-out | Probe time-out in seconden. De sonde wordt gemarkeerd als mislukt als binnen deze time-outperiode geen geldig antwoord wordt ontvangen. |
| Beschadigde drempel | Aantal nieuwe pogingen Probe. De back-end-server wordt gemarkeerd af nadat u het aantal opeenvolgende sonde fouten de beschadigde drempel bereikt. |


### <a name="solution"></a>Oplossing

Valideren dat de aangepaste gezondheid Probe is geconfigureerd als de voorgaande tabel. Naast de bovenstaande stappen voor probleemoplossing worden bovendien de volgende:

- Controleer of de sonde goed is opgegeven aan de hand van de [handleiding](application-gateway-create-probe-ps.md).
- Als Application Gateway is geconfigureerd voor één site, standaard de Host moet naam worden opgegeven als '127.0.0.1', tenzij anders is geconfigureerd in een aangepaste sonde.
- Ervoor te zorgen dat een oproep naar http://\<host\>:\<poort\>\<pad\> geeft als resultaat een HTTP-resultaatcode van 200.
- Zorg ervoor dat Interval, time- en UnhealtyThreshold binnen de aanvaardbare marges.

## <a name="request-time-out"></a>Time-out voor aanvraag

### <a name="cause"></a>Oorzaak

Wanneer een aanvraag wordt ontvangen, wordt toepassingsgateway geconfigureerde regels van toepassing is op de aanvraag en stuurt het door naar een exemplaar van de back-end-toepassingen. Wacht voor een configureerbaar interval van tijd voor een reactie van het exemplaar van de back-end. Dit interval is standaard **30 seconden**. Als toepassingsgateway niet een antwoord van een back-endtoepassing in dit interval ontvangt, ziet de aanvraag van de gebruiker een fout 502.

### <a name="solution"></a>Oplossing

Application Gateway kan gebruikers deze instelling via BackendHttpSetting, die vervolgens kan worden toegepast op andere groepen van toepassingen configureren. Verschillende back-end-toepassingen kunnen hebben verschillende BackendHttpSetting en dus verschillende verzoek time-out ingesteld.

    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## <a name="next-steps"></a>Volgende stappen

Als de bovenstaande stappen het probleem niet is opgelost, opent u een [support ticket](https://azure.microsoft.com/support/options/).
