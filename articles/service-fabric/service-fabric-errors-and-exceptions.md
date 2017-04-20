<properties
   pageTitle="Algemene FabricClient uitzonderingen | Microsoft Azure"
   description="Beschrijving van de algemene uitzonderingen en fouten die door de APIs FabricClient kunnen worden gegenereerd bij het uitvoeren van de toepassing en het beheer van clusterbewerkingen."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Algemene uitzonderingen en fouten bij het werken met de APIs FabricClient
De [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -API's kunnen cluster- en beheerders beheertaken uitvoeren op een toepassing, service of cluster van Fabric-Service. Bijvoorbeeld toepassingsimplementatie upgrade en verwijdering en controle van de gezondheid van een cluster of een service te testen. Softwareontwikkelaars en clusterbeheerders kunnen de APIs FabricClient gebruiken voor het ontwikkelen van hulpprogramma's voor het beheren van de cluster-Service Fabric en toepassingen.

Er zijn verschillende soorten bewerkingen die kunnen worden uitgevoerd met behulp van FabricClient.  Elke methode kunt uitzonderingen voor fouten als gevolg van onjuiste invoer, runtime-fouten of problemen van voorbijgaande aard infrastructuur genereren.  Zie de documentatie van de API te vinden welke uitzonderingen worden veroorzaakt door een specifieke methode. Er zijn enkele uitzonderingen, maar die door veel andere [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -API's kunnen worden gegenereerd. De volgende tabel worden de uitzonderingen die gebruikt in de APIs FabricClient worden.

|Uitzondering| Wanneer veroorzaakt|
|---------|:-----------|
|[System.Fabric.FabricObjectClosedException](https://msdn.microsoft.com/library/system.fabric.fabricobjectclosedexception.aspx)|Het object [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) is in een gesloten toestand. Buitengebruikstelling van het object [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) u gebruikt en een nieuw [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -object instantiëren. |
|[System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)|Er is een time-out opgetreden bij de bewerking. [OperationTimedOut](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) wordt geretourneerd als de bewerking meer dan MaxOperationTimeout is te voltooien.|
|[System.UnauthorizedAccessException](https://msdn.microsoft.com/en-us/library/system.unauthorizedaccessexception.aspx)|De toegangscontrole voor de bewerking is mislukt. E_ACCESSDENIED wordt geretourneerd.|
|[System.Fabric.FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)|Er is een runtime-fout opgetreden tijdens het uitvoeren van de bewerking. Een van de methoden FabricClient [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)mogelijk kunnen genereren, de eigenschap [ErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricexception.errorcode.aspx) geeft de exacte oorzaak van de uitzondering. Foutcodes zijn gedefinieerd in de opsomming [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) .|
|[System.Fabric.FabricTransientException](https://msdn.microsoft.com/library/system.fabric.fabrictransientexception.aspx)|De bewerking is mislukt vanwege een tijdelijke fout van het type. Bijvoorbeeld, mislukken een bewerking omdat een quorum van replica's tijdelijk niet bereikbaar is. Tijdelijke uitzonderingen komen overeen met de mislukte bewerkingen die opnieuw kunnen worden verzonden.|

Sommige [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) fouten die vaak in een [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)kunnen worden geretourneerd:

|Fout| Voorwaarde|
|---------|:-----------|
|CommunicationError|Een communicatiefout veroorzaakt de bewerking is mislukt, probeer het opnieuw.|
|InvalidCredentialType|Het referentietype is ongeldig.|
|InvalidX509FindType|De X509FindType is ongeldig.|
|InvalidX509StoreLocation|De X509 Winkellocatie is ongeldig.|
|InvalidX509StoreName|De X509 de naam is ongeldig.|
|InvalidX509Thumbprint|De X509 certificaat vingerafdruk tekenreeks is ongeldig.|
|InvalidProtectionLevel|Het niveau van bescherming is ongeldig.|
|InvalidX509Store|De X509-archief kan niet worden geopend.|
|InvalidSubjectName|Naam van de certificaathouder is ongeldig.|
|InvalidAllowedCommonNameList|De indeling van de gemeenschappelijke lijsttekenreeks is ongeldig. Deze moet een door komma's gescheiden lijst.|
