<properties
    pageTitle="Beheer van bronnen Azure Blob-opslag met Opslagverkenner (voorbeeld) | Microsoft Azure"
    description="Beheer van Azure Blob Containers en BLOB's met Opslagverkenner (voorbeeld)"
    services="storage"
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
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Beheer van bronnen Azure Blob-opslag met Opslagverkenner (voorbeeld)

## <a name="overview"></a>Overzicht

[Azure Blob Storage](./storage/storage-dotnet-how-to-use-blobs.md) is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens, die kan worden geopend vanuit overal in de wereld via HTTP of HTTPS.
Kunt u Blob-opslag gegevens openbaar gemaakt in de wereld toegankelijk maken of voor het opslaan van toepassingsgegevens privé. In dit artikel leert u hoe u Opslagverkenner (voorbeeld) om te werken met blob containers en BLOB's.

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel hebt u het volgende nodig:

- [Download en installeer Opslagverkenner (voorbeeld)](http://www.storageexplorer.com)
- [Verbinding maken met een Azure opslag- of -service](./vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Maak een blob-container

Alle BLOB's moeten zich bevinden in een blob-container gewoon een logische groepering van BLOB's is. Een account kan een onbeperkt aantal containers bevatten en een onbeperkt aantal BLOB's in elke container kan worden opgeslagen.

De volgende stappen illustreren het maken van een container blob in Opslagverkenner (voorbeeld).

1.  Open Opslagverkenner (voorbeeld).
1.  Vouw in het linkerdeelvenster de opslag account waarin u wilt maken van de blob-container.
1.  Klik met de rechtermuisknop op **De Containers Blob**en - selecteren in het contextmenu - **Blob Container maken**.

    ![Blob containers contextmenu maken][0]

1.  Een tekstvak wordt weergegeven onder de map **Blob Containers** . Voer de naam voor de container van de blob. Zie de sectie [naamgevingsregels Container](./storage/storage-dotnet-how-to-use-blobs.md#create-a-container) voor een lijst van regels en beperkingen op de naamgeving van blob-containers.

    ![Blob Containers tekstvak maken][1]

1.  Druk op **Enter** wanneer u klaar bent voor het maken van de blob-container of op **Esc** om te annuleren. Zodra de blob-container heeft gemaakt, wordt deze weergegeven onder de map **Blob Containers** voor de opslag van geselecteerde account.

    ![BLOB-Container gemaakt][2]

## <a name="view-a-blob-containers-contents"></a>Een blob-container inhoud weergeven

Containers BLOB bevatten BLOB's en mappen (kunnen ook met BLOB's).

De volgende stappen illustreren het weergeven van de inhoud van een container blob in Opslagverkenner (voorbeeld):

1.  Open Opslagverkenner (voorbeeld).
1.  Vouw in het linkerdeelvenster de opslag account de blob-container die u wilt weergeven.
1.  Vouw de opslag-account **Blob Containers**.
1.  Klik met de rechtermuisknop op de blob-container die u wilt bekijken en - selecteer in het contextmenu - **Editor openen Blob-Container**.
U kunt ook dubbelklikken op de blob-container die u wilt weergeven.

    ![Open blob container editor contextmenu][19]

1.  Het hoofdvenster weergegeven inhoud van de container van de blob.

    ![BLOB-container-editor][3]

## <a name="delete-a-blob-container"></a>Een blob-container

BLOB containers kunnen gemakkelijk worden gemaakt en verwijderd. (Als u wilt zien hoe u een afzonderlijk BLOB's verwijderen, raadpleegt u de sectie [BLOB's beheren in een blob-container](./#managing-blobs-in-a-blob-container).)

De volgende stappen illustreren hoe een blob-container in Opslagverkenner (voorbeeld):

1.  Open Opslagverkenner (voorbeeld).
1.  Vouw in het linkerdeelvenster de opslag account de blob-container die u wilt weergeven.
1.  Vouw de opslag-account **Blob Containers**.
1.  Klik met de rechtermuisknop op de blob-container die u wilt verwijderen en - selecteer **verwijderen**in het contextmenu.
U kunt ook op drukken **verwijderen** verwijdert de geselecteerde blob-container.

    ![Blob container contextmenu verwijderen][4]

1.  Selecteer **Ja** in het bevestigingsvenster.

    ![Blob Container bevestiging verwijderen][5]

## <a name="copy-a-blob-container"></a>Kopieer een blob-container

Opslagverkenner (voorbeeld) kunt u een blob-container naar het Klembord kopiëren en plak die blob-container in een andere account voor opslag. (Als u wilt zien hoe de afzonderlijke BLOB's kopiëren, raadpleegt u de sectie [BLOB's beheren in een blob-container](./#managing-blobs-in-a-blob-container).)

De volgende stappen illustreren hoe een blob-container van een opslag account naar de andere kopiëren.

1.  Open Opslagverkenner (voorbeeld).
1.  Vouw in het linkerdeelvenster de opslag account de blob-container die u wilt kopiëren.
1.  Vouw de opslag-account **Blob Containers**.
1.  Klik met de rechtermuisknop op de blob-container die u wilt kopiëren en - in het contextmenu - **Kopie Blob Container**selecteren.

    ![Blob container contextmenu kopiëren][6]

1.  Klik met de rechtermuisknop op de gewenste 'target' opslag account waarin u wilt plakken van de blob-container en - selecteren in het contextmenu - **Blob Container plakken**.

    ![Contextmenu voor plakken blob-container][7]

## <a name="get-the-sas-for-a-blob-container"></a>De SA's voor een blob-container ophalen

Een [gedeelde access-handtekening (SAS)](./storage/storage-dotnet-shared-access-signature-part-1.md) biedt een gedelegeerde toegang tot bronnen in uw account voor opslag.
Dit betekent dat u een client beperkte machtigingen op objecten in uw account opslag gedurende een bepaalde tijd en met een opgegeven reeks machtigingen, zonder dat u voor het delen van uw account toegangstoetsen kunt verlenen.

De volgende stappen illustreren het maken van een SA's voor een blob-container:

1.  Open Opslagverkenner (voorbeeld).
1.  Vouw in het linkerdeelvenster de opslag rekening met de blob-container die u wilt ophalen van een SAS.
1.  Vouw de opslag-account **Blob Containers**.
1.  Klik met de rechtermuisknop op de gewenste blob-container en - selecteer in het contextmenu - **Handtekening met gedeelde toegang krijgen**.

    ![SAS contextmenu ophalen][8]

1.  In het dialoogvenster **Handtekening van gedeelde toegang tot** het beleid, en vervaldatum, tijdzone opgeven en toegangsniveaus die u wilt gebruiken voor de resource.

    ![SAS-opties][9]

1.  Wanneer u klaar bent met de SAS-opties, selecteer **maken**.

1.  Een tweede dialoogvenster voor **Gedeelde toegang handtekening** vervolgens weergegeven waarin de blob-container en de URL en de QueryStrings die u gebruiken kunt voor toegang tot de opslagbron.
Selecteer **kopie** naast de URL die u wilt kopiëren naar het Klembord.

    ![SAS-URL's kopiëren][10]

1.  Wanneer u klaar bent, selecteer **sluiten**.

## <a name="manage-access-policies-for-a-blob-container"></a>-Beleid beheren voor een blob-container

De volgende stappen illustreren het beheer (toevoegen en verwijderen)-beleid voor een blob-container:

1.  Open Opslagverkenner (voorbeeld).
1.  Vouw in het linkerdeelvenster de opslag rekening met de blob-container waarvan-beleid dat u wilt beheren.
1.  Vouw de opslag-account **Blob Containers**.
1.  Selecteer de gewenste blob-container en - selecteer in het contextmenu - **-Beleid beheren**.

    ![Contextmenu access-beleid beheren][11]

1.  Het dialoogvenster **-Beleid** wordt een lijst een-beleid hebt gemaakt voor de geselecteerde blob-container.

    ![Toegang tot opties voor beleid][12]        

1.  Ga als volgt te werk, afhankelijk van de taak access policy management:

    - **Een nieuwe-beleid toevoegen** - Selecteer **toevoegen**. Eenmaal is gegenereerd, wordt het dialoogvenster **-Beleid** het toegevoegde-beleid (met standaardinstellingen) weergegeven.
    - Breng eventueel wijzigingen aan **een-beleid bewerken** - en selecteer **Opslaan**.
    - **Een-beleid verwijderen** - Selecteer **verwijderen** naast het-beleid dat u wilt verwijderen.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Het niveau van de toegang van het publiek voor een blob-container instellen

Elke container blob is standaard ingesteld op "Geen toegang voor publiek".

De volgende stappen illustreren hoe een niveau van toegang van het publiek voor een blob-container opgeven.

1.  Open Opslagverkenner (voorbeeld).
1.  Vouw in het linkerdeelvenster de opslag rekening met de blob-container waarvan-beleid dat u wilt beheren.
1.  Vouw de opslag-account **Blob Containers**.
1.  Selecteer de gewenste blob-container en - selecteren in het contextmenu - **Niveau van toegang van het publiek stellen**.

    ![Toegang van het publiek niveau contextmenu instellen][13]

1.  Geef het gewenste toegangsniveau in het dialoogvenster **Container publieke toegangsniveau ingesteld** .

    ![Stel opties voor het niveau van toegang van het publiek][14]

1.  Selecteer **toepassen**.

## <a name="managing-blobs-in-a-blob-container"></a>BLOB's in een blob-container beheren

Als u een container blob hebt gemaakt, kunt u een blob uploaden naar die blob-container, een blob downloaden naar uw lokale computer, een blob opent op uw lokale computer en nog veel meer.

De volgende stappen illustreren het beheren van de BLOB's (en -mappen) in een blob-container.

1.  Open Opslagverkenner (voorbeeld).
1.  Vouw in het linkerdeelvenster de opslag account de blob-container die u wilt beheren.
1.  Vouw de opslag-account **Blob Containers**.
1.  Dubbelklik op de blob-container die u wilt weergeven.
1.  Het hoofdvenster weergegeven inhoud van de container van de blob.

    ![Blob-container weergeven][3]

1.  Het hoofdvenster weergegeven inhoud van de container van de blob.

1.  Ga als volgt te werk, afhankelijk van de taak die u wilt uitvoeren:

    - **Bestanden uploaden naar een blob-container**

        1.  Selecteer op de werkbalk van het hoofdvenster **uploaden**en vervolgens **Uploaden van bestanden** uit de vervolgkeuzelijst.

            ![Menu bestanden uploaden][15]

        1.  Selecteer in het dialoogvenster **bestanden uploaden** , de knop met het weglatingsteken (**...**) aan de rechterkant van het tekstvak van de **bestanden** te selecteren van de bestanden die u wilt uploaden.

            ![Opties voor bestanden uploaden][16]

        1.  Geef het type van het **type Blob**. [Aan de slag met Azure Blob-opslag met behulp van .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) beschreven de verschillen tussen de verschillende typen van de blob.

        1.  Voer desgewenst een doelmap op waarin de geselecteerde bestanden worden geüpload. Als het pad van de map niet bestaat, wordt deze gemaakt.

        1.  Selecteer **uploaden**.

    - **Een map te uploaden naar een blob-container**

        1.  Selecteer op de werkbalk van het hoofdvenster, **uploaden**en vervolgens op **Upload map** uit de vervolgkeuzelijst.

            ![Menu map te uploaden][17]

        1.  Selecteer de knop met het weglatingsteken (**...**) aan de rechterkant van het tekstvak **map** selecteren de map die u wilt uploaden waarvan de inhoud in het dialoogvenster **map te uploaden** .

            ![Opties voor uploaden][18]

        1.  Geef het type van het **type Blob**. [Aan de slag met Azure Blob-opslag met behulp van .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) beschreven de verschillen tussen de verschillende typen van de blob.

        1.  Voer desgewenst een doelmap op waarin de inhoud van de geselecteerde map worden geüpload. Als het pad van de map niet bestaat, wordt deze gemaakt.

        1.  Selecteer **uploaden**.

    - **Een blob downloaden naar uw lokale computer**

        1.  Selecteer de label die u wilt downloaden.

        1.  Selecteer op de werkbalk van het hoofdvenster **downloaden**.

        1.  In het dialoogvenster **opgeven waar u de gedownloade blob opslaan** , geeft u de locatie waar u de blob gedownload en de naam wilt geven.  

        1.  Selecteer **Opslaan**.

    - **Een blob op uw lokale computer openen**

        1.  Selecteer de label die u wilt openen.

        1.  Selecteer **openen**op de werkbalk van het hoofdvenster.

        1.  De blob wordt gedownload en geopend met de toepassing van de blob onderliggende bestandstype is verbonden.

    - **Een blob naar het Klembord kopiëren**

        1.  Selecteer de label die u wilt kopiëren.

        1.  Selecteer **kopiëren**op de werkbalk van het hoofdvenster.

        1.  Ga naar een andere blob-container in het linkerdeelvenster en dubbelklik erop om het te bekijken in het hoofdvenster.

        1.  Selecteer op de werkbalk van het hoofdvenster **Plakken** een kopie maken van de blob.

    - **Een blob verwijderen**

        1.  Selecteer de label die u wilt verwijderen.

        1.  Op de werkbalk van het hoofdvenster, selecteert u **verwijderen**.

        1.  Selecteer **Ja** in het bevestigingsvenster.

## <a name="next-steps"></a>Volgende stappen

- Bekijk de [nieuwste Opslagverkenner (voorbeeld) release-info en video's](http://www.storageexplorer.com).
- Meer informatie over het [maken van toepassingen met Azure BLOB's, tabellen, wachtrijen, en bestanden](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png