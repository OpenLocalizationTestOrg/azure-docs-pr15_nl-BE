<properties
    pageTitle="Overname in Recovery Site | Microsoft Azure" 
    description="Azure-Site-Recovery coördineert de replicatie, failover en herstel van virtuele machines en fysieke servers. Meer informatie over failover naar Azure of een secundaire datacenter." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="failover-in-site-recovery"></a>Overname in de Site herstellen

De Azure-Site-Recovery-service draagt bij aan de continuïteit en noodherstel recovery (BCDR) bedrijfsstrategie door regie van replicatie, failover en herstel van virtuele machines en fysieke servers. Machines kunnen worden gerepliceerd naar Azure, of naar een secundaire op ruimten Datacenter. Voor een snel overzicht lezen [Wat is Azure Site terugzetten?](site-recovery-overview.md)

## <a name="overview"></a>Overzicht

Dit artikel bevat informatie over en instructies voor het herstellen (bij gebreke daarvan weer over en bij gebreke daarvan) virtuele machines en fysieke servers die zijn beveiligd met het herstel van de Site. 

Opmerkingen of vragen aan de onderkant van dit artikel, of op het [Forum van Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)boeken.


## <a name="types-of-failover"></a>Soorten failover

Wanneer beveiliging is ingeschakeld voor virtuele machines en fysieke servers en ze wilt repliceren kunt u failover uitvoeren als nodig is. Herstel van de site ondersteunt een aantal soorten van failover.

**Failover** | **Wanneer moet worden uitgevoerd** | **Details** | **Proces**
---|---|---|---
**Test-failover** | Uitvoeren om uw replicatiestrategie voor te valideren of uitvoeren van een detailanalyse disaster recovery | Geen verlies van gegevens of uitval.<br/><br/>Geen invloed op de replicatie<br/><br/>Geen invloed op uw productieomgeving. | Start de failover-functie<br/><br/>Opgeven hoe test computers worden verbonden met netwerken na een failover<br/><br/>Klik op het tabblad **taken** de voortgang bijhouden. Test machines zijn gemaakt en op de secundaire locatie<br/><br/>Azure - verbinding maken met de computer in de portal voor Azure<br/><br/>Secundaire site - toegang tot het systeem op de host en de cloud<br/><br/>Uitvoeren van testen en automatisch opschonen van failover-instellingen testen.
**Geplande failover** | Uitvoeren om te voldoen aan vereisten<br/><br/>Voor gepland onderhoud uitvoeren<br/><br/>Uitvoeren van failover uitgevoerd voor bekende storingen - zoals een stroomstoring verwachte of ernstige weerberichten werkbelasting behouden<br/><br/>Failback na een failover uitvoert vanaf een primaire naar een secundaire | Geen verlies van gegevens<br/><br/>Uitvaltijd is gedurende de tijd die nodig is voor het afsluiten van de virtuele machine op de primaire en brengen op de tweede locatie ontstaan.<br/><br/>Virtuele machines zijn invloed als doelcomputers bron machines wordt na een failover. | Start de failover-functie<br/><br/>Klik op het tabblad **taken** de voortgang bijhouden. Bron machines zijn afgesloten<br/><br/>Replica machines starten op secundaire locatie<br/><br/>Azure - verbinding maken met de computer replica in Azure portal<br/><br/>Secundaire site - toegang tot het systeem op dezelfde host en in de cloud dezelfde<br/><br/>Verbindt zich ertoe de failover-functie
**Niet-geplande failover** | Dit type van failover uitvoert op een reactieve manier wanneer een primaire site ontoegankelijk zijn na een onverwachte incident, zoals een stroomstoring of virusaanval aanval voor energiebeheer <br/><br/> U kunt uitvoeren als een niet-geplande failover kan worden uitgevoerd, zelfs als de primaire site is niet beschikbaar. | Afhankelijk van de frequentie-replicatie-instellingen voor het verlies van gegevens<br/><br/>Gegevens worden dan eveneens bijgewerkt volgens de laatste keer dat deze is gesynchroniseerd. | Start de failover-functie<br/><br/>Klik op het tabblad **taken** de voortgang bijhouden. Probeer het eventueel afsluiten van virtuele machines en synchroniseren van de meest recente gegevens<br/><br/>Replica machines starten op secundaire locatie<br/><br/>Azure - verbinding maken met de computer replica in Azure portal<br/><br/>Secundaire site toegang tot het systeem op dezelfde host en in de cloud dezelfde<br/><br/>Verbindt zich ertoe de failover-functie


