<properties
    pageTitle="Aanbevolen procedures voor autoscaling Azure Monitor. | Microsoft Azure"
    description="Informatie over de beginselen voor het effectief gebruik autoscaling in Azure Monitor."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="best-practices-for-azure-monitor-autoscaling"></a>Aanbevolen procedures voor Azure Monitor autoscaling

In de volgende secties in dit document kunnen u de aanbevolen procedures voor het automatisch schalen in Azure begrijpen. Controleer deze informatie en zult u beter kunnen automatisch schalen effectief te gebruiken in uw infrastructuur Azure.

## <a name="autoscale-concepts"></a>Concepten automatisch schalen

- Een resource kan slechts *één* instelling voor automatisch schalen hebben
- Een instelling automatisch schalen kan een of meer profielen en elk profiel kunnen een of meer regels voor automatisch schalen hebben.
- Een instelling automatisch schalen wordt geschaald exemplaren horizontaal, die is *uitgevoerd* door het verhogen van de instanties en *in* door het verlagen van het aantal exemplaren.
 Een instelling automatisch schalen heeft een maximum, minimum en standaardwaarde van exemplaren.
- Een taak automatisch schalen leest altijd de bijbehorende metric te schalen, controleren of het de geconfigureerde drempel voor schalen of de schaal is gepasseerd. U kunt een lijst weergeven van statistieken die automatisch schalen kunt schalen door op [Azure Monitor autoscaling gemeenschappelijke maatstaven](insights-autoscale-common-metrics.md).
- Alle drempels worden berekend op het niveau van het exemplaar. Bijvoorbeeld "schaal door 1 exemplaar als gemiddelde CPU > 80% als count exemplaar 2", scale-out betekent dat wanneer de gemiddelde CPU in alle exemplaren groter dan 80 is %.
- U ontvangt altijd fout meldingen via e-mail. De eigenaar, Inzender en lezers van de doelbron speciaal, ontvangen e-mail. U ontvangt een e-mail met een *herstel* ook altijd bij het automatisch schalen van een storing wordt hersteld en begint normaal functioneert.
- U kunt opt-in voor het ontvangen van een succesvolle schaal actie melding via e-mail en webhooks.

## <a name="autoscale-best-practices"></a>Aanbevolen procedures voor automatisch schalen

Gebruik de volgende procedures als u automatisch schalen gebruiken.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>De maximale en minimale waarden verschillend zijn en een voldoende marge tussen hen hebben
Als u een instelling die de minimum = 2, maximum = 2 en de huidige instantie count 2 is geen actie schaal kan optreden. Houd een voldoende marge tussen de maximale en minimale exemplaar tellingen, inclusief zijn. Automatisch schalen worden altijd geschaald tussen deze limieten.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Handmatig schalen wordt opnieuw ingesteld door automatisch schalen min en max.
Als u de telling van de instantie handmatig met een waarde boven of onder het maximum bijwerkt, wordt de motor automatisch schalen automatisch terug naar het minimum (indien hieronder) of het maximum (als boven) aangepast. U stelt bijvoorbeeld het bereik tussen 3 en 6. Als u een actieve sessie, wordt de motor automatisch schalen aangepast aan 3 exemplaren op de volgende keer wordt uitgevoerd. Ook dit zou schaal in 8 exemplaren terug naar 6 op de volgende keer wordt uitgevoerd.  Handmatig schalen is een zeer tijdelijk tenzij u ook de regels voor automatisch schalen opnieuw instellen.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Gebruik altijd een combinatie van schalen en schalen in regel waarmee een stijging en daling
Als u slechts een deel van de combinatie, is automatisch schalen schaal op die enkel uit of in, totdat het maximum of minimum, bereikt.

### <a name="do-not-switch-between-the-azure-portal-and-the-azure-classic-portal-when-managing-autoscale"></a>Niet overschakelen tussen de Azure portal en de klassieke Azure portal bij het beheren van automatisch schalen
Gebruik de Azure portal (portal.azure.com) maken en beheren van instellingen voor automatisch schalen voor Cloud Services en App Services (Web Apps). Gebruik voor virtuele Machine schaal Sets PoSH, CLI of REST API maken en beheren van de instelling automatisch schalen. Niet overschakelen tussen de klassieke Azure portal (manage.windowsazure.com) en de Azure portal (portal.azure.com) als automatisch schalen configuraties beheren. De Azure klassieke portal en de bijbehorende onderliggende backend heeft beperkingen. Verplaatsen naar de Azure portal beheren automatisch schalen met een grafische gebruikersinterface. De opties zijn het automatisch schalen PowerShell, CLI of REST API (via Azure Resource Explorer) gebruiken.

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Kies de juiste statistieken voor de diagnostische waarde
Voor diagnostische metrics, kunt u onder het *gemiddelde*, *Minimum*, *Maximum* - en *totale* als een metric door schalen. De meest voorkomende statistiek is *Gemiddeld*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>De drempels voor alle typen van metrische zorgvuldig kiezen
Wij raden u aan verschillende drempels voor schalen en schalen in op basis van praktische situaties zorgvuldig te kiezen.

