<properties
    pageTitle="Twitter gegevens analyseren met Hadoop in HDInsight | Microsoft Azure"
    description="Informatie over het onderdeel gebruiken om gegevens op Hadoop in HDInsight de frequentie van het gebruik van een bepaald woord zoeken Twitter te analyseren."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Twitter analyseren met behulp van de component in HDInsight

Sociale websites zijn een van de belangrijkste bevorderende factoren voor de aanneming van big data. Openbare API's die door websites zoals Twitter zijn een nuttige bron van gegevens voor het analyseren en begrijpen van de populaire trends. In deze zelfstudie wordt u tweets ophalen met behulp van een streaming API Twitter en gebruik vervolgens Apache component in Azure HDInsight om een lijst van Twitter gebruikers die de meeste tweets die deel uitmaakt van een bepaald woord verzonden.

> [AZURE.NOTE] De stappen in dit document moet een cluster van Windows-gebaseerde HDInsight. Zie voor specifieke stappen naar een Linux-gebaseerde cluster, [Twitter analyseren gegevens met behulp van de component in HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).


##<a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Een werkstation** met Azure PowerShell is geïnstalleerd en geconfigureerd. 

    U moet voor het uitvoeren van Windows PowerShell-scripts Azure PowerShell als administrator uitvoeren en het uitvoeringsbeleid ingesteld op *RemoteSigned*. [Uitvoeren van Windows PowerShell-scripts]Zie[powershell-script].

    Voordat u Windows PowerShell-scripts, moet dat u verbonden bent met uw abonnement Azure met behulp van de volgende cmdlet:

        Login-AzureRmAccount

    Als er meerdere Azure abonnementen, gebruikt u de volgende cmdlet instellen van het huidige abonnement:

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Een Azure HDInsight cluster**. Zie voor instructies voor het cluster wordt ingericht, [aan de slag met HDInsight] [ hdinsight-get-started] of [clusters bepaling HDInsight] [hdinsight-provision]. U moet de naam van het cluster verderop in de handleiding.

De volgende tabel worden de bestanden die in deze zelfstudie wordt gebruikt:

Bestanden|Beschrijving
---|---
/tutorials/Twitter/Data/tweets.txt|De brongegevens voor de taak van de component.
/tutorials/Twitter/output|De uitvoermap voor het project component. De standaard component taak output bestandsnaam is **000000_0**.
tutorials/Twitter/Twitter.hql|Het scriptbestand HiveQL.
/tutorials/Twitter/JobStatus|De status van de Hadoop.


##<a name="get-twitter-feed"></a>Get Twitter feed

