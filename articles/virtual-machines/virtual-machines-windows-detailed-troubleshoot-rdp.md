<properties
    pageTitle="Uitgebreide probleemoplossing extern bureaublad | Microsoft Azure"
    description="Bekijk de gedetailleerde stappen voor extern bureaublad fouten waar u niet naar een Windows virtuele machines in Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"
    keywords="kan geen verbinding maken met extern bureaublad oplossen van problemen met remote desktop, extern bureaublad geen verbinding maken, fouten met remote desktop, extern bureaublad oplossen, problemen met extern bureaublad
"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Gedetailleerde stappen voor probleemoplossing problemen met verbinding met extern bureaublad voor Windows VMs in Azure

Dit artikel bevat gedetailleerde stappen voor probleemoplossing kunt opsporen en herstellen van fouten in complexe extern bureaublad op basis van Windows Azure virtuele machines.

> [AZURE.IMPORTANT] Om te voorkomen het veelvoorkomende fouten van extern bureaublad, moet lezen [voor extern bureaublad basic troubleshooting artikel](virtual-machines-windows-troubleshoot-rdp-connection.md) voordat u verdergaat.

Kan een foutbericht van extern bureaublad die op een van de specifieke foutberichten vallen in [de probleemoplossingsgids basic extern bureaublad](virtual-machines-windows-troubleshoot-rdp-connection.md)niet lijken te verschijnen. Ga als volgt te werk om te bepalen waarom de extern bureaublad-client kan geen verbinding maken met de RDP-service op de Azure VM is.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Als u meer Help-informatie op elk punt in dit artikel, kunt u contact opnemen met de Azure experts op [de MSDN Azure en de forums stackoverloop](https://azure.microsoft.com/support/forums/). Ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [website voor ondersteuning van Azure](https://azure.microsoft.com/support/options/) en op **Ondersteuning krijgen**. Lees de [Veelgestelde vragen over het Microsoft Azure ondersteuning](https://azure.microsoft.com/support/faq/)voor meer informatie over het gebruik van Azure ondersteuning.


## <a name="components-of-a-remote-desktop-connection"></a>Onderdelen van een verbinding met extern bureaublad

De volgende onderdelen zijn in een RDP-verbinding betrokken:

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_0.png)

Voordat u doorgaat, kan het handig zijn om geestelijk te controleren wat er is gewijzigd sinds de laatste geslaagde verbinding met extern bureaublad voor VM. Bijvoorbeeld:

