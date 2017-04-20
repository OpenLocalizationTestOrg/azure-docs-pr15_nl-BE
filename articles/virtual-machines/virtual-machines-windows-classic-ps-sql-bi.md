<properties
    pageTitle="SQL Server Business Intelligence | Microsoft Azure"
    description="In dit onderwerp worden gemaakt met het klassieke implementatiemodel bronnen gebruikt en beschrijving van de functies voor Business Intelligence (BI) voor SQL Server wordt uitgevoerd op Microsoft Azure virtuele Machines (VMs)."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>SQL Server Business Intelligence in Azure virtuele Machines

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

De galerie met Microsoft Azure Virtual Machine bevat afbeeldingen met installaties van SQL Server. De edities van SQL Server worden ondersteund in de afbeeldingen zijn dezelfde installatiebestanden die u op computers op gebouwen en virtuele machines installeren kunt. Dit onderwerp bevat een overzicht van de functies van SQL Server Business Intelligence (BI) geïnstalleerd op de afbeeldingen en de configuratiestappen vereist zijn, een virtuele machine is ingericht. In dit onderwerp wordt ook beschreven ondersteunde implementatietopologieën voor BI-voorzieningen en aanbevolen procedures.

## <a name="license-considerations"></a>Licentie overwegingen

Er zijn twee manieren om een licentie voor SQL Server in Microsoft Azure virtuele Machines:

