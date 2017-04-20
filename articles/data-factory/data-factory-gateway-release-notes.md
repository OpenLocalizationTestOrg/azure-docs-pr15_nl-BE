<properties 
    pageTitle="Release-opmerkingen voor Data Management Gateway | Azure Data Factory" 
    description="Data Management Gateway taire release-opmerkingen" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="spelluru"/>

# <a name="release-notes-for-data-management-gateway"></a>Release-opmerkingen voor Data Management Gateway
Een van de uitdagingen voor moderne gegevensintegratie is naadloos om gegevens te verplaatsen naar en op ruimten naar cloud. Data Factory kunt u naadloos met Gateway voor gegevens die een agent die u kunt installeren op locatie zodat hybride gegevens verplaatsen door deze integratie.

Raadpleeg de volgende artikelen voor meer informatie over Data Management Gateway en hoe u deze gebruikt: 

- [Data Management Gateway](data-factory-data-management-gateway.md)
- [Gegevens verplaatsen tussen op gebouwen en cloud met Azure Data Factory](data-factory-move-data-between-onprem-and-cloud.md) 

## <a name="current-version-2260721"></a>Huidige versie (2.2.6072.1)

- Ondersteunt het instellen van de HTTP-proxy voor de gateway met behulp van de Gateway Configuration Manager. Indien geconfigureerd, Azure Blob, Azure tabel Azure gegevens Lake en DB-Document via HTTP proxy.
- Ondersteunt kop verwerking voor tekstopmaak bij het kopiëren van gegevens van/naar Azure Blob, Azure Lake gegevensarchief, bestandssysteem lokalen en HDFS gebouwen.
- Ondersteunt het kopiëren van gegevens uit de Blob toevoegen en pagina Blob met de Blob met al ondersteunde blokkeren.
- Introduceert een nieuwe gateway status **on line (beperkt)**, waarmee wordt aangegeven dat de hoofdfunctionaliteit van de gateway met uitzondering van de ondersteuning voor interactieve bewerking voor Wizard kopiëren werkt.
- Verbetert de stabiliteit van de gateway registratie met registratie sleutel.

## <a name="earlier-versions"></a>Eerdere versies

## <a name="2160401"></a>2.1.6040.1

- Stuurprogramma voor DB2 is nu opgenomen in de gateway-installatiepakket. U hoeft niet afzonderlijk te installeren. 
- Stuurprogramma voor DB2 biedt nu ondersteuning voor z/OS en DB2 voor ik (AS / 400) samen met de platforms reeds ondersteund (Linux, Unix en Windows). 
- Ondersteunt DocumentDB met als bron- of doeladres voor gegevensopslag voor gebouwen
- Ondersteunt het kopiëren van gegevens van/tot koud/hot blob-opslag met de account al ondersteunde algemene opslag. 
- Hiermee kunt u verbinding maken met de SQL-Server via een gateway met bevoegdheden voor externe aanmelding voor gebouwen.  

## <a name="2060131"></a>2.0.6013.1

- Kunt u de taal-cultuur door een gateway worden gebruikt tijdens de handmatige installatie.
- Als gateway niet werkt zoals verwacht, kunt u de logboeken van de afgelopen zeven dagen gateway naar Microsoft te verzenden ter vergemakkelijking van het oplossen van het probleem. Als de gateway is niet verbonden met de cloud-service, kunt u opslaan en archiveren van logboekbestanden gateway.  
- Verbeteringen in de gebruikersinterface voor gateway configuration manager:
    - Status van gateway meer zichtbaar maken op het tabblad Start.
    - Besturingselementen opnieuw ingedeelde en vereenvoudigd.
