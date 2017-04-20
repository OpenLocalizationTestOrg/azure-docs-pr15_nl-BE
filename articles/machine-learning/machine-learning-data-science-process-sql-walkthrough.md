<properties
    pageTitle="Het Team gegevens wetenschap proces in actie: met behulp van SQL Server | Microsoft Azure"
    description="Geavanceerde Analytics proces en de technologie in actie"  
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="fashah;bradsev"/>


# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Het Team gegevens wetenschap proces in actie: met behulp van SQL Server

In deze zelfstudie, u scenario maken en implementeren van een machine learning-model met behulp van SQL Server en een openbaar beschikbare dataset--de dataset [RDAM Taxi Trips](http://www.andresmh.com/nyctaxitrips/) . De procedure volgt een werkstroom standaardgegevens wetenschap: nemen en de gegevens, functies om te leren, te vergemakkelijken en vervolgens bouwen en implementeren van een model engineering verkennen.


## <a name="dataset"></a>RDAM Taxi Trips-Dataset beschrijving

De gegevens RDAM Taxi rit is ongeveer 20GB gecomprimeerd CSV-bestanden (~ 48GB niet-gecomprimeerd), die bestaat uit meer dan 173 miljoen afzonderlijke trips en de tarieven voor elke reis betaald. Elke reis record bevat de locatie ophalen en inleverbibliotheek en tijd, anoniem hack (bestuurder) licentienummer en nummer straten (unieke id van de taxi). De gegevens in het jaar 2013 heeft betrekking op alle trips en wordt geleverd in de volgende twee gegevenssets voor elke maand:

1. Trip_data CSV bevat details van de reis, zoals het aantal passagiers, ophalen en punten dropoff, duur van de reis en duur van reis. Hier zijn een aantal voorbeeldrecords:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. 'trip_fare' CSV bevat details van de voor elke reis, zoals betalingstype bedrag ritbedrag, toeslag en belastingen, tips en tolgelden, betaalde reissom en het totale bedrag betaald. Hier zijn een aantal voorbeeldrecords:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

De unieke sleutel reis deelnemen aan\_gegevens en reis\_ritbedrag is samengesteld uit de velden: straten, hack\_certificaat en ophalen van\_datum/tijd.

## <a name="mltasks"></a>Voorbeelden van taken die voorspelling

We zullen formuleren drie voorspelling problemen op basis van de *tip\_bedrag*, namelijk:

1. Binaire indeling: voorspellen of een tip betaald is voor een reis, dat wil zeggen een *tip\_bedrag* die groter is dan $0 een positief voorbeeld is, terwijl een *tip\_bedrag* is een negatief voorbeeld van $0.

2. Multiclass classificatie: voorspellen van het bereik van de tip voor de reis betaald. We delen de *tip\_bedrag* in vijf bakken of klassen:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. Regressie taak: te voorspellen, het bedrag van de tip voor een reis betaald.  


## <a name="setup"></a>Instelling van de Azure gegevensomgeving wetenschap voor geavanceerde analytics

Zoals u uit de gids voor [Uw omgeving plannen zien kunt](machine-learning-data-science-plan-your-environment.md) , zijn er verschillende opties voor het werken met de dataset RDAM Taxi Trips in Azure:

- Werken met de gegevens in Azure BLOB's en een model in Azure Machine Learning
- Laden van de gegevens in een SQL Server-database en het model in Azure Machine Learning

In deze zelfstudie wordt gedemonstreerd parallelle bulkimport van de gegevens naar een SQL Server verkennen, functie engineering en met behulp van SQL Server Management Studio af bemonstering, alsmede IPython notitieblok. [Voorbeeldscripts](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) en [IPython-laptops](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) worden gedeeld in de GitHub. Een monster IPython laptop te werken met de gegevens in Azure BLOB's is ook beschikbaar op dezelfde locatie.

Uw omgeving Azure Data Science instellen:

1. [Maak een account voor opslag](../storage/storage-create-storage-account.md)

2. [Een werkruimte Azure Machine Learning maken](machine-learning-create-workspace.md)

3. [Bepaling van een Data Science virtuele Machine](machine-learning-data-science-setup-sql-server-virtual-machine.md), die als een SQL-Server een server IPython-laptop fungeert.

    > [AZURE.NOTE] De voorbeeldscripts en IPython-laptops worden gedownload naar de virtuele machine van wetenschappelijke gegevens tijdens de installatie. Wanneer het script VM na de installatie is voltooid, worden de monsters in de bibliotheek documenten van uw VM:  
    > - Voorbeeld Scripts:`C:\Users\<user_name>\Documents\Data Science Scripts`  
    > - Monster IPython-laptops:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
    > waarbij `<user_name>` is Windows-aanmeldingsnaam van de VM. We verwijzen naar de mappen van het monster als **Voorbeeldscripts** en **Monster IPython-laptops**.


Op basis van de dataset grootte en locatie van gegevensbron de doelomgeving van geselecteerde Azure, dit scenario is vergelijkbaar met het [Scenario \#5: grote gegevensset in een lokale bestanden doel SQL Server in Azure VM](../machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>De gegevens ophalen uit een openbare bron

Als u de dataset [RDAM Taxi Trips](http://www.andresmh.com/nyctaxitrips/) uit de openbare locatie, kunt u een van de methoden die worden beschreven in [Gegevens verplaatsen naar en van Azure Blob-opslag](machine-learning-data-science-move-azure-blob.md) de gegevens kopiëren naar uw nieuwe virtuele machine.

Kopieert de gegevens met behulp van AzCopy:

1. Log in op de virtuele machine (VM)

2. Een nieuwe map maken in een schijf met gegevens van de VM (Opmerking: de tijdelijke schijf die wordt geleverd bij de VM als een schijf gegevens niet gebruiken).

3. Voer de volgende opdrachtregel voor Azcopy, < path_to_data_folder > vervangen door de map data gemaakt in (2) in een opdrachtpromptvenster:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

    Wanneer de AzCopy is voltooid, een totaal van 24 ingepakte CSV-bestanden (12 voor reis\_gegevens en 12 voor reis\_tarief) moet in de map data.

4. Unzip de gedownloade bestanden. Opmerking de map waar de gedecomprimeerde bestanden zich bevinden. Deze map zal worden verwezen als de < pad naar\_voor\_gegevens\_bestanden\>.

## <a name="dbload"></a>Bulk importeren van gegevens in SQL Server-Database

De prestaties van laden/overdracht van grote hoeveelheden gegevens naar een SQL-database en de daaropvolgende query's kunnen worden verbeterd met behulp van _gepartitioneerd tabellen en weergaven_. In deze sectie, zullen we de instructies beschreven in [Parallelle Bulk gegevens importeren met behulp van SQL-partitietabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) naar een nieuwe database maken en de gegevens in de gepartitioneerde tabellen tegelijkertijd laden.

1. Terwijl u bent aangemeld voor uw VM, start u **SQL Server Management Studio**.

2. Verbinding maken met Windows-verificatie.

    ![SSMS-verbinding][12]

3. Als u nog niet de SQL Server-verificatiemodus gewijzigd en een nieuwe SQL-aanmeldingsgebruiker gemaakt, opent u het scriptbestand met de naam **wijzigen\_auth.sql** in de map **Voorbeelden van Scripts** . De standaard-gebruikersnaam en het wachtwoord wijzigen. Klik op **! Uitvoeren van** in de werkbalk het script uit te voeren.

    ![Script uitvoeren][13]

4. Controleren en/of wijzigen van de SQL Server-database en logboekbestanden standaardmappen om ervoor te zorgen dat nieuwe databases gemaakt in een schijf met gegevens wordt opgeslagen. De SQL Server VM-afbeelding die is geoptimaliseerd voor datawarehousing belasting is vooraf geconfigureerd met de gegevens- en logboekbestanden van schijven. Als uw VM geen schijf met gegevens bevat en u een nieuwe virtuele harde schijven toegevoegd tijdens de installatie van VM, wijzigt u de standaardmappen als volgt:

    - Klik met de rechtermuisknop op de naam van de SQL Server in het linkerdeelvenster en klik op **Eigenschappen**.

        ![Eigenschappen van SQL Server][14]

    - **Database-instellingen** selecteren in de lijst **Selecteer een pagina** naar links.

    - Controleren en/of wijzigen van de **standaardlocatie van de Database** naar de locatie van de **Schijf met gegevens** van uw keuze. Dit is waar nieuwe databases als gemaakt met de standaardinstellingen locatie bevinden.

        ![Standaardinstellingen voor SQL-Database][15]  

5. U maakt een nieuwe database en een set van bestandsgroepen gepartitioneerde tabellen bevatten, opent u het voorbeeldscript **maken\_db\_default.sql**. Het script maakt een nieuwe database met de naam **TaxiNYC** en 12 bestandsgroepen op de standaardlocatie van de gegevens. Elke bestandsgroep wordt één maand van reis houdt\_gegevens en reis\_ritbedrag gegevens. Wijzig de naam van de database, indien gewenst. Klik op **! Uitvoeren van** voor het uitvoeren van het script.

6. Vervolgens maakt u twee partitietabellen, één voor de reis\_gegevens en een andere voor de reis\_tarief. Open het voorbeeldscript **maken\_gepartitioneerde\_table.sql**, die wordt:

    - Maak een partitiefunctie om de gegevens per maand te splitsen.
    - Een partitieschema van elke maand om gegevens te koppelen aan een andere bestandsgroep maken.
    - Twee gepartitioneerde tabellen die zijn toegewezen aan het partitieschema maken: **nyctaxi\_reis** komen de reis\_gegevens en **nyctaxi\_ritbedrag** komen de reis\_ritbedrag gegevens.

    Klik op **! Uitvoeren van** voor het uitvoeren van het script en de gepartitioneerde tabellen maken.

7. In de map **Voorbeelden van Scripts** zijn er twee PowerShell voorbeeldscripts voor het gebruik van bulk parallelle invoer van gegevens naar een SQL Server-tabellen.

    - **bcp\_parallelle\_generic.ps1** is een algemeen script met parallelle bulk importeren van gegevens in een tabel. Dit script voor de invoer- en variabelen zoals aangegeven in de regels met opmerkingen in het script wijzigen.
    - **bcp\_parallelle\_nyctaxi.ps1** is een vooraf geconfigureerde versie van het algemene script en in beide tabellen voor de RDAM Taxi Trips gegevens laden kan worden gebruikt.  

8. Met de rechtermuisknop op de **bcp\_parallelle\_nyctaxi.ps1** script naam en klik op **bewerken** om deze te openen in PowerShell. De vooraf ingestelde variabelen weergeven en wijzigen op de naam van de geselecteerde database, invoergegevens map, logboek doelmap en paden naar het sample format-bestanden **nyctaxi_trip.xml** en **nyctaxi\_fare.xml** (beschikbaar in de map **Voorbeelden van Scripts** ).

    ![Bulk Import Data][16]

    Kan ook u de verificatiemodus, de standaard Windows-verificatie. Klik op de groene pijl in de werkbalk uit te voeren. Het script start 24 bulkimportbewerkingen in parallelle, 12 voor elke gepartitioneerde tabel. U kan de voortgang van het importeren gegevens controleren door het openen van de SQL Server-standaardmap data als hierboven.

9. De PowerShell-script geeft de begin- en eindtijd. Wanneer alle bulk-invoer is voltooid, wordt de eindtijd gerapporteerd. Controleer de doelmap logboekbestand om te controleren dat de bulkimport gelukt, dat wil zeggen is, geen fouten gemeld in de doelmap voor het logboek.

10. De database is nu gereed voor de exploratie, functie-engineering en andere bewerkingen. Omdat de tabellen worden gepartitioneerd volgens de **afhalen\_datetime** veld, query's, waaronder **pickup\_datetime** voorwaarden in de **WHERE** -component zal profiteren van het partitieschema.

11. Verken het meegeleverde script in **SQL Server Management Studio** **monster\_queries.sql**. Als u wilt uitvoeren op een van de voorbeeldquery's, markeer de queryregels en klik op **! Uitvoeren van** in de werkbalk.

12. De RDAM Taxi Trips-gegevens zijn geladen in twee afzonderlijke tabellen. Ter verbetering van de join-bewerkingen, wordt het nadrukkelijk aanbevolen naar de index van de tabellen. Het voorbeeldscript **maken\_gepartitioneerde\_index.sql** gepartitioneerde indexen maakt voor de samengestelde join-sleutel **straten, hack\_licentie en afhalen\_datum/tijd**.

## <a name="dbexplore"></a>Verkennen en Engineering functie in SQL Server

In deze sectie zullen we verkennen en de functie voor het genereren van door het uitvoeren van SQL-query's rechtstreeks in de **SQL Server Management Studio** met behulp van de eerder gemaakte SQL Server-database uitvoeren. Een voorbeeld van een script met de naam **monster\_queries.sql** vindt u in de map **Voorbeelden van Scripts** . Het script te wijzigen van de naam van de database wijzigen als deze van de standaard afwijkt: **TaxiNYC**.

Wij zullen in deze oefening:

- Verbinding maken met **SQL Server Management Studio** gebruikt op Windows-verificatie of SQL-verificatie en SQL-aanmeldingsnaam en wachtwoord.
- Ontdek de verdelingen van de gegevens van een paar velden in verschillende tijdvensters.
- Onderzoek de kwaliteit van de gegevens van de velden lengte en breedte.
- Genereren binaire en multiclass classificatie labels op basis van de **tip\_bedrag**.
- Genereren van functies en reis afstanden compute/vergelijken.
- Voeg de twee tabellen en pak een steekproef die wordt gebruikt om modellen te bouwen.

Wanneer u klaar bent om te gaan naar Azure Machine Learning, kan u:  

1. De uiteindelijke SQL-query te pakken en de voorbeeldgegevens kopiëren plakken de query rechtstreeks in [Importeren gegevens] opslaan[ import-data] module in Azure Machine Learning, of
2. De bemonsterde en kunstmatige gegevens die u wilt gebruiken voor het model in een nieuwe database maken en de nieuwe tabel in de [Gegevens importeren] gebruiken[ import-data] module in Azure Machine Learning.

In deze sectie zullen we de uiteindelijke query om te pakken en de voorbeeldgegevens opslaan. De tweede methode wordt geïllustreerd in de sectie [verkennen en technische voorziening in de IPython-laptop](#ipnb) .

Voor een snelle controle van het aantal rijen en kolommen in de tabellen eerder met parallelle bulkimport, gevuld

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Exploratie: Reis distributie door de straten

In dit voorbeeld geeft de straten (taxi nummers) met meer dan 100 trips binnen een bepaalde periode. De query zouden kunnen profiteren van de toegang tot de gepartitioneerde tabel omdat deze is geconditioneerd door het partitieschema van **pickup\_datum/tijd**. Bij het controleren van de volledige gegevensset maakt ook gebruik van de gepartitioneerde tabel en/of scan indexeren.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Exploratie: Reis distributie door de straten en hack_license

    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Gegevens beoordeling: Controleer de records met onjuiste lengte en/of latitude

Hierbij wordt gekeken naar als een van de velden lengte en/of latitude een ongeldige waarde bevatten (radiaal graden moeten liggen tussen-90 en 90), of (0, 0) coördinaten.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Exploratie: Tipped versus niet-Gekantelde Trips distributie

In het volgende voorbeeld wordt het aantal trips die zijn Gekantelde versus niet binnen een bepaalde tijd Gekantelde periode (of de volledige gegevensset als die betrekking hebben op het volledige jaar) gezocht. Deze verdeling komt overeen met de verdeling van de binaire label later worden gebruikt voor de modellering van de binaire indeling.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Exploratie: Tip-klasse-bereik distributie

In dit voorbeeld berekent de verdeling van de tip bereiken in een bepaalde periode (of de volledige gegevensset als die betrekking hebben op het volledige jaar). Dit is de verdeling van de label-klassen die later worden gebruikt voor de modellering van de multiclass-classificatie.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Exploratie: Berekenen en vergelijken reis afstand

In dit voorbeeld wordt het ophalen en inleverbibliotheek lengtegraad en latitude naar SQL Geografie verwijst, berekent de afstand van de reis met SQL Geografie punten verschil en geeft als resultaat een aselecte steekproef van de resultaten van de vergelijking. In het voorbeeld beperkt de resultaten van geldige coördinaten alleen met de kwaliteit beoordeling gegevensquery waarvoor eerder.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>Functie Engineering in SQL-query 's

De label genereren en Geografie conversie exploratie query's kunnen ook worden gebruikt voor het genereren van labels/functies door de telling deel te verwijderen. Engineering SQL voorbeelden van extra functies zijn beschikbaar in de sectie [verkennen en technische voorziening in de IPython-laptop](#ipnb) . Het is efficiënter om de functie voor het genereren van query's uitvoeren op de volledige gegevensset of een grote subset van de SQL-query's die direct op het exemplaar van SQL Server-database uitvoeren met. De query's worden uitgevoerd in **SQL Server Management Studio**, IPython-laptop- of elke tool/ontwikkelomgeving die u toegang krijgen de database lokaal of extern tot kunt.

#### <a name="preparing-data-for-model-building"></a>Gegevens voorbereiden voor modellen bouwen

De volgende query joins de **nyctaxi\_reis** en **nyctaxi\_ritbedrag** tabellen, genereert een binaire indeling label **Gekantelde**, een indeling met meerdere klasse label **tip\_klasse**, en haalt een aselecte steekproef van 1% van de volledige, samengevoegde dataset. Deze query kan worden gekopieerd en vervolgens geplakt in de [Azure Machine Learning Studio](https://studio.azureml.net) [Gegevens importeren] rechtstreeks[ import-data] -module voor directe gegevens ingestie van het exemplaar van SQL Server-database in Azure. De query sluit de records met onjuiste (0, 0) coördinaten.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Verkennen en functie Engineering in IPython-laptop

In dit gedeelte worden we verkennen en functie genereren met behulp van Python- en SQL-query's op de eerder gemaakte SQL Server-database uitvoeren. Een monster IPython-laptop met de naam **machine-Learning-data-science-process-sql-story.ipynb** wordt in de map **Voorbeeld IPython-laptops** geleverd. Deze laptop is ook beschikbaar op [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

De aanbevolen volgorde bij het werken met grote gegevens is als volgt:

- In een kleine steekproef van de gegevens in een kader voor de gegevens in het geheugen gelezen.
- Sommige visualisaties en explorations met behulp van de bemonsterde gegevens uitvoeren.
- Experimenteer met de functie engineering met behulp van de gegevens van de steekproef.
- Python gebruiken om SQL-query's rechtstreeks op de SQL Server-database in Azure VM groter verkennen, gegevens manipuleren en functie-engineering.
- Bepaal de grootte van de steekproef voor Azure Machine Learning modellen bouwen.

Als u klaar bent om door te gaan naar Azure Machine Learning kan u:  

1. De uiteindelijke SQL-query te pakken en de voorbeeldgegevens kopiëren plakken de query rechtstreeks in [Importeren gegevens] opslaan[ import-data] module in Azure Machine Learning. Met deze methode wordt geïllustreerd in de sectie [Modellen bouwen in Azure Machine Learning](#mlmodel) .    
2. De bemonsterde en kunstmatige gegevens die u wilt gebruiken voor het model in een nieuwe database maken en gebruik vervolgens de nieuwe tabel de [Gegevens importeren] [ import-data] module.

Hier volgen een paar verkennen, data visualisatie en voorbeelden technische functie. Zie voor meer voorbeelden het voorbeeld SQL-IPython notitieblok in de map **Voorbeeld IPython-laptops** .

#### <a name="initialize-database-credentials"></a>Databasereferenties initialiseren

De verbindingsinstellingen voor de database in de volgende variabelen initialiseren:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Databaseverbinding maken

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Rapport aantal rijen en kolommen in de tabel nyctaxi_trip

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Totale aantal rijen = 173179759  
- Totaal aantal kolommen = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Alleen in een klein voorbeeld uit de SQL Server-Database

    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Tijd voor het lezen van dat de voorbeeldtabel is 6.492000 seconden  
Het aantal rijen en kolommen opgehaald = (84952, 21)

#### <a name="descriptive-statistics"></a>Beschrijvende statistiek

Nu gaan de bemonsterde gegevens verkennen. We beginnen met het kijken naar de beschrijvende statistiek voor de **reis\_afstand** (of andere) sleutelvelden:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Visualisatie: Voorbeeld van grafiek

Vervolgens kijken we de BoxPlot voor de reis afstand visualiseren van de quantiles

    df1.boxplot(column='trip_distance',return_type='dict')

![#1 uitzetten][1]

#### <a name="visualization-distribution-plot-example"></a>Visualisatie: Voorbeeld van de verdeling waarnemingspunt

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Uitzetten van #2][2]

#### <a name="visualization-bar-and-line-plots"></a>Visualisatie: De balk en lijn waarnemingspunten

In dit voorbeeld we bin de reis afstand in vijf opslaglocaties en visualiseren van de resultaten van binning.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

We kunnen uitzetten van de bovenstaande verdeling van de opslaglocatie in een bar of waarnemingspunt onderstaande regel

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 uitzetten][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 uitzetten][4]

#### <a name="visualization-scatterplot-example"></a>Visualisatie: Voorbeeld van de Scatterplot

We zien waarnemingspunt spreiding tussen **reis\_keer\_in\_sec.** en **reis\_afstand** om te zien of er een correlatie

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 uitzetten][6]

We kunnen op dezelfde manier controleren of de relatie tussen **rate\_code** en **reis\_afstand**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 uitzetten][8]

### <a name="sub-sampling-the-data-in-sql"></a>De gegevens in SQL bemonstering sub

Bij het voorbereiden van gegevens voor model bouwen in [Azure Machine Learning Studio](https://studio.azureml.net), kan u een besluit over de **SQL-query rechtstreeks in de module gegevens importeren gebruiken** of moet u de gegevens ontworpen en opgevangen in een nieuwe tabel, kunt u in de [Gegevens importeren] [ import-data] -module met een eenvoudige * *selecteren* van < uw\_nieuwe\_tabel\_naam >**.

In deze sectie maken we een nieuwe tabel waarin de gegevens van de bemonsterde en engineering. Een voorbeeld van een directe SQL-query voor gebouw model is beschikbaar in de sectie [gegevens te verkennen en technische functie in SQL Server](#dbexplore) .

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Maken van een voorbeeld van een tabel en te vullen met 1% van de gekoppelde tabellen. Plaatsen in de eerste tabel als deze bestaat.

In deze sectie, we de tabellen samenvoegen **nyctaxi\_reis** en **nyctaxi\_ritbedrag**, Pak een aselecte steekproef van 1% en de bemonsterde gegevens in een nieuwe tabelnaam **nyctaxi\_een\_procent**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Verkennen met behulp van SQL-query's in een notitieblok IPython

In deze sectie besproken verdelingen van gegevens met de gegevens van 1% bemonsterd die is vastgelegd in de nieuwe tabel die we eerder hebben gemaakt. Houd er rekening mee dat soortgelijke explorations kunnen worden uitgevoerd met behulp van de oorspronkelijke tabellen, met eventueel **TABLESAMPLE** beperken de exploratie steekproef of door het beperken van de resultaten op een bepaald moment periode met behulp van de **afhalen\_datetime** partities, zoals geïllustreerd in de sectie [gegevens te verkennen en technische functie in SQL Server](#dbexplore) .

#### <a name="exploration-daily-distribution-of-trips"></a>Exploratie: Dagelijkse distributie van trips

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Exploratie: Reis verdeling per straten

    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Functie genereren met behulp van SQL-query's in een notitieblok IPython

In deze sectie zullen we nieuwe labels genereren en functies rechtstreeks met behulp van SQL-query's op de tabel 1% gemaakt in de vorige sectie.

#### <a name="label-generation-generate-class-labels"></a>Etiketten genereren: Klasse etiketten genereren

In het volgende voorbeeld genereren we twee sets labels voor modellering:

1. Binaire klasse Labels **Gekantelde** (als een tip krijgt voorspellen)
2. Etiketten multiclass **tip\_klasse** (voorspellen de tip opslaglocatie of bereik)

        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''

        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()

        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''

        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Functie Engineering: Aantal functies voor kolommen bestaan

In dit voorbeeld worden een veld bestaan door elke categorie te vervangen door de telling van de exemplaren in de gegevens omgezet in een numeriek veld.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Functie Engineering: Opslaglocatie functies voor numerieke kolommen

In dit voorbeeld transformeert een continue numeriek veld in vooraf ingestelde categorie bereiken, numeriek veld bijvoorbeeld transformeren in een veld bestaan.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Functie Engineering: Locatie functies in decimale Latitude geografische lengte uitpakken

In dit voorbeeld uitsplitsing van de decimale weergave van een veld latitude en/of lengtegraad in meerdere regio velden van verschillende granulariteit, zoals land, plaats, stad, blok, enz. Houd er rekening mee dat de nieuwe geo-velden niet zijn toegewezen aan de werkelijke locaties. Zie voor informatie over toewijzing geocode locaties, [Bing Maps REST Services](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Controleer of de uiteindelijke vorm van de featurized-tabel

    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

We gaan nu verder met de modellen bouwen en implementeren van modellen in [Azure Machine Learning](https://studio.azureml.net). De gegevens zijn gereed voor de voorspelling problemen geïdentificeerd eerder, namelijk:

1. Binaire indeling: om te voorspellen of er een tip is betaald voor een reis.

2. Multiclass classificatie: voorspellen van het bereik van de tip betaald volgens de eerder gedefinieerde klassen.

3. Regressie taak: te voorspellen, het bedrag van de tip voor een reis betaald.  


## <a name="mlmodel"></a>Gebouw modellen in Azure Machine Learning

Om te beginnen met de oefening modellering, log in op uw werkruimte Azure Machine Learning. Als u nog niet een machine learning werkruimte hebt gemaakt, ziet u [een werkruimte Azure Machine leren maken](machine-learning-create-workspace.md).

1. Zie aan de slag met Azure Machine Learning [Wat is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)

2. Log in op [Azure Machine Learning Studio](https://studio.azureml.net).

3. De introductiepagina van Studio biedt een schat aan informatie, video's, zelfstudies, koppelingen naar de verwijzing Modules en andere bronnen. Neem contact op met de [Azure Machine Informatiepunt documentatie](https://azure.microsoft.com/documentation/services/machine-learning/)voor meer informatie over Azure Machine Learning.

Een typische training experiment bestaat uit het volgende:

1. Een **+ Nieuw** experiment maken.
2. De gegevens naar Azure Machine Learning ophalen.
3. Vooraf verwerken, transformeren en de gegevens desgewenst bewerken.
4. Functies worden gegenereerd wanneer dat nodig is.
5. De gegevens opsplitsen in datasets training, validatie/testen (of afzonderlijke gegevenssets voor elke).
6. Selecteer een of meer machine learning algoritmen afhankelijk van het leren-probleem op te lossen. Bijvoorbeeld, binaire indeling, multiclass classificatie en regressie.
7. Een of meer modellen met behulp van de dataset training trainen.
8. Score dataset validatie met behulp van de opgeleide modellen.
9. Evaluatie van de modellen berekent u de relevante parameters voor het probleem van leren.
10. Fijn afstemmen van de modellen en selecteer het beste model te implementeren.

In deze oefening hebben we al verkend en engineering van de gegevens in SQL Server en besloot op de grootte van de steekproef te nemen in Azure Machine Learning. We besloten een of meer van de voorspellende modellen te bouwen:

1. Azure Machine Learning met behulp van de [Gegevens importeren] de gegevens[ import-data] module beschikbaar in de sectie **gegevensinvoer en uitvoer** . Zie de [Gegevens importeren] voor meer informatie[ import-data] module pagina.

    ![Azure Machine Learning importgegevens][17]

2. Selecteer **Azure SQL-Database** als **gegevensbron** in het deelvenster **Eigenschappen** .

3. De DNS-naam van de database invoeren in het veld **naam van de databaseserver** . Indeling:`tcp:<your_virtual_machine_DNS_name>,1433`

4. De **naam van de Database** invoeren in het bijbehorende veld.

5. Voer de **SQL-gebruikersnaam** in de **aqccount gebruikersnaam en het wachtwoord in de **Server gebruiker account wachtwoord **.

6. Schakel de optie **accepteren van een servercertificaat** .

7. In het tekstgebied **databasequery** bewerken monsters plakken op de query die de nodige haalt databasevelden (met inbegrip van alle berekende velden, zoals de labels) of pijl-omlaag die de gegevens op de gewenste steekproefgrootte.

In de volgende afbeelding wordt een voorbeeld van een binaire indeling experiment lezen gegevens rechtstreeks vanuit de SQL Server-database. Soortgelijke experimenten kunnen worden samengesteld voor de multiclass-indeling en regressie problemen.

![Azure Machine Learning trein][10]

> [AZURE.IMPORTANT] In de gegevens modelleren extractie en bemonstering query voorbeelden vermeld in de voorgaande secties, **alle labels voor de oefeningen drie modellen zijn opgenomen in de query**. Een belangrijk (vereist) in elk van de oefeningen modelleren is uit te **sluiten** de onnodige labels voor de twee problemen en eventuele andere **doel lekt**. Voor bijvoorbeeld als u binaire indeling, gebruikt het label **Gekantelde** en uitsluiten van de velden **tip\_klasse**, **punt\_bedrag**, en **Totaal\_bedrag**. Zijn doel lekkage aangezien zij de tip impliceren betaald.
>
> Als u wilt uitsluiten van overbodige kolommen en/of lekkage richten, mag u de [Kolommen selecteren in de Dataset] [ select-columns] module of de [Metagegevens bewerken][edit-metadata]. Zie voor meer informatie [Kolommen selecteren in de Dataset] [ select-columns] en [Metagegevens bewerken] [ edit-metadata] verwijzen naar pagina's.

## <a name="mldeploy"></a>Modellen in Azure Machine Learning implementeren

Als uw model klaar is, kunt u het gemakkelijk implementeren als een webservice rechtstreeks van het experiment. Zie voor meer informatie over het implementeren van Azure Machine Learning web-services [implementeren een webservice Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

Als u wilt implementeren op een nieuwe webservice, moet u:

1. Een score experiment maken.
2. Implementeren van de webservice.

Klik op **Maken SCOREN EXPERIMENTEREN** in de onderste actiebalk om een score experiment uit een opleiding **voltooid** experiment.

![Azure scoren][18]

Azure Machine Learning probeert te maken van een score experiment op basis van de onderdelen van het experiment training. Het programma zal met name:

1. De opgeleide model opslaan en verwijderen van de trainingsmodules model.
2. Een logische **invoerpoort** overeenkomen met het schema van de verwachte ingevoerde gegevens identificeren.
3. Een logische **uitvoerpoort** voor de verwachte web service-uitvoerschema herkennen.

Wanneer de score experiment wordt gemaakt, bekijken en naar wens aanpassen. Een typische aanpassing is de dataset invoer en/of de query vervangen door een exclusief label velden, zoals deze is alleen beschikbaar als de service wordt genoemd. Het is een goede gewoonte om de grootte van de dataset invoer en/of de query beperken tot enkele records, net voldoende om aan te geven de invoer schema. Voor de uitvoerpoort voor alle invoervelden uitsluiten en alleen de **Labels gescoord** en **Kansen gescoord** in de uitvoer op basis van de [Kolommen selecteren in de Dataset] is[ select-columns] module.

In de volgende afbeelding wordt een voorbeeld van een experiment te scoren. Als u klaar bent om te implementeren, klik op **Publiceren WEBSERVICE** in de onderste actiebalk.

![Azure Machine Learning publiceren][11]

U hebt een Azure wetenschap gegevensomgeving, gewerkt met een grote openbare dataset van gegevens oplevert als model voor training en implementatie van een webservice Azure Machine Learning gemaakt om recap in deze zelfstudie met stapsgewijze instructies.

### <a name="license-information"></a>Licentie-informatie

In dit scenario van het monster en de bijbehorende scripts en IPython notebook(s) gedeeld door Microsoft onder de MIT-licentie. Controleer of u het bestand LICENSE.txt in in de directory van de voorbeeldcode op GitHub voor meer informatie.

### <a name="references"></a>Verwijzingen

• [Andrés Monroy RDAM Taxi Trips pagina downloaden](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing van RDAM Taxi rit gegevens door Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Taxi RDAM en Limousine Commissie onderzoek en statistieken](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
