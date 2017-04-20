<properties
    pageTitle="Met behulp van Mapomleiding in Azure RemoteApp | Microsoft Azure"
    description="Meer informatie over het configureren en gebruiken van omleiding in RemoteApp"
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

# <a name="using-redirection-in-azure-remoteapp"></a>Omleiding in Azure RemoteApp gebruiken

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Apparaatomleiding kan uw gebruikers communiceren met externe toepassingen met behulp van de apparaten die zijn aangesloten op de lokale computer, telefoon of tablet. Als u Skype via RemoteApp Azure hebt opgegeven, moet de gebruiker de camera geïnstalleerd op hun computer om met Skype te werken. Dit geldt ook voor printers, luidsprekers, monitors en een bereik van USB aangesloten randapparatuur.

RemoteApp maakt gebruik van de Remote Desktop Protocol (RDP) en RemoteFX voor omleiding.

## <a name="what-redirection-is-enabled-by-default"></a>De omleiding is standaard ingeschakeld?
Wanneer u met RemoteApp, wordt het volgende omleidingen zijn standaard ingeschakeld. De gegevens tussen haakjes de RDP-instelling weergeven.

- Geluiden afspelen op de lokale computer (**op deze computer afspelen**). (audiomode:i:0)
- Geluid opnemen van de lokale computer en naar de externe computer (**de Record van deze computer**). (audiocapturemode:i:1)
- Afdrukken op lokale printers (redirectprinters:i:1)
- COM-poorten (redirectcomports:i:1)
- Smartcard-apparaat (redirectsmartcards:i:1)
- Klembord (mogelijkheid om te kopiëren en plakken) (redirectclipboard:i:1)
- Schakel type lettertypen vloeiend maken (lettertypen vloeiend maken toestaan: i:1)
- Alle ondersteunde Plug en Play-apparaten omleiden. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Welke andere omleiding is beschikbaar?
Er zijn twee opties voor Mapomleiding standaard uitgeschakeld:

- Omleiding van clientstations (stationstoewijzing): schijven van de lokale computer worden toegewezen stations in de externe sessie. Hiermee kunt u opslaan of bestanden openen vanaf uw lokale stations terwijl u in de externe sessie werkt.
- USB-omleiding: U kunt de USB-apparaten aangesloten op uw lokale computer vanuit de externe sessie.

## <a name="change-your-redirection-settings-in-remoteapp"></a>In RemoteApp-omleidingsinstellingen wijzigen
U kunt de instellingen voor apparaatomleiding voor een verzameling wijzigen met behulp van de Microsoft Azure PowerShell met SDK. Na de installatie van de nieuwe SDK en PowerShell eerst configureren voor het beheren van uw abonnement, zoals beschreven in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

Vervolgens kunt een vergelijkbaar met de volgende opdracht stelt u de aangepaste RDP-eigenschappen:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Houd er rekening mee dat *'n* wordt gebruikt als scheidingsteken tussen afzonderlijke eigenschappen.)

Als u een lijst met welke aangepaste RDP-eigenschappen zijn geconfigureerd, voert u de volgende cmdlet. Houd er rekening mee dat alleen aangepaste eigenschappen worden weergegeven als resultaat en niet de standaardeigenschappen:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Als u aangepaste eigenschappen moet u alle aangepaste eigenschappen telkens; anders wordt de instelling uitgeschakeld.   

### <a name="common-examples"></a>Gangbare voorbeelden
Gebruik de volgende cmdlet stationsomleiding inschakelen:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*”

Deze cmdlet gebruiken voor het inschakelen van USB- en station omleiden:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Deze cmdlet gebruikt het gedeelde Klembord uitschakelen:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0”

> [AZURE.IMPORTANT] Zorg ervoor dat u volledig afmelden alle gebruikers in de collectie (en niet alleen deze verbinding verbreken) voordat u de wijzigingen test. Om zijn volledig afgemeld, Ga naar het tabblad **sessies** in de collectie in de portal Azure en gebruikers die worden verbroken of ondertekend afmelden. Soms kan het enkele seconden voor de lokale stations in Explorer weergeven binnen de sessie duren.

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>USB-omleidingsinstellingen wijzigen op de Windows-client

Als u USB-omleiding te gebruiken op een computer die verbinding met een RemoteApp wilt maakt, zijn er 2 acties die moeten gebeuren. 1 - de beheerder moet het USB-omleiding op het niveau van inschakelen met behulp van Azure PowerShell. 2 - op elk apparaat op de USB-omleiding gebruiken waar u wilt, moet u een groepsbeleid waarmee deze inschakelen. Deze stap moet worden uitgevoerd voor elke gebruiker die wil USB-omleiding te gebruiken.

> [AZURE.NOTE] USB-omleiding met Azure RemoteApp wordt alleen ondersteund voor Windows-computers.

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a>Schakel USB-omleiding voor de RemoteApp-collectie
De volgende cmdlet gebruiken voor het inschakelen van USB-omleiding op het niveau van:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a>USB-omleiding voor de client inschakelen

USB-omleidingsinstellingen configureren op uw computer:

1. Open de Editor voor lokaal groepsbeleid (GPEDIT. MSC). (Voer gpedit.msc uit vanaf de opdrachtprompt).
2. Open **Computer Configuration\Policies\Administrative Templates\Windows onderdelen\Extern Desktop Services\Remote Desktop Connection Client\RemoteFX USB-apparaat omleiden**.
3. Dubbelklik op de **omleiding van de andere ondersteunde RemoteFX USB-apparaten op deze computer toestaan RDP**.
4. Selecteer **ingeschakeld**en selecteer **beheerders en gebruikers in de toegangsrechten RemoteFX USB-omleiding**.
5. Open een opdrachtprompt met beheerdersmachtigingen en voer de volgende opdracht uit:

        gpupdate /force
6. Start de computer opnieuw.

U kunt ook het hulpprogramma Groepsbeleidsbeheer maken en toepassen van het omleidingsbeleid USB voor alle computers in uw domein:

1. Log in op de domeincontroller als beheerder van het domein.
2. Open de Console Groepsbeleidsbeheer. (Klik op **Start > Systeembeheer > beheer van Groepsbeleid**.)
3. Ga naar het domein of de organisatie-eenheid waarvoor u wilt het beleid maken.
4. Klik met de rechtermuisknop op **Standaarddomeinbeleid**en klik vervolgens op **bewerken**.
5. Open **Computer Configuration\Policies\Administrative Templates\Windows onderdelen\Extern Desktop Services\Remote Desktop Connection Client\RemoteFX USB-apparaat omleiden**.
6. Dubbelklik op de **omleiding van de andere ondersteunde RemoteFX USB-apparaten op deze computer toestaan RDP**.
7. Selecteer **ingeschakeld**en selecteer **beheerders en gebruikers in de toegangsrechten RemoteFX USB-omleiding**.
8. Klik op **OK**.  
