

<properties
   pageTitle="Controle-overzicht voor een toepassingsgateway Azure gezondheid | Microsoft Azure"
   description="Meer informatie over de bewakingsfuncties en-mogelijkheden in Azure Application Gateway"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="application-gateway-health-monitoring-overview"></a>Application Gateway gezondheid controleren-overzicht

Azure toepassingsgateway standaard wordt de status van alle bronnen in de groep met back-end en elke bron beschouwd wat de netwerkverbindingsmogelijkheden ervan uit de groep automatisch verwijderd. Toepassingsgateway blijft volgen de beschadigde exemplaren en terug naar de gezonde back-end-toepassingen toegevoegd zodra ze beschikbaar komen en te op gezondheid sondes reageren. Toepassingsgateway stuurt dat de gezondheid sondes met dezelfde poort die is gedefinieerd in de backend-HTTP-instellingen. Dit zorgt ervoor dat de sonde testen van de poort die klanten zou worden met behulp van verbinding met de back-end.

![Voorbeeld van de toepassing gateway sonde][1]

U kunt ook de sonde gezondheid aanpassen aan de vereisten van uw toepassing gebruiken standaard sonde statuscontrole, aanpassen. In dit artikel, zowel standaard als aangepaste gezondheid sondes vallen.

## <a name="default-health-probe"></a>Standaard gezondheid sonde

Een standaard gezondheid sonde een toepassingsgateway automatisch geconfigureerd wanneer u een aangepaste sonde configuratie niet instellen. Het gedrag van de controle werkt door middel van een HTTP-aanvraag naar de IP-adressen geconfigureerd voor de groep back-end. Voor standaard sondes als de backend HTTP-instellingen zijn geconfigureerd voor HTTPS, de sonde gebruikt https en gezondheid van de backends te testen.

Bijvoorbeeld: bij het configureren van uw toepassingsgateway voor het gebruik van back-endservers A, B en C voor het ontvangen van HTTP-verkeer op poort 80. Statuscontrole standaard test drie servers elke 30 seconden een gezonde HTTP-reactie. Een gezonde HTTP-antwoord heeft een [statuscode](https://msdn.microsoft.com/library/aa287675.aspx) tussen 200 en 399.

Als het selectievakje standaard sonde om server A mislukt, de toepassingsgateway verwijderd uit de groep met back-end en netwerkverkeer stopt die naar deze server. Toch blijft de sonde standaard controleren op server elke 30 seconden een. Wanneer server A is op een aanvraag van een standaard gezondheid sonde reageert, het terug zo gezond is toegevoegd aan de groep back-end en verkeer begint opnieuw overlopen naar de server.

### <a name="default-health-probe-settings"></a>Standaardinstellingen voor gezondheid sonde

|Sonde, eigenschap | Waarde | Beschrijving|
|---|---|---|
| URL van de sonde| http://127.0.0.1:\<poort\>/ | URL-pad |
| Interval | 30 | Sonde-interval in seconden |
| Time-out  | 30 | Sonde time-out in seconden |
| Beschadigde drempel | 3 | Aantal nieuwe pogingen Probe. De back-end-server wordt gemarkeerd af nadat u het aantal opeenvolgende sonde fouten de beschadigde drempel bereikt. |

> [AZURE.NOTE] De poort is de poort van de backend-HTTP-instellingen.

De sonde standaard kijkt alleen http://127.0.0.1:\<poort\> gezondheidstoestand te bepalen. Als u nodig hebt voor het configureren van de sonde gezondheid om te gaan met een aangepaste URL of andere instellingen te wijzigen, moet u aangepaste sondes gebruiken zoals beschreven in de volgende stappen uit.

## <a name="custom-health-probe"></a>Aangepaste gezondheid sonde

Aangepaste sondes kunnen u een meer gedetailleerde controle hebt over het gezondheidstoezicht op de. Als u aangepaste sondes gebruikt, kunt u het interval van de sonde, de URL en pad voor het testen en hoeveel mislukte antwoorden te accepteren voordat het exemplaar van de back-end-toepassingen als beschadigd gemarkeerd.

### <a name="custom-health-probe-settings"></a>Instellingen voor aangepaste gezondheid sonde

De volgende tabel bevat definities van de eigenschappen van een sonde met aangepaste gezondheid.

|Sonde, eigenschap| Beschrijving|
|---|---|
| Naam | De naam van de sonde. Deze naam wordt gebruikt om te verwijzen naar de sonde in de backend-HTTP-instellingen. |
| Protocol | Protocol dat wordt gebruikt voor het verzenden van de sonde. De sonde wordt gedefinieerd in de instellingen van de back-end-HTTP-protocol gebruiken |
| Host |  De hostnaam voor het verzenden van de sonde. Van toepassing alleen als er meerdere site is geconfigureerd op een toepassingsgateway, anders gebruiken '127.0.0.1'. Dit verschilt van de hostnaam VM. |
| Pad | Het relatieve pad van de sonde. Het geldige pad begint met '/'. |
| Interval | Probe interval in seconden. Dit is het interval tussen twee opeenvolgende sondes.|
| Time-out | Probe time-out in seconden. De sonde wordt gemarkeerd als mislukt als binnen deze time-outperiode geen geldig antwoord wordt ontvangen. |
| Beschadigde drempel | Aantal nieuwe pogingen Probe. De back-end-server wordt gemarkeerd af nadat u het aantal opeenvolgende sonde fouten de beschadigde drempel bereikt. |

> [AZURE.IMPORTANT] Als Application Gateway is geconfigureerd voor één site, standaard de Host moet naam worden opgegeven als '127.0.0.1', tenzij anders is geconfigureerd in een aangepaste sonde.
Voor verwijzing naar een aangepaste sonde wordt verzonden \<protocol\>://\<host\>:\<poort\>\<pad\>. De poort die wordt gebruikt is dezelfde poort als omschreven in de backend-HTTP-instellingen.

## <a name="next-steps"></a>Volgende stappen

Na het leren over gezondheidsmonitoring Application Gateway, kunt u een [aangepaste gezondheid sonde](application-gateway-create-probe-portal.md) in de Azure portal of een [aangepaste gezondheid sonde](application-gateway-create-probe-ps.md) met PowerShell en het implementatiemodel Azure Resource Manager configureren.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png