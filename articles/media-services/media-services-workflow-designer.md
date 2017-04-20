<properties 
    pageTitle="Geavanceerde codering werkstromen maken met Workflow Designer | Microsoft Azure" 
    description="Meer informatie over geavanceerde codering om workflows te maken met de werkstroomontwerper gaan werken." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;johndeu;anilmur"/>


#<a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Geavanceerde codering werkstromen maken met de werkstroomontwerper

##<a name="overview"></a>Overzicht

De **Workflow Designer** is een Windows desktop tool die wordt gebruikt voor het ontwerpen en bouwen van aangepaste werkstromen voor het coderen van **Media Encoder Premium Workflow**.
U kunt met behulp van de kracht van de workflow designer tool ontwerpen en complexe workflows maken die wordt uitgevoerd in de **Media Encoder Premium**.  

Werkstromen kunnen klant besluit logica en vertakking op basis van de eigenschappen van het bestand invoerbron. U kunt werkstromen maken met overridable eigenschappen en dynamische waarden dat zelfs de meest complexe codering taken gemakkelijker te herhalen en aanpassen in de cloud.

Voorbeeld van de werkstromen die u kunt maken zijn:

- Besluit op basis van werkstromen voor het controleren van de broninhoud voor resolutie en alleen de nummers van de gewenste uitvoer coderen.  Dit is helfpul doordat de ongebruikte nummers die zou worden gegenereerd door de bron inhoud per ongeluk upscaling.
- Meerdere invoerbestanden kunnen worden gebruikt ter ondersteuning van bijschriften, overlays en hechten samen inhoud. 

Dit hulpprogramma kan ook worden gebruikt voor het wijzigen van een van onze [werkstromen gepubliceerd](media-services-workflow-designer.md#existing_workflows). 

>[AZURE.NOTE]Als u uw exemplaar van de Workflow Designer tool, neem contact op met mepd@microsoft.com.


Nadat een workflow-bestand is gemaakt, als een actief kan worden geüpload en vervolgens worden gebruikt voor het coderen van mediabestanden. Zie [Geavanceerde codering met Media Encoder Premium werkstroom](media-services-encode-with-premium-workflow.md)voor meer informatie over het coderen van **Media Encoder Premium Workflow** met **.NET**.

##<a id="existing_workflows"></a>Bestaande werkstromen wijzigen

De standaard [gepubliceerd werkstromen](media-services-workflow-designer.md#existing_workflows) kunnen worden gewijzigd met het hulpprogramma designer. Krijgt u standaard workflow bestanden [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). De map bevat ook de beschrijving van deze bestanden.

De volgende video's laten zien hoe u met de ontwerper.

###<a name="day-1--getting-started"></a>Dag 1: Introductie

Dag 1 video heeft betrekking op:

- Designer-overzicht
- Eenvoudige Workflows – 'Hello World'
- Meerdere maken voor gebruik met streaming Media-Services Azure MP4-bestanden uitvoeren

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-1]

###<a name="day-2"></a>Dag 2

Dag 2 video heeft betrekking op:

- Verschillende scenario's voor bron bestand – audio verwerking
- Werkstromen met geavanceerde logica
- Fasen van de grafiek

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-2]

###<a name="day-3"></a>Dag 3

Dag 3 video heeft betrekking op:

- Scripting in Workflows/blauwdrukken
- Beperkingen met de huidige Encoder
- Q & A
 
> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-3]


## <a name="next-step"></a>Volgende stap

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


Als u moet ondersteuning of vragen over het maken van aangepaste werkstromen in de Workflow designer gereedschap hebt, stuur e-mail naar mepd@microsoft.com.

##<a name="see-also"></a>Zie ook

[Azure Premium Encoder Workflow Designer trainingsvideo 's](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)
