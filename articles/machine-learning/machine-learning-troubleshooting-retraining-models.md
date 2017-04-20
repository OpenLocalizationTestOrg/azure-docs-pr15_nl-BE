<properties
    pageTitle="Het oplossen van de Retraining van een Azure Machine Learning klassieke webservice | Microsoft Azure"
    description="Identificeren en corrigeren gemeenschappelijke problemen opgetreden wanneer u het model voor een webservice Azure Machine Learning zijn bijscholing."
    services="machine-learning"
    documentationCenter=""
    authors="VDonGlover"
   manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Het oplossen van de bijscholing van een Azure Machine Learning klassieke webservice

## <a name="retraining-overview"></a>Overzicht bijscholing

Bij het implementeren van een anticiperende experiment als score webservice is een statisch model. Zodra er nieuwe gegevens beschikbaar is of wanneer de consument van de API hun eigen gegevens heeft, moet het model worden retrained. 

Zie voor een compleet overzicht van het proces opnieuw opleiden van een klassieke Internet [Retrain Machine Learning modellen via programmacode](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Bijscholing van proces

Wanneer u de webservice retrain, moet u enkele aanvullende stukken toevoegen:

* Een webservice van het Experiment Training geïmplementeerd. De proef moet een **Web Service Output** module gekoppeld aan de uitvoer van de module **Trein Model** hebben.  

    ![De uitvoer van web service koppelen aan het model van de trein.][image1]

* Een nieuwe toegevoegd aan uw score Web-service-eindpunt.  U kunt het eindpunt programmatisch met de voorbeeldcode waarnaar wordt verwezen in de Machine Learning Retrain modellen programmatisch toevoegen onderwerp of via de klassieke Azure portal.

Vervolgens kunt u in het voorbeeld C#-code van de opleiding Web Service API help-pagina retrain model. Nadat u de resultaten hebt geëvalueerd en tevreden met bent, kunt u de opgeleide model scoren webservice met behulp van het nieuwe endpoint die u hebt toegevoegd bijwerken.

Alle stukken op zijn plaats zijn de belangrijkste stappen die u uitvoeren moet om het model retrain als volgt:

1.  Roep de webservice Training: de oproep is aan de Batch uitvoeren van Service (BES), niet de aanvraag Response Service (RR's). De steekproef C#-code kunt u op de help-pagina API voor het gesprek. 
2.  De waarden voor de *BaseLocation*, *RelativeLocation*en *SasBlobToken*vinden: deze waarden worden geretourneerd in de uitvoer van het gesprek met de opleiding Web Service. 
      ![de uitvoer van het monster opnieuw opleiden en de BaseLocation, RelativeLocation en SasBlobToken-waarden weergegeven.][image6]
3.  Het toegevoegde eindpunt van de score webservice bijwerken met het nieuwe model opgeleide: het nieuwe endpoint u toegevoegd aan het evaluatiemodel met het nieuwe opgeleide model van de webservice Training met behulp van de voorbeeldcode in de Machine Learning Retrain modellen via programmering bijwerken.

## <a name="common-obstacles"></a>Veelvoorkomende obstakels

### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Controleer of u de juiste PATCH URL hebt

De URL PATCH u moet worden gekoppeld aan het nieuwe score eindpunt dat u hebt toegevoegd aan de score van de webservice. Er zijn een aantal manieren om te downloaden van de PATCH-URL:

**Optie 1: via programmacode**

De juiste PATCH URL ophalen:

1.  De voorbeeldcode [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) uitvoeren.
2.  Zoek de waarde *HelpLocation* en kopieer de URL uit de uitvoer van AddEndpoint.

    ![HelpLocation in de uitvoer van het monster addEndpoint.][image2]

3.  Plak de URL in een browser om te navigeren naar een pagina die help-voor de webservice koppelingen.
4.  Klik op de koppeling **Bron bijwerken** om de patch help-pagina te openen.

**Optie 2: Gebruik de Azure klassieke portal**

1.  Log in om de [Azure klassieke portal](https://manage.windowsazure.com).
2.  Open het tabblad Machine Learning. 
     ![Machine leunend tabblad.][image4]
3.  Klik op de naam van uw werkruimte, vervolgens **Web Services**.
4.  Klik op de score webservice die u werkt. (Als u de standaardnaam van de webservice niet heeft wijzigt, het programma loopt [scoren Exp.].)
5.  Klik op het **eindpunt toevoegen**.
6.  Nadat het eindpunt is toegevoegd, klikt u op de naam van het eindpunt. Klik vervolgens op **Bron van Update** om de patch Helppagina te openen.

>[AZURE.NOTE] Als u het eindpunt aan de Training webservice in plaats van de voorspellende webservice hebt toegevoegd, ontvangt u het volgende foutbericht wanneer u klikt op de koppeling **Update-bron** : Sorry, maar deze functie wordt niet ondersteund of beschikbaar is in deze context. Deze webservice heeft geen bronnen worden bijgewerkt. Excuses voor het ongemak en we werken aan deze werkstroom te verbeteren.

![Nieuw eindpunt dashboard.][image3]

De PATCH help-pagina bevat de URL PATCH u moet gebruiken en vindt u voorbeelden van code die kunt u deze aanroepen.

![URL van de patch.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Controleer dat u de juiste score eindpunt wilt bijwerken

* De webservice Training niet doen patch: het patch-bewerking moet worden uitgevoerd op de webservice score.
* Voer niet het standaardeindpunt van de op webservice patch: het patch-bewerking moet worden uitgevoerd op de nieuwe score Web service-eindpunt dat u hebt toegevoegd.

U kunt controleren welke webservice door het eindpunt is de klassieke Azure portal bezoeken. 

>[AZURE.NOTE] Zorg ervoor dat u het eindpunt wilt toevoegen aan de voorspellende Web Service, niet de opleiding Web Service. Als u goed zowel een Training als een anticiperende webservice hebt geïmplementeerd, ziet u twee afzonderlijke Web-services die worden vermeld. De voorspellende webservice moet eindigen met "[voorspellende exp.]".

1.  Log in om de [Azure klassieke portal](https://manage.windowsazure.com).
2.  Open het tabblad Machine Learning. 
     ![Machine learning werkruimte UI.][image4]
3.  Selecteer uw werkruimte.
4.  Klik op de **webservices**.
5.  Selecteer uw voorspellende webservice.
6.  Controleer of uw nieuwe eindpunt is toegevoegd aan de webservice.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>Controleer de werkruimte die de web-service in zodat deze in de juiste regio

1.  Log in om de [Azure klassieke portal](https://manage.windowsazure.com).
2.  Machine Learning selecteert in het menu.
      ![Machine learning regio UI.][image4]
3.  Controleer de locatie van uw werkruimte.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png