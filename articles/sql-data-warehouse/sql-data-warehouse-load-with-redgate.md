<properties
   pageTitle="Redgate van Data Platform Studio gebruiken om gegevens te laden in SQL Data Warehouse | Microsoft Azure"
   description="Informatie over het gebruik van Redgate Data Platform Studio voor magazijnbeheer scenario's voor gegevens."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>


# <a name="load-data-with-redgate-data-platform-studio"></a>Gegevens laden met Redgate Data Platform Studio

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)

In deze zelfstudie wordt beschreven hoe u [De Redgate gegevens Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) gebruiken om gegevens te verplaatsen van een lokale SQL Server naar Azure SQL Data Warehouse. Data Platform Studio van toepassing de meest geschikte compatibiliteitscorrecties en optimalisatie, dus het is de snelste manier om aan de slag met SQL Data Warehouse.

> [AZURE.NOTE] [Redgate](http://www.red-gate.com) is een ervaren partner van Microsoft die verschillende hulpprogramma's voor SQL Server biedt. Deze functie in de Studio van Data Platform is beschikbaar gesteld voor commerciële en niet-commercieel gebruik vrij.

## <a name="before-you-begin"></a>Voordat u begint
### <a name="create-or-identify-resources"></a>Maak of Identificeer bronnen

Voordat u deze zelfstudie begint, nodig hebt u:

- **Lokale SQL Server-Database**: de gegevens die u wilt importeren naar SQL Data Warehouse moet afkomstig zijn van een lokale SQL Server (versie 2008R2 of hoger). Data Platform Studio kan gegevens importeren rechtstreeks vanaf een Azure SQL-Database of uit tekstbestanden.
- **Azure opslag Account**: Data Platform Studio fasen de gegevens in Azure Blob-opslag vóór het laden in het SQL-datawarehouse. De rekening voor de opslag moet het implementatiemodel ' Resource Manager ' (de standaardinstelling) gebruiken in plaats van de 'Klassieke' implementatiemodel. Als u geen account opslag, informatie over het maken van een account voor opslag. 
- **SQL Data Warehouse**: in deze zelfstudie wordt verplaatst de gegevens van lokale SQL Server naar SQL Data Warehouse, dus moet u online een datawarehouse hebben. Als u nog geen een datawarehouse, informatie over het maken van een Azure SQL Data Warehouse.

> [AZURE.NOTE] De prestaties zijn verbeterd als de account voor de opslag en het datawarehouse worden gemaakt in dezelfde regio.

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Stap 1: Aanmelden bij Data Platform Studio met uw account Azure
Open uw webbrowser en Ga naar de website [Gegevens Platform Studio](https://www.dataplatformstudio.com/) . Log in met dezelfde Azure rekening die u gebruikt voor het maken van de opslagplaats voor account en gegevens. Als uw e-mailadres gekoppeld aan een werk of school-account en een Microsoft-account is, moet u de account die toegang tot uw resources heeft kiezen.

> [AZURE.NOTE] Als dit de eerste keer met behulp van Data Platform Studio, wordt u gevraagd de toepassing machtigen voor het beheren van uw resources Azure.

## <a name="step-2-start-the-import-wizard"></a>Stap 2: Start de Wizard importeren
Selecteer in het hoofdscherm van DPS Azure SQL Data Warehouse koppeling de Import wizard importeren te starten.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Stap 3: De gegevens Platform Studio-Gateway installeren
Als u wilt verbinding maken met uw lokale SQL Server-database, moet u de DPS Gateway installeren. De gateway is een client-agent die biedt toegang tot uw lokale omgeving, de gegevens extraheert en geüpload naar uw account voor de opslag. Uw gegevens worden nooit Redgate van servers passeert. De Gateway installeren:

1.  Klik op de koppeling **Gateway maken**
2. Download en installeer de Gateway met het meegeleverde installatieprogramma

![][2]

> [AZURE.NOTE] De Gateway kan worden geïnstalleerd op een computer met netwerktoegang tot de SQL Server-bronserverdatabase. Deze toegang heeft tot de SQL Server-database met behulp van Windows-verificatie met de referenties van de huidige gebruiker.

Eenmaal geïnstalleerd, verandert de status van de Gateway in verbonden en kunt u volgende.

## <a name="step-4-identify-the-source-database"></a>Stap 4: Bepaal de brondatabase
Voer de naam van de server die fungeert als host voor de database en selecteer **vervolgens**in het tekstvak *Servernaam invoeren* . Selecteer vervolgens de database die u wilt importeren van gegevens van uit de vervolgkeuzelijst.

![][3]

DPS inspecteert de geselecteerde database voor tabellen wilt importeren. DPS worden standaard alle tabellen in de database geïmporteerd. U kunt selecteren of deselecteren tabellen door de koppeling met alle tabellen uit te breiden. Selecteer volgende om door te gaan.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Stap 5: Kies een account opslag fase van de gegevens
DPS vraagt u naar een locatie voor de fase van de gegevens. Een bestaande account voor de opslag van uw abonnement kiezen en op **volgende**.

> [AZURE.NOTE] DPS maakt een nieuwe blob-container in de account van de gekozen opslag en gebruiken een aparte map voor elke import.

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Stap 6: Selecteer een datawarehouse
Vervolgens selecteert u een on line [Azure SQL Data Warehouse](http://aka.ms/sqldw) -database te importeren in. Wanneer u de database hebt geselecteerd, moet u de referenties om verbinding met de database en selecteer **volgende**in te voeren.

![][5]

> [AZURE.NOTE] DPS samengevoegd de brontabellen gegevens met het datawarehouse. DPS waarschuwt u als de naam van het overschrijven van bestaande tabellen in het datawarehouse vereist. U kunt desgewenst verwijderen alle bestaande objecten in het datawarehouse door internetwebsite verwijderen alle bestaande objecten vóór het importeren.

## <a name="step-7-import-the-data"></a>Stap 7: De gegevens importeren
DPS bevestigt dat u wilt importeren. Klik op de knop Start importeren om te beginnen met het importeren van gegevens.

![][6]

DPS geeft een visualisatie waarin de voortgang van het uitpakken en het uploaden van de gegevens uit de lokale SQL Server en de voortgang van de invoer in SQL Data Warehouse.

![][7]

Nadat het importeren voltooid is, wordt een samenvatting van het importeren van gegevens en een rapport van de correcties voor compatibiliteitsproblemen die zijn uitgevoerd DPS weergegeven.

![][8]

## <a name="next-steps"></a>Volgende stappen
Als u wilt uw gegevens in SQL Data Warehouse verkennen, eerst weergeven:

- [Query Azure SQL datawarehouse (Visual Studio)][]
- [Gegevens met Power BI][]

Voor meer informatie over de Redgate gegevens Platform Studio:

- [Ga naar de homepage van DPS](http://www.dataplatformstudio.com/)
- [Bekijk een demonstratie van DPS op Channel9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Zie voor een overzicht van andere manieren om te migreren en uw gegevens in SQL Data Warehouse worden geladen:

- [Uw oplossing migreren naar SQL Data Warehouse][]
- [Gegevens laden in Azure SQL Data Warehouse](./sql-data-warehouse-overview-load.md)

Zie [SQL Data Warehouse ontwikkelen-overzicht](./sql-data-warehouse-overview-develop.md)voor meer tips voor ontwikkeling.

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Query Azure SQL datawarehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Gegevens met Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Uw oplossing migreren naar SQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md