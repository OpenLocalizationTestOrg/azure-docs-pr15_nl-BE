
<properties 
    pageTitle="Hoe wordt Azure RemoteApp gebruikersgegevens en -instellingen opgeslagen? | Microsoft Azure"
    description="Informatie over hoe gegevens van de gebruiker met behulp van de schijf van de gebruiker profiel in Azure RemoteApp wordt opgeslagen."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Hoe wordt Azure RemoteApp gebruikersgegevens en -instellingen opgeslagen?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp slaat gebruikersidentiteit en aanpassingen door apparaten en sessies. Gegevens van de gebruiker wordt opgeslagen in een schijf per gebruiker per collectie, een schijf van de gebruiker profiel (UPD) genoemd. De schijf volgt de gebruiker en zorgt voor dat de gebruiker heeft een consistente ervaring, ongeacht waar ze zich aanmelden. Hiermee slaat u 

Gebruiker profiel schijven zijn volledig transparant voor de gebruiker: gebruikers documenten opslaan in de map documenten (op wat lijkt op een lokaal station) en hun instellingen app gebruikelijke manier te wijzigen. Alle persoonlijke instellingen behouden tijdens de verbinding met RemoteApp Azure vanaf elk apparaat. Alle gebruikers zien hun gegevens op dezelfde plaats is.

Elke UPD 50GB permanente opslagruimte heeft en bevat instellingen voor beide gebruikers gegevens en toepassingen. 

Lees verder voor details op gebruikersprofielgegevens.

>[AZURE.NOTE] Moet de UPD uitschakelen? Doet u dat nu - uitchecken Pavithra van blogbericht, [Schakelen gebruikers profiel schijven (UPDs) in Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/), voor meer informatie.


## <a name="how-can-an-admin-get-to-the-data"></a>Hoe kan een admin om de gegevens te krijgen?

Als u toegang tot de gegevens voor een van uw gebruikers (voor herstel na storingen of als de gebruiker het bedrijf verlaat), neem contact op met ondersteuning van Azure en de abonnementsgegevens voor de collectie en identiteit van de gebruiker. De Azure RemoteApp-team bieden u een URL naar de VHD. Download die VHD en ophalen van alle documenten of bestanden die u nodig hebt. Let erop dat de VHD 50GB, dus er is een bit te downloaden.


## <a name="is-the-data-backed-up"></a>De gegevens een reservekopie wordt?

Ja, we een back-up van de gebruikersgegevens per geografische locatie opslaan. De gegevens alleen-lezen is en toegankelijk is op dezelfde manier de gegevens regelmatig worden (contact Azure RemoteApp aanschaffen), als het primaire gegevenscentrum niet beschikbaar is. De gegevens real-time wordt gekopieerd naar de back-uplocatie en we doen geen kopieën bewaren van verschillende versies. Dus we niet mogelijk om deze te herstellen naar een eerder bekende juiste versie op beschadiging van gegevens, maar als het primaire gegevenscentrum niet actief is, is het mogelijk om gegevens van de gebruiker van de andere locatie.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Hoe gebruikers zien de UPD op de server?

Elke gebruiker heeft een eigen map op de server die is toegewezen aan hun UPD: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Wat is de beste manier om het gebruik van Outlook en UPD?

Azure RemoteApp slaat het Outlook staat (postbussen PST-bestanden) tussen sessies. Daartoe moeten we het PST-bestand moet worden opgeslagen in de gebruikersprofielgegevens (c:\users\<gebruikersnaam >). Dit is de standaardlocatie voor de data, dus zolang de locatie niet hebt gewijzigd, blijft van de gegevens tussen sessies.

Ook aangeraden "cache"-modus gebruiken in Outlook te klikken en "server/on line"-modus gebruiken om te zoeken.

[In dit artikel](remoteapp-outlook.md) voor meer informatie over het gebruik van Outlook en Azure RemoteApp-uitchecken.

