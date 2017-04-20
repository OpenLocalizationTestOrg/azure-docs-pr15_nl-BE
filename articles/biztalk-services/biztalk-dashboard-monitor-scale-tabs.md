<properties 
    pageTitle="Dashboard, Monitor, schaal, configureren, en hybride verbindingen in BizTalk-Services | Microsoft Azure" 
    description="Meer informatie over de besturingselementen en de prestaties op de tabbladen van de portal voor BizTalk-Services controleren: Dashboard, Monitor, schaal, configureren en hybride verbindingen. MAB, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="mandia"/>




# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>De tabbladen Dashboard, Monitor, schaal, configureren en hybride verbinding controleren

Nadat u de BizTalk-Service maken en implementeren van uw toepassing, kunt u enkele van de BizTalk-Service-instellingen wijzigen en de prestaties van toepassingen te controleren. 

Als u de klassieke Azure portal opent, wordt u automatisch geplaatst op het tabblad **Alle ITEMS** . De BizTalk-Service weergeven, selecteer uw BizTalk-Service op het tabblad **Alle ITEMS** of klik op het tabblad **BizTalk-SERVICES** ; en selecteer vervolgens de naam van uw BizTalk-Service.

Hiermee opent u een nieuw venster met de volgende tabbladen. In dit onderwerp worden deze tabbladen beschreven.

## <a name="quick-start-quick-startquickstart"></a>Quick Start)![Snel starten][QuickStart])
Afhankelijk van de editie BizTalk-Services alle vermelde opties mogelijk niet beschikbaar. 
<table border="1">
    <tr>
        <td><strong>De hulpprogramma's ophalen</strong></td>
        <td>Download de SDK van BizTalk-Services om de Visual Studio-projectsjablonen installeren op de computer van de ontwikkeling van gebouwen. Deze sjablonen maken op de <strong>BizTalk-Services</strong> (bridge) en de <strong>BizTalk-serviceartefacten</strong> (transformatie) Visual Studio-projecten die zijn geïmplementeerd met uw BizTalk-Service.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Hoe kan ik starten via de Azure BizTalk-Services SDK</a> en het <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">installeren van de SDK van Azure BizTalk-Services</a> vindt u de stappen aan de slag.
        </td>
    </tr>
    <tr>
        <td><strong>Partner-overeenkomsten maken</strong></td>
        <td>Hiermee opent u de Portal Azure BizTalk-Services die worden gehost op Azure waarbij partners toevoegen en X12, AS2, maken en EDIFACT EDI-overeenkomsten.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Onderdelen configureren voor EDI berichten op de BizTalk-Services Portal</a> vindt u de stappen aan de slag.
        </td>
    </tr>

<tr>
        <td><strong>Meer informatie over BizTalk-Services</strong></td>
        <td>Ga naar het <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> voor meer informatie over Azure BizTalk-Services.</td>
</tr>
</table>


U kunt in de taakbalk onder:

<table border="1">

<tr>
<td><strong>Beheren van</strong> de implementatie van toepassingen</td>
<td>Hiermee opent u de portal Azure BizTalk-Services. De BizTalk-Services Portal is de toegang tot de configuratie van EDI, zoals partners toe te voegen en het maken van X12, AS2, en EDIFACT overeenkomsten.
<br/><br/>
Dit is hetzelfde als <strong>de partnerovereenkomst maken</strong> op het tabblad <strong>Quick Start</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Onderdelen configureren voor EDI berichten op de BizTalk-Services Portal</a> vindt u meer informatie op de Portal van BizTalk-Services.</td>
</tr>

<tr>
<td><strong>Gegevens van</strong> van het Access-besturingselement Namespace</td>
<td>Wanneer u gegevens selecteert, vervolgens de Access Control Namespace, standaard uitgever en standaard sleutel weergegeven. U kunt deze waarden kopiëren.
<br/><br/>
U kunt ook de toegang tot het Portal openen. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Een Namespace voor toegangsbeheer maken</a> vindt u meer informatie over de toegang tot het Portal.</td>
</tr>

