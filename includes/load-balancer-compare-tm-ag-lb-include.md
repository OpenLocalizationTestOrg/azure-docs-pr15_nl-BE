## <a name="load-balancer-differences"></a>Load Balancer verschillen

Er zijn verschillende opties voor het distribueren van netwerkverkeer met Microsoft Azure. Deze opties werken anders, met een verschillende functie instellen en verschillende scenario's. Kunnen ze elk in een geïsoleerde omgeving worden gebruikt of een combinatie van deze.

- **Azure-taakverdeling** werkt bij de transport-laag (laag 4 in de OSI-netwerkstack verwijzing). Deze biedt verdeling van het verkeer op netwerkniveau verschillende exemplaren van een toepassing wordt uitgevoerd in dezelfde Azure Datacenter.

- **Toepassingsgateway** werkt op de application-laag (laag 7 in de OSI-netwerkstack verwijzing). Het fungeert als een reverse proxy-service, beëindigt de clientverbinding en doorsturen van aanvragen naar de back-end-eindpunten.

- **Verkeer Manager** werkt op het niveau van DNS.  DNS-antwoorden wordt gebruikt om verkeer van de eindgebruiker naar globaal verdeelde eindpunten. Clients vervolgens verbinding maken met deze eindpunten rechtstreeks.

De volgende tabel bevat een overzicht van de functies van elke service:

| Service | Azure Load Balancer | Application Gateway | Beheer van netwerkverkeer |
|---|---|---|---|
|Technologie| Transportniveau (laag 4) | Toepassingsniveau (laag 7) | DNS-niveau |
| Ondersteunde toepassingsprotocollen | Alle | HTTP en HTTPS |  Alle (een HTTP-eindpunt is vereist voor het controleren van het eindpunt) |
| Eindpunten | Azure VMs en Cloud Services-rol exemplaren | Alle Azure interne IP-adres of de openbare internet-IP-adres | VMs Azure, Cloud Services, Azure Web Apps en externe eindpunten |
| Ondersteuning voor Vnet | Kan worden gebruikt voor beide toepassingen Internet gerichte en interne (Vnet) | Kan worden gebruikt voor beide toepassingen Internet gerichte en interne (Vnet) |    Biedt alleen ondersteuning voor internettoepassingen |
Controle eindpunt | Ondersteund via sondes | Ondersteund via sondes | Ondersteund via HTTP/HTTPS-GET | 

Azure taakverdeling en toepassingsgateway Routenetwerk verkeer aan eindpunten maar hebben andere gebruiksscenario's voor welke verkeer te verwerken. De volgende tabel kunt het verschil tussen de twee Netwerktaakverdeling:

| Type | Azure Load Balancer | Application Gateway |
|---|---|---|
| Protocollen | UDP EN TCP- | HTTP / HTTPS |
| IP-reservering | Ondersteund | Niet ondersteund | 
| Load balancing-modus | 5-tuple(source IP, source port, destination IP, destination port, protocol type) | Round Robin<br>Routering op basis van URL | 
| Modus voor taakverdeling (bron-IP / sticky sessies) |  2-tupel (IP-bron en doel-IP-), 3-tupel (bron-IP, doel-IP- en poort). Kan geschaald omhoog of omlaag op basis van het aantal virtuele machines | Op basis van een cookie affiniteit<br>Routering op basis van URL |
| Gezondheid sondes | Standaard: sonde interval - 15 sec. Uit de queryrotatie gehaald: 2 doorlopende storingen. Ondersteunt door de gebruiker gedefinieerde sondes | Niet-actieve sonde interval 30 sec. Die na 5 opeenvolgende live verkeer storingen of het uitvallen van één sonde in niet-actieve modus. Ondersteunt door de gebruiker gedefinieerde sondes | 
| SSL-offloading | Niet ondersteund | Ondersteund | 
  