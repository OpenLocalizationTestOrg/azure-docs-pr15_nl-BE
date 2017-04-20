<properties
   pageTitle="De rollen configureren voor een Service Azure Cloud met Visual Studio | Microsoft Azure"
   description="Informatie over het instellen en configureren van rollen voor Azure cloud-services met behulp van Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configure-the-roles-for-an-azure-cloud-service-with-visual-studio"></a>De rollen configureren voor een Service Azure Cloud met Visual Studio

Een Azure cloud-service kan een of meer werknemers of web-rollen hebben. Voor elke rol die u wilt definiëren hoe die rol wordt ingesteld en tevens configureren hoe die rol wordt uitgevoerd. Zie voor meer informatie over rollen in cloud-services, de video- [Inleiding tot Azure Cloud Services](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). De informatie voor uw cloud-service is opgeslagen in de volgende bestanden:

- **ServiceDefinition.csdef**

    Het definitiebestand definieert de runtime-instellingen voor uw cloud-service, inclusief welke rollen zijn vereist, de eindpunten en de grootte van de virtuele machine. Geen van de gegevens die zijn opgeslagen in dit bestand kan worden gewijzigd wanneer de functie wordt uitgevoerd.

- **ServiceConfiguration.cscfg**

    Het configuratiebestand van de service wordt geconfigureerd voor hoeveel exemplaren van een functie worden uitgevoerd en de waarden van de instellingen die zijn gedefinieerd voor een rol. De gegevens die zijn opgeslagen in dit bestand kan worden gewijzigd terwijl de functie wordt uitgevoerd.

Als u voor het opslaan van verschillende waarden voor deze instellingen voor de manier waarop de functie wordt uitgevoerd, kunt u meerdere configuraties hebt. U kunt de configuratie van een andere service voor elke omgeving. U kunt bijvoorbeeld de opslag account verbindingsreeks op de lokale opslag van Azure emulator gebruiken in de configuratie van een lokale service en maak een andere configuratie van de service voor het gebruik van de Azure opslag in de cloud instellen.

Wanneer u een nieuwe Azure cloud-service in Visual Studio maakt, worden twee configuraties gemaakt. Deze configuraties worden toegevoegd aan het project Azure. De configuraties worden benoemd:

- ServiceConfiguration.Cloud.cscfg

- ServiceConfiguration.Local.cscfg

## <a name="configure-an-azure-cloud-service"></a>Een Azure cloud-service configureren

Zoals in de volgende afbeelding wordt weergegeven, kunt u in Visual Studio een Azure cloud service in Solution Explorer configureren.

![Cloud-Service configureren](./media/vs-azure-tools-configure-roles-for-cloud-service/IC713462.png)

### <a name="to-configure-an-azure-cloud-service"></a>Een Azure cloud-service configureren

1. Configureren van elke rol in het project Azure in **Solution Explorer**, open het snelmenu voor de rol in het project Azure en kies vervolgens **Eigenschappen**.

    Een pagina met de naam van de rol wordt weergegeven in de editor van Visual Studio. De pagina bevat de velden voor het tabblad **configuratie** .

1. Kies de naam van de configuratie van de service die u wilt bewerken in de lijst van de **Configuratie van de Service** .

    Als u wijzigingen aanbrengen op alle van de configuraties voor de rol wilt, kunt u **Alle configuraties**.

    >[AZURE.IMPORTANT] Als u een specifieke service configureren, worden sommige eigenschappen zijn uitgeschakeld omdat ze kunnen alleen worden ingesteld voor alle configuraties. Als u wilt bewerken, moet u alle configuraties.

    U kunt nu een tabblad voor het bijwerken van de eigenschappen ingeschakeld op die weergave.

## <a name="change-the-number-of-role-instances"></a>Wijzig het aantal exemplaren van de rol