## <a name="what-about-redirection"></a>Hoe zit het met omleiding?
U kunt RemoteApp Azure als gebruikers toegang krijgen tot lokale apparaten door het instellen van [omleiding](remoteapp-redirection.md)wilt laten configureren. Lokale apparaten zijn vervolgens toegang krijgen tot de gegevens op de UPD.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Kan ik mijn UPD als een netwerkshare gebruiken?
Nr. UPDs kan niet worden gebruikt als een netwerkshare. Een UPD is alleen beschikbaar voor de gebruiker wanneer de gebruiker actief met Azure RemoteApp is verbonden.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Als ik een gebruiker uit een collectie verwijderen, hun UPD verwijderd?

Wanneer u een gebruiker verwijdert, Nee, we automatisch Verwijder niet de UPD - in plaats daarvan we de gegevens worden opgeslagen totdat u de collectie verwijderen. 90 dagen nadat u de collectie, verwijderen verwijderen we alle UPDs. 

Als u wilt een UPD uit een collectie verwijderen, neem contact op met Azure RemoteApp - UPD van onze kant kan worden verwijderd.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Kan ik mijn gebruikers UPDs (huidige of verwijderde gebruikers) openen?

Ja, als u contact opneemt met [Azure RemoteApp](mailto:remoteappforum@microsoft.com), we kunnen instellen u met een URL voor toegang tot de gegevens. Alle gegevens of bestanden downloaden van het UPD voordat de toegang verloopt hebt u ongeveer 10 uur.

## <a name="are-upds-available-offline"></a>UPDs off line beschikbaar te zijn?

Op dit moment we geen off line toegang tot UPDs, buiten de bieden hierboven beschreven 10 uur access-venster. Dit betekent dat we op dit moment geen kunt u toegang bieden voor lange genoeg meer complexe taken uitvoeren, zoals antivirusprogramma's uitgevoerd op de UPDs of toegang tot gegevens voor een controle.

## <a name="do-registry-key-settings-persist"></a>Registersleutelinstellingen aanhouden?
Ja, geschreven naar HKEY_Current_User is onderdeel van de UPD.

## <a name="can-i-disable-upds-for-a-collection"></a>Kan ik voor een verzameling UPDs uitschakelen?

Ja, kunt u vragen Azure RemoteApp UPDs uitschakelen voor een abonnement, maar u dat zelf niet doen. Dit betekent dat UPDs wordt uitgeschakeld voor alle collecties in het abonnement.

U kunt uitschakelen van de UPDs in een van de volgende situaties: 

- U moet toegang en beheer van gebruikersgegevens hebt voltooid (voor controle en de analyse van doeleinden, zoals financiële instellingen).
- U hebt gebruiker 3de partij profile management oplossingen in de lokalen en wilt blijven gebruiken in uw implementatie Azure RemoteApp-domein behoren. Hiervoor moeten de agent profiel in de Gouden afbeelding worden geladen. 
- U hoeft niet alle lokale gegevensopslag of u beschikt over alle gegevens in de cloud of bestandsshare en besturingselement wilt opslaan van gegevens met behulp van lokaal Azure RemoteApp.

[Uitschakelen gebruiker profiel schijven (UPDs) in Azure RemoteApp-](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) Zie voor meer informatie.

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Kan ik voorkomen dat gebruikers gegevens op het systeemstation opslaan?

Ja, maar je hebt ingesteld die in de Sjabloonafbeelding voordat u de collectie maken. Gebruik de volgende stappen uit voor het blokkeren van toegang tot het station:

1. Voer **gpedit.msc uit** op de Sjabloonafbeelding.
2. Ga naar **Gebruikersconfiguratie > Beheersjablonen > Windows-onderdelen > Explorer**.
3. Selecteer de volgende opties:
    - **Opgegeven stations verbergen in deze Computer**
    - **Toegang tot stations vanaf deze Computer verhinderen**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Kan ik UPDs zaad? Ik wil enkele gegevens in de UPD die beschikbaar is de eerste keer dat de gebruiker zich aanmeldt.