- Het openbare IP-adres van de VM of de cloud-service met de VM (ook wel virtuele IP-adres [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) is gewijzigd. De RDP-storing kan het zijn dat de cache van de DNS-client nog steeds het *oude IP-adres* voor de DNS-naam is geregistreerd heeft. De DNS-client leegmaken en probeer opnieuw verbinding maken met de VM. Of probeer verbinding te maken met de nieuwe VIP.
- U gebruikt een toepassing voor het beheren van uw extern bureaublad-verbindingen in plaats van de verbinding die wordt gegenereerd door de portal Azure. Controleer of de configuratie van de toepassing bevat de juiste TCP-poort voor het verkeer voor extern bureaublad. U kunt deze poort voor een klassieke virtuele machine in de [portal Azure](https://portal.azure.com)controleren door te klikken op instellingen van de VM > eindpunten.


## <a name="preliminary-steps"></a>Voorbereidende stappen

Voordat u overgaat tot de gedetailleerde oplossen

- Controleer de status van de virtuele machine in de klassieke Azure portal of de Azure portal voor de hand liggende problemen.
- Volg de [stappen voor algemene RDP-fouten in de probleemoplossingsgids basic quick fix](virtual-machines-windows-troubleshoot-rdp-connection.md#quick-troubleshooting-steps).


Probeer opnieuw verbinding met de VM via Extern bureaublad na deze stappen.


## <a name="detailed-troubleshooting-steps"></a>Gedetailleerde stappen voor probleemoplossing

De extern bureaublad-client mogelijk niet kunnen bereiken van de extern bureaublad-service op de Azure VM door problemen op de volgende bronnen:

- [Remote Desktop clientcomputer](#source-1-remote-desktop-client-computer)
- [Organisatie intranet edge-apparaat](#source-2-organization-intranet-edge-device)
- [Cloud-service-eindpunt en toegangsbeheerlijsten (ACL's)](#source-3-cloud-service-endpoint-and-acl)
- [Netwerk-beveiligingsgroepen](#source-4-network-security-groups)
- [Op basis van Windows Azure VM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>1 bron: Remote Desktop clientcomputer

Controleer of dat uw computer aan een andere op-, Windows-computer met extern bureaublad-verbindingen kunt maken.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_1.png)

Als u niet de volgende instellingen op uw computer controleren:

- Een lokale firewall-instelling die in extern bureaublad-verkeer blokkeert.
- Lokaal clientsoftware van proxy waardoor verbindingen met extern bureaublad hebt geïnstalleerd.
- Network monitoring software die verbindingen met extern bureaublad kan lokaal zijn geïnstalleerd.
- Andere soorten beveiligingssoftware om verkeer te controleren of bepaalde typen verkeer, waardoor verbindingen voor extern bureaublad toestaan/weigeren.

In deze gevallen tijdelijk uitschakelen van de software en maak verbinding met een computer op de gebouwen via Extern bureaublad. Als u de werkelijke oorzaak op deze manier vinden kunt, kunt u werken met de netwerkbeheerder om te corrigeren van de software-instellingen extern bureaublad-verbindingen wilt toestaan.

## <a name="source-2-organization-intranet-edge-device"></a>Bron 2: Organisatie intranet edge-apparaat

Controleer of dat een computer die rechtstreeks is verbonden met het Internet extern bureaublad-verbindingen met uw Azure virtuele machine kunt maken.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_2.png)

Als er niet een computer die rechtstreeks is verbonden met het Internet, maken en testen met een nieuwe Azure virtual machine in een resource-groep of cloud-service. Zie [virtuele machines maken waarop u Windows in Azure](virtual-machines-windows-hero-tutorial.md)voor meer informatie. U kunt de virtuele machine en de resourcegroep of de service cloud verwijderen na de test.

Als u verbinding met extern bureaublad met een computer die rechtstreeks is aangesloten op het Internet maken kunt, controleert u uw organisatie intranet edge-apparaat voor:

- Een interne firewall blokkeren HTTPS-verbindingen met het Internet.
- Een proxy-server extern bureaublad-verbindingen te blokkeren.
- Inbraakdetectie of network monitoring software die op apparaten in uw netwerk rand, waardoor verbindingen met extern bureaublad wordt uitgevoerd.

Werken met de netwerkbeheerder om de instellingen van uw organisatie intranet edge-apparaat HTTPS gebaseerde externe bureaubladverbindingen toestaan om Internet te corrigeren.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Bron 3: Cloud-service-eindpunt en ACL

Voor VMs gemaakt met behulp van het model Klassiek implementatie, moet u controleren of dat een andere Azure VM in dezelfde cloud service- of virtuele netwerk verbindingen met extern bureaublad voor uw Azure VM maken kunt.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_3.png)

> [AZURE.NOTE] Virtuele machines in Resource Manager gemaakt, gaat u naar [bron 4: beveiligingsgroepen netwerk](#source-4-network-security-groups).

Als u een virtuele machine in een virtuele netwerk of dezelfde cloud-service niet hebt, maken. Volg de stappen in [een virtuele machine maken waarop u Windows in Azure](virtual-machines-windows-hero-tutorial.md). De test virtuele machine verwijderen nadat de test is voltooid.

Als u verbinding via Extern bureaublad met een virtuele machine in dezelfde cloud service- of virtuele netwerk maken kunt, controleert deze instellingen:

- De endpoint-configuratie voor extern bureaublad-verkeer op het doel VM: de particuliere TCP-poort van het eindpunt moet overeenkomen met de TCP-poort waarop de VM van extern bureaublad-service luistert (3389 standaard is).
- De ACL voor het eindpunt van de extern bureaublad-verkeer op het doel VM: ACL's kunnen u opgeven toegestaan of geweigerd binnenkomend verkeer vanaf het Internet op basis van het bron-IP-adres. Onjuist geconfigureerde ACL's kunnen voorkomen dat binnenkomend verkeer voor extern bureaublad naar het eindpunt. Controleer uw ACL's om ervoor te zorgen dat binnenkomende verkeer van uw openbare IP-adressen van uw proxyserver of andere edge-server is toegestaan. Zie voor meer informatie [Wat is een Network Access Control-lijst (ACL)?](../virtual-network/virtual-networks-acl.md)

Verwijder het huidige eindpunt en een nieuwe maken, een willekeurige poort in het bereik 49152 – 65535 voor de externe poortnummer kiezen om te controleren of het eindpunt de bron van het probleem. Zie voor meer informatie, [het instellen van de eindpunten aan een virtuele machine](virtual-machines-windows-classic-setup-endpoints.md).

## <a name="source-4-network-security-groups"></a>Bron 4: Netwerk-beveiligingsgroepen

Netwerk-beveiligingsgroepen toestaan nauwkeurigere besturing van toegestane binnenkomend en uitgaand verkeer. U kunt regels spanning subnetten maken en cloud services in een virtueel netwerk van Azure. Controleer of uw netwerk groep regels om ervoor te zorgen dat de extern bureaublad-verkeer vanaf het Internet is toegestaan:

- Selecteer uw VM in Azure portal,
- Klik op **Instellingen voor alle** | **Netwerkinterfaces** en selecteer de netwerkinterface.
- Klik op **Instellingen voor alle** | **groep netwerk** en selecteer de beveiligingsgroep netwerk.
- Klik op **Instellingen voor alle** | **binnenkomende regels** en controleer of er een regel RDP op TCP-poort 3389 toestaan.
    - Als u een regel niet hebt, klikt u op **toevoegen** om een regel te maken. **TCP** voor het protocol en **3389** voor het poortbereik bestemming invoeren.
    - Controleer of dat de actie is ingesteld op **toestaan** en klik op OK om de nieuwe binnenkomende regel opslaan.


Zie voor meer informatie [Wat is een Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)

## <a name="source-5-windows-based-azure-vm"></a>Bron 5: Op basis van Windows Azure VM

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_5.png)

De [IaaS Azure (Windows) diagnostics-pakket](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) gebruiken om te controleren of de fout door de Azure virtual machine zelf. Als dit pakket met diagnostische gegevens niet kan het probleem van de **RDP-verbinding met een VM in Azure (opnieuw opstarten vereist)** , volg de instructies in [dit artikel](virtual-machines-windows-reset-rdp.md). Dit artikel stelt de service Extern bureaublad op de virtuele machine:

- De standaardregel 'Extern bureaublad' Windows Firewall (TCP-poort 3389) inschakelen.
- Extern bureaublad-verbindingen inschakelen door de registerwaarde HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections in te stellen op 0.

Probeer opnieuw de verbinding van uw computer. Als u nog steeds geen verbinding maken via Extern bureaublad, Controleer de volgende mogelijke problemen:

- De extern bureaublad-service wordt niet uitgevoerd op de doelcomputer VM.
- De extern bureaublad-service luistert niet op TCP-poort 3389.
- Windows Firewall of een andere lokale firewall heeft een uitgaande regel waardoor verkeer voor extern bureaublad.
- Inbraakdetectie of network monitoring software die wordt uitgevoerd op de Azure virtuele machine kunnen verbindingen met externe bureaubladen.

Voor VMs gemaakt met behulp van het implementatiemodel klassiek, kunt u een externe sessie Azure PowerShell Azure virtual machine gebruiken. Eerst moet u een certificaat installeren voor de virtuele machine hostingservice wolk. Ga naar de [Beveiligde externe PowerShell toegang configureren Azure virtuele Machines](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) en het **InstallWinRMCertAzureVM.ps1** scriptbestand downloaden naar uw lokale computer.

Vervolgens Azure PowerShell installeren als u dat nog niet gedaan hebt. Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

Open een opdrachtprompt met Azure PowerShell en wijzigt de huidige map naar de locatie van het scriptbestand **InstallWinRMCertAzureVM.ps1** . Als u wilt een Azure PowerShell script uitvoert, moet u de juiste uitvoering van beleid instellen. Voer de opdracht **Get uitvoeringsbeleid** uw huidige niveau van beleid. Zie voor meer informatie over het instellen van het juiste niveau [Instellen uitvoeringsbeleid](https://technet.microsoft.com/library/hh849812.aspx).

Vervolgens vult u de abonnementsnaam van uw Azure, de naam van de wolk en de naam van uw virtuele machine (verwijderen van de < en > tekens), en vervolgens deze opdrachten uitvoeren.

    $subscr="<Name of your Azure subscription>"
    $serviceName="<Name of the cloud service that contains the target virtual machine>"
    $vmName="<Name of the target virtual machine>"
    .\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName

Naam van het juiste abonnement krijgt u van de eigenschap _SubscriptionName_ van de weergave van de opdracht **Get-AzureSubscription** . U krijgt de service cloud-naam voor de virtuele machine uit de _servicenaam_ kolom in de weergave van de opdracht **Get-AzureVM** .

Controleer als u een nieuw certificaat hebben. Open een module Certificaten voor de huidige gebruiker en kijk in de map **Vertrouwde basis-CA Authorities\Certificates** . U ziet nu een certificaat met de DNS-naam van uw cloud-service in de kolom verleend aan (voorbeeld: cloudservice4testing.cloudapp.net).

Vervolgens wordt een externe Azure PowerShell-sessie starten met behulp van deze opdrachten.

    $uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
    $creds = Get-Credential
    Enter-PSSession -ConnectionUri $uri -Credential $creds

Na het invoeren van geldige referenties ziet er ongeveer uitzien als de volgende Azure PowerShell-prompt:

    [cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>

Het eerste deel van deze vraag is de naam van uw cloud-service met het doel VM, die kan afwijken van de 'cloudservice4testing.cloudapp.net'. Azure PowerShell-opdrachten voor deze service cloud onderzoeken de problemen vermeld en corrigeren van de configuratie kunt u nu uitgeven.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>De TCP-poort voor luisteren naar extern bureaublad-Services handmatig corrigeren

Als u niet de [Azure IaaS (Windows) diagnostics-pakket](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) voor het probleem van de **RDP-verbinding met een VM in Azure (opnieuw opstarten vereist)** , op de Azure PowerShell-prompt voor externe sessie wordt uitgevoerd, moet u deze opdracht uitvoert.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

De eigenschap PortNumber geeft het nummer van de huidige poort. Wijzig indien nodig het externe bureaublad poort nummer terug op de standaardwaarde (3389) met behulp van deze opdracht.

    Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389

Controleer of dat de poort naar 3389 met behulp van deze opdracht is gewijzigd.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

De externe Azure PowerShell-sessie af te sluiten met behulp van deze opdracht.

    Exit-PSSession

Controleer of het eindpunt van extern bureaublad voor Azure VM is ook TCP-poort 3398 als de interne poort. De Azure VM opnieuw en probeer opnieuw de verbinding met extern bureaublad.


## <a name="additional-resources"></a>Aanvullende bronnen

[Azure diagnostics-pakket IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)

[Het opnieuw instellen van een wachtwoord of de service Extern bureaublad voor Windows virtuele machines](virtual-machines-windows-reset-rdp.md)

[Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)

[Oplossen van problemen met verbindingen met een Linux-gebaseerde Azure virtuele machine Secure Shell (SSH)](virtual-machines-linux-troubleshoot-ssh-connection.md)

[Problemen met toegang tot een toepassing wordt uitgevoerd op een Azure virtuele machine](virtual-machines-linux-troubleshoot-app-connection.md)
