<properties
    pageTitle="Logica App tijdslimieten en de instellingen | Microsoft Azure"
    description="Overzicht van de grenzen van de service en de configuratiewaarden voor logica Apps beschikbaar."
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="logic-app-limits-and-configuration"></a>Logica App limieten en configuratie

Hieronder vindt u informatie over de huidige limieten en configuratiegegevens voor Azure logica Apps.

## <a name="limits"></a>Limieten

### <a name="http-request-limits"></a>HTTP-verzoeklimieten

Dit zijn limieten voor één HTTP-aanvraag en/of verbindingslijn aanroep

#### <a name="timeout"></a>Time-out

|Naam|Limiet|Notities|
|----|----|----|
|Time-out voor aanvraag|1 minuut|Een [asynchrone patroon](app-service-logic-create-api-app.md) of [tot lus](app-service-logic-loops-and-scopes.md) kan compenseren indien nodig|

#### <a name="message-size"></a>Grootte van het bericht

|Naam|Limiet|Notities|
|----|----|----|
|Grootte van het bericht|50 MB|API's en sommige verbindingslijnen mogelijk geen ondersteuning voor 50MB.  Aanvraag trigger ondersteunt tot 25MB|
|Expressie-evaluatie-limiet|131.072 tekens|`@concat()`, `@base64()`, `string` mag niet langer zijn dan deze|

#### <a name="retry-policy"></a>Beleid opnieuw

|Naam|Limiet|Notities|
|----|----|----|
|Herhalingspogingen|4|Met de [parameter van het beleid opnieuw](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx) kunt configureren|
|Maximale wachttijd|1 uur|Met de [parameter van het beleid opnieuw](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx) kunt configureren|
|Min vertraging|20 min|Met de [parameter van het beleid opnieuw](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx) kunt configureren|

### <a name="run-duration-and-retention"></a>Run duur en bewaren

Dit zijn de limieten voor een enkele logica app uitvoeren.

|Naam|Limiet|Notities|
|----|----|----|
|Duur uitvoering|90 dagen||
|Opslag bewaren|90 dagen|Dit is van de uitvoering van de begintijd|
|Interval van het terugkeerpatroon min|15 sec.||
|Terugkeerpatroon voor max.|500 dagen||


### <a name="looping-and-debatching-limits"></a>Doorlopen en limieten voor debatching

Dit zijn de limieten voor een enkele logica app uitvoeren.

|Naam|Limiet|Notities|
|----|----|----|
|ForEach artikelen|5.000|U kunt de [queryactie](../connectors/connectors-native-query.md) groter matrices desgewenst filteren|
|Tot herhalingen|10.000||
|SplitOn artikelen|5.000||
|ForEach-parallellisme|20|U kunt instellen op een sequentiële foreach toe te voegen `"operationOptions": "Sequential"` aan de `foreach` actie|


### <a name="throughput-limits"></a>Limieten voor doorvoer

Dit zijn de limieten voor een enkele logica app-exemplaar. 

|Naam|Limiet|Notities|
|----|----|----|
|Triggers per seconde|100|Werkstromen verdelen over meerdere apps, indien nodig|

### <a name="definition-limits"></a>Definitie van grenzen

Dit zijn de limieten voor een definitie van enkele logica app.

|Naam|Limiet|Notities|
|----|----|----|
|Acties in een foreach-constructie|1|U kunt geneste werkstromen om uit te breiden dit zo nodig toevoegen|
|Acties per werkstroom|60|U kunt geneste werkstromen om uit te breiden dit zo nodig toevoegen|
|Actie nesten diepte toegestaan|5|U kunt geneste werkstromen om uit te breiden dit zo nodig toevoegen|
|Overdrachten per regio, per abonnement|1000||
|Triggers per werkstroom|10||
|Maximum aantal tekens per expressie|8192||
|Max `trackedProperties` grootte in tekens|16.000|
|`action`/`trigger`limiet|80||
|`description`maximale lengte|256||
|`parameters`limiet|50||
|`outputs`limiet|10||

## <a name="configuration"></a>Configuratie

### <a name="ip-address"></a>IP-adres

Oproepen vanuit een [connector](../connectors/apis-list.md) is afkomstig van het IP-adres hieronder.

Oproepen vanuit een logica app rechtstreeks (dat wil zeggen via [HTTP](../connectors/connectors-native-http.md) of [http- + Swagger](../connectors/connectors-native-http-swagger.md)) afkomstig zijn uit een van de [Azure Datacenter IP-bereiken](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

|Logica App regio|Outbound IP|
|-----|----|
|Australië, Oost|40.126.251.213|
|Zuidoost Australië|40.127.80.34|
|Brazilië-Zuid|191.232.38.129|
|Centraal India|104.211.98.164|
|Centrale VS|40.122.49.51|
|Oost-Azië|23.99.116.181|
|Oost-Verenigde Staten|191.237.41.52|
|Oost-Verenigde Staten 2|104.208.233.100|
|East Japan|40.115.186.96|
|West Japan|40.74.130.77|
|Centrale Noord-Amerikaanse|65.52.218.230|
|Noord-Europa|104.45.93.9|
|Zuid-centraal-Verenigde Staten|104.214.70.191|
|Zuidoost-Azië|13.76.231.68|
|Zuid-India|104.211.227.225|
|West-Europa|40.115.50.13|
|West-India|104.211.161.203|
|West-Verenigde Staten|104.40.51.248|


## <a name="next-steps"></a>Volgende stappen  

- Volg de zelfstudie voor het [maken van een App logica](app-service-logic-create-a-logic-app.md) om te beginnen met logica Apps.  
- [Veelvoorkomende scenario's en voorbeelden weergeven](app-service-logic-examples-and-scenarios.md)
- [U kunt automatiseren van bedrijfsprocessen met logica Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Leren hoe u uw systemen integreren met logica Apps](http://channel9.msdn.com/Events/Build/2016/P462)