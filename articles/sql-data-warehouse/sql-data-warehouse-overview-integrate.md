<properties
   pageTitle="Geïntegreerde oplossingen bouwen met SQL Data Warehouse | Microsoft Azure"
   description="Hulpprogramma's en oplossingen voor integratie met SQL Data Warehouse-partners. "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="leverage-other-services-with-sql-data-warehouse"></a>Andere services van SQL Data Warehouse
Naast de kernfunctionaliteit kunnen SQL Data Warehouse gebruikers gebruikmaken van veel van de andere diensten in Azure ernaast.  Met name hebben we op dit moment nauw geïntegreerd met de volgende stappen genomen:

+ Power BI
+ Azure Data Factory
+ Azure Machine Learning
+ Azure Stream Analytics

We proberen te verbinden met meer services via het ecosysteem Azure.

##<a name="power-bi"></a>Power BI
Integratie van Power BI kunt u gebruikmaken van de rekenkracht van SQL Data Warehouse dynamische rapportering en visualisatie van Power BI. Integratie van Power BI bevat momenteel:

+ **Direct Connect**: een meer geavanceerde verbinding met logische naar beneden duwen tegen SQL Data Warehouse.  Dit biedt snellere analyse op een grotere schaal.
+ **Open in Power BI**: de knop 'openen in Power BI-informatie wordt doorgegeven exemplaar Power BI, zodat voor een naadloze verbinding.

Zie [integratie met Power BI](./sql-data-warehouse-integrate-power-bi.md) of de [Power BI-documentatie](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) voor meer informatie.

##<a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory hebben gebruikers een beheerde platform voor het maken van complexe Extract laden pijpleidingen.  SQL Data Warehouse integratie met Azure Data Factory omvat het volgende:

+ **Opgeslagen Procedures**: goedkeuringen door het uitvoeren van opgeslagen procedures in SQL Data Warehouse.
+ **Kopiëren**: ADF verplaatsen van gegevens in SQL Data Warehouse gebruiken.  Deze bewerking kunt gebruiken van de ADF standaardgegevens verplaatsing mechanisme of PolyBase achter de. 

Zie [integratie met Azure Data Factory](./sql-data-warehouse-integrate-azure-data-factory.md) of de [Azure Data Factory-documentatie](https://azure.microsoft.com/documentation/services/data-factory/) voor meer informatie.

##<a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning is een volledig beheerde analytics-service waarmee gebruikers kunnen complexe modellen gebruik te maken van een grote verzameling anticiperende hulpmiddelen maken.  SQL Data Warehouse wordt ondersteund als een bron en de bestemming van deze modellen met de volgende functionaliteit:

+ **Gegevens lezen:** Modellen op schaal met T-SQL SQL Data Warehouse ten opzichte van het station.
+ **Gegevens schrijven:** Wijzigingen doorvoeren in een model naar SQL Data Warehouse.

Zie [integratie met Azure Machine Learning](./sql-data-warehouse-integrate-azure-machine-learning.md) of de [Azure Machine Learning-documentatie](https://azure.microsoft.com/services/machine-learning/) voor meer informatie.

##<a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics is een complexe, volledig beheerde infrastructuur voor het verwerken en verbruikt gebeurtenisgegevens die zijn gegenereerd op basis van Azure gebeurtenis Hub.  Integratie met het datawarehouse SQL kunt u gegevens effectief worden verwerkt en opgeslagen samen met relationele gegevens, waardoor de diepere, meer geavanceerde analyse streaming.  

+ **Uitvoer job:** Uitvoer van Stream Analytics taken rechtstreeks verzenden naar SQL Data Warehouse.

Zie [integratie met Azure Stream Analytics](./sql-data-warehouse-integrate-azure-stream-analytics.md) of de [Azure Stream Analytics documentatie](https://azure.microsoft.com/documentation/services/stream-analytics/) voor meer informatie.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
