Resource|Gratis|Gedeelde (voorbeeld)|Basic|Standaard|Premium (voorbeeld)</th>
---|---|---|---|---|---
[Web, mobiele of API apps](https://azure.microsoft.com/services/app-service/) per [App serviceplan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup>|10|100|Onbeperkt<sup>2</sup>|Onbeperkt<sup>2</sup>|Onbeperkt<sup>2</sup>
[Logica apps](https://azure.microsoft.com/services/app-service/logic/) per [App serviceplan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup>|10|10|10|20 per core|20 per core
[App-serviceplan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)|1 per regio|10 per resourcegroep|100 per resourcegroep|100 per resourcegroep|100 per resourcegroep
Instantietype berekenen|Gedeeld|Gedeeld|Speciale<sup>3</sup>|Speciale<sup>3</sup>|Speciale<sup>3</sup></p>
[Scale-Out](../articles/app-service-web/web-sites-scale.md) (maximum aantal exemplaren)|1 gedeeld|1 gedeeld|3 speciale<sup>3</sup>|10 speciale<sup>3</sup>|20 (50 in ASE) toegewezen<sup>3,4</sup>
Opslag<sup>5</sup>|1 GB<sup>5</sup>|1 GB<sup>5</sup>|10 GB<sup>5</sup>|50 GB<sup>5</sup>|500 GB<sup>4,5</sup></p>
CPU tijd (korte)<sup>6</sup>|3 minuten|3 minuten|Onbeperkt, betalen bij standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a>|Onbeperkt, betalen bij standaardtarieven|Onbeperkt, betalen bij standaardtarieven
CPU-tijd (dag)<sup>6</sup>|60 minuten|240 minuten|Onbeperkt, betalen bij standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a>|Onbeperkt, betalen bij standaardtarieven|Onbeperkt, betalen bij standaardtarieven
Geheugen (1 uur)|1024 MB per App serviceplan|1024 MB per app|N.V.T.|N.V.T.|N.V.T.
Bandbreedte|165 MB|Onbeperkt, [gegevensoverdrachtsnelheden](https://azure.microsoft.com/pricing/details/data-transfers/) van toepassing|Onbeperkt, gegevensoverdracht tarieven toepassen|Onbeperkt, gegevensoverdracht tarieven toepassen|Onbeperkt, gegevensoverdracht tarieven toepassen
Toepassingsarchitectuur|32-bits|32-bits|32-bits/64-bits|32-bits/64-bits|32-bits/64-bits
Web-Sockets per exemplaar<sup>7</sup>|5|35|350|Onbeperkt|Onbeperkt
Gelijktijdige [verbindingen debugger](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) per toepassing|1|1|1|5|5
[azurewebsites.NET subdomein met FTP/S en SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|X|X|X|X|X
Ondersteuning voor [aangepaste domein](../articles/app-service-web/web-sites-custom-domain-name.md)||X|X|X|X
Aangepaste domein [SSL-ondersteuning](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|||Onbeperkt|Onbeperkt, 5 SNI SSL en 1 opgenomen IP SSL-verbindingen|Onbeperkt, 5 SNI SSL en 1 opgenomen IP SSL-verbindingen
Geïntegreerde Load Balancer||X|X|X|X
[Altijd aan](../articles/app-service-web/web-sites-configure.md)|||X|X|X
[Geplande back-ups](../articles/app-service-web/web-sites-backup.md)||||Eenmaal per dag|Één keer elke 5 minuten-<sup>8</sup>
[Automatisch schalen](../articles/app-service-web/web-sites-scale.md)|||X|X|X
[WebJobs](../articles/app-service-web/web-sites-create-web-jobs.md) <sup>9</sup>|X|X|X|X|X
Ondersteuning van [Azure Scheduler](https://azure.microsoft.com/services/scheduler/)||X|X|X|X
[Controle eindpunt](../articles/app-service-web/web-sites-monitor.md)|||X|X|X
[Staging-sleuven (voorbeeld)](../articles/app-service-web/web-sites-staged-publishing.md)||||5|20
Aangepaste domeinen per app</a>||500|500|500|500
SLA||<p>|99,9%|99.95%<sup>10</sup>|99.95%<sup>10</sup>

<sup>1</sup> Apps en quota zijn per App serviceplan tenzij anders vermeld.  
<sup>2</sup> Het werkelijke aantal toepassingen die u deze machines host kan zijn afhankelijk van de activiteit van de apps, de grootte van de exemplaren van de machine en de bijbehorende brongebruik.  
<sup>3</sup> Gespecialiseerde instanties kunnen worden van verschillende grootte. Zie de [Serviceprijzen App](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) voor meer informatie.  
<sup>4</sup> Premium laag kan maximaal 50 exemplaren (afhankelijk van beschikbaarheid) en 500 GB schijfruimte berekent bij het gebruik van App serviceomgevingen en 20 exemplaren en 250 GB opslag anders berekenen.  
<sup>5</sup> De maximale opslagruimte is de totale grootte van de inhoud in alle apps in dezelfde App serviceplan. Meer opties voor opslag zijn beschikbaar in de [App-omgeving](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup> Deze bronnen worden beperkt door de fysieke bronnen op de gespecialiseerde instanties (de grootte van de instantie en het aantal exemplaren).  
<sup>7</sup> Als u de schaal van een app in de elementaire fase twee exemplaren, hebt u 350 gelijktijdige verbindingen voor elk van de twee exemplaren.  
<sup>8</sup> Premium laag kan back-intervallen omlaag tot elke 5 minuten bij het gebruik van App serviceomgevingen en 50 keer per dag is anders.  
<sup>9</sup> Aangepaste uitvoerbare bestanden en/of scripts worden uitgevoerd op verzoek van een schema of doorlopend als een achtergrondtaak binnen uw App Service-exemplaar. Altijd is vereist voor continue WebJobs uitvoeren. Azure Scheduler gratis of standaard is vereist voor de geplande WebJobs. Er is geen vooraf gedefinieerde beperking van het aantal WebJobs dat in een App Service-exemplaar kan worden uitgevoerd, maar er zijn praktische beperkingen die afhankelijk zijn van wat de toepassingscode probeert te doen.   
<sup>10</sup> SLA van 99.95% voorzien die gebruikmaken van meerdere exemplaren met Azure verkeer Manager zijn geconfigureerd voor failover.  