- U kunt gegevens kopiëren uit een opslag met behulp van de [code gratis kopie voorbeeldfunctie](data-factory-copy-data-wizard-tutorial.md). Zie [Kopie klaargezet](data-factory-copy-activity-performance.md#staged-copy) in het algemeen voor meer informatie over deze functie. 
- U kunt Data Management Gateway ingress-gegevens rechtstreeks vanuit een SQL Server-database op de ruimten in Azure Machine Learning.
- Betere prestaties
    - De prestaties op Schema of het afdrukvoorbeeld op de SQL-Server weergeven in voorbeeldfunctie code vrij kopiëren.



## <a name="11259531"></a>1.12.5953.1
- Programmafouten

## <a name="11159181"></a>1.11.5918.1

- Maximale grootte van het gebeurtenislogboek van de gateway is verhoogd van 1 MB tot 40 MB.
- Een waarschuwingsvenster wordt weergegeven in het geval een herstart nodig is tijdens gateway automatisch bijwerken. U kunt rechts Klik of later opnieuw starten. 
- Als automatisch bijwerken niet lukt, pogingen gateway installer driemaal met maximum automatisch bijwerken.
- Betere prestaties
    - De prestaties voor grote tabellen geladen van de server voor ruimten in geval van code vrij kopiëren.
- Programmafouten

## <a name="11058921"></a>1.10.5892.1

- Betere prestaties
- Programmafouten

## <a name="1958652"></a>1.9.5865.2

- Nul updatefunctie touch auto
- Nieuw pictogram in het systeemvak met gateway statusindicatoren
- De mogelijkheid om 'Update nu' van de client
- Update schematijd instellen
- PowerShell script voor het uitschakelen van automatische updates in-of uitschakelen
- Ondersteuning voor JSON-indeling  
- Betere prestaties
- Programmafouten

## <a name="1858221"></a>1.8.5822.1

- Verbeteren van het oplossen van problemen
- Betere prestaties
- Programmafouten

### <a name="1757951"></a>1.7.5795.1

- Betere prestaties
- Programmafouten

### <a name="1757641"></a>1.7.5764.1

- Betere prestaties
- Programmafouten

### <a name="1657351"></a>1.6.5735.1

- Ondersteuning voor on-premises HDFS bron/Sink
- Betere prestaties
- Programmafouten

### <a name="1656961"></a>1.6.5696.1

- Betere prestaties
- Programmafouten

### <a name="1656761"></a>1.6.5676.1

- Ondersteuning van diagnostische hulpprogramma's voor Configuratiebeheer
- Tabelkolommen ondersteuning voor gegevensbronnen in tabelvorm voor Azure Data Factory
- Ondersteuning voor SQL-DW voor Azure Data Factory
- Ondersteuning voor Reclusive in BlobSource en FileSource voor Azure Data Factory
- Ondersteuning voor CopyBehavior-MergeFiles, PreserveHierarchy en FlattenHierarchy in BlobSink en FileSink met binaire kopiëren voor Azure Data Factory
- Ondersteuning van de activiteit van kopie voortgangsrapportage voor Azure Data Factory
- Ondersteuning voor bron connectiviteit gegevensvalidatie voor Azure Data Factory
- Programmafouten


### <a name="1656721"></a>1.6.5672.1

- De tabelnaam ondersteuning voor ODBC-gegevensbron voor Azure Data Factory
- Betere prestaties
- Programmafouten

### <a name="1656581"></a>1.6.5658.1

- Ondersteuningsbestand voor Azure Data Factory wastafel
- Ondersteuning voor het behouden van hiërarchie in binaire kopiëren voor Azure Data Factory
- Kopie activiteit Idempotency ondersteuning voor Azure Data Factory
- Programmafouten

### <a name="1656401"></a>1.6.5640.1

- Ondersteuning voor meer gegevensbronnen 3 voor Azure Data Factory (ODBC OData, HDFS)
- Aanhalingsteken in csv parser ondersteuning voor Azure Data Factory
- Compressieondersteuning (BZip2)
- Programmafouten

### <a name="1556121"></a>1.5.5612.1

- Ondersteuning voor vijf relationele databases voor Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata en Sybase)
- Compressieondersteuning (Gzip en Deflate)
- Betere prestaties
- Programmafouten


### <a name="1455491"></a>1.4.5549.1

- Oracle data source ondersteuning toevoegen voor Azure Data Factory
- Betere prestaties
- Programmafouten

### <a name="1454921"></a>1.4.5492.1

- Centrale binaire die Microsoft Azure Data Factory-en Office 365 Power BI ondersteunt
- De gebruikersinterface voor configuratie en registratie proces verfijnen
- Azure Data Factory – Azure Ingress- en Egress-ondersteuning voor SQL Server-gegevensbron

### <a name="1253031"></a>1.2.5303.1

-   Time-out probleem ter ondersteuning van meer tijdrovende gegevensbronverbindingen oplossen. 
    
### <a name="1155268"></a>1.1.5526.8

- Vereist .NET Framework 4.5.1 vereist tijdens de installatie.

### <a name="1051442"></a>1.0.5144.2

- Geen wijzigingen die van invloed zijn op Azure Data Factory-scenario's. 
