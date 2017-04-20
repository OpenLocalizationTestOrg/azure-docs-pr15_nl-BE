<properties
    pageTitle="Het implementeren van een Azure API Management service-exemplaar aan meerdere landen van Azure"
    description="Informatie over hoe u een exemplaar van Azure API Management service implementeren op meerdere Azure regio's." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Het implementeren van een Azure API Management service-exemplaar aan meerdere landen van Azure

Beheer-API ondersteunt meerdere regio implementatie waarmee uitgevers API een enkele service API over een willekeurig aantal gewenste Azure regio's te verdelen. Dit vermindert de aanvraag latentie waargenomen door consumenten API geografisch verspreid en verbetert de beschikbaarheid van de service ook als één gebied off line is. 

Wanneer een service Management API in eerste instantie wordt gemaakt, bevat slechts één [eenheid][] en bevindt zich in een enkel Azure het gebied is aangewezen als de primaire regio. Meer regio's kunnen eenvoudig worden toegevoegd via Azure klassieke Portal. Gateway-server Management API voor elke regio wordt geïmplementeerd en call-verkeer wordt doorgestuurd naar de dichtstbijzijnde gateway. Als een regio off line gaat, wordt het verkeer opnieuw automatisch gestuurde naar de volgende dichtstbijzijnde gateway. 

> [AZURE.IMPORTANT] Implementatie in meerdere landen/regio is alleen beschikbaar in de **[premie][]** laag.

## <a name="add-region"> </a>Een exemplaar van de service Management API implementeren op een nieuwe regio

Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API om te beginnen. Hiermee gaat u naar de portal Management API publisher.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de zelfstudie [aan de slag met Azure API beheer][] .

Ga naar het tabblad **schaal** in Azure klassieke Portal voor uw exemplaar van de service Management API. 

![Tabblad schaal][api-management-scale-service]

Als u wilt implementeren op een nieuw gebied, klikt u op de vervolgkeuzelijst onder het primaire regio en kies een regio in de lijst.

![Regio toevoegen][api-management-add-region]

Nadat u de regio hebt geselecteerd, kiest u het aantal eenheden voor het nieuwe gebied uit de vervolgkeuzelijst eenheid.

![Eenheden opgeven][api-management-select-units]

Nadat de gewenste regio's en de eenheden zijn geconfigureerd, klikt u op **Opslaan**.

## <a name="remove-region"> </a>Een exemplaar van de service Management API uit een gebied verwijderen

Als een exemplaar van de service Management API uit een regio wilt verwijderen, gaat u naar het tabblad **schaal** in Azure klassieke Portal voor uw exemplaar van de service Management API. 

![Tabblad schaal][api-management-scale-service]

Klik op de **X** rechts van het gewenste gebied te verwijderen.  

![Regio verwijderen][api-management-remove-region]

Zodra de gewenste regio's worden verwijderd, klikt u op **Opslaan**.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance
[Aan de slag met Azure API beheer]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[eenheid]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