<tr>
<td><strong>Sleutels synchroniseren</strong> in de opslag</td>
<td>Wanneer u een opslag-account maakt, worden automatisch een primaire en secundaire sleutel gemaakt. Deze sleutels voor gegevenscodering beheren toegang tot uw Account opslag. De BizTalk-Service gebruikt automatisch de primaire sleutel. <strong>Sync-sleutels</strong> kunnen gebruikers schakelen tussen de primaire en de secundaire sleutel zonder de BizTalk-Service te onderbreken.
<br/><br/>
U wilt dat de BizTalk-Service een nieuwe primaire sleutel gebruikt voor de opslag. Om dit te doen:
<br/><br/>
<ol>
<li>Selecteer uw BizTalk-Service en schakel <strong>Sync sleutels</strong>. Selecteer de secundaire sleutel. Wanneer u dit doet, wordt de BizTalk-Service gestart met de secundaire sleutel.</li>
<li>In de klassieke Azure portal, selecteert u de account voor de opslag en de primaire sleutel opnieuw genereren. Vergeet niet uw BizTalk-Service met behulp van de secundaire sleutel.</li>
<li>Selecteer uw BizTalk-Service en schakel <strong>Sync sleutels</strong>. Selecteer nu de primaire sleutel. Dit is de nieuwe primaire sleutel die u opnieuw gegenereerd.</li>
<li>In de klassieke Azure portal, selecteert u de account voor de opslag en de secundaire sleutel opnieuw genereren.</li>
</ol>
<br/>
Dit proces wordt 'rollover toetsen' genoemd. Het doel is om gebruikers te schakelen tussen de primaire en de secundaire sleutel zonder de BizTalk-Service te onderbreken.</td>
</tr>

<tr>
<td><strong>Verwijderen</strong> uw toepassing</td>
<td>Wanneer u verwijderen, uw BizTalk-Service en worden geïmplementeerd op deze alle gegevens verwijderd.</td>
</tr>
</table>


## <a name="dashboard"></a>Dashboard
Afhankelijk van de editie BizTalk-Services alle vermelde opties mogelijk niet beschikbaar. 

Als u de naam van uw BizTalk-Service selecteert, wordt het tabblad Dashboard wordt weergegeven. In Dashboard, kunt u het volgende doen:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Overzicht van gebruik: Geeft het aantal gebruikte hybride verbindingen
Ook weergegeven het gebruik van de gegevens in GB. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Metrische grafiek: Ziet u een vaste lijst van prestatiegegevens
Deze gegevens opgeven real-time waarden met betrekking tot de gezondheid van de BizTalk-Service. U kunt ook de **relatieve** of **Absolute** waarden en het periode- **Interval** van de parameters die worden weergegeven in de grafiek. 

