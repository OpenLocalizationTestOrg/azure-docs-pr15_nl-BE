<properties
    pageTitle="Uitvoeren van de Hyper-V-Capaciteitsplanner voor herstel van de Site | Microsoft Azure"
    description="Dit artikel bevat instructies voor het gebruik van de Hyper-V-Capaciteitsplanner voor Azure Site herstellen"
    services="site-recovery"
    documentationCenter="na"
    authors="rayne-wiselman"
    manager="jwhit"
    editor="" />
<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/12/2016"
    ms.author="raynew" />

# <a name="run-the-hyper-v-capacity-planner-tool-for-site-recovery"></a>De Hyper-V-Capaciteitsplanner voor herstel van de Site uitvoeren

Als onderdeel van uw implementatie Azure Site herstel moet u de replicatie- en bandbreedtevereisten uitvinden. De Hyper-V-Capaciteitsplanner voor herstel van de Site helpt u bij het uitzoeken van uw vereisten voor replicatie en bandbreedte voor replicatie van Hyper-V virtuele machine.


In dit artikel wordt beschreven hoe de Capaciteitsplanner voor Hyper-V wordt uitgevoerd. Dit programma moet worden gebruikt samen met de andere planningshulpmiddelen capaciteit en informatie beschreven in [Capaciteitsplanning voor herstel van de Site](site-recovery-capacity-planner.md).


## <a name="before-you-start"></a>Voordat u begint

U kunt het hulpprogramma uitvoert op een knooppunt voor Hyper-V server of cluster in uw primaire site. Het hulpprogramma uitvoeren moet de Hyper-V host-servers:

- Besturingssysteem: Windows®-2012 Server of Windows Server® 2012 R2
- Geheugen: 20 MB (minimum)
- CPU: 5 procent-overhead (minimaal)
- Schijfruimte: 5 MB (minimum)

Voordat u het hulpprogramma uitvoert moet u de primaire site voorbereiden. Als u wilt repliceren tussen twee op-premises sites en bandbreedte te controleren, moet u een replicaserver voorbereiden.


## <a name="step-1-prepare-the-primary-site"></a>Stap 1: Voorbereiden van de primaire site
1. Maak een lijst van alle de Hyper-V virtuele machines die u wilt repliceren en Hyper-V hosts/clusters waarop ze zich bevinden op de primaire site. Het hulpprogramma kunt uitvoeren elke keer voor meerdere zelfstandige hosts of voor een cluster, maar niet beide samen. Deze moeten ook afzonderlijk voor elk besturingssysteem worden uitgevoerd zodat u moet verzamelen en Let op uw servers Hyper-V als volgt:

  - Windows Server® 2012 zelfstandige servers
  - Clusters met Windows Server® 2012
  - Windows Server® 2012 R2 zelfstandige servers
  - Windows Server® 2012 R2 clusters

3. Schakel RAS met WMI op de Hyper-V hosts en clusters. Deze opdracht uitvoeren op elke servercluster te controleren of firewall-regels maken en machtigingen zijn ingesteld:

        netsh firewall set service RemoteAdmin enable

5. Inschakelen met het controleren van de prestaties van servers en clusters, als volgt:

  - Open Windows Firewall met de module **Geavanceerde beveiliging** en vervolgens de volgende regels voor binnenkomende verbindingen inschakelt: **COM +-netwerktoegang (DCOM-IN)** en alle regels in de **groep Extern Event Log-beheer**.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Stap 2: Bereid een replicaserver (op-ruimten voor bedrijfsruimten replicatie)

U hoeft niet te doen als u naar Azure repliceren wilt.

Wij raden u één Hyper-V host als een herstelserver, zodat een dummy VM kan worden gerepliceerd naar deze bandbreedte te controleren.  Dit kunt u overslaan, maar u niet meer bandbreedte meten, tenzij u dit doen.

