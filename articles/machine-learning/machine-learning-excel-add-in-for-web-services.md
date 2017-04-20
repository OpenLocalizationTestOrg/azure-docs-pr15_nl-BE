<properties
    pageTitle="Excel-invoegtoepassing voor webservices van Machine Learning | Microsoft Azure"
    description="Het gebruik van Azure Machine Learning webservices rechtstreeks in Excel een code te schrijven."
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/05/2016"
    ms.author="tedway;garye" />

# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Excel Add-in voor Azure Machine Learning Web-services

Met Excel gemakkelijk aanroepen van webservices rechtstreeks zonder code te schrijven.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Stappen voor het gebruik van een bestaand Web service in de werkmap

1. Open het [voorbeeldbestand Excel](http://aka.ms/amlexcel-sample-2)met de Excel-invoegtoepassing en gegevens over passagiers van de Titanic.
2. De webservice kiezen door erop te klikken-"Titanic nagelaten Predictor (voorbeeld Excel Add-in) [Score]" in het volgende voorbeeld.

    ![Selecteer webservice][01]

3. Hiervoor gaat u naar de sectie **Predict** .  Deze werkmap bevat al de voorbeeldgegevens, maar voor een lege werkmap kunt u een cel selecteren in Excel en klikt u op **voorbeeldgegevens gebruiken**.
4. Selecteer de gegevens die met kop en klik op het pictogram invoergegevens bereik.  Zorg ervoor dat het selectievakje "Mijn gegevens hebben koppen".
5. Geef onder **Output**, het celnummer van de waar u de uitvoer moet worden, bijvoorbeeld 'H1' hier.
6. Klik op **voorspellen**.

    ![Sectie voorspellen][02]

## <a name="steps-to-add-a-new-web-service"></a>Stappen voor het toevoegen van een nieuwe webservice.

Implementeren van een webservice of een bestaand webservice gebruikt. Zie voor meer informatie over het implementeren van een webservice [scenario stap 5: de Azure Machine Learning Web-service implementeert](machine-learning-walkthrough-5-publish-web-service.md).

De API-sleutel voor de webservice ophalen. Wanneer u dit doet afhankelijk van of u een klassieke Machine Learning webservice van een nieuwe Machine Learning op Internet gepubliceerd.

**Een klassieke webservice gebruiken** 

1. Machine Learning Studio op de sectie **WEB SERVICES** in het linkerdeelvenster en selecteer de webservice.

    ![Selecteer Studio een webservice][04]

2. Kopieer de API-sleutel voor de webservice.

    ![Studio API-sleutel][05]

3. Op het tabblad **DASHBOARD** voor de webservice, klikt u op de link **Aanvraag en respons** .
4. Zoek de sectie van de **Aanvraag-URI** .  Kopiëren en opslaan van de URL.

>[AZURE.NOTE] Het kan nu inloggen op de portal [Azure Machine Learning Web Services](https://services.azureml.net) te verkrijgen van de API-sleutel voor een klassieke Machine Learning Web service.

**Een nieuwe webservice gebruiken**

1. In de portal [Azure Machine Learning Web-Services](https://services.azureml.net) , klikt u op de **Web Services**en selecteer vervolgens uw webservice. 
2. Klik **in beslag nemen**.
3. Zoek de sectie **gegevens van elementaire verbruik** . Kopiëren en opslaan van de **Primaire sleutel** en de URL van de **Aanvraag-antwoord** .


## <a name="steps-to-add-a-new-web-service"></a>Stappen voor het toevoegen van een nieuwe webservice.

1. Implementeren van een webservice of een bestaand webservice gebruikt. Zie voor meer informatie over het implementeren van een webservice [scenario stap 5: de Azure Machine Learning Web-service implementeert](machine-learning-walkthrough-5-publish-web-service.md).
2. Klik **in beslag nemen**.
3. Zoek de sectie **gegevens van elementaire verbruik** . Kopiëren en opslaan van de **Primaire sleutel** en de URL van de **Aanvraag-antwoord** .
2. In Excel, gaat u naar de sectie **Web Services** (als u in de sectie **Predict** , klikt u op de pijl naar links te gaan naar de lijst van webservices).

    ![Ga naar Web service selectie][03]
    
3. Klik op **webservice toevoegen**.
4. Plak de URL in het Excel-invoegtoepassing tekstvak **URL**.
5. Plak de API/primaire sleutel in het tekstvak **API-sleutel**.
6. Klik op **toevoegen**.

    ![URL's en API-sleutel voor een klassieke webservice.][06]

10. Voer de voorgaande aanwijzingen, 'Stappen voor het gebruik van een bestaand webservice' met de webservice

## <a name="sharing-your-workbook"></a>Delen van de werkmap

Als u de werkmap opslaat, wordt ook de API/primaire sleutel voor de webservices die u hebt toegevoegd opgeslagen. Dit betekent dat u moet alleen de werkmap deelt met personen die u vertrouwt.

Vragen te stellen in de volgende sectie Opmerking of op ons [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
