<properties
    pageTitle="Aantal exemplaar schalen handmatig of automatisch | Microsoft Azure"
    description="Informatie over het schalen van uw diensten Azure."
    authors="rboucher"
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="scale-instance-count-manually-or-automatically"></a>Aantal exemplaar schalen handmatig of automatisch

In de [Azure Portal](https://portal.azure.com/)kunt u de telling van het exemplaar van de service handmatig instellen, of kunt u parameters instellen zodat deze automatisch schalen op basis van vraag. Dit is meestal genoemd *Scale out* of *schaal in*.

Voordat u schalen op basis van aantal exemplaar, kunt u overwegen dat schalen wordt beïnvloed door **prijzen laag** naast count exemplaar. Verschillende prijzen lagen kunnen verschillende nummers cores en geheugen, en hebben ze dus betere prestaties voor hetzelfde aantal exemplaren (dit is de *schaal omhoog* of *Omlaag-schaal*). Dit artikel specifiek betrekking heeft op *schaal in* en *out*.

U kunt schalen in de portal en u kunt ook de [REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx) of de [SDK voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Insights/) gebruiken om aan te passen schaal handmatig of automatisch.

> [AZURE.NOTE] In dit artikel wordt beschreven hoe een instelling automatisch schalen maken in de portal op [http://portal.azure.com](http://portal.azure.com). Instellingen voor automatisch schalen gemaakt in deze portal kunnen niet worden bewerkt dat de klassieke portal ([http://manage.windowsazure.com](http://manage.windowsazure.com)).

## <a name="scaling-manually"></a>Handmatig schalen

1. Klik op **Bladeren**en Ga naar de resource die u aanpassen wilt, zoals een **App serviceplan**in [Azure Portal](https://portal.azure.com/).

2. De **schaal** naast elkaar in **bewerkingen** ziet u hier de status van de schaal: **uitschakelen** voor wanneer u handmatig **op** voor wanneer u met de prestatiegegevens van een of meer schalen zijn schalen.
    ![Schaal naast elkaar](./media/insights-how-to-scale/Insights_UsageLens.png)

3. Klik op de tegel klikken, gaat u naar de **schaal** blade. Aan de bovenkant van het blad schaal ziet u een overzicht van de acties automatisch schalen de service.  
    ![Schaal blade](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)

>[AZURE.NOTE] Alleen de acties die worden uitgevoerd door automatisch schalen wordt in deze grafiek weergegeven. Als u de telling van de instantie handmatig aanpast, wordt de wijziging niet doorgevoerd in deze grafiek.

4. Het aantal **exemplaren** met de schuifregelaar kunt u handmatig aanpassen.
5. Klik op de opdracht **Opslaan** en u zult worden geschaald naar dat aantal exemplaren vrijwel onmiddellijk.

## <a name="scaling-based-on-a-pre-set-metric"></a>Schalen op basis van een vooraf ingestelde metric

Als u het aantal exemplaren automatisch wordt aangepast op basis van een metriek, selecteer de gewenste metric in de vervolgkeuzelijst **schaal door** . Bijvoorbeeld voor een **serviceplan App** kunt schalen met een **Percentage van de CPU**.

1. Wanneer u een waarde selecteert krijgt u een schuifregelaar en/of, geef het aantal exemplaren dat u aanpassen wilt tussen tekstvakken:

    ![Blade met CPU-Percentage schaal](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)

    Automatisch schalen duurt nooit uw service onder of boven de grenzen die u hebt ingesteld, ongeacht de belasting.

2. Ten tweede kunt u het doelbereik voor de metric. Bijvoorbeeld als u **CPU-percentage**kiest, kunt u een doel instellen voor de gemiddelde CPU in alle exemplaren in uw service. Een schaal die er gebeurt als de gemiddelde CPU overschrijdt het maximum dat u definieert, een schaal in gebeurt op dezelfde manier als de gemiddelde CPU zakt tot onder het minimum.

3. Klik op de opdracht **Opslaan** . Automatisch schalen wordt gecontroleerd om ervoor te zorgen dat u in het bereik van de instantie en het doeldomein voor de metric bent om de paar minuten. Als uw service extra verkeer ontvangt, krijgt u meer exemplaren zonder iets te doen.

## <a name="scale-based-on-other-metrics"></a>Schaal op basis van andere gegevens

U kunt schalen op basis van maatstaven dan de voorinstellingen die worden weergegeven in de vervolgkeuzelijst **schalen door** en kunnen zelfs een complexe reeks geschaald uitbreiden en schalen in regels.

### <a name="adding-or-changing-a-rule"></a>Toevoegen of wijzigen van een regel

1. Kies het **schema en de prestaties** in de vervolgkeuzelijst **schaal door** : ![prestaties regels](./media/insights-how-to-scale/Insights_PerformanceRules.png)

2. Als u automatisch schalen eerder had, ziet op u een overzicht van de exacte regels die u had.

3. Als u wilt schalen op basis van een andere metrische klikt u op de rij **Regel toevoegen** . U kunt ook klikken op een van de bestaande rijen wijzigen van de metric die eerder u de metric die u schalen moest wilt door.
![Regel toevoegen](./media/insights-how-to-scale/Insights_AddRule.png)

4. Nu moet u selecteren welke metric die u wilt schalen door. Bij het kiezen van een metric is een paar dingen te overwegen:
    * De *resource* die de metric vandaan. Normaal gesproken, dit is hetzelfde als de resource die u hebben. Als u aanpassen door de diepte van een wachtrij voor de opslag wilt, is de resource de wachtrij die u schalen wilt door.
    * De *metrische naam* zelf.
    * De *Tijdaggregatie* van de metric. Dit is hoe de gegevens combineren is over de *duur*.

5. Na het kiezen van de metric kiest u de drempel voor de metric en de operator. U kunt bijvoorbeeld zeggen dat **groter is dan** **80%**.

6. Kies de actie die u wilt maken. Er zijn een paar soorten acties:
    * Vergroten of verkleinen door - deze kunt toevoegen of verwijderen van de **waarde** aantal exemplaren dat u wilt definiëren
    * Verhogen of verlagen percentage - Hiermee wijzigt u de telling van het exemplaar met een percentage. Zo kan u 25 plaatsen in het veld **waarde** en als u op dit moment 8 exemplaren had, 2 zou worden toegevoegd.
    * Vergroten of verkleinen tot - Hiermee wordt de telling van de instantie ingesteld op de **waarde** die u definieert.

7. Ten slotte kunt u afkoelen beneden - hoe lang deze regel na de vorige actie met schaal wachten moet schaalt opnieuw.

8. Klik op **OK**na het configureren van uw regel.

9. Wanneer u alle gewenste regels hebt geconfigureerd, moet u Klik op de opdracht **Opslaan** .

### <a name="scaling-with-multiple-steps"></a>Schalen met meerdere stappen

De bovenstaande voorbeelden zijn vrij eenvoudig. Als u wilt meer agressief over schalen omhoog (of omlaag), kunt u echter ook meerdere regels voor dezelfde metric voor schaal toevoegen. U kunt bijvoorbeeld twee schaal regels definiëren op CPU-percentage:

1. Geschaald uit door 1 exemplaar als CPU-percentage hoger dan 60 is %
2. Geschaald uit door 3 exemplaren als CPU-percentage hoger dan 85 is %

![Meerdere regels voor schaal](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

Met deze extra regel als de belasting hoger is dan 85% voordat een actie schaal u hebt twee extra exemplaren in plaats van één.

## <a name="scale-based-on-a-schedule"></a>Schaal op basis van een schema


Standaard bij het maken van een regel schaal wordt altijd toegepast. U kunt zien dat wanneer u op de kop van het profiel klikt:

![Profiel](./media/insights-how-to-scale/Insights_Profile.png)

U kunt echter agressiever schalen tijdens de dag of de week, dan in het weekeinde hebben. U kan zelfs uw service geheel uitschakelen werkuren afgesloten.

1. Om dit te doen, op het profiel dat u hebt, selecteert u het **Terugkeerpatroon** **altijd,** en kiest u de gewenste tijden het profiel toe te passen.

2. Bijvoorbeeld, als u een profiel dat in de week past, in de vervolgkeuzelijst **dagen** uncheck **zaterdag** en **zondag**.

3. Als u een profiel dat van toepassing het overdag is, stelt de **Begintijd** en de tijd van de dag die u wilt laten beginnen op.
    ![Standaardterugkeerpatroon](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)

4. Klik op **OK**.

5. Vervolgens moet u het profiel dat u wilt toepassen op andere momenten toevoegen. Klik op de rij **Profiel toevoegen** .
    ![Vrije dagen](./media/insights-how-to-scale/Insights_ProfileOffWork.png)

6. Naam van uw nieuwe, tweede, profiel, bijvoorbeeld kunt u oproepen deze **vrije dagen**.

7. Selecteer vervolgens **Terugkeerpatroon** opnieuw, en kies het exemplaar count bereik u gedurende deze periode.

8. Met het standaardprofiel kiezen de **dagen** wilt u dit profiel wilt toepassen op en de **Begintijd** gedurende de dag.

>[AZURE.NOTE] Automatisch schalen gebruikt regels voor de zomertijd voor de **tijdzone** die u selecteert. Echter tijdens de zomertijd de UTC-afwijking de verschuiving van de tijdzone basis, niet de zomertijd besparingen UTC verschuiving geeft.

9. Klik op **OK**.

10. Nu moet u de regels wilt toepassen tijdens uw tweede profiel toevoegen. Klik op **Regel toevoegen**en vervolgens kunt u dezelfde regel tijdens het standaardprofiel.
    ![Regel uit werk toevoegen](./media/insights-how-to-scale/Insights_RuleOffWork.png)

11. Zorg ervoor dat u de regels voor de schaal van maken en de schaal in hetzij tijdens de telling van het exemplaar van het profiel wordt alleen groeien (of minder).

12. Ten slotte klikt u op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

* [Maatstaven voor monitor-service](insights-how-to-customize-monitoring.md) om te controleren of dat de service beschikbaar is en reageert.
* [Inschakelen, controle en diagnose](insights-how-to-use-diagnostics.md) voor het verzamelen van gedetailleerde statistieken van hoge frequentie op uw service.
* [Meldingen ontvangen](insights-receive-alert-notifications.md) wanneer operationele gebeurtenissen gebeuren of metrics cross een drempel.
* [Prestaties van de toepassing controleren](../application-insights/app-insights-azure-web-apps.md) als u wilt weten precies hoe uw code uitvoeren in de cloud.
* [Gebeurtenissen bekijken en controleren van Logboeken](insights-debugging-with-events.md) voor meer informatie over alles dat is er gebeurd met uw service.
* [Beschikbaarheid van de monitor en de reactiesnelheid van elke webpagina](../application-insights/app-insights-monitor-web-app-availability.md) met toepassing inzichten, zodat u of uw pagina controleren kunt is niet beschikbaar.
