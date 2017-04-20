<properties
 pageTitle="Pack HPC-cluster voor Excel en SOA | Microsoft Azure"
 description="Aan de slag met grootschalige Excel- en SOA-werklasten op een Pack van HPC-cluster in Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/25/2016"
 ms.author="danlep"/>

# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Aan de slag met Excel- en SOA-werklasten op een Pack van HPC-cluster in Azure

In dit artikel wordt beschreven hoe u een Pack voor Microsoft HPC-cluster op Azure virtuele machines met behulp van een sjabloon Azure quickstart of eventueel een implementatie Azure PowerShell script implementeren. Azure Marketplace VM-afbeeldingen die zijn ontworpen voor Microsoft Excel of de werkbelasting van service oriented architecture (SOA) HPC Pack maakt gebruik van het cluster. Het cluster kunt u eenvoudig Excel HPC- en SOA-services worden uitgevoerd vanaf een clientcomputer op gebouwen. De HPC Excel services omvatten offloading Excel-werkmap en door de gebruiker gedefinieerde functies voor Excel of UDF's.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Op een hoog niveau ziet in het volgende diagram u het Pack HPC-cluster dat u maken.

![HPC-cluster met knooppunten waarop Excel werklasten worden uitgevoerd][scenario]

## <a name="prerequisites"></a>Vereisten

*   **Client-computer** - u moet een Windows-clientcomputer voorbeeld Excel- en SOA-taken aan het cluster. Ook moet u een Windows-computer het implementatiescript Azure PowerShell cluster uitvoeren (als u die implementatiemethode) en

*   **Azure abonnement** - als u niet een Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) in een paar minuten.

