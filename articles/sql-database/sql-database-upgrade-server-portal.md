<properties
    pageTitle="Upgrade naar Azure SQL Database V12 met de Azure portal | Microsoft Azure"
    description="Wordt uitgelegd hoe u een upgrade uitvoeren naar Azure SQL Database V12 met inbegrip van Web- en databases bijwerken en het upgraden van een V11: server migreren van de databases rechtstreeks in een elastische database-toepassingen met behulp van de portal Azure."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/08/2016"
    ms.author="sstein"/>


# <a name="upgrade-to-azure-sql-database-v12-using-the-azure-portal"></a>Upgrade naar Azure SQL Database V12 met de Azure portal


> [AZURE.SELECTOR]
- [Azure portal](sql-database-upgrade-server-portal.md)
- [PowerShell](sql-database-upgrade-server-powershell.md)


SQL Database V12 is de nieuwste versie zodat de bestaande servers upgraden naar SQL Database V12 wordt aanbevolen.
SQL Database V12 heeft veel [voordelen ten opzichte van de vorige versie](sql-database-v12-whats-new.md) inclusief:

- Verbeterde compatibiliteit met SQL Server.
- Premie voor verbeterde prestaties en nieuwe prestatieniveaus.
- [Elastische database van toepassingen](sql-database-elastic-pool.md).

Dit artikel bevat instructies voor het upgraden van bestaande V11: SQL-Database-servers en databases voor SQL-Database V12.

Tijdens het uitvoeren van een upgrade naar V12 wordt u Web- en -databases upgraden naar een nieuwe service tier zodat instructies voor het upgraden van Web- en databases opgenomen zijn.

Bovendien kan migreren naar een [groep met elastische database](sql-database-elastic-pool.md) voordeliger zijn dan een upgrade naar individuele prestaties (prijzen lagen) voor enkele databases. Database management vereenvoudigen toepassingen ook omdat u alleen nodig voor het beheer van de prestatie-instellingen voor de groep in plaats van de prestaties van afzonderlijke databases afzonderlijk beheren. Als u databases op meerdere servers kunt u ze verplaatsen naar dezelfde server en profiteren van ze in een groep te plaatsen. U kunt eenvoudig [databases van servers rechtstreeks in elastische database toepassingen met PowerShell V11: automatische migreren](sql-database-upgrade-server-powershell.md). Ook kunt u de portal V11: databases migreren naar een groep, maar in de portal hebt u al een V12-server voor het maken van een groep. Routebeschrijving vindt u verderop in dit artikel voor het maken van de toepassingen na de serverupgrade als u [databases die van een groep profiteren kunnen](sql-database-elastic-pool-guidance.md).

Houd er rekening mee dat uw databases online blijft en blijven werken tijdens de upgradebewerking. Op het moment van de werkelijke overgang naar het nieuwe tijdelijke verbindingen met de database weghalen prestatieniveau kunnen voor een zeer kleine duur die meestal ongeveer 90 seconden kan gebeuren maar maar liefst 5 minuten. Als uw toepassing [tijdelijke fout afhandeling voor afsluitingen van de verbinding worden](sql-database-connectivity-issues.md) is het voldoende bescherming tegen verbroken verbindingen aan het einde van de upgrade.

Een upgrade naar SQL Database V12 kan niet ongedaan worden gemaakt. Na een upgrade van kan niet de server worden teruggedraaid door V11:.

Na de upgrade naar V12, [service tier aanbevelingen](sql-database-service-tier-advisor.md) en [Overwegingen bij prestaties van toepassingen elastische](sql-database-elastic-pool-guidance.md) onmiddellijk worden niet beschikbaar totdat de service heeft tijd om te evalueren uw werkbelasting op de nieuwe server. V11: server aanbeveling geschiedenis niet van toepassing op V12 servers dus niet behouden blijft.

## <a name="prepare-to-upgrade"></a>Bereid de upgrade

