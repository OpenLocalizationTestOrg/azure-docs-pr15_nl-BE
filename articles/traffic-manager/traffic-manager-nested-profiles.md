<properties
    pageTitle="Geneste verkeer profielen | Microsoft Azure"
    description="In dit artikel wordt uitgelegd dat de functie 'Genest profielen' van Azure verkeer Manager"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="nested-traffic-manager-profiles"></a>Geneste verkeer profielen

Verkeer Manager bevat een scala aan Routering van verkeer methoden waarmee u kunt bepalen hoe verkeer Manager kiest welk endpoint verkeer moet ontvangen van elke gebruiker. Zie [Routering van verkeer verkeer Manager-methoden](traffic-manager-routing-methods.md)voor meer informatie.

Elk serviceprofiel verkeer geeft een enkele methode voor het routeren van verkeer. Er zijn echter scenario's waarvoor meer geavanceerde verkeersroutering dan het bewerkingsplan die door één profiel verkeer Manager. U kunt profielen wilt combineren de voordelen van meer dan één methode voor routering van verkeer verkeer Manager nesten. Geneste profielen kunnen u het standaardgedrag verkeer Manager met ondersteuning voor grotere en complexere implementaties van toepassing overschrijven.

De volgende voorbeelden illustreren het gebruik van geneste verkeer profielen in verschillende scenario's.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Voorbeeld 1: Combineren 'Prestaties' en 'Gewogen' verkeer routing

Stel dat u een toepassing in de volgende gebieden Azure geïmplementeerd: West VS, West-Europa en Oost-Azië. U verkeer van de Manager 'Performance'-verkeer routeren methode gebruiken voor het distribueren van verkeer naar de regio die het dichtst bij de gebruiker.

![Enkel verkeer Manager profiel][1]

Stel dat u wilt een update van uw service te testen alvorens meer algemeen. Wilt u de 'gewogen' verkeer routing-methode gebruiken om een klein percentage van het verkeer naar uw implementatie test. De implementatie testen naast de bestaande productie-implementatie instellen u in West-Europa.

Beide 'gewogen' kan niet worden gecombineerd en ' prestaties verkeer-routing in één profiel. Voor dit scenario, kunt u met behulp van de twee eindpunten van West-Europa en de methode 'Gewogen' Routering van verkeer verkeer Manager profiel maken. Vervolgens wordt toevoegen u dit profiel 'child' als een eindpunt aan de 'parent' profiel. Het profiel van de bovenliggende nog steeds wordt de prestaties routering van verkeer methode en de algemene implementaties als eindpunten bevat.

In het volgende diagram ziet u in het volgende voorbeeld:

![Geneste verkeer profielen][2]

In deze configuratie distribueert verkeer omgeleid via de bovenliggende profiel verkeer tussen de regio's normaal. In West-Europa verdeelt het geneste profiel verkeer naar de productie- en eindpunten volgens de gewichten toegewezen.

