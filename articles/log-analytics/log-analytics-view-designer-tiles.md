<properties
    pageTitle="Verwijzing naar Analytics weergave Designer tegel melden | Microsoft Azure"
    description="Ontwerpfunctie voor weergaven in Analytics logboek kunt u in de console OMS met verschillende visualisaties van gegevens in de bibliotheek OMS aangepaste weergaven maken. Dit artikel bevat een verwijzing naar de instellingen voor elk van de tegels kunnen worden gebruikt in aangepaste weergaven."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-tile-reference"></a>Verwijzing naar het logboek Analytics weergave Designer tegel
De ontwerpfunctie voor weergaven in Analytics logboek kunt u in de console OMS met verschillende visualisaties van gegevens in de bibliotheek OMS aangepaste weergaven maken. Dit artikel bevat een verwijzing naar de instellingen voor elk van de tegels kunnen worden gebruikt in aangepaste weergaven.

Andere artikelen voor de ontwerpfunctie voor weergaven beschikbaar zijn:

- [Ontwerp weergeven](log-analytics-view-designer.md) - overzicht van de ontwerpfunctie voor weergaven en de procedures voor het maken en bewerken van aangepaste weergaven.
- [Visualisatie verwijzen](log-analytics-view-designer-parts.md) - verwijzing van de instellingen voor elk van de tegels kunnen worden gebruikt in aangepaste weergaven. 


De volgende tabel worden de verschillende soorten tegels in de ontwerpfunctie voor weergaven beschikbaar.  In de volgende secties beschrijven elk type tegel in detail en de bijbehorende eigenschappen.