De soorten failovers die worden ondersteund, is afhankelijk van het implementatiescenario.

**Failover-richting** | **Test-failover** | **Geplande failover** | **Niet-geplande failover**
---|---|---|---
Primaire VMM-site naar site van secundaire VMM | Ondersteund | Ondersteund | Ondersteund 
Secundaire VMM site naar site primaire VMM | Ondersteund | Ondersteund | Ondersteund
Wolk-wolk (één VMM server) |  Ondersteund | Ondersteund | Ondersteund
VMM Azure-site | Ondersteund | Ondersteund | Ondersteund 
Azure site VMM | Niet-ondersteunde | Ondersteund | Niet-ondersteunde 
Azure-site Hyper-V | Ondersteund | Ondersteund | Ondersteund
Azure met Hyper-V-site | Niet-ondersteunde | Ondersteund | Niet-ondersteunde
VMware Azure-site | Ondersteund (uitgebreide scenario)<br/><br/> Niet-ondersteunde (oude scenario) |Niet-ondersteunde | Ondersteund
Azure fysieke server | Ondersteund (uitgebreide scenario)<br/><br/> Niet-ondersteunde (oude scenario) | Niet-ondersteunde | Ondersteund

## <a name="failover-and-failback"></a>Failover en failback

U kunt niet via virtuele machines naar een site secundaire in ruimten of Azure, afhankelijk van uw implementatie. Een machine die wordt overgenomen door Azure is gemaakt als een Azure virtuele machine. U kunt niet via een enkele virtuele machine of fysieke server of een herstelplan. De herstelplannen bestaat uit een of meer groepen die beveiligde virtuele machines of fysieke servers bevatten besteld. Deze worden gebruikt voor de evoluerende failover van meerdere machines die volgens de groep failover ze zich op. [Lees meer](site-recovery-create-recovery-plans.md) over de herstelplannen. 

Houd rekening met nadat failover is voltooid en uw machines uitgevoerd en in een tweede locatie worden:

- Als u niet Azure, nadat de failover-computers niet beveiligd of repliceren op de primaire of secundaire locatie. De virtuele machines worden in Azure opgeslagen in geo-gerepliceerde opslag biedt flexibele, maar niet voor replicatie.
- Als u een niet-geplande failover naar een secundaire site nadat de failover-machines in de secundaire locatie niet beveiligd of repliceren.
- Als u een geplande failover naar een secundaire site nadat de failover-machines in de tweede locatie is beveiligd.
 

### <a name="failback-from-secondary-site"></a>Failback van secundaire site

Failback van een secundaire site aan een primaire gebruikt hetzelfde proces als failover van de primaire naar een secundaire. Nadat de failover-functie van primaire aan secundaire datacenter gepleegd en volledig is, kunt u reverse-replicatie starten wanneer de primaire site beschikbaar. Replicatie tussen de primaire en de secundaire site start reverse replicatie door de delta-gegevens te synchroniseren. Reverse replicatie brengt de virtuele machines in de protected-modus, maar de secundaire datacenter is nog steeds de actieve locatie. Om de primaire site naar de locatie van de actieve start u een geplande failover van primaire, secundaire gevolgd door een andere omgekeerde replicatie.

### <a name="failback-from-azure"></a>Failback van Azure

Als u hebt overgenomen door Azure worden uw virtuele machines worden beschermd door de tolerantiefuncties van Azure voor virtuele machines. Als u de oorspronkelijke primaire site in de actieve locatie kunt u een geplande failover uitvoeren. U kunt niet op de oorspronkelijke locatie of naar een andere locatie als uw oorspronkelijke site niet beschikbaar is. Om te repliceren na failback naar primaire locatie opnieuw starten start u een omgekeerde replicatie.

### <a name="failover-considerations"></a>Failover-overwegingen

