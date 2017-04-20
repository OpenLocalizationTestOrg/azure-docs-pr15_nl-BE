<properties
    pageTitle="On-premises altijd op beschikbaarheidsgroepen uitbreiden naar Azure | Microsoft Azure"
    description="In deze zelfstudie worden gemaakt met het klassieke implementatiemodel bronnen gebruikt en wordt beschreven hoe u de wizard toevoegen Replica in SQL Server Management Studio (SSMS) toe te voegen een replica altijd op groep beschikbaarheid in Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>On-premises altijd op beschikbaarheidsgroepen uitbreiden naar Azure

Altijd op beschikbaarheidsgroepen bieden een hoge beschikbaarheid voor groepen van database door secundaire replica's toe te voegen. Deze replica's kunnen databases bij een storing op een failover. Ze kunnen ook laten lezen werklast of back-uptaken worden gebruikt.

Door het inrichten van een of meer Azure VMs met SQL Server en vervolgens als replica's toevoegen aan de groepen van de beschikbaarheid van gebouwen kunt u op ruimten beschikbaarheidsgroepen uitbreiden naar Microsoft Azure.

In deze zelfstudie wordt ervan uitgegaan dat u hebt de volgende opties:

- Een abonnement op Azure active. U kunt [zich aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- Een bestaande altijd op beschikbaarheid groep op-ruimten. Zie voor meer informatie over de beschikbaarheid van groepen, [Altijd op beschikbaarheidsgroepen](https://msdn.microsoft.com/library/hh510230.aspx).

- De verbinding tussen het netwerk van lokalen en Azure virtueel netwerk. Zie voor meer informatie over het maken van deze virtuele netwerken [configureren VPN in de klassieke Azure portal - website](../vpn-gateway/vpn-gateway-site-to-site-create.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="add-azure-replica-wizard"></a>Azure Replica Wizard toevoegen

In deze sectie wordt beschreven hoe u de **Azure Replica Wizard toevoegen** gebruiken om uit te breiden uw oplossing altijd op groep beschikbaarheid Azure replica's opnemen.

1. In SQL Server Management Studio Vouw **Altijd in hoge mate van beschikbaarheid** > **Beschikbaarheidsgroepen** > **[naam van de groep beschikbaarheid]**.

1. **Beschikbaarheid van replica's**met de rechtermuisknop en klik vervolgens op **Replica toevoegen**.

1. Standaard wordt de **Replica toevoegen om de Wizard groep beschikbaarheid** weergegeven. Klik op **volgende**.  Als u **deze pagina niet weergeven** tijdens de optie onderaan de pagina op een vorige starten van de wizard hebt geselecteerd, wordt dit scherm niet weergegeven.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)

1. U moet verbinding maken met alle bestaande secundaire replica's. Klik op **verbinding maken...** naast elke replica of u kunt klikken op **Verbinding maken met alle...** onderaan op het scherm. Na verificatie, klikt u op **volgende** om door te gaan naar het volgende scherm.

1. Klik op de pagina **Geef replica's** meerdere tabbladen worden weergegeven aan de bovenkant: **replica's**, **eindpunten**, **Voorkeuren voor back-up**en **Listener**. Klik op het tabblad **replica's** **Toevoegen Azure Replica...** Start de Wizard Azure Replica toevoegen.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)

1. Selecteer een bestaand Azure Management certificaat uit het lokale certificaatarchief van Windows als u hebt geïnstalleerd vóór. Selecteer of typ de id van een Azure-abonnement hebt u gebruikt voordat. U kunt downloaden om te downloaden en installeren van een certificaat Azure Management en downloaden van de lijst met abonnementen met een account met Azure.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)

1. U vult elk veld op de pagina met de waarden die worden gebruikt voor het maken van de Azure Virtual Machine (VM) die als host voor de replica fungeert.

  	|Instelling|Beschrijving|
|---|---|
|**Afbeelding**|Selecteer de gewenste combinatie van besturingssysteem en SQL Server|
|**VM-grootte**|Selecteer de grootte van de VM die het beste aan uw bedrijfsbehoeften|
|**VM-naam**|Geef een unieke naam voor de nieuwe VM. De naam moet tussen 3 en 15 tekens bevatten, kan alleen letters, cijfers en streepjes, bevatten en moet met een letter beginnen en eindigen met een letter of een nummer.|
|**VM-gebruikersnaam**|Geef een naam die de administratoraccount op de VM|
|**VM Administrator-wachtwoord**|Geef een wachtwoord voor de nieuwe account|
|**Bevestig wachtwoord**|Bevestig het wachtwoord van de nieuwe account|
|**Virtueel netwerk**|Geef de Azure virtueel netwerk dat moet worden gebruikt door de nieuwe VM. Zie [Virtuele netwerk-overzicht](../virtual-network/virtual-networks-overview.md)voor meer informatie over virtuele netwerken.|
|**Virtual Network Subnet**|Geef het virtuele netwerk subnet dat moet worden gebruikt door de nieuwe VM|
|**Domein**|Controleer de vooraf ingestelde waarde voor het domein juist zijn|
|**Domeinnaam**|Geef een account in de lokale groep administrators op de lokale clusterknooppunten|
|**Wachtwoord**|Geef het wachtwoord voor de gebruikersnaam van het domein|

1. Klik op **OK** voor het valideren van de implementatie-instellingen.

1. Juridische voorwaarden worden weergegeven. Lezen en klik op **OK** als u akkoord met deze voorwaarden gaat.

1. De pagina **Geeft u replica's** opnieuw weergegeven. Controleer of de instellingen voor de nieuwe Azure replica op de tabbladen **replica's**, **eindpunten**en **Voorkeuren voor back-up** . Instellingen wijzigen om te voldoen aan uw zakelijke behoeften.  Zie voor meer informatie over de parameters die zich op deze tabbladen [Pagina opgeven voor replica's (beschikbaarheid Wizard Nieuwe groep Wizard/Add Replica)](https://msdn.microsoft.com/library/hh213088.aspx). Houd er rekening mee dat listeners kunnen niet worden gemaakt met behulp van het tabblad Listener voor de beschikbaarheid van groepen met Azure replica's. Ook als een listener al vóór het starten van de Wizard is gemaakt, ontvangt u een bericht dat aangeeft dat niet wordt ondersteund in Azure. Gaan we listeners maken in de sectie **een Listener beschikbaarheid groep maken** .

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)

1. Klik op **volgende**.

1. Selecteer de synchronisatiemethode voor gegevens die u wilt gebruiken op de pagina **Selecteer initiële synchronisatie van gegevens** en klik op **volgende**. Voor de meeste scenario's, selecteert u **Volledige gegevenssynchronisatie**. Zie voor meer informatie over methoden voor adreslijstsynchronisatie van gegevens, [Selecteert u gegevens synchroniseren beginpagina (altijd op beschikbaarheid groep Wizards)](https://msdn.microsoft.com/library/hh231021.aspx).

1. Bekijk de resultaten op de pagina voor de **validatie** . Openstaande kwesties op te lossen en de validatie opnieuw uitvoeren indien nodig. Klik op **volgende**.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)

