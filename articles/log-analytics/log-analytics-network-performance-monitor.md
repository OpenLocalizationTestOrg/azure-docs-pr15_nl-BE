<properties
    pageTitle="Prestatiemeter-oplossing in OMS netwerk | Microsoft Azure"
    description="Prestaties van Netwerkcontrole helpt u de prestaties van de netwerken in de buurt van real-eenmalige-te controleren detecteren en Ga naar netwerkknelpunten."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="banders"/>

# <a name="network-performance-monitor-preview-solution-in-oms"></a>Netwerk oplossing in OMS Prestatiemeter (voorbeeld)

>[AZURE.NOTE] Dit is een [voorbeeld van oplossing](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Dit document beschrijft hoe te installeren en gebruik de Prestatiemeter netwerk oplossing in OMS, waarmee u de prestaties van uw netwerken in de buurt van real-eenmalige-om te controleren voor het opsporen en vinden het netwerk knelpunten. Met de prestaties van Netwerkcontrole-oplossing kunt u de winst- en vertragingstijd tussen twee netwerken, subnetten of servers controleren. Prestaties van Netwerkcontrole detecteert de netwerkverbinding zoals verkeer blackholing, routering fouten en problemen die het conventionele spoorwegnet controle methoden zijn niet kunnen detecteren. Prestaties van Netwerkcontrole waarschuwingen genereert en ontvangt een bericht als en wanneer een drempel voor een netwerkverbinding wordt gekraakt. Deze drempels kunnen automatisch door het systeem worden geleerd of kunt u deze aangepaste regels gebruiken. Prestatiemeter netwerk zorgt voor tijdige opsporing van netwerkproblemen en de bron van het probleem met een bepaald netwerksegment of apparaat is vertaald.

U kunt netwerkproblemen met het dashboard oplossing waarin samengevatte informatie over uw netwerk met inbegrip van recente netwerkgebeurtenissen gezondheid, beschadigde netwerkverbindingen en subnetwerk koppelingen die met hoge pakketverlies en latentie kampen detecteren. U kunt inzoomen tot een netwerkverbinding om de huidige status van het subnetwerk koppelingen als knooppunt naar koppelingen weer te geven. U kunt ook de historische ontwikkeling van verlies en de wachttijden op het netwerk, subnetwerk en knooppunt naar niveau weergeven. U kunt tijdelijke netwerkproblemen opsporen door het bekijken van historische trendgrafieken voor pakketverlies en latentie en netwerkknelpunten op een topologieoverzicht vinden. De topologie van interactieve grafiek kunt u visualiseren de netwerkroutes door hop-hop-en de oorzaak van het probleem achterhalen. Net als andere oplossingen kunt u logboek zoeken naar verschillende analytics-vereisten voor het maken van aangepaste rapporten op basis van de gegevens die zijn verzameld door Netwerkcontrole prestaties.

De oplossing maakt gebruik van synthetische transacties als primaire mechanisme voor het detecteren van netwerkfouten. Zo kunt u zonder te letten op een bepaald netwerkapparaat leverancier of model. Het werkt voor bedrijven, (IaaS) wolk en hybride omgevingen. De oplossing detecteert automatisch de netwerktopologie en verschillende routes in het netwerk.

Typische netwerk monitoring producten gericht op het controleren van de status van netwerk apparaat (routers, switches, enz.) maar geen inzicht in de werkelijke kwaliteit van de netwerkverbinding tussen twee punten, die de prestaties van Netwerkcontrole heeft.

### <a name="using-the-solution-standalone"></a>De zelfstandige oplossing

Als u controleren de kwaliteit van de netwerkverbindingen tussen de kritieke werklast wilt, kunt netwerken, datacenters of sites van office en u de prestaties van Netwerkcontrole-oplossing zelf controleren gezondheid connectiviteit tussen:

- meerdere datacenters of office-sites die zijn verbonden met een openbaar of particulier netwerk
- kritieke werklast die zakelijke toepassingen worden uitgevoerd
- openbare cloud-services zoals Microsoft Azure of Amazon Web Services (AWS) en op ruimten netwerken, als u IaaS (VM) beschikbaar is en u gateways zodanig geconfigureerd dat communicatie tussen netwerken op gebouwen en netwerken cloud
- Azure en op ruimten netwerken wanneer u Route Express

### <a name="using-the-solution-with-other-networking-tools"></a>Met behulp van de oplossing met andere hulpmiddelen

Als u controleren van een aantal zakelijke toepassingen wilt, kunt u de prestaties van Netwerkcontrole oplossing als oplossing companion van andere hulpprogramma's. Een traag netwerk kan leiden tot trage toepassingen en Network Performance Monitor kunt u de toepassing problemen die worden veroorzaakt door netwerkproblemen onderliggende onderzoeken. Omdat de oplossing niet geen toegang hebben tot het netwerkapparaten hoeft, moet de beheerder van de toepassing niet afhankelijk zijn van een netwerk team informatie te verstrekken over hoe het netwerk is op het gebied van toepassingen.

Ook als u al in andere hulpprogramma's voor Netwerkcontrole investeert, vervolgens de oplossing kan een aanvulling op deze hulpprogramma's omdat het meest traditioneel netwerk monitoring oplossingen bieden inzicht in de end-to-end netwerk prestatiecijfers als verlies en latentie.  De prestaties van Netwerkcontrole-oplossing kunt die lacune in te vullen.


## <a name="installing-and-configuring-agents-for-the-solution"></a>Installeren en configureren van agenten voor de oplossing

De fundamentele processen installeren agentia op het [logboek Analytics verbinding maken met Windows-computers](log-analytics-windows-agents.md) en [Operations Manager verbinding maken met Analytics logboek](log-analytics-om-agents.md)gebruiken.

>[AZURE.NOTE]
U moet ten minste 2 agenten installeren om onvoldoende gegevens om te ontdekken en controleren van uw netwerkbronnen. Anders wordt blijft de oplossing in een configuratie staat totdat installeert en configureert u extra agenten.

### <a name="where-to-install-the-agents"></a>Waar de agents installeren

Voordat u de agenten installeert, kunt u de topologie van uw netwerk en welke delen van het netwerk dat u wilt controleren. Wij raden aan dat meer dan een agent voor elk subnet dat u wilt controleren. Met andere woorden, voor elk subnet dat u wilt controleren, kiest u twee of meer servers of VMs en de agent installeren op de systemen.

Als u over de topologie van uw netwerk twijfelt installeren de agenten op servers met kritieke werklast waar u de netwerkprestaties te bewaken. U wilt bijhouden van een netwerkverbinding tussen een webserver en een server waarop SQL Server wordt uitgevoerd. In dit voorbeeld zou u een agent installeren op beide servers.

Agenten controleren netwerkverbinding (koppelingen) tussen hosts--niet de hosts zelf. Voor het controleren van de netwerkkoppeling van een, moet u dus agenten installeren op beide eindpunten van de koppeling.

### <a name="configure-agents"></a>Agents configureren

Nadat u de agenten hebt geïnstalleerd, moet u openen firewallpoorten voor deze computers om ervoor te zorgen dat de agenten kunnen communiceren. U moet downloaden en vervolgens het [EnableRules.ps1 PowerShell script](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) zonder parameters uitvoeren in een venster PowerShell met beheerdersbevoegdheden

Het script maakt de Prestatiemeter netwerk vereiste registersleutels en het Windows firewall-regels zodat agenten TCP-verbindingen maken met elkaar maakt. De registersleutels die zijn gemaakt door het script opgeven of u aan te melden voor de logboeken voor foutopsporing en het pad voor het bestand met Logboeken. Het definieert ook de agent TCP-poort die wordt gebruikt voor communicatie. De waarden voor deze sleutels worden automatisch ingesteld door het script, zodat deze sleutels niet handmatig te wijzigen.

De poort die standaard geopend is 8084. U kunt een aangepaste poort door middel van de parameter `portNumber` aan het script. Echter, dezelfde poort moet worden gebruikt op alle computers waarop het script wordt uitgevoerd.

>[AZURE.NOTE] Het script EnableRules.ps1 configureert Windows firewall-regels alleen op de computer waarop het script wordt uitgevoerd. Hebt u een firewall van het netwerk, moet u ervoor zorgen dat verkeer dat bestemd is voor de TCP-poort die wordt gebruikt door Netwerkcontrole prestaties kan.


## <a name="configuring-the-solution"></a>Configuratie van de oplossing

Gebruik de volgende informatie te installeren en configureren van de oplossing.

1. De prestaties van Netwerkcontrole oplossing krijgt gegevens van computers met Windows Server 2008 SP 1 of hoger of Windows 7 SP1 of hoger, die dezelfde eisen als de Microsoft Monitoring Agent (MMA).
2. De prestaties van Netwerkcontrole-oplossing toevoegen aan uw Kantoorbeheersysteem werkruimte met behulp van het proces beschreven in [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).  
  ![Symbool voor netwerk Performance Monitor](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. De portal OMS ziet u een nieuwe tegel met de naam **Netwerkcontrole prestaties** met het bericht *oplossing is aanvullende configuratie vereist*. U moet de oplossing toevoegen van netwerken op basis van subnetwerken en knooppunten die worden ontdekt door agents configureren. Klik op **Prestaties van Netwerkcontrole** als u wilt beginnen met het configureren van het netwerk.  
  ![oplossing is aanvullende configuratie vereist.](./media/log-analytics-network-performance-monitor/npm-config.png)


### <a name="configure-the-solution-with-a-default-network"></a>De oplossing met een standaardnetwerk configureren

U ziet op de configuratiepagina van één netwerk met de naam **standaard**. Wanneer u alle netwerken nog niet hebt gedefinieerd, worden alle subnetten automatisch gedetecteerd in het netwerk geplaatst.

Telkens wanneer u een netwerk maakt, wordt u een subnet aan toe te voegen en dat subnet wordt verwijderd uit het netwerk. Als u een netwerk verwijdert, worden automatisch alle subnetten zijn met het netwerk standaard geretourneerd.

Met andere woorden, is het netwerk standaard de container voor alle subnetten die niet zijn opgenomen in een door de gebruiker gedefinieerde netwerk. U niet bewerken of verwijderen van het netwerk. Het blijft altijd in het systeem. U kunt echter zoveel netwerken als nodig maken.

In de meeste gevallen de subnetten die in uw organisatie worden geordend in meer dan één netwerk en moet u een of meer netwerken logisch groeperen de subnetten maken.

### <a name="create-new-networks"></a>Nieuwe netwerken maken

Een netwerk in Prestatiemeter netwerk is een container voor subnetten. U kunt een netwerk maken met een naam die u wilt en subnetten aan het netwerk toevoegen. Zo kunt u een netwerk met de naam *Building1* maken en voeg vervolgens de subnetten, of kunt u een netwerk met de naam *DMZ* maken en voeg vervolgens alle subnetten die deel uitmaken van dit netwerk gedemilitariseerde zone.

#### <a name="to-create-a-new-network"></a>Voor het maken van een nieuw netwerk

1. Klik op **netwerk toevoegen** en typ vervolgens de naam en beschrijving.
2.  Selecteer een of meer subnetten en klik vervolgens op **toevoegen**.
3. Klik op **Opslaan** om de configuratie op te slaan.  
  ![netwerk toevoegen](./media/log-analytics-network-performance-monitor/npm-add-network.png)



### <a name="wait-for-data-aggregation"></a>Wacht tot het samenvoegen van gegevens

Nadat u de configuratie voor de eerste keer hebt opgeslagen, wordt de oplossing begint pakket verlies en latentie Netwerkgegevens verzamelen tussen de knooppunten waarbij agents zijn geïnstalleerd. Dit proces kan even duren, soms gedurende 30 minuten. Tijdens deze staat wordt de tegel prestaties van Netwerkcontrole op de overzichtspagina een bericht weergegeven *tijdens het samenvoegen van gegevens*.

![het samenvoegen van gegevens in uitvoering](./media/log-analytics-network-performance-monitor/npm-aggregation.png)


Wanneer de gegevens is geladen, ziet u de gegevens met Netwerkcontrole prestaties bijgewerkt naast elkaar weergeven.

![Network Performance Monitor naast elkaar](./media/log-analytics-network-performance-monitor/npm-tile.png)

Klik op de tegel om het dashboard Network Performance Monitor weer te geven.

![Netwerk Prestatiemeter dashboard](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Controle-instellingen voor subnetten bewerken

Alle subnetten waarin ten minste één agent is geïnstalleerd, worden weergegeven op het tabblad **subnetwerken** op de configuratiepagina.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>Inschakelen of uitschakelen voor bepaalde subnetwerken controleren

1. Selecteren of het selectievakje naast het **subnetwerk-ID** en zorg dan dat **voor controle** ingeschakeld of uitgeschakeld, indien nodig is. U kunt selecteren of uit meerdere subnetten. Wanneer uitgeschakeld, worden geen subnetwerken bewaakt de agenten zal worden bijgewerkt om te stoppen met andere agentia ping.
2. Kies de knooppunten die u controleren op een bepaalde subnetwerk wilt door het subnetwerk selecteren in de lijst en de vereiste knooppunten verplaatsen tussen de lijsten met niet-beheerde en gecontroleerde knooppunten.
U kunt een aangepaste **Beschrijving** toevoegen aan het subnetwerk, als u dat wilt.
3. Klik op **Opslaan** om de configuratie op te slaan.  
  ![bewerken subnet](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Kies de knooppunten controleren

Alle knooppunten met een agent is geïnstalleerd op de systemen worden weergegeven in het tabblad **knooppunten** .

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>Inschakelen of uitschakelen voor knooppunten controleren

1. Selecteer of de knooppunten die u wilt controleren of het beëindigen van de controle uit.
2. Klik op **gebruiken voor controle**, in- of uitgeschakeld.
3. Klik op **Opslaan**.  
  ![knooppunt-controle inschakelen](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)


### <a name="set-monitoring-rules"></a>Set regels controleren

Prestaties van Netwerkcontrole genereert gezondheid gebeurtenissen over de verbinding tussen twee knooppunten of netwerk of subnetwerk koppelingen wanneer een drempel is nagekomen. Deze drempels kunnen automatisch worden geleerd door het systeem of u kunt deze aangepaste waarschuwingsregels.

Met de *standaard regel* wordt gemaakt door het systeem en is er een gebeurtenis gezondheid wanneer koppelingen verloren gaan of wachttijd tussen een willekeurig paar netwerken of subnetwerk inbreuken op de drempel systeem geleerd. U kunt de standaardregel uitschakelen en het maken van aangepaste regels voor controle

#### <a name="to-create-custom-monitoring-rules"></a>Voor het maken van aangepaste regels voor controle

1. Klik op **Regel toevoegen** op het tabblad **Monitor** en voer de naam en beschrijving.
2. De combinatie van een netwerk of subnetwerk koppelingen om te controleren in de lijst selecteren.
3. Selecteert u eerst het netwerk waarin de eerste subnetwerk (s) van belang is opgenomen in de vervolgkeuzelijst van het netwerk en selecteer de subnetwerk (s) in de vervolgkeuzelijst met bijbehorende subnetwerk.
Selecteer **alle subnetwerken** als u wilt controleren de subnetwerken in een netwerkkoppeling. Selecteer ook de andere subnetwerk (s) van belang. En klikt u op **Uitzondering toevoegen** als u wilt uitsluiten van controle voor bepaalde subnetwerk koppelingen uit de selectie die u hebt aangebracht.
4. Als u niet wilt maken van de gezondheid van gebeurtenissen voor de items die u hebt geselecteerd, schakelt u **controle op de links waarop deze regel van de gezondheid inschakelen**.
5. Kies voorwaarden controleren.
U kunt aangepaste drempelwaarden instellen voor het genereren van de gebeurtenis gezondheid door drempelwaarden te typen. Wanneer de waarde van de voorwaarde boven de geselecteerde drempel voor het geselecteerde netwerk/subnetwerk paar gaat, wordt een gebeurtenis gezondheid gegenereerd.
6. Klik op **Opslaan** om de configuratie op te slaan.  
  ![Aangepaste controle regel maken](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)


## <a name="data-collection-details"></a>Data collectie details

Prestaties van Netwerkcontrole pakketten van SYNACK-TCP SYN-ACK-handshake gebruikt voor het verzamelen van verlies en latentie informatie en traceroute wordt ook gebruikt voor het ophalen van gegevens over de topologie.

In de volgende tabel bevat de methoden van de collectie gegevens en andere details over hoe de gegevens worden verzameld voor de prestaties van Netwerkcontrole.

| platform | Directe Agent | SCOM agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | frequentie van verzameling |
|---|---|---|---|---|---|---|
| Windows |![Ja](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Ja](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Nee](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|            ![Nee](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|![Nee](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)| TCP-handshakes elke 5 seconden, gegevens verzonden om de 3 minuten |

De oplossing maakt gebruik van synthetische transacties voor de beoordeling van de gezondheid van het netwerk. OMS agenten geïnstalleerd op verschillende wijzen in de exchange TCP-pakketten met elkaar en, in het proces, informatie over retouren tijd en pakket verlies, indien van toepassing. Elke agent voert ook regelmatig een trace route met andere stoffen in alle verschillende routes te vinden in het netwerk die moet worden getest. De agenten zijn met behulp van deze gegevens, kunnen de netwerkvertraging en verliescijfers pakket afleiden. De tests worden elke vijf seconden herhaald en de gegevens worden samengevoegd voor een periode van drie minuten door de agenten voordat u deze uploadt naar OMS.

>[AZURE.NOTE] Hoewel vaak agenten met elkaar communiceren, ze tijdens de proeven geen veel netwerkverkeer genereren. Agenten vertrouwen alleen op SYNACK-TCP SYN-ACK-handshake pakketten om te bepalen van de winst en de latentie--geen gegevens pakketten worden uitgewisseld. Tijdens dit proces agenten met elkaar communiceren alleen wanneer dit nodig is en de communicatie agent topologie is geoptimaliseerd voor het netwerkverkeer te verminderen.

## <a name="using-the-solution"></a>Met behulp van de oplossing

Deze sectie wordt uitgelegd dat het dashboard alle functies en het gebruik ervan.

### <a name="solution-overview-tile"></a>Naast elkaar oplossing-overzicht

Nadat u de prestaties van Netwerkcontrole oplossing hebt ingeschakeld, bevat de tegel oplossing op de pagina overzicht OMS een snel overzicht van de status van het netwerk. Een ringdiagram met het nummer van het subnetwerk van gezonde en beschadigde koppelingen wordt weergegeven. Wanneer u op de tegel klikt, wordt de oplossing dashboard geopend.

![Network Performance Monitor naast elkaar](./media/log-analytics-network-performance-monitor/npm-tile.png)


### <a name="network-performance-monitor-solution-dashboard"></a>Network Performance Monitor oplossing dashboard

De **Samenvatting netwerk** blade bevat een overzicht van de netwerken en hun relatieve grootte. Dit wordt gevolgd door het totale aantal netwerkverbindingen, subnet koppelingen en paden in het systeem (een pad bestaat uit de IP-adressen van de twee hosts met agenten en de hops tussen hen) met tegels.

De **Top gezondheid netwerkgebeurtenissen** blade bevat een lijst met meest recente gezondheid gebeurtenissen en waarschuwingen in het systeem en de tijd sinds de gebeurtenis actief is geweest. Een gebeurtenis van de gezondheid of het signaal wordt gegenereerd wanneer de pakketverlies of vertraging van een netwerk of subnetwerk koppeling een drempelwaarde overschrijdt.

De blade **Boven beschadigd netwerkverbindingen** bevat een overzicht van beschadigde netwerkverbindingen. Dit zijn de netwerkkoppelingen die een of meer gezondheid schadelijke gebeurtenis ze op het moment dat hebben.

De blades **boven subnetwerk met meest verlies** en **Subnetwerk koppelingen met meest latentie** weergeven respectievelijk de bovenste subnetwerk door pakketverlies en bovenste subnetwerk koppelingen door latentie. Lange wachttijden of een bepaalde hoeveelheid pakketverlies kan worden verwacht op bepaalde netwerkkoppelingen. Deze koppelingen worden weergegeven in de top tien-lijsten, maar niet zijn gemarkeerd als beschadigd.

Het blad **Algemene query's** bevat een set van zoekquery's ruwe Netwerkcontrole gegevens rechtstreeks ophalen. U kunt deze query's als uitgangspunt voor het maken van uw eigen query's voor aangepaste rapportage.

![Netwerk Prestatiemeter dashboard](./media/log-analytics-network-performance-monitor/npm-dash01.png)


### <a name="drill-down-for-depth"></a>Drill-down voor diepte

U kunt klikken op verschillende koppelingen op de oplossing dashboard om drill-down dieper in elk gebied van belang. Bijvoorbeeld wanneer er een waarschuwing of een beschadigde netwerkkoppeling op het dashboard wordt weergegeven, kunt u deze verder onderzoeken. U zult worden genomen om een pagina met daarop de subnetwerk koppelingen voor de desbetreffende netwerkverbinding. Is het mogelijk voor de status van het verlies, vertraging en de gezondheid van elke koppeling subnetwerk en snel uitzoeken welke koppelingen subnetwerk het probleem veroorzaken. U kunt **koppelingen van knooppunt weergeven** om te zien alle koppelingen van een knooppunt voor de beschadigde subnet koppeling klikt. Vervolgens kunt u afzonderlijke knooppunten koppelingen weergeven en de koppelingen beschadigd knooppunt.

Kunt u **de topologie weergeven** om de topologie door hop-hop van de routes tussen de bron- en doeladressen knooppunten weer te geven. De routes zijn beschadigd of hops worden in rood weergegeven zodat u snel het probleem op een bepaald gedeelte van het netwerk kunt herkennen.

![drill-down-gegevens](./media/log-analytics-network-performance-monitor/npm-drill.png)


#### <a name="trend-charts"></a>Trendgrafieken

Op elk niveau dat u inzoomen, ziet u de trend van verlies en de wachttijden voor een netwerkverbinding. Trendgrafieken zijn ook beschikbaar voor subnetwerk en knooppunt koppelingen. U kunt het tijdsinterval voor de grafiek worden uitgezet met behulp van het besturingselement tijd boven aan de grafiek kunt wijzigen.

Trendgrafieken geven u een historisch perspectief van de prestaties van een netwerkverbinding. Sommige van de netwerkverbinding tijdelijk van aard zijn en zou het moeilijk zijn te vangen alleen door te kijken naar de huidige status van het netwerk. Dit komt doordat problemen kunnen snel over het oppervlak en verdwijnen voordat iedereen meldingen, alleen op een later tijdstip opnieuw. Dergelijke problemen van voorbijgaande aard kunnen ook lastig voor beheerders die problemen vaak oppervlak als Onverklaarbaar stijgingen van de toepassing responstijd, zelfs wanneer alle componenten lijken te functioneren.

Dit soort problemen kunt u gemakkelijk vinden door te kijken waar het probleem worden weergegeven als een plotselinge piek in de netwerkvertraging of pakketverlies trend grafieken.

![trend grafieken](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>Hop door hop topologieoverzicht

Prestaties van Netwerkcontrole ziet u de topologie door hop-hop van routes tussen twee knooppunten op een interactieve topologieoverzicht. U kunt het topologieoverzicht weergeven door een knooppunt te klikken en vervolgens op **weergave-topologie**. U kunt ook het topologieoverzicht weergeven door te klikken op de tegel **paden** op het dashboard. Als u de **paden** op het dashboard klikt, hebt u de bron- en doeladressen knooppunten selecteren in het linker deelvenster en klik vervolgens op **grafiek** wilt uitzetten van de routes tussen twee knooppunten.

Het topologieoverzicht geeft weer hoeveel routes zijn tussen de twee knooppunten en wat de gegevenspakketten nemen paden. Netwerkknelpunten zijn gemarkeerd in het rood op de kaart van de topologie. Een defecte netwerkverbinding of een defecte netwerkapparaat kunt u vinden door te kijken naar de rood gekleurde elementen in het topologieoverzicht.

Wanneer u op een knooppunt of het aanwijzen van het het topologieoverzicht, ziet u de eigenschappen van het knooppunt als de FQDN-naam en IP-adres. Klik op een hop om te zien is het IP-adres. U kunt bepaalde routes door te schakelen en vervolgens te klikken op alleen de routes die u wilt markeren op de kaart markeren. U kunt in- of het topologieoverzicht uitzoomen met het muiswiel.

Overigens de topologie die wordt weergegeven in de overzichtsweergave van layer 3-topologie en bevat geen laag 2-apparaten en verbindingen.

![hop door hop topologieoverzicht](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Fout lokalisatie

Netwerkcontrole prestaties is netwerkknelpunten in het geen verbinding met het netwerkapparaten te vinden. Op basis van de gegevens die worden verzameld uit het netwerk en door het toepassen van geavanceerde algoritmen in het diagram van het netwerk, is Network Performance Monitor een probabilistische schatting van de onderdelen van het netwerk die zeer waarschijnlijk de oorzaak van het probleem.

Deze benadering is handig om te bepalen van de netwerkknelpunten in het wanneer de toegang tot de hops is niet beschikbaar omdat deze niet alle gegevens worden verzameld uit de netwerkapparaten zoals routers of switches nodig. Dit is ook handig wanneer u de hops tussen twee knooppunten zijn niet in uw beheer. Zo mogelijk de hops ISP-routers.

### <a name="log-analytics-search"></a>Logboek Analytics zoeken

Alle gegevens die via het netwerk Prestatiemeter dashboard blootgestelde grafisch en drill-down's vindt u ook native in Analytics logboek zoeken. U kunt een query de gegevens met behulp van de querytaal zoeken en aangepaste rapporten maken door de gegevens te exporteren naar Excel of PowerBI. Het blad **Algemene query's** in het dashboard heeft enkele handige query's die u als uitgangspunt gebruiken kunt voor het maken van uw eigen query's en rapporten.

![zoekquery 's](./media/log-analytics-network-performance-monitor/npm-queries.png)


## <a name="investigate-the-root-cause-of-a-health-alert"></a>Onderzoeken van de onderliggende oorzaak van een waarschuwing voor de gezondheid

Nu dat u hebt gelezen over prestaties van Netwerkcontrole, bekijk een eenvoudig onderzoek naar de oorzaak voor de gezondheid.

1. Op de pagina overzicht krijgt u een snelle momentopname van de gezondheid van uw netwerk met inachtneming van de **Prestatiemeter netwerk** naast elkaar. Merk op dat van de 80 subnetwerken koppelingen worden gecontroleerd, 43 beschadigd zijn. Dit garandeert onderzoek. Klik op de tegel om het dashboard oplossing weer te geven.  
  ![Network Performance Monitor naast elkaar](./media/log-analytics-network-performance-monitor/npm-investigation01.png)

2. In de onderstaande voorbeeldafbeelding ziet u dat er zijn momenteel 4 gezondheid-gebeurtenissen en 4 netwerkverbindingen die beschadigd zijn. Wilt u het probleem onderzoeken en klik op de koppeling van de **Sharepoint-website** netwerk voor informatie over de oorzaak van het probleem.  
  ![Voorbeeld van een beschadigde netwerk](./media/log-analytics-network-performance-monitor/npm-investigation02.png)

3. De pagina ziet u alle koppelingen in het subnetwerk in **Sharepoint Web** netwerkkoppeling. U zult zien dat voor zowel de subnetwerk koppelingen de latentie heeft gepasseerd de drempel waardoor de netwerkkoppeling beschadigd. U ziet ook de latentie trends van zowel de koppelingen in het subnetwerk. U kunt de selectie tijd besturingselement in de grafiek te concentreren op de vereiste periode. Hier ziet u de tijd van de dag wanneer de latentie van de piek heeft bereikt. Verderop vindt u de logboeken voor deze periode het probleem kunnen onderzoeken. Klik op **knooppunt-koppelingen weergeven** naar drill-down verder.  
  ![Voorbeeld van de beschadigde subnet koppelingen](./media/log-analytics-network-performance-monitor/npm-investigation03.png)

4.  Net als bij de vorige pagina, de pagina voor de koppeling van bepaalde subnetwerk staan onder het knooppunt samenstellende koppelingen. U kunt dezelfde bewerkingen uitvoeren als in de vorige stap hier. Klik op **de topologie weergeven** om de topologie tussen de 2 knooppunten weer te geven.  
  ![Voorbeeld van de beschadigde knooppunt koppelingen](./media/log-analytics-network-performance-monitor/npm-investigation04.png)

5. De paden tussen de 2 geselecteerde knooppunten worden uitgezet in het topologieoverzicht. U kunt de topologie door hop-hop van routes tussen twee knooppunten in het topologieoverzicht visualiseren. Je krijgt een duidelijk beeld van het aantal routes bestaan tussen de twee knooppunten en wat de gegevenspakketten nemen paden. Netwerkknelpunten worden gemarkeerd in de kleur rood. Een defecte netwerkverbinding of een defecte netwerkapparaat kunt u vinden door te kijken naar de rood gekleurde elementen in het topologieoverzicht.  
  ![Voorbeeld van de beschadigde topologie weergeven](./media/log-analytics-network-performance-monitor/npm-investigation05.png)

6. Verlies, vertraging en het aantal hops in elk pad kunnen worden herzien in het detailvenster op **Pad** . In dit voorbeeld ziet u dat er 3 beschadigd paden zijn zoals vermeld in het deelvenster. Met de schuifbalk kunt u de details van die paden beschadigd.  Gebruik de selectievakjes selecteren een van de paden zodat de topologie voor slechts één pad worden getekend. U kunt uw muiswiel het topologieoverzicht in- of uitzoomen.

  In de onderstaande afbeelding duidelijk ziet u de oorzaak van het probleemgebieden aan de specifieke sectie van het netwerk door te kijken naar de paden en de hops in de kleur rood. Te klikken op een knooppunt in het topologieoverzicht blijkt dat de eigenschappen van het knooppunt, met inbegrip van de FQDN-naam en IP-adres. Ziet u het IP-adres van de hop op een hop te klikken.  
  ![beschadigde topologie - pad details voorbeeld](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="next-steps"></a>Volgende stappen

- [Zoeken in Logboeken](log-analytics-log-searches.md) om gedetailleerde prestaties gegevensrecords te bekijken.
