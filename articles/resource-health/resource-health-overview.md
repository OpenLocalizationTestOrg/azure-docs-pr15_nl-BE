<properties
   pageTitle="Overzicht van Azure Resource gezondheid | Microsoft Azure"
   description="Overzicht van Azure Resource gezondheid"
   services="Resource health"
   documentationCenter="dev-center-name"
   authors="BernardoAMunoz"
   manager=""
   editor=""/>

<tags
   ms.service="resource-health"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Supportability"
   ms.date="06/01/2016"
   ms.author="BernardoAMunoz"/>

# <a name="azure-resource-health-overview"></a>Azure Resource gezondheid-overzicht

Azure Resource gezondheid is een service die de gezondheid van individuele Azure resources worden en biedt inzichtelijker richtlijnen voor het oplossen van problemen. In een cloud omgeving waar het is niet mogelijk rechtstreeks toegang krijgen tot servers of infrastructuur elementen, is de doelstelling voor de gezondheid van de Resource minder tijd klanten kwijt bent aan het oplossen van problemen met name minder tijd besteed om te bepalen als de oorzaak van het probleem worden vastgesteld binnen de toepassing of als deze wordt veroorzaakt door een gebeurtenis in het Azure platform.

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-the-resource-is-healthy-or-not"></a>Wat wordt beschouwd als een Resource en hoe gezondheid resource wordt beslist of de resource al dan niet in orde is? 
Een bron is een exemplaar gemaakt van een brontype dat wordt geleverd door een service, bijvoorbeeld: een virtuele machine, een Web app of een SQL-database. 

Resource gezondheid afhankelijk van signalen gegenereerd door de bron-en/of de service om te bepalen of een resource al dan niet in orde is. Het is belangrijk dat u momenteel gezondheid alleen bronaccounts voor de gezondheid van een bepaalde bron typt en geen rekening met andere elementen die tot de algehele gezondheid bijdragen kunnen. Bijvoorbeeld bij het rapporteren van dat de status van een virtuele machine, alleen het gedeelte van de infrastructuur compute wordt beschouwd, d.w.z. problemen in het netwerk worden niet weergegeven in de gezondheid van de Resource, tenzij er een servicestoring aangegeven, zal in welk geval het worden opgehaald via de banner aan de bovenkant van het blad. Meer informatie over service stroomstoring wordt verderop in dit artikel aangeboden. 

## <a name="how-is-resource-health-different-from-service-health-dashboard"></a>Waarin verschilt de gezondheid van de Resource van Service Health Dashboard?

De informatie van de gezondheid van de Resource is meer granulair dan is opgegeven door de Health Service Dashboard. Hoewel SHD communiceert gebeurtenissen die invloed hebben op de beschikbaarheid van een service in een regio, gezondheid bron beschrijft de informatie die relevant is voor een bepaalde resource, zoals gebeurtenissen die invloed hebben op de beschikbaarheid van een virtuele machine, een web app of een SQL-database zal worden blootgesteld. Bijvoorbeeld als een knooppunt wordt onverwacht opnieuw opgestart, moeten klanten waarvan virtuele machines zijn uitgevoerd op dat knooppunt krijgen van de reden waarom de VM niet beschikbaar voor een periode van tijd is.   

## <a name="how-to-access-resource-health"></a>Toegang tot de gezondheid van de Resource
Er zijn 2 manieren toegang krijgen tot de gezondheid van de Resource voor de beschikbaar via Resource health services.

### <a name="azure-portal"></a>Azure Portal
De blade Resource gezondheid in de Portal Azure biedt gedetailleerde informatie over de gezondheid van de resource als aanbevolen acties afhankelijk van de huidige status van de bron zijn. Deze blade biedt de beste ervaring bij het opvragen van de gezondheid van de Resource, het vergemakkelijkt de toegang tot andere bronnen binnen de portal. Zoals eerder de aanbevolen acties in de Resource gezondheid blade set zijn afhankelijk van de huidige status:

* Gezonde bronnen: omdat er geen probleem dat invloed op de gezondheid van de resource hebben kan is gevonden, de acties zijn gericht op zodat het proces voor het oplossen van problemen. Zo biedt directe toegang tot de probleemoplossing-bladeserver die biedt hulp bij het oplossen van de meest voorkomende problemen klanten gezicht.
* Beschadigde bron: voor problemen die worden veroorzaakt door Azure, verschijnt de blade acties Microsoft neemt (of heeft) te herstellen van de resource. Voor problemen die worden veroorzaakt door de gebruiker gestarte acties, de blade wordt een lijst met klanten acties kunt uitvoeren zodat het probleem en herstellen van de resource.  

Als u bent ingelogd in de Portal Azure, zijn er twee manieren toegang krijgen tot de bron gezondheid blade: 

###<a name="open-the-resource-blade"></a>Open de bron-blade
Het blad van de Resource voor een bepaalde bron openen. Klik op het blad dat wordt geopend naast de Resource blade instellingen, op de gezondheid van de Resource voor het openen van de Resource gezondheid blade. 

![Resource gezondheid blade](./media/resource-health-overview/resourceBladeAndResourceHealth.png)

### <a name="help-and-support-blade"></a>Help en ondersteuning voor bladeservers
De blade Help en ondersteuning openen door op het vraagteken in de rechterbovenhoek te klikken en vervolgens selecteert Help + ondersteuning. 

**Op de bovenste navigatiebalk**

![Help + ondersteuning](./media/resource-health-overview/HelpAndSupport.png)

