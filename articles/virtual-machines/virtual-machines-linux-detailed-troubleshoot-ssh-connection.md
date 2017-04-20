<properties
    pageTitle="Gedetailleerde SSH het oplossen van problemen voor een Azure VM | Microsoft Azure"
    description="Meer gedetailleerde SSH probleemoplossing voor problemen met verbinding maken met een Azure virtuele machine"
    keywords="SSH verbinding geweigerd, ssh fout, azure ssh, SSH-verbinding is mislukt"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="detailed-ssh-troubleshooting-steps"></a>Gedetailleerde stappen voor SSH

Er zijn veel mogelijke redenen die de SSH-client mogelijk niet kunnen bereiken van de SSH-service op de VM. Als u de meer [algemene SSH stappen](virtual-machines-linux-troubleshoot-ssh-connection.md)hebt gevolgd, moet u het probleem verder oplossen. Dit artikel begeleidt u door gedetailleerde stappen om te bepalen waar de SSH-verbinding niet goed werkt en hoe u deze kunt oplossen.

## <a name="take-preliminary-steps"></a>Voorbereidende stappen nemen

In het volgende diagram ziet u de onderdelen die betrokken zijn.

![Diagram waarin onderdelen van SSH-service](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

De volgende stappen kunnen u de bron van de fout te isoleren en berekenen van oplossingen of oplossingen.

Raadpleeg eerst de status van de VM in de portal.

In de [Azure portal](https://portal.azure.com):

1. Selecteer **Bladeren**voor VMs gemaakt met behulp van het implementatiemodel klassiek, > **virtuele machines (klassiek)** > *naam VM*.

    -OF-

    Selecteer **Bladeren**voor VMs gemaakt met behulp van het model Resource Manager, > **virtuele machines** > *naam VM*.

    Het statusvenster voor VM moet **uitgevoerd**worden weergegeven. Ga naar de recente activiteit weergeven voor compute-, opslag- en netwerkbronnen.

2. Selecteer **Instellingen** te onderzoeken van eindpunten, IP-adressen en andere instellingen.

    Controleer of een [beveiligingsgroep netwerk](../virtual-network/virtual-networks-nsg.md) is gedefinieerd voor eindpunten in VMs die zijn gemaakt met behulp van bronbeheer. Controleer ook of dat de regels aan de beveiligingsgroep netwerk zijn toegepast en dat zij in het subnet verwezen bent.

In de [Azure klassieke portal](https://manage.windowsazure.com)voor VMs die zijn gemaakt met behulp van het implementatiemodel klassiek:

1. Selecteer de **virtuele machines** > *naam VM*.
2. Selecteren van de VM **Dashboard** om de status ervan controleren.
3. Selecteer **Monitor** voor een overzicht van recente activiteiten voor compute-, opslag- en netwerkbronnen.
4. Selecteer de **eindpunten** om ervoor te zorgen dat er een eindpunt voor SSH verkeer is.

Om te controleren of verbinding met het netwerk, controleert u de geconfigureerde eindpunten en Zie als u de VM via een ander protocol, zoals HTTP- of een andere service kan bereiken.

Na deze stap, de SSH-verbinding opnieuw te proberen.


## <a name="find-the-source-of-the-issue"></a>De bron van het probleem vinden

De SSH-client op uw computer mogelijk niet de SSH-service op de Azure VM als gevolg van problemen of configuraties van de volgende bereiken:

- [SSH-clientcomputer](#source-1-ssh-client-computer)
- [Organisatie-edge-apparaat](#source-2-organization-edge-device)
- [Cloud-service-eindpunt en toegangsbeheerlijsten (ACL's)](#source-3-cloud-service-endpoint-and-acl)
- [Netwerk-beveiligingsgroepen](#source-4-network-security-groups)
- [Linux-gebaseerde Azure VM](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>1 bron: SSH client-computer

Als u wilt verwijderen van uw computer als de bron van de fout, controleert u of die het SSH-verbindingen aan een andere op-, Linux-gebaseerde computer kunt aanbrengen.

![Diagram waarmee SSH client computeronderdelen](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Als de verbinding mislukt, controleert u het volgende op uw computer:

- Een lokale firewall die blokkeert inkomende of uitgaande SSH verkeer (TCP-22)
- Proxy-clientsoftware waardoor SSH verbindingen lokaal zijn geïnstalleerd
- Network monitoring software waardoor SSH verbindingen lokaal zijn geïnstalleerd
- Andere soorten beveiligingssoftware om verkeer te controleren of bepaalde typen verkeer toestaan/weigeren

Als een van deze voorwaarden van toepassing, tijdelijk de software uit te schakelen en probeer een SSH-verbinding met een computer op het bedrijf om te zien wat de reden dat de verbinding wordt geblokkeerd op de computer. Werk met uw netwerkbeheerder om SSH-verbindingen wilt toestaan de software-instellingen corrigeren.

Als u certificaatverificatie gebruikt, controleert u of u deze machtigingen hebt om de .ssh map in de basismap:

- Type chmod 700 ~/.ssh
- Type chmod 644 ~/.ssh/\*pub
- Type chmod 600 ~/.ssh/id_rsa (of andere bestanden met de persoonlijke sleutels die zijn opgeslagen in deze)
- Type chmod 644 ~/.ssh/known_hosts (bevat hosts waarmee u via SSH verbonden bent)

## <a name="source-2-organization-edge-device"></a>Bron 2: Organisatie edge-apparaat

Als bepaalde edge-apparaat van uw organisatie als de bron van de fout, controleert u of dat een computer die rechtstreeks verbonden met het Internet verbindingen met SSH aan uw Azure VM maken kunt. Als u de VM via een VPN website of een verbinding Azure ExpressRoute benaderen wilt, gaat u naar [bron 4: beveiligingsgroepen netwerk](#nsg).

![Diagram waarin organisatie edge-apparaat wordt gemarkeerd.](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Als er geen een computer die rechtstreeks is verbonden met het Internet, een nieuwe Azure VM in een eigen bronnengroep maakt of cloud service en gebruiken. Zie [virtuele machines maken waarop Linux in Azure](virtual-machines-linux-quick-create-cli.md)voor meer informatie. De resourcegroep of VM en cloud-service verwijderen als u met het testen bent klaar.

Als u een SSH-verbinding met een computer die rechtstreeks verbonden met Internet maken kunt, controleert u-edge-apparaat voor uw organisatie:

- Een interne firewall die SSH verkeer met het Internet blokkeert
- Een proxyserver waardoor SSH verbindingen
- Inbraakdetectie of network monitoring software die wordt uitgevoerd op de apparaten in uw netwerk rand waardoor SSH verbindingen

Werken met de netwerkbeheerder om de instellingen van uw organisatie edge-apparaten voor SSH verkeer met het Internet te corrigeren.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Bron 3: Cloud-service-eindpunt en ACL

> [AZURE.NOTE] Deze bron is alleen geldig voor VMs die zijn gemaakt met behulp van het implementatiemodel klassiek. VMs die zijn gemaakt met behulp van bronbeheer, gaat u naar [4 bron: beveiligingsgroepen netwerk](#nsg).

Om de cloud-service-eindpunt en ACL elimineren als de bron van de fout, controleert u of dat een andere Azure VM in hetzelfde virtuele netwerk SSH-verbindingen voor uw VM maken kunt.

![Diagram waarmee cloud-service-eindpunt en ACL](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Als u geen andere VM in hetzelfde virtuele netwerk hebt, kunt u eenvoudig een nieuwe maken. Zie voor meer informatie, [maken een Linux VM in Azure met behulp van de CLI](virtual-machines-linux-quick-create-cli.md). Verwijder de extra VM wanneer u klaar bent met het testen.

U kunt een SSH-verbinding maken met een VM in hetzelfde virtuele netwerk, controleert u het volgende:

- **De endpoint-configuratie voor SSH verkeer op het doel VM.** De particuliere TCP-poort van het eindpunt moet overeenkomen met de TCP-poort waarop de SSH-service op de VM luistert. (De standaardpoort is 22). VMs gemaakt met behulp van het implementatiemodel Resource Manager, Controleer of voor de SSH TCP-poortnummer in de portal Azure **Bladeren**door > **virtuele machines (v2)** > *naam VM* > **Instellingen** > **eindpunten**.

- **De ACL voor het eindpunt SSH verkeer op de virtuele machine van doel.** Een ACL kunt u toegestaan of geweigerd binnenkomend verkeer vanaf het Internet, op basis van het bron-IP-adres. Onjuist geconfigureerde ACL's kunnen voorkomen dat binnenkomend SSH verkeer naar het eindpunt. Controleer uw ACL's om ervoor te zorgen dat binnenkomend verkeer vanuit het openbare IP-adressen van uw proxyserver of andere edge-server is toegestaan. Zie voor meer informatie [over network access control list (ACL's)](../virtual-network/virtual-networks-acl.md).

Het eindpunt als een bron van het probleem, verwijder het huidige eindpunt, maak een nieuwe eindpunt en geeft u de naam SSH (TCP poort 22 voor het nummer van de openbare en particuliere). Zie de [eindpunten van een virtuele machine in Azure instellen](virtual-machines-windows-classic-setup-endpoints.md)voor meer informatie.

<a id="nsg"></a>
## <a name="source-4-network-security-groups"></a>Bron 4: Netwerk-beveiligingsgroepen

Netwerk-beveiligingsgroepen kunnen u meer gedetailleerde controle van de toegestane binnenkomend en uitgaand verkeer. U kunt regels maken die subnetten omvatten en cloud services in een virtueel netwerk van Azure. Controleer of uw netwerk groep regels om ervoor te zorgen dat SSH verkeer van en naar het Internet is toegestaan.
Zie voor meer informatie [over beveiligingsgroepen netwerk](../virtual-network/virtual-networks-nsg.md).

## <a name="source-5-linux-based-azure-virtual-machine"></a>Bron 5: Linux-gebaseerde Azure virtuele machine

De laatste bron van mogelijke problemen met is de Azure virtual machine zelf.

![Diagram dat Linux Azure virtuele machine wordt gemarkeerd](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Als u nog niet hebt gedaan, voert u de instructies [voor het opnieuw instellen van een wachtwoord of SSH voor Linux-gebaseerde virtuele machines](virtual-machines-linux-classic-reset-access.md).

Probeer opnieuw verbinding te maken van uw computer. Als het nog steeds niet zijn de volgende enkele van de mogelijke problemen:

- De SSH-service wordt niet uitgevoerd op de virtuele machine van doel.
- De SSH-service luistert niet op TCP-poort 22. U kunt dit testen een Telnet-client op de lokale computer installeren en uitvoeren "Telnet- *cloudServiceName*. cloudapp.net 22". Hiermee wordt bepaald als de virtuele machine kunt u binnenkomende en uitgaande communicatie naar het eindpunt SSH.
- De lokale firewall op de virtuele machine van doel heeft voorschriften die verhinderen binnenkomend of uitgaand verkeer voor SSH dat.
- Inbraakdetectie of network monitoring software die wordt uitgevoerd op de Azure virtual machine is de SSH verbindingen te blokkeren.


## <a name="additional-resources"></a>Aanvullende bronnen
Zie [problemen met toegang tot een toepassing wordt uitgevoerd op een Azure virtuele machine](virtual-machines-linux-troubleshoot-app-connection.md) voor meer informatie over het oplossen van toegang tot toepassingen