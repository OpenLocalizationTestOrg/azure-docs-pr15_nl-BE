<properties
   pageTitle="Een aangepaste sonde voor een toepassingsgateway maken met behulp van de portal | Microsoft Azure"
   description="Informatie over het maken van een aangepaste sonde voor Gateway-toepassing met behulp van de portal"
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

# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Maak een aangepaste sonde voor Application Gateway met behulp van de portal

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-probe-portal.md)
- [Azure PowerShell voor bronbeheer](application-gateway-create-probe-ps.md)
- [Azure klassieke PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

## <a name="scenario"></a>Scenario

In het volgende scenario gaat bij het maken van een aangepaste gezondheid sonde in een bestaande toepassingsgateway.
Het scenario wordt ervan uitgegaan dat u de stappen voor het [maken van een toepassingsgateway](application-gateway-create-gateway-portal.md)al hebt gevolgd.

## <a name="createprobe"></a>De sonde maken

Sondes worden geconfigureerd in een proces via de portal. De eerste stap is het maken van de sonde, vervolgens u de sonde toevoegen aan de back-end HTTP-instellingen van de toepassingsgateway.

### <a name="step-1"></a>Stap 1

Ga naar http://portal.azure.com en selecteer een bestaande toepassingsgateway.

![Gateway: overzicht][1]

### <a name="step-2"></a>Stap 2

**Sondes** en klik op de knop **toevoegen** om een nieuwe sonde toevoegen.

![Sonde blade toevoegen met gegevens gevuld][2]

### <a name="step-3"></a>Stap 3

Vul de vereiste gegevens voor de sonde en klik op **OK**als voltooid.

- **Naam** - dit is een beschrijvende naam voor de sonde die in de portal toegankelijk is.
- **Host** - dit is de hostnaam die wordt gebruikt voor de sonde. Van toepassing alleen als er meerdere site is geconfigureerd op een toepassingsgateway, anders gebruiken '127.0.0.1'. Dit verschilt van de hostnaam VM.
- **Pad** - de rest van de volledige url voor de aangepaste sonde. Een geldig pad begint met '/'
- **Interval (sec)** - hoe vaak de sonde wordt uitgevoerd om te controleren of de gezondheid. Het is niet raadzaam het lager instellen dan 30 seconden.
- **Time-out (sec)** - de hoeveelheid tijd dat de sonde wordt gewacht voordat een time-out optreedt. De time-outinterval moet hoog genoeg dat een HTTP-oproep kan worden gemaakt om ervoor te zorgen dat de gezondheid backend pagina beschikbaar is.
- **Beschadigde drempel** - aantal mislukte pogingen worden beschouwd wat de netwerkverbindingsmogelijkheden ervan. Een drempelwaarde van 0 betekent dat als een selectievakje mislukt voor de gezondheid van de back-end wordt bepaald wat de netwerkverbindingsmogelijkheden ervan onmiddellijk.

> [AZURE.IMPORTANT] de hostnaam is niet de naam van de server. Dit is de naam van de virtuele host op de application server. De sonde wordt verzonden naar http://(host name):(port from httpsetting)/urlPath

![sonde configuratie-instellingen][3]

## <a name="add-probe-to-the-gateway"></a>Sonde toevoegen aan de gateway

Nu de sonde is gemaakt, is het tijd om toe te voegen aan de gateway. Sonde-instellingen zijn ingesteld op de backend HTTP-instellingen van de toepassingsgateway.

### <a name="step-1"></a>Stap 1

Klik op de **http-instellingen** van de toepassingsgateway en klik vervolgens op de huidige back-end HTTP-instellingen in het venster om de configuratie-blade.

![HTTPS-instellingenvenster][4]

### <a name="step-2"></a>Stap 2

Op het blad van de instellingen voor **appGatewayBackEndHttp** **Gebruik aangepaste sonde** op en kies de sonde gemaakt in de sectie [maken de sonde](#createprobe) .
Klik op **OK** als dit klaar is, en de instellingen worden toegepast.

![appgatewaybackend instellingen blade][5]

De sonde standaard de toegang tot de webtoepassing wordt gecontroleerd. Nu dat een aangepaste sonde is gemaakt, wordt de toepassingsgateway het aangepast pad dat is gedefinieerd voor het controleren van de gezondheid voor de geselecteerde back-end. De toepassingsgateway op basis van de criteria die is gedefinieerd, wordt het opgegeven bestand in de sonde gecontroleerd. Als de aanroep van host: poort / pad geeft geen resultaat van een Http 200 OK antwoord, de server van rotatie, is genomen nadat de beschadigde drempel is bereikt. Scannen van het beschadigde exemplaar om te bepalen wanneer het weer in orde wordt voortgezet. Wanneer het exemplaar terug is toegevoegd aan groep gezonde server begint verkeer stroomt het opnieuw en gezocht naar de instantie verder bij het opgegeven interval als normale gebruiker.


## <a name="next-steps"></a>Volgende stappen

Zie informatie over het configureren van SSL-Offloading met Azure toepassingsgateway [Offload van SSL configureren](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[3]: ./media/application-gateway-create-probe-portal/figure3.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png