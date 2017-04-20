<properties
    pageTitle="Back-up maken van databases met ingeschakelde uitrekken | Microsoft Azure"
    description="Meer informatie over het back-up van Stretch\-databases ingeschakeld."
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
    ms.date="10/14/2016"
    ms.author="douglasl"/>

# <a name="backup-stretch-enabled-databases"></a>Back-up uitrekken geschikte databases

Back-ups kunnen u vele soorten storingen, fouten en rampen.  

-   U moet back-up van uw uitrekken\-SQL Server-databases wordt ingeschakeld.  

-   Microsoft Azure automatisch een back-up van de gegevens in externe uitrekken Database van SQL Server is gemigreerd naar Azure.  

>    [AZURE.NOTE] Back-up is slechts een onderdeel van een volledige hoge beschikbaarheid en continuïteit bedrijfsoplossing. Zie voor meer info over hoge beschikbaarheid [Oplossingen met hoge beschikbaarheid](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="back-up-your-sql-server-data"></a>Back-up van uw SQL Server-gegevens  

Back-up van uw uitrekken\-ingeschakeld SQL Server-databases, kunt u doorgaan met het gebruik van de SQL Server-back-methoden die u momenteel gebruikt. Zie [een Back-Up en terugzetten van SQL Server-Databases](https://msdn.microsoft.com/library/ms187048.aspx)voor meer informatie.

Back-ups van een database van SQL Server uitrekken is ingeschakeld, bevatten alleen lokale gegevens en gegevens komen in aanmerking voor migratie op het punt in de tijd waarop de back-up wordt uitgevoerd. \(In aanmerking komende gegevens zijn gegevens die nog niet zijn gemigreerd, maar worden gemigreerd naar Azure op basis van de migratie-instellingen van de tabellen.\) Dit staat bekend als een **recente** back-up en bevat niet de gegevens al zijn gemigreerd naar Azure.  

## <a name="back-up-your-remote-azure-data"></a>Back-ups extern Azure   

Microsoft Azure automatisch een back-up van de gegevens in externe uitrekken Database van SQL Server is gemigreerd naar Azure.  

### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure vermindert het risico van verlies van gegevens met automatische back-up  
De service SQL Server-Database uitrekken op Azure beschermt uw externe databases met automatische opslag momentopnamen ten minste elke 8 uur. Elke momentopname voor 7 dagen te voorzien van een scala aan mogelijke herstelpunten blijft behouden.  

### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure vermindert het risico van verlies van gegevens met geo\-redundantie  
Azure databaseback-ups worden opgeslagen op de geo\-redundante opslag voor Azure (RA\-GRS) en daarom geo\-standaard overbodig. Geo\-redundante opslag uw gegevens worden gerepliceerd naar een secundaire gebied dat honderden mijlen van de primaire regio. In de primaire en secundaire gebieden, uw gegevens drie keer elk gerepliceerd in fout met betrekking tot afzonderlijke domeinen en domeinen upgraden. Dit zorgt ervoor dat de gegevens duurzaam zelfs in het geval van een volledige regionale stroomstoring of rampen die Hiermee maakt u een van de Azure regio's niet beschikbaar.

### <a name="stretchRPO"></a>Uitrekken Database vermindert het risico van verlies van gegevens voor uw Azure gegevens door het tijdelijk bewaren van gemigreerde rijen
Nadat de Database uitrekken migreert in aanmerking komende rijen van SQL Server naar Azure, behouden de rijen in de staging-tabel voor een minimumaantal van 8 uur. Als u een back-up van uw Azure database terugzet, uitrekken Database de rijen die zijn opgeslagen in de staging-tabel gebruikt de SQL-Server en de databases Azure afstemmen.

Nadat u een back-up van uw gegevens Azure teruggezet, u moet de opgeslagen procedure [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) om te herstellen van het uitrekken uitvoeren\-SQL Server-database op de externe database Azure ingeschakeld. Wanneer u **sys.sp_rda_reauthorize_db**uitvoert, uitrekken Database automatisch sluitend wordt gemaakt de SQL-Server en de Azure-databases.

De opgeslagen procedure [sys.sp_rda_set_rpo_duration](https://msdn.microsoft.com/library/mt707766.aspx) uitvoeren om te verhogen van het aantal uren van de gemigreerde gegevens in die Database uitrekken tijdelijk in de staging-tabel behoudt, en geef een getal groter is dan 8 uur. Om te bepalen hoeveel gegevens wilt behouden, kunt u de volgende factoren:
-   De frequentie van de automatische Azure-ups (ten minste elke 8 uur).
-   Het duurt na een probleem herkennen het probleem en besluit een back-up terugzetten.
-   De duur van de bewerking voor terugzetten Azure.

> [AZURE.NOTE] De hoeveelheid gegevens in die Database uitrekken tijdelijk in de staging-tabel behoudt verhoogt, wordt het bedrag van de vereiste ruimte op de SQL-Server.

Als u wilt controleren op het aantal uren dat uitrekken Database momenteel tijdelijk in de staging-tabel behoudt, worden de opgeslagen procedure [sys.sp_rda_get_rpo_duration](https://msdn.microsoft.com/library/mt707767.aspx)uitgevoerd.

## <a name="see-also"></a>Zie ook

[Beheren en oplossen van problemen met Database uitrekken](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Back-Up en terugzetten van SQL Server-Databases](https://msdn.microsoft.com/library/ms187048.aspx)
