<properties
    pageTitle="Beveiligingsoverwegingen voor SQL Server in Azure | Microsoft Azure"
    description="In dit onderwerp wordt verwezen naar bronnen die zijn gemaakt met het klassieke implementatiemodel en algemene richtlijnen voor de beveiliging van SQL Server wordt uitgevoerd in een Azure Virtual Machine biedt."
    services="virtual-machines-windows"
    documentationCenter="na"
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
    ms.date="06/24/2016"
    ms.author="jroth" />

# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Beveiligingsoverwegingen voor SQL Server in Azure virtuele Machines
 
Dit onderwerp bevat algemene richtlijnen voor beveiliging waarmee u beveiligde toegang tot de SQL Server-exemplaren maken in een VM Azure. Echter met het oog op een betere bescherming voor uw SQL Server-database-exemplaren in Azure, wordt aangeraden om de procedures voor de beveiliging van traditionele op ruimten naast de aanbevolen procedures voor beveiliging te voor Azure implementeren.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Zie voor meer informatie over de procedures voor beveiliging van SQL Server, [SQL Server 2008 R2 Security Best Practices - operationele en administratieve taken](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx)

Azure voldoet aan verschillende industriële voorschriften en normen waarmee u een compatibele oplossing maken met SQL Server op een virtuele Machine wordt uitgevoerd. Zie voor meer informatie over de naleving van regelgeving met Azure [Azure Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/).

Hier volgt een lijst met aanbevelingen die u overwegen moet bij het configureren en verbinding te maken met het exemplaar van SQL Server in een VM Azure.

## <a name="considerations-for-managing-accounts"></a>Overwegingen voor het beheren van accounts:

- Maak een unieke lokale administrator-account niet met de naam **Administrator**.

- Complexe sterke wachtwoorden gebruiken voor al uw accounts. Voor meer informatie over het maken van een sterk wachtwoord, Zie [Tips voor het maken van een sterke wachtwoorden](http://windows.microsoft.com/en-us/windows-vista/Tips-for-creating-a-strong-password) artikel.

- Standaard selecteert Azure Windows-verificatie tijdens de installatie van SQL Server virtuele Machine. Daarom de **SA** -aanmelding is uitgeschakeld en wordt door setup een wachtwoord is toegewezen. Wij aanbevelen dat de **SA** -aanmelding moet niet worden gebruikt of ingeschakeld. Dit zijn alternatieve strategieën als een SQL-aanmelding is gewenst:
    - Maak een SQL-account met sysadmin-lidmaatschap.
    - Als u een aanmeldingsaccount **SA** , moet u de aanmelding inschakelen en wijzigt u de naam en een nieuw wachtwoord toewijzen.
    - De opties die eerder zijn vermeld, vereisen een wijziging van de verificatiemodus voor **SQL Server en Windows-verificatiemodus**. Zie [Server-verificatiemodus wijzigen](https://msdn.microsoft.com/library/ms188670.aspx)voor meer informatie.

## <a name="considerations-for-securing-connections-to-azure-virtual-machine"></a>Overwegingen voor het beveiligen van verbindingen met Azure Virtual Machine:

- Overweeg het gebruik van [Azure virtueel netwerk](../virtual-network/virtual-networks-overview.md) voor het beheer van de virtuele machines in plaats van openbare RDP-poorten.

- Gebruik een [Network Security Group](../virtual-network/virtual-networks-nsg.md) (NSG) toestaan of weigeren netwerkverkeer op uw virtuele machine. Als u wilt een NSG gebruikt en er een eindpunt ACL al aanwezig zijn, moet u eerst het eindpunt ACL verwijderen. Voor informatie over hoe u dit doet, Zie [Beheren van toegangsbeheerlijsten (ACL's) voor eindpunten met PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

- Als u eindpunten verwijderen eindpunten op de virtuele machine als u ze niet gebruikt. Zie [de ACL op een eindpunt beheren](../virtual-network/virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint)voor instructies over het gebruik van ACL's met eindpunten.

- Een optie gecodeerde verbinding inschakelen voor een exemplaar van de SQL Server-Database-Engine in Azure virtuele Machines. SQL server-exemplaar configureren met een certificaat. Voor meer informatie Zie [Gecodeerde verbindingen inschakelen met de Database-Engine](https://msdn.microsoft.com/library/ms191192.aspx) en de [Syntaxis van verbindingsreeksen](https://msdn.microsoft.com/library/ms254500.aspx).

- Als uw virtuele machines moet alleen worden geopend via een bepaald netwerk, gebruikt u Windows Firewall toegang beperken tot bepaalde IP-adressen of subnetten.

## <a name="next-steps"></a>Volgende stappen

Bent u ook geïnteresseerd in de beste praktijken om de prestaties, Zie [Aanbevolen procedures voor SQL Server in Azure virtuele Machines prestaties](virtual-machines-windows-sql-performance.md).

Zie [SQL Server op overzicht Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md)voor andere onderwerpen die betrekking hebben op het SQL Server wordt uitgevoerd in Azure VMs.