Voor betere prestaties van uw cloud-service, kunt u het aantal exemplaren van een rol die worden uitgevoerd op basis van het aantal gebruikers of de belasting voor een bepaalde functie verwacht. Een afzonderlijke virtuele machine wordt gemaakt voor elk exemplaar van een rol wanneer de cloud-service wordt uitgevoerd in Azure. Dit heeft invloed op de facturering voor de implementatie van deze cloud-service. Zie voor meer informatie over facturering, [begrijp wat uw factuur voor Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-number-of-instances-for-a-role"></a>Het aantal exemplaren voor een rol wijzigen

1. Kies het tabblad **configuratie** .

1. Kies in de lijst **Service** , de configuratie van de service die u wilt bijwerken.

    >[AZURE.NOTE] U kunt instellen dat de telling van het exemplaar voor de configuratie van een specifieke service of voor alle configuraties.

1. Voer het aantal exemplaren dat u wilt starten voor deze rol in het tekstvak **aantal exemplaar** .

    >[AZURE.NOTE] Elk exemplaar wordt uitgevoerd op een afzonderlijke virtuele machine wanneer u uw service cloud naar Azure publiceren.

1. Klik op de knop **Opslaan** op de werkbalk om deze wijzigingen opslaan in het bestand van de configuratie.

## <a name="manage-connection-strings-for-storage-accounts"></a>Verbindingstekenreeksen voor opslag accounts beheren

U kunt toevoegen, verwijderen of wijzigen verbindingsreeksen voor uw configuraties. U kunt verschillende tekenreeksen voor verschillende configuraties. Bijvoorbeeld: u kunt een lokale verbindingsreeks voor de configuratie van een lokale service met een waarde van `UseDevelopmentStorage=true`. U kunt ook configureren een cloud service die gebruikmaakt van een account voor opslag in Azure.

>[AZURE.WARNING] Wanneer u de sleutel accountgegevens Azure opslag voor een verbindingsreeks opslag account invoert, wordt deze informatie lokaal opgeslagen in het configuratiebestand van de service. Deze informatie is echter momenteel niet opgeslagen als gecodeerde tekst.

Met behulp van een andere waarde voor de configuratie van de service, er geen andere tekenreeksen in uw cloud-service of uw code te wijzigen wanneer u uw cloud-service op Azure publiceert. U kunt dezelfde naam voor de verbindingsreeks in de code en de waarde zullen afwijken, afhankelijk van de configuratie van de service die u selecteert wanneer u de service cloud maakt of wanneer u deze publiceert.

### <a name="to-manage-connection-strings-for-storage-accounts"></a>Verbindingstekenreeksen voor opslag accounts beheren

1. Kies het tabblad **Instellingen** .

1. Kies in de lijst **Service** , de configuratie van de service die u wilt bijwerken.

    >[AZURE.NOTE] U kunt tekenreeksen voor verbindingen voor een specifieke service-configuratie bijwerken, maar als u wilt toevoegen of verwijderen van een verbindingsreeks moet u alle configuraties.

1. Als u wilt een verbindingsreeks toevoegen, klik op de knop **Instelling toevoegen** . Er wordt een nieuwe vermelding toegevoegd aan de lijst.

1. Typ in het vak **naam** de naam die u wilt gebruiken voor de verbinding string.

1. Kies in de vervolgkeuzelijst **Type** de **Verbindingsreeks**.

1. De waarde voor de tekenreeks wilt wijzigen, kiest u de knop met het weglatingsteken (...). Het dialoogvenster **Verbindingsreeks opslag maken** wordt weergegeven.

1. Voor het gebruik van de account lokale opslag emulator, kiest u de optie **Microsoft Azure opslag emulator** knop en kies vervolgens de knop **OK** .

1. Als u een account voor opslag in Azure, kiest u de optie **uw abonnement** knop en selecteer de gewenste opslag.

1. Aangepaste referenties kiest u de knop opties **handmatig opgegeven referenties** . Voer de naam van de opslag en de primaire of de tweede sleutel. Zie voor informatie over het maken van een account voor de opslag en het invoeren van de details voor de opslag van het dialoogvenster **Verbindingsreeks opslag maken** , [voorbereiden om te publiceren of een Azure toepassing vanuit Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. U verwijdert een verbindingsreeks, selecteert u de verbindingsreeks en kies vervolgens de knop **Instelling verwijderen** .

1. Kies het pictogram **Opslaan** op de werkbalk om deze wijzigingen opslaan in het bestand van de configuratie.

1. Als u de verbindingsreeks in het configuratie-bestand, moet u de waarde van de configuratie-instelling. De volgende code toont een voorbeeld waarin de blob-opslag is gemaakt en gegevens met een verbindingstekenreeks geüpload `MyConnectionString` van het configuratie-bestand als een gebruiker **Button1** op de pagina Default.aspx in de Webrol voor een Azure cloud-service. Voeg de volgende Default.aspx.cs-instructies gebruiken:

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Default.aspx.cs in de ontwerpweergave openen en een knop toevoegen vanuit de werkset. Voeg de volgende code aan de `Button1_Click` methode. Deze code maakt gebruik van `GetConfigurationSettingValue` om de waarde van het configuratiebestand van de service voor de verbindingsreeks. Wordt gemaakt in de opslag account waarnaar wordt verwezen in de verbindingsreeks een blob `MyConnectionString` en ten slotte wordt tekst toegevoegd aan de blob.

    ```
    protected void Button1_Click(object sender, EventArgs e)
    {
        // Setup the connection to Azure Storage
        var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("MyConnectionString"));
        var blobClient = storageAccount.CreateCloudBlobClient();
        // Get and create the container
        var blobContainer = blobClient.GetContainerReference("quicklap");
        blobContainer.CreateIfNotExists();
        // upload a text blob
        var blob = blobContainer.GetBlockBlobReference(Guid.NewGuid().ToString());
        blob.UploadText("Hello Azure");

    }
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Aangepaste instellingen voor uw Azure cloud-service toevoegen

Aangepaste instellingen in het configuratiebestand van de service kunnen u een naam en een waarde voor een tekenreeks voor een specifieke service-configuratie toevoegen. U kunt deze instelling gebruiken om een functie in de cloud-service configureren door het lezen van de instelling en deze waarde te bepalen van de logica in uw code gebruiken. U kunt deze configuratiewaarden service wijzigen zonder opnieuw op te bouwen uw-pakket of wanneer uw cloud-service wordt uitgevoerd. Uw code kunt controleren voor kennisgevingen van wanneer een instelling wordt gewijzigd. Zie [De gebeurtenis RoleEnvironment.Changing](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

U kunt toevoegen, verwijderen of wijzigen van aangepaste instellingen voor uw configuraties. U kunt verschillende waarden voor deze tekenreeksen voor verschillende configuraties.

Met behulp van een andere waarde voor de configuratie van de service, er geen andere tekenreeksen in uw cloud-service of uw code te wijzigen wanneer u uw cloud-service op Azure publiceert. U kunt dezelfde naam voor de tekenreeks in de code en de waarde zullen afwijken, afhankelijk van de configuratie van de service die u selecteert wanneer u de service cloud maakt of wanneer u deze publiceert.

### <a name="to-add-custom-settings-to-use-in-your-azure-cloud-service"></a>Aangepaste instellingen voor uw Azure cloud-service toevoegen

1. Kies het tabblad **Instellingen** .

1. Kies in de lijst **Service** , de configuratie van de service die u wilt bijwerken.

    >[AZURE.NOTE] U kunt tekenreeksen voor de configuratie van een specifieke service bijwerken, maar als u wilt toevoegen of verwijderen van een tekenreeks, moet u **Alle configuraties**.

1. Als u een tekenreeks toevoegen, klik op de knop **Instelling toevoegen** . Er wordt een nieuwe vermelding toegevoegd aan de lijst.

1. Typ in het vak **naam** de naam die u wilt gebruiken voor de tekenreeks.

1. Kies in de vervolgkeuzelijst **Type** **String**.

1. Wilt toevoegen of wijzigen van de waarde voor de tekenreeks, typt u de nieuwe waarde in het vak **waarde** .

1. Als u wilt verwijderen van een tekenreeks, selecteert u de reeks en kies vervolgens de knop **Instelling verwijderen** .

1. Klik op de knop **Opslaan** op de werkbalk om deze wijzigingen opslaan in het bestand van de configuratie.

1. Als u de tekenreeks in het configuratie-bestand, moet u de waarde van de configuratie-instelling.

    U moet ervoor zorgen dat de volgende via overzichten zijn al toegevoegd aan Default.aspx.cs, net als in de vorige procedure.

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Voeg de volgende code aan de `Button1_Click` methode voor toegang tot deze tekenreeks op dezelfde manier dat u toegang een verbindingsreeks tot. Uw code kan uitvoeren sommige specifieke code op basis van de waarde van de reeks instellingen voor het configuratiebestand van de service die wordt gebruikt.

    ```
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("MySetting");
    if (settingValue == “ThisValue”)
    {
    // Perform these lines of code
    }
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Beheren van lokale opslag voor elk exemplaar van de rol

U kunt lokale bestandsopslag systeem voor elk exemplaar van een rol toevoegen. U kunt lokale gegevens opslaan hier dat niet hoeft te worden geopend door andere rollen. Hier kunnen alle gegevens die u niet wilt opslaan in een tabel of blob storage SQL-Database worden opgeslagen. U kunt bijvoorbeeld deze lokale opslag in cachegegevens die u moet mogelijk opnieuw worden gebruikt. Deze opgeslagen gegevens niet toegankelijk voor andere exemplaren van een rol. 

Lokale opslag-instellingen gelden voor alle configuraties. U kunt alleen toevoegen, verwijderen of wijzigen van de lokale opslag voor alle configuraties.

### <a name="to-manage-local-storage-for-each-role-instance"></a>Voor het beheren van lokale opslag voor elk exemplaar van de rol

1. Klik op het tabblad **Lokale opslag** .

1. Kies in de lijst **Service** **Alle configuraties**.

1. Een lokale opslag als vermelding wilt toevoegen, klik op de knop **Lokale opslag toevoegen** . Er wordt een nieuwe vermelding toegevoegd aan de lijst.

1. Typ in het vak **naam** de naam die u wilt gebruiken voor deze lokale opslag.

1. Typ in het tekstvak **grootte** de grootte in MB die u nodig hebt voor deze lokale opslag.

1. Schakel het selectievakje **Prullenbak Clean op rol** om de gegevens in deze lokale opslag verwijderen wanneer de virtuele machine voor deze rol gerecycled wordt.

1. Kies voor het bewerken van een bestaande vermelding in de lokale opslag, de rij die u wilt bijwerken. Vervolgens kunt u de velden, zoals beschreven in de vorige stappen.

1. Een lokale opslag als vermelding wilt verwijderen, kiest u het item opslag in de lijst en kies vervolgens de knop **Verwijderen lokale opslag** .

1. Als u deze wijzigingen opslaan in de configuratie-bestanden, kies het pictogram **Opslaan** op de werkbalk.

1. Voor toegang tot de lokale opslag die u hebt toegevoegd in het configuratiebestand van de service, moet u de waarde van de lokale resource configuratie-instelling. Gebruik de volgende regels code toegang krijgen tot deze waarde en maakt een bestand met de naam **MyStorageTest.txt** en een lijn van de testgegevens naar dat bestand schrijven. U kunt toevoegen dat deze code in de `Button_Click` methode die u in de voorgaande procedures gebruikt:

1. U moet ervoor zorgen dat de volgende via overzichten worden toegevoegd aan Default.aspx.cs:

    ```
    using System.IO;
    using System.Text;
    ```

1. Voeg de volgende code aan de `Button1_Click` methode. Hiermee wordt het bestand in de lokale opslag en testgegevens schrijft naar dat bestand.

    ```
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("LocalStorage1");

    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyStorageTest.txt" };
    String filePath = Path.Combine(paths);

    using (FileStream writeStream = File.Create(filePath))
    {
          Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
          writeStream.Write(textToWrite, 0, textToWrite.Length);
    }
    ```

1. (Optioneel) Deze als bestand wilt weergeven die u hebt gemaakt toen u uw cloud-service lokaal worden uitgevoerd, gebruikt u de volgende stappen uit:

  1. De Webrol uitvoeren en selecteer **Button1** om ervoor te zorgen dat de code binnen `Button1_Click` wordt aangeroepen.

  1. Opent het snelmenu voor het Azure pictogram in het systeemvak en kiest u **Berekenen Emulator gebruikersinterface weergeven**. Het dialoogvenster **Azure berekenen Emulator** wordt weergegeven.

  1. Selecteer de Webrol.

  1. Kies in het menu **Extra**, **lokale winkel openen**. Er verschijnt een venster van Windows Verkenner.

  1. Op de menubalk **MyStorageTest.txt** invoeren in het tekstvak **Zoeken** en klik op **Enter** om de zoekopdracht te starten.

    Het bestand wordt weergegeven in de zoekresultaten.

  1. Aan de inhoud van het bestand en opent u het snelmenu voor het bestand **openen**te kiezen.

## <a name="collect-cloud-service-diagnostics"></a>Cloud service diagnostische gegevens verzamelen

U kunt diagnostische gegevens verzamelen voor uw Azure cloud-service. Deze gegevens worden toegevoegd aan een account voor opslag. U kunt verschillende tekenreeksen voor verschillende configuraties. Bijvoorbeeld: u kunt een account met lokale opslag voor de configuratie van een lokale service met een waarde van UseDevelopmentStorage = true. U kunt ook configureren een cloud service die gebruikmaakt van een account voor opslag in Azure. Zie voor meer informatie over Azure diagnostische gegevens vastleggen gegevens verzamelen door met behulp van Azure diagnostische gegevens.

>[AZURE.NOTE] Configuratie van de lokale service is reeds geconfigureerd om lokale resources te gebruiken. Als u de configuratie van de cloud-service voor het publiceren van uw Azure cloud-service gebruikt, wordt de tekenreeks die u opgeeft wanneer u publiceert ook gebruikt voor de verbindingsreeks diagnostics tenzij u een verbindingsreeks hebt opgegeven. Als u uw cloud-service met behulp van Visual Studio inpakt, wordt de verbindingsreeks in de configuratie van de service niet gewijzigd.

### <a name="to-collect-cloud-service-diagnostics"></a>Cloud service diagnostische gegevens verzamelen

1. Kies het tabblad **configuratie** .

1. Kies in de lijst **Service-configuratie** de configuratie van de service die u wilt bijwerken of kiest u **Alle configuraties**.

    >[AZURE.NOTE] U kunt de account van de opslag voor een specifieke service-configuratie bijwerken, maar als u wilt in- of uitschakelen van diagnostische gegevens moet u alle configuraties.

1. Als u diagnostische gegevens, selecteert u het selectievakje **Diagnostics inschakelen** .

1. De waarde voor de opslag-account wilt wijzigen, kiest u de knop met het weglatingsteken (...).

    Het dialoogvenster **Verbindingsreeks opslag maken** wordt weergegeven.

1. Voor het gebruik van een lokale verbindingsreeks Azure opslag emulator optie en klik op **OK** .

1. Een opslag-account die is gekoppeld aan uw abonnement Azure kiest u de optie **uw abonnement** .

1. Een opslag-account voor de lokale verbindingsreeks kiest u de optie **handmatig opgegeven referenties** .

    Zie voor meer informatie over het maken van een account voor de opslag en het invoeren van de details voor de opslag van het dialoogvenster **Verbindingsreeks opslag maken** [voorbereiden om te publiceren of een Azure toepassing vanuit Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Kies de opslag-account die u wilt gebruiken in de **naam van de Account**.

    Als u handmatig invoeren van de referenties van de account opslag kopiëren of typt u de primaire sleutel in **sleutel rekening**. Deze sleutel kan worden gekopieerd vanaf de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885). Deze sleutel is, volgt uit de weergave van de **Rekeningen voor opslag** in [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)kopiëren:
    
  1. Selecteer de opslag-account die u wilt gebruiken voor uw cloud-service.

  1. Kies de knop **Sleutels beheren van toegang** aan de onderkant van het scherm. Het dialoogvenster **Toegangstoetsen beheren** wordt weergegeven.

  1. Als u wilt kopiëren op de toegangstoets, klik op de knop **kopiëren naar Klembord** . Nu kunt u deze sleutel in het **Account-sleutel** veld plakken.

1. Gebruik van de opslag-account die u, als de verbindingsreeks opgeeft voor diagnostische gegevens (en caching) wanneer u uw cloud-service op Azure publiceert, schakel het selectievakje **Update ontwikkeling opslag verbindingsreeksen voor diagnostiek en Caching met Azure opslag referenties wanneer u publiceert naar Azure** .

1. Klik op de knop **Opslaan** op de werkbalk om deze wijzigingen opslaan in het bestand van de configuratie.

## <a name="change-the-size-of-the-virtual-machine-used-for-each-role"></a>De grootte van de virtuele machine die wordt gebruikt voor elke rol wijzigen

U kunt instellen dat de grootte van de virtuele machine voor elke rol. U kunt alleen deze grootte voor alle configuraties instellen. Als u een kleiner computer selecteert, is vervolgens minder CPU cores, geheugen en lokale schijf opslag toegewezen. De toegewezen bandbreedte is ook kleiner. Zie voor meer informatie over deze formaten en de toegewezen resources [voor Cloud Services](cloud-services/cloud-services-sizes-specs.md).

De middelen die nodig zijn voor elke virtuele machine in Azure is van invloed op de kosten van het uitvoeren van uw service cloud in Azure. Zie voor meer informatie over facturering Azure [begrijp wat uw factuur voor Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-size-of-the-virtual-machine"></a>De grootte van de virtuele machine wijzigen

1. Kies het tabblad **configuratie** .

1. Kies in de lijst **Service** **Alle configuraties**.

1. Selecteer de grootte van de virtuele machine voor deze rol, kiest u de juiste grootte in de lijst **grootte VM** .

1. Klik op de knop **Opslaan** op de werkbalk om deze wijzigingen opslaan in het bestand van de configuratie.

## <a name="manage-endpoints-and-certificates-for-your-roles"></a>Eindpunten en certificaten voor uw rollen beheren

U configureren Inbelnetwerk eindpunten door te geven van het protocol, het nummer van de poort en HTTPS, de gegevens van het SSL-certificaat. HTTP, HTTPS en TCP-ondersteuning voor releases vóór juni 2012. De release van juni 2012 ondersteunt deze protocollen en UDP. Kunt u UDP voor invoer eindpunten in de compute-emulator. U kunt dat protocol alleen gebruiken voor interne eindpunten.

Ter verbetering van de beveiliging van uw Azure cloud-service, kunt u eindpunten die het HTTPS-protocol gebruiken. Bijvoorbeeld als u een cloud-service die door klanten wordt gebruikt voor inkooporders hebt, wilt u dat om ervoor te zorgen dat hun informatie beveiligd is met behulp van SSL.

U kunt ook de eindpunten die intern worden gebruikt of extern toevoegen. Externe eindpunten zijn invoer eindpunten genoemd. Een eindpunt invoer kunt een ander toegangspunt voor gebruikers met uw cloud-service. Als u een WCF-service hebt, is het raadzaam om een interne eindpunt voor een web-functie gebruiken voor toegang tot deze service beschikbaar te maken.

>[AZURE.IMPORTANT] U kunt alleen bijwerken eindpunten voor alle configuraties.

Als u HTTPS-eindpunten toevoegen, moet u een SSL-certificaat gebruiken. U kunt hiervoor het koppelen van certificaten aan uw rol voor alle configuraties en gebruik deze voor de eindpunten.

>[AZURE.IMPORTANT] Deze certificaten worden niet geleverd met uw service. U moet uw certificaten afzonderlijk uploaden naar Azure via de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

Beheer van certificaten die u aan uw configuraties koppelt alleen van toepassing als uw cloud-service wordt uitgevoerd in Azure. Wanneer uw cloud-service wordt uitgevoerd in de omgeving van plaatselijke ontwikkeling, wordt een standaardindeling voor certificaten die wordt beheerd door de emulator Azure compute gebruikt.

### <a name="to-add-a-certificate-to-a-role"></a>Een certificaat toevoegen aan een rol

1. Klik op het tabblad **certificaten** .

1. Kies in de lijst **Service** **Alle configuraties**.

    >[AZURE.NOTE] Als u wilt toevoegen of verwijderen van certificaten, moet u alle configuraties. U kunt de naam en de blauwdruk voor een specifieke service-configuratie bijwerken als dat nodig is.

1. Als u wilt een certificaat voor deze rol wilt toevoegen, klik op de knop **Certificaat toevoegen** . Er wordt een nieuwe vermelding toegevoegd aan de lijst.

1. Voer in het vak **naam** de naam voor het certificaat.

1. Kies de locatie voor het certificaat dat u wilt toevoegen in de lijst **Locatie** .

1. Kies de winkel die u wilt gebruiken om het certificaat te selecteren in de lijst **Naam opslaan** .

1. Als u wilt toevoegen het certificaat, klik op de knop weglatingsteken (...). Het dialoogvenster **Windows-beveiliging** wordt weergegeven.

1. Kies het certificaat dat u wilt gebruiken in de lijst en klik op **OK** .

    >[AZURE.NOTE] Wanneer u een certificaat uit het certificaatarchief toevoegen, worden alle tussenliggende certificaten automatisch toegevoegd aan de configuratie-instellingen voor u. Deze tussenliggende certificaten moeten ook worden geüpload naar Azure alleen goed configureren uw service voor SSL.

1. Als u wilt een certificaat wilt verwijderen, kiest u het certificaat en kies vervolgens de knop **Certificaat verwijderen** .

1. Kies het pictogram **Opslaan** in de werkbalk deze wijzigingen in de configuratie-bestanden wilt opslaan.

### <a name="to-manage-endpoints-for-a-role"></a>Voor het beheren van eindpunten voor een rol

1. Kies het tabblad **eindpunten** .

1. Kies in de lijst **Service** **Alle configuraties**.

1. Als u wilt toevoegen een eindpunt, klik op de knop **Toevoegen eindpunt** . Er wordt een nieuwe vermelding toegevoegd aan de lijst.

1. Typ in het vak **naam** de naam die u wilt gebruiken voor dit eindpunt.

1. Kies het type van het eindpunt dat u nodig hebt in de lijst **Type** .

1. Het protocol voor het eindpunt dat u moet kiezen uit de lijst met **protocollen** .

1. Als het eindpunt invoer, in het tekstvak **Openbare poort** Voer de openbare poort te gebruiken.

1. Typ in het tekstvak **Poort voor particuliere** de particuliere poort te gebruiken.

1. Als het eindpunt is het https-protocol vereist, kiest u een certificaat wilt gebruiken in de lijst **Naam van SSL-certificaat** .

    >[AZURE.NOTE] In deze lijst staan de certificaten die u hebt toegevoegd voor deze rol in het tabblad **certificaten** .

1. Klik op de knop **Opslaan** op de werkbalk om deze wijzigingen in de configuratie-bestanden opslaan.

## <a name="next-steps"></a>Volgende stappen
Lezen van [een Project Azure configureren](vs-azure-tools-configuring-an-azure-project.md)voor meer informatie over Azure projecten in Visual Studio. Lezen van [Schema: referentie](https://msdn.microsoft.com/library/azure/dd179398)voor meer informatie over het schema van de service cloud.
