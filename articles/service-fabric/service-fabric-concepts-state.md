<properties
   pageTitle="Definiëren en beheren van de status | Microsoft Azure"
   description="Het definiëren en beheren in Fabric-Service de status van service"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="service-state"></a>De status van service
**De status van service** verwijst naar de gegevens die de service is vereist. Het bevat de gegevensstructuren en variabelen die de service leest en schrijft naar werken.

Houd rekening met een eenvoudige rekenmachine-service, bijvoorbeeld. Deze service duurt twee getallen en geeft als resultaat de som. Dit is een zuiver stateless service zonder gegevens gekoppeld is.

Bekijk nu de Rekenmachine van dezelfde, maar naast de som berekenen, heeft ook een methode voor het retourneren van de laatste som die deze heeft berekend. Deze service is nu stateful--het bevat enkele met schrijft naar (bij het berekenen van een totaal nieuwe) en leest uit (als het resultaat de laatste berekende som).

In Azure Service Fabric, de eerste service een stateless service genoemd. De tweede service heet een stateful service.

## <a name="storing-service-state"></a>De status van service op te slaan
Status kan worden externalized of samen met de code voor het manipuleren van de staat is te vinden. Externalization van de staat wordt meestal gedaan met behulp van een externe database of een winkel. Dit kan een SQL-database waarin het huidige resultaat is opgeslagen in een tabel zijn in ons voorbeeld Rekenmachine. Elke aanvraag voor het berekenen van de som voert een update op deze rij.

Staat kan ook collocatie worden geplaatst met de code die in deze code wordt bewerkt. Stateful services Service stof zijn gebouwd met behulp van dit model. Fabric-service levert de infrastructuur om ervoor te zorgen dat deze toestand sterk beschikbaar is en fouttolerant in geval van een storing.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie op Fabric Service concepten:

- [Beschikbaarheid van de Service configuratieservices](service-fabric-availability-services.md)

- [Schaalbaarheid van Service configuratieservices](service-fabric-concepts-scalability.md)

- [Service configuratieservices partitioneren](service-fabric-concepts-partitioning.md)
