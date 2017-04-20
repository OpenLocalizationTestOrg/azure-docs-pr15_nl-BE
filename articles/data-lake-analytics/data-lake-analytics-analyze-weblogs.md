<properties
   pageTitle="Met behulp van Azure gegevens Lake Analytics Website-logboekbestanden analyseren | Azure"
   description="Informatie over het met behulp van gegevens Lake Analytics website-logboekbestanden analyseren. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-analyze-website-logs-using-azure-data-lake-analytics"></a>Zelfstudie: Website logs met Azure gegevens Lake Analytics analyseren

Informatie over het analyseren van Logboeken van website Analytics gegevens Lake, vooral op hoe u kunt nagaan welke verwijzingen naar fouten is uitgevoerd toen ze naar de website te gebruiken.

>[AZURE.NOTE] Als u alleen wilt zien van de toepassing werken, bespaart u tijd via de [interactieve zelfstudie gebruik Azure gegevens Lake Analytics](data-lake-analytics-use-interactive-tutorials.md). Deze zelfstudie is gebaseerd op hetzelfde scenario en dezelfde code. Het doel van deze zelfstudie is beschikken ontwikkelaars over de ervaring van het maken en uitvoeren van een toepassing gegevens Lake Analytics van begin tot eind.

## <a name="prerequisites"></a>Voorwaarden:

