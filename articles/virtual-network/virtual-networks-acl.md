<properties
   pageTitle="Wat is een Network Access Control-lijst (ACL)?"
   description="Meer informatie over toegangsbeheerlijsten"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-is-an-endpoint-access-control-list-acls"></a>Wat is een eindpunt toegangsbeheerlijst (ACL's)?

Een eindpunt lijst ACL (Access Control) is een beveiligingsverbetering van de voor uw Azure-implementatie. Een ACL biedt de mogelijkheid tot selectief toestaan of weigeren van verkeer voor het eindpunt van een virtuele machine. De filtervoorziening van dit pakket bevat een extra beveiligingslaag. U kunt netwerk ACL's voor alleen eindpunten opgeven. U kunt een ACL voor een virtueel netwerk of een specifiek subnet opgenomen in een virtueel netwerk niet opgeven.

> [AZURE.IMPORTANT] Het verdient aanbeveling gebruik van beveiligingsgroepen netwerk (NSGs) in plaats van ACL's zo veel mogelijk. Zie voor meer informatie over NSGs, [Wat is een netwerk-beveiligingsgroep?](virtual-networks-nsg.md).

ACL's kunnen worden geconfigureerd met PowerShell of de Portal beheren. Zie voor meer informatie over het configureren van een netwerk ACL via PowerShell [Beheren van toegangsbeheerlijsten (ACL's) voor eindpunten met PowerShell](virtual-networks-acl-powershell.md). Zie [instellen van eindpunten op een virtuele Machine](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md)voor informatie over het configureren van een netwerk ACL via de Portal Management.

Met behulp van ACL's netwerk, kunt u het volgende doen:

- Selectief toestaan of weigeren van inkomend verkeer op basis van het externe subnet IPv4-adresbereik met een virtuele machine invoer eindpunt.

- Zwarte lijst IP-adressen

- Meerdere regels per eindpunt van de virtuele machine maken

- Geef maximaal 50 ACL regels per virtuele machine eindpunt

- Gebruik regel bestellen om ervoor te zorgen de juiste set regels worden toegepast op een bepaalde virtuele machine eindpunt (laag naar hoog)

- Een ACL voor een specifiek extern subnet IPv4-adres opgeven.

## <a name="how-acls-work"></a>De werking van ACL 's

Een ACL is een object dat een lijst met regels bevat. Wanneer u een ACL maken en op een eindpunt van de virtuele machine toepassen, pakketfiltering van plaatsvindt op de hostknooppunt van de VM. Dit betekent dat het verkeer van het externe IP-adressen worden gefilterd op de hostknooppunt voor ACL toewijzingsregels in plaats van op uw VM. Hiermee voorkomt u dat uw VM de kostbare CPU-cycli uitgaven voor het filteren van pakketten.

Wanneer een virtuele machine wordt gemaakt, wordt een standaard ACL geroepen al het inkomende verkeer te blokkeren. Echter, indien een eindpunt is gemaakt voor (poort 3389), vervolgens de standaard ACL wordt gewijzigd als u wilt dat al het binnenkomende verkeer voor dat eindpunt. Inkomend verkeer uit een extern subnet dat eindpunt vervolgens mag en er geen firewall wordt ingericht is vereist. Alle andere poorten voor binnenkomend verkeer geblokkeerd tenzij eindpunten voor deze poorten zijn gemaakt. Uitgaand verkeer wordt standaard toegestaan.

**Voorbeeld van de standaard ACL-tabel**

| **Regel #** | **Extern Subnet** | **Eindpunt** | **Toestaan/weigeren** |
|--------|---------------|----------|-------------|
| 100    | 0.0.0.0/0     | 3389     | Vergunning      |

## <a name="permit-and-deny"></a>Toestaan of weigeren

U kunt selectief toestaan of weigeren voor een virtuele machine invoer eindpunt verkeer door regels die opgeeft "vergunning" of "weigeren" te maken. Het is belangrijk te weten dat standaard als een eindpunt wordt gemaakt, alle verkeer is toegestaan naar het eindpunt. Daarom is het belangrijk dat u begrijpt hoe regels toestaan/weigeren maken en in de juiste volgorde plaatsen als u gedetailleerde controle over het netwerkverkeer dat u wilt toestaan dat het eindpunt van de virtuele machine te bereiken.

Punten te overwegen:

1. **Geen ACL:** Standaard bij het maken van een eindpunt is toegestaan voor het eindpunt.

