<properties
    pageTitle="Eindpunten van een webservice in Machine Learning maken | Microsoft Azure"
    description="Eindpunten van een webservice maken in Azure Machine Learning"
    services="machine-learning"
    documentationCenter=""
    authors="hiteshmadan"
    manager="padou"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="10/04/2016"
    ms.author="himad"/>


# <a name="creating-endpoints"></a>Eindpunten maken

>[AZURE.NOTE] In dit onderwerp wordt beschreven technieken die van toepassing is op een klassieke webservice.

Wanneer u de webservices die u doorsturen naar uw klanten verkoopt maakt, moet u opgeleide modellen bieden aan elke klant die nog steeds zijn gekoppeld aan het experiment waaruit de webservice is gemaakt. Bovendien moeten updates van het experiment worden toegepast selectief op een eindpunt zonder dat de aanpassingen worden overschreven.

Om dit te bereiken, kunt u meerdere eindpunten voor een geïmplementeerde webservice Azure Machine Learning. Elk eindpunt in de webservice is onafhankelijk van elkaar behandeld, beperkte bandbreedte en beheerd. Elk eindpunt is een unieke URL en vergunning sleutel die u naar uw klanten distribueren kunt.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Eindpunten toevoegen aan een webservice

Er zijn drie manieren om een eindpunt toevoegen aan een webservice.

* Via programmering
* Via de portal Azure Machine Learning Web-Services
* Hoewel de klassieke Azure portal

Zodra het eindpunt is gemaakt, kunt u deze via synchrone API's batch-API's, verbruiken en excel-werkbladen. Naast het toevoegen van eindpunten via deze gebruikersinterface ook kunt u de API's voor eindpunt eindpunten programmatisch toevoegen.

 >[AZURE.NOTE] Als u extra eindpunten aan de webservice hebt toegevoegd, kunt u het standaardeindpunt niet verwijderen.

## <a name="adding-an-endpoint-programmatically"></a>Een eindpunt toevoegen programmatisch

U kunt een eindpunt toevoegen aan uw Web-service via programmacode met behulp van de [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) -voorbeeldcode.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Een eindpunt met de webservices van Azure Machine Learning portal toevoegen

1. Klik op de navigatiebalk aan de linkerkant kolom Machine Learning Studio Web Services.
2. Klik onderaan in het dashboard Web service **beheren eindpunten**. De webservices van Azure Machine Learning portal geopend op de pagina Tunneleindpunten voor de webservice.
3. Klik op **Nieuw**.
4. Typ een naam en beschrijving voor het nieuwe endpoint. Eindpunt namen 24 tekens of minder lang moeten zijn en moeten bestaan uit kleine letters of getallen. Selecteer het niveau van de logboekregistratie en of de voorbeeldgegevens is ingeschakeld. Zie [Logboekregistratie inschakelen voor Machine Learning Web-services](machine-learning-web-services-logging.md)voor meer informatie over het vastleggen.

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Een eindpunt met de Azure klassieke portal toevoegen


1. Aanmelden bij de [Azure klassieke portal](http://manage.windowsazure.com), klikt u op **Machine Learning** in de linkerkolom. Klik in de werkruimte met de webservice waarin u geïnteresseerd bent.

    ![Ga naar de werkruimte](./media/machine-learning-create-endpoint/figure-1.png)

2. Klik op de **webservices**.

    ![Ga naar webservices](./media/machine-learning-create-endpoint/figure-2.png)

3. Klik op de webservice die u geïnteresseerd bent in de lijst van beschikbare eindpunten.

    ![Ga naar het eindpunt](./media/machine-learning-create-endpoint/figure-3.png)

4. Onderaan op de pagina, klikt u op het **Eindpunt toevoegen**. Typ een naam en beschrijving, Controleer of er geen andere eindpunten met dezelfde naam in deze webservice. Laat het niveau van de gashendel met de standaardwaarde, tenzij u speciale eisen hebt. Zie voor meer informatie over het beperken, [Schalen API eindpunten](machine-learning-scaling-webservice.md).

    ![Eindpunt maken](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Volgende stappen

[Het verbruiken van een gepubliceerde Azure Machine Learning Web service](machine-learning-consume-web-services.md).