1. Licentie mobiliteit voordelen die deel van de Software Assurance uitmaken. Voor meer informatie Zie [Licentie mobiliteit via Software Assurance op Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. Betalen per uurtarief van Azure Virtual Machines met SQL Server is geïnstalleerd. Zie het gedeelte 'SQL Server' in de [Prijzen van virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Zie [Virtuele Machines licentieverlening Veelgestelde vragen](https://azure.microsoft.com/pricing/licensing-faq/%20/)voor meer informatie over licenties en huidige tarieven.

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server Images beschikbaar in de galerie met virtuele Machine Azure

De galerie met Microsoft Azure Virtual Machine bevat meerdere afbeeldingen met een Microsoft SQL Server. De software is geïnstalleerd op de virtuele machine afbeeldingen is afhankelijk van de versie van het besturingssysteem en de versie van SQL Server. De lijst met afbeeldingen die beschikbaar zijn in de galerie Azure virtuele machine vaak verandert.

![SQL-afbeelding in de galerie met azure VM](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) De volgende PowerShell script wordt de lijst met Azure afbeeldingen met 'SQL-Server' in de ImageName:

    # assumes you have already uploaded a management certificate to your Microsoft Azure Subscription. View the thumbprint value from the "settings" menu in Azure classic portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is the one specified with the -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Zie de volgende onderwerpen voor meer informatie over edities en onderdelen die worden ondersteund door SQL Server:

- [Edities van SQL Server](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)

- [Functies die worden ondersteund door de andere edities van SQL Server 2016](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>BI-functies geïnstalleerd op de SQL Server virtuele Machine-afbeeldingen

De volgende tabel bevat een overzicht van de Business Intelligence-functies op de algemene Microsoft Azure Virtual Machine-afbeeldingen voor SQL Server is geïnstalleerd"

- SQL Server 2016 RC3

- SQL Server 2014 SP1 Enterprise

- SQL Server 2014 SP1 Standard

- SQL Server 2012 SP2 Enterprise

- SQL Server 2012 SP2 Standard

|SQL Server BI-functie|Op de afbeelding is geïnstalleerd|Notities|
|---|---|---|
|**Reporting Services Native modus**|Ja|Geïnstalleerd, maar moet worden geconfigureerd, met inbegrip van de URL van report manager. Zie de sectie [Reporting Services configureren](#configure-reporting-services).|
|**Reporting Services SharePoint-modus**|Nee|De galerijafbeelding van Microsoft Azure Virtual Machine bevat geen SharePoint en SharePoint-installatiebestanden. <sup>1</sup>|
|**Multidimensionale van Analysis Services en Data mining (OLAP)**|Ja|Geïnstalleerd en geconfigureerd als standaard Analysis Services-sessie|
|**Analyseservices in tabelvorm**|Nee|Ondersteund in SQL Server 2012, wordt 2014 en 2016 afbeeldingen, maar deze niet standaard geïnstalleerd. Een ander exemplaar van Analysis Services installeren. Zie de sectie andere SQL Server-Services en onderdelen in dit onderwerp installeren.|
|**Analysis Services Power Pivot voor SharePoint**|Nee|De galerijafbeelding van Microsoft Azure Virtual Machine bevat geen SharePoint en SharePoint-installatiebestanden. <sup>1</sup>|

<sup>1</sup> Zie voor meer informatie over SharePoint en Azure virtuele machines, [Microsoft Azure architecturen voor SharePoint 2013](https://technet.microsoft.com/library/dn635309.aspx) en [SharePoint-implementatie op Microsoft Azure virtuele Machines](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) De volgende PowerShell-opdracht om een lijst van de geïnstalleerde services die 'SQL' in de servicenaam van de bevatten wordt uitgevoerd.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Algemene aanbevelingen en beste praktijken

- De aanbevolen minimumgrootte voor een virtuele machine is **A3** als u SQL Server Enterprise Edition. De grootte van de virtuele machine **A4** wordt aanbevolen voor SQL Server BI-implementaties van Analysis Services en Reporting Services.

    Zie voor informatie over de grootte van de huidige VM, [Formaten voor Azure Virtual Machine](virtual-machines-linux-sizes.md).

- Een best practice voor Schijfbeheer is voor het opslaan van gegevens, het logboek en back-upbestanden op stations **c**: en **D**:. Maak bijvoorbeeld gegevensschijven **E**: en **F**:.

    - Het beleid voor het standaardstation is **C**caching station: is niet optimaal voor het werken met gegevens.

    - De **D**: station is een tijdelijke station dat voornamelijk voor het wisselbestand gebruikt wordt. De **D**: station wordt niet behouden en wordt niet opgeslagen in de blob-opslag. Beheertaken, zoals een wijziging in de grootte van de virtuele machine opnieuw de **D**: station. Het is raadzaam **niet** te gebruiken met de **D**: station voor databasebestanden, met inbegrip van tempdb.

    Zie voor meer informatie over het maken en koppelen van schijven, [het koppelen van een schijf met gegevens op een virtuele Machine](virtual-machines-windows-classic-attach-disk.md).

- Stop of services die u niet wilt gebruiken verwijderen. Voor bijvoorbeeld als de virtuele machine wordt alleen gebruikt voor Reporting Services stoppen of verwijderen van Analysis Services en SQL Server Integration Services. De volgende afbeelding is een voorbeeld van de services die standaard worden gestart.

    ![SQL Server-services](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)

    >[AZURE.NOTE] De SQL Server-database-engine is vereist in de ondersteunde BI-scenario's. In een enkele server VM topologie, de database-engine moet worden uitgevoerd op dezelfde VM.

    Zie voor meer informatie het volgende: [Reporting Services verwijderen](https://msdn.microsoft.com/library/hh479745.aspx) en het [verwijderen van een exemplaar van Analysis Services](https://msdn.microsoft.com/library/ms143687.aspx).

- Controleer **Windows Update** nieuwe 'belangrijke updates'. De Microsoft Azure Virtual Machine afbeeldingen worden vaak vernieuwd; echter belangrijke updates kunnen worden door **Windows Update** nadat u de afbeelding VM voor het laatst is vernieuwd.

## <a name="example-deployment-topologies"></a>Voorbeeld van de implementatie van topologieën

Het volgende zijn voorbeeld-implementaties die gebruikmaken van Microsoft Azure virtuele Machines. De topologieën in deze diagrammen zijn slechts enkele van de mogelijke topologieën die kunt u met de functies van SQL Server BI en Microsoft Azure virtuele Machines.

### <a name="single-virtual-machine"></a>Één virtuele Machine

Analyse bronnen Services, Reporting Services, SQL Server Database Engine en gegevens op een enkele virtuele machine.

![scenario met 1 virtuele machine BI International Accounting Standards](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Twee virtuele Machines

- Analyseservices, Reporting Services en SQL Server Database Engine op een enkele virtuele machine. Deze installatie bevat de lijst server-databases.

- Gegevensbronnen op een tweede VM. De tweede VM bevat Database-Engine van SQL Server als gegevensbron.

![BI iaas scenario met 2 virtuele machines](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Gemengde Azure – gegevens op Azure SQL-database

- Analyseservices, Reporting Services en SQL Server Database Engine op een enkele virtuele machine. Deze installatie bevat de lijst server-databases.

- Gegevensbron is Azure SQL-database.

![BI iaas scenario's vm en AzureSQL als gegevensbron](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hybride – gegevens van ruimten

- In dit voorbeeld implementatie van Analysis Services uitvoeren Reporting Services en SQL Server Database Engine op een enkele virtuele machine. De virtuele machine host van de server-databases van het rapport. De virtuele machine is gekoppeld aan een domein op gebouwen via Azure virtuele netwerken of sommige andere VPN-tunneling oplossing.

- Gegevensbron is op gebouwen.

![BI iaas scenario's vm en op lokale gegevensbronnen](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Configuratie van Reporting Services Native modus

De galerijafbeelding van de virtuele machine voor SQL Server bevat Reporting Services Native modus is geïnstalleerd, maar de report server is niet geconfigureerd. De stappen in deze sectie configureren de Reporting Services report server. Zie de [Installatie van Reporting Services Native modus rapport Server (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx)voor meer gedetailleerde informatie over het configureren van Reporting Services Native-modus.

>[AZURE.NOTE] Zie voor soortgelijke inhoud met Windows PowerShell-scripts voor het configureren van de report server [PowerShell gebruiken voor het maken van een Azure VM met een Native modus Report Server](virtual-machines-windows-classic-ps-sql-report.md).

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>Verbinding maken met de virtuele Machine en het starten van Configuratiebeheer voor de Reporting Services

Er zijn twee algemene werkstromen voor het maken van een verbinding met een Azure Virtual Machine:

- Verbinden in, klik op de naam van de virtuele machine en klik vervolgens op **verbinden**. Een verbinding met extern bureaublad wordt geopend en de naam van de computer wordt automatisch gevuld.

    ![verbinding maken met azure virtual machine](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)

- Verbinding maken met de virtuele machine Windows verbinding met extern bureaublad. In de gebruikersinterface van het externe bureaublad:

    1. Typ de **naam van de service cloud** als de naam van de computer.

    1. Dubbele punt (:) en het nummer van de openbare poort die is geconfigureerd voor het TCP-eindpunt voor extern bureaublad.

        Myservice.cloudapp.NET:63133

        Zie voor meer informatie [Wat is een cloud service?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).

**Start Reporting Services Configuration Manager.**

1. In **Windows Server 2012**:

1. Typ in **het startscherm van** **Reporting Services** voor een lijst van Apps.

1. **Reporting Services Configuration Manager** met de rechtermuisknop en klik op **als Administrator uitvoeren**.

1. In **Windows Server 2008 R2**:

1. Klik op **Start**en klik vervolgens op **Alle programma's**.

1. Klik op **Microsoft SQL Server 2016**.

1. Klik op **Extra configuratie**.

1. **Reporting Services Configuration Manager** met de rechtermuisknop en klik op **als Administrator uitvoeren**.

Of

1. Klik op **Start**.

1. Typ in het dialoogvenster **Zoeken in programma's en bestanden** **reporting services**. Als de VM wordt uitgevoerd van Windows Server 2012, typt u **reporting services** op het Windows Server 2012 startscherm.

1. **Reporting Services Configuration Manager** met de rechtermuisknop en klik op **als Administrator uitvoeren**.

    ![Zoek naar configuration manager van SQL Server Reporting Services](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Reporting Services configureren

**-Serviceaccount en webservice-URL:**

1. Controleer of dat de **Servernaam** is de naam van de lokale server en klik op **verbinden**.

1. Noteer de lege **Database servernaam**. De database wordt gemaakt wanneer de configuratie is voltooid.

1. Controleer of dat het **Rapportstatus van de Server** is **gestart**. Als u wilt controleren of de service in Windows Server Manager, de service is de **SQL Server Reporting Services** Windows.

1. Klik op **Service-Account** en de account desgewenst wijzigen. Als de virtuele machine wordt gebruikt in een niet-gekoppelde domeinomgeving, is de ingebouwde **ReportServer** -account voldoende. Zie voor meer informatie over de service-account, [Service-Account](https://msdn.microsoft.com/library/ms189964.aspx).

1. Klik op **Webservice-URL** in het linkerdeelvenster.

1. Klik op **toepassen** om de standaardwaarden te configureren.

1. Opmerking de **lijst Server webservice-URL's**. Opmerking de standaard TCP-poort 80 en is onderdeel van de URL. In een latere stap maakt u een eindpunt Microsoft Azure Virtual Machine voor de poort.

1. Controleer in **het resultatenvenster** de acties met succes is voltooid.

**Database:**

1. Klik op **Database** in het linkerdeelvenster.

1. Klik op **Database wijzigen**.

1. Controleer of **dat een report server-database maken** is geselecteerd en klik op volgende.

1. Controleer of de **Servernaam** en klik op **Verbinding testen**.

1. Als het resultaat is de **verbindingstest is geslaagd**, klikt u op **OK** en klik vervolgens op **volgende**.

1. Let op de database **ReportServer** en de **Report Server-modus** is **standaard** en klik op **volgende**.

1. Klik op **volgende** op de pagina **referenties** .

1. Klik op de pagina **Samenvatting** op **volgende** .

1. Klik op **volgende** op de pagina **voortgang en voltooien** .

**Web Portal-URL of de URL van Report Manager voor 2012 en 2014:**

1. Klik op **Web Portal-URL**of de **URL van Report Manager** voor 2014 en 2012 in het linkerdeelvenster.

1. Klik op **toepassen**.

1. Controleer in **het resultatenvenster** de acties met succes is voltooid.

1. Klik op **Afsluiten**.

Zie voor meer informatie over machtigingen voor de lijst [Machtigingen verlenen voor een Native modus Report Server](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Ga naar de lokale Report Manager

Als u wilt controleren of de configuratie, Ga naar Rapportbeheer op de VM.

1. Start Internet Explorer op de VM, met administrator-bevoegdheden.

1. Ga naar http://localhost/reports op de VM.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>Klik op verbinding maken met externe webportal of Report Manager voor 2014 en 2012

Maak een nieuwe virtuele machine TCP-eindpunt als u wilt verbinden met de webportal of Report Manager voor 2014 en 2012 op de virtuele machine vanaf een externe computer. Standaard wordt de report server luistert naar HTTP-verzoeken op **poort 80**. Als u het rapport-URL's voor het gebruik van een andere poort configureert, moet u dat poortnummer in de volgende instructies.

1. Maak een eindpunt voor de virtuele Machine van TCP-poort 80. Zie voor meer informatie de sectie [virtuele Machine eindpunten en Firewall-poorten](#virtual-machine-endpoints-and-firewall-ports) in dit document.

1. Poort 80 openen in de firewall van de virtuele machine.

1. Ga naar het webportaal of met Azure Virtual Machine **DNS-naam** als de naam van de server in de URL van report manager. Bijvoorbeeld:

    **Report server**: http://uebi.cloudapp.net/reportserver  **webportal**: http://uebi.cloudapp.net/reports

    [Een Firewall configureren voor toegang tot de Server van rapporten](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Voor het maken en publiceren van rapporten op de Azure Virtual Machine

De volgende tabel bevat een overzicht van enkele van de opties beschikbaar voor het publiceren van een computer op gebouwen bestaande rapporten met de report server gehost op Microsoft Azure Virtual Machine:

- **Report Builder**: de virtuele machine bevat de Klik-eenmaal een versie van Microsoft SQL Server Report Builder voor SQL 2014 en 2012. Rapport opbouwfunctie voor de eerste keer op de virtuele machine met SQL 2016 starten:

    1. Start uw browser met beheerdersbevoegdheden.

    1. Ga naar het webportaal op de virtuele machine en selecteer het pictogram **downloaden** in de rechterbovenhoek.
    
    1. Selecteer **Report Builder**.

    Zie voor meer informatie, [Start Report Builder](https://msdn.microsoft.com/library/ms159221.aspx).

- **SQL Server Data Tools**: VM: SQL Server Data Tools op de virtuele machine is geïnstalleerd en kan worden gebruikt voor het **Rapport Server-projecten** en -rapporten maken op de virtuele machine. SQL Server Data Tools kunt u de rapporten publiceren naar de report server op de virtuele machine.

- **SQL Server Data Tools: externe**: op de lokale computer een Reporting Services-project in SQL Server Data Tools met Reporting Services-rapporten te maken. Het project verbinding maken met de URL van de webservice configureren.

    ![SSDT Projecteigenschappen voor SQL Server Reporting Services-project](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)

- Maak een. Harde schijf VHD die rapporten bevat en vervolgens uploaden en het station koppelen.

    1. Maak een. VHD harde schijf op de lokale computer waarop u uw rapporten.

    1. Maken en installeren van een certificaat.

    1. Het VHD-bestand uploaden naar Azure met de cmdlet Add-AzureVHD [maken en uploaden van een Windows Server VHD naar Azure](virtual-machines-windows-classic-createupload-vhd.md).

    1. Sluit de schijf aan de virtuele machine.

## <a name="install-other-sql-server-services-and-features"></a>Andere SQL Server-Services en -functies installeren

De wizard SQL server setup om de installatie van aanvullende SQL Server-services, zoals Analysis Services in de modus in tabelvorm worden uitgevoerd. De setup-bestanden zijn op de lokale schijf van de virtuele machine.

1. Klik op **Start** en klik vervolgens op **Alle programma's**.

1. Klik op **Microsoft SQL Server 2016**, **2014 van Microsoft SQL Server** of **Microsoft SQL Server 2012** en klik vervolgens op **Configuratiehulpprogramma's**.

1. Klik op **SQL Server installatie Center**.

Of C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe of C:\SQLServer_11.0_full\setup.exe

>[AZURE.NOTE] De eerste keer dat u de installatie van SQL Server uitvoert meer setup-bestanden kunnen worden gedownload en de virtuele machine opnieuw op te starten en het opnieuw opstarten van SQL Server setup vereist.
>
>Als u moet herhaaldelijk aanpassen van de afbeelding geselecteerd vanuit Microsoft Azure Virtual Machine, kunt u uw eigen SQL Server-image maakt. Functionaliteit van Analysis Services SysPrep is met SQL Server 2012 SP1 CU2 ingeschakeld. Zie [Overwegingen bij het installeren van SQL Server met behulp van SysPrep](https://msdn.microsoft.com/library/ee210754.aspx) en [Sysprep-ondersteuning voor Server-rollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)voor meer informatie.

### <a name="to-install-analysis-services-tabular-mode"></a>In tabelvorm modus van Analysis Services installeren

De stappen in deze sectie **vatten** de installatie van Analysis Services in tabelvorm modus. Zie de volgende onderwerpen voor meer informatie:

- [Analyseservices installeren in de modus in tabelvorm](https://msdn.microsoft.com/library/hh231722.aspx)

- [In tabelvorm modellering (Adventure Works-zelfstudie)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**In tabelvorm modus van Analysis Services installeren:**

1. In de wizard SQL Server installeren **installatie** in het linkerdeelvenster en klik op **nieuwe SQL server-zelfstandige installatie of functies toevoegen aan een bestaande installatie van**.

    - Als u de **Map**ziet, Ga naar c:\SQLServer_13.0_full, c:\SQLServer_12.0_full of c:\SQLServer_11.0_full en klik vervolgens op **Ok**.

1. Klik op **volgende** op de pagina product updates.

1. Selecteert u **een nieuwe installatie van SQL Server uitvoeren** op de pagina **Type installatie** en klik op **volgende**.

1. Klik op de pagina **Setup functie** **Functies van SQL Server-installatie**.

1. Klik op **Analysis Services**op de pagina **Functies selecteren** .

1. Typ een beschrijvende naam, bijvoorbeeld **in tabelvorm** in tekstvakken **Met de naam exemplaar** en een **Exemplaar-Id** op de pagina **Configuratie van het exemplaar** .

1. Selecteer op de pagina **Configuratie van Analysis Services** **-Modus in tabelvorm**. De huidige gebruiker toevoegen aan de lijst met machtigingen.

1. Voltooien en sluit de wizard SQL Server installeren.

## <a name="analysis-services-configuration"></a>Configuratie van Analysis Services

### <a name="remote-access-to-analysis-services-server"></a>RAS-toegang tot Analysis Services-Server

Analysis Services-server ondersteunt alleen windows-verificatie. De virtuele machine moet lid zijn van uw lokale domein, met behulp van Azure Virtual toegang toegang tot Analysis Services op afstand vanaf een client-toepassingen zoals SQL Server Management Studio of SQL Server Data Tools. Zie voor meer informatie, [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).

Een **standaardexemplaar** van Analysis Services luistert op TCP-poort **2383**. De poort openen in de firewall van virtuele machines. Geclusterde benoemde versie van Analysis Services wordt ook luistert op poort **2383**.

Voor een **benoemd exemplaar** van Analysis Services, SQL Server-browserservice vereist voor het beheren van toegang tot een haven. De standaardconfiguratie van SQL Server-Browser is poort **2382**.

In de virtuele machines firewall, poort **2382** openen en maken van een statische Analysis Services met de naam exemplaar poort.

1. Om te controleren of poorten die al in gebruik is op de VM en welk proces de poorten gebruikt, moet u de volgende opdracht uitvoeren met beheerdersmachtigingen:

        netstat /ao

1. Gebruik SQL Server Management Studio voor het maken van een statische Analysis Services met de naam exemplaar poort door te werken 'Poort' waarde in tabelvorm als instantie-algemene eigenschappen. Zie de 'gebruik van een vaste poort voor een standaard- of instantie met de naam' in [Analysis Services toegang toestaan Windows Firewall configureren](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed)voor meer informatie.

1. Het exemplaar in tabelvorm van de Analysis Services-service opnieuw starten.

Zie voor meer informatie de sectie **virtuele Machine eindpunten en Firewall-poorten** in dit document.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>De eindpunten van de virtuele Machine en Firewall-poorten

Deze sectie bevat een overzicht van Microsoft Azure Virtual Machine eindpunten te maken en de poorten te openen in de virtuele machine firewalls. In de volgende tabel bevat een overzicht van de **TCP-** poorten voor eindpunten voor en de poorten openen in firewall voor virtuele machines.

- Als u gebruikmaakt van een enkele VM de volgende twee vereisten is voldaan, hoeft u geen eindpunten VM maken en u hoeft niet de poorten openen in de firewall op de VM.

    - U verbinding geen extern met de SQL Server-functies op de VM. Tot stand brengen van een verbinding met extern bureaublad voor VM en toegang tot de onderdelen van SQL Server lokaal op de VM wordt niet beschouwd als een externe verbinding met de SQL Server-functies.

    - U doet de VM niet toevoegen aan een domein op gebouwen via Azure virtuele netwerken of een andere VPN-tunneling oplossing.

- Als de virtuele machine is geen lid van een domein, maar op afstand wilt verbinding met de SQL Server-functies op de VM:

    - Open de poorten in de firewall op de VM.

    - Maak de eindpunten van de virtuele machine voor de poorten vermeld (*).

- De eindpunten zijn niet vereist als de virtuele machine is verbonden met een domein met een VPN-tunnel zoals Azure Virtual Networking. Maar de poorten openen in de firewall op de VM.

  	|Poort|Type|Beschrijving|
|---|---|---|
|**80**|TCP|Report-server externe toegang (*).|
|**1433**|TCP|SQL Server Management Studio (*).|
|**1434**|UDP|SQL Server-Browser. Dit is nodig wanneer de VM in deel uitmaakt van een domein.|
|**2382**|TCP|SQL Server-Browser.|
|**2383**|TCP|Standaardexemplaar van SQL Server Analysis Services en geclusterde benoemde instanties.|
|**Door de gebruiker gedefinieerd**|TCP|Maak een statische Analysis Services met de naam exemplaar poort een poortnummer dat u kiest en vervolgens het nummer van de poort in de firewall blokkering.|

Zie de volgende onderwerpen voor meer informatie over het maken van eindpunten:

- Eindpunten maken:[het instellen van de eindpunten aan een virtuele Machine](virtual-machines-windows-classic-setup-endpoints.md).

- SQL Server: Zie de sectie 'Volledige configuratie stappen voor verbinding met de virtuele machine met SQL Server Management Studio' van het [inrichten van een SQL Server virtuele Machine op Azure](virtual-machines-windows-portal-sql-server-provision.md).

In het volgende diagram ziet u de poorten openen in firewall voor externe toegang tot functies en onderdelen op de VM VM.

![poorten openen voor bi-toepassingen in Azure VMs](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Bronnen

- Bekijk het ondersteuningsbeleid voor Microsoft-serversoftware die wordt gebruikt in de omgeving van Azure Virtual Machine. Het volgende onderwerp bevat een overzicht van de ondersteuning voor functies zoals BitLocker, failover-clusters en netwerktaakverdeling. [Ondersteuning van Microsoft-serversoftware voor Azure virtuele Machines](http://support.microsoft.com/kb/2721672).

- [SQL Server op Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md)

- [Virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)

- [Inrichten van een SQL Server virtuele Machine op Azure](virtual-machines-windows-portal-sql-server-provision.md)

- [Het koppelen van een schijf met gegevens op een virtuele Machine](virtual-machines-windows-classic-attach-disk.md)

- [Een Database migreren naar SQL Server op een Azure VM](virtual-machines-windows-migrate-sql.md)

- [De modus van een Analysis Services-instantie bepalen](https://msdn.microsoft.com/library/gg471594.aspx)

- [Multidimensionale modellering (Adventure Works-zelfstudie)](https://technet.microsoft.com/library/ms170208.aspx)

- [Azure-documentatiecentrum](https://azure.microsoft.com/documentation/)

- [Met behulp van Power BI in een hybride omgeving](https://msdn.microsoft.com/library/dn798994.aspx)

>[AZURE.NOTE] [Feedback en informatie over contactpersonen via Microsoft SQL Server-verbinding verzenden](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Community-inhoud

- [Azure SQL Database Management met PowerShell](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)
