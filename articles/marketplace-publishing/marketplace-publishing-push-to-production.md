<properties
   pageTitle="Uw aanbod op de markt Azure implementeren | Microsoft Azure"
   description="Informatie over en instructies voor de implementatie van uw voorstel--afbeelding virtuele machine, developer service, gegevensservice, enz., op de markt Azure doorlopen."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/02/2016"
   ms.author="hascipio" />

# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Uw aanbod op de markt Azure implementeren
Wanneer u tevreden bent met uw voorstel (dat wil zeggen, u hebt getest klantscenario, marketing, inhoud, enz.) en u bent klaar om te starten, vraagt u **Push productie** op het tabblad **publiceren** .  

1. De vier stappen in de procedure bij het publiceren pagina portal moet zijn voltooid en het groen. Zorg ervoor dat de volgende richtlijnen worden gevolgd voor virtuele Machine-aanbiedingen.

    ![tekening][img-pubportal-walkthru-checked]

2. Selecteer het tabblad **publiceren** in de lijst aan de linkerkant.

    ![tekening][img-pubportal-menu-publish]

3. Klik op **push productie en goedkeuring aanvragen**. Zodra de aanvraag wordt ingediend, de goedkeuring wordt uitgevoerd door een definitieve beoordeling en uw voorstel zal zijn beschikbaar in de markt Azure.

    ![tekening][img-pubportal-publish-pushproduction]

>[AZURE.IMPORTANT] In het geval van virtuele Machines, wanneer u klikt op de knop goedkeuring van aanvragen voor de productie, om de volgende stappen uitgevoerd achter de scène. Is het mogelijk de voortgang van elke stap in het tabblad publiceren in de publicatie bekijken portal. U moet controleren deze pagina met regelmatig interval (totdat de status van ' aanbieding' bevat) fout informatie die moet de correctie vanaf het einde.

> - Uw aanvraag voor de productie wordt in eerste instantie de certificering team dat de vhd te valideren. Echter, als u uw voorstel reeds vermelde bijwerkt en het verzoek heeft gekregen alleen marketing wijzigen, wordt de certificeringsinstantie stap overgeslagen.
> - Op de volgende stap komen de aanvraag aan het team van validatie van inhoud die de inhoud van de marketing van de aanbieding controleren.
> - Als de bovenstaande stappen uitgevoerd zijn, is het voorstel goedgekeurd in productie. Op dit moment de status worden 'weergegeven' in de portal voor publiceren. Deze status 'Aanbieding' betekent echter niet dat het proces is voltooid. De volgende stappen moeten zijn voltooid voordat het beschikbaar in de markt Azure is.
> - Zodra het voorstel is goedgekeurd in de productie in de stap hierboven, replicatie van de aanbieding wordt gestart via de Azure datacenters. Over het algemeen duurt 24-48hours voor de replicatie is voltooid maar duurt een week, afhankelijk van de grootte van de vhd. Echter, als u uw voorstel reeds vermelde bijwerkt die bevat immers alleen marketing wijzigen, vervolgens de replicatie is sneller.
> - Wanneer de replicatie voltooid is, klikt u vervolgens zijn het voorstel beschikbaar in de markt Azure.

> De aanbieding kunt u altijd verwijderen terwijl de status **concept** (dat wil zeggen, nooit **Push naar klaarzetten** of **Push productie**). Klik op de knop **Concept verwijderen** onder aan de pagina een concept verwijderen op het tabblad **Geschiedenis** .


## <a name="production-checklist-for-all-virtual-machine-offers"></a>Controlelijst voor de productie voor alle aanbiedingen voor virtuele Machine

