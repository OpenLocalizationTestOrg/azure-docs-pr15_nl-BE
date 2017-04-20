<properties
   pageTitle="Bladeren en het beheer van opslagbronnen in Server Explorer | Microsoft Azure"
   description="Bladeren en het beheer van opslagbronnen in Server Explorer"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Bladeren en het beheer van opslagbronnen in Server Explorer

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Overzicht
Als u hebt de Azure-hulpprogramma's voor Microsoft Visual Studio hebt geïnstalleerd, kunt u blob, wachtrij en gegevens in een tabel weergeven van uw opslag rekeningen voor Azure. Het knooppunt Azure opslag in Server Explorer bevat gegevens die in uw lokale opslag emulator-account en uw andere rekeningen van Azure opslag.

Om Server Explorer weergeven in Visual Studio op de menubalk, kiest u **Beeld**, **Server Explorer**. Het Opslagknooppunt bevat alle opslag rekeningen die bestaan onder elke Azure abonnement en certificaat waarmee u bent verbonden. Als uw account opslag niet wordt weergegeven, kunt u deze toevoegen door de aanwijzingen [verderop in dit onderwerp](#add-storage-accounts-by-using-server-explorer).

Starten in Azure SDK 2.7, kun je het nieuwe Cloud Explorer bekijken en beheren van uw resources Azure. Zie [Azure bronnen beheren met Cloud Explorer](./vs-azure-tools-resources-managing-with-cloud-explorer.md) voor meer informatie.


## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Weergeven en beheren van opslagbronnen in Visual Studio

Server Explorer geeft een overzicht van de BLOB's, wachtrijen en tabellen automatisch in uw account opslag emulator. De emulator opslag account wordt vermeld in Server Explorer onder het knooppunt opslag als het knooppunt **ontwikkeling** .

Vouw het knooppunt voor de **ontwikkeling** van de opslag-emulator account als resources wilt zien. Als u de emulator opslag nog niet is begonnen als u het knooppunt **ontwikkeling** uitvouwt, wordt deze automatisch gestart. Dit kan enkele seconden duren. U kunt verder werken in andere gebieden van Visual Studio tijdens het starten van de opslag-emulator.

Voor resources weergeven in een opslag-account, vouw de opslag van de account in Server Explorer. De volgende onderliggende knooppunten weergegeven:

- BLOB 's

- Wachtrijen

- Tabellen

## <a name="work-with-blob-resources"></a>Werken met Resources Blob

Het knooppunt BLOB's bevat een lijst van containers voor de opslag van geselecteerde account. Containers BLOB blob-bestanden bevatten en u kunt deze BLOB's indelen in mappen en submappen. Zie [hoe u Blob-opslag van .NET](./storage/storage-dotnet-how-to-use-blobs.md) voor meer informatie.

### <a name="to-create-a-blob-container"></a>Voor het maken van een blob-container

1. Open het snelmenu voor het knooppunt **BLOB's** en kies vervolgens **Blob Container maken**.

1. Voer de naam van de nieuwe container in het dialoogvenster **Blob Container maken** en kies vervolgens **Ok**

    >[AZURE.NOTE] De naam van de container blob moet beginnen met een cijfer (0-9) of kleine letter (a-z).

### <a name="to-delete-a-blob-container"></a>Een blob-container

- Open het snelmenu voor de blob-container die u wilt verwijderen en klik vervolgens op **verwijderen**.

### <a name="to-display-a-list-of-the-items-contained-in-a-blob-container"></a>Voor het weergeven van de items in een blob-container

- Open het snelmenu voor de naam van een blob-container in de lijst en kies vervolgens **Beeld Blob-Container**.

    Wanneer u de inhoud van een container blob weergeeft, wordt deze weergegeven in een tabblad bekend als de blob container weergeven.

    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

    U kunt de volgende bewerkingen uit op de BLOB's uitvoeren met de knoppen in de rechterbovenhoek van de weergave van blob-container:

    - Een filterwaarde invoeren en toepassen

    - Vernieuw de lijst met BLOB's in de container

    - Upload een bestand

    - Een blob verwijderen

      >[AZURE.NOTE] Verwijderen van een bestand in een blob-container verwijdert niet het onderliggende bestand; deze alleen verwijderd uit de blob-container.

    - Open een blob

    - Een blob opslaan op de lokale computer

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Een map of submap maken in een blob-container

1. Kies de blob-container in Server Explorer. Kies de knop **Uploaden Blob** in het venster van de container.

    ![Uploaden van een bestand in een blob-map](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

1. Klik op de knop **Bladeren** om op te geven van het bestand dat u wilt uploaden en voer vervolgens de naam van een map in het vak **map (optioneel)** in het dialoogvenster **Nieuw bestand uploaden** .

    U kunt submappen in container mappen toevoegen door dezelfde procedure te volgen. Als u geen naam van een map opgeeft, wordt het bestand worden geüpload naar het hoogste niveau van de blob-container. Het bestand wordt weergegeven in de opgegeven map in de container.

    ![Map toevoegen aan een blob-container](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Dubbelklik op de map of druk op ENTER om de inhoud van de map. Wanneer u in de map van de container, kunt u navigeren naar de hoofdmap van de container met de knop **Openen op de bovenliggende map** (pijl-omhoog).

### <a name="to-delete-a-container-folder"></a>Een container map verwijderen

 - Verwijder alle bestanden in de map

    >[AZURE.NOTE] Omdat in de blob-containers virtuele mappen zijn, u kunt een lege map maken, noch kunt u een map voor het verwijderen van de bestandsinhoud verwijderen. U moet verwijderen van de gehele inhoud van een map in de map te verwijderen.

### <a name="to-filter-blobs-in-a-container"></a>Voor het filteren van BLOB's in een container

U kunt filteren de BLOB's die worden weergegeven door een gemeenschappelijke prefix op te geven.

Als u het voorvoegsel opgeven bijvoorbeeld `hello` in de tekst van het filter in het vak en kies vervolgens **uitvoeren** (**!**) knop, alleen de BLOB's die met 'Hallo beginnen' weergegeven.

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)


>[AZURE.NOTE] Het veld is hoofdlettergevoelig en filteren met jokertekens niet ondersteunt. BLOB's kunnen alleen worden gefilterd door het voorvoegsel. Het voorvoegsel kan een scheidingsteken bevatten als u een scheidingsteken gebruikt om BLOB's in een virtuele hiërarchie te ordenen. Bijvoorbeeld filteren op het prefix HelloFabric / alle BLOB's beginnen met deze tekenreeks als resultaat gegeven.

### <a name="to-download-blob-data"></a>Blob-gegevens downloaden

- In **Server Explorer**opent het snelmenu voor een of meer BLOB's en vervolgens kiest u **openen**, kiest u de naam van de blob en vervolgens kiest u de knop **openen** of dubbelklik op de naam van de blob.

    De voortgang van het downloaden van een blob wordt weergegeven in het venster **Logboek voor faxactiviteit Azure** .

    De blob wordt geopend in de standaardeditor voor dat bestandstype. Als het besturingssysteem het bestandstype herkent, wordt het bestand geopend in een lokaal geïnstalleerde toepassing. anders wordt u gevraagd een toepassing die geschikt is voor het bestandstype van de blob. Het lokale bestand dat wordt gemaakt bij het downloaden van een blob is gemarkeerd als alleen-lezen.

    BLOB-gegevens is lokaal in de cache en worden gecontroleerd aan de hand van de blob laatst gewijzigd in de Blob-service. Als de blob is bijgewerkt sinds u het laatst is gedownload, wordt het gedownload opnieuw; anders is worden de label van de lokale schijf geladen. Standaard wordt een blob gedownload naar een tijdelijke map. BLOB's downloaden naar een bepaalde map, open het snelmenu voor de geselecteerde blob namen en kies **OpslaanAls**. Wanneer u een blob op deze manier opslaat, de blob-bestand wordt niet geopend en het lokale bestand wordt gemaakt met de kenmerken alleen-lezen.

### <a name="to-upload-blobs"></a>BLOB's uploaden

- Klik op de knop **Uploaden Blob** wanneer de container geopend voor weergave in de weergave van blob-container is.

    U kunt een of meer bestanden wilt uploaden en kunt u bestanden van elk type uploaden. Het **Logboek voor faxactiviteit Azure** wordt de voortgang van het uploaden. Zie voor meer informatie over het werken met blob-gegevens, [het gebruik van de Service Azure Blob Storage in .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>Overgebracht naar BLOB's Logboeken weergeven

- Als u met behulp van Azure diagnostische gegevens vastleggen vanuit uw toepassing Azure en u zich aanmeldt op uw account voor de opslag hebt overgebracht, ziet u de containers die zijn gemaakt door Azure voor deze logboeken. Deze logboeken weergeven in Server Explorer is een eenvoudige manier voor het identificeren van problemen met uw toepassing, vooral als het wordt naar Azure is geïmplementeerd. Zie voor meer informatie over Azure diagnostische gegevens [Vastleggen gegevens verzamelen door met behulp van Azure diagnostische gegevens](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>Ophalen van de URL van een blob

- Open het snelmenu van de blob en kies vervolgens **De URL kopiëren**.

### <a name="to-edit-a-blob"></a>Voor het bewerken van een blob

- Selecteer de blob en kies vervolgens de knop **Open Blob** .

    Het bestand is gedownload naar een tijdelijke locatie en op de lokale computer geopend. Nadat u wijzigingen hebt aangebracht, moet u de blob opnieuw uploaden.

## <a name="work-with-queue-resources"></a>Werken met Resources wachtrij

Storage services wachtrijen worden gehost op een rekening Azure opslag en u kunt ze gebruiken om uw cloud service rollen om te communiceren met elkaar en met andere diensten door een bericht geven mechanisme. U kunt via programmacode de wachtrij openen via een cloud service en via een webservice voor externe clients. U kunt ook toegang tot de wachtrij rechtstreeks via Server Explorer in Visual Studio.

Bij het ontwikkelen van een cloud-service die gebruikmaakt van wachtrijen kunt u Visual Studio gebruiken voor het maken van wachtrijen en interactief kunnen werken met hen tijdens het ontwikkelen en testen van uw code.

In Server Explorer, kunt u de wachtrijen in een opslag account bekijken, maken en wachtrijen verwijderen wachtrij om de berichten weer te openen en berichten naar een wachtrij toevoegen. Wanneer u een wachtrij voor weergave opent, kunt u de afzonderlijke berichten weergeven en u kunt de volgende acties uitvoeren in de wachtrij met behulp van de knoppen in de linkerbovenhoek:

- Vernieuw de weergave van de wachtrij

- Een bericht aan de wachtrij toevoegen

- Het bovenste bericht uit de wachtrij halen.

- De hele wachtrij wissen

De volgende afbeelding ziet u een rij met twee berichten.

![Een wachtrij weergeven](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Zie voor meer informatie over opslag services wachtrijen, [hoe: Gebruik de Queue Storage-Service](http://go.microsoft.com/fwlink/?LinkID=264702). Zie voor informatie over de webservice voor opslag services wachtrijen, [Wachtrij-Service-begrippen](http://go.microsoft.com/fwlink/?LinkId=264788). Zie [Berichten verzenden naar een wachtrij Storage-Services](https://msdn.microsoft.com/library/azure/jj649344.aspx)voor meer informatie over het verzenden van berichten naar een wachtrij storage-services met behulp van Visual Studio.

>[AZURE.NOTE] Opslag services wachtrijen zijn wachtrijen bus. Zie voor meer informatie over service bus wachtrijen, wachtrijen Bus, onderwerpen en abonnementen.

## <a name="work-with-table-resources"></a>Werken met de tabel Resources

De tabel Azure storage-service bevat grote hoeveelheden van gestructureerde gegevens. De service is een gegevensarchief NoSQL die accepteert geverifieerde oproepen van binnen en buiten de Azure cloud. Azure tabellen zijn ideaal voor het opslaan van gestructureerde en niet-relationele gegevens.

### <a name="to-create-a-table"></a>Een tabel maken

1. Selecteer het knooppunt **tabellen** van de rekening voor opslag in Server Explorer en kiest vervolgens **Tabel maken**.

1. Voer in het dialoogvenster **Tabel maken** een naam voor de tabel.

### <a name="to-view-table-data"></a>Gegevens in een tabel weergeven

1. Open het knooppunt **Azure** in Server Explorer en open vervolgens het knooppunt **opslag** .

1. Open het knooppunt opslag account waarin u bent geïnteresseerd in en open vervolgens het knooppunt van de **tabellen** voor een overzicht van de tabellen voor de opslag.

1. Open het snelmenu voor een tabel en kies **Tabel bekijken**.

    ![Een Azure-tabel in de Solution Explorer](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

De tabel wordt geordend op entiteiten (zoals weergegeven in rijen) en eigenschappen (zoals weergegeven in de kolommen). In de volgende afbeelding ziet u bijvoorbeeld entiteiten die in de **Ontwerpfunctie voor tabellen**worden vermeld:

### <a name="to-edit-table-data"></a>Voor het bewerken van gegevens in een tabel

1. Open het snelmenu voor een entiteit (één rij) of een eigenschap (een enkele cel) in de **Ontwerpfunctie voor tabellen**en kies vervolgens **bewerken**.

    ![Toevoegen of bewerken van een tabel-entiteit](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

    Entiteiten in een enkele tabel niet nodig hebben dezelfde set eigenschappen (kolommen) zijn. Houd rekening met de volgende beperkingen voor het weergeven en bewerken van gegevens in een tabel.
    - U niet weergeven of bewerken van binaire gegevens (type byte[]), maar u kunt opslaan in een tabel.

    - U kunt de waarden van **PartitionKey** of **RowKey** , niet bewerken omdat deze bewerking geen tabelopslag in Azure ondersteunt.

    - U kunt een eigenschap met de naam tijdstempel niet maken, opslag Azure services maken gebruik van een eigenschap met die naam.

    - Als u een datum / tijdwaarde invoert, moet u een indeling die geschikt is voor de landinstellingen van uw computer (bijvoorbeeld DD-MM-jjjj: mm: ss [AM | PM] voor Amerikaans Engels).

### <a name="to-add-entities"></a>Entiteiten toevoegen

1. Kies de knop **Toevoegen entiteit** , die zich nabij de rechterbovenhoek van de tabelweergave in de **Ontwerpfunctie voor tabellen**.

    ![Entiteit toevoegen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. Voer in het dialoogvenster **Toevoegen entiteit** de waarden van de eigenschappen **PartitionKey** en **RowKey** .

    ![Het dialoogvenster entiteit toevoegen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Voer de waarden zorgvuldig, omdat u deze niet meer wijzigen nadat u het dialoogvenster sluit, tenzij u de entiteit te verwijderen en opnieuw toevoegen.

### <a name="to-filter-entities"></a>Filteren van entiteiten

U kunt de set van entiteiten die worden weergegeven in een tabel als u de opbouwfunctie voor query's.

1. Om de opbouwfunctie voor query's openen, opent u een tabel weergeven.

1. Kies de knop uiterst rechts op de werkbalk van de tabelweergave.

    Het dialoogvenster **Opbouwfunctie voor Query's** wordt weergegeven. De volgende afbeelding ziet een query die wordt opgebouwd in de opbouwfunctie voor query's.

    ![Opbouwfunctie voor query 's](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)

1. Wanneer u klaar bent voor het bouwen van de query, sluit u het dialoogvenster. De resulterende tekstvorm van de query in een tekstvak wordt weergegeven als een filter gegevens van WCF-Services.

1. De query wilt uitvoeren, kiest u het groene driehoekje.

    U kunt ook de entiteitsgegevens die wordt weergegeven in de **Ontwerpfunctie voor tabellen** als u een filtertekenreeks WCF-gegevensservices rechtstreeks in het filterveld filteren. Dit soort tekenreeks lijkt op een SQL WHERE-component, maar als een HTTP-aanvraag bij de server wordt verzonden. Zie voor meer informatie over het maken van tekenreeksen [Samenstellen van tekenreeksen voor de ontwerpfunctie voor tabellen](https://msdn.microsoft.com/library/azure/ff683669.aspx).

    In de volgende afbeelding toont een voorbeeld van een tekenreeks geldig filter:

    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Opslaggegevens vernieuwen

Wanneer Server Explorer maakt of gegevens van een account voor opslag ontvangt, kan het duren tot een minuut voor de bewerking is voltooid. Als er geen verbinding kunt maken, kan de bewerking-time-out. Terwijl de gegevens worden opgehaald, kunt u verder werken in andere delen van Visual Studio. Om de bewerking te annuleren als het te lang duurt, klik op de knop **Vernieuwen stoppen** op de Server Explorer-werkbalk.

#### <a name="to-refresh-blob-container-data"></a>Container-blob-gegevens vernieuwen

- Selecteer het knooppunt **BLOB's** onder **opslag** en klik op de knop **vernieuwen** op de werkbalk van de Server Explorer.

- U vernieuwt de lijst met blobs die wordt weergegeven, klik op de knop **uitvoeren** .

#### <a name="to-refresh-table-data"></a>Voor het vernieuwen van gegevens in een tabel

- Selecteer het knooppunt **tabellen** onder **opslag** en klik op de knop **vernieuwen** .

- U vernieuwt de lijst met entiteiten die wordt weergegeven in de **Ontwerpfunctie voor tabellen**, klik op **uitvoeren** in de **Ontwerpfunctie voor tabellen**.

#### <a name="to-refresh-queue-data"></a>Wachtrijgegevens vernieuwen

- Selecteer het knooppunt voor **wachtrijen** en kies vervolgens de knop **vernieuwen** .

#### <a name="to-refresh-all-items-in-a-storage-account"></a>Voor het vernieuwen van alle items in een opslag account

- Kies de accountnaam en kies vervolgens de knop **vernieuwen** op de werkbalk voor Server Explorer.

### <a name="add-storage-accounts-by-using-server-explorer"></a>Opslag-accounts toevoegen met behulp van Server Explorer

Er zijn twee manieren opslag accounts toevoegen met behulp van Server Explorer. Kunt u een nieuwe account voor opslag van uw abonnement op Azure, of u kunt een bestaande opslag account koppelen.

#### <a name="to-create-a-new-storage-account-by-using-server-explorer"></a>Een nieuwe opslag-account maken met Server Explorer

1. In Server Explorer, open het snelmenu voor het knooppunt opslag, en kies vervolgens opslag Account maken.

    ![Maak een nieuwe account met Azure opslag](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. Selecteer of Voer de volgende gegevens voor de nieuwe account voor de opslag van het dialoogvenster **Opslag Account maken** .

    - De Azure abonnement dat u wilt de opslag-account toevoegen.

    - De naam die u wilt gebruiken voor de nieuwe account voor opslag.

    - De regio of affiniteit groep (bijvoorbeeld West-Amerikaanse of Oost-Azië).

    - Het type replicatie die u wilt gebruiken voor de opslag-account, zoals Geo-redundante.

1. Kies **maken**.

    De nieuwe opslag-account wordt weergegeven in de lijst van de **opslag** in de Solution Explorer.

#### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Een bestaande opslag account koppelen met behulp van Server Explorer

1. Open het snelmenu voor het Opslagknooppunt Azure in Server Explorer en kies **Koppelen van externe opslag**.

    ![Een bestaande opslag account toevoegen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)

1. Selecteer of Voer de volgende gegevens voor de nieuwe account voor de opslag van het dialoogvenster **Opslag Account maken** .

    - De naam van de bestaande opslag-account die u wilt koppelen. U kunt een naam invoeren of selecteren in de lijst.

    - De sleutel voor de opslag van geselecteerde account. Deze waarde is meestal opgegeven als u een account voor opslag. Als u Visual Studio te onthouden de sleutel opslag account, selecteer account sleutel het onthouden.

    - Het protocol bij het verbinding maken met de account van de opslag, zoals HTTP, HTTPS of een aangepaste eindpunt. Zie [verbindingsreeksen configureren](https://msdn.microsoft.com/library/azure/ee758697.aspx) voor meer informatie over aangepaste eindpunten.

### <a name="to-view-the-secondary-endpoints"></a>De secundaire eindpunten weergeven

- Als u een opslag-account met de optie met **Leestoegang Geo overbodige** replicatie hebt gemaakt, kunt u de secundaire eindpunten bekijken. Open het snelmenu voor de naam van de account en kiest u **Eigenschappen**.

    ![Secundaire eindpunten opslag](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Een account voor de opslag van Server Explorer verwijderen

- In Server Explorer, opent u het snelmenu voor de naam van de account en kiest u **verwijderen**. Als u een opslag-account verwijdert, worden alle opgeslagen belangrijke informatie voor de account wordt ook verwijderd.

    >[AZURE.NOTE] Als u een account voor de opslag van Server Explorer verwijdert, dat geen gevolgen voor uw account opslag of gegevens die het bevat; gewoon verwijdert de verwijzing vanuit Server Explorer. Als u wilt een opslag account permanent verwijdert, de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Zie [toegang tot de opslag Azure Services](https://msdn.microsoft.com/library/azure/ee405490.aspx)voor meer informatie dat over Azure storage-services gebruiken.
