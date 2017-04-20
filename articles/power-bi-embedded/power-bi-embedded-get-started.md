<properties
   pageTitle="Aan de slag met Microsoft Power BI ingesloten"
   description="Power BI ingesloten, interactieve Power BI rapporten toevoegen aan uw business intelligence-toepassingen"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="get-started-with-microsoft-power-bi-embedded"></a>Aan de slag met Microsoft Power BI ingesloten

**Power BI ingesloten** is een Azure service waarmee ontwikkelaars interactieve Power BI-rapporten in hun eigen toepassingen toevoegen. **Power BI ingesloten** werkt met bestaande toepassingen zonder ontwerpen of wijzigen van de manier waarop gebruikers inloggen.

Bronnen voor **Microsoft Power BI ingesloten** zijn via de [API's van Azure ARM](https://msdn.microsoft.com/library/mt712306.aspx)ingericht. In dit geval is de resource die u wilt inrichten een **Power BI werkruimte-collectie**.

![](media\power-bi-embedded-get-started\introduction.png)

## <a name="create-a-workspace-collection"></a>Een collectie werkruimte maken
Een **Collectie van de werkruimte** is het hoogste niveau Azure resource en een container voor de inhoud die in uw toepassing wordt ingesloten. Een **Collectie van de werkruimte** kunnen op twee manieren worden gemaakt:

   -    Handmatig met behulp van de Portal Azure
   -    Via programmacode met behulp van de API's van Azure Resource Manager(ARM)

We doorlopen de stappen voor het opbouwen van een **Collectie van de werkruimte** met behulp van de Portal Azure.

   1.   Openen en inloggen op **Portal Azure**: [http://portal.azure.com](http://portal.azure.com).

   2.   Klik op **+ Nieuw** in het bovenste deelvenster.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   Klik op **Power BI ingesloten**onder **gegevens + Analytics** .
   4.   Voer de vereiste informatie op de **Blade maken**. Zie voor **prijzen**, [prijzen voor Power BI ingesloten](http://go.microsoft.com/fwlink/?LinkID=760527).

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Klik op **maken**.

De **Collectie van de werkruimte** moet u even naar de voorziening. Wanneer voltooid, zult u worden genomen om de **Werkruimte collectie Blade**.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

De **Blade maken** bevat de gegevens die u moet aanroepen van API's die werkruimten maken en implementeren van inhoud op deze.

<a name="view-access-keys"/>
## <a name="view-power-bi-api-access-keys"></a>Weergave toegangstoetsen Power BI API

Een van de belangrijkste stukken van informatie die nodig is voor de Power BI REST API's aanroepen zijn de **Toegangstoetsen**. Deze worden gebruikt voor het genereren van de **app-tokens** die worden gebruikt voor het verifiëren van de API-verzoeken. Klik op **Access-toetsen** op de **Instellingen voor Blade** **Toegangstoetsen**. Zie voor meer informatie over **app tokens**, [Authenticating en machtigen met Power BI ingesloten](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

U zult ' mededeling dat er twee sleutels.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Kopieer deze sleutels en veilig opslaan in uw toepassing. Het is zeer belangrijk dat u deze toetsen behandelen als u een wachtwoord, omdat ze toegang tot alle inhoud in uw **Werkruimte-collectie bieden**.

Terwijl er twee sleutels worden weergegeven, is op een bepaald moment slechts één sleutel nodig. De tweede sleutel wordt geboden, zodat u sleutels periodiek opnieuw genereren kunt zonder toegang tot de service te onderbreken.

Nu dat u een exemplaar van de Power BI voor uw toepassing en de **Toegangstoetsen**hebt, kunt u een rapport kunt importeren in uw eigen app. Voordat u informatie over het importeren van een rapport, wordt de volgende sectie beschreven maken Power BI datasets en rapporten insluiten in een app.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app"></a>Power BI datasets en rapporten insluiten in een app maken

Nu dat u een exemplaar van de Power BI hebt gemaakt voor uw toepassing en **Toegangstoetsen**hebt, moet u voor het maken van de Power BI datasets en rapporten die u wilt insluiten. DataSets en rapporten kunnen worden gemaakt met behulp van **Power BI-bureaublad**. U kunt downloaden [Power BI bureaublad voor gratis](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/). Of snel aan de slag kunt u downloaden van de [Retail analyse voorbeeld PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547). Voor meer informatie over het gebruik van **Power BI-Desktop**, Zie [Aan de slag met Power BI bureaublad](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

Met **Power BI bureaublad**verbinding u met de gegevensbron door een kopie van de gegevens importeren in **Power BI bureaublad** of een rechtstreekse verbinding met de gegevensbron met behulp van **DirectQuery**.

Hier zijn de verschillen tussen het gebruik van **invoer** - en **DirectQuery**.

|Importeren | DirectQuery
|---|---
|Tabellen, kolommen *en gegevens* worden geïmporteerd of gekopieerd naar **Power BI-bureaublad**. Bij het werken met visualisaties, vraagt de **Power BI-Desktop** een kopie van de gegevens. Overzicht van alle wijzigingen die zijn opgetreden in de onderliggende gegevens, moet u vernieuwen of importeren, een volledige, de huidige gegevensset opnieuw.|*Tabellen en kolommen* worden geïmporteerd of gekopieerd naar **Power BI-bureaublad**. Bij het werken met visualisaties, vraagt **Power BI bureaublad** de onderliggende gegevensbron, wat betekent dat u altijd actuele gegevens weergeeft.

Zie voor meer informatie over verbinding maken met een gegevensbron [verbinden met een gegevensbron](power-bi-embedded-connect-datasource.md).

Nadat u uw werk in **Power BI-Desktop opslaat**, een PBIX-bestand gemaakt. Dit bestand bevat het rapport. Bovendien, als u gegevens importeert de PBIX bevat de volledige gegevensset, of als u **DirectQuery**gebruikt, de PBIX een dataset-schema bevat. U implementeren programmatisch de PBIX in uw werkruimte met behulp van de [API voor Power BI importeren](https://msdn.microsoft.com/library/mt711504.aspx).

> [AZURE.NOTE] **Power BI ingesloten** heeft extra API's om de server en de database waarnaar uw gegevensset wijst wijzigen en instellen van een service account referentie die de dataset wordt gebruikt voor verbinding met de database. Zie [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) en [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="next-steps"></a>Volgende stappen
U hebt gemaakt in de vorige stappen een collectie werkruimte en uw eerste rapport en de dataset. Nu is het tijd om te leren hoe u code schrijven voor **Power BI ingesloten**. Als u aan de slag, we een monster web app gemaakt: [aan de slag met het monster](power-bi-embedded-get-started-sample.md). In het voorbeeld ziet u hoe te:

  - Inhoud levering
      - Een werkruimte maken
      - Een PBIX-bestand importeren
      - Verbindingsreeksen bijwerken en referenties voor de gegevenssets instellen.

  - Een rapport veilig insluiten

## <a name="see-also"></a>Zie ook
- [Aan de slag met monster](power-bi-embedded-get-started-sample.md)
- [Verificatie en autorisatie met Power BI ingesloten](power-bi-embedded-app-token-flow.md)
- [Power BI-desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
