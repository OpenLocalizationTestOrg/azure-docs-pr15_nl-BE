<properties
    pageTitle="Het bijsnijden van video | Microsoft Azure"
    description="In dit artikel ziet u hoe video's met Media Encoder Standard bijsnijden."
    services="media-services"
    documentationCenter=""
    authors="anilmur"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"  
    ms.author="anilmur;juliako;"/>

# <a name="crop-videos-with-media-encoder-standard"></a>Video's bijsnijden met Media Encoder Standard

Media Encoder Standard (MES) kunt u uw invoer video bijsnijden. Uitsnijden is het proces van een rechthoekig venster in het videoframe te selecteren en de codering alleen de pixels in dat venster. In het volgende diagram kunt illustreren het proces.

![Een video bijsnijden](./media/media-services-crop-video/media-services-crop-video01.png)

Stel, dat u hebt als invoer een video die heeft een resolutie van 1920 x 1080 pixels (hoogte-breedteverhouding van 16:9), maar zwarte balken (pijler vakken) op de links en rechts, zodat alleen een 4:3-venster of 1440 x 1080 pixels bevat actieve video. U kunt bijsnijden of het bewerken van de zwarte balken met MES en coderen van de regio 1440 x 1080.

Bijsnijden in het MES is een pre-processing fase bijsnijden in de voorinstelling codering parameters van toepassing op de oorspronkelijke video input. Codering is een latere fase en de hoogte/breedte-instellingen zijn van toepassing op de *vooraf verwerkt* video, en niet op de oorspronkelijke video. Bij het ontwerpen van de voorinstelling moet u het volgende doen: (a) selecteert op basis van de oorspronkelijke video input parameters bijsnijden en (b) de instellingen op basis van de bijgesneden video te coderen. Als u niet overeenkomen met de instellingen van de bijgesneden video te coderen, de uitvoer wordt niet zoals verwacht.

Het [volgende](media-services-advanced-encoding-with-mes.md#encoding_with_dotnet) onderwerp bevat een codering project maken met MES en hoe u een aangepaste voorinstelling voor de codering taak opgeeft. 

## <a name="creating-a-custom-preset"></a>Een aangepaste voorinstelling maken

In het voorbeeld in het diagram weergegeven:

1. Oorspronkelijke invoer is 1920 x 1080
1. Deze moeten worden bijgesneden met een vermogen van 1440 x 1080, die in het kader van de invoer wordt gecentreerd
1. Dit betekent dat een X-verschuiving van (1920-1440) / 2 = 240 en een Y-verschuiving van nul
1. De breedte en hoogte van de rechthoek snijden zijn 1440 en 1080, respectievelijk
1. In het werkgebied coderen de vraag is voor de productie van drie lagen, zijn resoluties van 1440 x 1080, 960 x 720 en 480 x 360, respectievelijk

###<a name="json-preset"></a>JSON-voorinstelling


    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


##<a name="restrictions-on-cropping"></a>Beperkingen voor bijsnijden

De functie voor bijsnijden handmatige bedoeld. U moet uw input video laden in een geschikte bewerkingsprogramma waarmee u frames van belang selecteren, plaatst u de cursor om te bepalen van de offsets voor het snijkader, bepalen de voorinstelling voor tekencodering die is afgestemd op die bepaalde video's, enzovoort. Deze functie is niet bedoeld voor zaken als: automatische detectie en verwijdering van zwarte letterbox/pillarbox randen in uw video input.

Volgende beperkingen van toepassing op de functie voor bijsnijden. Als deze niet wordt voldaan, kan het coderen taak mislukt of een onverwachte uitvoer produceren.

1. De coördinaten en de grootte van de rechthoek snijden moeten passen binnen de video input
1. Zoals hierboven vermeld, hebt de breedte en hoogte in de instellingen voor coderen overeen te komen met de bijgesneden video
1. Bijsnijden is van toepassing op de video's die zijn vastgelegd in de modus Liggend (dat wil zeggen niet van toepassing op video's opgenomen met een smartphone die verticaal of horizontaal of verticaal gebruik)
1. Werkt het best met progressieve video opgenomen met vierkante pixels

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Volgende stap
 
Zie Azure Media Services paden leren om te leren werken met geweldige functies aangeboden door AMS.  

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