Ja, als u de Sjabloonafbeelding maakt, kunt u informatie toevoegen aan het standaardprofiel. Deze informatie wordt vervolgens toegevoegd aan de UPD.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>Kan ik de grootte van de UPD afhankelijk van hoeveel gegevens ik wil opslaan wijzigen?

Nee, alle UPDs 50 GB opslagruimte hebt. Als u verschillende hoeveelheden gegevens worden opgeslagen wilt, probeert u het volgende:

1. UPDs uitschakelen voor de collectie.
2. Instellen voor de gebruikers toegang hebben tot gedeelde bestanden.
3. Laad de bestandsshare met behulp van een opstartscript. Zie hieronder voor meer informatie over opstartscripts in Azure RemoteApp.
4. Directe gebruikers alle gegevens opslaan om het bestand te delen.


## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Hoe voer ik een opstartscript in Azure RemoteApp?

Als u een opstartscript uitvoert wilt, start u een geplande taak maakt in de Sjabloonafbeelding die u wilt gebruiken voor de collectie. (Doet dit *voordat* u sysprep uitvoert.) 

![Een systeemtaak maken](./media/remoteapp-upd/upd1.png)

![Een systeemtaak maken die wordt uitgevoerd wanneer een gebruiker zich aanmeldt](./media/remoteapp-upd/upd2.png)

Op het tabblad **Algemeen** , moet u beveiliging 'BUILTIN\Users.' de **Gebruikersaccount** wijzigen

![Wijzig de gebruikersaccount aan een groep](./media/remoteapp-upd/upd4.png)

Het opstartscript met behulp van de referenties van de gebruiker, de geplande taak wordt gestart. De taak uit te voeren elke keer een gebruiker zich aanmeldt.

![De trigger voor de taak 'op aanmelden' instellen](./media/remoteapp-upd/upd3.png)

U kunt ook [scripts starten op basis van Groepsbeleid](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx)gebruiken. 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Hoe zit een opstartscript plaatsen in het menu Start? Zou dat werken?

Met andere woorden, kan ik een bat-bestand dat wordt uitgevoerd een config venster script maken en opslaan in de map c:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp en hebt dat script uitvoeren wanneer een gebruiker een RemoteApp-sessie start?

Nee, dat niet wordt ondersteund met RemoteApp-Azure, dat gebruikmaakt van RDSH, die ook biedt geen ondersteuning voor opstartscripts in het menu Start.

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Kan ik mstsc.exe (het programma Extern bureaublad) gebruiken voor het configureren van aanmeldingsscripts

Nee, niet wordt ondersteund door Azure RemoteApp.

## <a name="can-i-store-data-on-the-vm-locally"></a>Kan ik gegevens opslaan op de VM lokaal?

Nee, op de VM, andere dan in de UPD opgeslagen gegevens niet verloren. Er is een grote kans dat de gebruiker krijgt geen dezelfde VM de volgende keer dat ze zich in Azure RemoteApp aanmeldt. Ofte geen gebruiker VM persistentie, zodat de gebruiker niet hetzelfde VM aanmelden, en de gegevens verloren. Wanneer we de collectie bijwerken, worden de bestaande VMs ook vervangen door een nieuwe reeks VMs - dat inhoudt dat gegevens die zijn opgeslagen op de VM zelf gaat verloren. De aanbeveling is voor het opslaan van gegevens in de UPD, gedeelde opslag, net als een bestandsserver in een VNET of van de wolk met behulp van een systeem voor cloud opslag zoals DropBox Azure bestanden.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Hoe ik een bestandsshare Azure koppelen op een VM, met PowerShell?

De Net-PSDrive-cmdlet kunt u het station, als volgt te koppelen:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


Ook kunt u uw referenties opslaat door het uitvoeren van de volgende opties:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Dat kunt u de - parameter van de referentie in de cmdlet New-PSDrive.
