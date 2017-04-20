<properties
    pageTitle="Logica in de Media Services SDK voor .NET opnieuw | Microsoft Azure"
    description="Het onderwerp geeft een overzicht van de logica opnieuw in de Media Services SDK voor .NET."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>


# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Opnieuw de logica in de Media Services SDK voor .NET

Als u werkt met Microsoft Azure services, kunnen tijdelijke fouten optreden. Als een tijdelijke storing in de meeste gevallen optreedt, na een paar pogingen de bewerking is geslaagd. De Media Services SDK voor .NET opnieuw logica voor de afhandeling van tijdelijke fouten die zijn gekoppeld aan uitzonderingen en fouten die zijn veroorzaakt door webverzoeken uitvoeren van query's voor het opslaan van wijzigingen en opslagbewerkingen worden geïmplementeerd.  Standaard wordt de Media Services SDK voor .NET vier pogingen uitgevoerd vóór de toepassing van de uitzondering opnieuw genereren. Deze uitzondering moet vervolgens goed omgaan met de code in uw toepassing.  
  
 Hieronder volgt een korte Richtsnoer van het Web aanvragen, opslag, Query- en SaveChanges beleid:  
  
-   Het beleid van de opslag wordt gebruikt voor blob storage bewerkingen (uploaden of downloaden van bestanden van activa).  
  
-   Web aanvraagbeleid wordt voor algemene aanvragen (bijvoorbeeld voor een verificatietoken ophalen en het oplossen van het cluster gebruikers eindpunt) gebruikt.  
  
-   Het querybeleid wordt gebruikt voor het opvragen van entiteiten van REST (bijvoorbeeld mediaContext.Assets.Where(...)).  
  
-   Het beleid SaveChanges wordt gebruikt voor iets dat gegevens binnen de service (bijvoorbeeld het maken van een entiteit voor het bijwerken van een entiteit, een servicefunctie aanroept voor een bewerking).  
  
 In dit onderwerp worden de typen uitzondering en foutcodes die worden verwerkt door de Media Services SDK voor .NET logica opnieuw uit.  
  
## <a name="exception-types"></a>Uitzondering typen  

De volgende tabel beschrijft de uitzonderingen die de Media Services SDK voor .NET worden verwerkt of verwerkt niet voor bepaalde bewerkingen die tijdelijke fouten kunnen veroorzaken.  
  
Uitzondering|Webaanvraag|Opslag|Query|SaveChanges
----|------|----|---|---
WebException<br/>Zie voor meer informatie de sectie [WebException statuscodes](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) .|Ja|Ja|Ja|Ja  
DataServiceClientException<br/> Zie [HTTP-fout statuscodes](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode)voor meer informatie.|Nee|Ja|Ja|Ja
DataServiceQueryException<br/> Zie [HTTP-fout statuscodes](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode)voor meer informatie.|Nee|Ja|Ja|Ja  
DataServiceRequestException<br/> Zie [HTTP-fout statuscodes](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode)voor meer informatie.|Nee|Ja|Ja|Ja  
DataServiceTransportException|Nee|Nee|Ja|Ja
TimeoutException|Ja|Ja|Ja|Nee
SocketException|Ja|Ja|Ja|Ja  
StorageException|Nee|Ja|Nee|Nee 
IOException|Nee|Ja|Nee|Nee
  
###  <a name="WebExceptionStatus"></a>WebException-statuscodes  

De volgende tabel ziet u welke foutcodes WebException de logica opnieuw wordt geïmplementeerd. De opsomming [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) definieert de statuscodes.  
  
Status|Webaanvraag|Opslag|Query|SaveChanges  
-----|-----------------|-------------|-----------|----------  
ConnectFailure|Ja|Ja|Ja|Ja
NameResolutionFailure|Ja|Ja|Ja|Ja  
ProxyNameResolutionFailure|Ja|Ja|Ja|Ja  
SendFailure|Ja|Ja|Ja|Ja
PipelineFailure|Ja|Ja|Ja|Nee  
ConnectionClosed|Ja|Ja|Ja|Nee  
KeepAliveFailure|Ja|Ja|Ja|Nee  
UnknownError|Ja|Ja|Ja|Nee 
ReceiveFailure|Ja|Ja|Ja|Nee  
RequestCanceled|Ja|Ja|Ja|Nee  
Time-out|Ja|Ja|Ja|Nee
Een protocolfout <br/>De poging van een protocolfout wordt bepaald door de verwerking van HTTP status code. Zie [HTTP-fout statuscodes](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode)voor meer informatie.|Ja|Ja|Ja|Ja|  
  
###  <a name="HTTPStatusCode"></a>HTTP-fout statuscodes  

Wanneer Query en SaveChanges throw DataServiceClientException, DataServiceQueryException of DataServiceQueryException, wordt de status van HTTP-foutcode geretourneerd in de eigenschap StatusCode.  De volgende tabel ziet u welke foutcodes de logica opnieuw wordt geïmplementeerd.  
  
 
Status|Webaanvraag|Opslag|Query|SaveChanges 
---|----|----|----|----
401|Nee|Ja|Nee|Nee
403|Nee|Ja<br/>Pogingen met langer wachten op verwerking.|Nee|Nee  
408|Ja|Ja|Ja|Ja
429|Ja|Ja|Ja|Ja  
500|Ja|Ja|Ja|Nee  
502|Ja|Ja|Ja|Nee  
503|Ja|Ja|Ja|Ja  
504|Ja|Ja|Ja|Nee  
  
Als u wilt nemen een blik op de daadwerkelijke uitvoering van de Media Services SDK voor .NET logica opnieuw, Zie [azure-sdk-voor-media-services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