De tegel klikken opent Resource gezondheid abonnement blade die een met alle resources in uw abonnement lijst wordt. Naast elke resource is er een pictogram dat aangeeft van de gezondheid. Op elke resource te klikken, wordt de Resource gezondheid blade geopend.

**Resource gezondheid naast elkaar**

![Resource gezondheid naast elkaar](./media/resource-health-overview/resourceHealthTile.png)

## <a name="what-does-my-resource-health-status-mean"></a>Wat betekent de gezondheidsstatus van mijn Resource?
Er zijn 4 verschillende gezondheid statussen die u voor de resource ziet.

### <a name="available"></a>Beschikbaar
De service heeft geen problemen gevonden in het platform dat kan worden invloed op de beschikbaarheid van de resource. Dit wordt aangegeven door een groen vinkje. 

![Resource is beschikbaar](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Niet beschikbaar

In dit geval is de service een voortdurende probleem aangetroffen in het platform dat van invloed is op de beschikbaarheid van deze resource, bijvoorbeeld het knooppunt waarop de VM is uitgevoerd wordt onverwacht opnieuw opgestart. Dit wordt aangegeven met een rode waarschuwing. Als u meer informatie over het probleem vindt u in het middelste gedeelte van het blad, met inbegrip van: 

1.  Wat Microsoft doet om te herstellen van de resource acties 
2.  Een gedetailleerde tijdlijn van het probleem, met inbegrip van de verwachte tijd
3.  Een lijst met aanbevolen acties voor gebruikers 

![Bron is niet beschikbaar](./media/resource-health-overview/Unavailable.png)

### <a name="unavailable--customer-initiated"></a>Niet beschikbaar: klant is gestart
De bron is niet beschikbaar als gevolg van een verzoek van de klant bijvoorbeeld een resource stoppen of opnieuw aanvragen. Dit wordt aangegeven met een blauw pictogram informatief. 

![Bron is niet beschikbaar omdat de gebruiker een actie gestart](./media/resource-health-overview/userInitiated.png)

### <a name="unknown"></a>Onbekend
De service heeft geen informatie over deze bron voor meer dan 5 minuten ontvangen. Dit wordt aangegeven door een grijs vraagteken. 

Het is belangrijk te weten dat dit niet een definitieve aanwijzing is dat er iets mis is met een bron, zodat klanten dienen deze aanbevelingen te volgen:

* Als de resource wordt uitgevoerd zoals verwacht, maar de gezondheid in gezondheid Resource is ingesteld op Onbekend, er zijn geen problemen en u kunt de status van de bron bij te werken naar een gezond na een paar minuten.
* Als er problemen met het openen van de resource en de status is ingesteld op Onbekend in Resource gezondheid, dit kan moeten een vroegtijdige indicatie kan er worden een probleem en aanvullende onderzoeken worden uitgevoerd totdat de gezondheid wordt bijgewerkt met gezonde of beschadigd

![Gezondheid van de bron is onbekend.](./media/resource-health-overview/unknown.png)

## <a name="service-impacting-events"></a>Gebeurtenissen van invloed op de service
Als de resource negatieve gevolgen voor door een continue Service beïnvloeden gebeurtenis hebben kan, wordt een banner weergegeven aan de bovenkant van de Resource gezondheid blade. Op de banner te klikken, wordt de controlegebeurtenissen blade, waarin u meer informatie wilt over de onderbreking geopend.

![De gezondheid van de resource kan negatieve gevolgen hebben voor van een SIE](./media/resource-health-overview/serviceImpactingEvent.png)

## <a name="what-else-do-i-need-to-know-about-resource-health"></a>Wat moet ik weten over de gezondheid van de bron?

### <a name="signal-latency"></a>Signaal latentie
De signalen die Resource gezondheid feed, mogelijk tot 15 minuten vertraagd, waardoor de verschillen tussen de huidige status van de resource en de werkelijke beschikbaarheid. Het is belangrijk dit rekening mee moet houden als het onnodig tijd besteed aan het behandelende mogelijke problemen worden voorkomen. 

### <a name="special-case-for-sql"></a>Speciaal geval voor SQL 
De gezondheid van de resource rapporteert de status van de SQL-database, niet op de SQL server. Terwijl deze route gaan een meer realistische afbeelding van de gezondheid biedt, is vereist dat meerdere onderdelen en services worden in aanmerking genomen om te bepalen van de gezondheid van de database. Het huidige signaal afhankelijk van de aanmeldingen bij de database, wat betekent dat voor databases die regelmatig aanmeldingen (waaronder onder andere query execution aanvragen ontvangen) ontvangt de gezondheid status wordt regelmatig weergegeven. Als de database niet is geopend voor een periode van 10 minuten of langer, wordt deze verplaatst naar de status onbekend. Dit betekent niet dat de database niet beschikbaar is, alleen dat er geen signaal is uitgestoten omdat geen aanmeldingen zijn uitgevoerd. Verbinden met de database en het uitvoeren van een query worden verzonden de signalen die nodig zijn om te bepalen en de status van de database bijwerken.

## <a name="feedback"></a>Feedback
We zijn altijd open voor feedback en suggesties! Stuur ons uw [suggesties](https://feedback.azure.com/forums/266794-support-feedback). Bovendien kunt u met ons benaderd via [Twitter](https://twitter.com/azuresupport) of de [MSDN forums](https://social.msdn.microsoft.com/Forums/azure).
