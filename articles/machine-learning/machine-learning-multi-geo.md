<properties
   pageTitle="Multi-Geo Help-documentatie van | Microsoft Azure"
   description="Informatie over hoe een werkruimte maken en publiceren van een webservice in een Azure-regio van de Zuid-centraal Verenigde Staten (SCUS) verschillende Azure regio."
   services="machine-learning"
   documentationCenter=""
   authors="tedway"
   manager="jhubbard"
   editor="rmca14"
   tags=""/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="tedway; neerajkh"/>

# <a name="multi-geo-help-documentation"></a>Multi-Geo Help-documentatie

In dit artikel wordt beschreven hoe u een werkruimte maken en publiceren van een webservice in andere gebieden Azure.

## <a name="create-a-workspace"></a>Een werkruimte maken

1. Aanmelden bij de klassieke Azure Portal.

2.  Klik op **+ Nieuw** > **DATA SERVICES** > **MACHINE LEARNING** > **snel maken**.  Selecteer onder **positie van** een ander gebied, zoals **Zuidoost-Azië**.
![Multi-Geo Help afbeelding 1][1]
3. Selecteer de werkruimte en klik vervolgens op **aanmelden bij ML Studio**.
![Multi-Geo Help afbeelding 2][2]

4. U hebt nu een werkruimte in een ander gebied dat u net als elke andere werkruimte kan gebruiken. U kunt schakelen tussen uw werkruimten, kijken naar de rechterbovenhoek van het scherm. Klik op de vervolgkeuzelijst, selecteer de regio en selecteert u de werkruimte. Alles bevindt zich in de werkruimte regio; bijvoorbeeld, worden alle webservices gemaakt op basis van een werkruimte in de werkruimte bevindt zich in hetzelfde gebied.
![Multi-Geo Help afbeelding 3][3]

## <a name="open-an-experiment-from-gallery"></a>Open een experiment uit galerie

Als u een experiment uit galerie opent, kunt u ook welke regio u wilt kopiëren van het experiment te selecteren.

![Multi-Geo Help afbeelding 4][4a]

## <a name="web-service-management"></a>Webservice beheren

Gebruiken om programmatisch beheren van webservices, zoals voor omscholing, het specifieke adres:
- https://asiasoutheast.Management.azureml.NET
- https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Dingen om te onthouden

1.  U kunt alleen experimenten tussen werkruimten die deel uitmaken van dezelfde regio op deze manier kopiëren. Als u wilt kopiëren experiment over werkruimten in verschillende regio's, kunt u de [PowerShell](http://aka.ms/amlps) -commandlet [*Kopie AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) hiervoor. Een andere oplossing is voor het publiceren van het experiment in de galerie in de niet-genoteerde modus vervolgens openen in de werkruimte van de andere regio's.
2.  Regio kiezer zal alleen werkruimten voor één regio tegelijk weergeven. U wordt in de toekomst een volledige lijst met werkruimten die u toegang tot in alle regio's op hetzelfde moment hebt te zien zijn.  
3.  Een gratis werkruimte of gasttoegang (anoniem) werkruimte wordt gemaakt en gehost in Centraal Zuid-Amerikaanse In de toekomst is u mogelijk vrije/gasttoegang om werkruimten te maken in de regio die u kiest.  
4.  Webservices geïmplementeerd via een werkruimte in Zuidoost-Azië ook ondergebracht in Zuidoost-Azië. In de toekomst is het mogelijk om de flexibiliteit van experimenten in een regio maken en implementeren van eindpunten van een webservice gegenereerd in verschillende regio's.  

## <a name="more-information"></a>Meer informatie

Een vraag stellen op het [forum Azure Machine Learning](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
