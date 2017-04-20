<properties
    pageTitle="Het gebruik van Hudson met Blob-opslag | Microsoft Azure"
    description="Beschrijving van het Hudson met Azure Blob-opslag gebruiken als opslagplaats voor build artefacten."
    services="storage"
    documentationCenter="java"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016" 
    ms.author="dinesh"/>

# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Gebruik van Azure opslag met een continue integratie Hudson-oplossing

## <a name="overview"></a>Overzicht

Hieronder ziet u hoe u Blob-opslag als opslagplaats van build artefacten gemaakt door een oplossing van Hudson Continuous Integration (CI) of als een bron van downloadbare bestanden in een proces wordt gebruikt. Een van de scenario's waarin u dit nuttig vinden zouden is wanneer u in een flexibele ontwikkelomgeving coderen wilt (met Java of andere talen) en moet u een opslagplaats voor de build-artefacten, zodat u, bijvoorbeeld, met andere leden van de organisatie, uw klanten delen, of een archief onderhouden builds worden uitgevoerd op basis van continue integratie.  Een ander scenario is wanneer uw project build zelf andere bestanden, zoals afhankelijkheden worden gedownload als onderdeel van de build van invoer vereist.

In deze zelfstudie wordt u de invoegtoepassing Azure opslag voor Hudson CI beschikbaar gesteld door Microsoft.

## <a name="introduction-to-hudson"></a>Inleiding tot de Hudson ##

Hudson continue integratie van een software-project kan door ontwikkelaars hun codewijzigingen eenvoudig integreren en builds geproduceerde automatisch en regelmatig, en verhoogt de productiviteit van de ontwikkelaars. Builds zijn versioned en build artefacten kunnen worden geüpload naar de verschillende opslaglocaties. In dit artikel wordt beschreven hoe Azure Blob opslag gebruiken als opslagplaats van de build-artefacten. Het wordt ook het downloaden van de afhankelijkheden van Azure Blob-opslag weergegeven.

Meer informatie over Hudson vindt u bij het [Voldoen aan de Hudson](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson).

## <a name="benefits-of-using-the-blob-service"></a>Voordelen van het gebruik van de Blob-service ##

Voordelen van de Blob-service gebruikt voor het hosten van uw flexibele ontwikkeling build artefacten zijn:

- Hoge beschikbaarheid van uw build artefacten en/of downloadbare afhankelijkheden.
- Prestaties wanneer uw oplossing CI Hudson uw build artefacten uploadt.
- Prestaties bij uw klanten en partners de build artefacten downloaden.
- Controle over gebruiker-beleid met een keuze tussen anonieme toegang, toegang tot gedeelde toegang op basis van vervaldatum-handtekening, persoonlijke toegang, enz.

## <a name="prerequisites"></a>Vereisten ##

U moet de volgende de Blob-service wilt gebruiken met uw Hudson CI-oplossing:

- Een continue integratie Hudson-oplossing.

    Als u momenteel geen oplossing voor een Hudson CI, kunt u een Hudson CI-oplossing met de volgende techniek uitvoeren:

    1. Download de OORLOG Hudson uit <http://hudson-ci.org/>op een machine Java ingeschakeld.
    2. Bij een MS-DOS-prompt wordt geopend met de map met de Hudson-WAR de OORLOG Hudson worden uitgevoerd. Bijvoorbeeld als u versie 3.1.2 hebt gedownload:

        `java -jar hudson-3.1.2.war`

    3. Open in uw browser `http://localhost:8080/`. Hiermee opent u het dashboard Hudson.

    4. Uitvoeren bij de installatie bij eerste gebruik van Hudson, `http://localhost:8080/`.

    5. Nadat u de installatie voltooien, annuleert het exemplaar van de Hudson-WAR opnieuw de OORLOG Hudson Start en opnieuw openen van het dashboard Hudson `http://localhost:8080/`, die u kunt installeren en configureren van de invoegtoepassing voor opslag van Azure.

        Terwijl een standaardoplossing Hudson CI zou worden ingesteld als een service, de Hudson-war uitgevoerd op de opdrachtregel worden uitgevoerd, zal volstaan voor deze zelfstudie.

