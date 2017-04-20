<properties
   pageTitle="Listener voor AlwaysOn availabilty groep maken voor SQL Server in Azure virtuele Machines"
   description="Stapsgewijze instructies voor het maken van een listener voor een groep AlwaysOn availabilty voor SQL Server in Azure virtuele Machines"
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="07/12/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-an-internal-load-balancer-for-an-alwayson-availability-group-in-azure"></a>Een interne taakverdeling voor een groep AlwaysOn beschikbaarheid in Azure configureren

In dit onderwerp wordt uitgelegd hoe u een interne taakverdeling voor een groep van de beschikbaarheid van SQL Server AlwaysOn in Azure virtuele machines die worden uitgevoerd in de resource manager model te maken. Een groep AlwaysOn beschikbaarheid vereist een load balancer als de SQL Server-exemplaren op Azure virtuele machines. De taakverdeling slaat het IP-adres voor de beschikbaarheid van groep listener. Als een beschikbaarheidsgroep van meerdere gebieden omvat, moet elke regio een taakverdeling.

Om deze taak moet u een beschikbaarheidsgroep van SQL Server AlwaysOn geïmplementeerd op Azure virtuele machines in de resource manager model. Zowel SQL Server virtuele machines moet behoren tot dezelfde set beschikbaarheid. Voor het automatisch maken AlwaysOn beschikbaarheid in Azure resourcemanager kunt u de [Microsoft-sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) . Met deze sjabloon maakt automatisch de interne taakverdeling voor u. 

