<properties
   pageTitle="Beveiliging van Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document wordt uitgelegd hoe gegevens worden beheerd en gevrijwaard in Azure Security Center."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-data-security"></a>Azure Security Center-gegevensbeveiliging
Zodat klanten voorkomen, opsporen en reageren op bedreigingen, Azure Beveiligingscentrum verzamelt en verwerkt gegevens over uw Azure bronnen, zoals configuratiegegevens, metagegevens, gebeurtenislogboeken, crash dumpbestanden en nog veel meer. Wij maken sterke verplichtingen ter bescherming van de privacy en beveiliging van deze gegevens. Microsoft voldoet aan strenge richtlijnen voor compatibiliteit en beveiliging — van het coderen van een dienst. 

In dit artikel wordt uitgelegd hoe gegevens worden beheerd en gevrijwaard in Azure Security Center.

## <a name="data-sources"></a>Gegevensbronnen

Beveiligingscentrum Azure analyseert u gegevens uit de volgende bronnen:

- Azure Services: Informatie over het configureren van Azure services die u hebt geïmplementeerd door te communiceren met deze serviceprovider resource leest.
- Netwerkverkeer: Leesbewerkingen bemonsterd verkeer metagegevens van de infrastructuur van Microsoft, zoals bron/doel IP/port, pakketgrootte, netwerk en netwerkprotocol.
- Oplossingen van partners: Verzamelt beveiligingswaarschuwingen van voor geïntegreerde partneroplossingen, zoals firewalls en antimalware oplossingen. Deze gegevens worden opgeslagen in Beveiligingscentrum Azure opslag op de huidige locatie in de Verenigde Staten.
- Uw virtuele Machines: Azure Security Center kunt configuratie-informatie en informatie over beveiligingsgebeurtenissen, zoals Windows-gebeurtenislogboek verzamelen en controleren van Logboeken, IIS-logboeken syslog berichten en crashdump-bestanden van de virtuele machines met behulp van data-agenten. Zie de sectie 'Gegevens verzamelen beheren' hieronder voor meer informatie.  

Bovendien wordt informatie over beveiligingswaarschuwingen, aanbevelingen en beveiliging gezondheidsstatus opgeslagen in Beveiligingscentrum Azure opslag, nu in de Verenigde Staten bevinden. Het kan onder meer verwante configuratiegegevens en beveiligingsgebeurtenissen van uw virtuele machines om te bieden met een beveiligingswaarschuwing, aanbeveling of beveiliging gezondheidsstatus verzameld.

## <a name="data-protection"></a>Bescherming van gegevens

**Scheiding van gegevens**: gegevens logisch gescheiden wordt gehouden op elk onderdeel in de service. Alle gegevens per organisatie wordt gelabeld. Op deze manier blijft bestaan gedurende de levenscyclus van gegevens en deze wordt toegepast op elke laag van de service. Bovendien worden gegevens worden verzameld uit uw virtuele machines opgeslagen in uw opslag (s).

**Toegang tot gegevens**: aanbevelingen voor beveiliging te bieden en potentiële beveiligingsrisico's te onderzoeken, personeel van Microsoft kunnen toegang tot informatie verzameld of geanalyseerd door Azure services crashdump-bestanden. Crashdump-bestanden en proces maken gebeurtenissen eventueel onbedoeld met klantgegevens of persoonlijke gegevens uit uw virtuele machines. Wij voldoen aan de [Voorwaarden van Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en de [Privacy-verklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), waarin staat dat Microsoft geen gegevens van klanten gebruiken of gegevens worden opgehaald van het voor commerciële doeleinden reclame of soortgelijke. We alleen gebruiken gegevens van de klant indien nodig voorzien van Azure services, met inbegrip van de toepassing die compatibel is met deze services leveren. U behoudt alle rechten op gegevens van de klant.