In deze zelfstudie, gebruikt u de [Twitter streaming API's][twitter-streaming-api]. De specifieke Twitter [status/filter]streaming API die u wilt gebruiken is[twitter-statuses-filter].

>[AZURE.NOTE] Een bestand met 10.000 tweets en het script componentbestand (behandeld in de volgende sectie) geüpload in een openbare Blob-container. Als u wilt gebruikmaken van de geüploade bestanden kunt u deze sectie overslaan.

[Tweets gegevens](https://dev.twitter.com/docs/platform-objects/tweets) worden opgeslagen in de indeling JavaScript Object Notation (JSON) met een complexe, geneste structuur. In plaats van een groot aantal regels code schrijven met behulp van een conventionele programmeertaal, kunt u deze geneste structuur in een component-tabel transformeren zodat deze kan worden opgevraagd door een Structured Query Language (SQL)-zoals taal, genaamd HiveQL.

Twitter gebruikt OAuth voor geautoriseerde toegang tot de API. OAuth is een verificatieprotocol dat gebruikers toepassingen te handelen namens hen zonder het delen van hun wachtwoord goedkeuren. Meer informatie vindt u op [oauth.net](http://oauth.net/) of in de uitstekende [basisinformatie OAuth](http://hueniverse.com/oauth/) van Hueniverse.

De eerste stap gebruikt OAuth is een nieuwe toepassing maken op de site van de ontwikkelaar van Twitter.

**Twitter-toepassing maken**

1. Aanmelden bij [https://apps.twitter.com/](https://apps.twitter.com/). Als u niet een Twitter-account hebt, klikt u op de koppeling **Nu aanmelden** .
2. Klik op **nieuwe App maken**.
3. Voer de **naam**, **Beschrijving**, **Website**. U kunt maken van een URL voor het veld **Website** . De volgende tabel ziet u enkele voorbeeldwaarden wilt gebruiken:

    Veld|Waarde
    ---|---
    Naam|MyHDInsightApp
    Beschrijving|MyHDInsightApp
    Website|http://www.myhdinsightapp.com

4. **Ja, ik ga akkoord**controleren en klik vervolgens op **uw Twitter-toepassing maken**.
5. Klik op het tabblad **machtigingen** . Standaard de machtiging is **alleen-lezen**. Dit is voldoende voor deze zelfstudie.
6. Klik op het tabblad **toetsen en Access Tokens** .
7. Klik op **Mijn toegangstoken maken**.
8. Klik op **Test OAuth** in de rechterbovenhoek van de pagina.
9. Noteer de **consumentsleutel**, **consumentgeheim** **Access token**en **Access token secret**. U moet de waarden hoger in de zelfstudie.

In deze zelfstudie gebruikt u Windows PowerShell om de webservice aanroept. Zie voor een voorbeeld .NET C# [realtime Twitter sentiment analyseren met HBase in HDInsight][hdinsight-hbase-twitter-sentiment]. De populaire tool voor web service gesprekken is [*Curl-*][curl]. Krul kan worden gedownload vanaf [hier][curl-download].

>[AZURE.NOTE] Gebruik dubbele aanhalingstekens wanneer u in Windows de curl-opdracht, in plaats van enkele aanhalingstekens voor de optiewaarden.

**Tweets ophalen**

1. Open Windows PowerShell Integrated Scripting Environment (ISE). (In het Windows 8 startscherm, typ **PowerShell_ISE** en klik op **Windows PowerShell ISE**. Zie [Windows PowerShell in Windows 8 en Windows Start][powershell-start].)

2. Kopieer het volgende script in het script-veld:

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Login-AzureRmAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. Stel de eerste vijf tot acht variabelen in het script:


    Variabele|Beschrijving
    ---|---
    $clusterName|Dit is de naam van de HDInsight cluster waar u de toepassing uitvoert.
    $oauth_consumer_key|Dit is de Twitter toepassing **consumentsleutel** die u eerder opgeschreven bij het maken van de Twitter-toepassing.
    $oauth_consumer_secret|Dit is de Twitter toepassing **consumentgeheim** die u eerder hebt opgeschreven.
    $oauth_token|Dit is de Twitter toepassing **toegangstoken** die u eerder hebt opgeschreven.
    $oauth_token_secret|Dit is de Twitter toepassing **access token secret** die u eerder hebt opgeschreven.
    $destBlobName|Dit is de naam van de blob uitvoer. De standaardwaarde is **tutorials/twitter/data/tweets.txt**. Als u de standaardwaarde wilt wijzigen, moet u de Windows PowerShell-scripts dienovereenkomstig bijwerken.
    $trackString|De webservice retourneert de tweets die betrekking hebben op deze trefwoorden. De standaardwaarde is **Azure, Cloud, HDInsight**. Als u de standaardwaarde wijzigt, wordt u de Windows PowerShell-scripts dienovereenkomstig bijwerken.
    $lineMax|De waarde bepaalt hoeveel tweets het script wordt gelezen. Het duurt ongeveer drie minuten 100 tweets lezen. U kunt instellen dat een groter aantal, maar het langer duurt om te downloaden.

5. Druk op **F5** om het script uitvoert. Selecteer alle regels en druk op **F8**als u problemen, als tijdelijke oplossing ondervonden.
6. Ziet u 'Complete'! aan het einde van de uitvoer. Eventuele foutberichten wordt weergegeven in rood.

Als een procedure voor de validering, kunt u het uitvoerbestand, **/tutorials/twitter/data/tweets.txt**, controleren op uw Azure Blob-opslag met behulp van een Azure Opslagverkenner of Azure PowerShell. Zie voor een voorbeeld Windows PowerShell-script voor het weergeven van bestanden [Gebruik Blob-opslag met HDInsight][hdinsight-storage-powershell].



##<a name="create-hiveql-script"></a>HiveQL-script maken

Azure PowerShell gebruikt, kunt u meerdere HiveQL instructies één tegelijk uitvoeren of pakket van de HiveQL-instructie in een scriptbestand. In deze zelfstudie maakt u een script HiveQL. Het scriptbestand moet worden geüpload naar Azure Blob-opslag. In het volgende gedeelte wordt u het scriptbestand uitvoeren met behulp van Azure PowerShell.

>[AZURE.NOTE] Het scriptbestand component en een bestand met 10.000 tweets geüpload in een openbare Blob-container. Als u wilt gebruikmaken van de geüploade bestanden kunt u deze sectie overslaan.

Het script HiveQL wordt als volgt te werk:

1. **De tabel tweets_raw** in geval de tabel bestaat al.
2. **Maak de tabel tweets_raw component**. Deze tijdelijke component gestructureerde tabel bevat de gegevens voor verdere extraheren, transformeren en laden (ETL) verwerking. Zie voor informatie over partities, [zelfstudie component][apache-hive-tutorial].  
3. **Gegevens laden** uit de bronmap, /tutorials/twitter/data. De dataset grote tweets in geneste JSON-indeling is nu veranderd in een tijdelijke tabelstructuur voor component.
3. **De tabel tweets verwijderen** in geval de tabel bestaat al.
4. **De tweets-tabel maken**. Voordat u kunt query op basis van de dataset tweets met behulp van de component, moet u een andere ETL-proces uitgevoerd. Dit proces ETL definieert een meer gedetailleerde tabelschema voor de gegevens die u hebt opgeslagen in de tabel 'twitter_raw'.  
5. **Overschrijven-tabel invoegen**. Dit complexe component script zal ere van een set lange MapReduce taken door het cluster Hadoop. Afhankelijk van uw gegevensset en de grootte van het cluster, kan dit ongeveer 10 minuten duren.
6. **Insert map overschrijven**. Een query uitvoeren en de uitvoer van de gegevensset naar een bestand. Deze query retourneert een lijst met Twitter gebruikers die de meeste tweets die het woord 'Azure'.

**Een component-script maken en uploaden naar Azure**

1. Open Windows PowerShell ISE.
2. Kopieer het volgende script in het script-veld:

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. Stel de eerste twee variabelen in het script:

    Variabele|Beschrijving
    ---|---
    $clusterName|Voer de naam van de cluster HDInsight waar u de toepassing uitvoert.
    $subscriptionID|Voer uw Azure abonnement-ID.
    $sourceDataPath|De Azure Blob opslaglocatie waar de component query's wordt de gegevens lezen uit. U hoeft niet deze variabele te wijzigen.
    $outputPath|De Azure Blob opslaglocatie waar de component query's worden de resultaten. U hoeft niet deze variabele te wijzigen.
    $hqlScriptFile|De locatie en de bestandsnaam van het scriptbestand HiveQL. U hoeft niet deze variabele te wijzigen.

5. Druk op **F5** om het script uitvoert. Selecteer alle regels en druk op **F8**als u problemen, als tijdelijke oplossing ondervonden.
6. Ziet u 'Complete'! aan het einde van de uitvoer. Eventuele foutberichten wordt weergegeven in rood.

Als een procedure voor de validering, kunt u het uitvoerbestand, **/tutorials/twitter/twitter.hql**, controleren op uw Azure Blob-opslag met behulp van een Azure Opslagverkenner of Azure PowerShell. Zie voor een voorbeeld Windows PowerShell-script voor het weergeven van bestanden [Gebruik Blob-opslag met HDInsight][hdinsight-storage-powershell].  


##<a name="process-twitter-data-by-using-hive"></a>Twitter gegevens verwerken met behulp van component

U hebt al het werk voorbereiding. Nu kunt u het script component starten en controleer de resultaten.

### <a name="submit-a-hive-job"></a>Een component-taak verzenden

De volgende Windows PowerShell-script gebruiken voor het uitvoeren van het script voor de component. U moet de eerste variabele instellen.

>[AZURE.NOTE] Met de tweets en de HiveQL-script dat u hebt geüpload in de laatste twee secties, stelt $hqlScriptFile op "/ tutorials/twitter/twitter.hql". Met de velden die zijn geüpload naar een openbare blob voor u, stelt $hqlScriptFile op "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
    #endregion

### <a name="check-the-results"></a>De resultaten controleren

De volgende Windows PowerShell-script gebruiken om de uitvoer van de taak component. U moet de eerste twee variabelen instellen.

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] \001 de component-tabel gebruikt als het veldscheidingsteken. Het scheidingsteken is niet zichtbaar in de uitvoer.

Nadat de resultaten van de analyse in Azure Blob-opslag zijn geplaatst, kunt u de gegevens exporteren naar een database/SQL Azure SQL server, de gegevens exporteren naar Excel met behulp van Power Query of de toepassing de gegevens met behulp van de component ODBC-stuurprogramma verbinding. Voor meer informatie, Zie [Sqoop voor gebruik met HDInsight][hdinsight-use-sqoop], [analyseren vertraging vluchtgegevens met HDInsight][hdinsight-analyze-flight-delay-data], [HDInsight met Power Query Excel verbinding][hdinsight-power-query], en [HDInsight met component ODBC-stuurprogramma voor de Microsoft Excel verbinding][hdinsight-hive-odbc].

##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebben we gezien hoe u een ongestructureerde JSON dataset transformeren in een gestructureerde component-tabel, query, verkennen en analyseren van gegevens op Twitter via HDInsight op Azure. Voor meer informatie, Zie:

- [Aan de slag met HDInsight][hdinsight-get-started]
- [Realtime Twitter sentiment met HBase in HDInsight analyseren][hdinsight-hbase-twitter-sentiment]
- [Gegevens te analyseren vlucht vertraging met HDInsight][hdinsight-analyze-flight-delay-data]
- [Excel verbinden met HDInsight met Power Query][hdinsight-power-query]
- [Excel verbinden met HDInsight met de component Microsoft ODBC-stuurprogramma][hdinsight-hive-odbc]
- [Gebruik Sqoop met HDInsight][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