Ga naar [Beschikbare maatstelsel](#Metrics) in dit onderwerp voor een beschrijving van deze prestatiegegevens.


##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Overzicht: Hier ziet u de eigenschappen van uw BizTalk-Service

<table border="1">

<tr>
<td><strong>Referenties voor het bijhouden van Database bijwerken</strong></td>
<td>Hiermee wijzigt u de gebruikersnaam en het wachtwoord waarmee u zich aanmelden bij de Database bijhouden.</td>
</tr>
<tr>
<td><strong>SSL-certificaat bijwerken</strong></td>
<td>De BizTalk-Service voor het gebruik van een ander SSL-certificaat kan worden bijgewerkt. Een zelfondertekend certificaat voor SSL wordt automatisch gemaakt wanneer u <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">de BizTalk-Service maken</a>.</td>
</tr>
<tr>
<td><strong>Certificaat downloaden</strong></td>
<td>U kunt de SSL-certificaat gebruikt door de BizTalk-Service op een lokale computer downloaden.</td>
</tr>
<tr>
<td><strong>Status</strong></td>
<td>Geeft de huidige status van uw BizTalk Service. Zie <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk-Services: Service staat grafiek</a>. </td>
</tr>
<tr>
<td><strong>De URL</strong></td>
<td>De URL voor uw BizTalk-Service. Dit is hetzelfde als het <strong>Domein URL</strong> ingevoerd wanneer uw BizTalk-Service wordt gemaakt.</td>
</tr>
<tr>
<td><strong>Openbare (VIP) in de virtuele IP-adres</strong></td>
<td>Het IP-adres toegewezen aan de BizTalk-Service. Het wordt gebruikt voor alle invoer eindpunten en het bronadres voor uitgaand verkeer. Dit IP-adres hoort bij uw BizTalk-Service wanneer deze wordt gemaakt. Als u de BizTalk-Service verwijdert, wordt het IP-adres is toegewezen aan een andere BizTalk-Service.</td>
</tr>
<tr>
<td><strong>ACS Namespace</strong></td>
<td>Met de BizTalk-Service verifieert.</td>
</tr>
<tr>
<td><strong>Edition</strong></td>
<td>Lijsten de uitgave ingevoerd als de BizTalk-Service wordt gemaakt.</td>
</tr>
<tr>
<td><strong>Locatie</strong></td>
<td>Het geografische gebied waarop de BizTalk-Service weergegeven.</td>
</tr>
<tr>
<td><strong>Gemaakt</strong></td>
<td>Geeft de datum en het tijdstip waarop die de BizTalk-Service is gemaakt.</td>
</tr>
<tr>
<td><strong>Database voor bijhouden</strong></td>
<td>De naam Azure SQL-Database waarin de tabellen bijhouden is gebruikt door de BizTalk-Service. 
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Vereisten uiteengezet</a> worden details weergegeven over het bijhouden van Database.</td>
</tr>
<tr>
<td><strong>Bewaking/archiveren van opslag</strong></td>
<td>De naam van de opslag van Azure waarin de uitvoer controle van uw BizTalk-Service.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Vereisten uiteengezet</a> worden details weergegeven over de opslag-account.</td>
</tr>
<tr>
<td><strong>De naam van abonnement</strong></td>
<td>Hier worden het abonnement waarop de BizTalk-Service. Het abonnement regelt de toegang tot de klassieke Azure portal.</td>
</tr>
<tr>
<td><strong>Abonnement-ID</strong></td>
<td>Wanneer een abonnement wordt gemaakt, wordt automatisch een abonnement-ID gegenereerd. Wanneer u REST API's gebruikt, moet u wellicht de abonnement-id invoeren.</td>
</tr>
</table>

[BizTalk-Services: inrichten met behulp van Azure klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) geeft een overzicht van de stappen voor het maken van een BizTalk-Service.


##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Verbindingsgegevens, Sync-sleutels beheren en verwijderen op de taakbalk:

<table border="1">

<tr>
<td><strong>Beheren van</strong> de implementatie van toepassingen</td>
<td>Hiermee opent u de Portal Azure BizTalk-Services. De BizTalk-Services Portal is de toegang tot de configuratie van EDI, zoals partners toe te voegen en het maken van X12, AS2, en EDIFACT overeenkomsten.
<br/><br/>
Dit is hetzelfde als <strong>de partnerovereenkomst maken</strong> op het tabblad <strong>Quick Start</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Onderdelen configureren voor EDI berichten op de BizTalk-Services Portal</a> vindt u meer informatie op de Portal van BizTalk-Services.</td>
</tr>
<tr>
<td><strong>Gegevens van</strong> van het Access-besturingselement Namespace</td>
<td>De Access Control Namespace, standaard uitgever en standaard sleutel waarden; die kunnen worden gekopieerd.
<br/><br/>
U kunt ook de toegang tot het Portal openen. Deze toegang tot het Portal is hetzelfde als met de optie Active Directory in het linkernavigatievenster.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Beheer uw ACS Namespace</a> vindt u meer informatie op de toegang tot het Portal.</td>
</tr>
<tr>
<td><strong>Sleutels synchroniseren</strong> in de opslag</td>
<td>Wanneer u een opslag-account maakt, worden automatisch een primaire en secundaire sleutel gemaakt. Deze sleutels voor gegevenscodering beheren toegang tot uw Account opslag. De BizTalk-Service gebruikt automatisch de primaire sleutel. <strong>Sync-sleutels</strong> kunnen gebruikers schakelen tussen de primaire en de secundaire sleutel zonder de BizTalk-Service te onderbreken.
<br/><br/>
U wilt dat de BizTalk-Service een nieuwe primaire sleutel gebruikt voor de opslag. Om dit te doen:
<br/><br/>
<ol>
<li>Selecteer uw BizTalk-Service en schakel <strong>Sync sleutels</strong>. Selecteer de secundaire sleutel. Wanneer u dit doet, wordt de BizTalk-Service gestart met de secundaire sleutel.</li>
<li>In de klassieke Azure portal, selecteert u de account voor de opslag en de primaire sleutel opnieuw genereren. Vergeet niet uw BizTalk-Service met behulp van de secundaire sleutel.</li>
<li>Selecteer uw BizTalk-Service en schakel <strong>Sync sleutels</strong>. Selecteer nu de primaire sleutel. Dit is de nieuwe primaire sleutel die u opnieuw gegenereerd.</li>
<li>In de klassieke Azure portal, selecteert u de account voor de opslag en de secundaire sleutel opnieuw genereren.</li>
</ol>
<br/>
Dit proces wordt 'rollover toetsen' genoemd. Het doel is om gebruikers te schakelen tussen de primaire en de secundaire sleutel zonder de BizTalk-Service te onderbreken.</td>
</tr>

<tr>
<td><strong>Verwijderen</strong> uw toepassing</td>
<td>Uw BizTalk-Service en alle items ingezet te worden verwijderd.</td>
</tr>
</table>


## <a name="monitor"></a>Monitor
Niet van toepassing op de gratis editie.

Als u de naam van uw BizTalk-Service selecteert, wordt het tabblad Monitor is beschikbaar en wordt het volgende weergegeven:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Metrische grafiek: Geeft het geselecteerde prestatiegegevens
Deze gegevens opgeven real-time waarden met betrekking tot de gezondheid van de BizTalk-Service. U kiezen welke prestatiegegevens worden weergegeven. Een maximum van zes prestatiewaarden kan gelijktijdig worden weergegeven. 

U kunt ook de **relatieve** of **Absolute** waarden en het periode- **Interval** van de parameters die worden weergegeven. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Verwijderen of weergeven van statistieken in de grafiek:
1. Selecteer het tabblad **Monitor** .
2. Selecteer de optie **Metrisch toevoegen** in de taakbalk:  
![Selecteer toevoegen parameters][AddMetrics]
3. Controleer de prestatiegegevens die u wilt weergeven.
4. Selecteer het vinkje om terug te keren naar het tabblad **Monitor** .
5. Selecteer de cirkel naast de metric van die metric-waarde in de grafiek weergegeven.  

    Bijvoorbeeld de metric van de **CPU-gebruik** is niet beschikbaar; de uitvoer wordt niet weergegeven in de grafiek:  
![CPU-gebruik metric wordt grijs weergegeven][GrayedMetric]  

    Selecteer het grijs uit de cirkel om de metric van de **CPU-gebruik** om de uitvoer in de grafiek weer te geven:  
![CPU-gebruik metric is ingeschakeld.][EnabledMetric]

6. Als een metric van de grafiek weergeven en de lijst, selecteert **Metric verwijderen** op de taakbalk. Selecteer het vinkje om terug te keren naar het tabblad **Monitor** om de metrische weer toe te voegen aan de lijst, **Metrics toevoegen** in de taakbalk en controleer de metric. Selecteer het grijs van cirkel de metric inschakelen.

## <a name="Metrics"></a>Statistieken beschikbaar
De volgende items/prestatiewaarden zijn beschikbaar:

<table border="1">

<tr>
<td><strong>Latentie van RountdTrip</strong></td>
<td>Geeft de gemiddelde tijd in milliseconden (ms) voor het verwerken van een bericht vanaf het tijdstip waarop dat het bericht is ontvangen, totdat het bericht volledig is verwerkt door de BizTalk-Service op alle bruggen. Alleen berichten met succes zijn verwerkt, worden geteld.<br/><br/>
Een tijdstempel is gemaakt bij de volgende gebeurtenissen:
<ul>
<li>Bericht voert de gateway</li>
<li>Bericht wordt doorgestuurd naar de bestemming</li>
<li>Bestemming antwoord wordt ontvangen.</li>
<li>Bestemming bevestiging antwoord verzonden naar de gateway</li>
</ul>
<br/>
Deze waarde geeft het resultaat van de volgende berekening:
<br/><br/>
[Doel bevestiging antwoord verzonden naar de gateway] - [bericht voert de gateway]</td>
</tr>
<tr>
<td><strong>Fouten bij de bron</strong></td>
<td>Het totale aantal berichten dat niet wordt weergegeven door de BizTalk-Service bij het ophalen van berichten van de eindpunten van de bron.</td>
</tr>
<tr>
<td><strong>CPU-gebruik</strong></td>
<td>Hier worden alle exemplaren van de rol van het gemiddelde percentage processortijd.</td>
</tr>
<tr>
<td><strong>Verwerking van latentie</strong></td>
<td>Geeft de gemiddelde tijd In milliseconden (ms) een bericht verwerkt door de BizTalk-Service op alle bruggen, met uitzondering van de tijd besteed aan bestemmingen. Alleen berichten met succes zijn verwerkt, worden geteld.<br/><br/>
Wanneer elk van de volgende gebeurtenissen plaatsvinden, wordt een tijdstempel gemaakt:

<ul>
<li>Bericht voert de gateway</li>
<li>Bericht wordt doorgestuurd naar de bestemming</li>
<li>Bestemming antwoord wordt ontvangen.</li>
<li>Bestemming bevestiging antwoord verzonden naar de gateway</li>
</ul>
<br/>Deze waarde geeft het resultaat van de volgende berekening:<br/><br/>
[Doel bevestiging antwoord verzonden naar de gateway] - [bericht invoert voor de gateway] - [doel antwoord wordt ontvangen] + [bericht wordt doorgestuurd naar de bestemming]</td>
</tr>
<tr>
<td><strong>Fouten In het proces</strong></td>
<td>Geeft het totale aantal berichten dat is mislukt tijdens de verwerking van de BizTalk-Service op alle bruggen binnen een tijdsinterval.</td>
</tr>
<tr>
<td><strong>Verzonden berichten</strong></td>
<td>Geeft het totale aantal berichten die door de BizTalk-Service via alle bruggen binnen een tijdsinterval. Deze metriek wordt verhoogd wanneer een bericht van een pijpleiding route bestemming bereikt. Deze metriek geeft niet aan dat een bericht wordt verwerkt.<br/><br/>
In een verzoek-antwoord-scenario worden de metric wordt verhoogd wanneer de bestemming van de route een ontvangstbevestiging teruggestuurd naar de pijpleiding wordt.</td>
</tr>
<tr>
<td><strong>Ontvangen berichten</strong></td>
<td>Geeft het totale aantal berichten dat is ontvangen door de BizTalk-Service op alle bruggen binnen een tijdsinterval. Deze waarde wordt verhoogd wanneer een nieuw bericht is ontvangen door de pijplijn.</td>
</tr>
<tr>
<td><strong>Berichten In het proces</strong></td>
<td>Geeft het totale aantal berichten dat momenteel door de BizTalk-Service verwerkt binnen een tijdsinterval.</td>
</tr>
<tr>
<td><strong>Verwerkte berichten</strong></td>
<td>Geeft het totale aantal berichten dat is verwerkt door de BizTalk-Service op alle bruggen binnen een tijdsinterval. Deze waarde wordt verhoogd wanneer een bericht is ontvangen door de pijplijn en goed gerouteerd naar de bestemming.</td>
</tr>
</table>


## <a name="scale"></a>Schaal
U kunt toevoegen of aftrekken van het aantal eenheden die worden gebruikt door de BizTalk-Service op het tabblad schaal. Er is standaard een eenheid zijn geconfigureerd. Aanvullende eenheden kunnen worden toegevoegd voor het schalen van uw BizTalk-Service. Wanneer u de schaal vergroten, meer doorvoer. Het bedrag van de middelen ook, met inbegrip van de geïmplementeerde bruggen, overeenkomsten, LOB-verbindingen, en meer verwerkingskracht. Zo vergroot u de schaal van 1 eenheid tot 2 eenheden. In dit geval kunt u dubbele aantal bruggen implementeren, dubbele van de overeenkomsten, dubbele verbindingen LOB en dubbele de verwerkingskracht.

Sommige BizTalk-edities bieden niet een optie voor schaal. In dit geval is één eenheid toegestaan. Om te bepalen hoeveel eenheden de edition kan worden geschaald, raadpleegt u [BizTalk-Services: edities grafiek](biztalk-editions-feature-chart.md).

Verhoging van het aantal eenheden mogelijk van invloed op prijzen. Als u de eenheden selecteren **Opslaan** wordt weergegeven een bericht dat u als betaling negatieve gevolgen hebben. U kiest om verder te gaan. Als u het aantal eenheden, BizTalk Service verandert de status actief om updates te verhogen. De BizTalk-Service blijft in de status bijwerken uit te voeren.

[BizTalk-Services: edities grafiek](biztalk-editions-feature-chart.md) definieert een "eenheid".


## <a name="configure"></a>Configureren
Niet van toepassing op hybride verbindingen.

De Status van de back-up ingesteld op geen of automatisch. Wanneer ingesteld op geen, worden geen back-ups automatisch gemaakt. Wanneer ingesteld op automatisch, u de back-uplocatie, de frequentie van de back-up, en hoe lang de back-upbestanden bewaren. 

[BizTalk-Services: een back-up en terugzetten van](biztalk-backup-restore.md) vindt u de details. 


## <a name="HybridConnections"></a>Hybride verbindingen
Hybride verbinding wordt een Azure-toepassing, zoals Web Apps of Mobile Apps in Azure App-Service met een bron op de ruimten die gebruikmaakt van een statisch TCP-poort, zoals SQL Server, MySQL, HTTP-Web-API's en de meeste aangepaste Web Services. Hybride verbindingen worden beheerd in BizTalk-Services in de klassieke Azure portal.

Zie [toegang op-premises bronnen met behulp van hybride verbindingen in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md)hybride verbindingen maken in Azure App-Service.

Om te maken of hybride verbindingen in Azure BizTalk-Services beheren Zie [Hybride verbindingen](integration-hybrid-connection-overview.md).



## <a name="next"></a>Volgende
Nu dat u bekend met de verschillende tabbladen bent, kunt u meer informatie over de functies van Azure BizTalk-Services:

- [BizTalk-Services: beperken](biztalk-throttling-thresholds.md)  
- [BizTalk-Services: Naam van de uitgevende instelling en de sleutel van de uitgevende instelling](biztalk-issuer-name-issuer-key.md)  
- [BizTalk-Services: Back-up en terugzetten](biztalk-backup-restore.md)

## <a name="see-also"></a>Zie ook
- [Hybride verbindingen](integration-hybrid-connection-overview.md)  
- [BizTalk-Services: Ontwikkelaar, Basic, Standard en Premium-edities grafiek](biztalk-editions-feature-chart.md)  
- [BizTalk-Services: Met behulp van Azure klassieke portal inrichten](biztalk-provision-services.md)  
- [BizTalk-Services: Grafiek de status van BizTalk-Service](biztalk-service-state-chart.md)  
- [Hoe kan ik starten via de Azure BizTalk-Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[QuickStart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png
 
