<properties
   pageTitle="Interactief werken met rapporten met behulp van de JavaScript-API | Microsoft Azure"
   description="Energie-BI ingesloten, interactief werken met rapporten met behulp van de JavaScript-API"
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

# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>Werken met Power BI-rapporten met behulp van de JavaScript-API

De Power BI JavaScript-API kunt u gemakkelijk rapporten Power BI insluiten in uw toepassingen. Uw toepassingen kunnen met de API, programmatische interactie met andere lijst elementen zoals pagina's en filters. Deze interactiviteit maakt Power BI verslagen een meer geïntegreerd onderdeel van uw toepassing.

U een rapport Power BI in uw toepassing met behulp van een iframe die wordt gehost als onderdeel van de toepassing. De iframe fungeert als een barrière tussen uw toepassing en het rapport, zoals u in de volgende afbeelding kunt zien. 

![Power BI ingesloten iframe zonder Javascript-API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

De iframe maakt het insluiten veel eenvoudiger, maar zonder de JavaScript-API in het rapport en de toepassing geen interactie met elkaar. Dit gebrek aan interactie, kunt u vindt dat het rapport niet echt deel van de toepassing uitmaakt. Het rapport en de toepassing echt moeten communiceren heen en weer, zoals in de volgende afbeelding.

![Power BI ingesloten iframe met Javascript-API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

De Power BI JavaScript-API kunt u code schrijven die de iframe-rand veilig kan passeren. Hierdoor kunnen de toepassing via programmering een handeling te verrichten in een rapport, en om te luisteren naar gebeurtenissen van acties die gebruikers in de lijst aanbrengen.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Wat kunt u doen met de JavaScript-API van Power BI?
Met de JavaScript-API kunt u rapporten beheren, Ga naar pagina's in een rapport, een rapport filteren en insluiten van gebeurtenissen verwerken. In het volgende diagram ziet u de structuur van de API.

![Power BI JavaScript API-diagram](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### <a name="manage-reports"></a>Rapporten beheren
De Javascript-API kunt u het probleem op het niveau en de pagina beheren:

- Een specifiek rapport voor Power BI veilig insluiten in uw toepassing - probeer de [demo van toepassingen insluiten](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  - Set access token
- Het rapport configureren
  - Inschakelen en uitschakelen van de pagina navigatiedeelvenster en filterdeelvenster - probeer de [demo-toepassing instellingen bijwerken](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Standaardwaarden instellen voor pagina's en filters - probeer de [demo standaardwaarden instellen](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Voer en de modus volledig scherm afsluiten

[Meer informatie over het insluiten van een rapport](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### <a name="navigate-to-pages-in-a-report"></a>Ga naar pagina's in een rapport
De JavaScript-API enbales u kunnen vinden alle pagina's in een rapport en stelt u de huidige pagina. Probeer de [voorbeeldtoepassing voor navigatie](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Meer informatie over de Paginanavigatie](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Een rapport filteren
De JavaScript-API biedt eenvoudige en geavanceerde filtermogelijkheden voor ingesloten rapporten en rapportpagina's. Probeer de [demo application filters](http://azure-samples.github.io/powerbi-angular-client/#/scenario4)en sommige inleidende code hier bekijken.  


#### <a name="basic-filters"></a>Eenvoudige filters
Een eenvoudige filter op een kolom- of niveau wordt geplaatst en bevat een lijst met waarden wilt opnemen of uitsluiten.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Geavanceerde filters
Geavanceerde filters gebruiken de logische operator en of of en accepteer de voorwaarden van één of twee, elk met hun eigen operator en waarde. Ondersteunde voorwaarden zijn:

- Geen
- LessThan
- LessThanOrEqual
- Groter dan
- GreaterThanOrEqual
- Bevat
- DoesNotContain
- StartsWith
- DoesNotStartWith
- Is
- IsNot
- ISLEEG
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Meer informatie over filteren](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### <a name="handling-events"></a>Gebeurtenissen afhandelen
Informatie in de iframe sturen, kan uw toepassing ook informatie ontvangen over de volgende gebeurtenissen die afkomstig zijn van de iframe:

- Insluiten
  - geladen
  - Fout
- Rapporten
  - pageChanged
  - dataSelected (binnenkort)

[Meer informatie over het afhandelen van gebeurtenissen](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## <a name="next-steps"></a>Volgende stappen
Bekijk de volgende koppelingen voor meer informatie over de Power BI JavaScript-API:

- [JavaScript API-Wiki](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Naslaginformatie](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Voorbeelden
  - [Hoek](http://azure-samples.github.io/powerbi-angular-client)
  - [Ember](https://github.com/Microsoft/powerbi-ember)
- [Live demo](https://microsoft.github.io/PowerBI-JavaScript/demo/)
