<properties
  pageTitle="Aangepaste sondes Balancer te laden en de controle op de gezondheidsstatus | Microsoft Azure"
  description="Informatie over het gebruik van aangepaste sondes voor Azure Load Balancer exemplaren achter de Load Balancer controleren"
  services="load-balancer"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="understand-load-balancer-probes"></a>Load balancer sondes begrijpen

Azure Load Balancer biedt de mogelijkheid om de gezondheid van de exemplaren van de server met behulp van sondes te controleren. Wanneer een sonde niet reageert, stopt Load Balancer verzenden van nieuwe verbindingen naar het beschadigde exemplaar. Bestaande verbindingen worden niet beïnvloed en nieuwe verbindingen worden verzonden naar gezonde exemplaren.

Een gast-agent cloud service rollen (werknemer rollen en functies van de webpagina) gebruiken voor het controleren van de sonde. Bij het gebruik van virtuele machines achter de Load Balancer worden TCP of HTTP aangepaste sondes geconfigureerd.

## <a name="understand-probe-count-and-timeout"></a>Aantal sonde en time-out begrijpen

Afhankelijk van het gedrag van de sonde:

- Het aantal geslaagde sondes waarmee een exemplaar wilt markeren als max.
- Het aantal mislukte sondes die ervoor zorgen een exemplaar dat wilt markeren als omlaag.

De waarde voor time-outs en frequentie in SuccessFailCount kunt u bepalen of een instantie worden uitgevoerd of niet actief is bevestigd. In de Azure portal is de time-out ingesteld op tweemaal de waarde van de frequentie.

De sonde configuratie van taakverdeling overal voor een eindpunt (dat wil zeggen een set verdeeld) moet hetzelfde zijn. Dit betekent dat een ander sonde configuratie voor elk exemplaar van de rol of een virtuele machine in dezelfde gehoste service voor een bepaald eindpunt combinatie kan er. Elke instantie moet bijvoorbeeld identieke lokale poorten en time-outs.

>[AZURE.IMPORTANT] Een sonde met taakverdeling gebruikt het IP-adres 168.63.129.16. Deze openbare IP-adres vergemakkelijkt de communicatie platform interne bronnen voor het inleveren-uw-eigen-IP-Azure virtueel netwerk. De virtuele openbare IP-adres 168.63.129.16 wordt gebruikt in alle regio's en wordt niet gewijzigd. We raden u aan dit IP-adres in een lokale firewall-beleid. Het moet niet worden overwogen een beveiligingsrisico omdat alleen het interne Azure-platform kan een bericht van dat adres van bron. Als u niet doet dit, zullen er onverwacht gedrag in verschillende scenario's zoals het configureren van hetzelfde IP-adresbereik van 168.63.129.16 en IP-adressen die worden gedupliceerd.

## <a name="learn-about-the-types-of-probes"></a>Meer informatie over de soorten sondes

### <a name="guest-agent-probe"></a>Gast agent sonde

Deze sonde is alleen beschikbaar voor Azure Cloud Services. Load Balancer wordt gebruikgemaakt van de agent van de gast binnen de virtuele machine, en vervolgens luistert en reageert met een HTTP-200 OK antwoord alleen wanneer het exemplaar in de toestand gereed is (dat wil zeggen, in een andere staat zoals bezet, Recycling of stoppen).