1. Als u een clusterknooppunt gebruiken wilt als de replica Replica Hyper-V broker configureren:

    - In **Server Manager**, moet u **Failoverclusterbeheer**openen.
    - Verbinding maken met het cluster, markeer de clusternaam van het en klik op **Acties** > **Rol configureren** voor het openen van de wizard maximale beschikbaarheid.
    - Klik op **Hyper-V Replica Broker**in **Rol selecteert** . Geef in de wizard een **NetBIOS-naam** en **IP-adres** moet worden gebruikt als het verbindingspunt aan het cluster (een client access point genoemd). De **Makelaar van Hyper-V-Replica** worden geconfigureerd, wat resulteert in een client access point name Houd er rekening mee.
    - Controleer of de functie Hyper-V Replica Broker komt online is en kan worden uitgevoerd tussen alle knooppunten van het cluster. Hiertoe klik met de rechtermuisknop op de rol, wijs **verplaatsen**en klik vervolgens op **Knooppunt selecteren**. Selecteer een knooppunt > **OK**.
    - Als u op certificaten gebaseerde verificatie gebruikt, moet elk clusterknooppunt en de client toegangspunt hebben het certificaat is geïnstalleerd.
2.  Een replicaserver inschakelen:

    - Voor een cluster Failure Cluster Manager openen, verbinding maken met het cluster en klik op **functies** > Selecteer rol > instellingen van de **Replicatie**> **dit cluster als een replicaserver inschakelen**. Houd er rekening mee dat als u een cluster als de replica moet u de functie Hyper-V Replica Broker aanwezig zijn op het cluster in de primaire site.
    - Open Hyper-V-beheer voor een zelfstandige server. In het deelvenster **Acties** op **Hyper-V-instellingen** voor de server die u wilt inschakelen en klik op **deze computer als een replicaserver inschakelen**in de **Configuratie van replicatie** .
3. Verificatie instellen:

    - Selecteer in de **poorten voor verificatie en** het verifiëren van de primaire server en de poorten voor verificatie. Als u het certificaat klikt u op **Certificaat selecteren** om een te selecteren. Kerberos gebruiken als de primaire en herstel Hyper-V hosts in hetzelfde domein of vertrouwde domeinen. Het gebruik van certificaten voor verschillende domeinen of de implementatie van een werkgroep.
    - In de sectie **verificatie en opslag** , zodat **een** geverifieerde (primaire) server om replicatiegegevens te verzenden naar deze replicaserver. Klik op **OK** of op **toepassen**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

    - Voer **netsh http servicestate weergeven** om te controleren of de listener wordt uitgevoerd voor het opgegeven protocol/poort:  
4. Instellen van firewalls. Firewall-regels zijn gemaakt tijdens de installatie van Hyper-V voor verkeer op de standaard poorten (HTTPS op 443, Kerberos op 80). Deze regels als volgt inschakelen:

        - Certificate authentication on cluster (443): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}**
        - Kerberos authentication on cluster (80): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}**
        - Certificate authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"**
        - Kerberos authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"**

## <a name="step-3-run-the-capacity-planner-tool"></a>Stap 3: De Capaciteitsplanner uitvoeren

Nadat u hebt opgesteld van de primaire site en een herstelserver instellen kunt u het hulpprogramma uitvoert.