1. **Toestaan-** Als u een of meer "vergunning" bereiken toevoegt, weigert u standaard alle andere bereiken. Alleen de pakketten van het toegestane bereik van IP-is het mogelijk om te communiceren met het eindpunt van de virtuele machine.

1. **Weigeren-** Als u een of meer "weigeren" bereiken toevoegt, verleent u standaard andere bereiken van verkeer.

1. **De combinatie van toestaan en weigeren-** U kunt een combinatie van "vergunning" en "deny" Als u wilt kunnen fungeren van een bepaald IP-bereik worden toegestaan of geweigerd.

## <a name="rules-and-rule-precedence"></a>Regels en de prioriteit van regels

Netwerk ACL's kunnen worden ingesteld op de eindpunten van de specifieke virtuele machine. U kunt bijvoorbeeld een ACL-netwerk voor een RDP-eindpunt dat is gemaakt op een virtuele machine welke vergrendelingen omlaag toegang voor bepaalde IP-adressen opgeven. De onderstaande tabel ziet u een manier om toegang te verlenen tot openbare virtuele IP-adressen (VIP's) van een bepaald bereik om toegang te verlenen voor RDP. Alle andere externe IP-adressen worden geweigerd. We houden een *laagste prioriteit* regelvolgorde.

### <a name="multiple-rules"></a>Meerdere regels

In het volgende voorbeeld als u toegang wilt geven tot het eindpunt RDP slechts uit twee openbare IPv4-adresbereiken (65.0.0.0/8 en 159.0.0.0/8), kunt dit u bereiken door te geven van twee regels *staan* . In dit geval sinds RDP is standaard voor een virtuele machine hebt gemaakt, kunt u vergrendelen toegang tot de RDP-poort op basis van een extern subnet. In het volgende voorbeeld ziet u een manier om toegang te verlenen tot openbare virtuele IP-adressen (VIP's) van een bepaald bereik om toegang te verlenen voor RDP. Alle andere externe IP-adressen worden geweigerd. Dit werkt omdat de ACL's netwerk kunt instellen voor een specifieke virtuele machine eindpunt en standaard de toegang is geweigerd.

**Voorbeeld: meerdere regels**

| **Regel #** | **Extern Subnet** | **Eindpunt** | **Toestaan/weigeren** |
|--------|---------------|----------|-------------|
| 100    | 65.0.0.0/8    | 3389     | Vergunning      |
| 200    | 159.0.0.0/8   | 3389     | Vergunning      |

### <a name="rule-order"></a>Regel order

Omdat er meerdere regels voor een eindpunt kunnen worden opgegeven, moet er een manier voor het indelen van regels om te bepalen welke regels voorrang. Volgorde van de regels geeft voorrang aan. ACL's netwerk volgen een *laagste prioriteit* regelvolgorde. In het volgende voorbeeld wordt het eindpunt op poort 80 selectief toegang tot alleen bepaalde IP-adresbereiken verleend. Om dit te configureren, hebben we een regel voor weigeren (regel \# 100) voor adressen in de 175.1.0.1/24 ruimte. Een tweede regel wordt vervolgens opgegeven met prioriteit 200 dat toegang tot alle adressen onder 175.0.0.0/8 biedt.

**Voorbeeld: de prioriteit van regels**

| **Regel #** | **Extern Subnet** | **Eindpunt** | **Toestaan/weigeren** |
|--------|---------------|----------|-------------|
| 100    | 175.1.0.1/24  | 80       | Weigeren        |
| 200    | 175.0.0.0/8   | 80       | Vergunning      |

## <a name="network-acls-and-load-balanced-sets"></a>Via ACL's en evenwichtige sets laden

Netwerk ACL's kunnen worden opgegeven op een eindpunt taakverdeling instellen (LB ingesteld). Als een ACL voor een LB instellen is opgegeven, wordt de ACL netwerk toegepast op alle virtuele Machines in die LB instellen. Bijvoorbeeld als een LB instellen met 'Poort 80' wordt gemaakt en de LB-Set 3 VMs bevat, de ACL netwerk gemaakt op eindpunt 'Poort 80' van een die VM automatisch op de andere VMs toegepast wordt.

![Via ACL's en evenwichtige sets laden](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Volgende stappen

[Toegangsbeheerlijsten (ACL's) voor eindpunten via PowerShell beheren](virtual-networks-acl-powershell.md)
