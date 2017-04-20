<properties
    pageTitle="Voordat u gaat Azure Stack Implementatiemodel implementeren | Microsoft Azure"
    description="Bekijk de milieu- en hardware-vereisten voor Azure Stack Implementatiemodel (de beheerder)."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="erikje"/>

# <a name="azure-stack-deployment-prerequisites"></a>Vereisten voor implementatie van Azure Stack

Voordat u Azure Stack Implementatiemodel ([Bewijs van Concept](azure-stack-poc.md)) implementeert, zorg ervoor dat uw computer voldoet aan de volgende eisen voldoen.
De technische Preview 2 eisen voor de Haalbaarheidstest zijn dezelfde als die welke vereist zijn voor technische Preview 1. Daarom kunt u dezelfde hardware die u voor het vorige voorbeeld van één vak gebruikt.

## <a name="hardware"></a>Hardware

| Onderdeel | Minimum  | Aanbevolen |
|---|---|---|
| Harde schijven: besturingssysteem | 1 OS schijf met minimaal 200 GB beschikbaar is voor de systeempartitie (SSD of HDD) | 1 OS schijf met minimaal 200 GB beschikbaar is voor de systeempartitie (SSD of HDD) |
| Harde schijven: algemene Azure Stack Implementatiemodel gegevens | 4 schijven. Elke schijf biedt minimaal 140 GB aan capaciteit (SSD of HDD). Alle beschikbare schijven worden gebruikt. | 4 schijven. Elke schijf biedt een minimum van 250 GB aan capaciteit (SSD of HDD). Alle beschikbare schijven worden gebruikt.|
| Berekenen: CPU | Dual Socket-: 12 fysieke Cores (totaal)  | Dual Socket-: 16 fysieke Cores (totaal) |
| Berekenen: geheugen | 96 GB RAM  | 128 GB RAM-GEHEUGEN |
| Berekenen: BIOS | Hyper-V ingeschakeld (met ondersteuning voor SLAT)  | Hyper-V ingeschakeld (met ondersteuning voor SLAT) |
| Netwerk: NIC | Windows Server 2012 R2 certificering vereist voor NIC; geen speciale functies die vereist zijn | Windows Server 2012 R2 certificering vereist voor NIC; geen speciale functies die vereist zijn |
| HW logocertificering | [Gecertificeerd voor Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Gecertificeerd voor Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

**Gegevens schijf configuratie:** Alle gegevensstations moeten van hetzelfde type (alle SAS of SATA alle) en capaciteit. Als SAS harde schijven worden gebruikt, moeten de schijven worden gekoppeld via een enkel pad (geen MPIO, ondersteuning voor meerdere paden wordt geleverd).

**HBA-configuratieopties**
 
- (Bij voorkeur) Eenvoudige HBA
- RAID HBA-Adapter moet worden geconfigureerd in de modus "passeren"
- HBA RAID-schijven moeten worden geconfigureerd als één schijf, RAID 0

**Ondersteunde bus en media type combinaties**

-   SATA HARDE SCHIJF

-   SAS HARDE SCHIJVEN

-   RAID HARDE SCHIJVEN

-   SSD RAID (als het mediatype niet opgegeven/onbekend is\*)

-   SATA SSD + SATA HARDE SCHIJF

-   SAS SSD + SAS HARDE SCHIJVEN

\*RAID-controllers zonder Pass Through-mogelijkheid het mediatype wordt niet herkend. Dergelijke domeincontrollers wordt zowel harde schijf en SSD markeren als niet opgegeven. In dat geval wordt de SSD-technologie gebruikt als permanente opslag in plaats van caching van apparaten. Daarom kunt u de Microsoft Azure Stack Implementatiemodel op deze SSD implementeren.

**Voorbeeld van de HBA's**: LSI 9207 8i, LSI-9300-8i of LSI 9265 8i in de modus pass-through

Monster OEM-configuraties zijn beschikbaar.

## <a name="operating-system"></a>Besturingssysteem

| | **Vereisten**  |
|---|---|
| **Versie van het besturingssysteem** | Windows Server 2012 R2 of later. De versie van het besturingssysteem is niet kritiek voordat de installatie wordt gestart, zoals u zult de host-computer opstarten in de VHD die opgenomen in de Stack Azure installatie zip. Het besturingssysteem en alle vereiste patches zijn al geïntegreerd in de afbeelding. Gebruik geen toetsen te activeren van Windows Server-exemplaren die worden gebruikt in het Implementatiemodel.|

## <a name="deployment-requirements-check-tool"></a>Implementatievereisten controleren hulpprogramma

Nadat het besturingssysteem is geïnstalleerd, kunt u de [Implementatie Checker voor Azure Stack technische Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) om te controleren of uw hardware voldoet aan alle eisen.



## <a name="microsoft-azure-active-directory-accounts"></a>Microsoft Azure Active Directory-accounts

De implementatie van Microsoft Azure Stack Implementatiemodel moet worden verbonden met Azure. Daarom moet u een account van Microsoft Azure Active Directory voorbereiden voordat u de implementatie van PowerShell script uitvoert. Deze rekening wordt het globale beheer voor de huurder Azure Active Directory. Deze kunt inrichten en het overdragen van toepassingen en service beveiligings-principals voor alle Stack Azure services die met Azure Active Directory en grafische API samenwerken. Ook wordt gebruikt als de eigenaar van de provider standaardabonnement (die u later kunt wijzigen). U kunt van uw systeem Azure Stack admin Portal aanmelden met deze account.

1. Maak een Azure AD-account die is de beheerder van de directory voor ten minste één Azure Active Directory. Als u al hebt, kunt u die gebruiken. Anders kunt u een gratis voor [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (in China, Ga naar <http://go.microsoft.com/fwlink/?LinkID=717821> in plaats daarvan.)

    Sla deze referenties voor gebruik in stap 6 van [de implementatie van PowerShell script uitvoert](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script). Deze account *service* kunt configureren en beheren van resource wolken, gebruikersaccounts, huurder plannen, quota en prijzen. In de portal kunnen ze website wolken, particuliere wolken virtuele machine maken, plannen maken en gebruikersabonnementen beheren.

2. [Maken](azure-stack-add-new-user-aad.md) , ten minste één account zodat u zich aanmelden bij de Haalbaarheidstest Azure Stack als een huurder.

  	| **Azure Active Directory-account**  | **Ondersteund?** |
  	|---|---| 
  	| Werk of school rekening met geldige openbare Azure-abonnement  | Ja |
  	| Microsoft-Account met geldige openbare Azure-abonnement  | Nee |
  	| Werk of school rekening met geldig abonnement voor China Azure  | Ja |
  	| Werk of school rekening met geldige US Government Azure-abonnement  | Ja |


## <a name="network"></a>Netwerk

### <a name="switch"></a>Switch

Een beschikbare poort op een schakeloptie voor de machine Implementatiemodel.  

De machine Azure Stack Implementatiemodel ondersteunt verbindingen met een switch-poort of trunk poort. Geen speciale functies zijn vereist op de switch. Als u een poort trunk gebruikt of als u een VLAN-ID configureren wilt, hebt u de VLAN-ID als parameter voor implementatie te bieden. Hier ziet u voorbeelden in de [lijst met parameters voor implementatie](azure-stack-run-powershell-script.md).

### <a name="subnet"></a>Subnet

Het Implementatiemodel computer geen verbinding met de volgende subnetten:
- 192.168.200.0/24
- 192.168.100.0/27
- 192.168.101.0/26
- 192.168.102.0/24
- 192.168.103.0/25
- 192.168.104.0/25

Deze subnetten zijn gereserveerd voor de interne netwerken binnen de stapel Implementatiemodel van Microsoft Azure-omgeving.

### <a name="ipv4ipv6"></a>IPv4/IPv6-

Alleen IPv4 wordt ondersteund. U kunt geen IPv6-netwerken maken.

### <a name="dhcp"></a>DHCP

Controleer of er een DHCP-server beschikbaar is op de Netwerkadapter die verbinding met maakt het netwerk. Als u DHCP niet beschikbaar is, moet u een extra statische IPv4-netwerk niet op de host alleen voorbereiden. U moet opgeven dat IP-adres en gateway als een parameter voor de implementatie. Hier ziet u voorbeelden in de [lijst met parameters voor implementatie](azure-stack-run-powershell-script.md).

### <a name="internet-access"></a>Toegang tot het Internet

Azure Stack vereist toegang tot het Internet, rechtstreeks of via een transparante proxy. Azure-Stack biedt geen ondersteuning voor de configuratie van een webproxy waarmee de toegang tot het Internet. Zowel de host en het nieuwe onderzoektijdvak toegewezen aan de MAS-BGPNAT01 (door DHCP- of statische IP) moet toegang tot Internet. Poorten 80 en 443 worden onder de domeinen graph.windows.net en login.windows.net gebruikt.

### <a name="telemetry"></a>Telemetrie

Ter ondersteuning van de gegevensstroom telemetrie is poort 443 (HTTPS) geopend in het netwerk. Het eindpunt van de client is https://vortex-win.data.microsoft.com.


## <a name="next-steps"></a>Volgende stappen

[Het implementatiepakket Azure Stack Implementatiemodel downloaden](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Azure Stack Haalbaarheidstest implementeren](azure-stack-run-powershell-script.md)
