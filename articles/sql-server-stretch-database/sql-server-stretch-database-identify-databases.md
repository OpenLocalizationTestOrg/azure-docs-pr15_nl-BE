<properties
    pageTitle="Databases en tabellen voor uitrekken Database identificeren door uitrekken Database Advisor | Microsoft Azure"
    description="Informatie over het identificeren van databases en tabellen die kandidaten voor de Database uitrekken zijn."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="identify-databases-and-tables-for-stretch-database-by-running-stretch-database-advisor"></a>Databases en tabellen voor uitrekken Database identificeren door het uitrekken Database Advisor uitvoeren

Voor databases en tabellen die kandidaten voor uitrekken Database zijn SQL Server 2016 Upgrade Advisor downloaden en advies uitrekken Database uitvoeren. Uitrekken Database Advisor geeft ook de problematische kwesties.

## <a name="download-and-install-upgrade-advisor"></a>Downloaden en installeren van de Upgrade Advisor
Download en installeer de Upgrade Advisor uit [hier](http://go.microsoft.com/fwlink/?LinkID=613421). Dit hulpprogramma is niet opgenomen op de installatiemedia van SQL Server.

## <a name="run-the-stretch-database-advisor"></a>Uitrekken Database Advisor uitvoeren

1.  Upgrade Advisor uitvoeren.

2.  **Scenario's**te selecteren en selecteer **UITREKKEN DATABASE ADVISOR uitvoeren**.

3.  Klik op de bladeserver **Uitrekken Database Advisor uitvoeren** , **Selecteer DATABASES te analyseren**.

4.  Invoeren op de bladeserver **databases selecteren** of selecteert u de naam van de server en de gegevens van de verificatie. Klik op **verbinden**.

5.  Er verschijnt een lijst met databases op de geselecteerde server. Selecteer de databases die u wilt analyseren. Klik op **selecteren**.

6.  Klik op het blad **Uitrekken Database Advisor uitvoeren** op **uitvoeren**.  De analyse wordt uitgevoerd.

## <a name="review-the-results"></a>Bekijk de resultaten

1.  Wanneer de analyse is voltooid, klikt u op het blad **Analyzed databases** , selecteer een van de databases die u wilt weergeven van de **resultaten van de analyse** -blade geanalyseerd.

    De **resultaten van de analyse** -blade worden aanbevolen tabellen in de geselecteerde database die voldoen aan de criteria van de aanbeveling standaard.

2.  Selecteer in de lijst met de tabellen op de **resultaten van de analyse** -blade, een van de tabellen aanbevolen om de bladeserver **tabel resultaten** weer te geven.

    Als er problemen zijn geblokkeerd, worden de **resultaten van de tabel** blade de belemmeringen voor de geselecteerde tabel. Zie voor meer informatie over belemmeringen gevonden door uitrekken Database Advisor [beperkingen voor uitrekken Database](sql-server-stretch-database-limitations.md).

3.  Selecteer een van de problemen om meer informatie over het geselecteerde probleem weer te geven in de lijst van belemmeringen voor de **tabel resultaten** blade en risicobeperking stappen worden voorgesteld. De stappen van de voorgestelde oplossing implementeren als u wilt dat de geselecteerde tabel voor uitrekken Database configureren.

## <a name="next-step"></a>Volgende stap
Uitrekken Database inschakelen.

-   Zie [Uitrekken Database inschakelen voor een database](sql-server-stretch-database-enable-database.md)Database uitrekken van een **database**inschakelen.

-   Zie [Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md)uitrekken Database op een andere **tabel**inschakelen bij het uitrekken is reeds ingeschakeld op de database.

## <a name="see-also"></a>Zie ook

[Beperkingen voor uitrekken Database](sql-server-stretch-database-limitations.md)

[Uitrekken Database voor een database inschakelen](sql-server-stretch-database-enable-database.md)

[Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md)

[Alle onderwerpen voor service Azure uitrekken SQL Server-Database](sql-server-stretch-database-index-all-articles.md)