We *raden u niet* automatisch schalen instellingen, zoals de voorbeelden hieronder met de dezelfde of zeer vergelijkbare drempelwaarden voor uit- en voorwaarden:

- Exemplaren met 1 verhoogd wanneer tellen aantal threads < = 600
- Exemplaren met 1 te verlagen wanneer tellen aantal threads > = 600

Bekijk een voorbeeld van wat kan leiden tot een probleem ontstaan. De volgende reeks cosider.

1. Stel dat er 2 exemplaren beginnen en vervolgens het gemiddelde aantal threads per exemplaar groeit tot 625.
2. Automatisch schalen, schaalt een 3e exemplaar toe.
3. Ga ervan uit dat het gemiddelde aantal threads op exemplaar aan 575 valt.
4. Voor het verkleinen, pogingen om te schatten wat de definitieve status automatisch schalen worden als deze aangepast. Bijvoorbeeld: 575 x 3 (huidige exemplaar count) = 1,725 / 2 (definitieve aantal gevallen waarin de verkleind) = 862.5 threads. Dit betekent dat automatisch schalen zou moeten onmiddellijk schalen opnieuw nadat deze geschaald in, als het gemiddelde aantal threads hetzelfde blijft of zelfs slechts een klein bedrag valt. Echter, als deze geschaald opnieuw het hele proces zou herhalen, leidt tot een oneindige lus.
5. Als u wilt voorkomen dat deze situatie ("flapping" genoemd), schaalt automatisch schalen niet naar beneden helemaal. In plaats daarvan wordt overgeslagen en reevaluates de voorwaarde opnieuw de volgende keer dat de taak van de service wordt uitgevoerd. Dit kan veel mensen verwarrend omdat automatisch schalen lijken wouldn't te werken wanneer het gemiddelde aantal threads 575 is.

Schatting tijdens een schaal in is bedoeld om "flappy"-situaties te vermijden. In gedachten wanneer u dezelfde drempels voor schalen en houd dit probleem.

Wij raden u aan een voldoende marge tussen de schalen en drempels te kiezen. Een voorbeeld, kunt u beter regel combinatie.

- Exemplaren verhoogd met 1 tellen wanneer CPU % > = 80
- Exemplaren met 1 verlagen tellen wanneer CPU % < = 60

In dit geval  

1. Stel er zijn beginnen met 2 exemplaren.
2. Als de gemiddelde CPU-percentage tussen exemplaren op 80 gaat, schalen automatisch schalen uit het toevoegen van een derde exemplaar.
3. Stel nu dat na verloop van tijd de CPU % 60 valt.
4. Automatisch schalen van schaal in regel maakt een schatting van de definitieve status te schalen in zijn. Bijvoorbeeld 60 x 3 (huidige exemplaar count) = 180 / 2 (definitieve aantal gevallen waarin de verkleind) = 90. Dus automatisch schalen heeft geen schaal in omdat deze schalen onmiddellijk opnieuw zou moeten. In plaats daarvan wordt het overgeslagen bij het verkleinen.
5. De volgende keer automatisch schalen blijft de CPU tot 50 vallen wordt gecontroleerd. Ramingen opnieuw - exemplaar 50 x 3 = 150 / 2 exemplaren = 75 die onder de drempel schalen van 80, zodat het schalen met succes 2 exemplaren.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Overwegingen bij het schalen van drempelwaarden voor bijzondere maatstaven
 De drempel is het gemiddelde aantal berichten per aantal exemplaren beschikbaar voor bijzondere maatstaven zoals lengte metric voor opslag of Bus-mailwachtrij Service. Kies het Kies zorgvuldig de drempelwaarde voor deze metriek.

Toegelicht aan het met een voorbeeld zodat u het probleem beter begrijpen.

- Exemplaren met 1 verhogen wanneer Storage Queue message telling > = 50
- Exemplaren met 1 verlagen wanneer Storage Queue message count < = 10

Houd rekening met de volgende volgorde:

1. Er zijn 2 opslag wachtrij exemplaren.
2. Berichten komen steeds en als u de wachtrij voor de opslag, het totale aantal 50 leest. U kan aannemen dat automatisch schalen een scale-out actie te starten. Bedenk wel dat het nog steeds 50/2 = 25 berichten per exemplaar. Scale-out treedt dus niet. Voor de eerste schalen te gebeuren, moet het totale aantal berichten in de wachtrij opslag 100.
3. Ga ervan uit dat het totale aantal berichten gelijk is aan 100.
4. Een 3e opslag wachtrij toevoegt als gevolg van een actie schalen.  De volgende schalen actie gebeurt niet totdat het totale aantal berichten in de wachtrij 150 bereikt omdat 150/3 = 50.
5. Nu wordt het aantal berichten in de wachtrij verkleind. Met 3 exemplaren, de eerste schaal in actie gebeurt wanneer het totale aantal berichten in alle wachtrijen tot 30 toevoegen omdat 30/3 = 10 berichten per exemplaar, wordt de drempel voor schaal.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Overwegingen bij het schalen als u meerdere profielen worden geconfigureerd in een instelling automatisch schalen