- Zorg ervoor dat u een Microsoft Azure Certified partner
- Klik op het tabblad SKU's moeten de optie 'verbergen deze SKU van de markt omdat moet altijd worden gekocht via een sjabloon oplossing"worden gemarkeerd als Ja alleen als de SKU een onderdeel van de sjabloon voor een oplossing is. In alle andere gevallen, moet deze optie altijd worden gemarkeerd als nr.
- Let op: U moet niet de SKU zichtbaarheid instelling wijzigen zodra de SKU wordt vermeld. Wij doen deze functionaliteit niet ondersteund.
- Zorg ervoor dat de logo's aan de richtlijnen voor Azure marktplaats logo hieronder voldoen.
- Beschrijving van het voorstel en SKU, hoeft niet dezelfde.
- SKU's titel en bieden lange samenvatting hoeft niet hetzelfde.
- Overzicht bieden en SKU titel hoeft niet hetzelfde.
- SKU titels zijn niet identiek voor een aanbieding met meerdere SKU's.

**Azure Marketplace-logo richtsnoeren**

- Het Azure ontwerp heeft een eenvoudige kleurenpalet. Houd het aantal primaire en secundaire kleuren voor uw logo laag.
- De themakleuren van Azure portal zijn wit en zwart. Dus Vermijd deze kleuren als de achtergrondkleur van uw logo. Een kleur gebruiken die uw logo prominent op de Azure portal zou maken. Wij raden aan eenvoudige primaire kleuren. Als u een transparante achtergrond gebruikt, zorg ervoor dat het logo/tekst is geen wit of zwart.
- Gebruik niet een achtergrond met kleurovergang op het logo.
- Plaats tekst, zelfs in uw bedrijf of merk, klik op het logo.
- De uitstraling van uw logo moet 'platte' en vermijd verlopen.
- Het logo mag niet worden uitgerekt.

**Aanvullende richtlijnen voor het logo van de held:**

- Het logo van de held is optioneel. De uitgever kunt niet een held logo uploaden. **Maar eenmaal geüpload de held pictogram kan niet worden verwijderd uit de publicatie portal. Op dat moment moet de partner de Azure Marketplace-richtsnoeren voor de held pictogrammen anders die het voorstel niet zal worden goedgekeurd voor de productie volgen.**
- De weergegeven naam van de uitgever, SKU titel en het aanbod lang overzicht worden weergegeven in de kleur wit. Daarom moet u niet houden van een lichte kleur op de achtergrond van het pictogram van de held. Zwart, wit en transparante achtergrond is niet toegestaan voor de held pictogrammen.
- De uitgever naam, titel SKU, de aanbieding lang overzicht weergegeven en de knop maken zodra de aanbieding wordt weergegeven via de programmacode in het logo van de held zijn ingesloten. Zo moet u niet alle tekst invoeren bij het ontwerpen van de held-logo. Alleen lege ruimte laat aan de rechterkant, omdat de tekst (dat wil zeggen Publisher weergegeven naam, titel, SKU, lang overzicht aanbod) wordt opgenomen via programmacode door ons hier. De lege ruimte voor de tekst moet 415 x 100 aan de rechterkant (en dit wordt gecompenseerd door de 370px van links).


## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Biedt aanvullende productie controlelijst voor het reeds vermelde virtuele Machine

- Controleren of er al een voorstel met dezelfde naam van uw aanbieding. Zo ja, moet u een nieuwe versie van de SKU van het bestaande aanbod in plaats van het maken van een nieuwe dubbele aanbieding toevoegen.
- De gegevensschijf tussen twee versies van dezelfde SKU niet te wijzigen.
- De Azure Marketplace ondersteunt geen prijzen wijzigen van de vermelde SKU's zoals deze van invloed op de facturering van de bestaande klanten. Zorg ervoor dat u niet wijzigt de prijs van de genoemde SKU's in de gebieden waar de SKU beschikbaar is. U kunt echter nieuwe SKU's of nieuwe regio's toevoegen aan een bestaande SKU.


## <a name="next-steps"></a>Volgende stappen
Zodra het voorstel live gaat, test de scenario's van klanten om te verifiëren dat alle opdrachten en functies correct in de productieomgeving functioneren getest en gevalideerd in de testomgeving.

## <a name="see-also"></a>Zie ook
- [Aan de slag: het publiceren van een aanbod op de markt Azure](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
