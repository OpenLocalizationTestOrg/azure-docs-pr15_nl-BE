<properties
    pageTitle="Beheer van verkeer - methoden voor routering van verkeer | Microsoft Azure"
    description="In dit artikel helpt u bij de ander verkeer routing-methoden gebruikt door verkeer Manager"
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

# <a name="traffic-manager-traffic-routing-methods"></a>Routering van verkeer methoden voor beheer van netwerkverkeer

Azure verkeer Manager ondersteunt drie routering van verkeer methoden om te bepalen hoe netwerkverkeer moet worden gerouteerd naar de verschillende eindpunten. De methode voor het routeren van verkeer geldt verkeer Manager voor elke DNS-query die wordt ontvangen. De methode voor het routeren van verkeer bepaalt welk endpoint geretourneerd in het DNS-antwoord.

De bronnenbeheerder Azure ondersteuning voor verkeer Manager gebruikt een andere terminologie dan de klassieke implementatiemodel. De volgende tabel ziet u de verschillen tussen de Resource Manager en klassieke voorwaarden:

| De term Resource Manager | Klassieke term |
|-----------------------|--------------|
| Methode voor routering van verkeer | Load balancing-methode |
| Prioriteit, methode | Failover-methode |
| Gewogen, methode | Round-robin methode |
| Prestaties, methode | Prestaties, methode |

Op basis van feedback van klanten, wij de terminologie om de duidelijkheid en minder voorkomende misverstanden gewijzigd. Er is geen verschil in functionaliteit.

Er zijn drie methoden voor het doorsturen van verkeer beschikbaar in beheer van verkeer:

- **Prioriteit:** Selecteer 'Priority' wanneer u een primaire-service-eindpunt voor alle verkeer en back-ups bieden voor het geval de primaire of de back-eindpunten niet beschikbaar zijn.
- **Gewogen:** Selecteer 'gewogen' als u wilt verkeer verdelen over een aantal eindpunten, een gelijkmatig of op basis van het gewicht, dat u kunt definiëren.
- **Prestaties:** Selecteer 'Performance' wanneer u eindpunten in verschillende geografische locaties hebt en u wilt dat gebruikers gebruiken de "dichtstbijzijnde" eindpunt van de laagste netwerkvertraging.

Alle verkeer profielen zijn controle eindpunt gezondheids- en eindpunt automatische failover. Zie [Beheer van verkeer eindpunt Monitoring](traffic-manager-monitoring.md)voor meer informatie. Één profiel verkeer Manager kunt slechts één verkeer routeringsmethode gebruiken. U kunt een ander verkeer routeringsmethode voor uw profiel op elk gewenst moment selecteren. Wijzigingen worden toegepast binnen één minuut en geen downtime is ontstaan. Routering van verkeer methoden kunnen worden gecombineerd met behulp van geneste verkeer profielen. Nesten kunt geavanceerde en flexibele verkeer routing configuraties die voldoen aan de behoeften van grotere, complexe toepassingen. Zie [geneste verkeer profielen](traffic-manager-nested-profiles.md)voor meer informatie.

## <a name="priority-traffic-routing-method"></a>Routering van verkeer prioriteit-methode

Vaak wil een organisatie betrouwbaarheid voor haar diensten door een of meer back-services implementeren voor het geval de primaire service afgesloten wordt. De methode 'Priority' Routering van verkeer kan Azure klanten dit patroon failover eenvoudig te implementeren.

![Azure verkeer Manager 'Priority' methode routering van verkeer][1]

Het beheer van netwerkverkeer-profiel bevat een naar prioriteit gerangschikte lijst van eindpunten. Manager verkeer verzendt standaard al het verkeer naar het primaire eindpunt van de (hoogste prioriteit). Als het primaire eindpunt niet beschikbaar is, stuurt verkeer Manager het verkeer naar het tweede eindpunt. Als de primaire en secundaire eindpunten niet beschikbaar zijn, gaat het verkeer naar de derde, enzovoort. Beschikbaarheid van het eindpunt is gebaseerd op de geconfigureerde status (ingeschakeld of uitgeschakeld) en het toezicht op de doorlopende eindpunt.

### <a name="configuring-endpoints"></a>Eindpunten configureren

