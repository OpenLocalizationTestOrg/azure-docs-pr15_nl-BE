<properties
    pageTitle="De afbeelding van een aangepaste sjabloon maken voor RemoteApp Azure | Microsoft Azure"
    description="Informatie over het maken van een afbeelding aangepaste sjabloon voor Azure RemoteApp. U kunt deze sjabloon gebruiken met een hybride of een cloud-collectie."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-custom-template-image-for-azure-remoteapp"></a>De afbeelding van een aangepaste sjabloon maken voor Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Een installatiekopie van Windows Server 2012 R2 sjabloon Azure RemoteApp gebruikt als host voor de programma's die u wilt delen met uw gebruikers. Om een aangepaste afbeelding voor RemoteApp-sjabloon maakt, kunt u beginnen met een bestaande afbeelding of maak een nieuwe. 


> [AZURE.TIP] Wist u dat u kunt een afbeelding maken van een Azure VM? True story en wordt het tijd het neemt de afbeelding wilt importeren. Bekijk de stappen [hier](remoteapp-image-on-azurevm.md).

De voorschriften voor de afbeelding die u voor gebruik met Azure RemoteApp uploaden kunt zijn.


- De grootte van de afbeelding moet een veelvoud van MBs. Als u een afbeelding die niet is een exact veelvoud uploaden probeert, mislukt het uploaden van bestand.
- De grootte van de afbeelding moet 127 GB of kleiner.
- Het moet op een VHD-bestand (VHDX bestanden [Hyper-V virtuele vaste schijven] worden momenteel niet ondersteund).
- De VHD moet een generatie 2 virtuele machine.
- De VHD zijn vaste grootte en dynamisch uitbreidbare. Een dynamisch uitbreidbare VHD wordt aanbevolen omdat er minder tijd om te uploaden naar Azure dan een vaste grootte VHD-bestand nodig.
- De schijf moet worden geïnitialiseerd met behulp van de MBR Master Boot Record () partitioneringsstijl. De partitiestijl GUID partition table (GPT) wordt niet ondersteund.
- De VHD moet bevatten één installatie van Windows Server 2012 R2. Het kan meerdere volumes, maar slechts één met een installatie van Windows bevatten.
- De functie Remote Desktop Session Host (RDSH) en de functie Bureaubladbelevenis moeten zijn geïnstalleerd.
- De functie Remote Desktop Connection Broker moet *niet* worden geïnstalleerd.
- Encrypting File System (EFS) moet worden uitgeschakeld.
- De afbeelding moet worden SYSPREPed met de parameters **/oobe / generalize/shutdown** (niet gebruiken met de parameter **/mode:vm** ).
- De VHD van de keten van een momentopname te uploaden, wordt niet ondersteund.


**Voordat u begint**

U moet voordat u de service maakt als volgt:

