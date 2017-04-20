<properties 
    pageTitle="End-to-End probleemoplossing met Azure opslag maatstaven en logboekregistratie, AzCopy en bericht Analyzer | Microsoft Azure" 
    description="Een zelfstudie om aan te tonen de end-to-end het oplossen van problemen met Azure opslag Analytics, AzCopy en Microsoft Message Analyzer" 
    services="storage" 
    documentationCenter="dotnet" 
    authors="robinsh" 
    manager="carmonm"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/03/2016" 
    ms.author="robinsh"/>

# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>End-to-End probleemoplossing met Azure opslag maatstaven en logboekregistratie, AzCopy en bericht Analyzer 

[AZURE.INCLUDE [storage-selector-portal-e2e-troubleshooting](../../includes/storage-selector-portal-e2e-troubleshooting.md)]

## <a name="overview"></a>Overzicht

Diagnose en het oplossen van problemen is een belangrijke vaardigheid voor het maken en ondersteunen van toepassingen met Microsoft Azure opslag. Vanwege de gedistribueerde aard van een toepassing Azure opsporen en oplossen van fouten en problemen met de prestaties mogelijk ingewikkelder dan in een traditionele omgeving.

In deze zelfstudie wordt gedemonstreerd hoe client bepaalde fouten die mogelijk invloed op de prestaties en het oplossen van deze fouten van end-to-end aangeven met behulp van hulpprogramma's die door Microsoft en Azure-opslag om de clienttoepassing te optimaliseren. 

Deze handleiding biedt een praktische uitleg van een end-to-end-scenario voor het oplossen van problemen. Zie [Monitor, vaststellen, en Microsoft Azure opslag oplossen](storage-monitoring-diagnosing-troubleshooting.md)voor een diepgaande conceptuele gids Azure opslagtoepassingen oplossen. 

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Hulpprogramma's voor het oplossen van Azure opslag, toepassingen

Clienttoepassingen die gebruikmaken van Microsoft Azure opslag oplossen, kunt u een combinatie van hulpmiddelen om te bepalen wanneer er een probleem is opgetreden en wat de oorzaak van het probleem kan zijn. Deze hulpmiddelen zijn:

