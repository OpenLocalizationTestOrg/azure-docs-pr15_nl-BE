<properties
   pageTitle="Webservice schalen | Microsoft Azure"
   description="Informatie over het schalen van een webservice door het bevorderen van gelijktijdigheid en toevoegen van nieuwe eindpunten."
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="Azure machine leren, webservices, uitoefening, schalen, eindpunt, gelijktijdigheid"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="10/05/2016"
   ms.author="neerajkh"/>

# <a name="scaling-a-web-service"></a>Schaal van een webservice

>[AZURE.NOTE] In dit onderwerp wordt beschreven technieken die van toepassing is op een klassieke Machine Learning Web service. 

Standaard kan elke gepubliceerde Web-service is geconfigureerd voor ondersteuning van 20 gelijktijdige aanvragen en zo hoog als 200 gelijktijdige aanvragen. Terwijl de Azure klassieke portal kunt u deze waarde instelt, Azure Machine Learning optimaliseert automatisch de instelling voor de beste prestaties voor uw webservice en de portal waarde wordt genegeerd. 

Als het plan aan te roepen, de API met een hogere belasting dan het maximum aantal gelijktijdige gesprekken met de waarde 200 ondersteunt, moet u meerdere endpoints op dezelfde webservice maken. U kunt uw belasting willekeurig verdelen over alle van hen.

## <a name="add-new-endpoints-for-same-web-service"></a>Nieuwe eindpunten voor dezelfde webservice toevoegen

De schaal van een webservice is een gemeenschappelijke taak. Enkele redenen om te schalen zijn voor ondersteuning van meer dan 200 gelijktijdige aanvragen, vergroot de beschikbaarheid via meerdere endpoints of afzonderlijke eindpunten opgeven voor de webservice. U kunt de schaal vergroten door extra eindpunten voor dezelfde webservice via [Azure klassieke portal](https://manage.windowsazure.com/) of de [Webservice van Azure Machine Learning](https://services.azureml.net/) portal toe te voegen.

Zie voor meer informatie over het toevoegen van nieuwe eindpunten [Eindpunten maken](machine-learning-create-endpoint.md).

Houd er rekening mee dat met behulp van een aantal hoge gelijktijdigheid schadelijk als u niet de API met een overeenkomstig hoog tarief belt. U ziet enkel geval time-outs en/of pieken in de vertragingstijd als u een relatief lage belasting op een API die is geconfigureerd voor hoge belasting.

De synchrone API's worden meestal gebruikt in situaties waar een lage latentie is gewenst. Latentie hier houdt de tijd die het duurt voor de API om een aanvraag te voltooien en geen rekening gehouden met eventuele vertragingen in het netwerk. Stel dat u beschikt over een API met een vertragingstijd van 50 ms. Volledig in beslag neemt de beschikbare capaciteit met gashendel niveau Hoog en het maximum aantal gelijktijdige gesprekken = 20, moet u deze API 20 * 1000 aanroepen / 50 = 400 maal per seconde. Een maximum aantal gelijktijdige gesprekken van 200 kunt dit verder uitbreidt, u roept de API 4000 keer per seconde, ervan uitgaande dat een 50 ms latentie.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