*   **Cores quota** - wellicht te verhogen van de quota van cores, vooral als u verschillende clusterknooppunten met multicore VM formaten implementeren. Als u een sjabloon Azure quickstart gebruikt, is het quotum cores in Resource Manager per regio Azure. In dat geval moet u mogelijk verhogen van de quota in een bepaalde regio. Zie [abonnementen Azure, quota's en beperkingen](../azure-subscription-service-limits.md). Een target, [open een verzoek voor ondersteuning van online klant](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) kosteloos verhogen.

*   **Licentie voor Microsoft Office** - als u een afbeelding Marketplace HPC Pack VM gebruiken met Microsoft Excel, een 30-daagse evaluatieversie van Microsoft Excel Professional Plus 2013-computerknooppunten is geïnstalleerd. Na de evaluatieperiode moet u een geldige licentie voor Microsoft Office Excel te voeren werkbelasting te activeren. [Excel activeren](#excel-activation) Zie verderop in dit artikel. 


## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Stap 1. Een Pack van HPC-cluster in Azure instellen

We zien twee opties voor het instellen van het cluster: eerste, met behulp van een sjabloon Azure quickstart en Azure-portal; en tweede, met behulp van een script voor Azure PowerShell implementatie.


### <a name="option-1-use-a-quickstart-template"></a>Optie 1. Gebruik een sjabloon quickstart
Gebruik een sjabloon Azure quickstart snel en gemakkelijk een Pack HPC-cluster in Azure portal implementeren. Als u de sjabloon in de portal opent, krijgt u een eenvoudige gebruikersinterface waarin u de instellingen voor uw cluster invoeren. Hier zijn de stappen. 

>[AZURE.TIP]Als u wilt, gebruikt u een [sjabloon Azure Marketplace](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) die een vergelijkbaar cluster speciaal voor Excel werklasten worden gemaakt. De stappen verschillen enigszins van de volgende.

1.  Ga naar de [pagina op GitHub HPC-Cluster maken](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Als u wilt, kunt u informatie over de sjabloon en de broncode weergeven.

2.  Klik op **distribueren naar Azure** een implementatie met de sjabloon in de Azure portal te starten.

    ![Sjabloon voor Azure implementeren][github]

3.  In de portal, volgt u de parameters voor de sjabloon HPC-cluster.

    een. Op de pagina **Parameters** invoeren of de waarden voor de sjabloonparameters wijzigen. (Klik op het pictogram naast elke instelling voor help-informatie). Voorbeeldwaarden worden weergegeven in het volgende scherm. In dit voorbeeld wordt een cluster met de naam *hpc01* in het *hpc.local* domein dat bestaat uit een hoofd knooppunt en 2 compute nodes. De compute nodes worden gemaakt van een afbeelding HPC Pack VM met Microsoft Excel.

    ![Parameters invoeren][parameters]

    >[AZURE.NOTE]Het knooppunt head VM automatisch wordt gemaakt uit de [meest recente Marketplace-afbeelding](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) van HPC Pack 2012 R2 op Windows Server 2012 R2. De afbeelding is momenteel gebaseerd op HPC Pack 2012 R2 Update 3.
    >
    >COMPUTE knooppunt VMs worden gemaakt van de meest recente afbeelding van de geselecteerde compute knooppunt-familie. Selecteer de optie **ComputeNodeWithExcel** voor de meest recente HPC Pack compute knooppunt afbeelding waarin u een evaluatieversie van Microsoft Excel Professional Plus 2013. Als u wilt een cluster implementeren voor algemene SOA-sessies of Excel UDF-offloading, kiest u de optie **ComputeNode** (zonder Excel hebt geïnstalleerd).

    b. Kies het abonnement.

    c. Maak een groep voor het cluster, zoals *hpc01RG*.

    d. Kies een locatie voor de resourcegroep, zoals de Amerikaanse centrale.

    e. Lees de voorwaarden op de pagina **juridische voorwaarden** . Als u akkoord gaat, klikt u op **aankoop**. Wanneer u klaar bent met het instellen van de waarden voor de sjabloon, klik vervolgens op **maken**.

4.  Wanneer de installatie is voltooid (dit duurt meestal ongeveer 30 minuten), het bestand voor het cluster uit het hoofd clusterknooppunt exporteren. In een latere stap kunt u een certificaat voor de verificatie van de server-side voor beveiligde HTTP-binding op de client deze openbare importeren.

    een. Verbinding maken met het knooppunt head met extern bureaublad vanaf de portal Azure.

     ![Verbinding maken met het knooppunt head][connect]

    b. Gebruik standaard procedures in Certificaatbeheer de statutaire knooppunt certificaat (te vinden onder Cert: \LocalMachine\My) zonder dat de persoonlijke sleutel exporteren. In dit voorbeeld exporteren *CN = hpc01.eastus.cloudapp.azure.com*.

    ![Exporteer het certificaat][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Optie 2. Gebruik het implementatiescript HPC Pack IaaS

Het implementatiescript HPC Pack IaaS biedt een andere veelzijdige manier een Pack HPC-cluster implementeren. Wordt gemaakt een cluster in de klassieke implementatiemodel dat de sjabloon het implementatiemodel Azure Resource Manager worden gebruikt. Het script is ook compatibel met een abonnement in de Global Azure of China Azure-service.

**Aanvullende vereisten**

* **Azure PowerShell** - [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) (versie 0.8.10 of hoger) op de clientcomputer.

* **Implementatiescript HPC Pack IaaS** - downloaden en uitpakken van de nieuwste versie van het script van het [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Controleer de versie van het script door het uitvoeren van `New-HPCIaaSCluster.ps1 –Version`. Dit artikel is gebaseerd op versie 4.5.0 of hoger van het script.

**Het configuratiebestand maken**

 Het implementatiescript HPC Pack IaaS wordt een XML-configuratiebestand gebruikt als invoer die de infrastructuur van de HPC-cluster beschrijft. Vervang de waarden voor uw omgeving in het volgende voorbeeld configuratiebestand voor de implementatie van een cluster dat bestaat uit een hoofd knooppunt en 18 computerknooppunten gemaakt op basis van de compute knooppunt afbeelding die Microsoft Excel bevat. Zie het bestand Manual.rtf in de scriptmap en [een HPC-cluster met het implementatiescript HPC Pack IaaS maken](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)voor meer informatie over het configuratiebestand.

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Opmerkingen over het configuratiebestand**

* De **VMName** van het hoofd knooppunt **moet** hetzelfde zijn als de **servicenaam**of SOA-taken niet uitgevoerd.

* Zorg ervoor dat u **EnableWebPortal** opgeven, zodat het knooppunt head-certificaat wordt gemaakt en geëxporteerd.

* Het bestand bevat een PowerShell-script configuratie na PostConfig.ps1 die wordt uitgevoerd op het knooppunt head. Het volgende voorbeeldscript wordt de verbindingsreeks Azure opslag geconfigureerd de rol compute knooppunt verwijdert uit het hoofd knooppunt en alle knooppunten on line brengt, wanneer ze worden gebruikt. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Het script uitvoeren**

1.  Open de console PowerShell op de clientcomputer als beheerder.

2.  Wijzig de map in de scriptmap (E:\IaaSClusterScript in dit voorbeeld).

    ```
    cd E:\IaaSClusterScript
    ```
    
3.  Als u wilt het Pack HPC-cluster implementeert, moet u de volgende opdracht uitvoeren. In dit voorbeeld wordt ervan uitgegaan dat het bestand bevindt zich in E:\HPCDemoConfig.xml.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

Het implementatiescript HPC Pack wordt uitgevoerd gedurende een bepaalde periode. Wat die het script doet is om te exporteren en de cluster-certificaat downloaden en opslaan in de map documenten van de huidige gebruiker op de clientcomputer. Het script genereert een bericht met de volgende strekking. In een volgende stap moet u het certificaat in het juiste certificaatarchief importeren.    
    
    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Stap 2. Offload van Excel-werkmappen en UDF's uitvoeren vanaf een client in gebouwen

### <a name="excel-activation"></a>Excel activeren

Als u de afbeelding ComputeNodeWithExcel VM voor productie werkbelasting, moet u een geldige licentiecode voor Microsoft Office Excel activeren op de compute nodes. De evaluatieversie van Excel verloopt na 30 dagen anders en Excel-werkmappen wordt uitgevoerd met de COMException (0x800AC472) zal mislukken. 

Opnieuw Excel kunt u een andere 30 dagen evaluatie periode: Meld u aan bij het knooppunt head en clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` berekenen in Excel alle knooppunten via HPC Cluster Manager. U kunt maximaal twee keer opnieuw uitrusten. Daarna moet u een geldige licentie voor Office-productcode opgeven.

Office Professional Plus 2013 geïnstalleerd op de VM-afbeelding is een volume-editie met een generieke Volume License Key (GVLK). Kan worden geactiveerd via de Key Management Service (KMS) / Active Directory-Based-activering (AD-BA) of sleutel MAK (Multiple Activation). 

    * AD-KMS-BA, een bestaande KMS-server gebruiken of een nieuwe met behulp van Microsoft Office 2013 Volume License Pack instellen. (Als u wilt instellen van de server op het knooppunt hoofd.) Vervolgens de KMS-host-sleutel via Internet of telefonisch activeren. Klik op clusrun `ospp.vbs` de KMS-server en poort instellen en activeren van Office op alle Excel-computerknooppunten. 
    
    * Gebruik MAK, eerste clusrun `ospp.vbs` invoeren van de sleutel en activeert u vervolgens alle Excel computerknooppunten via Internet of per telefoon. 

>[AZURE.NOTE]Retail-productcodes voor Office Professional Plus 2013 kunnen niet worden gebruikt voor deze afbeelding VM. Als u geldige sleutels en installatiemedia voor Office of Microsoft Excel-versies dan dit volume edition van Office Professional Plus 2013 hebt, kunt u ze in plaats daarvan. Eerst verwijdert de editie van dit volume en de versie die u hebt geïnstalleerd. De opnieuw geïnstalleerde Excel compute node kan worden vastgelegd als een aangepaste VM-afbeelding gebruiken in een implementatie met schaal.

### <a name="offload-excel-workbooks"></a>Offload van Excel-werkmappen

Ga als volgt te werk om een Excel-werkmap zodat deze wordt uitgevoerd op het Pack HPC-cluster in Azure-offload. Hiervoor hebt u Excel 2010 of 2013 al geïnstalleerd op de clientcomputer.

1. Gebruik een van de opties in stap 1 voor de implementatie van een Pack HPC-cluster met de Excel-knooppunt installatiekopie berekenen. Cluster-certificaatbestand (.cer) en cluster gebruikersnaam en wachtwoord verkrijgen.

2. Importeer het certificaat van de cluster onder Cert: \CurrentUser\Root op de clientcomputer.

3. Zorg ervoor dat Excel is geïnstalleerd. Maak een bestand Excel.exe.config met de volgende inhoud in dezelfde map als Excel.exe op de clientcomputer. Deze stap zorgt u ervoor dat de HPC Pack 2012 R2 Excel COM-invoegtoepassing geladen.

    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
    
4.  De client ingesteld taken kunnen verzenden naar het Pack HPC-cluster. Eén mogelijkheid is de volledige [installatie van HPC Pack 2012 R2 Update 3](http://www.microsoft.com/download/details.aspx?id=49922) downloaden en installeren van de client HPC Pack. U kunt ook downloaden en installeren van de [hulpprogramma's voor HPC Pack 2012 R2 Update 3-client](https://www.microsoft.com/download/details.aspx?id=49923) en de juiste Visual C++ 2010 redistributable voor uw computer ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.  In dit voorbeeld gebruiken we een voorbeeld Excel-werkmap met de naam ConvertiblePricing_Complete.xlsb. U kunt het downloaden [hier](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.  De Excel-werkmap kopiëren naar een werkmap zoals D:\Excel\Run.

7.  Open de Excel-werkmap. Klik op **COM-invoegtoepassingen** op het lint **opstellen** en bevestigen dat de HPC Pack Excel COM-invoegtoepassing wordt geladen.

    ![Excel-invoegtoepassing voor HPC Pack][addin]

8.  De VBA-macro HPCControlMacros in Excel bewerken door de commentaar regels zoals in het volgende script wordt weergegeven. Vervangen door de juiste waarden voor uw omgeving.

    ![Excel-macro's voor HPC Pack][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.  De Excel-werkmap kopiëren naar een map geüpload zoals D:\Excel\Upload. Deze map is opgegeven in de constante HPC_DependsFiles in de VBA-macro.

10. De werkmap op het cluster in Azure uitgevoerd, klikt u op de knop **Cluster** op het werkblad.

### <a name="run-excel-udfs"></a>Excel UDF's uitvoeren

Als u wilt Excel UDF's uitvoeren, de voorgaande stappen 1-3 voor het instellen van de clientcomputer. Voor UDF's van Excel moet u niet de Excel-toepassing geïnstalleerd op de computerknooppunten. Dus bij het maken van het cluster computerknooppunten, kunt u een gewone berekenen knooppunt afbeelding in plaats van de afbeelding van het knooppunt berekenen met Excel.

>[AZURE.NOTE] Er is een maximaal 34 tekens in het Excel 2010 en 2013 cluster connector-dialoogvenster. In dit dialoogvenster kunt u het cluster met de UDF's opgeven. Als de naam van het volledige cluster langer is (bijvoorbeeld hpcexcelhn01.southeastasia.cloudapp.azure.com), past niet in het dialoogvenster. De oplossing is voor het instellen van een variabele alle computers zoals *CCP_IAASHN* met de waarde van de naam van het cluster lang. Voer vervolgens *% CCP_IAASHN %* in het dialoogvenster de clusternaam head knooppunt. 

Nadat het cluster is geïnstalleerd, gaat u verder met de volgende stappen uit te voeren, een voorbeeld van een ingebouwde Excel UDF. Zie de volgende [bronnen](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) het XLL's maken en deze op het cluster IaaS voor aangepaste Excel UDF's.

1.  Een nieuwe Excel-werkmap opent. Klik op **Invoegtoepassingen**op het lint **opstellen** . Klik in het dialoogvenster, klikt u op **Bladeren**, navigeer naar de map %CCP_HOME%Bin\XLL32 en selecteer het monster ClusterUDF32.xll. Als de ClusterUDF32 op de clientcomputer niet bestaat, kopieert u vanuit de map %CCP_HOME%Bin\XLL32 op het hoofd knooppunt.

    ![Selecteer de UDF][udf]

2.  Klik op **bestand** > **Opties** > **Geavanceerde**. Controleer onder de **formules**, **toestaan door de gebruiker gedefinieerde functies voor het uitvoeren van een compute cluster XLL**. Klik op **Opties** en geef de clusternaam van het volledige in **hoofd knooppunt clusternaam**. (Zoals aangegeven eerder dit invoervak is beperkt tot 34 tekens, zodat een lange clusternaam niet past. U kunt hier een hele machine-variabele voor een lange clusternaam.)

    ![De UDF configureren][options]

3.  De UDF-berekening op de cluster worden uitgevoerd, klikt u op de cel met de waarde =XllGetComputerNameC() en druk op Enter. De functie haalt gewoon de naam van de compute node op de UDF wordt uitgevoerd. Voor het eerst wordt uitgevoerd, wordt een dialoogvenster referenties gevraagd voor de gebruikersnaam en het wachtwoord verbinding maken met het cluster IaaS.

    ![UDF uitvoeren][run]

    Er zijn veel cellen te berekenen, druk op Alt-Shift-Ctrl + F9 als u de berekening wilt uitvoeren op alle cellen.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Stap 3. Een SOA-werkbelasting uitvoeren vanaf een client in gebouwen

Algemene SOA-toepassingen uitvoeren op het cluster HPC Pack IaaS, eerst gebruik een van de methoden in stap 1 voor de implementatie van het cluster. Een algemene afbeelding knooppunt in dat geval berekenen omdat Excel niet nodig op de compute nodes opgeven. Volg deze stappen.

1. Na het ophalen van de cluster-certificaat op de clientcomputer onder Cert: \CurrentUser\Root importeren.

2. De [HPC Pack 2012 R2 Update 3 client-hulpprogramma's](https://www.microsoft.com/download/details.aspx?id=49923)en [SDK Update 3 van HPC Pack 2012 R2](http://www.microsoft.com/download/details.aspx?id=49921) installeren. Deze hulpprogramma's kunnen u ontwikkelen en uitvoeren van SOA-clienttoepassingen.

3. Download de HelloWorldR2- [voorbeeldcode](https://www.microsoft.com/download/details.aspx?id=41633). Open het HelloWorldR2.sln in Visual Studio 2010 of 2012.

4. Bouw eerst het project EchoService. De service aan het cluster IaaS vervolgens implementeren op dezelfde manier implementeren van een cluster op gebouwen. Zie het bestand Readme.doc in HelloWordR2 voor gedetailleerde stappen. Wijzigen en de HellWorldR2 en andere projecten zoals beschreven in de volgende sectie voor het genereren van de SOA-clienttoepassingen die worden uitgevoerd op een cluster Azure IaaS bouwen.

### <a name="use-http-binding-with-azure-storage-queue"></a>Gebruik HTTP-binding met Azure storage queue

Voor het gebruik van HTTP-binding aan een wachtrij Azure opslag, maken een paar wijzigingen aan de voorbeeldcode.

* Naam van het cluster worden bijgewerkt.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* Eventueel gebruik van de standaard TransportScheme in SessionStartInfo of expliciet instelt op Http.

```
    info.TransportScheme = TransportScheme.Http;
```

* Gebruik standaard binding voor de BrokerClient.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Of u expliciet de basicHttpBinding.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* U kunt desgewenst de vlag UseAzureQueue ingesteld op true in SessionStartInfo. Als niet is ingesteld, wordt deze ingesteld op true wanneer de naam van het cluster heeft Azure domeinachtervoegsels en de TransportScheme is Http.

    ```
    info.UseAzureQueue = true;
```

###<a name="use-http-binding-without-azure-storage-queue"></a>Gebruik HTTP-binding zonder wachtrij Azure opslag

Voor het gebruik van HTTP-binding zonder een wachtrij Azure opslag, de vlag UseAzureQueue expliciet wordt ingesteld op false in de SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Gebruik NetTcp-binding

De configuratie is vergelijkbaar met het verbinding maken met een cluster van gebouwen voor het gebruik van NetTcp-binding. U moet een paar eindpunten op het hoofd knooppunt VM openen. Als u de implementatie van HPC Pack IaaS script voor het maken van het cluster gebruikt, bijvoorbeeld de eindpunten in de klassieke Azure portal als volgt instellen.


1. Stop de VM.

2. Toevoegen van de TCP-poorten 9090, 9087, 9091, 9094 voor de sessie Broker, werknemer en Data services, respectievelijk Broker

    ![Eindpunten configureren][endpoint]

3. Start de VM.

De SOA-clienttoepassing vereist geen wijzigingen, met uitzondering van de naam van de kop naar de volledige naam van de IaaS cluster wijzigen.

## <a name="next-steps"></a>Volgende stappen

* Zie [deze bronnen](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) voor meer informatie over het uitvoeren van de werklast van Excel met HPC Pack.

* Zie [SOA-Services beheren in Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) voor meer informatie over het implementeren en beheren van SOA-services met HPC Pack.

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png
