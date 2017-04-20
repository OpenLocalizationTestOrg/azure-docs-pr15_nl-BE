<properties 
    pageTitle="Voertuig telemetrie analytics oplossing playbook | Microsoft Azure" 
    description="Met de functionaliteit van Cortana Intelligence te krijgen van real-time en voorspellende inzichten voertuig gezondheid en rijden gewoonten." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Voertuig telemetrie analytics oplossing playbook

Dit **menu** bevat koppelingen naar de hoofdstukken in deze playbook. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Overzicht
Super computers zijn uit het lab hebt verplaatst en nu in onze garage! Deze geavanceerde auto bevatten een groot aantal sensoren, zodat deze kunnen bijhouden en controleren van miljoenen gebeurtenissen per seconde. We verwachten dat in 2020, de meeste van deze auto's zal hebben is verbonden met het internet. Stel dat u toegang te krijgen tot deze schat aan gegevens naar het beste in de klasse veiligheid, betrouwbaarheid en bestuurder ervaring bieden! Microsoft heeft deze droom werkelijkheid met Cortana intelligentie gesteld.

Van Microsoft Cortana Intelligence is een volledig beheerde, big data en geavanceerde analytics suite waarmee u uw gegevens transformeren naar intelligente actie. Wij willen u kennis met de sjabloon Cortana Intelligence voertuig telemetrie Analytics oplossing. Deze oplossing wordt gedemonstreerd hoe auto dealers auto fabrikanten en verzekeringsmaatschappijen de mogelijkheden van Cortana Intelligence krijgen real-time gebruiken en gewoonten voorspellende inzichten voertuig gezondheid en rijden kunnen. 

De oplossing wordt geïmplementeerd als een [patroon van lambda-architectuur](https://en.wikipedia.org/wiki/Lambda_architecture) met het volledige potentieel van de Cortana Intelligence platform voor realtime en batch-verwerking. De oplossing: 

- biedt een voertuig telematica-simulator
- maakt gebruik van Hubs gebeurtenis voor miljoenen gesimuleerde voertuig telemetrie gebeurtenissen in Azure ingesting 
- Stream Analytics gebruikt om het real-time inzicht op de gezondheid van voertuig krijgen
-  de gegevens aanhoudt in de opslag voor langere termijn voor rijkere batch analytics. 
- maakt gebruik van Machine Learning voor de detectie van de afwijking in real-time en batch-verwerking voorspellende inzicht krijgen.
- maakt gebruik van HDInsight om gegevens op schaal en Data Factory verwerken orchestration, planning, resourcemanagement en monitoring van de pijpleiding batch-verwerking 
- Deze oplossing resulteert in een rijke dashboard voor real-time gegevens en voorspellende analytics visualisaties met Power BI

## <a name="architecture"></a>Architectuur

![](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Afbeelding 1: het voertuig telemetrie Analytics Solution Architecture*

Deze oplossing omvat de volgende **onderdelen van Cortana Intelligence** en toonbeeld geeft van de end-to-end integratie


- **Gebeurtenis Hubs** voor miljoenen voertuig telemetrie gebeurtenissen in Azure ingesting.
- **Stream Analytics** voor real-time inzicht op de gezondheid van het voertuig en die gegevens in de opslag voor langere termijn voor rijkere batch analytics zich blijft voordoen.
- **Machine Learning** voor de detectie van de afwijking in real-time en batch-verwerking voorspellende inzichten te krijgen.
- **HDInsight** wordt gebruikt om gegevens op schaal transformeren
- **Data Factory** verwerkt orchestration, Bronnenbeheer plannen, en de controle van de pijpleiding batch-verwerking.
- **Power BI** biedt deze oplossing een rijke dashboard voor realtime-gegevens en voorspellende analytics visualisaties.

Deze oplossing opent twee verschillende **gegevensbronnen**: 

- **Gesimuleerde voertuig signalen en diagnostische gegevens**: een voertuig telematica simulator zendt de diagnostische gegevens en signalen die overeenkomen met de staat van het voertuig en de bestuurder patroon op een bepaald moment in de tijd. 
- **Voertuig catalogus**: een verwijzing naar dataset met een Chassisnummer model toewijzen aan.