Wanneer de bovenliggende profiel de methode 'Prestaties' Routering van verkeer gebruikt, moet elk eindpunt een locatie worden toegewezen. De locatie wordt toegewezen wanneer u het eindpunt configureren. Kies de Azure regio die het dichtst bij de implementatie. De Azure gebieden zijn de waarden van de locatie wordt ondersteund door de latentie Internet tabel. Zie voor meer informatie [verkeer Manager 'Performance'-verkeer routeren methode](traffic-manager-routing-methods.md#performance-traffic-routing-method).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Voorbeeld 2: Controle van eindpunt genest profielen

Beheer van netwerkverkeer wordt actief de status van elke service-eindpunt. Als een eindpunt beschadigd is, verkeer Manager gebruiker wordt doorverwezen naar alternatieve eindpunten voor het behoud van de beschikbaarheid van de service. Dit eindpunt controle en failover-gedrag geldt voor alle methoden voor routering van verkeer. Zie [Beheer van verkeer eindpunt Monitoring](traffic-manager-monitoring.md)voor meer informatie. Controle eindpunt werkt anders voor geneste profielen. Met geneste profielen uitvoeren het profiel van de bovenliggende niet op de onderliggende controles rechtstreeks. In plaats daarvan, de gezondheid van de eindpunten van het kind profiel wordt gebruikt voor het berekenen van de algehele gezondheid van het profiel van het kind. Deze gezondheidsinformatie van wordt de hiërarchie van geneste profiel doorgegeven. Het bovenliggende profiel dit gezondheid om te bepalen of u wilt direct verkeer naar het profiel van het kind samengevoegd. Zie de sectie [Veelgestelde vragen](#faq) van dit artikel voor meer informatie over het controleren van de gezondheid van geneste profielen.

Terugkeren naar het vorige voorbeeld, Stel dat de productie-implementatie in West-Europa is mislukt. Het profiel 'child' Standaard zorgt ervoor dat alle verkeer voor de implementatie van de test. Als de implementatie test ook mislukt, bepaalt het bovenliggende profiel dat het profiel van het kind geen verkeer ontvangen moet omdat alle onderliggende eindpunten beschadigd zijn. Vervolgens wordt het profiel van de bovenliggende verkeer naar andere regio's verdeeld.

![Geneste profiel failover (standaardinstelling)][3]

Het is mogelijk dat u tevreden met deze regeling. Of mogelijk betrokken dat alle verkeer voor West-Europa nu de implementatie testen in plaats van een beperkte subset verkeer gaat is. Ongeacht de status van de implementatie test wilt u failover uitvoeren naar de andere regio's als de productie-implementatie in West-Europa is mislukt. Als u wilt dat deze failover, kunt u de parameter 'MinChildEndpoints' bij het configureren van het profiel van het kind als een eindpunt in het profiel van de bovenliggende. De parameter bepaalt het minimum aantal beschikbare eindpunten in het profiel van het kind. De standaardwaarde is '1'. In dit scenario kunt u de waarde van MinChildEndpoints ingesteld op 2. Onder deze drempel, het profiel van de bovenliggende beschouwt de hele onderliggende profiel niet beschikbaar is en zorgt ervoor dat het verkeer naar de andere eindpunten.

De volgende afbeelding ziet u deze configuratie:

![Geneste profiel failover met 'MinChildEndpoints' = 2][4]

>[AZURE.NOTE]
>De methode 'Priority' Routering van verkeer wordt al het verkeer naar een enkel eindpunt verdeeld. Er is dus weinig zin in een MinChildEndpoints dan '1' voor het profiel van een kind.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Voorbeeld 3: Prioriteit failover-regio's in de 'Performance'-verkeer routeren

De standaardinstelling voor de methode 'Prestaties' Routering van verkeer is ontworpen om te voorkomen dat te veel laden van de volgende dichtstbijzijnde eindpunt en een trapsgewijze reeks fouten veroorzaakt. Als een eindpunt is mislukt, wordt al het verkeer dat zou zijn gericht op dat eindpunt evenredig verdeeld over de andere eindpunten in alle regio's.

!['Prestaties'-verkeer routeren met standaard failover][5]

Stel echter dat u liever de overname van West-Europa verkeer West VS en alleen directe verkeer naar andere regio's als beide eindpunten niet beschikbaar zijn. U kunt deze oplossing met behulp van het profiel van een kind met de methode 'Priority'-verkeer routeren.

!['Prestaties'-verkeer routeren met preferentiële failover][6]

Aangezien het eindpunt van de West-Europa heeft een hogere prioriteit dan de Amerikaanse West eindpunt, wordt alle verkeer wordt verzonden naar het eindpunt van de West-Europa als beide eindpunten on line bent. Als West-Europa is mislukt, is het verkeer gericht op West VS. Verkeer wordt omgeleid naar Oost-Azië met de geneste profiel alleen wanneer zowel West-Europa en West VS mislukken.

U kunt dit patroon herhalen voor alle regio's. Vervangen door alle drie de eindpunten in het profiel van de bovenliggende drie onderliggende profielen, elk voor een naar prioriteit gerangschikte failover-reeks.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Voorbeeld 4: Beheren 'Performance'-verkeer routeren tussen meerdere endpoints in dezelfde regio

Stel dat de prestaties die verkeer routing-methode wordt gebruikt in een profiel met meer dan één eindpunt in een bepaalde regio. Standaard verkeer dat bestemd is voor die regio gelijkmatig verdeeld over alle beschikbare eindpunten in die regio.

!["Prestaties" verkeer routering van verkeer in de regio distributie (standaardinstelling)][7]

In plaats van het toevoegen van meerdere endpoints in West-Europa, tussen de eindpunten in een afzonderlijk onderliggend profiel. Het profiel van het kind wordt toegevoegd aan de hoofd-als het eindpunt alleen in West-Europa. De instellingen in het profiel van het kind kunnen bepalen van de verdeling van het verkeer met West-Europa doordat op basis van prioriteit of gewogen verkeer routing binnen die regio.

!['Performance'-verkeer routeren met aangepaste in regio verkeer verdeling][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Voorbeeld 5: Per eindpunt controle-instellingen

Stel dat u met behulp van beheer van verkeer vlotter migreren verkeer uit een oudere versie van-ruimten website naar een nieuwe versie op basis van Cloud gehost in Azure. Voor de oude site die u wilt gebruiken de URI van de introductiepagina voor het controleren van de gezondheid van de site. Maar voor de nieuwe Cloud-versie u een aangepaste controle pagina implementeert (pad ' / monitor.aspx') die worden aanvullende controles uitgevoerd.

![Beheer van netwerkverkeer eindpunt monitoring (standaardinstelling)][9]

De controle-instellingen in een profiel Manager verkeer van toepassing op alle eindpunten in één profiel. Met geneste profielen kunt u een ander kind profiel per site verschillende controle-instellingen te definiëren.

![Beheer van netwerkverkeer eindpunt met per eindpunt instellingen controleren][10]

## <a name="faq"></a>FAQ

### <a name="how-do-i-configure-nested-profiles"></a>Hoe kan ik geneste profielen configureren?

Geneste verkeer profielen kunnen worden geconfigureerd met behulp van bronbeheer Azure en de klassieke Azure REST API's, Azure PowerShell-cmdlets en cross-platform Azure CLI-opdrachten. Ze worden ook ondersteund via de nieuwe portal Azure. Ze worden niet ondersteund in de classic-portal.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Hoeveel lagen van nesten wordt verkeer Manager ondersteund?

U kunt profielen tot 10 niveaus diep worden genest. De lus"zijn niet toegestaan.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Kan ik andere typen eindpunt mengen met onderliggende geneste profielen, in hetzelfde verkeer Manager profiel

Ja. Er zijn geen beperkingen op hoe u de eindpunten van de verschillende typen binnen een profiel combineren.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Hoe is het factuuradres model van toepassing voor geneste profielen?

Er is geen negatieve gevolgen van het gebruik van geneste profielen prijzen.

Beheer van verkeer facturering bestaat uit twee onderdelen: eindpunt health checks en miljoenen van DNS-query's

- De gezondheidscontroles eindpunt: Er zijn geen kosten voor een profiel van het kind als geconfigureerd als een eindpunt in het profiel van een bovenliggende. Controle van de eindpunten in het profiel van het kind worden in rekening gebracht op de gebruikelijke manier.
- DNS-query's: elke query is slechts één keer geteld. Een query op een bovenliggende profiel dat geeft als een eindpunt van een profiel van het kind resultaat wordt ten opzichte van het bovenliggende profiel alleen geteld.

Zie de [prijzen pagina voor beheer van netwerkverkeer](https://azure.microsoft.com/pricing/details/traffic-manager/)voor alle details.

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Is er een invloed op de prestaties voor geneste profielen?

Nr. Er is geen invloed op de prestaties bij het gebruik van geneste profielen ontstaan.

De naamservers van het beheer van verkeer via de hiërarchie profiel intern bij de verwerking van elke DNS-query. Een DNS-query naar een bovenliggende profiel kunt u een DNS-antwoord aan een eindpunt van een profiel kind ontvangen. Één CNAME-record wordt gebruikt, ongeacht of u één profiel of geneste profielen. Er is niet nodig voor het maken van een CNAME-record voor elk profiel in de hiërarchie.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Hoe wordt verkeer Manager de gezondheid van een geneste eindpunt in een bovenliggende profiel berekend?

Het bovenliggende profiel uitvoeren geen controles op de onderliggende rechtstreeks. In plaats daarvan, de gezondheid van de eindpunten van het kind profiel worden gebruikt voor het berekenen van de algehele gezondheid van het profiel van het kind. Deze informatie wordt doorgegeven van de hiërarchie van geneste profiel om te bepalen van de gezondheid van de geneste eindpunt. Deze geaggregeerde gezondheid het bovenliggende profiel gebruikt om te bepalen of het verkeer kan worden omgeleid naar de onderliggende.

De volgende tabel beschrijft het gedrag van verkeer Manager, gezondheidscontroles voor een geneste eindpunt.

|Status van de onderliggende profiel Monitor|Bovenliggende eindpunt Monitor status|Notities|
|---|---|---|
|Uitgeschakeld. Het profiel van het kind is uitgeschakeld.|Gestopt|De status van de bovenliggende wordt gestopt, niet uitgeschakeld. De status is gereserveerd voor die aangeeft dat u het eindpunt in het bovenliggende profiel hebt uitgeschakeld.|
|Verslechterd. Ten minste één onderliggend profiel eindpunt is een gedegradeerd.| Online: het aantal Online eindpunten in het profiel van het kind is ten minste de waarde van MinChildEndpoints.<BR>CheckingEndpoint: het aantal Online plus CheckingEndpoint eindpunten in het profiel van het kind is ten minste de waarde van MinChildEndpoints.<BR>Verslechterd: anders.|Verkeer wordt doorgestuurd naar een eindpunt van de status van CheckingEndpoint. Als MinChildEndpoints te hoog is ingesteld, wordt het eindpunt is altijd verslechterd.|
|On line. Ten minste één onderliggend profiel eindpunt is on line zijn. Er is geen eindpunt in staat gedegradeerd.|Zie hierboven.||
|CheckingEndpoints. Ten minste één onderliggend profiel eindpunt is 'CheckingEndpoint'. Geen eindpunten zijn 'Online' of 'Doorgaan'|Hetzelfde als hierboven.||
|Niet-actief. Alle onderliggende profiel eindpunten zijn uitgeschakeld of gestopt, of dit profiel bevat geen eindpunten.|Gestopt||


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [werking van beheer van netwerkverkeer](traffic-manager-how-traffic-manager-works.md)

Meer informatie over het [maken van een serviceprofiel verkeer](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png

