<properties
   pageTitle="Voorbereiden om te publiceren of een Azure toepassing vanuit Visual Studio | Microsoft Azure"
   description="Informatie over de procedures voor het accountservices voor cloud en opslag instellen en configureren van uw toepassing Azure."
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

# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Voorbereiden om te publiceren of een Azure toepassing vanuit Visual Studio

## <a name="overview"></a>Overzicht

Voordat u een cloud service project publiceren kunt, moet u de volgende services instellen:

- Een **cloud-service** uit te voeren van de rollen in de Azure-omgeving

- Een **opslag account** die toegang tot de services Blob, wachtrij en tabel biedt.

Gebruik de volgende procedures om deze services instellen en configureren van uw toepassing


## <a name="create-a-cloud-service"></a>Een cloud-service maken

Als u wilt publiceren een cloud-service naar Azure, maakt u eerst een cloud-service, die uw rollen in de Azure-omgeving wordt uitgevoerd. Zoals beschreven in de sectie **een cloud-service via de klassieke Azure portal maken**, verderop in dit onderwerp, kunt u een cloud-service in de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885). U kunt ook een cloud-service maken in Visual Studio met behulp van de wizard publiceren.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Een cloud-service maken met behulp van Visual Studio

1. Open het snelmenu voor het project Azure en kies **publiceren**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)

1. Als u nog niet ingelogd, log in met uw gebruikersnaam en wachtwoord voor het Microsoft-account of de organisatie-account die is gekoppeld aan uw abonnement op Azure.

1. Kies de knop **volgende** om door te gaan naar de pagina **Instellingen** .

    ![Algemene instellingen van Publishing Wizard](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)

1. Kies **Nieuw**in de lijst met **Cloud Services** . Het dialoogvenster **Azure Services maken** verschijnt.

1. Voer de naam van uw cloud-service. De naam maakt deel uit van de URL voor uw service en daarom moet uniek zijn. De naam is niet hoofdlettergevoelig.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Een cloud-service maken met behulp van de klassieke Azure portal

1. Log in om de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkId=253103) op de website van Microsoft.

1. (optioneel) Voor een lijst van cloud-services die u al hebt gemaakt, kiest u de koppeling Cloud Services aan de linkerkant van de pagina.

1. Kies de **+** pictogram in de linkerbenedenhoek hoek en **Cloud Service** kiest in het menu dat wordt weergegeven. Een ander scherm met twee opties, **Snelle** en **Aangepaste maakt**, wordt weergegeven. Als u **Snel maken**, kunt u een cloud-service maken door alleen te geven de URL en de regio waar het fysiek ondergebracht. Als u **Aangepaste maakt**, kunt u een cloud-service onmiddellijk publiceren door te geven van een pakket (.cspkg-bestand), een configuratiebestand (.cscfg) en een certificaat. Aangepaste maken is niet nodig als u van plan bent uw cloud-service met behulp van de opdracht **publiceren** in Azure project publiceren. De opdracht **publiceren** is in het snelmenu voor een Azure project beschikbaar.

1. Kies **Snel maken** voor het publiceren van uw cloud-service later met behulp van Visual Studio.

1. Geef een naam voor uw cloud-service. De volledige URL wordt weergegeven naast de naam.

1. Kies de regio waar de meeste van de gebruikers in de lijst.

1. Kies onderaan in het venster de koppeling **Cloud-Service maken** .

## <a name="create-a-storage-account"></a>Maak een account voor opslag

Een opslag-account biedt toegang tot de services Blob, wachtrij en tabel. Met behulp van Visual Studio of de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkId=253103)kunt u een account voor opslag.

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Een opslag-account maken met behulp van Visual Studio

1. In de **Solution Explorer**, open het snelmenu voor het knooppunt **opslag** , en kies vervolgens **Opslag Account maken**.

    ![Maak een nieuwe account met Azure opslag](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)

1. Selecteer of Voer de volgende gegevens voor de nieuwe account voor de opslag van het dialoogvenster **Opslag Account maken** .
    - De Azure abonnement dat u wilt de opslag-account toevoegen.
    - De naam die u wilt gebruiken voor de nieuwe account voor opslag.
    - De regio of affiniteit groep (bijvoorbeeld West-Amerikaanse of Oost-Azië).
    - Het type replicatie die u wilt gebruiken voor de opslag-account, zoals Geo-redundante.

