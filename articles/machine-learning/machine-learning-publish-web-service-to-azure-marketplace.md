<properties 
    pageTitle="Machine learning publiceren webservice met Azure Marketplace | Microsoft Azure" 
    description="Het publiceren van uw webservice Azure Machine Learning naar Azure Marketplace" 
    services="machine-learning" 
    documentationCenter="" 
    authors="BharathS" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="bharaths"/>

# <a name="publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a>Azure Machine Learning webservice publiceren naar Azure Marketplace 

De markt Azure biedt de mogelijkheid aan Azure Machine Learning webservices publiceren als betaald of gratis services voor consumptie door externe klanten. Dit artikel biedt een overzicht van het proces met koppelingen naar richtlijnen om u aan de slag. Met behulp van dit proces, kunt u uw web-services beschikbaar voor andere ontwikkelaars in hun toepassingen in beslag neemt.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a>Overzicht van het publicatieproces 

Hieronder volgen de stappen voor het publiceren van een webservice Azure Machine Learning naar Azure Marketplace:

1. Maken en publiceren van een Machine Learning Request-Response service (RR's)
2. De service implementeren voor de productie en de API-sleutel en OData eindpunt informatie verkrijgen.
3. De URL van de gepubliceerde webservice gebruikt om te publiceren naar [Azure Marketplace (markt gegevens)](https://publish.windowsazure.com/workspace/) 
4. Eenmaal ingediend, wordt uw aanbieding herzien en moet worden goedgekeurd voordat uw klanten kunt beginnen het kopen. Het publicatieproces kan enkele dagen duren. 

## <a name="walk-through"></a>Doorloop
###<a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Stap 1: Maken en publiceren van een Machine Learning Request-Response service (RR's)###
 Als u hebt dit nog niet gedaan, bekijk dan op deze [doorlopen](machine-learning-walkthrough-5-publish-web-service.md).

###<a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a>Stap 2: De service implementeren voor de productie en de API-sleutel en OData eindpunt informatie verkrijgen###
1. Selecteer de optie **MACHINE LEARNING** in de linkernavigatiebalk van het [Klassieke Portal Azure](http://manage.windowsazure.com)en selecteer uw werkruimte. 

2. Klik op het tabblad **WEB SERVICES** en selecteer de webservice die u wilt publiceren op de markt.

    ![Azure Marketplace][workspace]

3. Selecteer het eindpunt dat u graag wilt hebben van de markt in beslag nemen. Als u geen extra eindpunten hebt gemaakt, kunt u de **standaard** -eindpunt.

4. Nadat u hebt geklikt op het eindpunt, wordt u de **API-sleutel**te zien zijn. U moet dit stuk van de informatie later in stap 3, dus een kopie maken van het.

    ![Azure Marketplace][apikey]

5. Klik op de methode **Aanvraag/reactie** op dit punt dat niet ondersteunen we publishing batch uitvoering van services op de markt. Zo gaat u naar de help-pagina voor de API voor de aanvraag en respons.

6. Het **Adres van de OData-eindpunt**kopiëren, moet u deze gegevens later in stap 3.

    ![Azure Marketplace][odata]




de service implementeren in productie.



###<a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a>Stap 3: Gebruik de URL van de gepubliceerde webservice voor het publiceren van Azure Marketplace (DataMarket)###

1.  Ga naar [Azure Marketplace (markt gegevens)](http://datamarket.azure.com/home) 
2.  Klik op de koppeling **publiceren** aan de bovenkant van de pagina. Hiermee gaat u naar de [Microsoft Azure Portal voor publiceren](https://publish.windowsazure.com)
3.  Klik op de sectie **uitgevers** te registreren als uitgever.
4.  Wanneer u een nieuwe aanbieding maakt, selecteert u **Data Services**en klik vervolgens op **een nieuwe Service maken**. 
 
    ![Azure Marketplace][image1]

    <br />


5.  Onder de **plannen** geven u informatie over uw aanbod, met inbegrip van een plan voor prijzen. Als u een gratis of betaalde service biedt een besluit. U wordt uitbetaald, bieden betalingsinformatie zoals uw bank- en btw.

6.  Onder **Marketing** bevatten informatie over uw voorstel, zoals de titel en beschrijving voor uw aanbieding.

7.  U kunt onder de **prijzen** de prijs instellen voor uw plannen voor specifieke landen of laat het systeem 'autoprice' uw voorstel.

8. Klik op het tabblad **Data Service** **Webservice** als **Gegevensbron**.

    ![Azure Marketplace][image2]

9.  De URL en API sleutel voor de webservice worden opgehaald uit de klassieke Portal Azure zoals beschreven in stap 2 hierboven.

10. Plak het adres van de OData-eindpunt in het tekstvak **URL van** in het dialoogvenster Marketplace Data Service setup.

11. Kies **koptekst** als de **Verificatiemethode**voor de **verificatie**.

    - "Vergunning" voor de **Header-naam**invoeren.
    - 'Aan toonder' (zonder de aanhalingstekens) invoeren voor de **Header-waarde**, klikt u op **de SPATIEBALK** en plakt u de API-sleutel.
    - Schakel het selectievakje **deze Service OData is** .
    - Klik op **Verbinding testen** om de verbinding te testen.

12. Controleer de **Dat machine Learning** is geselecteerd onder **categorieën**.

13. Wanneer u klaar bent met invoeren van de metagegevens van uw aanbiedingen, klik op **publiceren**en **Push naar klaarzetten**. Op dit punt, u krijgt van de overige problemen die u nodig hebt om op te lossen.

14. Als u zeker bent dat de voltooiing van de resterende problemen, klikt u op op de **push voor de productie en goedkeuring aanvragen**. Het publicatieproces kan enkele dagen duren. 


[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png
 