- **IP-adres na een failover**-standaard een mislukte via machine heeft een ander IP-adres dan de broncomputer. Als u bewaren de hetzelfde IP-adres Zie wilt: 
    - **Secundaire site**, als u nog niet via een secundaire site en u wilt bewaren van een IP-adres [lezen](http://blogs.technet.com/b/scvmm/archive/2014/04/04/retaining-ip-address-after-failover-using-hyper-v-recovery-manager.aspx) in dit artikel. Opmerking kunt u een openbaar IP-adres bewaren als uw Internet-provider wordt ondersteund.
    - **Azure**— als u bent niet via Azure kunt u het IP-adres dat u wilt toewijzen in het tabblad **configureren** van de eigenschappen van de virtuele machine. U kunt een openbaar IP-adres kan niet behouden na een failover naar Azure. U kunt RFC 1918 adresruimten die worden gebruikt als interne adressen worden behouden.
- **Gedeeltelijke failover**— als u een failover deel uitmaakt van een site wilt maken in plaats van een hele site Houd er rekening mee dat: 
    - **Secundaire site**, als u niet over het deel van een primaire naar een secundaire site-site en u wilt de primaire site verbinding, een VPN-verbinding van site naar site via verbinding met mislukte toepassingen op de secundaire site uitvoeren op de site van de primaire onderdelen van de infrastructuur. Als een hele subnet overgenomen behoudt u het IP-adres van virtuele machine. Als u niet over een gedeeltelijke subnet kunt u het IP-adres van virtuele machine kan niet behouden omdat subnetten tussen sites kunnen niet worden gesplitst.
    - **Azure**— als u een gedeeltelijke Azure-site een failover en de primaire site verbinding wilt maken, kunt u een site naar site VPN via een toepassing in Azure infrastructuuronderdelen uitgevoerd op de primaire site een kan geen verbinding maken. Houd er rekening mee dat als de hele subnet mislukt als u het IP-adres van virtuele machine kunnen bewaren. Als u niet over een gedeeltelijke subnet kunt u het IP-adres van virtuele machine kan niet behouden omdat subnetten tussen sites kunnen niet worden gesplitst.
 
- **Stationsaanduiding**: als u wilt behouden de stationsletter op virtuele machines na een failover stelt u de SAN-beleid voor de virtuele machine op **op**. Virtuele machine schijven on automatisch line worden gebracht. [Lees meer](https://technet.microsoft.com/library/gg252636.aspx).
- **Route clientaanvragen**— Recovery Site werkt met Azure verkeer Manager route van clientaanvragen naar uw toepassing na een failover.




## <a name="run-a-test-failover"></a>Een failover test uitvoeren

Tijdens het uitvoeren van een test failover wordt u gevraagd naar netwerkinstellingen selecteren voor test replica machines. U hebt een aantal opties.  

**Test de failover-optie** | **Beschrijving** | **Failover-selectievakje** | **Details**
---|---|---|---
**Failover uitvoeren naar Azure — zonder netwerk** | Selecteert u een doel Azure netwerk | Failover-cheques die virtuele machine testen start zoals verwacht in Azure | Alle test virtuele machines in een plan herstellen in een enkele cloud-service worden toegevoegd en met elkaar kunt verbinden<br/><br/>Machines zijn niet verbonden met een netwerk Azure na een failover.<br/><br/>Gebruikers kunnen verbinding met de test-machines met een openbaar IP-adres
**Failover uitvoeren naar Azure — met netwerk** | Selecteer een doel Azure netwerk | Failover wordt gecontroleerd dat test machines zijn aangesloten op het netwerk | Maak een Azure netwerk dat geïsoleerd is van het productienetwerk Azure en instellen van de infrastructuur voor de virtuele machine met gerepliceerde werken zoals verwacht.<br/><br/>Het subnet van de virtuele machine van test is gebaseerd op het subnet waarop de mislukte via virtuele machine moet verbinding maken met het geval van een failover of niet gepland.
**Failover uitgevoerd naar een secundaire site van VMM — zonder netwerk** | Selecteer een netwerk voor VM niet | Failover wordt gecontroleerd dat machines test worden gemaakt.<br/><br/>De test virtuele machine wordt gemaakt op dezelfde host als de host waarop de replica virtuele machine bestaat. Het is niet toegevoegd aan de wolk waarin de replica virtuele machine bevindt. | <p>De mislukte via de computer niet wordt aangesloten op een netwerk.<br/><br/>De computer kan worden verbonden met een netwerk voor VM nadat deze is gemaakt
**Een failover uitvoeren naar een secundaire VMM-site, met een netwerk** | Selecteer een bestaande VM netwerk een | Failover wordt gecontroleerd dat de virtuele machines zijn gemaakt | De test virtuele machine wordt gemaakt op dezelfde host als de host waarop de replica virtuele machine bestaat. Het is niet toegevoegd aan de wolk waarin de replica virtuele machine bevindt.<br/><br/>Een VM-netwerk dat is geïsoleerd van het productienetwerk maken<br/><br/>Als u een VLAN-netwerk het beste maakt u een afzonderlijk logisch netwerk (niet gebruikt in de productie) in VMM voor dit doel. Deze logisch netwerk wordt gebruikt voor VM-netwerken voor het failover-test maken.<br/><br/>Logische netwerken moet worden gekoppeld aan ten minste een van de netwerkadapters van alle Hyper-V-servers die als host voor virtuele machines.<br/><br/>Logische netwerken VLAN, de netwerksites aan de logische netwerk toevoegen worden geïsoleerd.<br/><br/>Als u een logische Windows netwerk virtualisatie – netwerk, maakt Azure Site herstel automatisch geïsoleerd VM-netwerken.
**Een failover uitvoeren naar een secundaire VMM-site, een netwerk maken** | Een tijdelijke proef-netwerk wordt gemaakt op basis van de instellingen die u in de **Logische netwerken** en de netwerk-gerelateerde sites opgeeft automatisch | Failover wordt gecontroleerd dat de virtuele machines zijn gemaakt | Gebruik deze optie als u meer dan een VM-netwerk wordt gebruikt voor het herstelplan. Als u Windows netwerk virtualisatie, netwerken, kan deze optie automatisch VM netwerken met dezelfde instellingen (de subnetten en IP-adresgroepen) maken in het netwerk van de replica virtuele machine. Deze netwerken VM worden automatisch opgeschoond nadat de test failover uitgevoerd is.</p><p>De test virtuele machine wordt gemaakt op dezelfde host als de host waarop de replica virtuele machine bestaat. Het is niet toegevoegd aan de wolk waarin de replica virtuele machine bevindt.

>[AZURE.NOTE] Het IP-adres aan een virtuele machine tijdens failover test doet hetzelfde als het IP-adres dat het wanneer ontvangen zou een geplande of ongeplande failover (ervan uitgaande dat het IP-adres beschikbaar in het netwerk van de failover-test is. Als hetzelfde IP-adres is niet beschikbaar in het netwerk van de failover-test wordt virtuele machine een ander IP-adres dat beschikbaar is in het netwerk van de failover-test.



### <a name="run-a-test-failover-from-on-premises-to-azure"></a>Een failover testen uitgevoerd op ruimten naar Azure

Deze procedure wordt beschreven hoe u een failover testen voor een herstelplan. U kunt ook de failover-functie voor een enkele computer uitvoeren op het tabblad **virtuele Machines** .

1. Selecteer **Recovery plan** > *recoveryplan_name*. Klik op **Failover** > **Test Failover**.
2. Geef op de pagina **Bevestigen Test Failover** hoe replica machines worden verbonden met een netwerk Azure na een failover.
3. Als u nog niet Azure en gegevenscodering is ingeschakeld voor de cloud, selecteer het certificaat dat is afgegeven bij het inschakelen van codering van gegevens tijdens de installatie van de Provider in **Coderingssleutel** . 
4. Failover-voortgang volgen op het tabblad **taken** . U moet mogelijk de testmachine replica in de portal Azure Zie.
5. Toegankelijk machines in Azure replica van de site op het bedrijf starten een RDP-verbinding met de virtuele machine. Open op het eindpunt voor de virtuele machine moet poort 3389.
5. Zodra u klaar bent, als de failover-functie bereikt de fase **voltooid testen** , klikt u op **Volledige Test** te voltooien.
5. In **notities** vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de overname van de test.
8. Klik op **de test failover is uitgevoerd** voor het automatisch opschonen van de testomgeving. Nadat dit voltooid is wordt failover test de**omplete** C status weergegeven.

> [AZURE.NOTE] Als een test-failover gedurende meer dan twee weken die wordt opgesteld door force blijft. Alle elementen of virtuele machines automatisch gemaakt tijdens de overname van de test worden verwijderd.
  

### <a name="run-a-test-failover-from-a-primary-on-premises-site-to-a-secondary-on-premises-site"></a>Een test failover uitvoeren vanaf een primaire op gebouwen-site naar een secundaire op gebouwen-site

U moet een aantal dingen voor het uitvoeren van een test failover, met inbegrip van een kopie van de domeincontroller en test DHCP en DNS-servers in uw testomgeving te plaatsen. U kunt dit op twee manieren doen:

- Als u testen met behulp van een bestaand netwerk failover uitvoeren wilt, voorbereiden van Active Directory, DHCP en DNS in dat netwerk.
- Als u automatisch VM netwerken maken met behulp van de optie test failover uitvoeren wilt, handmatige stap invoegen voor groep 1 in het herstelplan dat u wilt gebruiken voor de overname van de test en vervolgens de infrastructuur resources toevoegen aan het netwerk automatisch gemaakte voordat de test failover wordt uitgevoerd.

#### <a name="things-to-note"></a>Dingen om te onthouden

- Bij het repliceren van een secundaire site, het type netwerk dat wordt gebruikt door de machine replica hoeft niet overeenkomt met het type van logische netwerk gebruikt voor failover-test, maar sommige combinaties werken mogelijk niet. Als de replica wordt gebruikt voor DHCP en isolatie op basis van VLAN, hoeft de VM-netwerk voor de replica niet een statische IP-adresgroep. Met behulp van virtualisatie voor Windows-netwerk voor de overname van de test is niet goed omdat er geen adresgroepen beschikbaar zijn. Daarnaast werkt test failover niet als het netwerk van de replica geen isolatie is en het testnetwerk Windows netwerk virtualisatie is. Dit is omdat het netwerk geen isolatie niet de subnetten is vereist voor het maken van een netwerk met Windows netwerk virtualisatie.
- De manier waarop in de replica welke virtuele machines zijn verbonden met toegewezen VM netwerken nadat failover is afhankelijk van hoe de VM-netwerk is geconfigureerd in de console VMM:
    - **VM-netwerk geconfigureerd zonder isolatie of VLAN isolatie**, als DHCP is gedefinieerd voor het VM-netwerk, wordt de replica virtuele machine verbonden met de VLAN-ID met de instellingen die zijn opgegeven voor de netwerksite in het bijbehorende logische netwerk. De virtuele machine wordt het IP-adres ontvangt van de beschikbare DHCP-server. U hoeft niet een statische IP-adresgroep die is gedefinieerd voor het doelnetwerk VM. Als u een statische IP-adresgroep wordt gebruikt voor het VM-netwerk worden de replica virtuele machine verbonden met de VLAN-ID met de instellingen die zijn opgegeven voor de netwerksite in het bijbehorende logische netwerk. De virtuele machine ontvangt het IP-adres van de groep is gedefinieerd voor het VM-netwerk. Als u een statische IP-adresgroep niet is gedefinieerd in het doelnetwerk VM, toewijzing van IP-adressen niet. De IP-adresgroep, moet worden gemaakt op de bron- en doel VMM-servers die u wilt gebruiken voor bescherming en herstel.
    - **VM-netwerk met een Windows-netwerk virtualisatie**— als een VM-netwerk is geconfigureerd met deze instelling een statische adresgroep moet worden gedefinieerd voor het VM netwerk, ongeacht of de bron VM netwerk is geconfigureerd om te gebruiken DHCP of een statische IP-groep met adressen. Als u DHCP definieert, wordt de doelserver VMM fungeren als een DHCP-server en een IP-adres uit de groep die is gedefinieerd voor het doel VM netwerk bieden. Als gebruik van een statische IP-adresgroep voor de bronserver is gedefinieerd, wordt de doelserver VMM een IP-adres toewijzen uit de groep. In beide gevallen toewijzing van IP-adressen niet als een statische IP-adresgroep niet is gedefinieerd.

#### <a name="run-test"></a>Test uitvoeren

Deze procedure wordt beschreven hoe u een failover testen voor een herstelplan. U kunt ook voor een enkele virtuele machine of fysieke server failover uitvoeren op het tabblad **virtuele Machines** .

1. Selecteer **Recovery plan** > *recoveryplan_name*. Klik op **Failover** > **Test Failover**.
2. Geef op de pagina **Bevestigen testen Failover** hoe virtuele machines moet worden aangesloten netwerken na de overname van de test.
3. Failover-voortgang volgen op het tabblad **taken** . Als de failover-functie de** Complete test** fase bereikt, klikt u op **Volledige Test** van de overname van de test is voltooid.
4. Klik op **notities** als u wilt opnemen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de overname van de test.
4. Controleer of de virtuele machines met succes gestart nadat deze voltooid is.
5. Na controle van virtuele machines is gestart, voltooid failover test voor het opschonen van de geïsoleerde omgeving. Als u kiest voor het automatisch maken van netwerken van VM, Schijfopruiming verwijdert alle test virtuele machines en netwerken te testen.

> [AZURE.NOTE] Als een test-failover gedurende meer dan twee weken die wordt opgesteld door force blijft. Alle elementen of virtuele machines automatisch gemaakt tijdens de overname van de test worden verwijderd.


#### <a name="prepare-dhcp"></a>Voorbereiden van DHCP

Als de virtuele machines die betrokken zijn bij test failover, DHCP gebruiken, een test DHCP-server moet worden gemaakt binnen het geïsoleerde netwerk die is gemaakt voor de test failover.


### <a name="prepare-active-directory"></a>Active Directory voorbereiden
Een test failover voor de toepassing testen uitgevoerd, moet u een kopie van de productieomgeving van Active Directory in de testomgeving. Ga via de sectie [test de failover-overwegingen voor active directory](site-recovery-active-directory.md#considerations-for-test-failover) voor meer informatie. 


### <a name="prepare-dns"></a>DNS voorbereiden

Een DNS-server voor de overname van de test als volgt voorbereiden:

- **DHCP**-als virtuele machines met DHCP, het IP-adres van de DNS-test moet worden bijgewerkt op de test DHCP-server. Als u een netwerktype virtualisatie voor Windows-netwerk gebruikt, worden de VMM-server fungeert als de DHCP-server. Daarom moet het IP-adres van DNS worden bijgewerkt in het netwerk van de failover-test. In dit geval wordt de virtuele machines geregistreerd op de relevante DNS-Server.
- **Statisch adres**, als virtuele machines met een statisch IP-adres, het IP-adres van de DNS-testserver in test failover-netwerk moet worden bijgewerkt. Mogelijk moet u DNS bijwerken met het IP-adres van de virtuele machines voor testen. U kunt hiervoor het volgende voorbeeldscript gebruiken: 

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-planned-failover-primary-to-secondary"></a>Uitvoeren van een geplande failover (primair naar secundair)

 Deze procedure wordt beschreven hoe u voor het uitvoeren van een geplande failover voor een herstelplan. U kunt ook voor een enkele virtuele machine failover uitvoeren op het tabblad **virtuele Machines** .

1. Zorg voordat u begint met alle virtuele machines die u wilt een failover de eerste replicatie is voltooid.
2. Selecteer **Recovery plan** > *recoveryplan_name*. Klik op **Failover** > **geplande Failover**. 
3. Kies de bron- en doellocaties op de pagina **Bevestigen geplande Failover **. Opmerking de failover-richting.

    - Als vorige failovers werkten zoals verwacht en alle virtuele machine servers bevinden zich op de locatie van de bron- of doel, worden de failover-richting details zijn alleen ter informatie. 
    - Als de virtuele machines zijn actief op zowel de bron- en doellocaties, verschijnt de knop **Richting wijzigen** . Met deze knop kunt wijzigen en geeft u de richting waarin de failover moet worden uitgevoerd.

5. Als u nog niet Azure en gegevenscodering is ingeschakeld voor de cloud, selecteer het certificaat dat is afgegeven bij het inschakelen van codering van gegevens tijdens de installatie op de server VMM Provider in **Coderingssleutel** . 
6. Wanneer een geplande failover begint is de eerste stap voor het afsluiten van de virtuele machines, zodat er geen gegevens verloren gaan. Klik op het tabblad **taken** kunt u de failover-voortgang volgen. Als er een fout optreedt in de failover-functie (hetzij op een virtuele machine in een script dat is opgenomen in het herstelplan), stopt de geplande overname van een herstelplan. U kunt de overname opnieuw starten.
8. Nadat de replica virtuele machines worden gemaakt die ze in een commit in afwachting van de staat zijn. Klik op **vastleggen** vast te leggen van de failover. 
9. Als de replicatie is voltooid de virtuele machines op de secundaire locatie te starten. 

## <a name="run-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren

Deze procedure wordt beschreven hoe u een niet-geplande failover voor een herstelplan. U kunt ook voor een enkele virtuele machine of fysieke server failover uitvoeren op het tabblad **virtuele Machines** .

1. Selecteer **Recovery plan** > *recoveryplan_name*. Klik op **Failover** > **niet-geplande Failover**. 
3. Kies de bron- en doellocaties op de pagina **Bevestigen niet-geplande Failover **. Opmerking de failover-richting.

    - Als vorige failovers werkten zoals verwacht en alle virtuele machine servers bevinden zich op de locatie van de bron- of doel, worden de failover-richting details zijn alleen ter informatie. 
    - Als de virtuele machines zijn actief op zowel de bron- en doellocaties, verschijnt de knop **Richting wijzigen** . Met deze knop kunt wijzigen en geeft u de richting waarin de failover moet worden uitgevoerd.

4. Als u nog niet Azure en gegevenscodering is ingeschakeld voor de cloud, selecteer het certificaat dat is afgegeven bij het inschakelen van codering van gegevens tijdens de installatie op de server VMM Provider in **Coderingssleutel** . 
5. Selecteer **afsluiten van virtuele machines en synchroniseert u de meest recente gegevens** op te geven dat herstel Site proberen moet de beschermde virtuele machines afsluiten en de gegevens synchroniseren zodat de meest recente versie van de gegevens overgenomen. Als u deze optie niet selecteert of als de poging niet succesvol worden de failover-functie van de meest recente beschikbare herstelpunt voor de virtuele machine.
6. Klik op het tabblad **taken** kunt u de failover-voortgang volgen. Houd er rekening mee dat zelfs als er fouten optreden tijdens een niet-geplande failover, het herstelplan wordt uitgevoerd totdat deze voltooid is.
7. Na de overname zijn de virtuele machines in een **wachtende commit** staat. Klik op **vastleggen** vast te leggen van de failover.
8. Als u replicatie ingesteld op meerdere punten van de herstelconsole gebruiken, in het herstelpunt wijzigen kunt u gebruik van een herstelpunt dat is niet de meest recente (nieuwste standaard wordt gebruikt). Nadat u extra doorvoeren worden herstel punten verwijderd.
9. Nadat de replicatie is voltooid wordt de virtuele machines worden gestart en op de tweede locatie worden uitgevoerd. Ze zijn echter niet beveiligd of repliceren. Als de primaire site opnieuw met dezelfde onderliggende infrastructuur beschikbaar is, klikt u op **Reverse repliceren** om te beginnen met reverse-replicatie. Dit zorgt ervoor dat de virtuele machine opnieuw klaar voor failover is en dat alle gegevens worden gerepliceerd naar de primaire site. Replicatie ongedaan maken nadat u een niet-geplande failover maakt een belasting van de overdracht van gegevens. De overdracht gebruikt dezelfde methode die is geconfigureerd voor de eerste replicatie-instellingen voor de cloud.

## <a name="failback-from-secondary-to-primary"></a>Failback van primaire secundaire

 Na een failover van de primaire naar een secundaire locatie gerepliceerde virtuele machines worden niet beveiligd door sites worden hersteld en de secundaire locatie nu fungeert als de primaire. Volg deze procedures niet terug naar de oorspronkelijke primaire site. Deze procedure wordt beschreven hoe u voor het uitvoeren van een geplande failover voor een herstelplan. U kunt ook voor een enkele virtuele machine failover uitvoeren op het tabblad **virtuele Machines** .

1. Selecteer **Recovery plan** > *recoveryplan_name*. Klik op **Failover** > **geplande Failover**.
2. Kies de bron- en doellocaties op de pagina **Bevestigen geplande Failover **. Opmerking de failover-richting. De overname van primaire gewerkt als verwacht als alle virtuele machines worden in het secundaire locatie, dat dit alleen ter informatie is.
3. Als u bent bij gebreke van Azure instellingen te selecteren in **Synchronisatie van gegevens**:

    - **Synchroniseren van gegevens voordat failover wordt uitgevoerd (alleen synchroniseren deltawijzigingen)**— deze optie wordt de uitvaltijd voor virtuele machines beperkt zoals het synchroniseren van deze zonder af te sluiten. Dit doet het volgende:
        - Fase 1: Momentopname van de virtuele machine wordt in Azure en gekopieerd naar de host van de Hyper-V op gebouwen. De machine wordt verder uitgevoerd in Azure.
        - Fase 2: Hiermee sluit u de virtuele machine in Azure zodat er geen nieuwe wijzigingen worden uitgevoerd. De laatste reeks wijzigingen naar de server in de ruimten worden overgebracht en de op het bedrijf virtuele machine wordt gestart.
    

    - **Synchronize gegevens tijdens een overname alleen (volledige download)**— deze optie gebruiken als u hebt uitgevoerd op Azure voor een lange tijd. Deze optie is sneller omdat we verwachten dat de meeste van de schijf is gewijzigd en we willen niet opzien in checksum berekening. Downloaden van de schijf wordt uitgevoerd. Het is ook handig wanneer de virtuele machine van op prem is verwijderd.
    
    > [AZURE.NOTE] Aangeraden dat u deze optie gebruiken als u hebt is met Azure een tijdje (een maand of langer) of de VM op prem is verwijderd. Deze optie wordt niet controlesom berekeningen uit te voeren.
    
5. Als u nog niet Azure en gegevenscodering is ingeschakeld voor de cloud, selecteer het certificaat dat is afgegeven bij het inschakelen van codering van gegevens tijdens de installatie op de server VMM Provider in **Coderingssleutel** . 
5. Het laatste herstelpunt wordt standaard gebruikt, maar in **Het herstelpunt wijzigen** kunt u een ander herstelpunt. 
6. Klik op het vinkje om te beginnen de failback-bewerking.  Klik op het tabblad **taken** kunt u de failover-voortgang volgen. 
7. f die u hebt geselecteerd op de optie voor het synchroniseren van de gegevens vóór de failover, nadat de initiële gegevens synchroniseren voltooid is en u kunt nu de virtuele machines in Azure, afgesloten **taken** Klik op >  <planned failover job name> **Complete Failover**. Dit de Azure machine wordt afgesloten, brengt de meest recente wijzigingen op ruimten virtual machine en opstart.
8. U kunt nu het logboek naar de virtuele machine te valideren is beschikbaar als verwacht. 
9. De virtuele machine wordt een commit in afwachting van de staat. Klik op **vastleggen** vast te leggen van de failover.
10. Nu voltooien de failback-bewerking op de **Omgekeerde repliceren** Start de virtuele machine in de primaire site te beschermen.



## <a name="failback-to-an-alternate-location"></a>Failback naar een andere locatie

Als u hebt geïmplementeerd beveiliging tussen een [site met Hyper-V en Azure](site-recovery-hyper-v-site-to-azure.md) u failback van Azure te kunnen moet een alternatief op-lokalen locatie. Dit is handig als u nodig hebt voor het instellen van nieuwe hardware voor op gebouwen. Hier ziet u hoe u dit doet.

1. Als u nieuwe hardware installeert Windows Server 2012 R2 en de functie Hyper-V op de server.
2. Een virtueel netwerkswitch maken met dezelfde naam die op de oorspronkelijke server.
3. Selecteer **Beveiligde Items** -> **Bescherming groep**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> gewenste failback-bewerking en selecteer **Geplande Failover**.
4. Selecteer in de **Geplande Failover bevestigen** **maken op ruimten virtuele machine als deze nog niet bestaat**. 
5. Selecteer de nieuwe Hyper-V host-server waarop u wilt plaatsen van de virtuele machine in de **Host-naam** .
6. In synchronisatie van gegevens is het raadzaam dat de optie **synchroniseren van de gegevens voordat u de failover-functie**. Dit verkleint de uitvaltijd voor virtuele machines zoals het synchroniseren van deze zonder af te sluiten. Dit doet het volgende:

    - Fase 1: Momentopname van de virtuele machine wordt in Azure en gekopieerd naar de host van de Hyper-V op gebouwen. De machine wordt verder uitgevoerd in Azure.
    - Fase 2: Hiermee sluit u de virtuele machine in Azure zodat er geen nieuwe wijzigingen worden uitgevoerd. De laatste reeks wijzigingen naar de server in de ruimten worden overgebracht en de op het bedrijf virtuele machine wordt gestart.
    
7. Klik op het vinkje om te beginnen met de overname (failback).
8. Nadat de initiële synchronisatie is voltooid en u kunt nu de virtuele machine in Azure afsluiten, klikt u op **taken** > <planned failover job> > **Complete Failover**. Dit de Azure machine wordt afgesloten, brengt de meest recente wijzigingen virtual machine op gebouwen en opstart.
9. U kunt zich aanmelden op de virtuele machine van op het bedrijf om te controleren of dat alles werkt zoals verwacht. Klik vervolgens op **doorvoeren** om te voltooien de failover-functie.
10. Klik op **Reverse repliceren** om te beginnen met het beschermen van de virtuele machine van op gebouwen.

    >[AZURE.NOTE] Als u de failback-taak annuleert terwijl het in stap synchronisatie van gegevens, worden de VM op gebouwen in een beschadigd staat. Dit komt omdat Data synchronisatie exemplaren de meest recente gegevens van Azure VM schijven om de gegevens op prem schijven en kun meer totdat de synchronisatie is voltooid, wordt de schijf gegevens zijn mogelijk niet in een consistente status. Als de On-prem VM wordt opgestart nadat de synchronisatie van gegevens wordt geannuleerd, kan het niet start. Failover om de synchronisatie van gegevens opnieuw worden geactiveerd.
 