Als u wilt, kunt u [een groep AlwaysOn beschikbaarheid handmatig configureren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Dit onderwerp vereist dat al uw beschikbaarheid groepen zijn geconfigureerd.  

Verwante onderwerpen omvatten:

 - [AlwaysOn beschikbaarheidsgroepen configureren in Azure VM (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
 
 - [VNet-VNet-verbinding configureren met behulp van bronbeheer Azure en PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="steps"></a>Werk

Door stap voor stap in dit document maakt en een taakverdeling configureren in de portal Azure. Als dat is voltooid, configureert u het cluster voor het gebruik van het IP-adres van de verdeling van de belasting voor de listener AlwaysOn beschikbaarheid groep.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Maken en configureren van de taakverdeling in de Azure portal

In dit gedeelte van de taak wordt u de volgende stappen uit in de Azure portal doen:

1. De taakverdeling maakt en configureert u het IP-adres

1. De back-end-toepassingen configureren

1. De sonde maken 

1. De regels voor taakverdeling instellen

>[AZURE.NOTE] Als de SQL-Servers in verschillende resourcegroepen en regio's, doet u al deze stappen tweemaal, eenmaal in elke bronnengroep.

## <a name="1-create-the-load-balancer-and-configure-the-ip-address"></a>1. Maak de taakverdeling en het IP-adres configureren

De eerste stap is het maken van de taakverdeling. Open de resourcegroep met de SQL Server virtuele machines in de portal voor Azure. Klik op **toevoegen**in de resourcegroep.

- **De belasting voor documentconversies**zoeken. Selecteer in de zoekresultaten **-Taakverdeling**, die is gepubliceerd door **Microsoft**.

- Klik op de **Load Balancer** -blade **maken**.

- Configureer op **de belasting voor documentconversies maken**, de verdeling van de belasting als volgt:

| Instelling | Waarde |
| ----- | ----- |
| **Naam** | Een naam voor de taakverdeling. Bijvoorbeeld **sqlLB**. |
| **Schema** | **Interne** |
| **Virtueel netwerk** | Kies het virtuele netwerk waarmee de SQL-Servers zijn.   |
| **Subnet**  | Kies de subnet die de SQL-Servers in. |
| **Abonnement** | Als er meerdere abonnementen, wordt dit veld weergegeven. Selecteer het abonnement dat u aan deze bron wilt koppelen. Het is normaal gesproken het abonnement hetzelfde als alle bronnen voor de beschikbaarheidsgroep.  |
| **Resourcegroep** | Kies de resourcegroep die de SQL-Servers in. | 
| **Locatie** | Kies de Azure locatie die de SQL-Servers in. |

- Klik op **maken**. 

Azure maakt de taakverdeling die u hierboven hebt geconfigureerd. De taakverdeling behoort tot een bepaald netwerk, subnet, resourcegroep en locatie. Nadat Azure is voltooid, controleert u of de instellingen van de load balancer in Azure. 

Het IP-adres van de load balancer nu configureren.  

- Klik op de load balancer **Instellingen** blade, **IP-adres**. De blade **-IP-adres** zien dat dit een privé-taakverdeling op dezelfde virtuele netwerk als uw SQL-Servers. 

- Stel de volgende instellingen: 

| Instelling | Waarde |
| ----- | ----- |
| **Subnet** | Kies de subnet die de SQL-Servers in. |
| **Toewijzing** | **Statische** |
| **IP-adres** | Typ een ongebruikte virtuele IP-adres van het subnet.  |

- De instellingen opslaan.

Verdeling van de belasting heeft nu een IP-adres. Dit IP-adres vastleggen. U gebruikt dit IP-adres wanneer u een listener op het cluster maken. Gebruik dit adres in een PowerShell script verderop in dit artikel, de `$ILBIP` variabele.



## <a name="2-configure-the-backend-pool"></a>2. de back-end-toepassingen configureren

De volgende stap is het maken van een back-end-adresgroep. Azure roept de back-end-toepassingen- *back-end-toepassingen*. In dit geval is de back-end-toepassingen de adressen van de twee SQL-Servers in de groep van de beschikbaarheid. 

- Klik op de verdeling van de belasting die u hebt gemaakt in de resourcegroep. 

- Klik op **Instellingen voor** **back-end-toepassingen**.

- Op de **Backend-adresgroepen**, klikt u op **toevoegen** om een back-end-adresgroep maken. 

- Typ een naam voor de back-end-toepassingen op de **backend van toepassingen toevoegen** onder **naam**.

- Klik op **+ toevoegen een virtuele machine**onder **virtuele machines** . 

- Klik onder **Kies virtuele machines** op **kiest u een beschikbaarheid** en geeft dat de beschikbaarheid wordt ingesteld dat de SQL Server virtuele machines te horen.

- Nadat u de set beschikbaarheid hebt gekozen, klikt u op **de virtuele machines te kiezen**. Klik op de twee virtuele machines die host zijn van de SQL Server-exemplaren in de beschikbaarheidsgroep. Klik op **selecteren**. 

- Klik op **OK** om de blades voor **Kies virtuele machines**en **back-end-toepassingen toevoegen**te sluiten. 

Azure werkt u de instellingen voor de back-end-adresgroep. Uw set beschikbaarheid heeft nu een groep van twee SQL-Servers.

## <a name="3-create-a-probe"></a>3. een sonde maken

De volgende stap is het maken van een sonde. De sonde wordt gedefinieerd hoe Azure controleert of die van de SQL-Servers momenteel eigenaar is van de groep beschikbaarheid listener. Azure zal de service op basis van IP-adres op een poort die u opgeeft bij het maken van de sonde probe.

- Klik op de load balancer **Instellingen** -blade **sondes**. 

- Klik op **toevoegen**op het blad **sondes** .

- Configureer de sonde op de blade **sonde toevoegen** . De volgende waarden gebruiken voor het configureren van de sonde:

| Instelling | Waarde |
| ----- | ----- |
| **Naam** | Een naam die de sonde. Bijvoorbeeld **SQLAlwaysOnEndPointProbe**. |
| **Protocol** | **TCP** |
| **Poort** | U kunt een willekeurige beschikbare poort. Bijvoorbeeld *59999*.    |
| **Interval**  | *5* | 
| **Beschadigde drempel**  | *2* | 

- Klik op **OK**. 

>[AZURE.NOTE] Zorg ervoor dat de poort die u opgeeft geopend op de firewall van zowel SQL Server is. Beide servers vereisen een binnenkomende regel voor de TCP-poort die u gebruikt. Zie [toevoegen of de firewallregel bewerken](http://technet.microsoft.com/library/cc753558.aspx) voor meer informatie. 

Azure maakt de sonde. Azure zal de sonde gebruiken om te testen welke SQL-Server de listener voor de groep van de beschikbaarheid heeft.

## <a name="4-set-the-load-balancing-rules"></a>4. de regels voor taakverdeling instellen

De regels voor taakverdeling instellen. De load balancing regels configureren hoe de taakverdelingsvoorziening verkeer routeert naar de SQL-Servers. Voor de verdeling van de belasting wordt u direct retour server inschakelen omdat slechts één van de twee SQL-Servers ooit eigenaar is van de beschikbaarheid van groep listener resource tegelijk.

- Klik op de load balancer **Instellingen** blade, **balancing regels geladen**. 

- Klik op **toevoegen**op het blad **Load balancing regels** .

- Het blad **toevoegen laden balancing regels** gebruiken voor het configureren van de regel voor de taakverdeling. Gebruik de volgende instellingen: 

| Instelling | Waarde |
| ----- | ----- |
| **Naam** | Een naam die de regels voor de taakverdeling. Bijvoorbeeld **SQLAlwaysOnEndPointListener**. |
| **Protocol** | **TCP** |
| **Poort** | *1433*   |
| **Back-end-poort** | *1433*. Houd er rekening mee dat dit wordt uitgeschakeld omdat deze regel wordt gebruikt voor **Zwevende IP (directe server return)**.   |
| **Sonde** | Gebruik de naam van de sonde die u hebt gemaakt voor de verdeling van de belasting. |
| **Persistentie van de sessie**  | **Geen** | 
| **Time-out voor inactiviteit (minuten)**  | *4* | 
| **Zwevende IP (directe server return)**  | **Ingeschakeld** | 

 >[AZURE.NOTE] Mogelijk moet omlaag schuiven in het blad voor een overzicht van alle instellingen.

- Klik op **OK**. 

- Azure configureert u de regel voor de taakverdeling. Nu is de taakverdeling geconfigureerd voor het routeren van netwerkverkeer naar de SQL-Server die host is van de listener voor de groep van de beschikbaarheid. 

Op dit moment heeft de bronnengroep een taakverdeling verbinding te maken met zowel SQL Server-machines. De taakverdeling bevat ook een IP-adres voor de SQL Server AlwaysOn beschikbaarheid groep listener zodat beide computers op aanvragen voor de van beschikbaarheidsgroepen reageren kan.

>[AZURE.NOTE] Als uw SQL-Servers in twee afzonderlijke gebieden zijn, herhaalt u de stappen in de andere regio. Elke regio moet een taakverdeling. 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Configureer het cluster de load balancer IP-adres gebruiken 

De volgende stap is het configureren van de listener op het cluster en de listener on line brengen. U kunt dit doen door het volgende te doen: 

1. De listener beschikbaarheid groep op het failover-cluster maken 

1. De listener on line brengen

## <a name="1-create-the-availablity-group-listener-on-the-failover-cluster"></a>1. de groep beschikbaarheid listener op het failover-cluster maken

In deze stap moet u handmatig de listener beschikbaarheid groep in een failover-Cluster Manager en SQL Server Management Studio (SSMS) maken.

- Met RDP verbinding maken met de Azure virtuele machine die fungeert als host voor de primaire replica. 

- Failoverclusterbeheer openen.

- Selecteer het knooppunt voor **netwerken** en noteer de naam van het cluster. Deze naam wordt gebruikt in de `$ClusterNetworkName` variabele in het PowerShell-script.

- Vouw de naam van het cluster en klik vervolgens op **rollen**.

- Klik met de rechtermuisknop op de naam van de beschikbaarheid in het venster **rollen** en selecteer **Add Resource** > **Client Access Point**.

- In het vak **naam** een naam voor deze nieuwe listener en klik tweemaal op **volgende** en klik op **Voltooien**. Worden niet weergegeven in de bron on line of listener op dit punt.

 >[AZURE.NOTE] De naam voor de nieuwe listener is de naam van het netwerk waarmee toepassingen verbinding maken met databases in de groep van de beschikbaarheid van SQL Server.

- Klik op het tabblad **bronnen** en vouw de Client Access Point dat u zojuist hebt gemaakt. Klik met de rechtermuisknop op de IP-bron en klik op Eigenschappen. Noteer de naam van het IP-adres. Gebruikt u deze naam in de `$IPResourceName` variabele in het PowerShell-script.

- Op **Statische IP-adres** onder **IP-adres** en het statische IP-adres ingesteld op hetzelfde adres dat u gebruikt bij het instellen van het IP-adres van de load balancer op de Azure portal. NetBIOS inschakelen voor dit adres en klik op **OK**. Herhaal deze stap voor elke resource IP-als uw oplossing uit meerdere Azure VNets. 

- Op het knooppunt dat momenteel fungeert als host voor de primaire replica, een verhoogde PowerShell ISE open en plak de volgende opdrachten in een nieuw script.

        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    
        Import-Module FailoverClusters
    
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

- Bijwerken van de variabelen en het IP-adres en poort configureren voor de nieuwe listener PowerShell script uitvoeren.

 >[AZURE.NOTE] Als uw SQL-Servers in verschillende regio's, moet u tweemaal het PowerShell script uitvoeren. De eerste keer gebruikt de naam van het cluster, cluster-IP-resourcenaam, en IP-adres van de verdeling van de eerste groep laden. Naam van het cluster, cluster-IP-resourcenaam, gebruikt u de tweede keer en IP-adres van de verdeling van de tweede groep van bronnen laden.

Het cluster heeft nu een resource beschikbaarheid groep listener.

## <a name="2-bring-the-listener-online"></a>2. de listener on line brengen

Met de beschikbaarheid van listener groepsbron geconfigureerd, u kunt de listener on line brengen zodat toepassingen verbinding met databases in de beschikbaarheidsgroep met de listener maken kunnen.

- Ga terug naar Failoverclusterbeheer. Vouw **rollen** uit en selecteer vervolgens de groep beschikbaarheid. Klik met de rechtermuisknop op de naam van de listener en klik op **Eigenschappen**op het tabblad **bronnen** .

- Klik op het tabblad **afhankelijkheden** . Als er meerdere resources zijn opgenomen, Controleer of de IP-adressen hebt of niet en afhankelijkheden. Klik op **OK**.

- Klik met de rechtermuisknop op de naam van de listener en klik op **On line brengen**.


- Zodra de listener online, via het tabblad **bronnen is** , met de rechtermuisknop op de beschikbaarheidsgroep en klik op **Eigenschappen**.

- Maak een afhankelijkheid op bron (niet het IP-adres resources-naam) van de listener. Klik op **OK**.


- Start SQL Server Management Studio en verbinding maken met de primaire replica.


- Ga naar **AlwaysOn hoge beschikbaarheid** | **beschikbaarheidsgroepen** | **groep beschikbaarheid Listeners**. 


- U ziet nu de listener-naam die u hebt gemaakt in Failover-clusterbeheer. Klik met de rechtermuisknop op de naam van de listener en klik op **Eigenschappen**.


- Geef in het vak **poort** het poortnummer voor de listener beschikbaarheid groep met behulp van de $EndpointPort u eerder hebt gebruikt (1433 is de standaardinstelling), klikt u op **OK**.

U hebt nu een groep van de beschikbaarheid van SQL Server AlwaysOn in Azure virtuele machines die worden uitgevoerd in de modus voor resource manager. 

## <a name="test-the-connection-to-the-listener"></a>Test de verbinding met de listener

De verbinding testen:

1. RDP met een SQL-Server die zich in hetzelfde virtuele netwerk, maar is niet de eigenaar van de replica. Dit is de andere SQL Server in het cluster.

1. Hulpprogramma **sqlcmd** gebruiken om de verbinding te testen. Bijvoorbeeld het volgende script een verbinding tot stand **sqlcmd** naar de primaire replica via de listener met Windows-verificatie:

        sqlcmd -S <listenerName> -E

De SQLCMD verbinding wordt automatisch verbonden met het exemplaar van SQL Server fungeert als host voor de primaire replica. 

## <a name="guidelines-and-limitations"></a>Richtlijnen en beperkingen

Houd rekening met de volgende richtlijnen op beschikbaarheid groep listener in Azure met interne balancer laadt:

- Listener van de groep slechts één interne beschikbaarheid wordt per cloud-service ondersteund, omdat de listener is geconfigureerd voor de taakverdeling en er slechts één interne taakverdeling is. Het is echter mogelijk multipe externe listeners maken. 

- Met een interne taakverdeling u alleen toegang tot de listener uit binnen hetzelfde virtuele netwerk.
 
