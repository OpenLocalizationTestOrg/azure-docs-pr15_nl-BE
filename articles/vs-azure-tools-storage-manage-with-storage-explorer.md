<properties
    pageTitle="Aan de slag met Opslagverkenner (voorbeeld) | Microsoft Azure"
    description="Beheer Azure met Opslagverkenner (voorbeeld)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="getting-started-with-storage-explorer-preview"></a>Aan de slag met Opslagverkenner (voorbeeld)

## <a name="overview"></a>Overzicht 

Microsoft Azure Opslagverkenner (voorbeeld) is een zelfstandige toepassing waarmee u gemakkelijk werken met gegevens in Azure opslag op Windows, OS X en Linux. In dit artikel leert u de verschillende manieren van verbinden met en beheren van uw accounts Azure opslag.

![Microsoft Azure Opslagverkenner (voorbeeld)][15]

## <a name="prerequisites"></a>Vereisten

- [Download en installeer Opslagverkenner (voorbeeld)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Verbinding maken met een opslag- of -service

Opslagverkenner (voorbeeld) kunt u een allerlei manieren verbinding maken met opslag rekeningen. Dit omvat verbinden met opslag accounts die zijn gekoppeld aan uw Azure abonnementen, verbinding maken met opslag accounts en services van andere Azure abonnementen gedeeld en zelfs verbinden met en beheren van lokale opslag met behulp van de Emulator Azure opslag:

- [Verbinding maken met een abonnement op Azure](#connect-to-an-azure-subscription) - die deel uitmaken van uw abonnement Azure opslagbronnen beheren.
- [Werken met opslag van plaatselijke ontwikkeling](#work-with-local-development-storage) - lokale opslag met behulp van de Emulator Azure opslag beheren. 
- [Koppelen naar een externe opslag](#attach-or-detach-an-external-storage-account) - beheer die deel uitmaken van een ander Azure abonnement de accountnaam en de sleutel van de account van de opslag.
- [Opslag-account koppelen met behulp van SAS](#attach-storage-account-using-sas) - die deel uitmaken van een ander Azure abonnement met een SAS opslagbronnen beheren.
- [Service toevoegen met behulp van SAS](#attach-service-using-sas) - een specifieke storage-service (blob-container, wachtrij of tabel) beheren die deel uitmaken van een ander Azure abonnement met een SAS.

## <a name="connect-to-an-azure-subscription"></a>Verbinding maken met een Azure-abonnement

> [AZURE.NOTE] Als u geen Azure account hebt, kunt u [uw voordelen van Visual Studio abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)of [registreren voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) .

1. Selecteer **Accountinstellingen Azure**in Opslagverkenner (voorbeeld). 

    ![Azure Accountinstellingen][0]

1. Het linker deelvenster wordt nu weergegeven voor alle Microsoft-accounts die hebt aangemeld. Selecteer **een account toevoegen**voor verbinding met een andere account, en volg de dialoogvensters aan te melden met een Microsoft-account die is gekoppeld aan ten minste één actieve Azure abonnement.

    ![Een account toevoegen][1]

1. Zodra u met een Microsoft-account aanmelden, vult het linkerdeelvenster met de Azure abonnementen die aan die account is gekoppeld. Selecteer de Azure abonnementen die u wilt werken en selecteer **toepassen**. ( **Alle abonnementen** in-of uitschakelen te selecteren alle of geen van de genoemde Azure abonnementen selecteren).

    ![Selecteer Azure abonnementen][3]

1. Het linker deelvenster wordt nu de opslag-accounts die zijn gekoppeld aan de geselecteerde Azure abonnementen weergegeven.

    ![Geselecteerde Azure abonnementen][4]

## <a name="work-with-local-development-storage"></a>Werken met opslag van plaatselijke ontwikkeling

Opslagverkenner (voorbeeld) kunt u tegen lokale opslag met behulp van de Emulator Azure opslag werkt. Hiermee kunt u de code tegen schrijven en opslag testen zonder dat hiervoor een opslag-account geïmplementeerd op Azure (Aangezien de opslag account wordt geëmuleerd door de Emulator Azure opslag).

>[AZURE.NOTE] De Emulator Azure opslag wordt momenteel alleen ondersteund voor Windows. 

1. Vouw in het linkerdeelvenster van Opslagverkenner (voorbeeld), de **(bijlage en lokale** > **Opslag rekeningen** > knooppunt**(ontwikkeling)** .

    ![Knooppunt voor plaatselijke ontwikkeling][21]

1. Als u de Emulator Azure opslag nog niet hebt geïnstalleerd, wordt u gevraagd om dit te doen via de infobalk. Als de informatiebalk wordt weergegeven, selecteert u **de nieuwste versie downloaden**en installeren van de emulator. 

    ![Azure opslag Emulator prompt downloaden][22]

1. Zodra de emulator is geïnstalleerd, hebt u de mogelijkheid om te maken en werken met lokale BLOB's, tabellen en wachtrijen. Meer informatie over het werken met elk type opslag account, selecteert u op de onderstaande link:

    - [Azure blob storage resources beheren](./vs-azure-tools-storage-explorer-blobs.md)
    - Azure opslag bestandssharebronnen - *binnenkort* beheren
    - Beheer van opslagbronnen Azure wachtrij - *binnenkort*
    - Beheer van opslagbronnen tabel Azure - *binnenkort*

## <a name="attach-or-detach-an-external-storage-account"></a>Koppelen of ontkoppelen van een account voor externe opslag.

Opslagverkenner (voorbeeld) biedt de mogelijkheid om te koppelen aan accounts voor externe opslag zodat opslag accounts eenvoudig kunnen worden gedeeld. In dit gedeelte wordt uitgelegd hoe u koppelen aan (en niet) accounts voor externe opslag.

### <a name="get-the-storage-account-credentials"></a>De opslag referenties ophalen

Om delen van een account voor externe opslag, moet de eigenaar van die account de referenties - accountnaam en sleutel - de eerste keer voor de account en vervolgens delen die informatie met de persoon willen koppelen aan de account (extern). Het verkrijgen van de referenties van de opslag kan worden uitgevoerd via de Azure portal door de volgende stappen: 

1.  Log in om de [Azure portal](https://portal.azure.com).
1.  Selecteer **Bladeren**.
1.  Selecteer **de rekeningen opslag**.
1.  Selecteer de gewenste opslag in de blade **Opslag rekeningen** .
1.  Selecteer in het blad van de **Instellingen** voor de geselecteerde opslag account, **toegangstoetsen**.

    ![De optie sleutels][5]
    
1.  Kopieer in de blade **toegangstoetsen** de **Accountnaam voor opslag** en **1 sleutel** waarden voor gebruik bij het koppelen aan de account voor de opslag. 

    ![Toegangstoetsen][6]

### <a name="attach-to-an-external-storage-account"></a>Koppelen aan een account voor externe opslag.
Als u wilt koppelen aan een account voor externe opslag, moet u de naam van de account en de sleutel. De *referenties van de opslag krijgen* sectie wordt uitgelegd hoe u deze waarden verkrijgen in de portal Azure. Bedenk wel dat in het portal voor de sleutel rekening wordt genoemd "sleutel 1" dus waar de Opslagverkenner (voorbeeld) om een sleutel van de account wordt gevraagd, u zult invoeren (of plak) de waarde 'key 1'. 
 
1.  Selecteer **verbinding maken met Azure opslag**in Opslagverkenner (voorbeeld).

    ![Verbinding maken met de opslagoptie Azure][23]

1.  De sleutel rekening ('key 1' waarde van het portal voor Azure) opgeven in het dialoogvenster **verbinding maken met Azure opslag** en selecteer vervolgens **volgende**.

    ![Verbinding maken met het dialoogvenster Azure opslag][24] 

1.  In het dialoogvenster **Externe opslag koppelen** , voert u de naam van de opslag in het vak **accountnaam** en geef de gewenste instellingen op **volgende** wanneer u klaar bent. 

    ![Koppelen van externe opslag, dialoogvenster][8]

1.  Controleer of de gegevens in het dialoogvenster **Verbinding samenvatting** . Als u wijzigingen aanbrengen wilt, selecteert u **terug** en geef de gewenste instellingen. Zodra u klaar bent, selecteer **verbinding maken**.

1.  Wanneer een verbinding, wordt de rekening van externe opslag weergegeven met de tekst **(externe)** toegevoegd aan de naam van de opslag. 

    ![Het resultaat van een verbinding met een account voor externe opslag.][9]

### <a name="detach-from-an-external-storage-account"></a>Loskoppelen van een account voor externe opslag.

1.  Klik met de rechtermuisknop op de externe opslag-account die u wilt loskoppelen en - selecteer in het contextmenu - **loskoppelen**.

    ![Loskoppelen van de opslagoptie][10]

1.  Het bevestigingsbericht wordt weergegeven, selecteer **Ja** om te bevestigen het losmaken van de rekening voor externe opslag.

## <a name="attach-storage-account-using-sas"></a>SAS met opslag-account koppelen

Een [SAS (gedeelde toegang handtekening)](storage/storage-dotnet-shared-access-signature-part-1.md) biedt de beheerder van een Azure-abonnement de mogelijkheid toegang te verlenen tot een account met opslag op tijdelijke basis zonder hun abonnement Azure referenties op te geven. 

Ter illustratie, Stel dat gebruiker a is een beheerder van een Azure-abonnement en GebruikerA wil verleent toegang tot een opslag account voor een beperkte tijd met bepaalde machtigingen toestaan:

1. Gebruiker a genereert een SAS (bestaande uit de verbindingsreeks voor de opslag) voor een bepaalde periode en met de gewenste machtigingen.
1. De SAS deelt GebruikerA met de persoon die u toegang tot de opslag account - verleent, in ons voorbeeld willen.  
1. Gebruiker b wordt Opslagverkenner (voorbeeld) te koppelen aan de account van gebruiker a met behulp van de meegeleverde SA's. 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Een SAS ophalen voor de account die u wilt delen

1.  In Opslagverkenner (voorbeeld), klik met de rechtermuisknop op de opslag-account die u wilt delen en - selecteer in het contextmenu - **Handtekening met gedeelde toegang krijgen**.

    ![SAS context menuoptie ophalen][13]

1. Geef het gewenste tijdsbestek en de gewenste machtigingen voor de account in het dialoogvenster **Gedeelde toegang handtekening** en selecteer **maken**.

    ![SAS-dialoogvenster ophalen][14]
 
1. Een tweede dialoogvenster voor **Gedeelde toegang handtekening** verschijnt de SA's weer te geven. Selecteer **kopie** naast de **Verbindingsreeks** naar het Klembord kopiëren. Selecteer **sluiten** om het dialoogvenster te sluiten.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Toevoegen aan de gedeelde account met behulp van de SA 's

1.  Selecteer **verbinding maken met Azure opslag**in Opslagverkenner (voorbeeld).

    ![Verbinding maken met de opslagoptie Azure][23]

1.  De verbindingsreeks opgeven in het dialoogvenster **verbinding maken met Azure opslag** en selecteer vervolgens **volgende**.

    ![Verbinding maken met het dialoogvenster Azure opslag][24] 

1.  Controleer of de gegevens in het dialoogvenster **Verbinding samenvatting** . Als u wijzigingen aanbrengen wilt, selecteert u **terug** en geef de gewenste instellingen. Zodra u klaar bent, selecteer **verbinding maken**.

1.  Eenmaal is toegevoegd, wordt de account voor de opslag weergegeven met de tekst (SAS) toegevoegd aan de accountnaam die u hebt opgegeven.

    ![Resultaat van die is gekoppeld aan een account met SAS][17]

## <a name="attach-service-using-sas"></a>Koppelen met SAS-service

De [opslag-account koppelen met SAS](#attach-storage-account-using-sas) wordt geïllustreerd hoe een admin Azure abonnement kunt tijdelijke toegang verlenen tot een opslag-account door te genereren (en delen) een SAS voor de opslag. Op dezelfde manier kan een SAS worden gegenereerd voor een specifieke service (blob-container, wachtrij of tabel) binnen een opslag-account.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Genereren van een SAS voor de service die u wilt delen

In deze context is een service een blob-container, wachtrij of tabel. In de volgende secties wordt uitgelegd hoe de Beveiligingskoppelingen voor de vermelde service genereren:

- [De SA's voor een blob-container ophalen](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- De SA's voor een bestandsshare - *binnenkort* ophalen
- De SA's voor een wachtrij - *binnenkort* ophalen
- De SA's voor een tabel - *binnenkort* ophalen

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Toevoegen aan de gedeelde accountservice met behulp van de SA 's

1.  Selecteer **verbinding maken met Azure opslag**in Opslagverkenner (voorbeeld).

    ![Verbinding maken met de opslagoptie Azure][23]

1.  Geef de URI SAS in het dialoogvenster **verbinding maken met Azure opslag** en selecteer vervolgens **volgende**.

    ![Verbinding maken met het dialoogvenster Azure opslag][24] 

1.  Controleer of de gegevens in het dialoogvenster **Verbinding samenvatting** . Als u wijzigingen aanbrengen wilt, selecteert u **terug** en geef de gewenste instellingen. Zodra u klaar bent, selecteer **verbinding maken**.

1.  Eenmaal is toegevoegd, wordt de zojuist gekoppelde service wordt weergegeven onder het knooppunt **(SAS Service)** .

    ![Resultaat van het koppelen aan een gedeelde service met SAS][20]

## <a name="search-for-storage-accounts"></a>Zoeken naar opslag rekeningen

Als u een lange lijst met accounts voor opslag hebt, is een snelle manier om te zoeken naar een bepaalde opslag account gebruik van het zoekvak boven in het linkerdeelvenster. 

Als u in het zoekvak typt, wordt het linkerdeelvenster alleen de opslag accounts die overeenkomen met de zoekwaarde tot ingevoerde dat punt weergegeven. De volgende schermopname ziet u een voorbeeld waarin ik alle opslag rekeningen waar de naam van de opslag met de tekst 'tarcher' hebt gezocht.

![Opslag account zoeken][11]
    
Als u wilt wissen, klik op **x** in het zoekvak.

## <a name="next-steps"></a>Volgende stappen
- [Beheer van opslagbronnen Azure blob met Opslagverkenner (voorbeeld)](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