1. Controleer de instellingen op de pagina **Samenvatting** en klik op **Voltooien**.

1. Het inrichtingsproces begint. Wanneer de wizard voltooid is, klikt u op **sluiten** om de wizard af te sluiten.

>[AZURE.NOTE] De Wizard Azure Replica maakt een logbestand in Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Dit logboekbestand kan worden gebruikt om problemen met mislukte Azure replica-installaties. Als de Wizard niet uitvoeren van een actie, worden alle vorige bewerkingen teruggedraaid, met inbegrip van de ingerichte VM verwijderen.

## <a name="create-an-availability-group-listener"></a>Maak een groep beschikbaarheid listener

Nadat de beschikbaarheidsgroep is gemaakt, moet u een listener voor de clients verbinding maken met de replica's maken. Listeners directe inkomende verbindingen met de primaire of secundaire replica alleen-lezen. Zie voor meer informatie over listeners [configureren een listener ILB voor altijd op beschikbaarheidsgroepen in Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

## <a name="next-steps"></a>Volgende stappen

De **Azure Replica Wizard toevoegen** gebruiken uit te breiden uw altijd op beschikbaarheid groep in Azure, kunt u ook verplaatsen sommige werklasten SQL Server volledig naar Azure. Zie [een SQL Server virtuele Machine op Azure ingericht](virtual-machines-windows-portal-sql-server-provision.md)om te beginnen.

Zie [SQL Server Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md)voor andere onderwerpen die betrekking hebben op het SQL Server wordt uitgevoerd in Azure VMs.
