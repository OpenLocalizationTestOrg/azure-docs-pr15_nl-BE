<properties
   pageTitle="Ondersteuning voor toepassingen Gateway WebSocket | Microsoft Azure"
   description="Deze pagina biedt een overzicht van de toepassing Gateway WebSocket ondersteuning."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-websocket-support"></a>Ondersteuning voor toepassingen Gateway WebSocket

Toepassingsgateway biedt native ondersteuning voor WebSocket in alle formaten van de gateway. Er is geen gebruiker-configureerbare instelling in-of uitschakelen WebSocket ondersteuning. U kunt doorgaan met behulp van een standaard HTTPListener op poort 80/443 WebSocket verkeer ontvangen. WebSocket verkeer wordt vervolgens gericht aan de WebSocket ingeschakelde back-end-server met behulp van de juiste back-end-toepassingen als omschreven in de regels van toepassing-gateway. WebSocket protocol gestandaardiseerd in [RFC6455](https://tools.ietf.org/html/rfc6455) kunt een full-duplex-communicatie tussen client en server via een langdurige TCP-verbinding. Met deze functie kunnen voor een meer interactieve communicatie tussen een webserver en een client die bidirectionele zonder polling als vereist in HTTP-gebaseerde implementaties kan worden.  WebSocket hebben lage overhead in tegenstelling tot HTTP en dezelfde TCP-verbinding voor meerdere/antwoorden vragen wat resulteert in een meer efficiënt gebruik van resources kunt hergebruiken. WebSocket protocollen zijn ontworpen voor gebruik via traditionele HTTP-poorten 80 en 443.

De back endserver moet reageren op application gateway sondes die worden beschreven in de sectie [overzicht van gezondheid sonde](application-gateway-probe-overview.md) . Application gateway gezondheid sondes zijn alleen HTTP/HTTPS, dit houdt in dat elke back endserver moet reageren op http-sondes voor toepassingsgateway WebSocket verkeer doorsturen naar de server.

## <a name="listener-configuration-element"></a>Listener, configuratie-element

Bestaande HTTPListener kan worden gebruikt ter ondersteuning van WebSocket. Hieronder ziet u een fragment van een HttpListeners-element van een voorbeeldbestand van de sjabloon. U moet http- en HTTPS-listeners WebSocket ondersteunen en WebSocket verkeer wordt beveiligd. Op dezelfde manier kunt u de [portal](application-gateway-create-gateway-portal.md) of [PowerShell](application-gateway-create-gateway-arm.md) een toepassingsgateway maken met listeners op poort 80/443 voor ondersteuning van WebSocket verkeer.


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool, BackendHttpSetting en routering, configuratie

BackendAddressPool moet worden gebruikt voor het definiëren van een back-end-groep met servers WebSocket ingeschakeld. BackendHttpSetting moet worden gedefinieerd met behulp van back-end poort 80/443 alleen. Eigenschappen voor affiniteit op basis van een cookie en requestTimeouts zijn niet relevant voor WebSocket verkeer. Er is geen wijziging in de routing-regel vereist. Routing-regel 'Basic' moet blijven worden gebruikt voor het koppelen van de juiste listener aan de bijbehorende backend-adresgroep. 

    "requestRoutingRules": [{
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }, {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }

        }
    }]

## <a name="websocket-enabled-backend"></a>Backend WebSocket ingeschakeld

Uw back-end moet een HTTP/HTTPS-webserver waarop de geconfigureerde poort (meestal 80/443) voor WebSocket werken. Deze eis is omdat WebSocket protocol vereist is de oorspronkelijke handshake HTTP met Upgrade WebSocket protocol als een headerveld zijn.

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

Een andere reden hiervoor is dat toepassing gateway backend gezondheid sonde alleen HTTP/HTTPS-protocollen ondersteunt. Als de back endserver niet reageert op HTTP/HTTPS-sondes, back-end-toepassingen en er geen aanvragen met inbegrip van WebSocket aanvragen zouden moeten worden genomen, zou dit back-end bereiken.

## <a name="next-steps"></a>Volgende stappen

Na het leren over de ondersteuning van WebSocket, gaat u naar [een toepassingsgateway maken](application-gateway-create-gateway.md) om aan de slag met een webtoepassing WebSocket ingeschakeld.
