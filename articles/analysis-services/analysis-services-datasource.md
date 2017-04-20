<properties
   pageTitle="DataSource verbindingen | Microsoft Azure"
   description="Beschrijving van gegevensbronverbindingen voor gegevensmodellen in Azure Analysis Services."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="datasource-connections"></a>DataSource-verbindingen

Gegevensmodellen in Azure Analysis Services mogelijk verschillende gegevensproviders wanneer verbinding wordt gemaakt met bepaalde gegevensbronnen. In sommige gevallen kunnen in tabelvorm modellen verbinding maken met gegevensbronnen native providers zoals SQL Server Native Client (SQLNCLI11) met een fout geretourneerd.

Bijvoorbeeld; Als u een geheugen of een directe Query gegevens modelleren die verbinding met een gegevensbron wolk zoals Azure SQL-Database, maakt als u eigen providers dan SQLOLEDB kan foutbericht wordt weergegeven: **'De provider 'SQLNCLI11.1' is niet geregistreerd'**.

Of als u een verbinding maken met gegevensbronnen voor gebouwen, DirectQuery-model als u eigen providers hebt u foutbericht weergegeven: "Fout bij het maken van OLE DB-rij-ingestelde **. Onjuiste syntaxis bij 'LIMIET' "**.

## <a name="data-source-providers"></a>Leveranciers van gegevensbronnen

De volgende gegevensbron-voorzieningen worden ondersteund voor in het geheugen of gegevensmodellen Query Direct verbinding met op gebouwen of cloud gegevensbronnen:

|               | **Gegevensbron**                     | **In het geheugen**                            |  **Directe Query**                                           |
|---------------------------|-------------------------------|---------------------------------------------|---------------------------------------------|
| **Wolk**                     | Azure SQL datawarehouse      | .NET framework Data Provider voor SQL Server | .NET framework Data Provider voor SQL Server |
|                           | Azure SQL-Database            | .NET framework Data Provider voor SQL Server | .NET framework Data Provider voor SQL Server |
| **On-premises** (via de Gateway) | SQL Server                    | SQL Server Native Client 11.0               | .NET framework Data Provider voor SQL Server |
|                           |  SQL Server                             | Microsoft OLE DB Provider for SQL Server    |   .NET framework Data Provider voor SQL Server                                          |
|                           |  SQL Server                             | .NET framework Data Provider voor SQL Server |  .NET framework Data Provider voor SQL Server                                           |
|                           | Oracle                        | Microsoft OLE DB-Provider voor Oracle        | Oracle Data Provider voor .NET               |
|                           |  Oracle                             | Oracle Data Provider voor .NET               | Oracle Data Provider voor .NET                                            |
|                           | Teradata                      | OLE DB-voorziening voor Teradata                | Teradata Data Provider voor .NET             |
|                           |  Teradata                             | Teradata Data Provider voor .NET             |  Teradata Data Provider voor .NET                                            |
|                           | Analytics Platform systeem | .NET framework Data Provider voor SQL Server | .NET framework Data Provider voor SQL Server |


> [AZURE.NOTE] Controleer de 64-bits computer zijn bij het gebruik van On-Premises Gateway.

Wanneer een tabelvorm op gebouwen SQL Server Analysis Services-model met Azure Analysis Services migreren, is het mogelijk noodzakelijk de provider wilt wijzigen.

**Een provider gegevensbron opgeven**

1. In SSDT > **In tabelvorm Modelverkenner** > **Data Sources**, met de rechtermuisknop op de verbinding met een gegevensbron en klik vervolgens op **Gegevensbestand bewerken**.

2. In **Verbinding bewerken**, klikt u op **Geavanceerd** om het venster Geavanceerde eigenschappen openen.

3. **Geavanceerde eigenschappen instellen**van > **Providers**en selecteer vervolgens de juiste provider.

## <a name="impersonation"></a>Imitatie
In sommige gevallen mogelijk een andere imitatie account opgeven. Imitatie-account kan worden opgegeven in SSDT of SSMS.

Voor gegevensbronnen voor bedrijven:

- Als SQL-verificatie gebruikt, moeten de imitatie-serviceaccount.
- Als Windows-verificatie, stelt Windows gebruikerswachtwoord. Voor SQL Server, wordt Windows-verificatie met een specifieke imitatie-account alleen ondersteund voor modellen van de gegevens in het geheugen.

Voor cloud-gegevensbronnen:

- Als SQL-verificatie gebruikt, moeten de imitatie-serviceaccount.


## <a name="next-steps"></a>Volgende stappen

Als u gegevensbronnen op gebouwen, moet u de [On-premises gateway](analysis-services-gateway.md)installeren. Zie voor meer informatie over het beheren van uw server in SSDT of SSMS, [uw server beheren](analysis-services-manage.md).