- **Azure opslag Analytics**. [Azure opslag Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) biedt metrics en logboekregistratie voor Azure opslag.
    - **Opslag statistieken** bijgehouden transactie maatstaven en maatstaven voor uw account opslag capaciteit. Met cijfers, kunt u bepalen hoe uw toepassing presteert volgens een groot aantal verschillende maatregelen. Zie [Opslag Analytics Metrics tabelschema](http://msdn.microsoft.com/library/azure/hh343264.aspx) voor meer informatie over de soorten bijgehouden door opslag Analytics metrics. 

    - Elke aanvraag **opslag registreren** om de Azure Storage-services op een server-side-logboek worden vastgelegd. Het logboek wordt bijgehouden gedetailleerde gegevens voor elke aanvraag, met inbegrip van de bewerking die wordt uitgevoerd, de status van de bewerking en de latentie van informatie. Zie [Opslag Analytics Log Format](http://msdn.microsoft.com/library/azure/hh343259.aspx) voor meer informatie over de aanvraag en het antwoord gegevens die in de logboeken wordt geschreven door Analytics opslag.

> [AZURE.NOTE] Opslag-rekeningen van het replicatietype van Zone redundante opslag (ZRS) beschikt niet over de metrics of de mogelijkheid tot het vastleggen op dit moment is ingeschakeld. 

- **Azure klassieke Portal**. Voor uw account voor opslag in de [Klassieke Azure-Portal](https://manage.windowsazure.com)kunt u maatstaven en logboekregistratie configureren. U kunt ook diagrammen en grafieken die laten zien hoe uw toepassing uitvoert na verloop van tijd weergeven en configureren van waarschuwingen om u te waarschuwen als u uw toepassing uitvoert anders dan verwacht voor een opgegeven waarde. 
    
    Zie [een account voor opslag in de Portal Azure Monitor](storage-monitor-storage-account.md) voor informatie over het configureren van het toezicht op de klassieke Azure-Portal.

- **AzCopy**. Logboeken voor Azure Storage Server worden opgeslagen als BLOB's, zodat u AzCopy gebruiken kunt om het logboekbestand BLOB's kopiëren naar een lokale map voor de analyse met behulp van Microsoft Message Analyzer. Zie de [overdracht van gegevens met het hulpprogramma AzCopy voor Command-Line](storage-use-azcopy.md) voor meer informatie over AzCopy.

- **Microsoft Message Analyzer**. Bericht Analyzer is een hulpprogramma waarmee logboekbestanden worden verbruikt en logboekgegevens worden weergegeven in een visuele indeling die gemakkelijk te filteren, zoeken en groep logboekgegevens in handige sets die u gebruiken kunt voor het analyseren van fouten en problemen met de prestaties. Zie [Bericht Analyzer operationele handleiding](http://technet.microsoft.com/library/jj649776.aspx) voor meer informatie over bericht Analyzer.

## <a name="about-the-sample-scenario"></a>Over het voorbeeldscenario

Voor deze zelfstudie, hier scenario waar metrics Azure opslag geeft aan dat een laag percentage succespercentage voor een toepassing die Azure opslag roept nader. De lage percentage voltooid percentage metric (weergegeven als **PercentSuccess** in de klassieke Azure-Portal en in de tabellen metrics) houdt de bewerkingen die slagen, maar die een HTTP-statuscode op die groter is dan 299 retourneren. Deze bewerkingen zijn in de logboekbestanden van de server-side storage met een status van de **ClientOtherErrors**vastgelegd. Zie voor meer informatie over het lage percentage geslaagde metric [zien lage PercentSuccess of analytics logitems bewerkingen hebben met de status van ClientOtherErrors](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure opslagbewerkingen mogelijk resultaat HTTP-statuscodes groter dan 299 als onderdeel van hun normale functionaliteit. Maar deze fouten in sommige gevallen blijkt dat u mogelijk uw clienttoepassing voor betere prestaties te optimaliseren. 

In dit scenario zullen we rekening houden met een laag percentage Slagingskans tot iets minder dan 100% worden. Kunt u een ander metrische niveau echter aan uw behoeften. Wij raden aan dat tijdens het testen van de toepassing, u een tolerantie van de basislijn voor de voornaamste prestatiestatistieken ophalen maakt. Bijvoorbeeld, kunt u wellicht bepalen, op basis van tests, dat de toepassing een consistente percentage slagingspercentage van 90% of 85% moet zijn. Als de metrische gegevens dat blijkt de toepassing is die afwijken van dat nummer, dan kunt u onderzoeken wat de oorzaak van de verhoging. 

In ons voorbeeld scenario nadat we hebben vastgesteld dat de metric van percentage voltooid percentage lager is dan 100%, onderzoeken we de logboeken om de fouten die overeenstemmen met de cijfers en deze gebruiken om erachter te komen wat de oorzaak van het lagere percentage slagingspercentage. Zullen we speciaal op fouten in het bereik van 400. Vervolgens gaat nauwer gaan we 404 fouten (niet gevonden).

### <a name="some-causes-of-400-range-errors"></a>Enkele oorzaken van fouten 400 bereik

De volgende voorbeelden ziet u een steekproef van enkele fouten 400 bereik voor aanvragen tegen Azure Blob-opslag en hun mogelijke oorzaken. Een van deze fouten, evenals fouten in het bereik 300 en de 500-serie, kan bijdragen aan een laag percentage slagingspercentage. 

Houd er rekening mee dat de volgende lijsten verre van compleet zijn. Zie [Status en foutcodes](http://msdn.microsoft.com/library/azure/dd179382.aspx) voor meer informatie over algemene opslag Azure-fouten en de fouten die specifiek zijn voor elk van de storage-services.

**Voorbeelden van status Code 404 (niet gevonden)**

Doet zich voor wanneer een leesbewerking tegen een container of blob mislukt omdat het blob of de container is niet gevonden.

- Treedt op als een container of blob is verwijderd door een andere client voordat dit verzoek. 
- Treedt op als u een API-aanroep die wordt gemaakt van de container of blob nadat u hebt gecontroleerd of het bestaat. De APIs CreateIfNotExists gebeld hoofd eerst om te controleren op de aanwezigheid van de container of blob; Als deze niet bestaat nog, wordt een 404-fout geretourneerd en vervolgens een tweede PUT aanroep naar de container of blob schrijven.

**Statuscode 409 (Conflict)-voorbeelden**

- Treedt op als een API maken maakt u een nieuwe container of blob, zonder aanwezigheid eerst controleren en een container of blob met die naam al bestaat. 
- Treedt op als een container wordt verwijderd en u probeert een nieuwe container met dezelfde naam maken voordat de verwijderingsbewerking voltooid is.
- Deze gebeurtenis vindt plaats als u een lease-overeenkomst in een container of blob opgeeft en er al een lease aanwezig bestaat.
 
**Statuscode 412 (voorwaarde mislukt) voorbeelden**

- Treedt op wanneer de voorwaarde in een voorwaardelijke kop is niet gehaald.
- Treedt op wanneer de opgegeven lease-ID komt niet overeen met de lease-ID op de container of blob.

## <a name="generate-log-files-for-analysis"></a>Genereren van logboekbestanden voor analyse

In deze zelfstudie zullen we bericht Analyzer gebruiken om te werken met drie verschillende typen logboekbestanden, maar kunt u werken met een van deze:

- Het **serverlogboek**, dat wordt gemaakt wanneer u opslag Azure-logboekregistratie inschakelen. Het serverlogboek bevat gegevens over elke bewerking tegen een van de opslag van Azure services - blob, wachtrij, tabel en bestand genoemd. Het serverlogboek geeft aan welke bewerking is aangeroepen en de statuscode is geretourneerd, evenals andere details over de aanvraag en het antwoord.
- Het **.NET clientlogboek**, dat wordt gemaakt wanneer een client-side logboekregistratie uit te schakelen in uw toepassing .NET. De clientlogboek bevat gedetailleerde informatie over hoe de client de aanvraag wordt voorbereid en ontvangt en verwerkt het antwoord. 
- Het **traceerlogboek voor HTTP-netwerk**, dat gegevens worden verzameld op HTTP/HTTPS-aanvraag en het antwoord gegevens, met inbegrip van bewerkingen op Azure opslag. In deze zelfstudie genereert de netwerk-trace via bericht Analyzer.

### <a name="configure-server-side-logging-and-metrics"></a>Configureren van server-side-logboekregistratie en statistieken

Ten eerste moet we configureren Azure opslag registreren en statistieken, zodat we de gegevens van de clienttoepassing te analyseren. Kunt u logboekregistratie en maatstaven op tal van manieren - via de [Klassieke Portal Azure](https://manage.windowsazure.com)met PowerShell, of via programmacode. Zie [opslag Metrics inschakelen en metrische gegevens bekijken](http://msdn.microsoft.com/library/azure/dn782843.aspx) en [opslag vastleggen inschakelen en toegang tot gegevens](http://msdn.microsoft.com/library/azure/dn782840.aspx) voor meer informatie over het configureren van Logboeken en statistieken.

**Via de klassieke Azure Portal**

Volg de instructies op de [Monitor een account voor opslag in de Portal Azure](storage-monitor-storage-account.md)configureren logboekregistratie en statistieken voor uw opslag-account met behulp van de portal.

> [AZURE.NOTE] Het is niet minuut maatstaven met de klassieke Portal Azure instellen. We raden echter aan dat u hen instelt voor de toepassing van deze zelfstudie, en voor het onderzoeken van prestatieproblemen met uw toepassing. Minuut metrics met PowerShell zoals hieronder of via programmacode of via de klassieke Azure-Portal kunt u instellen.
>
> Houd er rekening mee dat de klassieke Portal Azure minuut metrics, alleen per uur metrics kan niet worden weergegeven. 

**Via PowerShell**

Zie aan de slag met PowerShell voor Azure [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

1. Gebruik de cmdlet [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) uw Azure gebruikersaccount toevoegen aan de PowerShell-venster:

    ```
    Add-AzureAccount
    ```

2. Typ het e-mailadres en wachtwoord voor uw account in het venster **aanmelden bij Microsoft Azure** . Azure verifieert de referentie-informatie wordt opgeslagen en vervolgens wordt het venster gesloten.
3. De standaardaccount voor opslag naar de opslag account die u voor de zelfstudie door het uitvoeren van deze opdrachten in het venster PowerShell instellen:

    ```
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount' 
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName 
    ```

4. Logboekregistratie voor de service Blob opslag inschakelen: 
 
    ```
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0 
    ```
5. Inschakelen opslag maatstaven voor de Blob-service ervoor te zorgen **- MetricsType** instellen op `Minute`:

    ```
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0 
    ```

### <a name="configure-net-client-side-logging"></a>.NET client-side-logboekregistratie configureren

Configureren van logboekregistratie voor een .NET-toepassing aan clientzijde inschakelen .NET diagnostische gegevens in het configuratiebestand van de toepassing (web.config of app.config). [Client-side logboekregistratie met de bibliotheek .NET opslag Client](http://msdn.microsoft.com/library/azure/dn782839.aspx) en [Client-side vastleggen met de Microsoft Azure opslag SDK voor Java](http://msdn.microsoft.com/library/azure/dn782844.aspx) Zie voor meer informatie.

Het client-side logboek bevat gedetailleerde informatie over hoe de client de aanvraag wordt voorbereid en ontvangt en verwerkt het antwoord.

Client-bibliotheek van de opslag worden client-side-logboekgegevens opgeslagen op de locatie die is opgegeven in het configuratiebestand van de toepassing (web.config of app.config). 

### <a name="collect-a-network-trace"></a>Een netwerk-trace verzamelen

Bericht Analyzer kunt u een netwerk-trace HTTP/HTTPS verzamelen terwijl de clienttoepassing wordt uitgevoerd. [Fiddler](http://www.telerik.com/fiddler) wordt bericht Analyzer op de achtergrond. Voordat u de netwerk-trace verzamelt, raden we aan dat u Fiddler om vast te leggen, niet-gecodeerde HTTPS-verkeer configureren:

1. [Fiddler](http://www.telerik.com/download/fiddler)installeren.
2. Fiddler starten.
2. Selecteer extra **| Fiddler opties**.
3. In het dialoogvenster Opties, ervoor te zorgen dat **HTTPS CONNECTs vastleggen** en **HTTPS-verkeer decoderen** beide zijn geselecteerd, zoals hieronder wordt weergegeven.

![Fiddler-opties configureren](./media/storage-e2e-troubleshooting-classic-portal/fiddler-options-1.png)

Voor de zelfstudie, verzamelen, een netwerk-trace eerst opslaan in een bericht Analyzer en vervolgens een sessie analyse om de logboeken voor het traceren en analyseren. Voor het verzamelen van een netwerk-trace in bericht Analyzer:

1. In Analyzer bericht, selecteert u bestand **| Snel traceren | Niet-HTTPS versleuteld**.
2. De tracering wordt onmiddellijk gestart. Selecteer **Stoppen met** het traceren stoppen zodat we het trace opslag uitsluitend voor verkeer kunt configureren.
3. Selecteer **bewerken** de traceringssessie bewerken.
4. Selecteer de koppeling **Configure** rechts van de **Microsoft-Pef-WebProxy** ETW-provider.
5. Klik in het dialoogvenster **Geavanceerde instellingen** op het tabblad **Provider** .
6. Geef in het veld **Hostname Filter** opslag-eindpunten, gescheiden door spaties. Bijvoorbeeld, kunt u de eindpunten als volgt; wijzigen `storagesample` op de naam van uw account opslag:
    
    ``` 
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net 
    ```

7. Sluit het dialoogvenster en klikt u op **opnieuw starten** om te beginnen met het verzamelen van de curve met het filter hostname in plaats zodat alleen opslag Azure netwerkverkeer wordt opgenomen in het traceerlogboek.

>[AZURE.NOTE] Nadat u klaar bent met het verzamelen van uw netwerk-trace wordt aangeraden dat u herstelt de instellingen die u hebt gewijzigd in Fiddler te decoderen HTTPS-verkeer. Schakel de selectievakjes **HTTPS CONNECTs vastleggen** en **Decoderen van HTTPS-verkeer** in het dialoogvenster Opties voor Fiddler.

Zie [De functies netwerk traceren](http://technet.microsoft.com/library/jj674819.aspx) op Technet voor meer informatie.

## <a name="review-metrics-data-in-the-azure-classic-portal"></a>Metrische gegevens in de klassieke Portal Azure bekijken

Als de toepassing al actief voor een periode van tijd, kunt u de grafieken metrische gegevens die worden weergegeven in de klassieke Azure-Portal te zien hoe de service heeft is het controleren. Eerst gaan we de metric **Succespercentage** toevoegen aan de pagina controle:

1. Ga naar het Dashboard voor uw account voor opslag in de [Klassieke Azure-Portal](https://manage.windowsazure.com)en selecteer vervolgens **Monitor** de controle pagina weer te geven.
2. Klik op **Parameters toevoegen** om het dialoogvenster **Kies metrische gegevens** weer te geven.
3. Blader naar de groep **Succespercentage** zoeken vouwen, selecteert u vervolgens **statistische**, zoals in de volgende afbeelding wordt weergegeven. Deze metriek aggregeert succes percentagegegevens van alle Blob-bewerkingen.

![Kies Metrics](./media/storage-e2e-troubleshooting-classic-portal/choose-metrics-portal-1.png)

In de klassieke Portal Azure ziet u **Succespercentage** in de grafiek controle, samen met andere metrische gegevens u hebt toegevoegd (maximaal zes kan worden weergegeven in de grafiek in één keer). In de volgende afbeelding ziet u het percentage voltooid percentage is enigszins onder de 100%, het scenario dat we vervolgens onderzoeken is door een analyse van de logboekbestanden in bericht Analyzer:

![Metrics grafiek in de portal](./media/storage-e2e-troubleshooting-classic-portal/portal-metrics-chart-1.png)

Zie voor meer informatie over parameters toe te voegen aan de pagina Monitoring, [hoe: parameters toevoegen aan de tabel parameters](storage-monitor-storage-account.md#how-to-add-metrics-to-the-metrics-table).

> [AZURE.NOTE] Het duurt enige tijd voor de metrische gegevens worden weergegeven in de klassieke Portal Azure nadat u metrics opslag inschakelen. Dit komt omdat elk uur statistieken voor het vorige uur niet in de klassieke Portal Azure weergegeven worden totdat het huidige uur is verstreken. Minuut metrics worden ook niet weergegeven in de klassieke Azure-Portal. Dus afhankelijk van wanneer u statistieken inschakelt, duurt het twee uur om metrische gegevens te bekijken.

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>AzCopy-serverlogboeken kopiëren naar een lokale map gebruiken

Azure Storage server logboekgegevens naar worden geschreven BLOB's, terwijl de metrische gegevens worden geschreven naar tabellen. Logboek BLOB's beschikbaar zijn in de bekende `$logs` container voor uw opslag-account. Logboek BLOB's worden hiërarchisch worden benoemd met jaar, maand, dag en uur, zodat u het bereik van de tijd die u wilt onderzoeken gemakkelijk kunt vinden. Bijvoorbeeld, in de `storagesample` , de container voor het logboek blobs voor 01/02/2015 van 8 en 9 uur, is `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. De afzonderlijke BLOB's in deze container opeenvolgende naam hebben, beginnen met `000000.log`.

U kunt het opdrachtregelprogramma AzCopy deze server-side-logboekbestanden naar een locatie van uw keuze op de lokale computer downloaden. Bijvoorbeeld, kunt u de volgende opdracht voor het downloaden van de logboekbestanden voor blob transacties die op 2 januari 2015 naar de map plaatsvonden `C:\Temp\Logs\Server`; vervangen `<storageaccountname>` met de naam van uw account opslag en `<storageaccountkey>` met uw account toegang tot sleutel:

    AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V

AzCopy is beschikbaar voor download op de pagina [Downloads Azure](https://azure.microsoft.com/downloads/) . Zie de [overdracht van gegevens met het hulpprogramma AzCopy voor Command-Line](storage-use-azcopy.md)voor meer informatie over het gebruik van AzCopy.

Zie voor meer informatie over het downloaden aan de serverzijde logboeken [logboekgegevens downloaden opslag registreren](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata). 

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Microsoft Message Analyzer gebruiken voor het analyseren van gegevens

Microsoft Message Analyzer is een hulpprogramma voor het vastleggen, weergeven en analyseren van verkeer, gebeurtenissen en andere systeem- of berichten in scenario's voor probleemoplossing en diagnostiek messaging protocol. Bericht Analyzer kunt u laden, verzamelen en analyseren van gegevens uit het logboek ook en traceringsbestanden opgeslagen. Zie voor meer informatie over bericht Analyzer [Bericht Analyzer operationele handleiding](http://technet.microsoft.com/library/jj649776.aspx).

Activa bevat Analyzer bericht voor Azure opslag die u helpen bij het analyseren van de server, client en logs netwerk. We bespreken hoe het probleem van lage percentage voltooid in de logboeken voor opslag met deze hulpprogramma's in deze sectie.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>Bericht Analyzer en de opslagmiddelen Azure downloaden en installeren

1. [Bericht Analyzer](http://www.microsoft.com/download/details.aspx?id=44226) downloaden vanaf het Microsoft Download Center en het installatieprogramma uitvoert.
2. Bericht Analyzer starten.
3. Selecteer **Asset Manager**in het menu **Extra** . Selecteer **Downloads**in het dialoogvenster **Beheer van activa** , en filteren op **Azure opslag**. U ziet de opslagmiddelen Azure zoals in de volgende afbeelding wordt weergegeven.
4. Klik op **Alle weergegeven Items synchroniseren** als u wilt installeren van de opslagmiddelen Azure. De beschikbare elementen omvatten:
    - **Regels voor azure opslag kleur:** Azure opslag kleur regels kunt u speciale filters die kleur, tekst- en tekenstijlen gebruiken om te markeren van berichten die specifieke informatie in een trace bevatten definiëren.
    - **Opslag azure grafieken:** Azure opslag grafieken zijn vooraf gedefinieerde diagrammen die server log-gegevens in een grafiek. Opmerking voor het gebruik van grafieken Azure opslag op dit moment, kunnen alleen laden als het serverlogboek in het raster voor de analyse.
    - **-Parsers azure opslag:** De opslag van Azure parsers ontleden de Azure opslag client-, server- en HTTP-Logboeken om weer te geven in het raster voor de analyse.
    - **Filters azure opslag:** Azure opslag filters zijn vooraf gedefinieerde criteria voldoen, kunt u de gegevens in het raster analyse zoeken.
    - **Indelingen azure opslag:** Azure opslag weergave-indelingen zijn vooraf gedefinieerde kolomindelingen en groeperingen in het raster voor de analyse.
4. Bericht Analyzer starten nadat u de activa hebt geïnstalleerd.

![Bericht Analyzer Asset Manager](./media/storage-e2e-troubleshooting-classic-portal/mma-start-page-1.png)

> [AZURE.NOTE] Installeer alle activa Azure opslag weergegeven voor de toepassing van deze zelfstudie.

### <a name="import-your-log-files-into-message-analyzer"></a>Uw logboekbestanden importeren in bericht Analyzer

U kunt al uw opgeslagen logboek (server-side, client-side en netwerk) importeren in één sessie van Microsoft Message Analyzer voor analyse.

1. In het menu **bestand** van Microsoft Message Analyzer, klikt u op **Nieuwe sessie**en klik vervolgens op **Lege sessie**. Voer een naam voor uw sessie analyse in het dialoogvenster **Nieuwe sessie** . Klik op de knop **bestanden** in het deelvenster **Details van de sessie** . 
1. De netwerk-trace om gegevens te laden door Analyzer bericht gegenereerd, klikt u op **Bestanden toevoegen**, Ga naar de locatie waar u het bestand .matp in uw web traceringssessie opgeslagen selecteert u het bestand .matp en klik op **openen**. 
1. De server-side om logboekgegevens te laden, klikt u op **Bestanden toevoegen**, blader naar de locatie waar u de logboeken serverside gedownload selecteert u de logboekbestanden voor de periode die u wilt analyseren en klik op **openen**. Vervolgens in het deelvenster **Details van sessie** stelt de **Logboekconfiguratie tekst** vervolgkeuzelijst voor elk logboekbestand aan de serverzijde op **AzureStorageLog** om ervoor te zorgen Microsoft Message Analyzer het logboekbestand kunt parseren correct.
1. Het client-side om logboekgegevens te laden, klikt u op **Bestanden toevoegen**, blader naar de locatie waar u de logboekbestanden van client-side opgeslagen, selecteert u de logboekbestanden die u wilt analyseren en klik op **openen**. Vervolgens in het deelvenster **Details van sessie** stelt de **Logboekconfiguratie tekst** vervolgkeuzelijst voor elke client-side-logboekbestand op **AzureStorageClientDotNetV4** om ervoor te zorgen Microsoft Message Analyzer het logboekbestand kunt parseren correct.
1. Klik op **starten** in het dialoogvenster **Nieuwe sessie** te laden en parseren van gegevens. De logboekgegevens worden weergegeven in het raster weergegeven Analyzer analyse.

De volgende afbeelding ziet u een voorbeeld sessie met de server, client en traceerlogboekbestanden netwerk geconfigureerd.

![Configureer Message Analyzer-sessie](./media/storage-e2e-troubleshooting-classic-portal/configure-mma-session-1.png)

Houd er rekening mee dat bericht Analyzer logboekbestanden in het geheugen laadt. Als u een grote verzameling van gegevens, wilt u filteren om de beste prestaties uit bericht Analyzer.

Eerst bepalen de periode waarin u geïnteresseerd bent in het controleren en deze periode zo klein mogelijk te houden. In veel gevallen zult u een periode van minuten of uren bij de meeste bekijken. Importeer de kleinste set van logboeken die aan uw behoeften kan voldoen.

Als u nog steeds een grote hoeveelheid gegevens hebt, kan aan een sessie datumfilter voor uw gegevens voordat u deze laden u wilt. In het vak **Sessie Filter** , klik op de knop **bibliotheek** kiezen van een vooraf gedefinieerd filter; bijvoorbeeld voor kiezen **algemene tijd Filter I** uit de opslag van Azure filters filter op een tijdsinterval. Vervolgens kunt u de filtercriteria opgeven voor het eerste en laatste tijdstempel voor het interval dat u wilt weergeven. U kunt ook filteren op een bepaalde statuscode; u kunt bijvoorbeeld alleen vermeldingen in het logboek waarin de statuscode 404 is geladen.

Zie voor meer informatie over het importeren van gegevens in Microsoft Message Analyzer [Berichtgegevens ophalen](http://technet.microsoft.com/library/dn772437.aspx) op TechNet.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>De aanvraag-ID van client gebruiken om te correleren Logbestandsgegevens

De bibliotheek Azure opslag Client genereert automatisch een aanvraag voor unieke client-ID voor elke aanvraag. Deze waarde wordt geschreven naar het clientlogboek, logboek van de server en netwerk-trace zodat u kunt het relateren van gegevens over alle drie logboeken in bericht Analyzer. Zie de [aanvraag-ID van Client](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) voor meer informatie over de aanvraag van de client-id.

In de volgende secties wordt beschreven hoe u met behulp van vooraf geconfigureerde en aangepaste lay-outweergaven correleren en gegevens groeperen op basis van de aanvraag-id van client

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Selecteer een lay-out weer te geven in het raster analyse

De opslagmiddelen voor bericht Analyzer bevatten Azure opslag weergave-indelingen zijn voorgedefinieerde weergaven die u gebruiken kunt om uw gegevens met nuttige groeperingen en kolommen voor verschillende scenario's weer te geven. U kunt ook aangepaste indelingen maken en deze opslaan voor hergebruik. 

De volgende afbeelding ziet u het menu **Lay-out weergeven** door de **Lay-out** van het lint werkbalk selecteren. De weergave-indelingen voor Azure opslag worden gegroepeerd onder het knooppunt **Azure opslag** in het menu. U kunt zoeken naar `Azure Storage` in het zoekvak kunt u filteren op Azure opslag alleen indelingen bekijken. Ook kunt u de ster naast een lay-out maken het een favoriet en boven aan het menu weer te geven.

![Weergavemenu-indeling](./media/storage-e2e-troubleshooting-classic-portal/view-layout-menu.png)

Selecteer allereerst **gegroepeerd per Module en ClientRequestID**. Deze weergave-indeling groepen vastleggen gegevens van alle drie logboeken eerst op aanvraag-ID van client, vervolgens op de bron-logboekbestand (of van de **Module** Message Analyzer). Met deze weergave kunt u inzoomen op een bepaalde client aanvraag-ID en gegevens uit alle drie logboekbestanden voor die clientaanvraag-id.

De afbeelding hieronder ziet u deze weergave toegepast op de logboekgegevens steekproef een subset van kolommen weergegeven. U ziet dat voor een bepaalde client aanvraag-ID, de analyse raster worden weergegeven gegevens uit het clientlogboek, serverlogboek en netwerk-trace.

![Lay-out Azure opslag weergeven](./media/storage-e2e-troubleshooting-classic-portal/view-layout-client-request-id-module.png)

>[AZURE.NOTE] Verschillende logboekbestanden hebben verschillende kolommen, zodat wanneer gegevens uit meerdere logboekbestanden wordt weergegeven in het raster analyse, sommige kolommen kunnen geen gegevens voor een bepaalde rij. Bijvoorbeeld in de afbeelding weergeven de client aanmelden rijen niet gegevens voor het **tijdstempel**, **TimeElapsed**, **bron**en **doel** -kolommen omdat deze kolommen bestaan niet in het clientlogboek, maar in de netwerk-trace. Ook de kolom **tijdstempel** timestamp-gegevens uit het logboek van de server wordt weergegeven, maar worden geen gegevens weergegeven voor de kolommen **TimeElapsed**, **bron**en **bestemming** , die geen deel uitmaken van het logboek van de server. 

De Azure opslag weergave-indelingen gebruiken, kunt u ook definiëren en uw eigen indelingen opslaan. U kunt de andere gewenste velden voor het groeperen van gegevens selecteren en de groepering op te slaan als onderdeel van uw aangepaste lay-out ook. 

### <a name="apply-color-rules-to-the-analysis-grid"></a>Kleur regels toepassen op het raster analyse

De opslagmiddelen ook kleur regels dat een visuele betekent bieden voor verschillende soorten fouten in het raster voor de analyse. De kleur van vooraf gedefinieerde regels van toepassing op HTTP-fouten, zodat ze alleen voor het traceren van logboek- en server worden weergegeven.

Selecteren om toe te passen regels voor kleur, **Kleur regels** van het lint van de werkbalk. Hier ziet u de regels van de opslag van Azure kleur in het menu. Selecteer voor de zelfstudie, **Client-fouten (StatusCode tussen 400 en 499)**, zoals in de volgende afbeelding wordt weergegeven.

![Lay-out Azure opslag weergeven](./media/storage-e2e-troubleshooting-classic-portal/color-rules-menu.png)

De regels van de kleur Azure opslag gebruiken, ook definiëren en opslaan van uw eigen regels van kleur.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>Groeperen en filteren Logboekgegevens 400 bereik fouten vinden

Vervolgens we groeperen en filteren van gegevens naar alle fouten in het bereik van 400.

1. Zoek de **StatusCode** -kolom in het raster analyse, met de rechtermuisknop op de kolomkop en selecteer de **groep**.
2. Volgende groep in de kolom **ClientRequestId** . Ziet u de gegevens in het raster analyse is nu georganiseerd door de statuscode en client aanvraag-ID.
1. Het gereedschap venster weergeven als deze nog niet wordt weergegeven. Selecteer **Hulpprogramma Windows**, vervolgens **Filter weergeven**op de werkbalk van het lint.
2. Het om logboekgegevens te filteren om alleen 400 bereik fouten weer te geven, de volgende filtercriteria toevoegen aan **het venster** en klik op **toepassen**:

        (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)

De volgende afbeelding ziet u de resultaten van deze groep en filter. Het veld **ClientRequestID** onder de groepering voor statuscode 409 uit te breiden, bijvoorbeeld bevat een bewerking waardoor deze statuscode.

![Lay-out Azure opslag weergeven](./media/storage-e2e-troubleshooting-classic-portal/400-range-errors1.png)

Nadat dit filter is toegepast, ziet u dat de rijen van het clientlogboek worden uitgesloten, als de client logboek bevat geen een **StatusCode** kolom. Allereerst getoetst server en netwerk traceerlogboeken vinden 404-fouten en geven we vervolgens naar het clientlogboek aan het bestuderen van de activiteiten van clients die heeft geleid tot deze.

>[AZURE.NOTE] U kunt filteren op de kolom **StatusCode** en gegevens uit drie logboeken, met inbegrip van het clientlogboek, als u een expressie toevoegen aan de filter dat vermeldingen in het logboek waarin de statuscode null is bevat nog steeds weergegeven. Als u wilt deze filterexpressie samenstellen, gebruikt u:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code> 
>
> Dit filter retourneert alle rijen van de client aanmelden en alleen rijen uit het serverlogboek en HTTP-logboek waar de statuscode groter is dan 400. Als u het toepassen op de lay-out gegroepeerd per module en aanvraag-ID van client, kunt u zoeken of bladeren door de vermeldingen in het logboek te vinden zijn waar alle drie de logboeken worden weergegeven.   

### <a name="filter-log-data-to-find-404-errors"></a>Filtergegevens vinden 404-fouten

De opslagmiddelen zijn vooraf gedefinieerde filters kunt u logboekgegevens om fouten of u naar zoekt trends te beperken. Vervolgens zullen we twee vooraf gedefinieerde filters toepassen: een filter voor de server en het netwerk traceerlogboeken voor 404-fouten, en een filter de gegevens in een opgegeven periode.

1. Het gereedschap venster weergeven als deze nog niet wordt weergegeven. Selecteer **Hulpprogramma Windows**, vervolgens **Filter weergeven**op de werkbalk van het lint.
2. In het venster Filter selecteren de **bibliotheek**en zoek op `Azure Storage` filters voor de opslag van Azure vinden. Selecteer het filter voor **404 (niet gevonden) berichten in alle logboeken**.
3. Weer het **bibliotheek** -menu, en zoek en selecteer het **Filter globale tijd**.
4. Bewerk de tijdstempels die in het filter van het bereik dat u wilt weergeven. Hierdoor kunt u het bereik van de te analyseren gegevens beperken.
5. Het filter moet worden weergegeven zoals in het onderstaande voorbeeld. Klik op **toepassen** om het filter toepassen op het raster analyse.

        ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And 
        (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)

![Lay-out Azure opslag weergeven](./media/storage-e2e-troubleshooting-classic-portal/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>De logboekgegevens analyseren

Nu dat u hebt gegroepeerd en uw gegevens gefilterd, kunt u de details van individuele aanvragen die gegenereerd van 404-fouten kunt onderzoeken. In de huidige lay-out, worden de gegevens gegroepeerd op client aanvraag-ID, klik op logboek bron. Aangezien we op aanvragen waarbij het veld StatusCode 404 bevat filtert, ziet we alleen de server en netwerk-traceringsgegevens, niet de logboekgegevens van de client.

De volgende afbeelding ziet u een specifiek verzoek waar een Blob Get-bewerking een 404 overgedragen omdat de blob niet bestaat. Opmerking Sommige kolommen zijn verwijderd uit de weergave Normaal om de relevante gegevens weer te geven.

![Gefilterde Server en netwerk traceerlogboeken](./media/storage-e2e-troubleshooting-classic-portal/server-filtered-404-error.png)

Vervolgens zullen we deze aanvraag-ID van client correleren met de logboekgegevens van de client om te zien welke acties de client werd genomen als de fout zich voordeed. U kunt een nieuwe analyse raster weergeven voor deze sessie de logboekgegevens client die wordt geopend in een tweede tabblad bekijken weergeven:

1. De waarde van het veld **ClientRequestId** eerst naar het Klembord kopiëren. U kunt dit doen door het selecteren van een rij, zoeken naar het **ClientRequestId** veld met de rechtermuisknop op de waarde en **kopie 'ClientRequestId'**te kiezen. 
1. Op het lint werkbalk **Nieuwe Viewer**selecteren en selecteer vervolgens **Analyse raster** op een nieuw tabblad openen. Het nieuwe tabblad worden alle gegevens in de logboekbestanden, zonder te groeperen, filteren of regels van kleur. 
2. Op het lint werkbalk **Lay-out**selecteren en selecteer vervolgens **Alle .NET Client kolommen** in de sectie **Opslag Azure** . Deze weergave-indeling worden gegevens weergegeven van de client logboekbestand als de server- en traceerlogboeken. Standaard wordt het gesorteerd op de kolom **MessageNumber** .
3. Vervolgens het clientlogboek voor de client aanvraag-id zoeken Selecteer **Berichten zoeken**op de werkbalk van het lint en geeft u een aangepast filter op de client aanvraag-ID in het veld **Zoeken** . Gebruik de volgende syntaxis voor het filter, uw eigen client aanvraag-ID op te geven:

        *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"

Bericht Analyzer zoekt en selecteert de eerste vermelding overeenkomt met de zoekcriteria de aanvraag-id van client In het clientlogboek zijn er verschillende posten voor elke client aanvraag-ID, zodat u groeperen op het veld **ClientRequestId wilt** gemakkelijker te zien elkaar. De volgende afbeelding ziet alle berichten in het clientlogboek voor de opgegeven client aanvraag-ID. 

![Client logboek tonen 404-fouten](./media/storage-e2e-troubleshooting-classic-portal/client-log-analysis-grid1.png)

De gegevens in de weergave-indelingen in deze twee tabbladen kunt u de aanvraaggegevens om te bepalen wat de fout hebben veroorzaakt analyseren. U kunt ook zoeken op aanvragen die dit bestand om te zien als een eerdere gebeurtenis kan hebben geleid tot de fout 404 voorafgegaan. U kunt bijvoorbeeld de client logboekvermeldingen voorafgaand aan dit verzoek client-ID te bepalen of de blob is verwijderd, of als de fout is veroorzaakt door de clienttoepassing een CreateIfNotExists-API aanroept voor een container of blob bekijken. In het clientlogboek vindt u van de blob-adres in het veld **Omschrijving** . Deze informatie wordt weergegeven in het veld **Samenvatting** in de server en het netwerk traceerlogboeken.

Als u eenmaal het adres van de blob die de fout 404 hebben opgeleverd weet, kunt u verder onderzoeken. Als u de logboekvermeldingen voor andere berichten die zijn gekoppeld aan bewerkingen op de dezelfde blob zoekt, kunt u controleren of de entiteit die door de client eerder verwijderd. 

## <a name="analyze-other-types-of-storage-errors"></a>Andere soorten opslag fouten analyseren

Nu dat u bekend bent met bericht Analyzer gebruiken voor het analyseren van uw gegevens, kunt u analyseren andere typen fouten met weergave-indelingen, regels voor kleur en zoeken/filteren. De onderstaande tabellen worden enkele problemen beschreven die kunnen optreden en de filtercriteria kunt u ze kunt vinden. Zie voor meer informatie over het maken van filters en het bericht Analyzer filtering taal [Berichtgegevens filteren](http://technet.microsoft.com/library/jj819365.aspx).

|    Te onderzoeken...                                                                                               |    Gebruik de filterexpressie...                                                                                                                                                                                                                                        |    Toepassing van de Log (Client, Server, netwerk, alle)    |
|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
|    Onverwachte vertragingen in de bezorging van berichten in een wachtrij                                                              |    AzureStorageClientDotNetV4.Description bevat "Bezig met opnieuw proberen mislukt de bewerking."                                                                                                                                                                                |    Client                                                        |
|    HTTP-stijging van de PercentThrottlingError                                                                       |    HTTP. Response.StatusCode == 500 & #124; & #124; HTTP. Response.StatusCode == 503                                                                                                                                                                                          |    Netwerk                                                       |
|    Toename van het PercentTimeoutError                                                                               |    HTTP. Response.StatusCode == 500                                                                                                                                                                                                                             |    Netwerk                                                       |
|    Toename van het PercentTimeoutError (alle)                                                                         |    * De StatusCode 500 ==                                                                                                                                                                                                                                          |    Alle                                                           |
|    Toename van het PercentNetworkError                                                                               |    AzureStorageClientDotNetV4.EventLogEntry.Level < 2                                                                                                                                                                                                          |    Client                                                        |
|    HTTP-fout 403 (verboden) berichten                                                                                 |    HTTP. Response.StatusCode == 403                                                                                                                                                                                                                             |    Netwerk                                                       |
|    HTTP-fout 404 (niet gevonden) berichten                                                                                 |    HTTP. Response.StatusCode == 404                                                                                                                                                                                                                             |    Netwerk                                                       |
|    404 (alle)                                                                                                     |    *-StatusCode 404 ==                                                                                                                                                                                                                                          |    Alle                                                           |
|    Gedeelde toegang handtekening (SAS) vergunning probleem                                                             |    AzureStorageLog.RequestStatus == "SASAuthorizationError"                                                                                                                                                                                                     |    Netwerk                                                       |
|    HTTP 409 (Conflict) berichten                                                                                  |    HTTP. Response.StatusCode == 409                                                                                                                                                                                                                             |    Netwerk                                                       |
|    409 (alle)                                                                                                     |    * StatusCode == 409                                                                                                                                                                                                                                          |    Alle                                                           |
|    Lage PercentSuccess of analytics logitems zijn transacties met de status van ClientOtherErrors    |    AzureStorageLog.RequestStatus == "ClientOtherError"                                                                                                                                                                                                         |    Server                                                        |
|    Waarschuwing Nagle                                                                                               |    ((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1,5)) en (AzureStorageLog.RequestPacketSize < 1460) en (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200)        |    Server                                                        |
|    Tijdsbereik in Logboeken van Server- en netwerkbeheer                                                                    |    #Tijdstempel > = 2014-10-20T16:36:38 en #Timestamp < = 2014-10-20T16:36:39                                                                                                                                                                                     |    Server, netwerk                                               |
|    Tijdsbereik in Logboeken van Server                                                                                |    AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 en AzureStorageLog.Timestamp < = 2014-10-20T16:36:39                                                                                                                                                     |    Server                                                        |


## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over het oplossen van problemen end-to-end scenario's in Azure opslag:

- [Controleren, opsporen en oplossen van problemen met Microsoft Azure opslag](storage-monitoring-diagnosing-troubleshooting.md)
- [Analytics voor opslag](http://msdn.microsoft.com/library/azure/hh343270.aspx)
- [Een account voor opslag in de Portal Azure controleren](storage-monitor-storage-account.md)
- [Gegevens overbrengen met het opdrachtregelprogramma AzCopy](storage-use-azcopy.md)
- [Microsoft Message Analyzer operationele handleiding](http://technet.microsoft.com/library/jj649776.aspx)
 
 
