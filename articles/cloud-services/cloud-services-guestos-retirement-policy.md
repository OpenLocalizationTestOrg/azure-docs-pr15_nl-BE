<properties 
   pageTitle="Handleiding voor ondersteuning en pensioen beleid voor het Gastbesturingssysteem Azure | Microsoft Azure" 
   description="Geeft informatie over wat Microsoft ondersteuning met betrekking tot aan de Azure Gast OS door Cloud Services gebruikt." 
   services="cloud-services" 
   documentationCenter="na" 
   authors="raiye" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="10/24/2016"
   ms.author="raiye"/>

# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure Gast OS ondersteuning en pensioen beleid
De informatie op deze pagina in relatie tot de Azure gastbesturingssysteem ([Gast OS](cloud-services-guestos-update-matrix.md)) voor werknemer Cloud Services en web-rollen (PaaS). Dit geldt niet voor virtuele Machines (IaaS). 

Microsoft heeft een gepubliceerde [ondersteuningsbeleid voor het Gastbesturingssysteem](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). De pagina die u aan het lezen bent nu wordt beschreven hoe het beleid wordt geïmplementeerd.

Het beleid is 

1. Microsoft biedt ondersteuning voor **ten minste de laatste twee families van de Gast OS**. Wanneer een familie is ingetrokken, hebben klanten 12 maanden na de datum van de officiële pensioen bij te werken naar een nieuwere ondersteunde Gast OS-familie.
2. Microsoft ondersteunt de **ten minste de laatste twee versies van de ondersteunde Gast OS families**. 
3. Microsoft ondersteunt het op **ten minste de laatste twee versies van de SDK Azure**. Wanneer u een versie van de SDK is ingetrokken, hebben klanten 12 maanden na de datum van de officiële pensioen bij te werken naar een nieuwere versie. 

Soms meer dan twee families of versies worden ondersteund. Officiële informatie over de ondersteuning van Gast OS wordt weergegeven op de [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).


## <a name="when-a-guest-os-family-or-version-is-retired"></a>Wanneer een gast OS- of versie is ingetrokken 


Een nieuwe Gast OS **familie** is ergens na de release van een nieuwe officiële versie van het besturingssysteem Windows Server geïntroduceerd. Telkens wanneer een nieuwe Gast OS-familie is geïntroduceerd, zal Microsoft de oudste Gastbesturingssysteem familie intrekken. 

Nieuwe Gast OS **versies** worden over elke maand op te nemen van de meest recente updates voor MSRC geïntroduceerd. Vanwege de reguliere maandelijkse updates is een gast OS-versie normaal uitgeschakeld 60 dagen na de release. Dit houdt ten minste twee versies van de Gast OS voor iedere familie beschikbaar voor gebruik. 

### <a name="process-during-a-guest-os-family-retirement"></a>Proces tijdens een Gastbesturingssysteem familie pensioen 


Wanneer het pensioen wordt aangekondigd, hebben klanten een overgangsperiode van twaalf maanden '' voordat de oudere familie officieel wordt verwijderd uit de service. Deze overgangstijd worden goeddunken van Microsoft verlengd. Updates worden geboekt op de [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).

6 maanden in de overgangsperiode begint een proces van geleidelijke uittreding. Gedurende deze tijd:

1. Microsoft zal u verwittigen klanten van het pensioen. 
2. De nieuwere versie van de SDK Azure de teruggetrokken Gast OS-familie niet wordt ondersteund.
3. Nieuwe implementaties en nieuwe distributies van Cloud-Services niet toegestaan op de familie teruggetrokken

Microsoft zal blijven introduceren nieuwe Gast OS versie bevat de nieuwste updates MSRC-tot de laatste dag van de overgangsperiode 'vervaldatum' genoemd. Op dat moment wordt de Cloud Services nog uitgevoerd onder de SLA Azure worden ondersteund. Microsoft heeft een upgrade te forceren, verwijderen of deze services stoppen na die datum naar keuze.



### <a name="process-during-a-guest-os-version-retirement"></a>Proces tijdens de buitengebruikstelling van een gast OS-versie 
Als klanten hun Gast OS automatisch bij te werken, hebben ze nooit zorgen te maken over het omgaan met de Gast OS versies. Zij zullen altijd gebruiken de meest recente versie van de Gast OS.

Gast OS versies worden elke maand uitgebracht. Vanwege de snelheid van reguliere releases heeft elke versie een vaste levensduur.

Na 60 dagen in de levensduur is een versie '*uitgeschakeld*'. "Uitgeschakeld" betekent dat de versie van het klassieke Azure portal wordt verwijderd. Ook kan niet meer worden ingesteld in het configuratiebestand CSCFG. Bestaande installaties's actief blijven, maar nieuwe implementaties en code- en updates van bestaande installaties zijn niet toegestaan. 

Op een later tijdstip, de Gast OS versie '*verloopt*' en alle installaties zijn die versie nog actief kracht bijgewerkt en ingesteld op het Gastbesturingssysteem in de toekomst automatisch worden bijgewerkt. Vervaldatum is gedaan in batches zodat de tijdsperiode van deactivering aflooptijd kan variëren. 

Deze periodes kunnen naar goeddunken van Microsoft te vereenvoudigen klant overgangen worden verlengd. Wijzigingen zal worden meegedeeld op de [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).



### <a name="notifications-during-retirement"></a>Meldingen bij pensionering 

* **Familie pensioen** <br>Microsoft gebruikt blogberichten en Azure klassieke portal melding. Klanten die nog steeds met een teruggetrokken Gastbesturingssysteem familie krijgt via rechtstreekse communicatie (e-mailberichten portal, telefoongesprek) met toegewezen servicebeheerders. Alle wijzigingen worden geboekt naar deze pagina en de RSS-feed aan het begin van deze pagina vermeld. 


* **Versie pensioen** <br>Alle wijzigingen worden geboekt naar deze pagina en de RSS-feed die aan het begin van deze pagina, met inbegrip van de release, uitgeschakeld en vervaldatums. Beheerders Services ontvangen berichten als ze beschikken over installaties uitvoeren op een uitgeschakelde Gast OS-versie of familie. De timing van deze e-mails kan variëren. Ze zijn over het algemeen ten minste een maand vóór de deactivering, al deze timing niet een officiële SLA is. 


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Hoe kan ik het risico van de gevolgen van migratie?**

Nieuwste Gast OS-familie moet u gebruiken voor het ontwerpen van uw Cloud-Services. 

1. Start de voorbereiding van uw migratie naar een nieuwere familie vroeg. 
2. Tijdelijke proef implementaties instellen voor het testen van uw Cloud-Service op de nieuwe familie. 
3. Uw gast OS versie ingesteld op **automatisch** (Versie_besturing = * in het [.cscfg](cloud-services-model-and-package.md#cscfg) bestand) zodat de migratie naar nieuwe versies van de Gast OS vindt automatisch plaats.

**Wat gebeurt er als mijn webtoepassing vereist betere integratie met het besturingssysteem?**

Als uw web application architecture diepere afhankelijkheid van het onderliggende besturingssysteem vereist, ondersteund platform gebruik mogelijkheden voor [opstarttaken](cloud-services-startup-tasks.md) of andere mechanismen voor uitbreidbaarheid die in de toekomst kunnen voorkomen. U kunt ook ook [Azure virtuele Machines](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS – Infrastructure als Service), waar u verantwoordelijk bent voor het onderhoud van het onderliggende besturingssysteem gebruiken.
 
## <a name="next-steps"></a>Volgende stappen
Bekijk de nieuwste [Gast OS releases](cloud-services-guestos-update-matrix.md).
