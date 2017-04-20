<properties
    pageTitle="Ontwerpfunctie voor weergaven Analytics melden | Microsoft Azure"
    description="Ontwerpfunctie voor weergaven in Analytics logboek kunt u in de console OMS met verschillende visualisaties van gegevens in de bibliotheek OMS aangepaste weergaven maken. Dit artikel bevat een verwijzing naar de instellingen voor elk van de visualisatie-onderdelen voor gebruik in uw aangepaste weergaven."
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
    ms.date="10/20/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-visualization-part-reference"></a>Logboek Analytics View Designer visualisatie verwijzen
De ontwerpfunctie voor weergaven in Analytics logboek kunt u in de console OMS met verschillende visualisaties van gegevens uit de opslagplaats OMS aangepaste weergaven maken. Dit artikel bevat een verwijzing naar de instellingen voor elk van de visualisatie-onderdelen voor gebruik in uw aangepaste weergaven.

Andere artikelen voor de ontwerpfunctie voor weergaven beschikbaar zijn:

- [Ontwerp weergeven](log-analytics-view-designer.md) - overzicht van de ontwerpfunctie voor weergaven en de procedures voor het maken en bewerken van aangepaste weergaven.
- [Tegel referentie](log-analytics-view-designer-tiles.md) - verwijzing van de instellingen voor elk van de tegels kunnen worden gebruikt in aangepaste weergaven. 

De volgende tabel beschrijft de verschillende soorten tegels in de ontwerpfunctie voor weergaven beschikbaar.  In de volgende secties beschrijven elk type tegel in detail en de bijbehorende eigenschappen.

