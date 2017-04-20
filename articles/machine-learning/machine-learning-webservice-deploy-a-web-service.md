<properties
   pageTitle="Implementatie van een nieuwe webservice"
   description="De workflow van de implementatie van een ARM gebaseerd webservice"
   services="machine-learning"
   documentationCenter=""
   authors="vDonGlover"
   manager="raymondl"
   editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>

# <a name="deploy-a-new-web-service"></a>Een nieuwe webservice implementeren

Microsoft Azure Machine nu leren biedt webservices op basis van [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor het nieuwe plan factureringsopties en implementeren van uw webservice aan meerdere landen.

De algemene workflow voor de implementatie van een webservice met Microsoft Azure Machine Learning Web Services is:

* Maken van een anticiperende experiment
* deze implementeren
* Configureer de naam van de
* factuuradres-plan
* het testen
* Deze verbruiken.

In de volgende afbeelding ziet u de workflow.

![Web-service implementatie van workflow][1]
 
## <a name="deploy-web-service-from-studio"></a>Web-service vanuit Studio implementeren 

Implementatie van een experiment als een nieuwe webservice. Aanmelden bij de computer leren Studio en maak een nieuwe voorspellende webservice. 

**Opmerking**: als u al een experiment als klassieke webservice werkt u deze niet implementeren als nieuwe webservice.
 
Klik op **uitvoeren** op de onderkant van het papier experiment en klik vervolgens op **Web-Service installeren** en **Implementeren van Web Service [Nieuw]**. De pagina van de implementatie van de Machine Learning Web Service manager wordt geopend.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Machine Learning Web Service Manager Experiment pagina implementeren
Voer een naam voor de web-service op de pagina Experiment implementeren.
Selecteer een prijsstelling plan. Als u een bestaande prijsstelling plan dat kunt u dit, moet anders u een nieuw plan van de prijs voor de service. 

1.  Vervolgkeuzelijst, selecteer een bestaand schema of selecteert u de optie **nieuwe plan selecteren dat** in het **Plan van de prijs** .
2.  Typ in het vak **Naam**een naam op voor het plan op uw rekening.
3.  Selecteer een van de **Maandelijkse plannen lagen**. Houd er rekening mee dat de standaard plan lagen aan de plannen voor uw regio standaard en de webservice wordt gedistribueerd in dat gebied.

Klik op **Deploy** en de Quickstart-pagina voor uw web service wordt geopend.

## <a name="quickstart-page"></a>QuickStart-pagina
De pagina met Quickstart hebt u toegang en richtlijnen op de meest voorkomende taken die u uitvoert na het maken van een nieuwe webservice. Hier kunt u zowel de **Test** en **verbruiken** pagina eenvoudig openen.

## <a name="testing-your-web-service"></a>Testen van uw web-service

Klik vanaf de pagina Quickstart webservice testen onder algemene taken.   

Als een verzoek-antwoord Service (RR's) voor de webservice testen:

* Klik op **testen** op de menubalk.
* Klik op **Verzoek-antwoord**.
* Waarden invoeren voor de invoer kolommen van het experiment.
* Klik op Test **Verzoek-antwoord**.

U resultaten wilt weergeven aan de rechterkant van de pagina.

Als u wilt testen een webservice Batch uitvoeren van Service (BES), gebruikt u een CSV-bestand:

* Klik op **testen** op de menubalk.
* Klik op **Batch**.
* Onder uw invoer, klikt u op Bladeren en navigeer naar het bestand met voorbeeldgegevens.
* Klik op **testen**.

De status van de test wordt weergegeven onder de **Batchverwerkingen testen**.

## <a name="consuming-your-web-service"></a>Uw Web-Service gebruiken

Wanneer wordt geïmplementeerd als een webservice, bieden Azure Machine Learning experimenten een REST API die kan worden gebruikt door een groot aantal apparaten en platforms. Dit is omdat de eenvoudige REST API accepteert en reageert met JSON berichten opgemaakte. De Azure Machine Learning portal bevat code die kan worden gebruikt voor het aanroepen van de webservice in R, C# en Python.
 
Op de pagina verbruik te vinden:

* De API-sleutel en URI's voor het gebruik van web-service in apps.
* Sjablonen voor Excel en het web app Activeer starten uw verbruik.
* Voorbeeld van code in C#, python en R aan de slag.

Zie voor meer informatie over het gebruik van webservices, [verbruiken een Azure Machine Learning web-service die is geïmplementeerd in een Machine Learning experiment](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van webservices:

[Hoe een Azure Machine Learning web-service die is geïmplementeerd in een Machine Learning experiment verbruiken](machine-learning-consume-web-services.md)

[Azure Machine Learning Web-Services: Distributie en verbruik](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