Met Azure Resource Manager u de prioriteit van het eindpunt met expliciet de eigenschap 'priority' voor elk eindpunt. Deze eigenschap is een waarde tussen 1 en 1000. Lagere waarden geven een hogere prioriteit. Eindpunten delen niet prioriteitswaarden. De eigenschap is optioneel. Als dit argument wordt weggelaten, wordt een standaardprioriteit op basis van de volgorde van het eindpunt gebruikt.

Met de klassieke interface, de prioriteit van het eindpunt impliciet geconfigureerd. De prioriteit is gebaseerd op de volgorde waarin de eindpunten in de definitie van het profiel.

## <a name="weighted-traffic-routing-method"></a>Gewogen routingmethode voor verkeer

De 'Gewogen' verkeer routing-methode kunt u verkeer gelijkmatig verdelen of een vooraf gedefinieerde wegingsfactor gebruiken.

![Azure verkeer Manager 'Gewogen' verkeer-routeringsmethode][2]

In de methode gewogen routering van verkeer moet u een gewicht toewijzen aan elk eindpunt in de profielconfiguratie van verkeer Manager. Het gewicht is een geheel getal tussen 1 en 1000. Deze parameter is optioneel. Als dit argument wordt weggelaten, wordt verkeer Managers een hoeveelheid '1'.

Voor elke DNS-query wordt ontvangen, kiest beheer van netwerkverkeer willekeurig een eindpunt beschikbaar. De kans van het kiezen van een eindpunt is gebaseerd op het gewicht van alle beschikbare eindpunten. Met behulp van hetzelfde gewicht tussen alle eindpunten resultaten in een distributie zelfs verkeer. Hogere of lagere gewichten op specifieke eindpunten, treedt deze eindpunten meer of minder vaak worden geretourneerd in het DNS-antwoorden.

De gewogen methode kunnen sommige gevallen handig:

- Geleidelijke toepassing upgrade: een percentage van het verkeer door te sturen naar een nieuw eindpunt toewijzen en geleidelijk verhogen van het verkeer over de tijd op 100%.
- Toepassingen migreren naar Azure: Maak een profiel met Azure zowel externe eindpunten. Het gewicht van de eindpunten op de eindpunten van de nieuwe voorkeur aanpassen.
- Voor extra capaciteit bursting cloud: snel een implementatie op gebouwen in de cloud uitbreiden door de gegevens achter een serviceprofiel verkeer. Wanneer u extra capaciteit in de cloud, kunt u toevoegen of meer eindpunten en opgeven welk gedeelte van het verkeer naar elk eindpunt gaat.

De nieuwe portal Azure biedt ondersteuning voor de configuratie van de verkeersroutering gewogen. Het gewicht kunnen niet worden geconfigureerd in de klassieke portal. U kunt ook de gewichten met de resourcemanager en classic versies van Azure PowerShell, CLI en de REST API's.

Het is belangrijk te begrijpen dat DNS-antwoorden in de cache opgeslagen door de clients en de recursieve DNS-servers die clients gebruiken om DNS-namen te herleiden. Deze cache, kan gevolgen hebben voor de gewogen verkeer verdelingen. Wanneer het aantal clients en DNS-servers recursieve groot is, wordt verkeer distributie werkt zoals verwacht. Echter, wanneer het aantal clients of recursieve DNS-servers klein is, caching kunt aanzienlijk scheeftrekken de verdeling van het verkeer.

Algemene use-cases zijn:

- Ontwikkel- en testomgevingen
- Communicatie toepassingen
- Toepassingen die zijn gericht op een smalle-gebruikersgroep die delen een gemeenschappelijke recursieve DNS-infrastructuur (bijvoorbeeld medewerkers van een bedrijf dat een verbinding via een proxy)

Deze DNS-caching effecten gelden voor alle DNS-verkeer routeren systemen niet alleen Azure verkeer Manager. In sommige gevallen kan uitdrukkelijk het wissen van de DNS-cache oplossing geboden. In andere gevallen een alternatieve methode voor het routeren van verkeer mogelijk beter.

## <a name="performance-traffic-routing-method"></a>Routering van verkeer methode van prestaties

Implementeren van eindpunten in twee of meer locaties over de hele wereld kan de reactietijd van de vele toepassingen door het routeren van verkeer naar de locatie die zich het dichtst bij u verbeteren. De methode 'Prestaties'-verkeer routeren biedt deze functionaliteit.