| Naast elkaar | Beschrijving |
|:--|:--|
| [Nummer](#number-tile) | Enkel getal met het aantal records uit een query. |
| [Twee getallen](#two-numbers-tile) | Één twee getallen telt het aantal records uit twee afzonderlijke query's weergeven. |
| [Donut](#donut-tile) | Cirkelgrafiek op basis van een query met een totale waarde in het midden. |
| [Lijndiagram & toelichting](#line-chart-amp-callout-tile) | Lijndiagram op basis van een query en een toelichting is met een totale waarde. |
| [Lijndiagram](#line-chart-tile) | Lijndiagram op basis van een query. |
| [Twee tijdlijnen](#two-timelines-tile) | Kolomdiagram met twee reeksen die elkaar op basis van een andere query. |



## <a name="number-tile"></a>Aantal tegels

Een enkel getal dat het aantal records uit een query log en een label met weergeven het **nummer** naast elkaar

![Aantal tegels](media/log-analytics-view-designer/tile-number.png)

| Instelling | Beschrijving |
|:--|:--|
| Naam        | De tekst moet worden weergegeven aan de bovenkant van de tegel. |
| Beschrijving | De tekst moet worden weergegeven onder de naam van de tegel.    |
| **Naast elkaar** |
| Legenda | De tekst moet worden weergegeven onder de waarde. |
| Query | De query uit te voeren.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Geavanceerde** |  **> Gegevensstroom controle** |
| Ingeschakeld | Selecteer als controle gegevensstroom moet worden ingeschakeld voor de tegel.  Dit biedt een alternatieve weergegeven als gegevens niet beschikbaar voor de tegel is.  Dit wordt meestal gebruikt om een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en gegevens beschikbaar komt. |
| Query | Een query uitvoeren om te controleren of gegevens beschikbaar voor de weergave.  Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de hoofdquery. |
| Bericht | Bericht dat wordt weergegeven als de gegevensstroom verificatie geen gegevens geretourneerd.  Als u geen bericht opgeeft, wordt de *Beoordeling uitvoeren* weergegeven. |
| **Tijdsinterval** |
| Duur | De duur van de huidige datum moet worden gebruikt voor het interval van de query.  Bijvoorbeeld als **7 dagen** is opgegeven, wordt de query beperkt tot records die zijn gemaakt op basis van 7 dagen geleden op de huidige datum. |
| Einde gegevens-offset | Optionele verschuiving ten opzichte van de huidige gegevens gebruikt voor het interval van de hoofdquery.  Bijvoorbeeld, als **-1 dag** wordt gebruikt voor de **verschuiving van de datum einde** en **7 dagen** tijdens de **duur**, wordt de query beperkt tot records die zijn gemaakt op basis van 8 dagen geleden op gisteren. |

## <a name="two-numbers-tile"></a>Twee nummers naast elkaar

De **Nummer twee** naast elkaar worden twee getallen met het aantal records uit twee verschillende log-query's en een label voor elk.

![Twee nummers naast elkaar](media/log-analytics-view-designer/tile-two-numbers.png)

| Instelling | Beschrijving |
|:--|:--|
| Naam        | De tekst moet worden weergegeven aan de bovenkant van de tegel. |
| Beschrijving | De tekst moet worden weergegeven onder de naam van de tegel.    |
| **Eerste tegel** |
| Legenda | De tekst moet worden weergegeven onder de waarde. |
| Query | De query uit te voeren.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Tweede deel** |
| Legenda | De tekst moet worden weergegeven onder de waarde. |
| Query | De query uit te voeren.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Geavanceerde** | **> Gegevensstroom controle** |
| Ingeschakeld | Selecteer als controle gegevensstroom moet worden ingeschakeld voor de tegel.  Dit biedt een alternatieve weergegeven als gegevens niet beschikbaar voor de tegel is.  Dit wordt meestal gebruikt om een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en gegevens beschikbaar komt. |
| Query | Een query uitvoeren om te controleren of gegevens beschikbaar voor de weergave.  Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de hoofdquery. |
| Bericht | Bericht dat wordt weergegeven als de gegevensstroom verificatie geen gegevens geretourneerd.  Als u geen bericht opgeeft, wordt de *Beoordeling uitvoeren* weergegeven. |
| **Tijdsinterval** |
| Duur | De duur van de huidige datum moet worden gebruikt voor het interval van de query.  Bijvoorbeeld als **7 dagen** is opgegeven, wordt de query beperkt tot records die zijn gemaakt op basis van 7 dagen geleden op de huidige datum. |
| Einde gegevens-offset | Optionele verschuiving ten opzichte van de huidige gegevens gebruikt voor het interval van de hoofdquery.  Bijvoorbeeld, als **-1 dag** wordt gebruikt voor de **verschuiving van de datum einde** en **7 dagen** tijdens de **duur**, wordt de query beperkt tot records die zijn gemaakt op basis van 8 dagen geleden op gisteren. |

## <a name="donut-tile"></a>Donut tegel

De tegel **Ring** geeft een enkel getal uit een waardenkolom in een query log samengevat.  De Ring worden de resultaten van de bovenste drie records grafisch weergegeven.

![Donut tegel](media/log-analytics-view-designer/tile-donut.png)

| Instelling | Beschrijving |
|:--|:--|
| Naam        | De tekst moet worden weergegeven aan de bovenkant van de tegel. |
| Beschrijving | De tekst moet worden weergegeven onder de naam van de tegel.    |
| **Donut** |
| Query | De query uit te voeren voor de ring.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap van een numerieke waarde.  Dit is meestal een query die resultaten worden samengevat met het sleutelwoord **maatregel** . |
| **Donut** | **> Center** |
| Tekst | De tekst moet worden weergegeven onder de waarde in de ring. |
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten in één waarde.<br><br>-Som: De waarden van alle records met de waarde van de eigenschap toevoegen.<br>-Percentage: Het Percentage van de opgetelde waarden van de records met de waarde van de eigenschap ten opzichte van de opgetelde waarden van alle records. |
| Waarden gebruikt in midden-bewerking | Klik desgewenst op het plusteken om een of meer waarden toevoegen.  De resultaten van de query zal worden beperkt tot de records met de waarden van de eigenschappen die u opgeeft.  Als er geen waarden worden toegevoegd dan alle records in de query zijn opgenomen. |
| **Donut** | **> Extra opties** |
| Kleuren | De kleur voor elk van de drie belangrijkste eigenschappen.  Als u andere kleuren opgeven voor specifieke waarden van eigenschappen, gebruiken geavanceerde kleur toewijzen. |
| Geavanceerde kleur toewijzen | Een kleur voor de specifieke waarden van eigenschappen weergegeven.  Als de waarde die u opgeeft in de bovenste drie is, wordt de alternatieve kleur weergegeven in plaats van de standaard kleur.  Als de eigenschap niet in de bovenste drie, wordt de kleur niet wordt weergegeven. |
| **Geavanceerde** | **> Gegevensstroom controle** |
| Ingeschakeld | Selecteer als controle gegevensstroom moet worden ingeschakeld voor de tegel.  Dit biedt een alternatieve weergegeven als gegevens niet beschikbaar voor de tegel is.  Dit wordt meestal gebruikt om een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en gegevens beschikbaar komt. |
| Query | Een query uitvoeren om te controleren of gegevens beschikbaar voor de weergave.  Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de hoofdquery. |
| Bericht | Bericht dat wordt weergegeven als de gegevensstroom verificatie geen gegevens geretourneerd.  Als u geen bericht opgeeft, wordt de *Beoordeling uitvoeren* weergegeven. |
| **Tijdsinterval** |
| Duur | De duur van de huidige datum moet worden gebruikt voor het interval van de query.  Bijvoorbeeld als **7 dagen** is opgegeven, wordt de query beperkt tot records die zijn gemaakt op basis van 7 dagen geleden op de huidige datum. |
| Einde gegevens-offset | Optionele verschuiving ten opzichte van de huidige gegevens gebruikt voor het interval van de hoofdquery.  Bijvoorbeeld, als **-1 dag** wordt gebruikt voor de **verschuiving van de datum einde** en **7 dagen** tijdens de **duur**, wordt de query beperkt tot records die zijn gemaakt op basis van 8 dagen geleden op gisteren. |

## <a name="line-chart-tile"></a>Lijn grafiek tegel

Een lijndiagram met meerdere reeksen uit een query log wordt de tegel **lijndiagram** na verloop van tijd.  

![Lijndiagram & toelichting tegel](media/log-analytics-view-designer/tile-line-chart.png)

| Instelling | Beschrijving |
|:--|:--|
| Naam        | De tekst moet worden weergegeven aan de bovenkant van de tegel. |
| Beschrijving | De tekst moet worden weergegeven onder de naam van de tegel.    |
| **Lijndiagram** |  
| Query | De query uit te voeren voor het lijndiagram.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap van een numerieke waarde.  Dit is meestal een query die resultaten worden samengevat met het sleutelwoord **maatregel** .  Als de query wordt gebruikt voor het **interval** trefwoord gebruik de x-as van de grafiek dit tijdsinterval.  Als de query niet het sleutelwoord **interval** worden per uur intervallen gebruikt voor de x-as. |
| **Lijndiagram** | **> Y-as** |
| Logaritmische schaal | Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden | Geef de eenheden voor de waarden die worden geretourneerd door de query.  Deze informatie wordt gebruikt voor labels in de grafiek geven de typen en eventueel voor het omzetten van de waarden worden weergegeven.  Het **Type eenheid** geeft de categorie van de eenheid en definieert het **Type van de huidige eenheid** waarden die beschikbaar zijn.  Als u een waarde in het **converteren naar selecteert** worden vervolgens numerieke waarden geconverteerd van de **Huidige Unit** type **converteren naar** type. |
| Aangepast Label | De tekst moet worden weergegeven voor de Y-as naast de label voor het eenheidstype.  Als u geen naam opgeeft, wordt alleen het eenheidstype weergegeven. |
| **Geavanceerde** | **> Gegevensstroom controle** |
| Ingeschakeld | Selecteer als controle gegevensstroom moet worden ingeschakeld voor de tegel.  Dit biedt een alternatieve weergegeven als gegevens niet beschikbaar voor de tegel is.  Dit wordt meestal gebruikt om een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en gegevens beschikbaar komt. |
| Query | Een query uitvoeren om te controleren of gegevens beschikbaar voor de weergave.  Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de hoofdquery. |
| Bericht | Bericht dat wordt weergegeven als de gegevensstroom verificatie geen gegevens geretourneerd.  Als u geen bericht opgeeft, wordt de *Beoordeling uitvoeren* weergegeven. |
| **Tijdsinterval** |
| Duur | De duur van de huidige datum moet worden gebruikt voor het interval van de query.  Bijvoorbeeld als **7 dagen** is opgegeven, wordt de query beperkt tot records die zijn gemaakt op basis van 7 dagen geleden op de huidige datum. |
| Einde gegevens-offset | Optionele verschuiving ten opzichte van de huidige gegevens gebruikt voor het interval van de hoofdquery.  Bijvoorbeeld, als **-1 dag** wordt gebruikt voor de **verschuiving van de datum einde** en **7 dagen** tijdens de **duur**, wordt de query beperkt tot records die zijn gemaakt op basis van 8 dagen geleden op gisteren. |


## <a name="line-chart--callout-tile"></a>Lijn grafiek & toelichting tegel

De tegel **lijndiagram & toelichting** geeft een lijndiagram met meerdere reeksen uit een query log gedurende een bepaalde tijd een toelichting is met een samenvattingswaarde.  

![Lijndiagram & toelichting tegel](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Instelling | Beschrijving |
|:--|:--|
| Naam        | De tekst moet worden weergegeven aan de bovenkant van de tegel. |
| Beschrijving | De tekst moet worden weergegeven onder de naam van de tegel.    |
| **Lijndiagram** |  
| Query | De query uit te voeren voor het lijndiagram.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap van een numerieke waarde.  Dit is meestal een query die resultaten worden samengevat met het sleutelwoord **maatregel** .  Als de query wordt gebruikt voor het **interval** trefwoord gebruik de x-as van de grafiek dit tijdsinterval.  Als de query niet het sleutelwoord **interval** worden per uur intervallen gebruikt voor de x-as. |
| **Lijndiagram** | **> Toelichting** |
| Toelichting | De tekst boven de waarde van het bijschrift weergegeven. |
| Reeksnaam | De waarde van de eigenschap voor de reeks die u wilt gebruiken voor de waarde van de toelichting.  Als er geen reeks is opgegeven, worden alle records van de query worden gebruikt. |
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten in één waarde voor de toelichting.<br>-Gemiddelde: Het gemiddelde van de waarde van alle records.<br><br>-Aantal: Aantal records geretourneerd door de query.<br>-Het laatste voorbeeld: De waarde uit het laatste interval dat is opgenomen in de grafiek.<br>-Max: De maximumwaarde van de intervallen in de grafiek opgenomen.<br>-Min: De minimumwaarde van de intervallen in de grafiek opgenomen.<br>-Totaal: Som van de waarde van alle records. |
| **Lijndiagram** | **> Y-as** |
| Logaritmische schaal | Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden | Geef de eenheden voor de waarden die worden geretourneerd door de query.  Deze informatie wordt gebruikt voor labels in de grafiek geven de typen en eventueel voor het omzetten van de waarden worden weergegeven.  Het **Type eenheid** geeft de categorie van de eenheid en definieert het **Type van de huidige eenheid** waarden die beschikbaar zijn.  Als u een waarde in het **converteren naar selecteert** worden vervolgens numerieke waarden geconverteerd van de **Huidige Unit** type **converteren naar** type. |
| Aangepast Label | De tekst moet worden weergegeven voor de Y-as naast de label voor het eenheidstype.  Als u geen naam opgeeft, wordt alleen het eenheidstype weergegeven. |
| **Geavanceerde** | **> Gegevensstroom controle** |
| Ingeschakeld | Selecteer als controle gegevensstroom moet worden ingeschakeld voor de tegel.  Dit biedt een alternatieve weergegeven als gegevens niet beschikbaar voor de tegel is.  Dit wordt meestal gebruikt om een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en gegevens beschikbaar komt. |
| Query | Een query uitvoeren om te controleren of gegevens beschikbaar voor de weergave.  Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de hoofdquery. |
| Bericht | Bericht dat wordt weergegeven als de gegevensstroom verificatie geen gegevens geretourneerd.  Als u geen bericht opgeeft, wordt de *Beoordeling uitvoeren* weergegeven. |
| **Tijdsinterval** |
| Duur | De duur van de huidige datum moet worden gebruikt voor het interval van de query.  Bijvoorbeeld als **7 dagen** is opgegeven, wordt de query beperkt tot records die zijn gemaakt op basis van 7 dagen geleden op de huidige datum. |
| Einde gegevens-offset | Optionele verschuiving ten opzichte van de huidige gegevens gebruikt voor het interval van de hoofdquery.  Bijvoorbeeld, als **-1 dag** wordt gebruikt voor de **verschuiving van de datum einde** en **7 dagen** tijdens de **duur**, wordt de query beperkt tot records die zijn gemaakt op basis van 8 dagen geleden op gisteren. |

## <a name="two-timelines-tile"></a>Naast elkaar twee tijdlijnen

De **twee tijdlijnen** tegel geeft de resultaten van twee query's het logboekbestand na verloop van tijd als kolomdiagrammen.  Een bijschrift wordt weergegeven voor elke reeks.  

![Naast elkaar twee tijdlijnen](media/log-analytics-view-designer/tile-two-timelines.png)

| Instelling | Beschrijving |
|:--|:--|
| Naam        | De tekst moet worden weergegeven aan de bovenkant van de tegel. |
| Beschrijving | De tekst moet worden weergegeven onder de naam van de tegel.    |
| Eerste grafiek   
| Legenda | De tekst moet worden weergegeven in de toelichting voor de eerste reeks.
| Kleur | Kleur voor de kolommen in de eerste reeks.
| Grafiek Query | De query uit te voeren voor de eerste reeks.  Het aantal records over elk tijdsinterval wordt vertegenwoordigd door de kolommen van de grafiek.
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten in één waarde voor de toelichting.<br><br>-Gemiddelde: Het gemiddelde van de waarde van alle records.<br>-Aantal: Aantal records geretourneerd door de query.<br>-Het laatste voorbeeld: De waarde uit het laatste interval dat is opgenomen in de grafiek.<br>-Max: De maximumwaarde van de intervallen in de grafiek opgenomen.
| **Tweede grafiek** |
| Legenda | De tekst moet worden weergegeven in de toelichting voor de tweede reeks.
| Kleur | Kleur voor de kolommen in de tweede reeks.
| Grafiek Query | De query uit te voeren voor de tweede reeks.  Het aantal records over elk tijdsinterval wordt vertegenwoordigd door de kolommen van de grafiek.
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten in één waarde voor de toelichting.<br><br>-Gemiddelde: Het gemiddelde van de waarde van alle records.<br>-Aantal: Aantal records geretourneerd door de query.<br>-Het laatste voorbeeld: De waarde uit het laatste interval dat is opgenomen in de grafiek.<br>-Max: De maximumwaarde van de intervallen in de grafiek opgenomen. |
| **Geavanceerde** | **> Gegevensstroom controle** |
| Ingeschakeld | Selecteer als controle gegevensstroom moet worden ingeschakeld voor de tegel.  Dit biedt een alternatieve weergegeven als gegevens niet beschikbaar voor de tegel is.  Dit wordt meestal gebruikt om een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en gegevens beschikbaar komt. |
| Query | Een query uitvoeren om te controleren of gegevens beschikbaar voor de weergave.  Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de hoofdquery. |
| Bericht | Bericht dat wordt weergegeven als de gegevensstroom verificatie geen gegevens geretourneerd.  Als u geen bericht opgeeft, wordt de *Beoordeling uitvoeren* weergegeven. |
| **Tijdsinterval** |
| Duur | De duur van de huidige datum moet worden gebruikt voor het interval van de query.  Bijvoorbeeld als **7 dagen** is opgegeven, wordt de query beperkt tot records die zijn gemaakt op basis van 7 dagen geleden op de huidige datum. |
| Einde gegevens-offset | Optionele verschuiving ten opzichte van de huidige gegevens gebruikt voor het interval van de hoofdquery.  Bijvoorbeeld, als **-1 dag** wordt gebruikt voor de **verschuiving van de datum einde** en **7 dagen** tijdens de **duur**, wordt de query beperkt tot records die zijn gemaakt op basis van 8 dagen geleden op gisteren. |

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [logboek zoeken](log-analytics-log-searches.md) ter ondersteuning van de query's in tegels.
- [Visualisatie onderdelen](log-analytics-view-designer-parts.md) toevoegen aan de aangepaste weergave.