- **Alle Web- en databases bijwerken**: Zie [alle Web- en databases bijwerken](sql-database-upgrade-server-portal.md#upgrade-all-web-and-business-databases) sectie hieronder of Zie [Monitor en beheren van een elastische database-toepassingen (PowerShell)](sql-database-elastic-pool-manage-powershell.md).
- **Controleren en schorsen Geo-replicatie**: als de Azure SQL-database is geconfigureerd voor Geo-replicatie moet u de huidige configuratie en [Geo-replicatie stoppen](sql-database-geo-replication-portal.md#remove-secondary-database)documenteren. Configuratie van de database voor Geo-replicatie nadat de upgrade is voltooid.
- **Openen van deze poorten als u clients op een Azure VM**: als het clientprogramma verbinding maakt met SQL Database V12 terwijl de client wordt uitgevoerd op een Azure VM (virtual machine), moet u poortbereiken 11000 11999 en 14000-14999 op de VM openen. Zie [poorten voor V12 voor SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md)voor meer informatie.



## <a name="start-the-upgrade"></a>De upgrade starten

1. In de [portal Azure](https://portal.azure.com/) bladeren naar de server die u wilt bijwerken door te selecteren op **Bladeren** > **SQL-servers**en de v2.0-server die u wilt bijwerken te selecteren.
2. Selecteer de **Meest recente Update voor SQL-Database**en selecteer vervolgens **een Upgrade deze server**.

      ![server bijwerken][1]

3. Upgraden van een server naar de meest recente Update voor SQL-Database is permanent en onomkeerbaar. Typ de naam van de server en klik op **OK**om de upgrade te bevestigen.

## <a name="upgrade-all-web-and-business-databases"></a>Alle Web- en databases bijwerken

Als uw server Web- of -databases moet u deze opwaarderen. Tijdens het uitvoeren van een upgrade naar SQL Database V12 moet u alle Web- en databases bijwerken naar een nieuwe servicelaag.    

Om u te helpen met upgraden, adviseert de service SQL-Database een passende niveaus en prestaties serviceniveau (prijzen laag) voor elke database. De service wordt aanbevolen de beste laag voor het uitvoeren van uw bestaande database werkbelasting door analyse van de historische voor uw database.

3. Selecteer elke database bekijken en selecteren om te upgraden naar de aanbevolen prijs trapsgewijs in het blad van **deze server bijwerken** . U kunt altijd de beschikbare prijzen lagen bladeren en selecteren die het beste aansluit op uw omgeving.


     ![databases][2]


7. Na het klikken op de voorgestelde laag u krijgt het blad **kiezen de prijzen laag** kunt u een bepaalde laag selecteren en klik vervolgens op de knop **selecteren** om te wijzigen in die laag. Selecteer een nieuwe laag voor elke Web- of database.

    Wat is het belangrijk te weten is dat de SQL-database niet is vergrendeld in een bepaalde laag of prestaties serviceniveau, dus als de vereisten van uw database wijzigen u gemakkelijk tussen de lagen van de beschikbare service en de prestaties wijzigen kunt. In feite, Basic, Standard en Premium SQL-Databases worden gefactureerd per uur en u hebt de mogelijkheid om de schaal van elke database omhoog of omlaag 4 keer binnen een periode van 24 uur.

    ![aanbevelingen][6]


Nadat u alle databases op de server in aanmerking komen bent u klaar om de upgrade

## <a name="confirm-the-upgrade"></a>Bevestig de upgrade

3. Wanneer alle databases op de server in aanmerking voor een upgrade komen moet u **TYPE de naam van de SERVER** om te controleren of u wilt uitvoeren van de upgrade en klik op **OK**.

    ![Controleer of de upgrade][3]


4. De upgrade wordt gestart en de voortgang in melding. Het upgradeproces is gestart. Afhankelijk van de details van uw specifieke databases kan upgraden naar V12 enige tijd duren. Gedurende deze tijd alle databases op de server on line blijven maar server en database management acties worden beperkt.

    ![upgrade wordt uitgevoerd][4]

    Niveau tijdelijk neer te zetten van de verbindingen met de database kan op het tijdstip van de feitelijke overgang naar de nieuwe prestaties gebeuren gedurende een zeer kleine (meestal gemeten in seconden). Als een toepassing heeft een fout met betrekking tot tijdelijke behandeling (Pogingslogica) voor afsluitingen van de verbinding worden is het voldoende bescherming tegen verbroken verbindingen aan het einde van de upgrade.

5. De **Meest recente Update** blade weergegeven **ingeschakeld**nadat u de upgrade uitvoert, is voltooid.

    ![V12 ingeschakeld][5]  

## <a name="move-your-databases-into-an-elastic-database-pool"></a>Uw database verplaatsen naar een elastische database toepassingen

Blader naar de server V12 in [Azure portal](https://portal.azure.com/) en klik op **groep toevoegen**.

- of -

Als er een bericht **Klik hier voor de aanbevolen elastische database van toepassingen voor deze server**, klikt u op een groep die is geoptimaliseerd voor de databases van de server gemakkelijk te maken. Zie [Overwegingen voor prijs- prestatieverhouding voor een elastische database-toepassingen](sql-database-elastic-pool-guidance.md)voor meer informatie.

![Groep van toepassingen toevoegen aan een server][7]

Volg de aanwijzingen in het artikel [een elastische database van toepassingen maken](sql-database-elastic-pool.md) hebt gemaakt van uw groep.

## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Databases controleren na een upgrade naar SQL Database V12

>[AZURE.IMPORTANT] Een upgrade uitvoeren naar de nieuwste versie van SQL Server Management Studio (SSMS) om te profiteren van de nieuwe v12-mogelijkheden. [SQL Server Management Studio downloaden] (https://msdn.microsoft.com/library/mt238290.aspx).

Na de upgrade, kan de database zodat toepassingen worden uitgevoerd op de gewenste prestaties controleren en vervolgens waar nodig de instellingen te optimaliseren.

Naast het controleren van de afzonderlijke databases kunt u de [Monitor, beheren, en de grootte van een elastische database toepassingen met Azure portal](sql-database-elastic-pool-manage-portal.md) van elastische database toepassingen controleren of met [PowerShell](sql-database-elastic-pool-manage-powershell.md).


**Verbruiksgegevens bron:** Resourcegegevens verbruik is voor Basic-, Standard- en Premium-databases beschikbaar via de [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV in de database. Deze DMV vindt u in de buurt van real-time verbruik resourcegegevens op 15 tweede granulatie voor de vorige bewerking uur. De DTU percentage van het verbruik voor een interval wordt berekend als het verbruik van het maximumpercentage van de CPU, IO en logboek dimensies. Hier wordt een query voor het berekenen van het gemiddelde DTU percentage verbruik over het afgelopen uur:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Als u meer informatie over prestatiebewaking:

- [Azure SQL Database prestaties richtlijnen voor enkele databases](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Overwegingen voor prijs- prestatieverhouding voor een elastische database-toepassingen](sql-database-elastic-pool-guidance.md).
- [Azure SQL-Database met behulp van beheer van dynamische weergaven controleren](sql-database-monitoring-with-dmvs.md)




**Waarschuwingen:** "Waarschuwingen instellen ' in de Azure portal om u te waarschuwen wanneer de DTU het verbruik voor een upgrade van een database bepaald hoog niveau nadert. Waarschuwingen van de database kunnen worden ingesteld in Azure portal voor verschillende prestatiegegevens zoals DTU, CPU, IO en logboek. Blader naar de database en selecteer **waarschuwingsregels** in het blad van de **Instellingen** .

Bijvoorbeeld: u kunt instellen een waarschuwingsbericht per e-mail op "DTU Percentage" Als de gemiddelde waarde van DTU percentage groter is dan 75% gedurende de laatste 5 minuten. Raadpleeg voor het [ontvangen van meldingen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) voor meer informatie over het configureren van meldingen.





## <a name="next-steps"></a>Volgende stappen

- [Controleren op aanbevelingen van toepassingen en een groep maken](sql-database-elastic-pool-create-portal.md).
- [Het serviceniveau voor laag en de prestaties van uw database te wijzigen](sql-database-scale-up.md).



## <a name="related-links"></a>Verwante koppelingen

- [Nieuwe functies in een SQL-Database V12](sql-database-v12-whats-new.md)
- [Plannen en voorbereiden voor een upgrade naar SQL Database V12](sql-database-v12-plan-prepare-upgrade.md)


<!--Image references-->
[1]: ./media/sql-database-upgrade-server-portal/latest-sql-database-update.png
[2]: ./media/sql-database-upgrade-server-portal/upgrade-server2.png
[3]: ./media/sql-database-upgrade-server-portal/upgrade-server3.png
[4]: ./media/sql-database-upgrade-server-portal/online-during-upgrade.png
[5]: ./media/sql-database-upgrade-server-portal/enabled.png
[6]: ./media/sql-database-upgrade-server-portal/recommendations.png
[7]: ./media/sql-database-upgrade-server-portal/new-elastic-pool.png
