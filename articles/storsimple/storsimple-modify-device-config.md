<properties 
   pageTitle="Wijzigen van de apparaatconfiguratie StorSimple | Microsoft Azure" 
   description="Beschrijving van het gebruik van de StorSimple Manager-service te configureren een StorSimple apparaat dat al is geïmplementeerd." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="09/29/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>De StorSimple Manager-service gebruiken voor het wijzigen van de apparaatconfiguratie StorSimple

## <a name="overview"></a>Overzicht 

De klassieke Azure portal pagina **configureren** bevat alle apparaatparameters die u kunt configureren op een StorSimple apparaat dat wordt beheerd door een StorSimple Manager-service. In deze zelfstudie wordt uitgelegd hoe u kunt de pagina **configureren** het volgende apparaat niveau taken uitvoeren:

- Apparaatinstellingen wijzigen 
- Tijdinstellingen wijzigen 
- DNS-instellingen wijzigen 
- Netwerkinterfaces wijzigen
- Verwisselen of IP-adressen toewijzen

## <a name="modify-device-settings"></a>Apparaatinstellingen wijzigen

De instellingen omvatten de beschrijvende naam van het apparaat en de beschrijving van het apparaat.

Een StorSimple-apparaat dat is verbonden met de service Manager StorSimple wordt een standaardnaam toegewezen. De standaardnaam wordt normaal gesproken het serienummer van het apparaat. Apparaat met een standaardnaam die is 15 tekens, zoals 8600-SHX0991003G44HT geeft bijvoorbeeld aan het volgende:

- **8600** – geeft aan dat het model.
- **SHX** -geeft aan dat de productie-inrichting.
- **0991003** - geeft aan dat een bepaald product.
- **G44HT**- de laatste 5 cijfers worden opgehoogd als unieke serienummers maken. Een set sequentieel niet mogelijk.

De klassieke Azure portal kunt u de naam van het apparaat wijzigen en hieraan een unieke beschrijvende naam van uw keuze. De beschrijvende naam kan kan willekeurige tekens bevatten en maximaal 64 tekens lang zijn.

U kunt ook een apparaatbeschrijving opgeven. Een beschrijving van het apparaat identificeert meestal de eigenaar en de fysieke locatie van het apparaat. Het omschrijvingsveld moet minder dan 256 tekens bevatten.
 
## <a name="modify-time-settings"></a>Tijdinstellingen wijzigen

Het apparaat moet tijd synchroniseren om te verifiëren met uw serviceprovider voor cloud-opslag. Selecteer uw tijdzone in de vervolgkeuzelijst en maximaal twee Network Time Protocol (NTP)-servers opgeven. De primaire NTP-server is vereist en wanneer u Windows PowerShell voor StorSimple gebruikt voor het configureren van het apparaat is opgegeven. U kunt de standaard Windows Server **time.windows.com** opgeven als de NTP-server. Kunt u de configuratie van de primaire NTP server via de klassieke Azure portal weergeven, maar moet u de interface van Windows PowerShell gebruiken om het te wijzigen.

De configuratie van de secundaire NTP-server is optioneel. De klassieke portal kunt u een secundaire NTP-server configureren. 

Bij het configureren van de NTP-server, moet u ervoor zorgen dat uw netwerk kan het NTP-verkeer voor het doorsturen van uw datacenter met het Internet. Wanneer u een openbare NTP-server opgeeft, moet u ervoor zorgen dat uw netwerkfirewalls en andere security-apparaten zijn geconfigureerd voor NTP-verkeer worden verzonden naar en van het externe netwerk. Als bidirectionele NTP-verkeer is niet toegestaan, moet u een interne NTP-server (een Windows-domeincontroller biedt deze functie). Als uw apparaat geen tijd synchroniseren kan, het niet mogelijk om te communiceren met uw provider cloud opslag.