Zie voor meer informatie [het definitiebestand (csdef) configureren voor gezondheid sondes](https://msdn.microsoft.com/library/azure/ee758710.aspx) of het [maken van een internetverbinding taakverdeling voor cloud services aan de slag](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Wat maakt een sonde met gast-agent een exemplaar als beschadigd gemarkeerd?

Als de Gast-agent niet reageert met een HTTP-200 OK, is de taakverdeling markeert de sessie reageert en stopt met het verzenden van verkeer naar dat exemplaar. Verdeling van de belasting blijft de instantie de opdracht ping. Als de Gast-agent met een HTTP-200 reageert, verzendt de taakverdeling verkeer opnieuw naar die instantie.

Wanneer u een Webrol, de websitecode meestal uitgevoerd in w3wp.exe die niet wordt gecontroleerd door de Azure fabric of Gast agent. Dit betekent dat fouten in w3wp.exe (bijvoorbeeld HTTP 500 reacties) wordt niet gemeld aan de Gast-agent en de taakverdeling dat exemplaar van rotatie onderneemt.

### <a name="http-custom-probe"></a>Aangepaste HTTP-sonde

De aangepaste HTTP-Load Balancer sonde overschrijft de standaard Gast agent sonde, wat betekent dat u uw eigen aangepaste logica om te bepalen van de gezondheid van de rol-instantie kunt maken. De taakverdeling sondes uw eindpunt elke 15 seconden standaard. De instantie wordt beschouwd als in de taakverdelingsrotatie als deze met een HTTP-200 binnen de time-outperiode (31 seconden standaard reageert).

Dit is handig als u uw eigen logica wilt voor het verwijderen van exemplaren van taakverdelingsrotatie implementeren. U wilt bijvoorbeeld kan een exemplaar verwijderen als deze hoger dan 90% CPU is en geeft als de status van een niet-200 resultaat. Als u web-rollen die w3wp.exe wordt gebruikt, dit betekent ook dat u automatische controle van uw website, omdat de fouten in de websitecode van uw de load balancer sonde retourneert de status van een niet-200.

>[AZURE.NOTE] De aangepaste HTTP-sonde ondersteunt relatieve paden en HTTP-protocol. HTTPS wordt niet ondersteund.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Wat maakt een aangepaste HTTP-sonde een exemplaar als beschadigd gemarkeerd?

- De HTTP-aanvraag wordt een HTTP-antwoordcode dan 200 (bijvoorbeeld, 403, 404 of 500). Dit is een positieve bevestiging dat exemplaar van de toepassing dient te worden gehouden bij de service meteen.
- De HTTP-server reageert niet op alle na de time-outperiode. Afhankelijk van de time-outwaarde die is ingesteld, kan dit betekenen dat meerdere sonde gaan voordat de sonde wordt gemarkeerd als niet wordt uitgevoerd met openstaande aanvragen (dat wil zeggen voordat de SuccessFailCount sondes worden verzonden).
- De server sluit de verbinding via een TCP-reset.

### <a name="tcp-custom-probe"></a>Aangepaste sonde TCP

TCP-sondes initiëren een verbinding door middel van een drieweg-handshake met gedefinieerde poortregels.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Wat maakt een aangepaste sonde met TCP een exemplaar als beschadigd gemarkeerd?

- De TCP-server reageert niet op alle na de time-outperiode. Wanneer de sonde is gemarkeerd als niet wordt uitgevoerd, is afhankelijk van het aantal mislukte testverzoeken die zijn geconfigureerd voor onbeantwoorde gaan voordat u het markeert de sonde niet wordt uitgevoerd.
- De sonde ontvangt een TCP reset van het exemplaar van de rol.

Voor meer informatie over het configureren van een HTTP-gezondheid sonde of een sonde met TCP Zie [aan de slag maken van een internetverbinding-taakverdeling in Bronbeheer met PowerShell](load-balancer-get-started-internet-arm-ps.md#create-lb-rules-nat-rules-a-probe-and-a-load-balancer).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Gezonde exemplaren terug naar load balancer rotatie toevoegen

TCP- en HTTP-sondes worden beschouwd als gezond en markeert de instantie rol als gezonde wanneer:

- De taakverdeling haalt een positieve sonde voor het eerst de VM wordt opgestart.
- Het aantal SuccessFailCount (eerder beschreven) bepaalt de waarde van succesvolle sondes die nodig zijn om de rol van instantie als gezonde markeren. Als een exemplaar van de rol is verwijderd, moet het aantal geslaagde, opeenvolgende sondes gelijk is aan of groter is dan de waarde van SuccessFailCount aan de instantie van de rol als actief markeren.

>[AZURE.NOTE] Als de status van een exemplaar van de rol is schommelde, wacht de taakverdelingsvoorziening langer voordat u het exemplaar van de functie weer in orde zijn. Dit wordt gedaan via het beleid ter bescherming van de gebruiker en de infrastructuur.

## <a name="use-log-analytics-for-load-balancer"></a>Logboek analytics gebruiken voor taakverdeling

[Analytics voor Load Balancer logboek](load-balancer-monitor-log.md) kunt u controleren op de gezondheidstoestand van de sonde en het aantal van de sonde. Logboekregistratie kan met Power BI of operationele inzichten Azure worden gebruikt om statistische gegevens over de gezondheidstoestand van de Load Balancer.
