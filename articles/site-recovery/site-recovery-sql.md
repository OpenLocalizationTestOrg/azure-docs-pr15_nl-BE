<properties 
    pageTitle="Beveiligen van SQL Server met SQL Server disaster recovery en herstel van Azure-Site | Microsoft Azure" 
    description="Dit artikel wordt beschreven hoe u SQL Server met de mogelijkheden van Azure Site herstellen van SQL Server disaster repliceren." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.workload="backup-recovery" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2016" 
    ms.author="raynew"/>


# <a name="protect-sql-server-with-sql-server-disaster-recovery-and-azure-site-recovery"></a>Beveiligen van SQL Server met SQL Server disaster recovery en Azure Site herstellen 


De Azure-Site-Recovery-service draagt bij aan de continuïteit en noodherstel recovery (BCDR) bedrijfsstrategie door regie van replicatie, failover en herstel van virtuele machines en fysieke servers. Machines kunnen worden gerepliceerd naar Azure, of naar een secundaire op ruimten Datacenter. Voor een snel overzicht lezen [Wat is Azure Site terugzetten?](site-recovery-overview.md).

 In dit artikel wordt beschreven hoe ter bescherming van de SQL Server-back-end van een toepassing met een combinatie van BCDR van SQL Server-technologieën en Azure Site herstellen. Er is een goed begrip van de mogelijkheden van SQL Server disaster recovery (failover-clustering, AlwaysOn beschikbaarheidsgroepen, database-mirroring, meld verzending) en Azure Site herstellen voordat u de in dit artikel beschreven scenario's implementeert.



## <a name="overview"></a>Overzicht

Vele werklasten SQL Server gebruikt als basis. Toepassingen zoals SharePoint en Dynamics, SAP gebruiken SQL Server data services implementeren.  SQL Server implementeren toepassingen in een aantal verschillende manieren:

- **Zelfstandige SQL Server**: SQL Server en alle databases worden gehost op een enkele computer (fysieke of een virtueel). Gevirtualiseerde, wordt clustering host gebruikt voor lokale beschikbaarheid. Geen hoge beschikbaarheid van de Gast-niveau wordt geïmplementeerd.
- **SQL Server Failover-Clustering-exemplaren (altijd op FCI)**: twee of meer knooppunten van exemplaren van SQL server met gedeelde schijven in een Windows-failover-cluster zijn geconfigureerd. Als een van de exemplaren van het cluster is uitgeschakeld, kan het cluster naar een ander exemplaar over SQL Server mislukken. Deze instelling wordt meestal gebruikt voor HA op een primaire site. Het biedt geen bescherming tegen storingen of storing in de laag met gedeelde opslag. Gedeelde schijf kan worden geïmplementeerd met ISCSI, Fibre channel of gedeelde VHDx.
- **SQL altijd op beschikbaarheidsgroepen**: In deze instelling, worden twee knooppunten instellen in de gedeelde niets met SQL Server-databases die zijn geconfigureerd in een beschikbaarheidsgroep met synchrone replicatie en automatische failover-cluster.

In Enterprise-edities biedt SQL Server ook native disaster recovery technologieën voor het herstellen van databases naar een externe site. In dit artikel, we benutten en integreren deze native SQL disaster recovery-technologieën: 

- SQL altijd op beschikbaarheidsgroepen voor herstel na noodgevallen voor SQL Server 2012 of 2014 Enterprise Edition.
- SQL database-mirroring in de modus hoge beveiliging voor SQL Server Standard edition (alle versies) of SQL Server 2008 R2.


Herstel van de site kan SQL Server kunt beveiligen zoals samengevat in de tabel.

 |**Voor ruimten tot op de gebouwen** | **On-premises tot Azure** 
---|---|---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja 
**Fysieke server** | Ja | Ja


## <a name="support-and-integration"></a>Ondersteuning en integratie

Deze versies van SQL Server worden ondersteund door de scenario's in dit artikel:


- SQL Server 2014 Enterprise en standaard
- SQL Server 2012 Enterprise en standaard
- SQL Server 2008 R2 Enterprise en standaard