- **Visual Studio 2015, Visual Studio 2013 update 4, of Visual Studio 2012 met Visual C++ is geïnstalleerd**.
- **Microsoft Azure SDK voor .NET versie 2.5 of hoger**.  Installeren met de [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).
- **[Hulpmiddelen voor Visual Studio voor meer gegevens](http://aka.ms/adltoolsvs)**.

    Zodra de gegevens meer Tools voor Visual Studio is geïnstalleerd, ziet u een menu met **Meer gegevens** in Visual Studio:

    ![Visual Studio U SQL-menu](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Basiskennis van Lake Analytics van gegevens en de gegevens meer hulpprogramma's voor Visual Studio**. Zie aan de slag:

    - [Aan de slag met Azure gegevens Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md).
    - [Ontwikkelt U SQL - script met meer gegevens tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

- **Gegevens Lake Analytics-account.**  Zie [een Azure gegevens Lake Analytics-account maken](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).

    De extra gegevens Lake ondersteunt geen gegevens meer Analytics-accounts maken.  U hebt dus te maken met behulp van de Portal in Azure, Azure PowerShell, .NET SDK of Azure CLI.
- **De voorbeeldgegevens uploaden naar de gegevens Lake Analytics-account.** Zie [SearchLog.tsv voor de standaardaccount voor gegevensopslag Lake uploaden](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Een Data Lake Analytics als taak wilt uitvoeren, moet u enkele gegevens. Hoewel de gegevens Lake's uploaden-gegevens ondersteunt, gebruikt u de portal voor het uploaden van de voorbeeldgegevens gemakkelijker als u deze zelfstudie volgen.

## <a name="connect-to-azure"></a>Verbinding maken met Azure

Voordat u kunt bouwen en testen van alle U SQL-scripts, moet u eerst verbinding maken met Azure.

**Verbinding maken met gegevens Lake Analytics**

1. Open de Visual Studio.
2. Klik in het menu **Data meer** **Opties en instellingen**.
4. Klik op **Aanmelden**of **Gebruiker wijzigen** als iemand is aangemeld en volg de instructies.
5. Klik op **OK** om het dialoogvenster Opties en instellingen te sluiten.

**Uw gegevens Lake Analytics accounts zoeken**

1. Open in Visual Studio **Server Explorer** door druk op **CTRL + ALT + S**.
2. In **Server Explorer** **Azure**vouwen en vouw **Gegevens Lake Analytics**. U dient een overzicht van uw gegevens Lake Analytics accounts als die er zijn. U kunt geen gegevens Lake Analytics accounts maken vanuit de studio. Zie [Aan de slag met Azure gegevens Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md) of [Aan de slag met Azure gegevens Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md)een account maken.

## <a name="develop-u-sql-application"></a>U SQL-toepassingen ontwikkelen

Een toepassing U SQL is voornamelijk een U SQL-script. Meer informatie over U SQL, Zie [aan de slag met U SQL](data-lake-analytics-u-sql-get-started.md).

U kunt operatoren voor optellen door de gebruiker gedefinieerde toevoegen aan de toepassing.  Zie [U-SQL ontwikkelen door de gebruiker gedefinieerde operators voor gegevens Lake Analytics taken](data-lake-analytics-u-sql-develop-user-defined-operators.md)voor meer informatie.

**Maken en indienen van een taak gegevens Lake Analytics**

1. Klik op **Nieuw**in het menu **bestand** en klik vervolgens op **Project**.
2. Selecteer het type Project U SQL.

    ![nieuwe U SQL Visual Studio-project](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Klik op **OK**. Visual studio maakt een oplossing met een Script.usql-bestand.
4. Voer het volgende script in het bestand Script.usql:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    De U-SQL, Zie [aan de slag met Data Lake Analytics U SQL-taal](data-lake-analytics-u-sql-get-started.md).    

5. Een nieuwe U SQL-script toevoegen aan het project en voer de volgende gegevens:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. Ga terug naar de eerste U SQL-script en naast de knop **verzenden** klikt, geeft u uw Analytics-account.
7. In **Solution Explorer**, klikt u met de rechtermuisknop op **Script.usql**en klik op **Script maken**. Controleer of de resultaten in het deelvenster Uitvoer.
8. In de **Solution Explorer**, klikt u met de rechtermuisknop op **Script.usql**en klik op **Script indienen**.
9. Controleer of u de **Analytics-Account** de waar u de taak uitvoeren en klik op **verzenden**. Resultaten van indienen en de taakkoppeling zijn beschikbaar in de gegevens meer hulpprogramma's voor Visual Studio venster wanneer het verzenden is voltooid.
10. Wacht totdat de taak is voltooid.  Als de taak is mislukt, ontbreekt er waarschijnlijk het bestand.  Zie de sectie vereisten van deze zelfstudie. Zie voor meer informatie over probleemoplossing [Monitor en het oplossen van Azure gegevens Lake Analytics taken](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Wanneer de taak is voltooid, ziet u het volgende scherm:

    ![gegevens lake analytics analyseren weblogs website logboekbestanden](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. Herhaal stappen 7 - 10 nu voor **Script1.usql**.

>[AZURE.NOTE]U kan niet lezen uit of schrijven naar een U SQL-tabel die is gemaakt of gewijzigd in hetzelfde script.  Daarom heb ik twee scripts gebruiken voor dit voorbeeld.

**Voor een overzicht van de uitvoer van de taak**

1. In **Server Explorer** **Azure**uitvouwen, vouw **Gegevens Lake Analytics**Vouw uw account gegevens Lake Analytics, vouw **Opslag rekeningen**, met de rechtermuisknop op de standaardaccount voor gegevensopslag Lake en klik vervolgens op **Explorer**.
2.  Dubbelklik op **voorbeelden** om de map te openen, en dubbelklik vervolgens op **uitvoer**.
3.  Dubbelklik op **UnsuccessfulResponsees.log**.
4.  U kunt ook dubbelklikken op het uitvoerbestand in de grafiekweergave van de taak om te navigeren naar de uitvoer rechtstreeks.

## <a name="see-also"></a>Zie ook

Om te beginnen met gegevens Lake Analytics met verschillende hulpprogramma's Zie:

- [Aan de slag met Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md)
- [Aan de slag met Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [Aan de slag met Data Lake Analytics met .NET SDK](data-lake-analytics-get-started-net-sdk.md)

Meer onderwerpen van de ontwikkeling zien:

- [U SQL scripts schrijven met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Aan de slag met Azure gegevens Lake Analytics U SQL-taal](data-lake-analytics-u-sql-get-started.md)
- [U SQL door gebruiker gedefinieerde operators voor gegevens Lake Analytics projecten ontwikkelen](data-lake-analytics-u-sql-develop-user-defined-operators.md)