- Een Azure-account. U kunt zich aanmelden voor een Azure-account op <http://www.azure.com>.

- Een account Azure opslag. Als u niet al een opslag-account hebt, kunt u één van de stappen in [een opslag-Account maken](storage-create-storage-account.md#create-a-storage-account).

- Vertrouwdheid met de Hudson-CI-oplossing wordt aanbevolen, maar niet is vereist, zoals de volgende inhoud wordt met een eenvoudig voorbeeld tonen de stappen die nodig zijn bij het gebruik van de Blob-service als opslagplaats voor Hudson CI artefacten bouwen.

## <a name="how-to-use-the-blob-service-with-hudson-ci"></a>Het gebruik van de service Blob met CI Hudson ##

De Blob om service te gebruiken met de Hudson, moet u de invoegtoepassing Azure opslag installeren, configureren van de invoegtoepassing voor het gebruik van uw account voor de opslag en maak vervolgens een actie na het samenstellen waarmee uw build artefacten aan uw account voor de opslag. Deze stappen worden in de volgende secties beschreven.

## <a name="how-to-install-the-azure-storage-plugin"></a>Het installeren van de invoegtoepassing Azure opslag ##

1. Klik in het dashboard Hudson **Hudson beheren**.
2. Klik op de pagina **Hudson beheren** **Invoegtoepassingen beheren**.
3. Klik op het tabblad **beschikbaar** .
4. Klik op **anderen**.
5. Selecteer in de sectie **Artefact Uploaders** **Microsoft Azure Storage plugin**.
6. Klik op **installeren**.
7. Nadat de installatie voltooid is, start u de Hudson.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>De invoegtoepassing Azure opslag gebruik van uw account opslag configureren ##

1. Klik in het dashboard Hudson **Hudson beheren**.
2. Klik op de pagina **Beheren Hudson** **Systeem configureren**.
3. In de sectie **Configuratie van Microsoft Azure opslag Account** :

    een. Voer uw accountnaam opslag, kunt u vanuit de [Portal Azure](https://portal.azure.com).

    b. Voer uw sleutel opslag account, ook kan worden verkregen uit de [Azure-Portal](https://portal.azure.com).

    c. Gebruik de standaardwaarde voor **Blob Service-eindpunt-URL** als u de openbare Azure cloud. Als u een andere wolk Azure, het eindpunt zoals opgegeven in de [Azure Portal](https://portal.azure.com) voor uw opslag account gebruiken.

    d. Klik op **valideren opslag referenties** voor het valideren van je account voor opslag.

    e. [Optioneel] Als u extra opslagruimte gewenste accounts beschikbaar gesteld aan de Hudson-CI, klikt u op **meer opslag accounts toevoegen**.

    f. Klik op **Opslaan** om uw instellingen te slaan.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Het maken van een actie na het samenstellen waarmee uw build artefacten aan uw account voor opslag ##

Voor instructie doeleinden, eerst moet wij een taak maken diverse bestanden en voegt u de actie na het samenstellen de bestanden te uploaden naar uw account opslag maken.

1. Klik op **Nieuwe taak**in het dashboard Hudson.
2. **De **MyJob**van de taak een naam en klikt u op **een taak vrije stijl software bouwen**.**
3. In de sectie van de configuratie van het **bouwen** op **toevoegen stap maken** en kies **batch-opdracht uitvoeren van Windows**.
4. In **opdracht**, gebruikt u de volgende opdrachten:

        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt

5. Klik op **uploaden artefacten naar Microsoft Azure Blob-opslag**in de sectie **Acties na het samenstellen** van de taakconfiguratie.
6. Selecteer de opslag te gebruiken voor **Opslag-accountnaam**.
7. Opgeven voor de **Naam van de Container**, de containernaam van de. (De container wordt gemaakt als deze niet bestaat nog wanneer de build artefacten zijn geüpload.) U kunt omgevingsvariabelen gebruiken, dus in dit voorbeeld **${JOB_NAME}** geeft als de containernaam van de.

    **Tip**

    Sectie waar u een script hebt ingevoerd voor het **uitvoeren van Windows de opdracht batch** is onder de **opdracht** een koppeling naar de omgevingsvariabelen worden herkend door Hudson. Klik op deze koppeling voor meer informatie over de namen van omgevingsvariabelen en beschrijvingen. Opmerking de omgevingsvariabelen die speciale tekens, zoals de omgevingsvariabele **BUILD_URL bevatten** zijn niet toegestaan als een container- of algemene virtuele pad.

8. Klik op **nieuwe container standaard openbaar maken** voor dit voorbeeld. (Als u een privé-container gebruikt wilt, moet u voor het maken van een gedeelde access-handtekening om toegang te verlenen. Die valt buiten het bestek van dit artikel. U kunt meer informatie over gedeelde toegang handtekeningen op [Met behulp van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Optioneel] Klik op de **schone container voor uploaden** als u wilt dat de container van de inhoud worden gewist voordat build artefacten worden geüpload (laat het uitgeschakeld als u niet wilt dat de inhoud van de container schoon).
10. **Lijst van onderdelen om te uploaden**, voert u * *tekst /*.txt**.
11. Voer voor **algemene virtuele pad voor de geüploade onderdelen**, **${bouwen\_ID} / ${bouwen\_nummer}**.
12. Klik op **Opslaan** om uw instellingen te slaan.
13. Klik in het dashboard Hudson **Bouwen nu** **MyJob**uitvoeren. Bekijk de console-uitvoer voor de status. Statusberichten voor Azure opslag wordt opgenomen in de console-uitvoer wanneer de actie na het samenstellen wordt gestart voor het uploaden van build artefacten.
14. Nadat de taak is voltooid, kunt u de build artefacten kunt bekijken via de openbare blob.

    een. Aanmelden bij de [Azure Portal](https://portal.azure.com).

    b. Klik op **opslag**.

    c. Klik op de naam van de opslag die u voor Hudson gebruikt.

    d. Klik op **Containers**.

    e. Klik op de container met de naam **myjob**, die de kleine versie van de naam van de taak die u bij het maken van de Hudson taak toegewezen. De namen van de container en blob worden kleine letters (en hoofdlettergevoelig) in Azure opslag. In de lijst met blobs voor de container met de naam **myjob** ziet u **hello.txt** en **date.txt**. De URL voor een van deze items kopiëren en in uw browser te openen. Als een artefact build ziet u het tekstbestand dat is geüpload.

Slechts één actie op na het samenstellen die artefacten naar Azure Blob-opslag uploadt kan per project worden gemaakt. Opmerking de actie na het samenstellen artefacten uploaden naar Azure Blob-opslag kunt verschillende bestanden (inclusief jokertekens) en de paden naar de bestanden in de **Lijst van onderdelen om te uploaden** met een puntkomma als scheidingsteken opgeven. Bijvoorbeeld, als uw Hudson build JAR-bestanden en TXT-bestanden in de map van uw werkruimte van **maken produceert** en u wilt uploaden op Azure Blob-opslag, gebruikt u de volgende voor de waarde van de **Lijst van onderdelen om te uploaden** : **bouwen /\*JAR; build /\*.txt**. U kunt dubbele syntaxis ook gebruiken om een pad te gebruiken in de naam van de blob. Bijvoorbeeld, als u wilt dat de potten te krijgen geüpload met **binaire bestanden** in het pad van de blob- en TXT-bestanden te krijgen geüpload met **Aankondigingen** in de blob-pad, gebruikt u de volgende voor de waarde van de **Lijst van onderdelen om te uploaden** : **bouwen /\*. jar::binaries, build /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Het maken van een build-stap die u van Azure Blob-opslag downloadt ##

De volgende stappen laten zien hoe een stap build items downloaden van Azure Blob-opslag configureren. Dit is handig als u items wilt opnemen in de bouw, bijvoorbeeld potten die u in Azure Blob-opslag bijhoudt.

1. In de sectie van de configuratie van het **bouwen** op **toevoegen stap maken** en kies **downloaden van Azure Blob-opslag**.
2. Selecteer de opslag te gebruiken voor **opslag-accountnaam**.
3. Geef de naam van de container met de BLOB's die u wilt downloaden voor de **naam van de Container**. U kunt omgevingsvariabelen gebruiken.
4. Geef het blob **Blob-naam**. U kunt omgevingsvariabelen gebruiken. Ook kunt u een asterisk, als een jokerteken wanneer u de eerste letter (s) van de blob-naam opgegeven. Bijvoorbeeld: **project\* ** geeft alle BLOB's waarvan de naam met het **project begint**.
5. [Optioneel] **Downloadpad**, geef het pad op de Hudson machine waar u bestanden downloaden van Azure Blob-opslag. Omgevingsvariabelen kunnen ook worden gebruikt. (Als u een waarde voor **Download pad**niet opgeeft, de bestanden van Azure Blob-opslag worden gedownload naar de werkruimte van het project.)

Als u extra artikelen die u wilt downloaden van Azure Blob-opslag hebt, kunt u extra build stappen.

Nadat u een build uitgevoerd, kunt u controleren de build geschiedenis console-uitvoer, of kijk naar uw locatie, om te zien of de verwachte blobs zijn gedownload.

## <a name="components-used-by-the-blob-service"></a>Onderdelen die worden gebruikt door de service Blob ##

Hieronder vindt u een overzicht van de onderdelen van de Blob-service.

- **Opslag account**: alle toegang tot opslag Azure vindt plaats via een account voor opslag. Dit is het hoogste niveau van de naamruimte voor de toegang tot de BLOB's. Een account kan een onbeperkt aantal containers, bevatten, zolang hun totale grootte onder 100 TB.
- **Container**: een container biedt een groepering van een aantal BLOB's. Alle BLOB's moeten in een container. Een account kan een onbeperkt aantal containers bevatten. Een container kan een onbeperkt aantal BLOB's worden opgeslagen.
- **BLOB**: een bestand van het type en de grootte. Er zijn twee soorten BLOB's die kunnen worden opgeslagen in Azure opslag: BLOB's blokkeren en pagina. De meeste bestanden zijn BLOB's blokkeren. Een blob één blok mag maximaal 200 GB. In deze zelfstudie wordt BLOB's blokkeren. BLOB's pagina, een ander type blob, kunnen tot 1 TB in grootte en zijn efficiënter worden bij het bereiken van de bytes in een bestand vaak worden gewijzigd. Zie voor meer informatie over BLOB's [Wat blok BLOB's, BLOB's toevoegen en BLOB's pagina](http://msdn.microsoft.com/library/azure/ee691964.aspx).
- **URL-notatie**: BLOB's worden gebruikt met de volgende URL-indeling:

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`

    (De indeling hierboven van toepassing op de openbare Azure cloud. Als u een andere Azure cloud gebruiken het eindpunt in de [Portal Azure](https://portal.azure.com) bepalen uw eindpunt URL.)

    In de bovenstaande notatie `storageaccount` staat voor de naam van uw account opslag `container_name` staat voor de naam van de container, en `blob_name` de naam vertegenwoordigt van de blob, respectievelijk. In de containernaam van de, hebt u meerdere paden, gescheiden door een slash, **/**. De containernaam van het voorbeeld in deze zelfstudie is **MyJob**, en **${bouwen\_ID} / ${bouwen\_nummer}** is gebruikt voor het algemene virtuele pad, waardoor de blob met met de volgende URL:

    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Volgende stappen

- [Voldoen aan de Hudson](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
- [Azure opslag SDK voor Java](https://github.com/azure/azure-storage-java)
- [Verwijzing naar Azure opslag Client SDK](http://dl.windowsazure.com/storage/javadoc/)
- [Azure opslagservices REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx )
- [Azure opslag-teamblog](http://blogs.msdn.com/b/windowsazurestorage/)

Zie ook de [Java Developer Center](https://azure.microsoft.com/develop/java/)voor meer informatie.