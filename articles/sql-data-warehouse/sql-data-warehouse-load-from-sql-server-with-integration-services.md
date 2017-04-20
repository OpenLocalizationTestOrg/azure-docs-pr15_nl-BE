<properties
   pageTitle="Laden van gegevens uit SQL Server in Azure SQL gegevens magazijn (SSIS) | Microsoft Azure"
   description="Laat zien hoe u een pakket van SQL Server Integration Services (SSIS) gegevens uit uiteenlopende gegevensbronnen naar SQL Data Warehouse maken."
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
   ms.date="08/08/2016"
   ms.author="lodipalm;sonyama;barbkess"/>

# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Laden van gegevens uit SQL Server in Azure SQL gegevens magazijn (SSIS)

> [AZURE.SELECTOR]
- [SSI 'S](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)


Maak een pakket van SQL Server Integration Services (SSIS) voor het laden van gegevens uit SQL Server in Azure SQL Data Warehouse. U kunt desgewenst herstructureren, transformeren en de gegevens opruimen als dit wordt doorgegeven via de gegevensstroom SSIS.

In deze zelfstudie wordt u:

- Maak een nieuwe Integration Services-project in Visual Studio.
- Verbinding maken met gegevensbronnen, waaronder SQL Server (als bron) en SQL Data Warehouse (als bestemming).
- Ontwerp een SSIS-pakket dat gegevens worden geladen vanuit de bron naar de bestemming.
- Voer de SSIS-pakket om de gegevens te laden.

In deze zelfstudie wordt SQL Server als gegevensbron gebruikt. SQL Server kan worden uitgevoerd in ruimten of in een Azure virtuele machine.

## <a name="basic-concepts"></a>Basisbegrippen

Het pakket is de eenheid van werk in SSIS. Gerelateerde pakketten worden in projecten gegroepeerd. U maken projecten en pakketten ontwerp in Visual Studio met SQL Server Data Tools. Het ontwerpproces is een visuele proces waarin u slepen en neerzetten onderdelen uit de werkset naar het ontwerpvlak, verbinden, en de eigenschappen ervan instellen. Nadat u het pakket, kunt u eventueel gebruiken deze met SQL Server op uitgebreid beheer, controle en beveiliging.

## <a name="options-for-loading-data-with-ssis"></a>Opties voor het laden van gegevens met SSI 's

SQL Server Integration Services (SSIS) is een flexibele set hulpprogramma's waarmee u allerlei opties voor verbinding met en het laden van gegevens in SQL Data Warehouse.

1. Met een bestemming ADO NET verbinding maken met SQL Data Warehouse. In deze zelfstudie wordt een ADO NET bestemming omdat er zo weinig mogelijk de configuratie-opties.
2. Met een OLE DB-bestemming verbinding maken met SQL Data Warehouse. Deze optie kan bieden iets betere prestaties dan de bestemming ADO NET.
3. Gebruik de taak Azure Blob uploaden om de gegevens in Azure Blob-opslag fase. Gebruik vervolgens de taak SQL uitvoeren van SSIS starten een Polybase script dat de gegevens in SQL Data Warehouse worden geladen. Deze optie biedt de beste prestaties van de drie opties die hier worden vermeld. Als u de taak Azure Blob uploaden, download het [Microsoft SQL Server 2016 Integration Services Feature Pack voor Azure][]. Zie voor meer informatie over Polybase, [PolyBase gids][].

## <a name="before-you-start"></a>Voordat u begint

Stap in deze zelfstudie, wilt u het volgende nodig:

1. **SQL Server integratieservices (SSIS)**. SSI's is een onderdeel van SQL Server en een evaluatie-exemplaar of een gelicentieerde versie van SQL Server vereist. Als u een evaluatieversie van SQL Server 2016 Preview, Zie [Evaluaties van SQL Server][].
2. **Visual Studio**. Zie als u de gratis 2015 Community Edition van Visual Studio, [Visual Studio-Gemeenschap][].
3. **SQL Server Data Tools voor Visual Studio (SSDT)**. Als u SQL Server Data Tools voor Visual Studio 2015, Zie [Download SQL Server Data Tools (SSDT)][].
4. **Voorbeeldgegevens**. In deze zelfstudie wordt gebruikgemaakt van voorbeeldgegevens in de AdventureWorks-voorbeelddatabase in SQL Server opgeslagen als de bron van gegevens in SQL Data Warehouse worden geladen. Zie [Voorbeelddatabases AdventureWorks-2014][]de AdventureWorks-voorbeelddatabase.
5. **A SQL Data Warehouse-database en -machtigingen**. Deze zelfstudie verbinding maakt met een exemplaar van SQL Data Warehouse en gegevens in deze geladen. U moet over machtigingen beschikt om een tabel te maken en om gegevens te laden.
6. **Een firewallregel**. Hebt u een firewallregel maken op SQL Data Warehouse met het IP-adres van de lokale computer, voordat u gegevens naar de SQL-datawarehouse uploaden kunt.

## <a name="step-1-create-a-new-integration-services-project"></a>Stap 1: Maak een nieuw project voor de integratie van diensten

1. Start Visual Studio 2015.
2. Selecteer in het menu **bestand** **Nieuw | Project**.
3. Ga naar de **geïnstalleerd | Sjablonen | Business Intelligence | Integratieservices** projecttypen.
4. **Integratie Services Project**selecteren. Waarden opgeven voor de **naam** en **locatie op**en klik vervolgens op **OK**.

Visual Studio wordt geopend en wordt een nieuw project voor Integration Services (SSIS) gemaakt. Visual Studio wordt geopend de ontwerper voor de enkele nieuwe SSIS-pakket (Package.dtsx) in het project. Ziet u het volgende schermgebieden:

- Aan de linkerkant van de **werkset** van SSIS onderdelen.
- In het midden het ontwerpvlak met meerdere tabbladen. U gebruikt gewoonlijk ten minste de **Controlestroom** en de **Gegevensstroom** tabbladen.
- Aan de rechterkant, de **Solution Explorer** en het deelvenster **Eigenschappen** .

    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a>Stap 2: Maak de basisgegevens stroom

1. Sleep een taak voor het stromen van gegevens vanuit de werkset naar het midden van het ontwerpoppervlak (op het tabblad **Controlestroom** ).

    ![][02]

2. Dubbelklik op de Data Flow taak om over te schakelen naar het tabblad gegevensstroom.
3. Sleep een ADO.NET-bron naar het ontwerpvlak van de lijst met andere bronnen in de werkset. Met de bron-adapter is nog steeds is geselecteerd, wijzigen de naam **SQL Server-bron** in het deelvenster **Eigenschappen** .
4. Sleep een ADO.NET-bestemming naar het ontwerpvlak onder de ADO.NET-bron in de lijst andere bestemmingen in de werkset. Met de bestemming gebruikt nog steeds is geselecteerd, wijzig de naam **SQL DW bestemming** in het deelvenster **Eigenschappen** .

    ![][09]

## <a name="step-3-configure-the-source-adapter"></a>Stap 3: De bron-adapter configureren

1. Dubbelklik op de bron-adapter om de **ADO.NET-broncode-Editor**te openen.

    ![][03]

2. Klik op de knop **Nieuw** naast de lijst met **ADO.NET Verbindingsbeheer** openen in het dialoogvenster **Verbindingsbeheer ADO.NET configureren** en instellingen voor de SQL Server-database van waaruit deze zelfstudie gegevens laadt maken op het tabblad **Connection Manager** van **ADO.NET broncode-Editor**.

    ![][04]

3. Klik op de knop **Nieuw** om het dialoogvenster **Verbindingsbeheer** openen en een nieuwe gegevensverbinding maken in het dialoogvenster **Verbindingsbeheer ADO.NET configureren** .

    ![][05]

4. Klik in het dialoogvenster **Verbindingsbeheer** het volgende doen.

    1. Selecteer de SqlClient-gegevensvoorziening voor **Provider**.
    2. Voor de **naam van de Server**, voer de naam van de SQL Server.
    3. Selecteer in de sectie **aanmelden bij de server** of verificatie-informatie invoeren.
    4. Selecteer in de sectie **verbinding maken met een database** , de AdventureWorks-voorbeelddatabase.
    5. Klik op **verbinding testen**.
    
        ![][06]
    
    6. In het dialoogvenster waarin de resultaten van de verbindingstest, klikt u op **OK** om terug te keren naar het dialoogvenster **Verbindingsbeheer** .
    7. Klik op **OK** om terug te keren naar het dialoogvenster **Verbindingsbeheer ADO.NET configureren** in het dialoogvenster **Verbindingsbeheer** .
 
5. Klik op **OK** om terug te keren naar de **ADO.NET-broncode-Editor**in het dialoogvenster **Verbindingsbeheer ADO.NET configureren** .
6. Selecteer de tabel **Sales.SalesOrderDetail** in de **ADO.NET-broncode-Editor**in de lijst **naam van de tabel of de weergave** .

    ![][07]

7. Klik op **voorbeeld** als u wilt zien van de eerste 200 rijen met gegevens in de tabel in het dialoogvenster **Query-resultaten bekijken** .

    ![][08]

8. Klik op **sluiten** om terug te keren naar de **ADO.NET-broncode-Editor**in het dialoogvenster **Query-resultaten bekijken** .
9. Klik op **OK** als u klaar bent met het configureren van de gegevensbron in de **ADO.NET-broncode-Editor**.

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a>Stap 4: Sluit het bron-adapter aan op de adapter van de bestemming

1. Selecteer de bron-adapter in de ontwerpweergave.
2. De blauwe pijl die zich vanaf de bron adapter uitstrekt selecteren en sleep deze naar de bestemming editor totdat deze vastklikt.

    ![][10]

    In een typische SSIS-pakket kunt u een aantal andere onderdelen uit de werkset SSIS tussen de bron en de bestemming structureren, transformeren en gegevens verwijderen als deze de gegevensstroom SSIS passeert. Om dit voorbeeld zo eenvoudig mogelijk te houden, verbinding we de bron naar de bestemming.

## <a name="step-5-configure-the-destination-adapter"></a>Stap 5: Configureren van de adapter van de bestemming

1. Dubbelklik op de adapter van de bestemming om de **ADO.NET bestemming Editor**te openen.

    ![][11]

2. Klik op de knop **Nieuw** naast de lijst met **Verbindingsbeheer** verbindingsinstellingen voor de Azure SQL Data Warehouse-database waarin gegevens voor deze zelfstudie worden geladen en open het dialoogvenster **Verbindingsbeheer ADO.NET configureren** op het tabblad **Connection Manager** van **ADO.NET bestemming Editor**.
3. Klik op de knop **Nieuw** om het dialoogvenster **Verbindingsbeheer** openen en een nieuwe gegevensverbinding maken in het dialoogvenster **Verbindingsbeheer ADO.NET configureren** .
4. Klik in het dialoogvenster **Verbindingsbeheer** het volgende doen.
    1. Selecteer de SqlClient-gegevensvoorziening voor **Provider**.
    2. Voer de naam van de SQL Data Warehouse voor de **naam van de Server**.
    3. **Gebruik SQL Server** -verificatie en verificatie-informatie invoeren in de sectie **aanmelden bij de server** .
    4. Selecteer een bestaande SQL Data Warehouse-database in de sectie **verbinding maken met een database** .
    5. Klik op **verbinding testen**.
    6. In het dialoogvenster waarin de resultaten van de verbindingstest, klikt u op **OK** om terug te keren naar het dialoogvenster **Verbindingsbeheer** .
    7. Klik op **OK** om terug te keren naar het dialoogvenster **Verbindingsbeheer ADO.NET configureren** in het dialoogvenster **Verbindingsbeheer** .
5. Klik op **OK** om terug te keren naar de **Editor van ADO.NET bestemming**in het dialoogvenster **Verbindingsbeheer ADO.NET configureren** .
6. Klik op **Nieuw** in de **Editor van ADO.NET bestemming**naast de lijst met **gebruik van een tabel of weergave** te openen in het dialoogvenster **Tabel maken** een nieuwe bestemming als tabel wilt maken met een lijst met kolommen die overeenkomen met de brontabel.

    ![][12a]

7. Klik in het dialoogvenster **Tabel maken** de volgende dingen te doen.

    1. Wijzig de naam van de doeltabel in **verkooporderdetail**.
    2. Verwijder de **rowguid** -kolom. Het gegevenstype **uniqueidentifier** wordt niet ondersteund in SQL Data Warehouse.
    3. Wijzig het gegevenstype van de kolom **LineTotal** in **geld**. Het gegevenstype **decimaal** wordt niet ondersteund in SQL Data Warehouse. Zie voor informatie over ondersteunde gegevenstypen, [CREATE TABLE (Azure SQL Data Warehouse, Parallel datawarehouse)][].
    
        ![][12b]
    
    4. Klik op **OK** om de tabel te maken en terug te keren naar de **Editor van ADO.NET-bestemming**.

8. Selecteer het tabblad **toewijzingen** om te zien hoe de kolommen in de brontabel zijn toegewezen aan kolommen in de bestemming in de **Editor van ADO.NET bestemming**.

    ![][13]

9. Klik op **OK** als u klaar bent met het configureren van de gegevensbron.

## <a name="step-6-run-the-package-to-load-the-data"></a>Stap 6: Voer het pakket om de gegevens te laden uit.

Het pakket uitvoeren door te klikken op de knop **Start** op de werkbalk of door een van de opties **uitvoeren** in het menu **Foutopsporing** te selecteren.

Als het pakket gestart wordt, ziet u de gele draaiende wielen om aan te geven voor activiteiten zoals het aantal rijen dat tot nu toe is verwerkt.

![][14]

Wanneer het pakket is voltooid, ziet u groene vinkjes om succes zoals het totale aantal rijen met gegevens die worden geladen vanuit de bron naar de bestemming te geven.

![][15]

Gefeliciteerd! U hebt SQL Server Integration Services met succes gebruikt om gegevens te laden in Azure SQL Data Warehouse.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de gegevensstroom SSIS. Begin hier: [Gegevensstroom][].
- Informatie over het opsporen en oplossen van problemen met uw pakketten rechtstreeks in de ontwerpomgeving. Begin hier: [Problemen met hulpprogramma's voor de ontwikkeling van het pakket][].
- Informatie over het implementeren van SSI's projecten en pakketten Integration Services-server of naar een andere opslaglocatie. Begin hier: [implementatie van projecten en pakketten][].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[Handleiding voor PolyBase]: https://msdn.microsoft.com/library/mt143171.aspx
[Download SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[TABEL (Azure SQL datawarehouse, Parallel datawarehouse) maken]: https://msdn.microsoft.com/library/mt203953.aspx
[Gegevensoverdracht]: https://msdn.microsoft.com/library/ms140080.aspx
[Probleemoplossing, hulpprogramma's voor de ontwikkeling van pakket]: https://msdn.microsoft.com/library/ms137625.aspx
[Implementatie van projecten en pakketten]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 integratieservices functiepakket voor Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[Beoordelingen voor SQL Server]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio Community]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[Voorbeelddatabases AdventureWorks-2014]: https://msftdbprodsamples.codeplex.com/releases/view/125550