1. Wanneer u klaar bent, kiest u **maken**. De nieuwe opslag-account wordt weergegeven in de lijst van de **opslag** in **Server Explorer**.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Een opslag-account maken met behulp van de klassieke Azure portal

1. Log in om de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkId=253103) op de website van Microsoft.

1. (Optioneel) Als u wilt uw opslag rekeningen bekijken, kiest u de koppeling van de **opslag** in het deelvenster aan de linkerkant van de pagina.

1. Kies in de linkerbenedenhoek van de pagina, de **+** pictogram.

1. In het menu dat verschijnt, kiest u **opslag**, en kies vervolgens **Snel maken**.

1. De opslag-account een naam die tot een unieke url leidt geven.

1. Uw cloud-service een naam geven. De volledige URL wordt weergegeven naast de naam.

1. Kies een regio waar de meeste van de gebruikers in de lijst van regio's.

1. Geef op of u wilt inschakelen, geo-replicatie. Als u de geo-replicatie inschakelt, wordt uw gegevens worden opgeslagen op meerdere fysieke locaties te verminderen de kans op verlies. Deze functie maakt opslag duurder, maar kunt u de kosten beperken door het inschakelen van geo-locatie als u de account van de opslag in plaats van de functie later maakt. Zie voor meer informatie, [Geo-replicatie](http://go.microsoft.com/fwlink/?LinkId=253108).

1. Kies onderaan in het venster de koppeling **Opslag Account maken** .

Nadat u uw account opslag maakt, ziet u de URL's die u gebruiken kunt voor toegang tot bronnen in elk van de Azure storage-services en de primaire en secundaire toegangstoetsen voor uw account. Met deze sleutels kunt u verificatie ten opzichte van de storage-services aanvragen.

>[AZURE.NOTE] De secundaire sneltoets biedt dezelfde toegang tot uw account opslag als primaire toegangstoets en wordt gegenereerd als een back-up moet uw primaire toegangstoets worden aangetast. Daarnaast is het raadzaam de toegangstoetsen regelmatig opnieuw te genereren. Kunt u een instelling van de tekenreeks voor de secundaire sleutel gebruiken terwijl u de primaire sleutel opnieuw genereren en vervolgens kunt u het gebruik van de primaire sleutel van geregenereerde terwijl u de secundaire sleutel opnieuw genereren.

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Uw app voor het gebruik van de diensten van de opslag-account configureren

Een rol die toegang heeft tot de opslagservices opslag Azure Services gebruikt die u hebt gemaakt, moet u configureren. Hiervoor kunt u meerdere configuraties voor uw project Azure. Standaard worden twee gemaakt in het project Azure. Met behulp van meerdere configuraties, kunt u dezelfde verbindingsreeks opgeeft in uw code gebruiken, maar hebben een andere waarde voor een verbindingsreeks in de configuratie van de service. Zo kunt u een configuratie van de service uit te voeren en fouten opsporen in uw toepassing lokaal met behulp van de emulator Azure opslag en een configuratie van de andere service voor het publiceren van uw toepassing naar Azure. Zie voor meer informatie over configuraties [Configureren uw Azure Project met behulp van meerdere configuraties](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Uw toepassing gebruik van services waarmee u de account opslag configureren

1. Open uw Azure-oplossing in Visual Studio. Open het snelmenu voor elke rol in uw Azure project dat de storage-services opent in de Solution Explorer en kies **Eigenschappen**. Een pagina met de naam van de rol wordt weergegeven in de editor van Visual Studio. De pagina bevat de velden voor het tabblad **configuratie** .

1. **Kies in de eigenschappenvensters voor de rol.**

1. Kies de naam van de configuratie van de service die u wilt bewerken in de lijst van de **Configuratie van de Service** . Als u wijzigingen aanbrengen op alle van de configuraties voor de rol wilt, kunt u **Alle configuraties**.  Zie de sectie **Verbindingsreeksen voor opslag Accounts beheren** in het onderwerp van [de rollen voor een Azure Cloud Service met Visual Studio configureren](vs-azure-tools-configure-roles-for-cloud-service.md)voor meer informatie over het bijwerken van configuraties.

1. De connection string om instellingen te wijzigen, kiest u de **...** naast de instelling. Het dialoogvenster **Verbindingsreeks opslag maken** wordt weergegeven.

1. Klik onder **verbinding maken via**de optie **uw abonnement** .

1. Kies uw abonnement in de lijst met **abonnementen** . Als de lijst met abonnementen niet die u wilt opnemen, kiest u de koppeling **Downloaden publicatie-instellingen** .

1. Kies in de lijst **naam** de naam van uw opslag. Azure extra verkrijgt opslag referenties automatisch met behulp van het bestand .publishsettings. De referenties van uw opslag als handmatig wilt opgeven, kiest u de optie **handmatig opgegeven referenties** en gaat u verder met deze procedure. Krijgt u uw accountnaam voor opslag en de primaire sleutel uit de [Azure klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=213885). Als u niet wilt dat uw opslag geeft accountinstellingen handmatig, klik op **OK** om het dialoogvenster te sluiten.

1. Kies de koppeling **Enter opslag account** referenties.

1. Voer in het vak **accountnaam** de naam van de account van uw opslag.

    >[AZURE.NOTE] Log in op de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)en kies vervolgens de knop **opslag** . De portal bevat een overzicht van de rekeningen van de opslag. Als u een account, wordt een pagina geopend. Vanaf deze pagina kunt u de naam van de account van de opslag. Als u een vorige versie van de klassieke portal gebruikt, verschijnt de naam van uw account voor opslag in de weergave van de **Rekeningen van de opslag** . Met deze naam te kopiëren, markeert u deze in het venster **Eigenschappen** van deze weergave en kies vervolgens de toetsen Ctrl-C. Plak de naam in Visual Studio, kiest u in het tekstvak **naam** en kiest u de toetsen Ctrl + V.

1. In het vak **sleutel rekening** voert u de primaire sleutel of kopiëren en plakken uit de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).
    Kopiëren van deze sleutel:

    1. Klik onderaan de pagina voor de account de juiste opslag **Sleutels beheren** .

    1. Op de pagina **Toegang tot sleutels beheren** , selecteert u de tekst van de van primaire toegangssleutel en kies vervolgens de toetsen Ctrl + C.

    1. In extra Azure, plakt u de sleutel in het vak **Account sleutel** .

    1. U moet een van de volgende opties om te bepalen hoe de service toegang tot de opslag account selecteren:
        - **Gebruik HTTP**. Dit is de standaard optie. Bijvoorbeeld `http://<account name>.blob.core.windows.net`.
        - **Gebruik HTTPS** voor een veilige verbinding. Bijvoorbeeld `https://<accountname>.blob.core.windows.net`.
        - **Aangepaste eindpunten opgeven** voor elk van de drie services. Vervolgens kunt u deze eindpunten invoeren in het veld voor de specifieke service.

        >[AZURE.NOTE] Als u aangepaste eindpunten maakt, kunt u een meer complexe verbindingsreeks. Wanneer u de indeling van deze tekenreeks gebruikt, kunt u opslag eindpunten die een aangepaste domeinnaam opgeeft die voor uw account opslag met de Blob-service zijn geregistreerd. Ook kunt u alleen toegang tot bronnen in dezelfde container via een gedeelde access-handtekening blob verlenen. Zie voor meer informatie over het maken van aangepaste eindpunten [Azure opslag verbindingsreeksen configureren](storage-configure-connection-string.md).

1. Klik op **OK** om wijzigingen op te slaan deze verbinding string, en kies vervolgens de knop **Opslaan** op de werkbalk. Nadat u deze wijzigingen hebt opgeslagen, kunt u de waarde van deze verbindingsreeks in uw code met behulp van [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Wanneer u de toepassing uitgeeft aan Azure, kiest u de configuratie van de service de account Azure opslag voor de verbindingsreeks bevat. Nadat uw aanvraag is gepubliceerd, Controleer of de toepassing werkt zoals verwacht tegen de Azure storage-services

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het publiceren van apps op Azure vanuit Visual Studio, Zie [een Cloud-service met behulp van de hulpprogramma's voor Azure](vs-azure-tools-publishing-a-cloud-service.md).
