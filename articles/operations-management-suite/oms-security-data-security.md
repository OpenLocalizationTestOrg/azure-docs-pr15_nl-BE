<properties
   pageTitle="Operations Management Suite veiligheid en beveiliging van gegevens voor Audit oplossing | Microsoft Azure"
   description="Dit document wordt uitgelegd hoe gegevens worden beheerd en gevrijwaard van Operations Management Suite beveiliging en controle-oplossing."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="yurid"/>

# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Operations Management Suite beveiliging en controle oplossing voor beveiliging van gegevens

Zodat klanten voorkomen, opsporen en reageren op bedreigingen worden [Operations Management Suite (OMS) beveiliging en controle oplossing](operations-management-suite-overview.md) verzamelt en verwerkt gegevens over uw bronnen, waaronder:

- Beveiligingslogboek
- Gebeurtenis Tracing voor Windows (ETW), gebeurtenissen
- AppLocker controlegebeurtenissen
- Logboek van Windows Firewall
- Geavanceerde Analytics bedreiging gebeurtenissen
- Resultaten van de beoordeling van de basislijn
- Resultaten van de beoordeling antimalware
- Resultaten van de beoordeling update/patch
- Syslogs stromen die expliciet zijn ingeschakeld op de agent

Wij maken sterke verplichtingen ter bescherming van de privacy en beveiliging van deze gegevens. Microsoft voldoet aan strenge richtlijnen voor compatibiliteit en beveiliging — van het coderen van een dienst.
In dit artikel wordt uitgelegd hoe gegevens worden beheerd en gevrijwaard van OMS beveiliging en controle-oplossing.

## <a name="data-sources"></a>Gegevensbronnen

OMS beveiligings- en controle-oplossing analyseren gegevens van uw virtuele Machines en fysieke computers waarop de Agent OMS is geïnstalleerd. OMS beveiligings- en controle-oplossing kunt verzamelen van configuratiegegevens over beveiligingsgebeurtenissen, zoals Windows-gebeurtenislogboek, controlelogboeken, IIS-logboeken en syslog berichten. Voorbeelden van dergelijke gegevens zijn: besturingssysteemtype en de versie met processen, computernaam, IP-adressen, die zijn vastgelegd in de gebruiker en huurder-ID.  

## <a name="data-protection"></a>Bescherming van gegevens

**Scheiding van gegevens**: gegevens logisch gescheiden wordt gehouden op elk onderdeel in de service. Alle gegevens per organisatie wordt gelabeld. Op deze manier blijft bestaan gedurende de levenscyclus van gegevens en deze wordt toegepast op elke laag van de service. 

**Toegang tot gegevens**: aanbevelingen voor beveiliging en potentiële beveiligingsrisico's te onderzoeken, personeel van Microsoft kunnen toegang tot informatie verzameld of geanalyseerd door de diensten. Wij voldoen aan de [Voorwaarden van Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en de [Privacy-verklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), waarin staat dat Microsoft geen gegevens van klanten gebruiken of gegevens worden opgehaald van het voor commerciële doeleinden reclame of soortgelijke. Aanbevelingen voor beveiliging en potentiële beveiligingsrisico's te onderzoeken, personeel van Microsoft toegang krijgen tot gegevens verzameld of geanalyseerd door de diensten. We alleen gebruiken gegevens van de klant indien nodig voorzien van Azure services, met inbegrip van de toepassing die compatibel is met deze services leveren. U behoudt alle rechten op uw eigen gegevens.

**Gegevens gebruiken**: Microsoft patronen en threat intelligence gezien tussen meerdere huurders wordt gebruikt voor het verbeteren van onze mogelijkheden voor detectie en preventie; dit doen we volgens de privacy verbintenissen die worden beschreven in onze [Privacyverklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [AZURE.NOTE] Locatie is geconfigureerd op het niveau van de werkruimte OMS tijdens het maken van werkruimten, die deel van de oorspronkelijke configuratieproces OMS beveiliging en controle uitmaakt.

## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe gegevens worden beheerd en in OMS gevrijwaard. Voor meer informatie over de oplossing van OMS beveiliging en controle, Zie:

- [Operations Management Suite (OMS)-overzicht](operations-management-suite-overview.md)
- [Toezicht en het reageren op de beveiligingswaarschuwingen in Operations Management Suite beveiliging en controle-oplossing](oms-security-responding-alerts.md)
- [Resources controleren in Operations Management Suite beveiliging en controle-oplossing](oms-security-monitoring-resources.md)

