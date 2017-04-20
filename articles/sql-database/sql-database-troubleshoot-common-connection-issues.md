<properties
    pageTitle="Algemene problemen verbinding Azure SQL-database"
    description="Stappen voor het identificeren en oplossen van veelvoorkomende fouten voor Azure SQL-Database."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Problemen met verbinding met Azure SQL-Database

Wanneer de verbinding met Azure SQL Database mislukt, kunt u [foutberichten](sql-database-develop-error-messages.md)ontvangen. Dit artikel is een centrale onderwerp waarmee u problemen met Azure SQL-Database. Het introduceert [het gemeenschappelijk veroorzaakt](#cause) van verbindingsproblemen, raadt aan om [een hulpprogramma voor probleemoplossing](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) identiteit het probleem, en worden de stappen beschreven voor het oplossen van [fouten in tijdelijke](#troubleshoot-transient-errors) en [permanente of tijdelijke](#troubleshoot-the-persistent-errors)bieden. Ten slotte worden [alle artikelen voor verbindingsproblemen Azure SQL-Database](#all-topics-for-azure-sql-database-connection-problems).

Als u de problemen ondervindt, probeert u de stappen voor probleemoplossing die in dit artikel worden beschreven.
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Oorzaak

Problemen met de verbinding kunnen een van de volgende oorzaken hebben:

- Aanbevolen procedures en richtlijnen voor het ontwerpen van toepassing tijdens het ontwerpproces van toepassing is mislukt.  Zie [Overzicht van SQL Database ontwikkeling](sql-database-develop-overview.md) aan de slag.
- Azure SQL-Database opnieuw configureren
- Firewall-instellingen
- Verbindingstime-out
- Aanmelding met onjuiste informatie
- Maximale limiet bereikt op sommige bronnen Azure SQL-Database

Verbindingsproblemen met Azure SQL-Database kunnen in het algemeen als volgt worden ingedeeld:

- [Tijdelijke fouten (tijdelijk of onderbroken)](#troubleshoot-transient-errors)
- [De permanente of tijdelijke fouten (fouten die regelmatig terugkeren)](#troubleshoot-the-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Probeer de probleemoplosser voor problemen met de Azure SQL-Database

Als u een specifieke verbindingsfout tegenkomt, [Dit hulpprogramma](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), waarmee u snel identiteit en proberen het probleem.

## <a name="troubleshoot-transient-errors"></a>Tijdelijke fouten oplossen
Als uw toepassing fouten van voorbijgaande aard is, lees de volgende onderwerpen voor tips over het oplossen van problemen en het verminderen van de frequentie van deze fouten:

- [Problemen met Database &lt;x&gt; op de Server &lt;y&gt; is niet beschikbaar (fout: 40613)](sql-database-troubleshoot-connection.md)
- [Problemen oplossen, diagnose en voorkomen van fouten in SQL-verbinding en tijdelijke voor SQL-Database](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>

## <a name="troubleshoot-persistent-errors-non-transient-errors"></a>Fouten permanente (tijdelijke fouten)

Als de toepassing niet voortdurend verbinding maken met Azure SQL-Database, wijst dit doorgaans op een probleem met een van de volgende opties:

- Configuratie van de firewall. De Azure SQL-database of de client firewall blokkeert verbindingen met Azure SQL-Database.
- Nieuwe configuratie op de client het netwerk: bijvoorbeeld een nieuw IP-adres of een proxyserver.
- Gebruikersfout: bijvoorbeeld verkeerd verbindingsparameters, zoals de naam van de server in de verbindingsreeks.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Stappen voor het oplossen van problemen met de permanente

1.  [Firewall-regels](sql-database-configure-firewall-settings.md) instellen dat de client-IP-adres.
2.  Zorg dat poort 1433 geopend voor uitgaande verbindingen is op alle firewalls tussen de client en Internet. [SQL servertoegang toestaan Windows Firewall configureren](https://msdn.microsoft.com/library/cc646023.aspx) voor extra verwijzingen bekijken.
3.  Controleer of de verbindingsreeks en andere instellingen. Zie de sectie Connection String in het [onderwerp van connectiviteit problemen](sql-database-connectivity-issues.md#connections-to-azure-sql-database).
4.  Controleer of health service in het dashboard. Als u denkt er een regionale stroomstoring dat, Zie [herstellen na een storing](sql-database-disaster-recovery.md) voor stappen om een nieuwe regio te herstellen.

## <a name="all-topics-for-azure-sql-database-connection-problems"></a>Alle onderwerpen voor Azure SQL-Database oplossen

De volgende tabel worden voor elke verbinding probleem onderwerp die rechtstreeks voor de service Azure SQL-Database.


| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 1 | [Problemen met verbinding met Azure SQL-Database](sql-database-troubleshoot-common-connection-issues.md) | Dit is de pagina voor het oplossen van verbindingsproblemen in Azure SQL-Database. Hierin is het identificeren en oplossen van fouten in tijdelijke en permanente of tijdelijke in Azure SQL-Database. |
| 2 | [Problemen oplossen, diagnose en voorkomen van fouten in SQL-verbinding en tijdelijke voor SQL-Database](sql-database-connectivity-issues.md) | Informatie over het oplossen van problemen, opsporen en voorkomen dat een tijdelijke fout in Azure SQL-Database of een SQL-verbindingsfout. |
| 3 | [Algemene richtlijnen voor tijdelijke fout afhandeling](best-practices-retry-general.md) | Algemene richtlijnen voor het afhandelen van de fout met betrekking tot tijdelijke biedt wanneer u verbinding maakt met Azure SQL-Database. |
| 4 | [Problemen met verbindingen met Microsoft Azure SQL-Database](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) | Dit hulpprogramma helpt identiteit het probleem oplossen van fouten. |
| 5 | [Problemen met ' Database &lt;x&gt; op de server &lt;y&gt; is momenteel niet beschikbaar. Probeer de verbinding later"fout](sql-database-troubleshoot-connection.md) | Beschrijving van het identificeren en oplossen van een 40613-fout: "Database &lt;x&gt; op de server &lt;y&gt; is momenteel niet beschikbaar. Probeer opnieuw de verbinding later." |
| 6 | [SQL-foutcodes voor SQL-Database-clienttoepassingen: verbindingsfout en andere problemen](sql-database-develop-error-messages.md) | Biedt informatie over de foutcodes van de SQL voor SQL-Database-clienttoepassingen, zoals algemene database verbindingsfouten, problemen met de database kopiëren en algemene fouten. |
| 7 | [Richtlijnen voor Azure SQL-Database prestaties voor enkele databases](sql-database-performance-guidance.md) | Biedt richtlijnen waarmee u kunt bepalen welke service tier geschikt is voor uw toepassing. Ook bevat aanbevelingen voor de optimalisatie van uw toepassing te halen uit uw Azure SQL-Database. |
| 8 | [Overzicht SQL Database-ontwikkeling](sql-database-develop-overview.md) | Koppelingen naar voorbeelden van code voor diverse technologieën waarmee u kunt verbinding maken met en interactie met Azure SQL-Database. |
| 9 | Een upgrade uitvoeren naar Azure SQL Database v12 pagina ([portal Azure](sql-database-upgrade-server-portal.md), [PowerShell](sql-database-upgrade-server-powershell.md)) | Geeft aanwijzingen voor het upgraden van bestaande V11: Azure SQL Database servers en databases naar Azure SQL Database V12 met Azure portal of PowerShell. |


## <a name="next-steps"></a>Volgende stappen

- [Oplossen van prestatieproblemen Azure SQL-Database](sql-database-troubleshoot-performance.md)
- [Problemen met machtigingen Azure SQL-Database](sql-database-troubleshoot-permissions.md)
- [Zie alle onderwerpen voor de service Azure SQL-Database](sql-database-index-all-articles.md)
- [De documentatie over Microsoft Azure zoeken](http://azure.microsoft.com/search/documentation/)
- [De meest recente updates voor de service Azure SQL-Database weergeven](http://azure.microsoft.com/updates/?service=sql-database)


## <a name="additional-resources"></a>Aanvullende bronnen

- [Overzicht SQL Database-ontwikkeling](sql-database-develop-overview.md)
- [Algemene richtlijnen voor tijdelijke fout afhandeling](../best-practices-retry-general.md)
- [Gegevensverbindingsbibliotheek voor SQL-Database en SQL Server](sql-database-libraries.md)
- [Het pad van het leren voor het gebruik van Azure SQL-Database](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database)
- [Het pad van het leren voor het gebruik van de elastische databasefuncties en hulpprogramma 's](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale) 