![Azure verkeer Manager 'Performance' methode routering van verkeer][3]

Het eindpunt van de 'beste' is niet noodzakelijkerwijs het dichtst zoals gemeten volgens de geografische afstand. In plaats daarvan bepaalt de methode 'Prestaties' Routering van verkeer het dichtstbijzijnde eindpunt door het meten van netwerkvertraging. Verkeer Manager houdt een Internet latentie tabel voor het bijhouden van de round trip-tijd tussen elke Azure datacenter en de IP-adresbereiken.

Beheer van verkeer, zoekt u het bron-IP-adres van de inkomende DNS-aanvraag in de tabel Internet latentie. Beheer van verkeer kiest een eindpunt beschikbaar in de Azure datacenter dat is de laagste latentie voor dat bereik van IP-adres en geeft vervolgens dat eindpunt in het DNS-antwoord.

Zoals uiteengezet in [Het verkeer Manager Works](traffic-manager-how-traffic-manager-works.md), ontvangt verkeer Manager geen DNS-query's van clients rechtstreeks. In plaats daarvan wordt DNS-query's zijn afkomstig van de recursieve DNS-service dat de clients zijn geconfigureerd voor gebruik. Daarom het IP-adres wordt gebruikt om te bepalen van het eindpunt van de "dichtstbijzijnde" heeft geen IP-adres van de client, maar is het IP-adres van de recursieve DNS-service. Dit IP-adres is in de praktijk een goede proxy voor de client.

Beheer van netwerkverkeer wordt regelmatig bijgewerkt in de tabel Internet latentie voor wijzigingen in de wereldwijde Internet en nieuwe Azure gebieden. Echter afhankelijk prestaties van real-time variaties in laden via het Internet. Prestaties-verkeer routeren controleert belasting van een bepaalde service-eindpunt. Echter, als een eindpunt niet beschikbaar is, Manager verkeer niet bestaat in de reacties op DNS-query's.

Punten om te onthouden:

- Als uw profiel meerdere endpoints in hetzelfde gebied, Azure bevat, vervolgens verdeelt verkeer Manager verkeer gelijkmatig over de beschikbare eindpunten in die regio. U kunt desgewenst een ander verkeer verdeling binnen een gebied kunt u [geneste verkeer profielen](traffic-manager-nested-profiles.md).

- Als u alle ingeschakelde eindpunten in een bepaald gebied Azure zijn verslechterd, verdeelt Manager verkeer verkeer over andere beschikbare eindpunten in plaats van de volgende dichtstbijzijnde eindpunt. Deze logica wordt voorkomen dat een CSS-fout optreden door overbelasting van de volgende dichtstbijzijnde eindpunt niet. Als u wilt dat een reeks voorkeur failover definiëren, gebruikt u [geneste verkeer Manager profielen](traffic-manager-nested-profiles.md).

- Als u de prestaties routeringsmethode voor verkeer met externe eindpunten of geneste eindpunten, moet u de locatie van deze eindpunten opgeven. Kies de Azure regio die het dichtst bij de implementatie. Deze locaties zijn de waarden die worden ondersteund door de latentie Internet tabel.

- Het algoritme dat ervoor het eindpunt kiest is deterministisch. Herhaalde DNS-query's vanaf dezelfde client worden doorgestuurd naar hetzelfde eindpunt. Clients gebruiken meestal verschillende recursieve DNS-servers als u onderweg bent. De client kan worden gerouteerd naar een eindpunt. Routering kan ook worden beïnvloed door wijzigingen aan de tabel Internet latentie. De methode prestaties routering van verkeer is daarom geen garantie dat een client altijd doorgestuurd naar hetzelfde eindpunt.

- Wanneer de latentie Internet tabel verandert, is het mogelijk dat sommige clients worden doorgestuurd naar een eindpunt. Deze wijziging routering is nauwkeuriger op basis van de huidige gegevens van de latentie. Deze updates zijn essentieel voor het handhaven van de nauwkeurigheid van prestaties-verkeer routeren Internet voortdurend ontwikkeld.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het hoge beschikbaarheid ontwikkelen met behulp van [verkeer Manager eindpunt controleren](traffic-manager-monitoring.md)

Meer informatie over het [maken van een serviceprofiel verkeer](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png
