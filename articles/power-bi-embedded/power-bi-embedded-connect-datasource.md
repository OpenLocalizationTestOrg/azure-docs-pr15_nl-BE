<properties
   pageTitle="De ingesloten Microsoft Power BI - verbinding te maken met een gegevensbron"
   description="Energie-BI ingesloten, verbinding maken met gegevensbronnen"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="connect-to-a-data-source"></a>Verbinding maken met een gegevensbron

Met **Power BI ingesloten**, kunt u rapporten insluiten in uw eigen app. Wanneer u een rapport Power BI in uw toepassing insluit, het rapport maakt verbinding met de onderliggende gegevens door het **importeren van** een kopie van de gegevens of door **een directe verbinding** met de gegevensbron met behulp van **DirectQuery**.

Hier zijn de verschillen tussen het gebruik van **invoer** - en **DirectQuery**.

|Importeren | DirectQuery
|---|---
|Tabellen, kolommen *en gegevens* worden geïmporteerd of gekopieerd naar het rapport van de dataset. Overzicht van wijzigingen in de onderliggende gegevens, moet u vernieuwt of importeert, een volledige, de huidige gegevensset opnieuw.|*Tabellen en kolommen* worden geïmporteerd of gekopieerd naar het rapport van de dataset. U weergeven altijd de meest recente gegevens.
Met Power BI ingesloten u DirectQuery kunt gebruiken met cloud-gegevensbronnen, maar niet op-premises gegevensbronnen op dit moment.

## <a name="benefits-of-using-directquery"></a>Voordelen van het gebruik van DirectQuery

Er zijn twee belangrijke voordelen bij het gebruik van **DirectQuery**:

   -    **DirectQuery** kunt u visualisaties maken over zeer grote gegevenssets, waar anders naar eerste invoer unfeasible worden zou alle gegevens.
   -    Wijzigingen in de onderliggende gegevens vernieuwen kunt vereisen en voor sommige rapporten moet worden actuele gegevens weergegeven, grote gegevensoverdracht, waardoor de gegevens opnieuw importeren unfeasible kunt vereisen. **DirectQuery** rapporten daarentegen altijd de huidige gegevens gebruikt.

## <a name="limitations-of-directquery"></a>Beperkingen van DirectQuery

   Er zijn enkele beperkingen bij het gebruik van **DirectQuery**:

   -    Alle tabellen moeten afkomstig zijn van één database.
   -    Als de query te complex is is, treedt er een fout op. Om de fout te verhelpen moet u de query refactoring, dus het is minder complex. Als het quuery complex zijn moet, moet u de gegevens in plaats van **DirectQuery**te importeren.
   -    Relatie-filtering is beperkt tot een en dezelfde richting, in plaats van beide richtingen.
   -    U kunt het gegevenstype van een kolom niet wijzigen.
   -    Standaard beperkingen op DAX expressies toegestaan in maatregelen. Zie [DirectQuery en maatregelen](#measures).

<a name="measures"/>
## <a name="directquery-and-measures"></a>DirectQuery en maatregelen

Query's verzonden naar de onderliggende gegevensbron acceptabel zijn, zodat worden beperkingen opgelegd aan de maatregelen. Wanneer deze beperking omzeilen door te kiezen met behulp van **Power BI-Desktop**, geavanceerde gebruikers kunt kiezen **Bestand > Opties en instellingen > Opties voor**. Kies **DirectQuery**in het dialoogvenster **Opties** en selecteert u de optie **onbeperkte maatregelen in de DirectQuery modus staan**. Wanneer deze optie is geselecteerd, kan een willekeurige expressie DAX die geldig is voor een maatregel kan worden gebruikt. Gebruikers moeten zich realiseren; dat sommige expressies die uitstekend presteren wanneer de gegevens worden geïmporteerd kunnen echter resulteren in een zeer trage query's naar de bron van de back-end in de **DirectQuery** modus. 

## <a name="see-also"></a>Zie ook
- [Aan de slag met Microsoft Power BI ingesloten](power-bi-embedded-get-started.md)
- [Power BI-Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