- [Meld u aan](https://azure.microsoft.com/services/remoteapp/) voor RemoteApp.
- Een gebruikersaccount maken in Active Directory moet worden gebruikt als de serviceaccount RemoteApp. De machtigingen voor deze account beperken zodat deze alleen computers aan het domein toevoegen kunt. [Configureer Azure Active Directory voor RemoteApp](remoteapp-ad.md) Zie voor meer informatie.
- Informatie verzamelen over uw netwerk op ruimten: IP-adres VPN-apparaat gegevens.
- Installeer de module [Azure PowerShell](../powershell-install-configure.md) .
- Informatie verzamelen over de gebruikers die u toegang wilt verlenen aan. Dit kan Microsoft gegevens of gegevens over de account Active Directory werk voor gebruikers zijn.



## <a name="create-a-template-image"></a>De Sjabloonafbeelding van een maken ##

Dit zijn de hoogste niveau stappen voor het maken van een nieuwe Sjabloonafbeelding maken:

1.  Zoek naar een Windows Server 2012 R2 Update DVD of ISO-installatiekopie.
2.  Een VHD-bestand maken.
4.  Installeer Windows Server 2012 R2.
5.  De functie Remote Desktop Session Host (RDSH) en de functie Bureaubladbelevenis installeren.
6.  Extra functies die nodig zijn voor uw toepassingen te installeren.
7.  Installeer en configureer uw toepassingen. Delen van apps om gemakkelijker te maken, voeg apps of programma's die u wilt delen in het menu **Start** van de afbeelding, in het bijzonder in **%systemdrive%\ProgramData\Microsoft\Windows\Start Start\Programma 's.
8.  Voer eventuele aanvullende Windows-configuraties nodig zijn voor uw toepassingen.
9.  Uitschakelen van Encrypting File System (EFS).
10. **REQUIRED:** Ga naar Windows Update en installeer alle belangrijke updates.
9.  SYSPREP om de installatiekopie.

De gedetailleerde stappen voor het maken van een nieuwe afbeelding zijn:

1.  Zoek naar een Windows Server 2012 R2 Update DVD of ISO-installatiekopie.
2.  Een VHD-bestand maken met behulp van Schijfbeheer.
    1.  Start Schijfbeheer (diskmgmt.msc).
    2.  Maak een dynamisch uitbreidbare VHD van 40 GB of meer in grootte. (Schatting van de hoeveelheid ruimte die nodig is voor Windows, uw toepassingen en aanpassingen. Windows Server met de rol van de RDSH en de functie Bureaubladbelevenis is geïnstalleerd moet ongeveer 10 GB aan ruimte).
        1.  Klik op **actie > VHD maken**.
        2.  Geef de locatie, grootte en indeling van de VHD. **Dynamisch uitbreidbare**selecteren en klik vervolgens op **OK**.

            Gedurende enkele seconden wordt uitgevoerd. Bij het maken van de VHD voltooid is, ziet u een nieuwe schijf zonder een stationsletter en Schijfbeheer staat 'Niet geïnitialiseerd'.

        - Klik met de rechtermuisknop op de schijf (niet in de vrije ruimte) en klik vervolgens op **Schijf initialiseren**. Selecteer de partitiestijl **MBR** (Master Boot Record) en klik vervolgens op **OK**.
        - Een nieuw volume maakt: niet-toegewezen ruimte met de rechtermuisknop en klik vervolgens op **Nieuw eenvoudig Volume**. U kunt accepteer de standaardinstellingen in de wizard, maar zorg ervoor dat u een stationsletter aan het voorkomen van mogelijke problemen bij het uploaden van de Sjabloonafbeelding toewijzen.
        - Klik met de rechtermuisknop op de schijf en klik vervolgens op **VHD ontkoppelen**.





1. Windows Server 2012 R2 installeert:
    1. Maak een nieuwe virtuele machine. Gebruik de Wizard Nieuwe virtuele Machine in Hyper-V Manager of Client Hyper-V.
        1. Kies op de pagina Geef generatie **1 generatie**.
        2. Selecteer **een bestaande virtuele harde schijf gebruiken**op de pagina verbinding maken met virtuele harde schijf en blader naar de VHD u in de vorige stap hebt gemaakt.
        2. Selecteer **een besturingssysteem vanaf een opstart-CD/DVD_ROM installeren**op de pagina installatieopties en selecteert u de locatie van de installatiemedia van Windows Server 2012 R2.
        3. Kies andere opties in de wizard voor het installeren van Windows en uw toepassingen. De wizard is voltooid.
    2.  Nadat de wizard is voltooid, de instellingen van de virtuele machine bewerken en breng eventuele andere wijzigingen nodig zijn voor de installatie van Windows en uw programma's, zoals het aantal virtuele processors, en klik vervolgens op **OK**.
    4.  Verbinding maken met de virtuele machine en installeren van Windows Server 2012 R2.
1. De functie Remote Desktop Session Host (RDSH) en de functie Bureaubladbelevenis installeren:
    1. Server Manager starten.
    2. Klik op **functies toevoegen en onderdelen** in het welkomstscherm of via het menu **beheren** .
    3. Klik op **volgende** op de pagina voordat u begint.
    4. **Installatie op basis van de rol of functie**te selecteren en klik vervolgens op **volgende**.
    5. Selecteer de lokale computer uit de lijst en klik op **volgende**.
    6. Selecteer **Extern bureaublad-Services**en klik vervolgens op **volgende**.
    7. Vouw **gebruikersinterfaces en infrastructuur** en selecteer **Bureaubladbelevenis**.
    8. Klik op **Onderdelen toevoegen**en klik vervolgens op **volgende**.
    9. Klik op **volgende**op de pagina extern bureaublad-Services.
    10. Klik op **Host voor extern bureaublad-sessie**.
    11. Klik op **Onderdelen toevoegen**en klik vervolgens op **volgende**.
    12. Selecteer **de doelserver opnieuw opstarten automatisch indien nodig**op de pagina bevestigen installatie selecties en klik vervolgens op **Ja** in de waarschuwing opnieuw wordt opgestart.
    13. Klik op **installeren**. De computer wordt opnieuw opgestart.
1.  Extra functies die nodig zijn voor toepassingen, zoals het .NET Framework 3.5 installeren. Om de functies te installeren, de functies toevoegen en de Wizard functies worden uitgevoerd.
7.  Installeer en configureer de programma's en toepassingen die u wilt publiceren via RemoteApp.

>[AZURE.IMPORTANT]
>
>Installeer de rol van de RDSH voor de installatie van toepassingen om ervoor te zorgen dat eventuele problemen met de compatibiliteit van toepassingen worden ontdekt voordat de installatiekopie wordt geüpload naar RemoteApp.
>
>Controleer of dat er verschijnt een snelkoppeling naar de toepassing (**lnk** -bestand) in het menu **Start** voor alle gebruikers (%systemdrive%\ProgramData\Microsoft\Windows\Start Start\Programma 's). Ook voor zorgen dat het pictogram dat u in het menu **Start** ziet wat u wilt dat gebruikers is te zien. Als dat niet het geval is, deze wijzigen. (U niet *hebben* de toepassing toevoegen aan de Start menu, maar veel makkelijker voor het publiceren van de toepassing in RemoteApp. Anders, u moet het installatiepad voor de toepassing wanneer u de app publiceert.)


8.  Voer eventuele aanvullende Windows-configuraties nodig zijn voor uw toepassingen.
9.  Uitschakelen van Encrypting File System (EFS). De volgende opdracht bij een verhoogde opdrachtvenster uitvoeren:

        Fsutil behavior set disableencryption 1

    U kunt ook instellen of de volgende DWORD-waarde toevoegen in het register:

        HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
9.  Als u de afbeelding binnen een Azure virtuele machine bouwt, wijzig de naam van de ** \%windir%\Panther\Unattend.xml** bestand, als dit het uploaden script later gebruikt niet werkt blokkeert. De naam van dit bestand wijzigen naar Unattend.old, zodat nog u het bestand hebt in het geval u moet terugkeren uw implementatie.
10. Ga naar Windows Update en installeer alle belangrijke updates. Mogelijk moet u Windows Update meerdere malen om alle updates uitvoeren. (Soms u een update installeert en die update zelf moet worden bijgewerkt).
10. SYSPREP om de installatiekopie. Voer de volgende opdracht bij een opdrachtprompt:

    **C:\Windows\System32\sysprep\sysprep.exe / generalize/oobe/shutdown**

    **Opmerking:** Gebruik de schakeloptie **/mode:vm** van de opdracht SYSPREP niet zelfs als dit een virtuele machine.


## <a name="next-steps"></a>Volgende stappen ##
Nu u hebt uw image aangepaste sjabloon, moet u die afbeelding uploaden naar uw RemoteApp-collectie. Gebruik de informatie in de volgende artikelen om uw verzameling te maken:


- [Het maken van een hybride RemoteApp-collectie](remoteapp-create-hybrid-deployment.md)
- [Het maken van een wolk RemoteApp-collectie](remoteapp-create-cloud-deployment.md)
 