1. [Download](https://www.microsoft.com/download/details.aspx?id=39057) het hulpprogramma van het Microsoft Download Center.
2. Het hulpprogramma uitvoeren vanaf een van de primaire servers (of een van de knooppunten van de primaire cluster). Klik met de rechtermuisknop op het .exe-bestand en kies **Als administrator uitvoeren**.
3. Opgeven hoe lang u wilt gegevens verzamelen in **voordat u begint** . U wordt aangeraden dat het hulpprogramma wordt uitgevoerd tijdens de productie-uren om ervoor te zorgen dat de gegevens representatief is. Als u alleen verbinding met het netwerk te valideren probeert, kunt u slechts een minuut.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. In de **primaire site details** geven de servernaam of de FQDN-naam voor een host die zelfstandig of voor een cluster, geeft de FQDN-naam van de client accepteert punt, de naam van het cluster of een willekeurig knooppunt in het cluster en klik op **volgende**. Het hulpprogramma detecteert automatisch de naam van de server die wordt uitgevoerd. Het gereedschap neemt de VMs die kunnen worden gecontroleerd voor de opgegeven servers.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. In **Replica Site Details** als u naar Azure repliceren wilt of als u naar een secundaire datacenter repliceren wilt en een replicaserver hebt ingesteld, selecteer **tests waarbij replica site overslaan**. Als u naar een secundaire datacenter repliceert en u een ReplicaType in de FQDN-naam van de zelfstandige server of de client-toegangspunt voor het cluster in de **naam van de Server (of) Hyper-V Replica Broker CAP**hebt ingesteld.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. In **Replica Details Extended** inschakelen **de proeven met betrekking tot uitgebreide Replica site overslaan**. Ze worden niet ondersteund door sites worden hersteld.
7. In **VMs kiezen om te repliceren** de extra verbinding maakt met de server of het cluster en VMs weergegeven en schijven op de primaire server, volgens de instellingen opgegeven op de pagina **Details van primaire Site** . Overigens VMs die al zijn ingeschakeld voor replicatie of die niet zijn uitgevoerd worden niet weergegeven. Selecteer het VMs waarvoor u wilt verzamelen metrics. Het automatisch selecteren van de VHD's worden gegevens verzameld voor het VMs te.
9. Als u een replicaserver of cluster hebt geconfigureerd, in **netwerkgegevens** opgeven de benaderende WAN-bandbreedte u denkt dat wordt gebruikt tussen de primaire en replica sites en selecteer de certificaten als u certificaatverificatie hebt geconfigureerd.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

10. In **Samenvatting** instellingen controleren en klik op **volgende** om te beginnen met het verzamelen van statistieken. Gereedschap Verloop en de status wordt weergegeven op de pagina van de **Capaciteit berekenen** . Wanneer het hulpprogramma is voltooid met klikt u op **Rapport weergeven** om te gaan over de uitvoer. Logboeken en rapporten worden standaard opgeslagen in **%systemdrive%\Users\Public\Documents\Capacity Planner**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)


## <a name="step-4-interpret-the-results"></a>Stap 4: De resultaten interpreteren
Hier vindt u de belangrijke maatstaven. Maatstaven die hier niet worden weergegeven, kunt u negeren. Ze zijn niet relevant voor het herstel van de Site.

### <a name="on-premises-to-on-premises-replication"></a>In ruimten voor replicatie voor gebouwen
  - Gevolgen van de replicatie op de primaire host compute, geheugen
  - Gevolgen van de replicatie op de primaire opslag schijfruimte herstel hosts van IOP 's
  - Totale bandbreedte nodig is voor replicatie delta (Mbps)
  - Waargenomen netwerkbandbreedte tussen de primaire host en het herstel (Mbps)
  - Suggestie voor het ideale aantal actieve parallelle overdrachten tussen de twee hosts/clusters

### <a name="on-premises-to-azure-replication"></a>Over lokalen Azure replicatie
  - Gevolgen van de replicatie op de primaire host compute, geheugen
  - Gevolgen van de replicatie van de primaire host opslag schijfruimte, IOP 's
  - Totale bandbreedte nodig is voor replicatie delta (Mbps)

## <a name="more-resources"></a>Meer bronnen

- Lees het document bij het downloaden van het hulpprogramma voor gedetailleerde informatie over het hulpprogramma.
- Bekijk een overzicht van het programma op Keith Mayer van [TechNet-blog](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
- [De resultaten](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) van onze prestatietest voor op-premises Hyper-V-replicatie voor gebouwen



## <a name="next-steps"></a>Volgende stappen

Nadat u klaar bent met het plannen van capaciteit kunt u starten met het herstel Site implementeren:

- [Hyper-V VMs in wolken VMM repliceren naar Azure](site-recovery-vmm-to-azure.md)
- [Hyper-V VMs (zonder VMM) repliceren naar Azure](site-recovery-hyper-v-site-to-azure.md)
- [Hyper-V VMs te repliceren tussen sites VMM](site-recovery-vmm-to-vmm.md)
- [Hyper-V VMs te repliceren tussen sites met SAN VMM](site-recovery-vmm-san.md)
- [Hyper-V VMs op één VMM server repliceren](site-recovery-single-vmm.md)