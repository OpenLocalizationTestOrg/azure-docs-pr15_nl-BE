<properties
    pageTitle="Integratie van de Azure sleutel kluis voor SQL Server configureren op Azure VMs (Resource Manager)"
    description="Informatie over het automatiseren van de configuratie van SQL Server-versleuteling voor gebruik met Azure sleutel kluis. Dit onderwerp wordt uitgelegd hoe u Azure sleutel kluis integratie met SQL Server virtuele machines met Resource Manager gemaakt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-resource-manager"></a>Integratie van de Azure sleutel kluis voor SQL Server configureren op Azure VMs (Resource Manager)

> [AZURE.SELECTOR]
- [Resource Manager](virtual-machines-windows-ps-sql-keyvault.md)
- [Klassiek](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Overzicht
Er zijn meerdere SQL Server coderingsfuncties zoals [transparante gegevenscodering (TDE)](https://msdn.microsoft.com/library/bb934049.aspx) [kolom niveau van codering (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)en [back-codering](https://msdn.microsoft.com/library/dn449489.aspx). Deze vormen van codering, moet u de cryptografiesleutels die voor codering u opslaan en beheren. De Azure sleutel kluis (AKV)-service is ontworpen voor het verbeteren van de beveiliging en het beheer van deze sleutels in een veilige en hoge beschikbaarheid locatie. De [SQL Server-Connector](http://www.microsoft.com/download/details.aspx?id=45344) kunnen deze toetsen van Azure sleutel kluis worden gebruikt door SQL Server.

Als u SQL Server wordt uitgevoerd met op ruimten machines, er zijn [stappen die u kunt volgen om toegang tot Azure sleutel kluis van uw SQL Server-computer op gebouwen](https://msdn.microsoft.com/library/dn198405.aspx). Maar voor SQL Server in Azure VMs, kunt u tijd besparen met behulp van de functie *Integratie met Azure sleutel kluis* .

Wanneer deze functie is ingeschakeld, het automatisch de SQL Server-Connector installeert, configureert u de provider EKM toegang tot Azure sleutel kluis en maakt u de referenties waarmee u toegang tot uw kluis. Als u de stappen in de documentatie van de eerder genoemde op gebouwen bekeken, kunt u zien dat deze functie kunt u stap 2 en 3 automatiseren. Het enige wat dat nog moet u handmatig doen is de sleutel kluis en toetsen te maken. Daar is de instellingen van uw SQL-VM geautomatiseerd. Als deze functie kan deze setup is voltooid, kunt u de T-SQL-instructies om te beginnen met het coderen van uw databases of back-ups als normaal uitvoeren.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Inschakelen en configureren van AKV-integratie
U kunt de integratie AKV tijdens het inrichten of configureren voor bestaande VMs.

### <a name="new-vms"></a>Nieuwe VMs
Als u een nieuwe SQL Server virtuele machine met Resource Manager zijn ingericht, biedt de portal Azure een stap om de integratie van de Azure sleutel kluis. De Azure sleutel kluis-functie is alleen beschikbaar voor de onderneming, de ontwikkelaar en de evaluatie-edities van SQL Server.

![Integratie van SQL Azure sleutel kluis](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Zie voor een gedetailleerd overzicht van het inrichten van [een SQL Server virtuele machine in de Portal Azure bepaling](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Bestaande VMs
Selecteer uw SQL Server virtuele machine voor bestaande SQL Server virtuele machines. Selecteer de sectie **SQL Server-configuratie** van de **Instellingen voor** blade.

![Integratie van SQL-AKV voor bestaande VMs](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

Klik op de knop **bewerken** in de sectie automatisch sleutel kluis integratie in de **configuratie van SQL Server** -blade.

![Configureer SQL AKV-integratie voor bestaande VMs](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Wanneer u klaar bent, klikt u op de knop **OK** op de onderkant van de **configuratie van SQL Server** -blade uw wijzigingen op te slaan.

>[AZURE.NOTE] U kunt ook AKV integratie met behulp van een sjabloon. Zie voor meer informatie [Azure quickstart sjabloon voor de integratie van de Azure sleutel kluis](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
