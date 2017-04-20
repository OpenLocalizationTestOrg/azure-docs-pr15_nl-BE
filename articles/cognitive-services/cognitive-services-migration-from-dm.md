
<properties
    pageTitle="Migreren naar aanbevelingen Azure cognitieve Services API van de aanbevelingen van de DataMarket API | Microsoft Azure"
    description="Azure machine learning aanbevelingen--migreren naar aanbevelingen cognitieve service"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="luisca"/>


# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>Migreren naar aanbevelingen Azure cognitieve Services API van de aanbevelingen van de DataMarket API
In dit artikel wordt beschreven hoe u de [API Microsoft DataMarket-aanbevelingen](https://datamarket.azure.com/dataset/amla/recommendations) migreren naar de [Microsoft Azure cognitieve Services aanbevelingen API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api).

De API DataMarket aanbevelingen zal accepteert geen nieuwe klanten op 31 December 2016, en 28 februari 2017 zal worden afgeschaft.

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>Hoe start met Azure cognitieve Services aanbevelingen API?

Als u wilt migreren naar de cognitieve Services API aanbevelingen, het volgende doen:

1.  Als u niet al een Azure-abonnement, [melden](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) voor een. 

1.  Stapsgewijze instructies worden opgehaald uit de [Introductiehandleiding](cognitive-services-recommendations-quick-start.md) aanmelden voor de cognitieve Services API aanbevelingen en het programmatisch gebruik. 

1.  Ga naar de [portaalpagina cognitieve Services aanbevelingen API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) voor meer informatie over de service en documentatie vinden.

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>Ik de aanbevelingen-gebruikersinterface gebruikt om mijn modellen te bouwen. Is er een vergelijkbaar hulpprogramma voor de cognitieve Services API aanbevelingen?

Absoluut! De URL voor de nieuwe [Gebruikersinterface van aanbevelingen](http://recommendations-portal.azurewebsites.net/) is http://recommendations-portal.azurewebsites.net. 

>[AZURE.NOTE] Uw referenties DataMarket werken hier niet. Aanmelden voor een abonnement in de Portal Azure en de Account-sleutel die nodig zijn voor het gebruik van de nieuwe [Gebruikersinterface voor aanbevelingen](http://recommendations-portal.azurewebsites.net/).
Als u geen Account-toets, Zie taak 1 van de [Slag](cognitive-services-recommendations-quick-start.md).

##<a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>De nieuwe API-indeling is hetzelfde als de API DataMarket aanbevelingen?

De API ondersteunt het hetzelfde scenario's en het proces stromen als de scenario's worden ondersteund in de versie DataMarket, maar de werkelijke API-interface is bijgewerkt om te voldoen aan de [Richtlijnen van Microsoft REST API](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). De API's zijn meer consistente en werk beter met extra ondersteuning van Swagger.

Uitchecken om te begrijpen van de API's, de [explorer API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db).
Gebruik de *probeert* deze knop als u wilt testen een API-aanroep. De indeling van de bestanden die worden gebruikt door de API aanbevelingen (catalogus en het gebruik van bestanden) is niet gewijzigd, zodat u de dezelfde bestanden en/of infrastructuur die u hebt gemaakt voor het genereren van deze bestanden kunt blijven gebruiken.

##<a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>Wat zijn enkele nieuwe functies in de API voor aanbevelingen van cognitieve Services?

In de afgelopen twee maanden hebben we nieuwe mogelijkheden voor de cognitieve Services API aanbevelingen uitgebracht:
-   Aanbevelingen UI voor training en testen van modellen zonder één regel code te schrijven
-   Batch waarmee u duizenden aanbevelingen in één keer scoren
-   Maatstelsel van ondersteuning aan de kwaliteit van de aanbevelingen query opbouwen
-   Ondersteuning voor zakelijke regels
-   Mogelijkheid om te inventariseren en downloaden van bestanden en de catalogus
-   Ranking build ondersteuning van de kwaliteit van de functies in een model aanbevelingen item opvragen
-   Toegevoegde mogelijkheid om te zoeken naar een product uit de catalogus

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Wanneer stopt Microsoft de API DataMarket aanbevelingen ondersteunen?

[API op DataMarket aanbevelingen](https://datamarket.azure.com/dataset/amla/recommendations) accepteert geen nieuwe klanten na 31 December 2016 en zal volledig worden vervangen door 28 februari 2017. 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>Wat gebeurt er als ik heb niet de gegevens die ik wil opnieuw mijn modellen in de cognitieve Services API aanbevelingen?

Wij willen deze overgang zo eenvoudig mogelijk te maken voor u. Wij kunnen helpen u uw oude modellen van uw DataMarket-account verplaatsen naar uw nieuwe abonnement op Azure cognitieve Services aanbevelingen API. Neem contact met ons op [mlapi@microsoft.com](mailto://mlapi@microsoft.com). Vragen we u uw DataMarket-abonnement-ID en uw Services cognitieve abonnements-ID, voordat we de modellen met de nieuwe account wilt koppelen.