| Weergavetype | Beschrijving |
|:--|:--|
| [Lijst met query 's](#list-of-queries-part) | Geeft een lijst van logboek zoekquery's.  De gebruiker kan klikken op elke query om de resultaten weer te geven.  |
| [Lijst & nummer](#number-amp-list-part) | Koptekst heeft één getal weergeven het aantal records uit een logboek zoekopdracht.  Lijst staan de top tien resultaten van een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging over tijd. |
| [Twee getallen & lijst](#two-numbers-amp-list-part) | Kop heeft twee getallen tellen van records van een afzonderlijk logboek zoekquery's weergegeven.  Lijst staan de top tien resultaten van een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging over tijd. |
| [Lijst & ring](#donut-amp-list-part) | Kop geeft een enkel getal uit een waardenkolom in een query log samengevat.  De Ring worden de resultaten van de bovenste drie records grafisch weergegeven. |
| [Twee tijdlijnen & list](#two-timelines-amp-list-part) | Koptekst geeft de resultaten van twee query's het logboekbestand na verloop van tijd als kolomdiagrammen met een toelichting met een enkel getal uit een waardenkolom in een query log samengevat.  Lijst staan de top tien resultaten van een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging over tijd. |   
| [Informatie](#information-part) | Koptekst weergegeven statische tekst en een optionele link.  Een of meer items met statische tekst en titel worden hier weergegeven. |
| [Lijndiagram, toelichting & lijst](#line-chart-callout-amp-list-part) | Een lijndiagram met meerdere reeksen uit een query log weergegeven koptekst gedurende een bepaalde tijd een toelichting is met een samenvattingswaarde.  Lijst staan de top tien resultaten van een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging over tijd. |
| [Lijst & lijndiagram](#line-chart-amp-list-part) | Een lijndiagram met meerdere reeksen uit een query log wordt koptekst na verloop van tijd.  Lijst staan de top tien resultaten van een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging over tijd. |
| [Stapel lijn grafieken deel](#stack-of-line-charts-part) | Drie afzonderlijke grafieken met meerdere reeksen uit een query log geeft na verloop van tijd. |


## <a name="list-of-queries-part"></a>Lijst met query's deel

Geeft een lijst van logboek zoekquery's.  De gebruiker kan klikken op elke query om de resultaten weer te geven.  De weergave bevat één query standaard en klikt u op **+ Query** om toe te voegen extra query's.

![Lijst met query's weergeven](media/log-analytics-view-designer/view-list-queries.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemeen** |
| Titel | De tekst moet worden weergegeven aan de bovenkant van de weergave. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Vooraf geselecteerde filters | Door komma's gescheiden lijst met eigenschappen wilt opnemen in het filterdeelvenster links wanneer de gebruiker selecteert in een query. |
| De rendermodus | Oorspronkelijke weergave weergegeven wanneer de query is geselecteerd.  De gebruiker kan alle beschikbare weergaven selecteren na het openen van de query. |
| **Query 's** |
| Zoekopdracht | De query uit te voeren. |
| Beschrijvende naam | Beschrijvende naam van de query weer te geven voor de gebruiker. |


## <a name="number--list-part"></a>Lijst & nummer deel

Koptekst heeft één getal weergeven het aantal records uit een logboek zoekopdracht.  Lijst staan de top tien resultaten van een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging over tijd.


![Lijst met query's weergeven](media/log-analytics-view-designer/view-number-list.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemeen** |
| Titel | De tekst moet worden weergegeven aan de bovenkant van de weergave. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram | Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de koptekst.
| Pictogram gebruiken | Selecteer het pictogram Beeldscherm. |
| **Titel** |
| Legenda | De tekst moet worden weergegeven aan de bovenkant van de koptekst. |
| Query | De query uit te voeren voor de koptekst.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Lijst** |
| Query | De query uit te voeren voor de lijst.  De eerste twee eigenschappen die voor de eerste tien records in de resultaten wordt weergegeven.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap van een numerieke waarde.  Balken worden automatisch gemaakt op basis van de relatieve waarde van een numerieke kolom.<br><br>De opdracht Sort gebruiken in de query de records sorteren in de lijst.  De gebruiker kan klikken Zie alle de query uitvoeren en alle records als resultaat. |
| Grafiek verbergen | Selecteer de grafiek rechts van de numerieke kolom uitschakelen. |
| Sparklines inschakelen | Selecteer sparkline in plaats van een horizontale balk weergegeven.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Kleur | De kleur van de balken of sparklines. |
| Naam & scheidingsteken | Één teken als scheidingsteken als u wilt dat de eigenschap text in meerdere waarden te parseren.  Zie [Gemeenschappelijke instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query | De query moet worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Zie [Gemeenschappelijke instellingen](#navigation-query) voor meer informatie. |
| **Lijst** | **> Kolomtitels** |
| Naam | De tekst moet worden weergegeven aan de bovenkant van de eerste kolom van de lijst. |
| Waarde | De tekst moet worden weergegeven aan de bovenkant van de tweede kolom van de lijst. |
| **Lijst** | **> Drempelwaarden** |
| Drempels inschakelen | Selecteer inschakelen drempels.  Zie [Gemeenschappelijke instellingen](#thresholds) voor meer informatie. |


## <a name="two-numbers--list-part"></a>Twee getallen & lijst deel

Kop heeft twee getallen tellen van records van een afzonderlijk logboek zoekquery's weergegeven.  Lijst staan de top tien resultaten van een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging over tijd.

![Twee getallen & lijstweergave](media/log-analytics-view-designer/view-two-numbers-list.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemeen** |
| Titel | De tekst moet worden weergegeven aan de bovenkant van de weergave. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram | Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de koptekst.
| Pictogram gebruiken | Selecteer het pictogram Beeldscherm. |
| **Titel** |
| Legenda | De tekst moet worden weergegeven aan de bovenkant van de koptekst. |
| Query | De query uit te voeren voor de koptekst.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Lijst** |
| Query | De query uit te voeren voor de lijst.  De eerste twee eigenschappen die voor de eerste tien records in de resultaten wordt weergegeven.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap van een numerieke waarde.  Balken worden automatisch gemaakt op basis van de relatieve waarde van een numerieke kolom.<br><br>De opdracht Sort gebruiken in de query de records sorteren in de lijst.  De gebruiker kan klikken Zie alle de query uitvoeren en alle records als resultaat. |
| Grafiek verbergen | Selecteer de grafiek rechts van de numerieke kolom uitschakelen. |
| Sparklines inschakelen | Selecteer sparkline in plaats van een horizontale balk weergegeven.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Kleur | De kleur van de balken of sparklines. |
| Bewerking | De bewerking uit te voeren voor de sparkline.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Naam & scheidingsteken | Één teken als scheidingsteken als u wilt dat de eigenschap text in meerdere waarden te parseren.  Zie [Gemeenschappelijke instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query | De query moet worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Zie [Gemeenschappelijke instellingen](#navigation-query) voor meer informatie. |
| **Lijst** | **> Kolomtitels** |
| Naam | De tekst moet worden weergegeven aan de bovenkant van de eerste kolom van de lijst. |
| Waarde | De tekst moet worden weergegeven aan de bovenkant van de tweede kolom van de lijst. |
| **Lijst** | **> Drempelwaarden** |
| Drempels inschakelen | Selecteer inschakelen drempels.  Zie [Gemeenschappelijke instellingen](#thresholds) voor meer informatie. |

## <a name="donut--list-part"></a>Lijst & donut deel

Kop geeft een enkel getal uit een waardenkolom in een query log samengevat.  De Ring worden de resultaten van de bovenste drie records grafisch weergegeven.

![Lijst & donut weergeven](media/log-analytics-view-designer/view-donut-list.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemeen** |
| Titel | De tekst moet worden weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram | Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de koptekst. |
| Pictogram gebruiken | Selecteer het pictogram Beeldscherm. |
| **Koptekst** |
| Titel | De tekst moet worden weergegeven aan de bovenkant van de koptekst.
| Ondertitel | De tekst moet worden weergegeven onder de titel aan de bovenkant van de koptekst.
| **Donut** |
| Query | De query uit te voeren voor de ring.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap van een numerieke waarde. |
| **Donut** |  **> Center** |
| Tekst | De tekst moet worden weergegeven onder de waarde in de ring. |
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten in één waarde.<br><br>-Som: De waarden van alle records toevoegen.<br>-Percentage: Het Percentage van de records die worden geretourneerd door de waarden in het **resultaatwaarden gebruikt in midden-bewerking** aan het totaal aantal records in de query. |
| Waarden gebruikt in midden-bewerking | Klik desgewenst op het plusteken om een of meer waarden toevoegen.  De resultaten van de query zal worden beperkt tot de records met de waarden van de eigenschappen die u opgeeft.  Als er geen waarden worden toegevoegd, worden alle records opgenomen in de query. |
| **Extra opties** | **> Kleuren** |
| Kleur 1<br>Kleur 2<br>Kleur 3 | Selecteer de kleur voor elk van de waarden in de ring. |
| **Extra opties** | **> Geavanceerde kleur toewijzen** |
| De waarde van veld | Typ de naam van een veld weer te geven als een andere kleur als deze is opgenomen in de ring. |
| Kleur | Selecteer de kleur voor het unieke veld. |
| **Lijst** |
| Query | De query uit te voeren voor de lijst.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen | Selecteer de grafiek rechts van de numerieke kolom uitschakelen. |
| Sparklines inschakelen | Selecteer sparkline in plaats van een horizontale balk weergegeven.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Kleur | De kleur van de balken of sparklines. |
| Bewerking | De bewerking uit te voeren voor de sparkline.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Naam & scheidingsteken | Één teken als scheidingsteken als u wilt dat de eigenschap text in meerdere waarden te parseren.  Zie [Gemeenschappelijke instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query | De query moet worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Zie [Gemeenschappelijke instellingen](#navigation-query) voor meer informatie. |
| **Lijst** | **> Kolomtitels** |
| Naam | De tekst moet worden weergegeven aan de bovenkant van de eerste kolom van de lijst. |
| Waarde | De tekst moet worden weergegeven aan de bovenkant van de tweede kolom van de lijst. |
| **Lijst** | **> Drempelwaarden** |
| Drempels inschakelen | Selecteer inschakelen drempels.  Zie [Gemeenschappelijke instellingen](#thresholds) voor meer informatie. |

## <a name="two-timelines--list-part"></a>Twee tijdlijnen & list deel

Koptekst geeft de resultaten van twee query's het logboekbestand na verloop van tijd als kolomdiagrammen met een toelichting met een enkel getal uit een waardenkolom in een query log samengevat.  Lijst staan de top tien resultaten van een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging over tijd.

![Twee tijdlijnen & lijst weergeven](media/log-analytics-view-designer/view-two-timelines-list.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemeen** |
| Titel | De tekst moet worden weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram | Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de koptekst. |
| Pictogram gebruiken | Selecteer het pictogram Beeldscherm. |
| **Grafiek eerst<br>tweede grafiek** |
| Legenda | De tekst moet worden weergegeven in de toelichting voor de eerste reeks. |
| Kleur | Kleur voor de kolommen in de reeks. |
| Query | De query uit te voeren voor de eerste reeks.  Het aantal records over elk tijdsinterval wordt vertegenwoordigd door de kolommen van de grafiek. |
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten in één waarde voor de toelichting.<br><br>-Totaal: Som van de waarde van alle records.<br>-Gemiddelde: Het gemiddelde van de waarde van alle records.<br>-Het laatste voorbeeld: De waarde uit het laatste interval dat is opgenomen in de grafiek.<br>-Het eerste voorbeeld: De waarde uit het eerste interval in de grafiek opgenomen.<br>-Aantal: Aantal records geretourneerd door de query.|
| **Lijst** |
| Query | De query uit te voeren voor de lijst.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen | Selecteer de grafiek rechts van de numerieke kolom uitschakelen. |
| Sparklines inschakelen | Selecteer sparkline in plaats van een horizontale balk weergegeven.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Kleur | De kleur van de balken of sparklines. |
| Bewerking | De bewerking uit te voeren voor de sparkline.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Navigatie-query | De query moet worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Zie [Gemeenschappelijke instellingen](#navigation-query) voor meer informatie. |
| **Lijst** | **> Kolomtitels** |
| Naam | De tekst moet worden weergegeven aan de bovenkant van de eerste kolom van de lijst. |
| Waarde | De tekst moet worden weergegeven aan de bovenkant van de tweede kolom van de lijst. |
| **Lijst** | **> Drempelwaarden** |
| Drempels inschakelen | Selecteer inschakelen drempels.  Zie [Gemeenschappelijke instellingen](#thresholds) voor meer informatie. |

## <a name="information-part"></a>Deel informatie

Koptekst weergegeven statische tekst en een optionele link.  Een of meer items met statische tekst en titel worden hier weergegeven.

![Informatie weergeven](media/log-analytics-view-designer/view-information.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemeen** |
| Titel | De tekst moet worden weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Kleur | De achtergrondkleur voor de koptekst. |
| **Koptekst** |
| Afbeelding | Image-bestand weergeven in de koptekst. |
| Label | De tekst moet worden weergegeven in de koptekst. |
| **Koptekst** | **> Koppeling** |
| Label | De tekst van de koppeling. |
| URL | URL voor koppeling. |
| **Informatie-Items** |
| Titel | De tekst voor de titel van elk artikel weergegeven. |
| Inhoud | De tekst moet worden weergegeven voor elk item. |


## <a name="line-chart-callout--list-part"></a>Lijndiagram, toelichting & lijst deel

Een lijndiagram met meerdere reeksen uit een query log weergegeven koptekst gedurende een bepaalde tijd een toelichting is met een samenvattingswaarde.  Lijst staan de top tien resultaten van een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging over tijd.

![Lijndiagram, toelichting & lijstweergave](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemeen** |
| Titel | De tekst moet worden weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram | Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de koptekst. |
| Pictogram gebruiken | Selecteer het pictogram Beeldscherm. |
| **Koptekst** |
| Titel | De tekst moet worden weergegeven aan de bovenkant van de koptekst. |
| Ondertitel | De tekst moet worden weergegeven onder de titel aan de bovenkant van de koptekst. |
| **Lijndiagram** |
| Query | De query uit te voeren voor het lijndiagram.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap van een numerieke waarde.  Dit is meestal een query die resultaten worden samengevat met het sleutelwoord **maatregel** .  Als de query wordt gebruikt voor het **interval** trefwoord gebruik de x-as van de grafiek dit tijdsinterval.  Als de query niet het sleutelwoord **interval** worden per uur intervallen gebruikt voor de x-as. |
| **Lijndiagram** | **> Toelichting** |
| Titel van de toelichting | Tekst boven de waarde van het bijschrift weergegeven. |
| Reeksnaam | De waarde van de eigenschap voor de reeks die u wilt gebruiken voor de waarde van de toelichting.  Als er geen reeks is opgegeven, worden alle records van de query worden gebruikt. |
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten in één waarde voor de toelichting.<br><br>-Gemiddelde: Het gemiddelde van de waarde van alle records.<br>-Count het aantal records geretourneerd door de query.<br>-Het laatste voorbeeld: De waarde uit het laatste interval dat is opgenomen in de grafiek.<br>-Max: De maximumwaarde van de intervallen in de grafiek opgenomen.<br>-Min: De minimumwaarde van de intervallen in de grafiek opgenomen.<br>-Totaal: Som van de waarde van alle records. |
| **Lijndiagram** | **> Y-as** |
| Logaritmische schaal | Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden | Geef de eenheden voor de waarden die worden geretourneerd door de query.  Deze informatie wordt gebruikt voor labels in de grafiek geven de typen en eventueel voor het omzetten van de waarden worden weergegeven.  Het eenheidstype geeft de categorie van de eenheid en definieert het Type van de huidige eenheid waarden die beschikbaar zijn.  Als u een waarde in omzetten en vervolgens de numerieke waarden van het type van de huidige eenheid worden geconverteerd naar het omzetten te typen. |
| Aangepast Label | De tekst moet worden weergegeven voor de Y-as naast de label voor het eenheidstype.  Als u geen naam opgeeft, wordt alleen het eenheidstype weergegeven. |
| **Lijst** |
| Query | De query uit te voeren voor de lijst.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen | Selecteer de grafiek rechts van de numerieke kolom uitschakelen. |
| Sparklines inschakelen | Selecteer sparkline in plaats van een horizontale balk weergegeven.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Kleur | De kleur van de balken of sparklines. |
| Bewerking | De bewerking uit te voeren voor de sparkline.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Naam & scheidingsteken | Één teken als scheidingsteken als u wilt dat de eigenschap text in meerdere waarden te parseren.  Zie [Gemeenschappelijke instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query | De query moet worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Zie [Gemeenschappelijke instellingen](#navigation-query) voor meer informatie. |
| **Lijst** | **> Kolomtitels** |
| Naam | De tekst moet worden weergegeven aan de bovenkant van de eerste kolom van de lijst. |
| Waarde | De tekst moet worden weergegeven aan de bovenkant van de tweede kolom van de lijst. |
| **Lijst** | **> Drempelwaarden** |
| Drempels inschakelen | Selecteer inschakelen drempels.  Zie [Gemeenschappelijke instellingen](#thresholds) voor meer informatie. |

## <a name="line-chart--list-part"></a>Grafiek & lijst deel van lijn

Een lijndiagram met meerdere reeksen uit een query log wordt koptekst na verloop van tijd.  Lijst staan de top tien resultaten van een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging over tijd.

![Grafiek & lijst regelweergave](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemeen** |
| Titel | De tekst moet worden weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram | Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de koptekst. |
| Pictogram gebruiken | Selecteer het pictogram Beeldscherm. |
| **Koptekst** |
| Titel | De tekst moet worden weergegeven aan de bovenkant van de koptekst. |
| Ondertitel | De tekst moet worden weergegeven onder de titel aan de bovenkant van de koptekst. |
| **Lijndiagram** |
| Query | De query uit te voeren voor het lijndiagram.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap van een numerieke waarde.  Dit is meestal een query die resultaten worden samengevat met het sleutelwoord **maatregel** .  Als de query wordt gebruikt voor het **interval** trefwoord gebruik de x-as van de grafiek dit tijdsinterval.  Als de query niet het sleutelwoord **interval** worden per uur intervallen gebruikt voor de x-as. |
| **Lijndiagram** | **> Y-as** |
| Logaritmische schaal | Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden | Geef de eenheden voor de waarden die worden geretourneerd door de query.  Deze informatie wordt gebruikt voor labels in de grafiek geven de typen en eventueel voor het omzetten van de waarden worden weergegeven.  Het eenheidstype geeft de categorie van de eenheid en definieert het Type van de huidige eenheid waarden die beschikbaar zijn.  Als u een waarde in omzetten en vervolgens de numerieke waarden van het type van de huidige eenheid worden geconverteerd naar het omzetten te typen. |
| Aangepast Label | De tekst moet worden weergegeven voor de Y-as naast de label voor het eenheidstype.  Als u geen naam opgeeft, wordt alleen het eenheidstype weergegeven. |
| **Lijst** |
| Query | De query uit te voeren voor de lijst.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen | Selecteer de grafiek rechts van de numerieke kolom uitschakelen. |
| Sparklines inschakelen | Selecteer sparkline in plaats van een horizontale balk weergegeven.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Kleur | De kleur van de balken of sparklines. |
| Bewerking | De bewerking uit te voeren voor de sparkline.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Naam & scheidingsteken | Één teken als scheidingsteken als u wilt dat de eigenschap text in meerdere waarden te parseren.  Zie [Gemeenschappelijke instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query | De query moet worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Zie [Gemeenschappelijke instellingen](#navigation-query) voor meer informatie. |
| **Lijst** | **> Kolomtitels** |
| Naam | De tekst moet worden weergegeven aan de bovenkant van de eerste kolom van de lijst. |
| Waarde | De tekst moet worden weergegeven aan de bovenkant van de tweede kolom van de lijst. |
| **Lijst** | **> Drempelwaarden** |
| Drempels inschakelen | Selecteer inschakelen drempels.  Zie [Gemeenschappelijke instellingen](#thresholds) voor meer informatie. |

## <a name="stack-of-line-charts-part"></a>Stapel lijn grafieken deel

Drie afzonderlijke grafieken met meerdere reeksen uit een query log geeft na verloop van tijd.

![Stapel lijndiagrammen](media/log-analytics-view-designer/view-stack-line-charts.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemeen** |
| Titel | De tekst moet worden weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram | Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de koptekst. |
| **Grafiek 1<br>grafiek 2<br>3 grafiek** | **> Kop** |
| Titel | De tekst moet worden weergegeven boven aan de grafiek. |
| Ondertitel | De tekst moet worden weergegeven onder de titel boven aan de grafiek. |
| **Grafiek 1<br>grafiek 2<br>3 grafiek** | **Lijndiagram** |
| Query | De query uit te voeren voor het lijndiagram.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap van een numerieke waarde.  Dit is meestal een query die resultaten worden samengevat met het sleutelwoord **maatregel** .  Als de query wordt gebruikt voor het **interval** trefwoord gebruik de x-as van de grafiek dit tijdsinterval.  Als de query niet het sleutelwoord **interval** worden per uur intervallen gebruikt voor de x-as. |
| **Grafiek** | **> Y-as** |
| Logaritmische schaal | Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden | Geef de eenheden voor de waarden die worden geretourneerd door de query.  Deze informatie wordt gebruikt voor labels in de grafiek geven de typen en eventueel voor het omzetten van de waarden worden weergegeven.  Het eenheidstype geeft de categorie van de eenheid en definieert het Type van de huidige eenheid waarden die beschikbaar zijn.  Als u een waarde in omzetten en vervolgens de numerieke waarden van het type van de huidige eenheid worden geconverteerd naar het omzetten te typen. |
| Aangepast Label | De tekst moet worden weergegeven voor de Y-as naast de label voor het eenheidstype.  Als u geen naam opgeeft, wordt alleen het eenheidstype weergegeven. |

## <a name="common-settings"></a>Algemene instellingen
De volgende secties worden de instellingen voor verschillende onderdelen van de visualisatie.

### <a name="name-value-separator">Naam & scheidingsteken</a>
Één teken als scheidingsteken als u wilt dat de eigenschap text van een met lijstquery in meerdere waarden te parseren.  Als u een scheidingsteken opgeeft, kunt u de namen voor elk veld gescheiden door hetzelfde scheidingsteken wordt gebruikt in het vak naam opgeven.

Neem bijvoorbeeld een eigenschap met de naam van de *locatie* die u als *Redmond gebouw 41* *Bellevue Building12*opgenomen.  U kunt aangeven – voor de naam & scheidingsteken en de naam van de *Stad gebouw* .  Dit zou elke waarde verdelen in twee eigenschappen, *stad* en *gebouw*genoemd. 

### <a name="navigation-query">Navigatie-query</a>
De query moet worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  *{Het geselecteerde item}* gebruiken ook de syntaxis voor het item dat de gebruiker heeft geselecteerd.

Als de query een kolom met de naam van de *Computer* en de navigatie-query bevat is bijvoorbeeld *{geselecteerd item}*, een query, zoals *Computer = "DezeComputer"* zou worden uitgevoerd wanneer de gebruiker een computer geselecteerd.  Als de query navigatie *Type gebeurtenis {geselecteerd item} =* vervolgens de query *Type = Computer gebeurtenis = "DezeComputer"* zou worden uitgevoerd.

### <a name="sparklines">Sparklines</a>
Een sparkline is een kleine lijngrafiek ziet u de waarde van een item uit de lijst in de tijd.  Voor visualisatie delen met een lijst, kunt u selecteren of een horizontale balk aangeeft van de relatieve waarde van een numerieke kolom of een sparkline die de waarde aangeeft die na verloop van tijd wordt weergegeven.

De volgende tabel worden de instellingen voor sparkline beschreven.

| Instelling | Beschrijving |
|:--|:--|
| Sparklines inschakelen | Selecteer sparkline in plaats van een horizontale balk weergegeven. |
| Bewerking | Als sparklines zijn ingeschakeld, is dit de bewerking uit te voeren op elke eigenschap in de lijst voor het berekenen van de waarden voor de sparkline.<br><br>-Het laatste voorbeeld: De laatste waarde voor de reeks over het tijdsinterval.<br>-Max: Maximale waarde voor de reeks over het tijdsinterval.<br>-Min: Minimale waarde voor de reeks over het tijdsinterval.<br>-Totaal: Som van de waarden voor de reeks over het tijdsinterval.<br>-Samenvatting: Dezelfde maatregel opdracht gebruikt als de query in de kop. |

### <a name="thresholds">Drempels</a>
Drempels, kunt u een gekleurd pictogram naast elk item weergegeven in een lijst waarin u een snelle visuele indicator van items die groter zijn dan een bepaalde waarde of binnen een bepaald bereik vallen.  U kan bijvoorbeeld een groen pictogram voor artikelen met een aanvaardbare waarde, rood en geel als de waarde binnen een bereik dat een waarschuwing geeft weergeven als deze groter is dan een foutwaarde.

Wanneer u de drempels voor een onderdeel inschakelt, moet u een of meer drempels.  Als de waarde van een item groter dan de drempelwaarde en lager is dan de volgende drempelwaarde is, wordt deze kleur gebruikt.  Als het item groter dan vervolgens hoogste drempelwaarde is, wordt deze kleur ingesteld.   

Elke set drempel heeft een drempel met een waarde van **standaard**.  Dit is de kleur die is ingesteld als geen andere waarden worden overschreden.  U kunt toevoegen of verwijderen van drempels door te klikken op de **+** of de **x** -knop.

De volgende tabel worden de instellingen voor tresholds.

| Instelling | Beschrijving |
|:--|:--|
| Drempels inschakelen | Selecteer een Kleurenpictogram wordt weergegeven links van elke waarde die de gezondheid ten opzichte van de opgegeven drempelwaarden aangeeft. |
| Naam | De naam voor de drempelwaarde. |
| Drempel | De waarde voor de drempelwaarde.  De kleur van de gezondheid van elk item wordt ingesteld op de kleur van de hoogste waarde voor drempel overschreden door de waarde van het artikel.  Er is een standaard-drempel die de kleur is als er geen drempelwaarde wordt overschreden. |
| Kleur | Kleur voor de drempelwaarde. |


## <a name="next-steps"></a>Volgende stappen

- Informatie over het [logboek zoeken](log-analytics-log-searches.md) ter ondersteuning van de query's in de visualisatie delen.