Voor een overzicht van openbare NTP-servers, gaat u naar het [Web voor NTP-Servers](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Wat gebeurt er als het apparaat is geïmplementeerd in een andere tijdzone?

Als het apparaat is geïmplementeerd in een andere tijdzone, wordt de tijdzone van het apparaat wijzigen. Gezien het feit dat de back-up beleid de tijdzone van het apparaat gebruikt, wordt automatisch de back-up beleid overeenkomstig de nieuwe tijdzone aangepast. Er is geen tussenkomst van de gebruiker vereist.

## <a name="modify-dns-settings"></a>DNS-instellingen wijzigen

Een DNS-server wordt gebruikt wanneer uw apparaat probeert te communiceren met uw serviceprovider voor cloud-opslag. U moet zowel de primaire als de secundaire DNS-servers tijdens de implementatie van het eerste apparaat configureren voor maximale beschikbaarheid. Als het primaire DNS-server configureren, moet u de Windows PowerShell-interface gebruiken op uw apparaat StorSimple.

Als u wilt wijzigen van de secundaire DNS-server, kunt u de klassieke Azure portal.



## <a name="modify-network-interfaces"></a>Netwerkinterfaces wijzigen

Het apparaat heeft zes apparaat netwerkinterfaces, waarvan vier 1 GbE zijn en waarvan er twee 10 GbE zijn. Deze interfaces worden aangeduid als 0 – 5 gegevens gegevens. 1 GbE, zijn 0, 1 gegevens, gegevens 4 en 5 gegevens gegevens gegevens 2 en 3 van de gegevens 10 GbE-netwerkinterfaces zijn.

**Netwerk Interface-instellingen** configureren voor elk van de interfaces worden gebruikt. Hoge beschikbaarheid, zodat wordt aangeraden dat er minstens twee iSCSI-interfaces en twee interfaces wolk is ingeschakeld op het apparaat. We raden aan, maar hoeft geen ongebruikte interfaces uitgeschakeld.

Wanneer u een van de netwerkinterfaces configureert, moet u een virtuele IP-(VIP).

DATA 0 is cloud-standaard ingeschakeld. Bij het configureren van DATA 0, moet u ook twee vaste IP-adressen, één voor elke domeincontroller configureren. Deze vaste IP-adressen kunnen worden gebruikt voor rechtstreekse toegang tot de apparaatcontrollers en zijn handig wanneer u updates installeert op het apparaat of wanneer u de domeincontrollers voor het oplossen van problemen.

In Update 1 voor StorSimple 8000-serie, de routeringsmetric gegevens 0 is ingesteld op de laagste; dus als uw apparaat met StorSimple 8000-serie Update 1, worden al het verkeer via de cloud gerouteerd via DATA 0. Noteer dit als er meer dan één cloud-netwerk-interface op uw apparaat StorSimple.

>[AZURE.NOTE] Het vaste IP-adressen voor de controller worden gebruikt voor het onderhoud van de updates voor het apparaat. Het vaste IP-adressen moet dus routeerbaar en verbinding kunnen maken met het Internet.

De volgende parameters worden weergegeven voor elke netwerkinterface:

- **Snelheid** – geen parameter gebruiker worden geconfigureerd. DATA 0, gegevens 1, gegevens 4 en 5 van de gegevens zijn altijd 1 GbE, gegevens 2 en 3 van de gegevens 10 GbE-interfaces zijn.

     >[AZURE.NOTE] Snelheid en duplex wordt altijd auto-onderhandeld. Jumbo-frames worden niet ondersteund.
 
- **Interfacestatus** – een interface kan worden ingeschakeld of uitgeschakeld. Als ingeschakeld, probeert het apparaat te gebruiken van de interface. Het is raadzaam dat alleen die interfaces die zijn verbonden met het netwerk en gebruikt worden ingeschakeld. Alle interfaces die u niet gebruikt uit te schakelen.

- **Type interface** : deze parameter kunt u iSCSI-verkeer van cloud opslagverkeer te isoleren. Deze parameter kan een van de volgende zijn:

    - **Wolk ingeschakeld** – wanneer ingeschakeld, het apparaat wordt deze interface gebruiken om te communiceren met de cloud.
    - **iSCSI ingeschakeld** – wanneer ingeschakeld, het apparaat wordt deze interface gebruiken om te communiceren met de iSCSI-host.

    Wij raden aan iSCSI-verkeer van cloud opslagverkeer te isoleren. Let ook op dat als de host zich in hetzelfde subnet als het apparaat, hoeft u niet voor het toewijzen van een gateway; echter, als de host zich in een ander subnet bevindt dan uw apparaat, u moet voor het toewijzen van een gateway.

- **IP-adres** : dit is IPv4 of IPv6 of beide. Families van de IPv4- en IPv6-adres worden van de netwerkinterfaces van het apparaat ondersteund. Als u IPv4 gebruikt, geeft u een 32-bits IP-adres (*xxx.xxx.xxx.xxx*) in punt decimale notatie met punten. Wanneer u IPv6 gebruikt, simpelweg een 4-cijferige voorvoegsel en een 128-bits adres wordt automatisch gegenereerd voor de netwerkinterface van uw apparaat op basis van dat voorvoegsel.

- **Subnet** – dit verwijst naar het subnetmasker en via de Windows PowerShell-interface is geconfigureerd.

- **Gateway** : dit is de standaard-gateway die door deze interface moet worden gebruikt wanneer er wordt geprobeerd te communiceren met de knooppunten die zich niet in dezelfde IP-adresruimte (subnet). De standaard-gateway moet zich in dezelfde adresruimte (subnet) als de interface IP-adres, zoals bepaald door het subnetmasker.

- **Vast IP-adres** : dit veld is alleen beschikbaar als u de gegevens 0 interface. Voor bewerkingen, zoals updates of problemen met het apparaat, moet u rechtstreeks verbinding maken met de apparaatbesturing. Het vaste IP-adres kan worden gebruikt voor toegang tot zowel de actieve als de passieve controller op uw apparaat.

U kunt Controller 0 en 1 Controller opnieuw configureren via de klassieke Azure portal.

>[AZURE.NOTE] 
>
>- Teneinde de goede werking controleren van de interfacesnelheid en duplex op de switch die op elke apparaatinterface is aangesloten. Switch-interfaces moet ofwel onderhandelen met of worden geconfigureerd voor Gigabit Ethernet (1000 Mbps) en full-duplex. Interfaces op lagere snelheden of in half duplex leidt tot problemen met de prestaties.
>
>- Om onderbrekingen en downtime beperken, is het raadzaam portfast op elk van de poorten die de iSCSI-netwerk-interface van uw apparaat verbinding te maken in te schakelen. Zo ziet dat de netwerkverbinding van een failover snel kan worden opgericht.
 
## <a name="swap-or-reassign-ips"></a>Verwisselen of IP-adressen toewijzen

Op dit moment als elke netwerkinterface op de controller is een VIP die in gebruik is toegewezen (door hetzelfde apparaat of een ander apparaat in het netwerk), failover vervolgens de controller. Daarom moet u de juiste procedure volgen als u bij het wisselen van VIP's voor de netwerkinterface apparaat omdat u een dubbele IP-situatie.

Voer de volgende stappen uit om te wisselen of de VIP's voor elk van de netwerkinterfaces toewijzen:

#### <a name="to-reassign-ips"></a>IP-adressen toewijzen

1. Schakel het IP-adres voor beide interfaces.

2. Na het OT adressen is ingeschakeld, wordt de nieuwe IP-adressen toewijzen aan de bijbehorende interfaces.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [configureren van MPIO voor uw apparaat StorSimple](storsimple-configure-mpio-windows-server.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
     