Herstel van de site kan worden geïntegreerd met native SQL Server BCDR technologieën die zijn samengevat in de tabel hieronder als u wilt een disaster recovery oplossing bieden.

**Functie** |**Details** | **SQL Server-versie** 
---|---|---
**AlwaysOn-beschikbaarheidsgroep** | Meerdere zelfstandige exemplaren van SQL Server wordt uitgevoerd in een failover-cluster met meerdere knooppunten.<br/><br/>Databases kunnen worden gegroepeerd in failover-groepen die kunnen worden gekopieerd (gespiegeld) op exemplaren van SQL Server, zodat er geen gedeelde opslag nodig is.<br/><br/>Herstel na noodgevallen tussen een primaire en een of meer secundaire sites biedt. Twee knooppunten kunnen worden ingesteld in een gedeelde niets cluster met SQL Server-databases in een beschikbaarheidsgroep met synchrone replicatie en automatische failover worden geconfigureerd. | SQL Server 2014 & 2012 Enterprise edition
**Failover-clusters (AlwaysOn FCI)** | SQL Server maakt gebruik van Windows voor failover clustering voor maximale beschikbaarheid van SQL Server een standaardwerkbelasting voor gebouwen.<br/><br/>Knooppunten met exemplaren van SQL Server met gedeelde schijven zijn in een failover-cluster geconfigureerd. Als een instantie naar beneden is het cluster failover wordt uitgevoerd naar andere.<br/><br/>Het cluster beschermd niet tegen storingen of storingen in gedeelde opslag. De gedeelde schijf kan worden geïmplementeerd met iSCSI, Fibre channel, of gedeelde VHDXs. | SQL Server Enterprise-edities<br/><br/>SQL Server Standard edition (beperkt tot slechts twee knooppunten)
**Database-mirroring (modus voor hoge veiligheid)** | Een enkele database naar een enkele kopie van de secundaire beschermt. Beschikbaar in zowel hoge veiligheid (synchroon) en hoge prestaties (asynchroon) replicatie modi. Niet nodig een failover-cluster. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise alle edities
**Zelfstandige SQL Server** | De SQL Server en de database worden gehost op een enkele server (fysiek of virtueel). Host clustering wordt gebruikt voor hoge beschikbaarheid, als de server virtueel is. Geen Gast-niveau met hoge beschikbaarheid. | Enterprise of Standard edition

## <a name="deployment-recommendations"></a>Aanbevelingen voor de implementatie


Deze tabel bevat een overzicht van onze aanbevelingen voor de integratie van SQL Server BCDR technologieën met terugwinning van de Site.

**Versie** |**Edition** | **Implementatie** | **Op prem aan op prem** | **Op prem naar Azure** 
---|---|---|---|---
SQL Server 2014 of 2012 | Enterprise | Instantie van failover-cluster | AlwaysOn beschikbaarheidsgroepen | AlwaysOn beschikbaarheidsgroepen
 | Enterprise | AlwaysOn beschikbaarheidsgroepen voor hoge beschikbaarheid | AlwaysOn beschikbaarheidsgroepen | AlwaysOn beschikbaarheidsgroepen
 | Standaard | Failover-clusterexemplaar (FCI) | Site-replicatie met lokale mirror herstel | Site-replicatie met lokale mirror herstel
 | Enterprise of Standard | Zelfstandige | Replicatie van site-herstel | Replicatie van site-herstel
SQL Server 2008 R2 | Enterprise of Standard | Failover-clusterexemplaar (FCI) | Site-replicatie met lokale mirror herstel | Site-replicatie met lokale mirror herstel
 | Enterprise of Standard | Zelfstandige | Replicatie van site-herstel | Replicatie van site-herstel
SQL Server (elke versie) | Enterprise of Standard | Failover-cluster instance - DTC-toepassing | Replicatie van site-herstel | Niet ondersteund


## <a name="deployment-prerequisites"></a>Vereisten voor implementatie

Hier is wat u nodig hebt voordat u begint:

- Een op ruimten SQL Server-implementatie met een ondersteunde versie van SQL Server. Meestal hebt u ook een Active Directory nodig voor uw SQL server.
- De vereisten voor het scenario dat u wilt implementeren. Vereisten vindt u in elk artikel distributie. Koppelingen naar deze zijn beschikbaar in het [Site-overzicht](site-recovery-overview.md).
- Als u wilt instellen van de herstelconsole in Azure, moet u het hulpprogramma [Evaluatie van gereedheid voor Azure Virtual Machine](http://www.microsoft.com/download/details.aspx?id=40898) op uw virtuele machines van SQL Server om te controleren of dat ze compatibel zijn met Azure en herstel van de Site zijn uitgevoerd.


## <a name="set-up-active-directory"></a>Active Directory instellen

U moet Active Directory op de website van secundaire herstel voor SQL Server worden uitgevoerd. Er zijn een aantal opties:

- **Kleine onderneming**, als een klein aantal toepassingen en één domeincontroller voor de site op het bedrijf en u wilt de hele site failover wordt aangeraden te gebruiken voor het herstel van de Site repication de domeincontroller worden gerepliceerd naar de secundaire datacenter of Azure.

- **Middelgrote tot grote onderneming**, als u beschikt over een groot aantal toepassingen, wordt uitgevoerd een Active Directory-forest en u wilt niet door de toepassing of de werkbelasting, raden we aan u een extra domeincontroller in het tweede datacenter of Azure instellen. Houd er rekening mee dat als u met behulp van AlwaysOn beschikbaarheidsgroepen te herstellen naar een externe site raden we aan dat een andere domeincontroller op de secundaire site of Azure, extra instellen voor de herstelde SQL Server-exemplaar.

De instructies in dit document wordt ervan uitgegaan dat er een domeincontroller beschikbaar op de secundaire locatie is. [Lees meer](site-recovery-active-directory.md) over het beveiligen van Active Directory met terugwinning van de Site.

## <a name="integrate-protection-with-sql-server-always-on-on-premises-to-azure"></a>Bescherming integratie met SQL Server Always-On (op-ruimten naar Azure)


Site-Recovery ondersteunt SQL AlwaysOn. Als u een SQL-servergroep beschikbaarheid met een Azure virtuele machine instellen als 'Secundaire' kunt u de Site herstellen gebruiken voor het beheren van de overname van de beschikbaarheid van groepen hebt gemaakt. 

>[AZURE.NOTE] Deze functionaliteit is momenteel in voorvertoning en beschikbaar wanneer de host-servers Hyper-V in het primaire datacenter worden beheerd in VMM wolken en VMware setup wordt beheerd door een [Server Configuration](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Op dit moment deze mogelijkheid niet beschikbaar in de nieuwe portal Azure is.

#### <a name="prerequisites"></a>Vereisten

Dit is wat u moet SQL AlwaysOn integreren met terugwinning van de Site:

- Een in de lokalen SQL Server (zelfstandige server of een failover-cluster).
- Een of meer Azure virtuele machines met SQL Server is geïnstalleerd
- Een groep van de beschikbaarheid van SQL instellen tussen een lokalen op SQL Server en SQL Server wordt uitgevoerd in Azure
- PowerShell remoting moet zijn ingeschakeld op de SQL Server-computer op gebouwen. De VMM-server of de Server-configuratie moet kunnen externe PowerShell gesprekken met de SQL Server.
- Een gebruikersaccount worden op de SQL Server op de gebouwen, in deze SQL-gebruikersgroepen met ten minste deze machtigingen toegevoegd:
    - ALTER beschikbaarheid: groepsmachtigingen [hier](https://msdn.microsoft.com/library/hh231018.aspx)en [hier](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - ALTER DATABASE - machtigingen[hier](https://msdn.microsoft.com/library/ff877956.aspx#Security)
- Een RunAs-account moet worden gemaakt op VMM-Server of een account moet worden gemaakt op de Server van de configuratie met de CSPSConfigtool.exe voor de gebruiker die wordt vermeld in de vorige stap 
- De module SQL PS moet worden geïnstalleerd in lokalen met SQL-Servers en Azure virtuele machines
- De VM-Agent moet worden geïnstalleerde virtuele machines die worden uitgevoerd op Azure
- NTAUTHORITY\System moet na de machtigingen in SQL Server wordt uitgevoerd op virtuele machines in Azure:
    - ALTER beschikbaarheid groep - machtigingen [hier](https://msdn.microsoft.com/library/hh231018.aspx)en [hier](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - ALTER DATABASE - machtigingen [hier](https://msdn.microsoft.com/library/ff877956.aspx#Security)

####  <a name="step-1-add-a-sql-server"></a>Stap 1: Een SQL-Server toevoegen


1. Klik op **SQL toevoegen** om een nieuwe SQL Server toevoegen. 

    ![SQL toevoegen](./media/site-recovery-sql/add-sql.png)

2. In de **SQL-instellingen configureren** > **naam** een beschrijvende naam verwijzen naar de SQL-Server opgeven.
3. **In SQL Server (Fully Qualified Domain Name)** opgeven voor de FQDN-naam van de bron van SQL Server die u wilt toevoegen. De SQL-Server wordt geïnstalleerd op een failover-Cluster, geeft u FQDN-naam van het cluster en niet een van de clusterknooppunten.  
4. Kies het standaardexemplaar of de naam opgeven van de aangepaste exemplaar in **SQL Server-exemplaar** .
5. Selecteer een VMM of configuratieserver geregistreerd in de kluis Site herstellen in **Management Server** . Deze server Management Recovery site gebruikt om te communiceren met de SQL-Server.
6. Geef de naam van een RunAs-account die is gemaakt op de opgegeven VMM-server of de Account die is gemaakt op de Configuraaaon Server uitvoeren **als een Account** . Deze rekening wordt gebruikt voor toegang tot de SQL-Server en de machtigingen lezen en Failover moet hebben op de van beschikbaarheidsgroepen op de computer met SQL Server.

    ![SQL-dialoogvenster toevoegen](./media/site-recovery-sql/add-sql-dialog.png)

Nadat u de SQL-Server toevoegen verschijnt op het tabblad **SQL-Servers** . 

![Lijst met SQL Server](./media/site-recovery-sql/sql-server-list.png)


#### <a name="step-2-add-a-sql-availability-group"></a>Stap 2: Een beschikbaarheid van SQL-servergroep toevoegen

1. Machine toegevoegd na de SQL-Server dat de volgende stap is de beschikbaarheid van groepen toevoegen aan sites worden hersteld. Hiertoe inzoomen in de SQL-Server in de vorige stap hebt toegevoegd en klik op SQL beschikbaarheid groep toevoegen. 

    ![SQL AG toevoegen](./media/site-recovery-sql/add-sqlag.png)

2. SQL-servergroep beschikbaarheid kan worden gerepliceerd naar een of meer virtuele machines in Azure. Wanneer de sql beschikbaarheid-groep die bent u verplicht de naam en het abonnement van Azure virtual machine waar u de beschikbaarheidsgroep worden overgenomen door het herstel van de Site wilt toevoegen.

    ![Dialoogvenster SQL AG toevoegen](./media/site-recovery-sql/add-sqlag-dialog.png)

3. In het bovenstaande voorbeeld beschikbaarheid groep DB1-AG zou worden primair op de virtuele machine SQLAGVM2 binnen DevTesting2 abonnement op een failover uitgevoerd. 

>[AZURE.NOTE] Alleen de beschikbaarheid van groepen die zich primair op de SQL-Server die is toegevoegd in stap hierboven zijn beschikbaar voor herstel van de Site worden toegevoegd. Als u een primaire beschikbaarheid-groep hebt gemaakt op de SQL Server of als u meer groepen van beschikbaarheid op de SQL-Server hebt toegevoegd nadat deze is toegevoegd, kunt u deze met de optie vernieuwen beschikbaar op de SQL-Server vernieuwen.

#### <a name="step-3-create-a-recovery-plan"></a>Stap 3: Maak een herstelplan

De volgende stap is het maken van een herstelplan met behulp van virtuele machines en de van beschikbaarheidsgroepen. Selecteer dezelfde VMM Server of Server-configuratie die u hebt gebruikt in stap 1 als bron en Microsoft Azure als doel.

![Herstelplan maken](./media/site-recovery-sql/create-rp1.png)

![Herstelplan maken](./media/site-recovery-sql/create-rp2.png)

In het voorbeeld wordt de Sharepoint-toepassing bestaat uit 3 virtuele machines die een beschikbaarheid van SQL-servergroep als de backend. In dit herstelplan dat kan zowel de beschikbaarheidsgroep en u selecteert de virtuele machine vormen, de toepassing. 

U kunt het herstelplan verder aanpassen door het verplaatsen van virtuele machines aan andere failover-groepen aan het volgnummer van de volgorde voor failover. Beschikbaarheidsgroep is altijd over eerste mislukt als deze zou worden gebruikt als een back-end van een toepassing. 

![Herstelplan aanpassen](./media/site-recovery-sql/customize-rp.png)

### <a name="step-4--fail-over"></a>Stap 4: Failover

Andere failover-opties zijn beschikbaar wanneer u een groep beschikbaarheid hebt toegevoegd aan een herstelplan.

Failover | Details
--- | ---
**Geplande failover** | Geplande Failover houdt in dat er geen gegevens verloren gaan failover. Om te bereiken dat synchroon eerst SQL beschikbaarheid groep beschikbaarheid modus wordt ingesteld en vervolgens een failover wordt geactiveerd als u wilt dat de groep primaire beschikbaarheid op de virtuele machine geleverd tijdens het toevoegen van de beschikbaarheidsgroep voor herstel van de Site. Nadat de failover uitgevoerd is, worden beschikbaarheid-modus is ingesteld op dezelfde waarde als het was voordat de geplande overname is geactiveerd.
**Niet-geplande failover** | Niet-geplande Failover kan resulteren in gegevensverlies. Tijdens het activeren van niet-geplande failover de beschikbaarheid van de groep van de beschikbaarheid is niet gewijzigd en de it primair op de virtuele machine geleverd tijdens het toevoegen van de beschikbaarheidsgroep voor herstel van de Site is gemaakt. Als niet-geplande failover uitgevoerd is en de op het bedrijf server met SQL Server weer beschikbaar is, is replicatie omkeren worden geactiveerd op de groep van de beschikbaarheid. Houd er rekening mee dat deze actie niet beschikbaar op het herstelplan is en kan worden genomen op de beschikbaarheid van SQL-servergroep onder tabblad SQL Servers
**Test-failover** | Failover voor de groep van de beschikbaarheid van SQL test wordt niet ondersteund. Als u trigger Test Failover van een herstelplan met beschikbaarheid van SQL-servergroep, zou failover voor de groep beschikbaarheid worden overgeslagen.


U kunt deze opties failover.

Optie | Details
--- | ---
**Optie 1** | 1. Voer een failover testen van de toepassing en het front-lagen.<br/><br/>2. de toepassingslaag als u de replica-exemplaar in de modus alleen-lezen toegang wilt bijwerken en testen van de toepassing alleen-lezen.
**Optie 2** | 1. Maak een kopie van de replica SQL Server virtuele machine-exemplaar (met VMM kloon van site naar site of Azure back-up) en brengen in een testnetwerk<br/><br/> 2. test met het herstelplan failover uitvoeren.

Stap 5: Een failback

Als u wilt de groep beschikbaarheid opnieuw primair maken op de SQL-Server op het bedrijf kunt vervolgens u doen door Failover gepland op het herstelplan te activeren en de richting van Microsoft Azure voor bedrijfsruimten VMM server kiezen.

>[AZURE.NOTE] Nadat u een niet-geplande failover reverse replicatie heeft op de beschikbaarheid van groep hervatten de replicatie worden geactiveerd. Totdat dit de replicatie blijft geschorst wordt gedaan.



### <a name="protect-machines-without-a-vmm-server-or-a-configuration-server"></a>Beveiligen van computers zonder een VMM-Server of een Server Configuration

De omgevingen worden niet beheerd door een VMM-Server of een Server configuratie kan Azure automatisering Runbooks worden gebruikt voor het configureren van een geautomatiseerde failover beschikbaarheid van SQL. Hieronder volgen de stappen voor het configureren van die:

1.  Een lokaal bestand voor het script dat failover een beschikbaarheidsgroep maken. Dit voorbeeldscript geeft een pad naar de beschikbaarheidsgroep op de Azure replica en failover naar dit replica-exemplaar. Dit script wordt uitgevoerd op de SQL-Server die replica virtuele machine door door te geven met de extensie aangepast script is.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2.  Het script uploaden naar een blob in een account Azure opslag. In dit voorbeeld gebruikt:

        $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key"
        Set-AzureStorageBlobContent -Blob "AGFailover.ps1" -Container "script-container" -File "ScriptLocalFilePath" -context $context

3.  Maak een Azure automatisering runbook om aan te roepen de scripts op de SQL Server-replica VM in Azure. Dit voorbeeldscript gebruiken om dit te doen. [Meer informatie](site-recovery-runbook-automation.md) over het gebruik van automatisering runbooks in de herstelplannen. 

        workflow SQLAvailabilityGroupFailover
        {
            param (
                [Object]$RecoveryPlanContext
            )

            $Cred = Get-AutomationPSCredential -name 'AzureCredential'
    
            #Connect to Azure
            $AzureAccount = Add-AzureAccount -Credential $Cred
            $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
            Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    
            InLineScript
            {
            #Update the script with name of your storage account, key and blob name
            $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key";
            $sasuri = New-AzureStorageBlobSASToken -Container "script-container"- Blob "AGFailover.ps1" -Permission r -FullUri -Context $context;
     
            Write-output "failovertype " + $Using:RecoveryPlanContext.FailoverType;
               
            if ($Using:RecoveryPlanContext.FailoverType -eq "Test")
                {
                #Skipping TFO in this version.
                #We will update the script in a follow-up post with TFO support
                Write-output "tfo: Skipping SQL Failover";
                }
            else
                {
                Write-output "pfo/ufo";
                #Get the SQL Azure Replica VM.
                #Update the script to use the name of your VM and Cloud Service
                $VM = Get-AzureVM -Name "SQLAzureVM" -ServiceName "SQLAzureReplica";     
       
                Write-Output "Installing custom script extension"
                #Install the Custom Script Extension on teh SQL Replica VM
                Set-AzureVMExtension -ExtensionName CustomScriptExtension -VM $VM -Publisher Microsoft.Compute -Version 1.3| Update-AzureVM; 
                    
                Write-output "Starting AG Failover";
                #Execute the SQL Failover script
                #Pass the SQL AG path as the argument.
       
                $AGArgs="-SQLAvailabilityGroupPath sqlserver:\sql\sqlazureVM\default\availabilitygroups\testag";
       
                Set-AzureVMCustomScriptExtension -VM $VM -FileUri $sasuri -Run "AGFailover.ps1" -Argument $AGArgs | Update-AzureVM;
       
                Write-output "Completed AG Failover";

                }
        
            }
        }

4.  Bij het maken van een herstelplan voor de toepassing een op 'vooraf groep 1 boot' script stap die roept de runbook automatisering failover beschikbaarheidsgroepen toevoegen.

## <a name="integrate-protection-with-sql-alwayson-on-premises-to-on-premises"></a>Bescherming integreren met SQL AlwaysOn (op-bedrijven om op ruimten)

Als de SQL-Server beschikbaarheidsgroepen voor hoge beschikbaarheid of de instantie van een failover-cluster gebruikt, kunt u het beste beschikbaarheidsgroepen op de herstel-site gebruiken. Let erop dat deze richtlijnen voor toepassingen die geen van gedistribueerde transacties gebruikmaken.

1. [Databases configureren](https://msdn.microsoft.com/library/hh213078.aspx) in beschikbaarheidsgroepen.
2. Maak een nieuwe virtuele netwerk op secundaire site.
3. Instellen van een site naar site VPN tussen het nieuwe virtuele netwerk en de primaire site.
4. Een virtuele machine op de herstel-site maken en installeren van SQL Server op deze.
5. Uitbreiden van de bestaande groepen AlwaysOn beschikbaarheid op de nieuwe virtuele machine voor SQL Server. Dit exemplaar van SQL Server configureren als een asynchrone replica-exemplaar.
6. Een listener beschikbaarheid groep maken of bijwerken van de bestaande listener om op te nemen van de asynchrone replica virtuele machine.
7. Zorg ervoor dat de toepassing farm instellen met behulp van de listener. Als deze ingesteld met behulp van de naam van de databaseserver is, werk bij het gebruik van de listener zodat u niet hoeft te configureren dat na de failover.

Voor toepassingen die gebruikmaken van gedistribueerde transacties we u [Site-herstel met SAN-replicatie](site-recovery-vmm-san.md) of [replicatie van site naar site server VMWare/fysieke](site-recovery-vmware-to-vmware.md)aanbeveling.

### <a name="recovery-plan-considerations"></a>Overwegingen bij het Recovery plan

1. Dit voorbeeld van een script toevoegen aan de bibliotheek VMM op de primaire en secundaire sites.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2. Bij het maken van een herstelplan voor de toepassing een op 'vooraf groep 1 boot' script stap die activeert het script failover beschikbaarheidsgroepen toevoegen.



## <a name="protect-a-standalone-sql-server"></a>Een zelfstandige SQL Server beveiligen

In deze configuratie wordt aangeraden dat u Recovery Site-replicatie gebruikt ter bescherming van de SQL Server-computer. De exacte procedure is afhankelijk van of SQL Server als een virtuele machine of fysieke server is ingesteld en u wilt repliceren naar Azure of een secundair op-premises site. Instructies voor alle implementatiescenario's krijgen in het [Site-overzicht](site-recovery-overview.md).


## <a name="protect-a-sql-server-cluster-standard-or-2008-r2"></a>Beveiligen van een SQL Server-cluster (standaard of 2008 R2)

Voor een cluster met SQL Server Standard edition of SQL Server 2008 R2 is het raadzaam dat u replicatie van Site-herstel gebruiken om SQL Server te beveiligen.

### <a name="on-premises-to-on-premises"></a>Voor ruimten tot op de gebouwen

- Als de toepassing maakt gebruik van gedistribueerde transacties raden wij dat u [Recovery Site met SAN-replicatie](site-recovery-vmm-san.md) implementeren voor een omgeving met Hyper-V en [VMware/fysieke server VMware](site-recovery-vmware-to-vmware.md) VMware omgeving.

- Voor niet-DTC-toepassingen maken gebruik van deze methode om te herstellen van het cluster als een zelfstandige server door gebruik te maken van een lokale hoge veiligheid DB mirror.

### <a name="on-premises-to-azure"></a>On-premises tot Azure

Herstel van de site ondersteunt geen clusterondersteuning gast bij het repliceren van Azure. SQL Server ook bieden niet een goedkope disaster recovery oplossing voor Standard edition. Het is raadzaam om u de SQL Server-cluster op ruimten aan een zelfstandige SQL Server beveiligen en herstellen in Azure.


1. Een zelfstandige aanvullende SQL Server-exemplaar configureren op de site van gebouwen.
2. Configureer deze instantie fungeren als een mirror voor de databases die u wilt beveiligen. Configureer de spiegeling in de modus hoge veiligheid.
3.  Recovery-Site configureren op de site van gebouwen is gebaseerd op het milieu ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) of [VMware/fysieke server](site-recovery-vmware-to-azure-classic.md).
4.  Herstel Site replicatie gebruiken voor de nieuwe SQL Server-exemplaar repliceren naar Azure. Is een hoge veiligheid mirrorkopie en dus moet worden gesynchroniseerd met de primaire cluster, maar je gegevens worden gerepliceerd naar Azure Recovery Site-replicatie.

De volgende afbeelding ziet u deze installatie.

![Standaard cluster](./media/site-recovery-sql/BCDRStandaloneClusterLocal.png)


### <a name="failback-considerations"></a>Failback-overwegingen

Na een niet-geplande failover failback-bewerking wordt voor een standaard SQL-clusters vereisen een back-up van SQL en van de Mirror-exemplaar terugzetten naar het oorspronkelijke cluster en het herstellen van de mirror.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie](site-recovery-best-practices.md) over het verkrijgen van Recovery-Site te implementeren.










 