In een instelling automatisch schalen kunt u een standaardprofiel, die altijd wordt toegepast zonder een afhankelijkheid op schema of tijd, of u kunt een terugkerende profiel of een profiel voor een bepaalde periode met een datum- en tijdbereik.

Wanneer de service automatisch schalen verwerkt, altijd worden gecontroleerd in de volgende volgorde:

1. Vaste datum profiel
2. Terugkerende profiel
3. Standaard profiel ('altijd')

Als u een profiel voorwaarde is voldaan, wordt automatisch schalen de volgende voorwaarde profiel eronder niet gecontroleerd. Automatisch schalen verwerkt slechts één profiel tegelijk. Dit betekent dat als u ook een verwerkingsvoorwaarde van een lagere laag profiel wilt, moet u opnemen die voorschriften ook in het huidige profiel.

Bekijken we dit met een voorbeeld:

De onderstaande afbeelding ziet u een instelling automatisch schalen met een standaardprofiel van minimale exemplaren = 2- en maximumaantal exemplaren = 10. In dit voorbeeld regels zijn geconfigureerd om te schalen wanneer het aantal berichten in de wachtrij groter dan 10 is en schaal in als het aantal berichten in de wachtrij kleiner dan 3 is. De bron kan nu schaal tussen 2 en 10 exemplaren.

Bovendien is het een terugkerende profiel instellen voor maandag. Waarde voor minimum exemplaren = 2- en maximumaantal exemplaren = 12. Dit betekent dat op maandag, de eerste keer automatisch schalen wordt gecontroleerd op deze voorwaarde, als het aantal exemplaar 2 is, wordt aangepast aan het nieuwe minimum van 3. Als automatisch schalen blijft vinden dit profiel voorwaarde voldaan (maandag), alleen verwerkt de op basis van CPU scale-out/in regels die zijn geconfigureerd voor dit profiel. Op dit moment is het niet meer of lengte van de wachtrij. Echter, als u wilt dat de wachtrij lengte voorwaarde moet worden gecontroleerd, moet u opnemen deze regels uit het standaardprofiel ook in uw profiel voor maandag.

Op dezelfde manier als automatisch schalen terug naar de standaard-profiel schakelt, wordt eerst gecontroleerd als het minimum en maximum voorwaarden. Als het aantal exemplaren op het moment 12 is, wordt deze op 10, het maximum voor het standaardprofiel.

![instellingen voor automatisch schalen](./media/insights-autoscale-best-practices/insights-autoscale-best-practices.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Overwegingen bij het schalen als u meerdere regels in een profiel zijn geconfigureerd
Er bestaan situaties waarin u wellicht meerdere regels in een profiel instellen. De volgende reeks regels automatisch schalen worden gebruikt door services gebruiken wanneer meerdere regels zijn ingesteld.

Op *de schaal van*automatisch schalen uitgevoerd als aan alle regels wordt voldaan.
Automatisch schalen nodig op *schaal in*alle regels wordt voldaan.

Ter illustratie wordt uitgegaan van de volgende 4 regels voor automatisch schalen:

- Als CPU < 30%, schaal-1
- Als geheugen < 50% schaal in 1
- Als CPU > 75%, schalen met 1
- Als geheugen > 75%, schalen met 1

Klik op de volgende plaats:

- Als de CPU is 76% en het geheugen is 50%, we schalen.
- Als is 50% van CPU en geheugen 76 is % we schalen.

Aan de andere kant als is 25% van CPU en geheugen automatisch schalen 51 is % heeft **geen** schaal in. Om schaal, in CPU moet 29% en geheugen 49%.

### <a name="always-select-a-safe-default-instance-count"></a>Selecteer altijd een aantal veilige standaard exemplaar
Het aantal standaard exemplaar is belangrijk automatisch schalen die count-service wordt geschaald wanneer parameters niet beschikbaar zijn. Selecteer daarom een standaard exemplaar telling die veilig is voor uw werkbelasting.

### <a name="configure-autoscale-notifications"></a>Automatisch schalen meldingen configureren
Automatisch schalen wordt beheerders en medewerkers van de resource per e-mail gewaarschuwd als een van de volgende omstandigheden optreden:

- automatisch schalen service niet om een actie te ondernemen.
- Parameters zijn niet beschikbaar voor een beslissing schalen automatisch schalen-service.
- Parameters zijn beschikbaar (recovery) opnieuw een schaal om beslissing te nemen.
Afgezien van de bovenstaande voorwaarden, kunt u e-mail- of webhook meldingen te waarschuwen voor schaal geslaagde acties.
