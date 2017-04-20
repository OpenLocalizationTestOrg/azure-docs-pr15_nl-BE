<properties
    pageTitle="Voorbereiden van een Windows-VHD te uploaden naar Azure | Microsoft Azure"
    description="Aanbevolen procedures voor het voorbereiden van een VHD Windows voordat u uploadt naar Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="genlin"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/18/2016"
    ms.author="glimoli;genli"/>

# <a name="prepare-a-windows-vhd-to-upload-to-azure"></a>Voorbereiden van een Windows-VHD te uploaden naar Azure
Een Windows VM op ruimten naar Azure uploaden, moet u de virtuele harde schijf (VHD) goed voorbereiden. Er zijn verschillende aanbevolen stappen kunt uitvoeren voordat u een VHD naar Azure uploaden. Dit artikel ziet u hoe u een Windows-VHD te uploaden naar Microsoft Azure voorbereidt en hierin wordt ook uitgelegd [wanneer en hoe het gebruik van Sysprep](#step23).

## <a name="prepare-the-virtual-disk"></a>De virtuele schijf voorbereiden

>[AZURE.NOTE] 
> Azure ondersteunt alleen [generation 1 virtuele machines](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx) worden in de VHD-indeling. De nieuwere VHDX-indeling wordt niet ondersteund in Azure. 
>
> De VHD moet een vaste grootte niet dynamisch. Indien nodig, details de onderstaande instructies uit VHDX of dynamische schijven converteren. De maximale grootte voor de VHD 1023 GB is.


Zorg ervoor dat de VHD Windows goed op de lokale server werkt. Corrigeer eventuele fouten in de VM zelf voordat u probeert te converteren of te uploaden naar Azure.

Als u de virtuele schijf converteren naar de juiste indeling voor Azure, een van de methoden vermeld in de volgende secties gebruiken. Back-up van de VM voordat u een virtuele schijf conversie of Sysprep uitvoert.

### <a name="convert-using-hyper-v-manager"></a>Converteren met behulp van Hyper-V-beheer
- Hyper-V-beheer te openen en selecteer de lokale computer aan de linkerkant. Klik in het menu boven deze **actie** > **Schijf bewerken**.
    - Ga naar op het scherm **virtuele harde schijf zoeken** en selecteren van de virtuele schijf.
    - In het volgende scherm selecteert u **converteren**
        - Als u converteren van VHDX wilt, selecteert u **VHD** en klik op **volgende**
        - Als u converteren van een dynamische schijf wilt, selecteert u **vaste grootte** en klik op **volgende**

    - Blader naar en selecteer **het pad voor de nieuwe VHD-bestand**.
    - Klik op **Voltooien** om af te sluiten.

### <a name="convert-using-powershell"></a>Met PowerShell converteren
U kunt een virtuele schijf met behulp van de [omzetten VHD PowerShell-cmdlet](http://technet.microsoft.com/library/hh848454.aspx)kunt converteren. In het volgende voorbeeld worden we converteren van een VHDX naar een VHD en converteren van een dynamische naar vaste-type:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

### <a name="convert-from-vmware-vmdk-disk-format"></a>Converteren van VMware VMDK schijfindeling
Als u een installatiekopie van Windows VM in de [bestandsindeling VMDK hebt](https://en.wikipedia.org/wiki/VMDK), deze converteren naar een VHD met het [Conversieprogramma voor Microsoft Virtual Machine](https://www.microsoft.com/download/details.aspx?id=42497). Lees het blog van [het converteren van een VMware VMDK te VHD Hyper-V](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) voor meer informatie.

## <a name="prepare-windows-configuration-for-upload"></a>Windows configureren voor het uploaden van voorbereiden

> [AZURE.NOTE] De volgende opdrachten worden uitgevoerd met [beheerdersbevoegdheden](https://technet.microsoft.com/library/cc947813.aspx).

1. Verwijder alle statische permanente route in de routeringstabel:

    - Uitvoeren als u wilt weergeven in de routetabel, `route print`.
    - Raadpleeg de secties **Permanente Routes** . Als er een permanente route, gebruik [route verwijderen](https://technet.microsoft.com/library/cc739598.apx) om deze te verwijderen.

2. Verwijder de WinHTTP-proxy:

    ```
    netsh winhttp reset proxy
    ```

3. De schijf SAN-beleid om [Onlineall](https://technet.microsoft.com/library/gg252636.aspx)te configureren:

    ```
    diskpart san policy=onlineall
    ```

4. Time Coordinated Universal Time (UTC) gebruiken voor Windows en het opstarttype van de Windows Time (w32time)-service ingesteld op **automatisch**:

    ```
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    sc config w32time start= auto
    ```


## <a name="configure-windows-services"></a>Windows-services configureren
5. Zorg ervoor dat elk van de volgende Windows-services is ingesteld op de **standaardwaarden van Windows**. Ze worden geconfigureerd met de opstartinstellingen in de volgende lijst wordt vermeld. U kunt de standaardinstellingen van het opstarten van deze opdrachten uitvoeren:

    ```
    sc config bfe start= auto

    sc config dcomlaunch start= auto

    sc config dhcp start= auto

    sc config dnscache start= auto

    sc config IKEEXT start= auto

    sc config iphlpsvc start= auto

    sc config PolicyAgent start= demand

    sc config LSM start= auto

    sc config netlogon start= demand

    sc config netman start= demand

    sc config NcaSvc start= demand

    sc config netprofm start= demand

    sc config NlaSvc start= auto

    sc config nsi start= auto

    sc config RpcSs start= auto

    sc config RpcEptMapper start= auto

    sc config termService start= demand

    sc config MpsSvc start= auto

    sc config WinHttpAutoProxySvc start= demand

    sc config LanmanWorkstation start= auto

    sc config RemoteRegistry start= auto
    ```


## <a name="configure-remote-desktop-configuration"></a>Extern bureaublad configureren
6. Als er zelfondertekende certificaten gekoppeld aan de listener Remote Desktop Protocol (RDP), deze verwijderen:

    ```
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
    ```

    Zie voor meer informatie over het configureren van certificaten voor RDP-listener [Listener certificaat configuraties in Windows Server](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)

7. De waarden voor [KeepAlive](https://technet.microsoft.com/library/cc957549.aspx) voor RDP-service configureren:

    ```
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```

8. De verificatiemodus voor de RDP-service configureren:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```

9. RDP-service inschakelen door de volgende subsleutels in het register toe te voegen:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```


## <a name="configure-windows-firewall-rules"></a>Windows Firewall-regels configureren
10. WinRM toestaan via drie firewallprofielen (domein, privé en openbaar) en PowerShell Remote service in te schakelen:

    ```
    Enable-PSRemoting -force
    ```

11. Zorg ervoor dat de volgende Gast besturingssysteem firewall-regels zijn:

    - Inkomend

    ```
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    ```

    - Inkomende en uitgaande

    ```
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

    netsh advfirewall firewall set rule group="Core Networking" new enable=yes
    ```

    - Uitgaande

    ```
    netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
    ```


## <a name="additional-windows-configuration-steps"></a>Extra configuratiestappen voor Windows
12. Voer `winmgmt /verifyrepository` om te bevestigen dat de opslagplaats voor Windows Management Instrumentation (WMI) consistent is. Als de opslagplaats beschadigd is, raadpleegt u [dit blogbericht](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

13. Controleer of de gegevens BCD (Boot Configuration)-instellingen overeenkomen met de volgende:

    ```
    bcdedit /set {bootmgr} integrityservices enable

    bcdedit /set {default} device partition=C:

    bcdedit /set {default} integrityservices enable

    bcdedit /set {default} recoveryenabled Off

    bcdedit /set {default} osdevice partition=C:

    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
    ```

14. Verwijder eventuele extra filters Transport Driver Interface, zoals de software die u analyseert de TCP-pakketten.
15. Uitvoeren om te controleren of de schijf in orde en consistent is, de `CHKDSK /f` opdracht.
16. Verwijder alle andere software van andere leveranciers en betrekking hebben op fysieke onderdelen of andere virtualisatietechnologie stuurprogramma.
17. Zorg ervoor dat een toepassing geen gebruik van poort 3389 maakt. Deze poort wordt gebruikt voor het RDP-service in Azure. U kunt de `netstat -anob` opdracht om te controleren de poorten die door de toepassingen worden gebruikt.
18. Als een domeincontroller is de VHD van Windows die u wilt uploaden, voert u [deze extra stappen](https://support.microsoft.com/kb/2904015) voor het voorbereiden van de schijf.
19. Opnieuw opstarten van de VM om ervoor te zorgen dat Windows nog steeds in orde is kan worden bereikt met behulp van de RDP-verbinding.
20. Het lokale beheerderswachtwoord opnieuw en zorg ervoor dat u kunt deze account aanmelden bij Windows via de RDP-verbinding.  Deze machtiging wordt bepaald door het beleidsobject 'Aanmelden toestaan via Extern bureaublad-Services'. Dit object bevindt zich onder "computer\Computerconfiguratie\Windows-instellingen\Beveiligingsinstellingen\Lokaal Beleid\toewijzing."


## <a name="install-windows-updates"></a>Windows-Updates installeren
22. Installeer de nieuwste updates voor Windows. Als dat niet mogelijk is, zorg dat de volgende updates zijn geïnstalleerd:

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VM's uit een netwerkstoring niet herstellen en voorkomen van problemen met beschadigde gegevens

    - [KB3115224](https://support.microsoft.com/kb/3115224) Verbeteringen van de betrouwbaarheid van VMs die worden uitgevoerd op een Windows Server 2012 R2 of Windows Server 2012-host

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Beveiligingsupdate voor Microsoft Windows tot misbruik van bevoegdheden adres: 8 maart 2016

    - [KB3063075](https://support.microsoft.com/kb/3063075) Veel ID 129 gebeurtenissen worden vastgelegd wanneer u een virtuele machine voor Windows Server 2012 R2 in Microsoft Azure uitvoert

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VM's uit een netwerkstoring niet herstellen en voorkomen van problemen met beschadigde gegevens

    - [KB3114025](https://support.microsoft.com/kb/3114025) Trage prestaties wanneer u bestanden Azure opslag vanaf Windows 8.1 of Server 2012 R2

    - [KB3033930](https://support.microsoft.com/kb/3033930) Hotfix verhoogt de limiet van 64K op RIO buffers per proces voor Windows Azure-service

    - [KB3004545](https://support.microsoft.com/kb/3004545) U heeft geen toegang tot virtuele machines die worden gehost op Azure hosting-services via een VPN-verbinding in Windows

    - [KB3082343](https://support.microsoft.com/kb/3082343) Cross-Premises VPN-verbindingen gaat verloren wanneer Windows Server 2012 R2 RRAS Azure site naar site VPN-tunnels

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Beveiligingsupdate voor Microsoft Windows tot misbruik van bevoegdheden adres: 8 maart 2016

    - [KB3146723](https://support.microsoft.com/kb/3146723) MS16-048: Beschrijving van de beveiligingsupdate voor CSRSS: 12 April 2016
    - [KB2904100](https://support.microsoft.com/kb/2904100) Systeem loopt vast tijdens de schijf I/O in Windows<a id="step23"></a>
23. Als u wilt voor het maken van een afbeelding voor de implementatie van meerdere machines uit, moet u de installatiekopie generaliseren door te voeren `sysprep` voordat u de VHD naar Azure uploaden. U hoeft niet uit te voeren `sysprep` voor het gebruik van een speciale VHD. Raadpleeg de volgende artikelen voor meer informatie over het maken van een algemene installatiekopie:

    - [Een VM-afbeelding maken van een bestaande Azure VM met behulp van het implementatiemodel Resource Manager](virtual-machines-windows-create-vm-generalized.md)
    - [Een VM-afbeelding maken van een bestaande Azure VM met de implementatie van klassieke modem](virtual-machines-windows-classic-capture-image.md)
    - [Sysprep-ondersteuning voor Server-rollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)



## <a name="suggested-extra-configurations"></a>Voorgestelde extra configuraties

De volgende instellingen hebben geen invloed op de VHD uploaden. Echter, wordt aangeraden dat u ze geconfigureerd hebt.

- De [Agent Azure virtuele Machines](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)installeren. Nadat u de agent hebt geïnstalleerd, kunt u de VM-extensies inschakelen. De VM-serverextensies implementeert de meeste van de essentiële functionaliteit die u gebruiken met uw VMs wilt zoals wachtwoorden, opnieuw instellen configureren RDP en vele anderen.

- Het logboek Dump kan helpen bij het oplossen van problemen met Windows crash zijn. De collectie Dump logboek inschakelen:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

    sc config wer start= auto
    ```

- Nadat de VM in Azure is gemaakt, configureert u het systeem gedefinieerde grootte van wisselbestand op station D:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```


## <a name="next-steps"></a>Volgende stappen

- [Een Windows VM afbeelding uploaden naar Azure voor implementaties Resource Manager](virtual-machines-windows-upload-image.md)
