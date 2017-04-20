<properties
    pageTitle="Problemen met Azure bestand opslag | Microsoft Azure"
    description="Problemen met Azure bestand opslag"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genli"/>

# <a name="troubleshooting-azure-file-storage-problems"></a>Problemen met Azure bestand opslag

In dit artikel worden bekende problemen met betrekking tot Microsoft Azure opslagruimte wanneer u verbinding vanaf een Windows- en Linux-clients maakt. Het biedt ook de mogelijke oorzaken en oplossingen voor deze problemen.

**Algemene problemen (optreden in Windows- en Linux-clients)**

- [Quota-fout wanneer u probeert een bestand te openen](#quotaerror)

- [Trage prestaties wanneer u Linux of Windows Azure bestandsopslag](#slowboth)

**Problemen met Windows-client**

- [Trage prestaties wanneer u Azure bestandsopslag vanuit Windows 8.1 of Windows Server 2012 R2](#windowsslow)

- [Fout 53 probeert te koppelen van een bestandsshare Azure](#error53)

- [Gebruik NET is gelukt, maar ik Zie de Azure-bestand in Windows Verkenner gemonteerde delen](#netuse)

- [Mijn account opslag bevat "/" en het net met opdracht mislukt](#slashfails)

- [Mijn toepassingsservice geen toegang tot gekoppelde bestanden Azure-station.](#accessfiledrive)

- [Extra aanbevelingen om prestaties te optimaliseren](#additional)

**Problemen met Linux-client**

- [Foutbericht 'U kopieert een bestand naar een bestemming die codering wordt niet ondersteund' bij het uploaden/bestanden kopiëren naar Azure-bestanden](#encryption)

- [Foutbericht 'Host is niet beschikbaar' in het bestaande bestand deelt, of de shell loopt vast bij het uitvoeren van opdrachten op het koppelpunt weergeven](#errorhold)

- [Fout 115 koppelen wanneer u probeert te koppelen Azure-bestanden op Linux VM](#error15)

- [Linux VM willekeurige vertraging in opdrachten zoals "ls"](#delayproblem)

## <a name="general-problems"></a>Algemene problemen
<a id="quotaerror"></a>
### <a name="quota-error-when-trying-to-open-a-file"></a>Quota-fout wanneer u probeert een bestand te openen

In Windows wordt de volgende foutberichten:

**1816 ERROR_NOT_ENOUGH_QUOTA <> – 0xc0000044**

**STATUS_QUOTA_EXCEEDED**

**Er is onvoldoende quotum beschikbaar om deze opdracht te verwerken**

**Ongeldige ingang waarde GetLastError: 53**

Op Linux foutberichten die vergelijkbaar zijn met het volgende:

**<filename>[toestemming geweigerd]**

**Schijfquotum overschreden**

#### <a name="cause"></a>Oorzaak

Het probleem treedt op omdat de bovengrens van gelijktijdige open ingangen die zijn toegestaan voor een bestand is bereikt.

#### <a name="solution"></a>Oplossing

Verminder het aantal gelijktijdige open ingangen door sommige grepen te sluiten en probeer het vervolgens opnieuw. Zie voor meer informatie [Microsoft Azure opslag prestaties en schaalbaarheid, controlelijst](storage-performance-checklist.md).

<a id="slowboth"></a>
### <a name="slow-performance-when-accessing-file-storage-from-windows-or-linux"></a>Verminderde prestaties bij het opslaan van bestanden openen vanuit een Windows- of Linux

- Als u een bepaalde i/o-grootte minimumvereiste niet hebt, wordt u aangeraden 1 MB te gebruiken als de i/o-grootte voor optimale prestaties.

- Als u weet dat de uiteindelijke grootte van een bestand dat u zijn uitbreiden met schrijven en uw software heeft geen compatibiliteitsproblemen optreden wanneer de staart nog niet geschreven op het bestand met nullen, stelt u de grootte van het vooraf in plaats van elke schrijven wordt een uitgebreid schrijven.

## <a name="windows-client-problems"></a>Problemen met Windows-client
<a id="windowsslow"></a>
### <a name="slow-performance-when-accessing-the-file-storage-from-windows-81-or-windows-server-2012-r2"></a>Verminderde prestaties bij het opslaan van bestanden openen vanuit Windows 8.1 of Windows Server 2012 R2

Zorg ervoor dat de hotfix [KB3114025](https://support.microsoft.com/kb/3114025) is geïnstalleerd voor clients die werken met Windows 8.1 of Windows Server 2012 R2. Deze hotfix verbetert de maken en prestaties van de ingang sluit.

U kunt het volgende script om te controleren of de hotfix is geïnstalleerd op uitvoeren:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Als de hotfix is geïnstalleerd, wordt de volgende uitvoer weergegeven:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies**

**{96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1**

> [AZURE.NOTE]  Windows Server 2012 R2 afbeeldingen in Azure Marketplace hebt de hotfix geïnstalleerd standaard vanaf December 2015 KB3114025.

<a id="additional"></a>
### <a name="additional-recommendations-to-optimize-performance"></a>Extra aanbevelingen om prestaties te optimaliseren

Nooit maken of openen van een bestand op in de cache I/O die is aangevraagd, maar geen lees-schrijftoegang. Dat wil zeggen, wanneer u belt **CreateFile()**, nooit alleen **GENERIC_WRITE opgeven**, maar altijd opgeven **GENERIC_READ | GENERIC_WRITE**. Een ingang alleen-schrijven kan niet lokaal, kleine schrijfopdrachten cache zelfs wanneer het is de enige geopende ingang voor het bestand. Dit legt de prestaties van een ernstige op kleine schrijfopdrachten. Houd er rekening mee dat de 'a'-modus voor CRT- **fopen()** een handle met alleen-schrijven opent.

<a id="error53"></a>
### <a name="error-53-when-you-try-to-mount-or-unmount-an-azure-file-share"></a>'Fout 53' wanneer u probeert te koppelen en ontkoppelen van een bestandsshare Azure

Dit probleem kan worden veroorzaakt door de volgende voorwaarden:

#### <a name="cause-1"></a>Oorzaak 1

"Systeemfout 53 is opgetreden. Toegang is geweigerd." Verbindingen met Azure bestandsshares worden om veiligheidsredenen geblokkeerd als het communicatiekanaal is niet versleuteld en de verbindingspoging niet is vervaardigd van dezelfde Datacenter waarop gedeelde Azure bestanden zich bevinden. Codering van communicatie is niet beschikbaar als de gebruiker van de client-OS SMB-codering niet ondersteunt. Dit wordt aangegeven door een 'Systeemfout 53 is opgetreden. Toegang geweigerd' wordt weergegeven wanneer een gebruiker probeert om een bestandsshare op ruimten of uit een ander datacenter te koppelen. Windows 8, Windows Server 2012 en latere versies van elke aanvraag onderhandelen over SMB-3.0 codering ondersteunt met.

#### <a name="solution-for-cause-1"></a>Oplossing voor oorzaak 1

Verbinding maken vanaf een client die voldoet aan de vereisten van Windows 8, Windows Server 2012 of nieuwere versies of die verbinding maken vanaf een virtuele machine die zich in het datacenter hetzelfde als de opslag Azure-account die wordt gebruikt voor de bestandsshare Azure.

#### <a name="cause-2"></a>Oorzaak 2

'Systeemfout 53' wanneer u koppelt een Azure bestandsshare kan optreden als uitgaande communicatie tot datacenter Azure bestanden poort 445 wordt geblokkeerd. Klik [hier](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx) voor het overzicht van de Internet-providers die wel of niet toestaan van toegang via poort 445.

Deze poort wordt geblokkeerd door comcast en sommige IT-organisaties. Als u wilt weten of dit de reden achter het bericht 'Systeemfout 53 is', kunt u Portqry opvragen van het eindpunt van de TCP:445. Als het eindpunt van de TCP:445 wordt weergegeven, zoals filteren, is de TCP-poort geblokkeerd. Hier volgt een van de voorbeeldquery:

`g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Als de TCP 445 wordt geblokkeerd door een regel op het netwerkpad, u de volgende uitvoer ziet:

**TCP-poort 445 (service microsoft ds): GEFILTERD**

Zie voor meer informatie over het gebruik van Portqry.exe [Beschrijving van het opdrachtregelprogramma Portqry.exe](https://support.microsoft.com/kb/310099).

#### <a name="solution-for-cause-2"></a>Oplossing voor oorzaak 2

Werken met uw IT-organisatie poort 445 uitgaande [Azure IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653)te openen.

#### <a name="cause-3"></a>Oorzaak 3

'Systeemfout 53' kan ook als NTLMv1 communicatie is ingeschakeld op de client worden ontvangen. NTLMv1 ingeschakeld met maakt een minder veilige client. Daarom wordt de communicatie geblokkeerd voor Azure-bestanden. Om te controleren of dit de oorzaak van de fout is, controleert u of de volgende subsleutel in het register is ingesteld op een waarde van 3:

HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel.

Zie het onderwerp [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) op TechNet voor meer informatie.

#### <a name="solution-for-cause-3"></a>Oplossing voor oorzaak 3

Dit probleem oplossen door de waarde van LmCompatibilityLevel in de registersleutel HKLM\SYSTEM\CurrentControlSet\Control\Lsa om de standaardwaarde van 3 te herstellen.

Azure-bestanden ondersteunt alleen NTLMv2-verificatie. Zorg ervoor dat Groepsbeleid wordt toegepast op de clients. Hierdoor wordt voorkomen dat deze fout optreedt. Dit wordt ook beschouwd als veiligheidsoverwegingen. Zie voor meer informatie [het gebruik van NTLMv2-clients configureren met behulp van Groepsbeleid](https://technet.microsoft.com/library/jj852207(v=ws.11).aspx)

De aanbevolen instelling is **alleen alleen een NTLMv2-antwoord**. Dit komt overeen met een waarde van 3. Clients gebruiken alleen NTLMv2-verificatie en ze gebruiken NTLMv2-sessiebeveiliging als de server dit ondersteunt. Domeincontrollers accepteren LM-, NTLM- en NTLMv2-verificatie.

<a id="netuse"></a>
### <a name="net-use-was-successful-but-dont-see-the-azure-file-share-mounted-in-windows-explorer"></a>Gebruik NET is voltooid, maar het bestand niet ziet Azure delen gekoppelde in Windows Verkenner

#### <a name="cause"></a>Oorzaak

Standaard uitvoeren Windows Verkenner niet als Administrator. Als u **net gebruik** vanaf een opdrachtprompt als Administrator uitvoert, toewijzen u het netwerkstation 'als Administrator'. Aangezien toegewezen stations gebruiker-georiënteerde, weer de gebruikersaccount waarmee is aangemeld niet de stations als zij onder een andere account zijn gekoppeld.

#### <a name="solution"></a>Oplossing

Koppel de share vanaf de opdrachtregel voor een niet-administrator. U kunt ook [dit onderwerp TechNet](https://technet.microsoft.com/library/ee844140.aspx) configureren van de registerwaarde **EnableLinkedConnections** volgen.

<a id="slashfails"></a>
### <a name="my-storage-account-contains--and-the-net-use-command-fails"></a>Mijn account opslag bevat "/" en het net met opdracht mislukt

#### <a name="cause"></a>Oorzaak

Wanneer u de opdracht **net use** is uitgevoerd onder de MS-DOS-Prompt (cmd.exe), wordt deze door het toevoegen van '/' als een opdrachtregeloptie geparseerd. Dit zorgt ervoor dat de stationstoewijzing mislukken.

#### <a name="solution"></a>Oplossing

U kunt een van de volgende stappen uit om het probleem te omzeilen:

• Gebruik de volgende PowerShell-opdracht:

`New-SmbMapping -LocalPath y: -RemotePath \\server\share  -UserName acountName -Password "password can contain / and \ etc"`

Vanuit een batchbestand kan dit gebeuren als

`Echo new-smbMapping ... | powershell -command –`

• Plaatst u dubbele aanhalingstekens rond de sleutel om dit probleem te omzeilen, tenzij "/" is het eerste teken. Als, de interactieve modus gebruiken en voer uw wachtwoord afzonderlijk of opnieuw genereren van sleutels om een sleutel die niet met een slash (/)-teken beginnen.

<a id="accessfiledrive"></a>
### <a name="my-applicationservice-cannot-access-mounted-azure-files-drive"></a>Mijn toepassingsservice geen toegang tot gekoppelde bestanden Azure station

#### <a name="cause"></a>Oorzaak

Stations zijn per gebruiker gekoppeld. Als uw toepassing of service wordt uitgevoerd onder een andere gebruikersaccount, niet het station door gebruikers zien.

#### <a name="solution"></a>Oplossing

Station koppelen vanuit dezelfde gebruikersaccount waaronder de toepassing is. U kunt dit doen met behulp van hulpprogramma's zoals gg.bat.

U kunt ook een nieuwe gebruiker die dezelfde bevoegdheden als de network service- of systeem-account heeft en vervolgens **cmdkey** en **Gebruik net** onder die account worden uitgevoerd. De gebruikersnaam moet de naam van de opslag en wachtwoord moet de sleutel opslag account. Een andere optie **net** use is om door te geven in de accountnaam van de opslag en de sleutel van de gebruiker en wachtwoord de parameters van de opdracht **net use** .

Nadat u deze instructies volgt, wordt het volgende foutbericht weergegeven: 'Systeemfout 1312. De opgegeven aanmeldingssessie bestaat niet. Het is mogelijk al beëindigd' bij het uitvoeren van **netto gebruik** voor het systeem of netwerk service-account. Als dit gebeurt, zorg ervoor dat de gebruikersnaam die is doorgegeven aan **netto gebruik** domeininformatie bevat (bijvoorbeeld: ' [accountnaam opslag]. file.core.windows .net ').

## <a name="linux-client-problems"></a>Problemen met Linux-client

<a id="encryption"></a>
### <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Foutbericht 'U kopieert een bestand naar een bestemming die codering wordt niet ondersteund'

#### <a name="cause"></a>Oorzaak

BitLocker gecodeerde bestanden kunnen worden gekopieerd naar Azure-bestanden. De opslag van bestanden ondersteunt echter geen NTFS-EFS. Daarom gebruik u waarschijnlijk van EFS in dit geval. Als u bestanden die zijn gecodeerd met EFS hebt, kan een kopieerbewerking voor de opslag van bestanden niet tenzij de opdracht copy wordt een gekopieerde bestand decoderen.

#### <a name="workaround"></a>Tijdelijke oplossing

Als u wilt een bestand kopiëren naar de opslag van bestanden, moet u deze eerst decoderen. U kunt dit doen met behulp van een van de volgende methoden:

• Gebruik **/d kopiëren**.

• Stel de volgende registersleutel:

- Path = HKLM\Software\Policies\Microsoft\Windows\System
- Type de waarde DWORD =
- Naam = CopyFileAllowDecryptedRemoteDestination
- Waarde = 1

Bedenk wel dat het instellen van de registersleutel is van invloed op alle kopieerbewerkingen op netwerkshares.

<a id="errorhold"></a>
### <a name="host-is-down-error-on-existing-file-shares-or-the-shell-hangs-when-you-run-list-commands-on-the-mount-point"></a>Foutbericht 'Host is niet beschikbaar' in het bestaande bestand deelt, of de shell loopt vast wanneer u de lijst opdrachten op het koppelpunt uitvoeren

#### <a name="cause"></a>Oorzaak

Deze fout treedt op de Linux client wanneer de client gedurende langere tijd inactief is geweest. De client wordt verbroken wanneer deze fout optreedt, en time-out voor de clientverbinding.

#### <a name="solution"></a>Oplossing

Dit probleem is nu opgelost in de Linux kernel als onderdeel van een [verzameling wijzigen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93), in behandeling zijnde backport in Linux-distributie.

Om te werken rond dit probleem, de verbinding te handhaven en te voorkomen dat een inactief, houden de Azure-share die u om periodiek schrijft een bestand. Dit is een schrijfbewerking, zoals de datum gemaakt/gewijzigd in het bestand te herschrijven. Anders kunt u krijgen in de cache opgeslagen resultaten en de verbinding mogelijk niet geactiveerd door de bewerking.

<a id="error15"></a>
### <a name="mount-error-115-when-you-try-to-mount-azure-files-on-the-linux-vm"></a>'Fout 115 koppelen' wanneer u probeert bestanden op de Linux VM Azure koppelen

#### <a name="cause"></a>Oorzaak

Linux-distributies ondersteunen niet coderingsfunctie in SMB-3.0. In sommige distributies ontvangen gebruiker een '115'-foutbericht als ze mount Azure bestanden via SMB 3.0 vanwege een ontbrekend onderdeel.

#### <a name="solution"></a>Oplossing

Als de Linux SMB-client die wordt gebruikt, ondersteunt geen codering, mount Azure-bestanden via SMB 2.1 van een Linux VM op dezelfde Datacenter als het bestand opslag account.

<a id="delayproblem"></a>
### <a name="linux-vm-experiencing-random-delays-in-commands-like-ls"></a>Linux VM willekeurige vertraging in opdrachten zoals "ls"

#### <a name="cause"></a>Oorzaak

Dit kan gebeuren wanneer de opdracht mount niet de optie **serverino omvat** . Zonder **serverino**, wordt de opdracht ls een **stat** uitgevoerd op elk bestand.

#### <a name="solution"></a>Oplossing

Controleer de **serverino** in de post "/ etc/fstab":

azureuser.File.Core.Windows.NET/WMS/Comer in/home/sampledir type cifs (rw, nodev, relatime, ver = 2.1, sec = ntlmssp, cache = strikte, username = xxx, domein = X, file_mode = 0755, dir_mode 0755, serverino, rsize = = 65536, wsize = 65536, actimeo = 1)

Als de optie **serverino** niet aanwezig is, te ontkoppelen en Azure bestanden opnieuw koppelen met de optie **serverino** is geselecteerd.

## <a name="learn-more"></a>Meer informatie

- [Aan de slag met Azure opslaan van bestanden in Windows](storage-dotnet-how-to-use-files.md)

- [Aan de slag met Azure bestandsopslag op Linux](storage-how-to-use-files-linux.md)
