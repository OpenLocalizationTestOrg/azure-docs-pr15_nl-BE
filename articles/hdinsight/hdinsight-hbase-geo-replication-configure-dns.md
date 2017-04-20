<properties 
   pageTitle="DNS configureren tussen twee Azure virtuele netwerken | Microsoft Azure" 
   description="Meer informatie over het configureren van VPN-verbindingen en domeinnamen omzetten tussen twee virtuele netwerken, en het HBase geo-replicatie configureren." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-dns-between-two-azure-virtual-networks"></a>DNS configureren tussen twee Azure virtuele netwerken

> [AZURE.SELECTOR]
- [VPN-verbindingen configureren](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [DNS configureren](hdinsight-hbase-geo-replication-configure-dns.md)
- [HBase replicatie configureren](hdinsight-hbase-geo-replication.md) 


Informatie over het toevoegen en configureren van DNS-servers met Azure virtuele netwerken voor het verwerken van naamomzetting binnen en tussen de virtuele netwerken.

Deze zelfstudie is het tweede deel van de [reeks] [ hdinsight-hbase-geo-replication] over het maken van HBase geo-replicatie:

- [Een VPN-verbinding tussen twee virtuele netwerken configureren][hdinsight-hbase-geo-replication-vnet]
- DNS configureren voor de virtuele netwerken (deze zelfstudie)
- [HBase geo-replicatie configureren][hdinsight-hbase-geo-replication]


In het volgende diagram ziet u de twee virtuele netwerken die u hebt gemaakt in [een VPN-verbinding tussen twee virtuele netwerken configureren][hdinsight-hbase-geo-replication-vnet]:

![HDInsight HBase replicatie virtuele netwerkdiagram][img-vnet-diagram]

##<a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Een workstation met Azure PowerShell**.

    Voordat u PowerShell scripts, moet dat u verbonden bent met uw Azure-abonnement met de volgende cmdlet:

        Add-AzureAccount

    Als er meerdere Azure abonnementen, gebruikt u de volgende cmdlet instellen van het huidige abonnement:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Virtueel netwerk met een VPN-verbinding met twee Azure**.  Zie [een VPN-verbinding tussen twee Azure virtuele netwerken configureren]voor meer informatie[hdinsight-hbase-geo-replication-vnet].

>[AZURE.NOTE] Servicenamen van Azure en virtuele machine moet uniek zijn. De naam die wordt gebruikt in deze zelfstudie wordt Contoso-[naam van Azure Service/VM]-[EU / VS]. Contoso VNet EU is bijvoorbeeld de Azure virtueel netwerk in het datacenter van Noord-Europa; Contoso-DNS-US is de DNS-server VM in het datacenter van Oost-VS. U moet afkomstig zijn van uw eigen namen.
 
 
##<a name="create-azure-virtual-machines-to-be-used-as-dns-servers"></a>Azure virtuele machines worden gebruikt als DNS-servers maken

**Voor het maken van een virtuele machine binnen Contoso VNet EU, genaamd Contoso-DNS-EU**

1.  Klik op **Nieuw**, **berekenen**, **virtuele MACHINE**, **FROM GALERIE**.
2.  Kies **Windows Server 2012 R2 Datacenter**.
3.  Voer in:
    - **Naam van de virtuele MACHINE**: Contoso-DNS-EU
    - **Nieuwe gebruikersnaam**: 
    - **Nieuw wachtwoord**: 
4.  Voer in:
    - **CLOUD SERVICE**: een nieuwe wolk service maken
    - **Regio/AFFINITEIT groep/VIRTUEEL netwerk**: (Selecteer de Contoso-VNet-EU)
    - **VIRTUEEL NETWERKSUBNETTEN**: Subnet 1
    - **Opslag rekening**: een automatisch gegenereerde opslag gebruikt.
    
        De naam van de wolk is hetzelfde als de naam van de virtuele machine. In dit geval is dat DNS Contoso EU. Ik kan kiezen voor latere virtuele machines, gebruiken dezelfde cloud-service.  De virtuele machines onder dezelfde cloud service delen hetzelfde virtuele netwerk en het domeinachtervoegsel.

        De rekening van de opslag wordt gebruikt voor het opslaan van het afbeeldingsbestand voor de virtuele machine. 
    - **EINDPUNTEN**: (Schuif omlaag en selecteer **DNS**) 

Nadat u de virtuele machine hebt gemaakt, ontdek het interne IP-adres en het externe IP-adres.

1.  Klik op de naam van de virtuele machine, **Contoso-DNS-EU**.
2.  Klik op het **DashBoard**.
3.  Opschrijven:
    - OPENBARE VIRTUEEL IP-ADRES
    - INTERNE IP-ADRES


**Voor het maken van een virtuele machine binnen Contoso VNet VS, genaamd Contoso-DNS-US** 

- Herhaal dezelfde procedure om een virtuele machine maken met de volgende waarden:
    - De naam van de virtuele MACHINE: Contoso-DNS-US
    - REGIO AFFINITEIT groep/VIRTUEEL netwerk: Selecteer Contoso-VNET-US
    - SUBNETTEN VIRTUEEL netwerk: Subnet-1
    - OPSLAG-ACCOUNT: Een account automatisch gegenereerde opslag gebruiken
    - EINDPUNTEN: (Selecteer DNS)

##<a name="set-static-ip-addresses-for-the-two-virtual-machines"></a>Statische IP-adressen voor de twee virtuele machines instellen

DNS-servers moet u statische IP-adressen.  Deze stap kan niet worden uitgevoerd vanuit de klassieke Azure-Portal. Gebruikt u PowerShell Azure.

**Statische IP-adres voor de twee virtuele machines configureren**

1. Open Windows PowerShell ISE.
2. Voer de volgende cmdlets.  

        Add-AzureAccount
        Select-AzureSubscription [YourAzureSubscriptionName]
        
        Get-AzureVM -ServiceName Contoso-DNS-EU -Name Contoso-DNS-EU | Set-AzureStaticVNetIP -IPAddress 10.1.0.4 | Update-AzureVM
        Get-AzureVM -ServiceName Contoso-DNS-US -Name Contoso-DNS-US | Set-AzureStaticVNetIP -IPAddress 10.2.0.4 | Update-AzureVM 

    Servicenaam is de naam van de wolk. Omdat de DNS-server de eerste virtuele machine van de cloud-service is, wordt de naam van de wolk is hetzelfde als de naam van de virtuele machine.

    Mogelijk moet u de servicenaam en naam overeenkomen met de namen die u hebt bijgewerkt.


##<a name="add-the-dns-server-role-the-two-virtual-machines"></a>De rol van de twee virtuele machines van DNS-Server toevoegen

**De functie van DNS-Server toevoegen voor DNS de Contoso-EU**

1.  Klik op **virtuele Machines** aan de linkerkant van het klassieke Portal Azure. 
2.  Klik op de **Contoso-DNS-EU**.
3.  Klik op **DASHBOARD** vanaf de bovenkant.
4.  Klik op **verbinding maken** vanaf de onderkant en volg de instructies om verbinding met de virtuele machine via RDP.
2.  Klik op de Windows-knop in de linkerbenedenhoek op het startscherm openen binnen de RDP-sessie.
3.  Klik op de tegel **Server Manager** .
4.  Klik op **functies en onderdelen toevoegen**.
5.  Klik op **volgende**
6.  **Installatie op basis van de rol of functie**te selecteren en klik vervolgens op **volgende**.
7.  Selecteer uw DNS-virtuele machine (deze moet worden gemarkeerd al) en klik op **volgende**.
8.  Controleer de **DNS-Server**.
9.  Klik op **Onderdelen toevoegen**en klik vervolgens op **Doorgaan**.
10. Klik driemaal op **volgende** en klik vervolgens op **installeren**. 

**De functie van DNS-Server toevoegen voor Contoso-DNS-US**

- Herhaal de stappen om toe te voegen DNS-rol **Contoso-DNS -**US.

##<a name="assign-dns-servers-to-the-virtual-networks"></a>DNS-servers toewijzen aan de virtuele netwerken

**De twee DNS-servers registreren**

1.  Klik op **Nieuw**, **NETWERKSERVICES**, **VIRTUAL NETWORK**, **DNS-SERVER REGISTREREN**via de klassieke Portal Azure.
2.  Voer in:
    - **Naam**: Contoso-DNS-EU
    - **IP-adres van de DNS-SERVER**: 10.1.0.4: het IP-adres moet overeenkomen met het IP-adres van DNS-server virtuele machine.
     
3.  Herhaal de procedure voor de Contoso-DNS-US registreren met de volgende instellingen:
    - **Naam**: Contoso-DNS-US
    - **IP-adres van de DNS-SERVER**: 10.2.0.4

**De twee DNS-servers toewijzen aan de twee virtuele netwerken**

1.  Klik op **netwerken** in het linkerdeelvenster in de klassieke Portal.
2.  Klik op de **Contoso-VNet-EU**.
3.  Klik op **configureren**.
4.  **Contoso-DNS-EU-** Selecteer in het gedeelte van de **DNS-servers** .
5.  Klik op **Opslaan** op de onderkant van de pagina en klik op **Ja** om te bevestigen.
6.  Herhaal dit proces om de **Contoso-DNS-US** DNS-server toewijzen aan het virtuele netwerk **Contoso-VNet-US** .

Alle virtuele machines die zijn geïmplementeerd met de virtuele netwerken moet opnieuw worden opgestart om het bijwerken van de configuratie van de DNS-server.

**Opnieuw opstarten van de virtuele machines**

1. Klik op **virtuele Machines** aan de linkerkant van het klassieke Portal Azure.
2. Klik op de **Contoso-DNS-EU**.
3. Klik op **Dashboard** vanaf de bovenkant.
4. Klik onderaan op **opnieuw opstarten** .
5. Herhaal dezelfde stappen uit om **DNS Contoso VS**opnieuw te starten.


##<a name="configure-dns-conditional-forwarders"></a>Configureert DNS voorwaardelijke doorstuurservers

DNS-namen in die virtuele netwerk kunt alleen oplossen door de DNS-server op elk virtueel netwerk. U moet een voorwaardelijke doorstuurserver om te verwijzen naar de naam oplossingen in het virtuele peernetwerk peer DNS-server configureren.

U kunt voorwaardelijke doorstuurserver configureren, moet u weten de domeinachtervoegsels van de twee DNS-servers. De DNS-achtervoegsels kunnen afwijken, afhankelijk van de configuratie van de wolk Services die u gebruikt bij het maken van de virtuele machines. Voor elke DNS-achtervoegsel in het virtuele netwerk gebruikt, moet u een voorwaardelijke doorstuurserver toevoegen. 

**Tot de domeinachtervoegsels van de twee DNS-servers zoeken**

1. RDP in **DNS de Contoso-EU**.
2. Open de console Windows PowerShell of de opdrachtprompt.
3. **Ipconfig**uitvoeren en schrijf **verbindingsspecifiek DNS-achtervoegsel**.
4. De RDP-sessie niet sluit, moet u deze. 
5. Herhaal dezelfde stappen uit om erachter te komen van de **verbindingsspecifieke DNS-achtervoegsel** van **Contoso-DNS-US**.


**Voor het configureren van DNS-doorstuurservers**
 
1.  Klik op de Windows-toets in de linkerbenedenhoek van de RDP-sessie aan **Contoso-DNS-EU**.
2.  Klik op **Systeembeheer**.
3.  Klik op **DNS**.
4.  Vouw in het linkerdeelvenster **DSN** **Contoso DNS EU**.
5.  Voer de volgende gegevens:
    - **DNS-domein**: Voer het DNS-achtervoegsel van de Contoso-DNS-Verenigde Staten. Bijvoorbeeld: Contoso-DNS-US.b5.internal.cloudapp.net.
    - **IP-adressen van de masterservers**: 10.2.0.4, namelijk de Contoso-DNS-Verenigde Staten van IP-adres invoeren.
6.  Druk op **ENTER**en klik vervolgens op **OK**.  Nu kun je de Contoso-DNS-Verenigde Staten van IP-adres van DNS-Contoso EU oplossen.
7.  Herhaal de stappen voor het toevoegen van een DNS-doorstuurserver aan de DNS-service op de Contoso-DNS-US virtuele machine met de volgende waarden:
    - **DNS-domein**: Voer het DNS-achtervoegsel van de Contoso-DNS-EU. 
    - **IP-adressen van de masterservers**: 10.2.0.4, namelijk de Contoso-DNS-EU-IP-adres invoeren.

##<a name="test-the-name-resolution-across-the-virtual-networks"></a>Test de naamomzetting via de virtuele netwerken

Test nu hostnamen om te zetten in de virtuele netwerken. Ping is standaard geblokkeerd door een firewall.  Nslookup kunt u kunt oplossen door de DNS-server virtuele machines (hebt u FQDN-naam) in de peer-netwerken.  


##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe naamomzetting configureren via virtuele netwerken met VPN-verbindingen. Betrekking hebben op de twee artikelen in de reeks:

- [Een VPN-verbinding tussen twee Azure virtuele netwerken configureren][hdinsight-hbase-geo-replication-vnet]
- [HBase geo-replicatie configureren][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[powershell-install]: powershell-install-configure.md

[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-DNS/HDInsight.HBase.VPN.diagram.png 