**Gegevens gebruiken**: Microsoft patronen en threat intelligence gezien tussen meerdere huurders wordt gebruikt voor het verbeteren van onze mogelijkheden voor detectie en preventie; dit doen we volgens de privacy verbintenissen die worden beschreven in onze [Privacyverklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

**Locatie**: een opslag rekening is opgegeven voor elke regio waar de virtuele machines worden uitgevoerd. Hiermee kunt u gegevens opslaan in hetzelfde gebied, als de virtuele machine van waaruit de gegevens worden verzameld. Deze gegevens, inclusief de crashdump-bestanden, wordt permanent opgeslagen in uw account voor opslag. De service bevat ook informatie over beveiligingswaarschuwingen, inclusief waarschuwingen van geïntegreerde partner solutions, aanbevelingen en beveiliging gezondheidsstatus in Beveiligingscentrum Azure opslag op de huidige locatie in de Verenigde Staten.

## <a name="managing-data-collection-from-virtual-machines"></a>Het verzamelen van gegevens van virtuele machines te beheren

Wanneer u kiest voor Azure Beveiligingscentrum inschakelen, is het verzamelen van gegevens voor elk van uw abonnementen ingeschakeld. U kunt het verzamelen van gegevens in de sectie 'Security Policy' van het Dashboard Azure Security Center uitschakelen. Bij het verzamelen van gegevens is ingeschakeld, ondersteund Azure Beveiligingscentrum bepalingen Azure Monitoring Agent op alle bestaande virtuele machines en nieuwe bestanden die worden gemaakt. De extensie Azure Security Monitoring scant voor verschillende beveiligingsproblemen configuraties en gebeurtenissen in [Event Tracing voor Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) wordt getraceerd. Bovendien wordt het besturingssysteem logboekgebeurtenissen verhogen in de loop van de computer wordt uitgevoerd. Voorbeelden van dergelijke gegevens zijn: besturingssysteemtype en de versie, het systeemlogboek in Logboeken (Windows-gebeurtenislogboeken), processen, computernaam, de IP-adressen, die ingelogd gebruiker en huurder-ID. De Azure Monitoring Agent logboekvermeldingen leest en ETW traceert en kopieert deze naar uw account opslag voor analyse. 

Een opslag-rekening is opgegeven voor elke regio waarin u de virtuele machines die worden uitgevoerd, waarbij gegevens verzameld van virtuele machines die hetzelfde gebied wordt opgeslagen hebt. Hierdoor kunt gemakkelijk gegevens op te slaan in hetzelfde geografische gebied voor privacy en gegevens soevereiniteit doeleinden. U kunt accounts opslag voor elke regio in de sectie 'Security Policy' van het Dashboard Azure Security Center.

Crashdump-bestanden de Azure Monitoring Agent ook gekopieerd naar de rekening van de opslag.  Beveiligingscentrum Azure tijdelijke kopieën van uw crashdump-bestanden verzamelt en analyseert ze van pogingen misbruik en succesvolle inbreuk.  Azure Security Center voert deze analyse binnen hetzelfde geografische gebied, als de account voor de opslag en de tijdelijke kopieën worden verwijderd wanneer de analyse is voltooid.

Het verzamelen van virtuele machines op elk gewenst moment, waarbij alle agenten controleren eerder geïnstalleerd door Beveiligingscentrum Azure wordt verwijderd, kunt u uitschakelen.


## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe gegevens worden beheerd en gevrijwaard in Azure Security Center. Meer informatie over Azure Security Center, Zie:

- [Azure Security Center Planning en Operations Guide](security-center-planning-and-operations-guide.md) , informatie over het plannen en inzicht in de overwegingen Azure Beveiligingscentrum vast te stellen.
- [Security health monitoring in Beveiligingscentrum Azure](security-center-monitoring.md) , informatie over het controleren van de gezondheid van uw resources Azure
- [Beheren en reageren op de beveiligingswaarschuwingen in Beveiligingscentrum Azure](security-center-managing-and-responding-alerts.md) , informatie over het beheren van en reageren op de beveiligingswaarschuwingen
- [Monitoring oplossingen van partners met Azure Security Center](security-center-partner-solutions.md) , informatie over het controleren van de status van uw oplossingen van partners.
- [Azure Security Center FAQ](security-center-faq.md) , veelgestelde vragen over het gebruik van de service zoeken
- [Beveiligingsblog voor Azure](http://blogs.msdn.com/b/azuresecurity/) — vinden over Azure beveiliging en naleving van blogberichten
