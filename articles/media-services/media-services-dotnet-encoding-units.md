<properties 
    pageTitle="Het toevoegen van versleuteling eenheden" 
    description="Meer informatie over het toevoegen van versleuteling eenheden met .NET"  
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016"
    ms.author="juliako;milangada;gtrifonov"/>


#<a name="how-to-scale-encoding-with-net-sdk"></a>Schaalaanpassing van codering met .NET SDK

> [AZURE.SELECTOR]
- [Portal](media-services-portal-scale-media-processing.md )
- [.NET](media-services-dotnet-encoding-units.md)
- [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Overzicht

>[AZURE.IMPORTANT] Zorg ervoor dat het onderwerp [Overzicht](media-services-scale-media-processing-overview.md) voor meer informatie over de schaal media onderwerp verwerking bekijken.
 
Als u wilt wijzigen in het eenheidstype gereserveerd en het aantal gereserveerde eenheden met behulp van .NET SDK codering, het volgende doen:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);
    
    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();
    
    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

##<a name="opening-a-support-ticket"></a>U opent een Support Ticket

Elke Media Services-account kunt schalen tot maximaal 25 standaard codering en 5 On-Demand Streaming gereserveerde eenheden. U kunt een hogere grenswaarde aanvragen via een support ticket.

###<a name="open-a-support-ticket"></a>Een support ticket openen

U opent een support ticket als volgt:

1. Klik op [ondersteuning](https://manage.windowsazure.com/?getsupport=true). Als u niet bent aangemeld, wordt u gevraagd uw referenties op te geven.

1. Selecteer uw abonnement.

1. Selecteer onder type ondersteuning "Technisch".

1. Klik op 'Ticket maken'.

1. Selecteer 'Azure Media Services' in de lijst met producten op de volgende pagina weergegeven.

1. Selecteer een probleemtype"" dat geschikt is voor uw probleem.

1. Klik op Doorgaan.

1. Volg de instructies op de volgende pagina en voert u de details over uw probleem.

1. Klik op verzenden om het ticket te openen.



##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
