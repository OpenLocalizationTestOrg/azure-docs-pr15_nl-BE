<properties
   pageTitle="Detectiemogelijkheden in Azure Security Center | Microsoft Azure"
   description="Dit document helpt u te begrijpen hoe de detectiemogelijkheden Azure Security Center werken."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-detection-capabilities"></a>Detectiemogelijkheden Azure Security Center
Dit document wordt beschreven mogelijkheden voor geavanceerde detectie Azure Security Center, dat identificeert actieve bedreigingen voor uw resources Microsoft Azure en biedt u de inzichten die nodig zijn om snel te reageren.

> [AZURE.NOTE] Geavanceerde detectie zijn beschikbaar in het standaard laag van Azure Security Center. Er is een gratis proefversie van 90 dagen beschikbaar. U kunt upgraden van de selectie van de prijzen laag in het [Beveiligingsbeleid](security-center-policies.md). Ga naar de [pagina Security Center](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie over prijzen. 


## <a name="responding-to-todays-threats"></a>Reageren op bedreigingen van vandaag
Er zijn belangrijke wijzigingen in het landschap bedreiging de afgelopen 20 jaar. In het verleden moest bedrijven doorgaans alleen zorgen te maken over de website defacement voor afzonderlijke kwaadwillende gebruikers die voornamelijk geïnteresseerd zijn in het zien 'wat zij kunnen doen'. De aanvallers van vandaag zijn veel meer geavanceerde en georganiseerd. Ze hebben vaak specifieke financiële en strategische doelstellingen. Zij hebben meer bronnen beschikbaar zijn, ook als ze kunnen worden gefinancierd door de natie-Staten of de georganiseerde misdaad.

Deze aanpak heeft geleid tot een ongekend niveau van professionalisme in de rangen van de aanvaller. Niet meer zijn ze geïnteresseerd in web defacement. Ze zijn nu geïnteresseerd in het stelen van informatie, financiële rekeningen en persoonlijke gegevens – die ze gebruiken kunnen voor het genereren van geld op de markt of gebruikmaken van een bepaalde zakelijke, politieke of militaire positie. Nog meer betreffende dan een financiële doelstelling die hackers de aanvallers die in strijd is met netwerken zijn te schade aan de infrastructuur en personeel.

Als antwoord wordt implementeren organisaties vaak verschillende punt oplossingen die zich richten op het bedrijfsnetwerk of eindpunten te zoeken naar bekende aanval handtekeningen verdediging. Deze oplossingen zijn vaak genereren een groot aantal waarschuwingen van lage kwaliteit, waarvoor een analist van beveiliging voor uw beoordelen wanneer bent en onderzoeken. De meeste organisaties niet over de tijd en de expertise die vereist zijn om te reageren op deze signalen – zo veel Ga niet geadresseerd.  Ondertussen aanvallers hebben zich ontwikkeld van hun methoden ondermijnt veel verdediging op basis van een handtekening en [aan te passen aan de cloud-omgevingen](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Nieuwe benaderingen worden snel opkomende bedreigingen te identificeren en te versnellen en het antwoord. 

## <a name="how-azure-security-center-detects-and-responds-to-threats"></a>Hoe Azure Beveiligingscentrum detecteert en reageert op bedreigingen

Microsoft security-onderzoekers zijn voortdurend op zoek naar bedreigingen. Zij hebben toegang tot een uitgebreide reeks telemetrie die is opgedaan bij de wereldwijde aanwezigheid van Microsoft in de cloud en op gebouwen. Deze wide bereiken en gevarieerde verzameling van datasets kan Microsoft nieuwe aanval patronen en trends te ontdekken via zijn op ruimten consumenten- en enterprise-producten, alsmede de on line services. Als gevolg hiervan kunt Security Center snel bijwerken algoritmen voor de detectie van aanvallers vrij nieuwe en steeds exploits. Deze benadering kunt u gelijke tred houden met een snel bewegende problematische omgeving. 

Security Center bedreiging detectie werkt door het automatisch verzamelen van informatie over de beveiliging van Azure resources, het netwerk en de oplossingen van partners verbonden. Deze informatie is vaak gegevens uit meerdere bronnen, voor het identificeren van bedreigingen correleren wordt geanalyseerd. Beveiligingswaarschuwingen in Beveiligingscentrum voorrang aangevuld met aanbevelingen voor het verhelpen van de bedreiging.

![Security Center gegevens verzamelen en presentatie](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Security Center maakt gebruik van geavanceerde beveiliging analytics die ver voorbij de aanpak op basis van een handtekening. Doorbraken in grote gegevens en [computer leren](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) technologieën wordt gebruikt om gebeurtenissen over de hele cloud fabric – opsporen van bedreigingen die onmogelijk te identificeren met behulp van handmatige methoden en voorspellen van het verloop van de aanvallen te evalueren. Deze beveiliging analytics omvatten: 

- **Geïntegreerde threat intelligence**: bekende slechte acteurs gezocht door gebruik te maken van global threat intelligence van Microsoft-producten en -services, de Microsoft Digital misdrijven eenheid (DCU), het Microsoft Security Response Center (MSRC) en externe feeds.
- **Analytics doorgevoerd**: bekende patronen te ontdekken van schadelijk gedrag van toepassing is. 
- **Afwijking detecteren**: gebruikt statistische profilering voor het maken van een historische basislijn. Wordt gewaarschuwd over afwijkingen van de vastgestelde basislijnen die aan een mogelijke aanvalsvector vormt voldoen.


### <a name="threat-intelligence"></a>Threat intelligence
Microsoft heeft een zeer grote hoeveelheid global threat intelligence. Telemetrie in loopt uit meerdere bronnen, zoals Azure, Office 365, on line Microsoft CRM, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft Digital misdrijven eenheid (DCU) en Microsoft Security Response Center (MSRC). Onderzoekers ook ontvangen threat intelligence-informatie die wordt gedeeld tussen grote cloud serviceproviders en geabonneerd op threat intelligence-feeds van derden. Azure Security Center kunt u deze informatie gebruiken om een waarschuwing voor bedreigingen van bekende slecht actoren. Enkele voorbeelden:

- **Uitgaande communicatie naar een schadelijke IP-adres**: uitgaand verkeer naar een botnet bekende of waarschijnlijke darknet geeft aan dat de bron is geknoeid en de aanvaller deze probeert uit te voeren van dat systeem- of exfiltrate opdrachten. Beveiligingscentrum Azure vergelijkt netwerkverkeer naar een database van Microsoft met globale bedreigingen en waarschuwt u als de communicatie met een schadelijke IP-adres wordt gedetecteerd.

## <a name="behavioral-analytics"></a>Analytics doorgevoerd

Doorgevoerd analytics is een techniek die analyseert en vergelijkt de gegevens aan een collectie van bekende patronen. Deze patronen zijn echter niet eenvoudig handtekeningen. Ze worden bepaald door middel van complexe machine algoritmen die worden toegepast op datasets enorme leren. Ze worden ook zorgvuldige analyse van schadelijk gedrag bepaald door ervaren analisten. Azure Security Center kunt analytics doorgevoerd om gemanipuleerde resources op basis van de analyse van de virtuele machine Logboeken, virtueel netwerk apparaat Logboeken, logboeken fabric, crashdumps en andere bronnen te gebruiken. 

Bovendien is de correlatie met andere signalen om bewijzen van een wijdverspreide campagne te controleren. Deze correlatie helpt bij het identificeren van gebeurtenissen die consistent met de vastgestelde indicatoren van compromissen zijn. Enkele voorbeelden:

- **Suspicious verwerken uitvoering**: aanvallers gebruikmaken van verschillende technieken voor het uitvoeren van schadelijke software zonder detectie. Bijvoorbeeld een aanvaller mogelijk malware geven dezelfde namen als legitieme systeembestanden maar plaats deze bestanden in een andere locatie, een naam die lijkt op een onschadelijk bestand of true bestandsextensie maskeren. Security Center processen gedrag modellen en monitoren uitvoeringen uitschieters dergelijke opsporen verwerken.  
- **Malware verborgen en exploitatie probeert**: geavanceerde malware kan omzeilen antimalware traditionele producten nooit schrijven naar schijf of software-onderdelen die zijn opgeslagen op een schijf coderen.  Echter kan deze malware worden gedetecteerd met behulp van geheugenanalyse, als de malware sporen in het geheugen laten moet om te kunnen functioneren. Wanneer de software crasht, wordt een crashdump een gedeelte van het geheugen vastgelegd op het moment van de crash.  Door een analyse van het geheugen in de crashdump, Azure Beveiligingscentrum kan detecteren technieken gebruikt om misbruik van beveiligingslekken in de software, toegang tot vertrouwelijke gegevens en ongemerkt aanhouden met in een getroffen machine zonder de prestaties van uw computer beïnvloeden.
- **Zijdelingse verplaatsing en interne reconnaissance**: persistent maken in een gemanipuleerde netwerk en zoek/oogst waardevolle gegevens, vaak aanvallers proberen u lateraal van de machine gemanipuleerde naar anderen binnen hetzelfde netwerk. Beveiligingscentrum controleert de proces-en aanmelding te ontdekken pogingen om uit te breiden van een aanvaller foothold binnen het netwerk, zoals een externe opdracht tot uitvoering van netwerk scannen en de account (opsomming).
- **Schadelijke PowerShell Scripts**: PowerShell wordt gebruikt door aanvallers schadelijke code uitvoeren op target virtuele machines voor verschillende doeleinden. Beveiligingscentrum controleert PowerShell activiteit aanwezigheid van verdachte activiteiten. 
- **Uitgaande van aanvallen**: hackers richten vaak cloud resources met de doelstelling van het gebruik van deze middelen meer inbraken koppelen. Gemanipuleerde virtuele machines kunnen bijvoorbeeld starten felle aanvallen tegen andere virtuele machines, SPAM verzenden of scan open poorten en andere apparaten op het internet worden gebruikt. Door toepassing van machine learning voor netwerkverkeer, kan Beveiligingscentrum detecteren wanneer uitgaande netwerkcommunicatie groter is dan de norm. In het geval van SPAM, Security Center ook verwijzen naar ongebruikelijke e-mail verkeer intelligence van Office 365 te bepalen of de e-mail waarschijnlijk is slechte of het resultaat van een legitiem e-campagne.  

### <a name="anomaly-detection"></a>Afwijking detectie

Afwijking detectie Azure Security Center ook gebruikt voor het identificeren van bedreigingen. Doorgevoerd analytics (dat afhankelijk is van de bekende patronen die zijn afgeleid van grote gegevenssets) daarentegen afwijking detectie is meer 'aangepast' en is gericht op basislijnen die specifiek voor de distributie zijn. Machine learning wordt toegepast om te bepalen van de normale activiteit voor de distributie en vervolgens regels worden gegenereerd weliswaar voorwaarden waaraan een beveiligingsgebeurtenis kan vertegenwoordigen. Hier volgt een voorbeeld:

- **Inbound RDP/SSH brute-force aanvallen**: uw implementaties mogelijk bezet virtuele machines met veel aanmeldingen elke werkdag en andere virtuele machines met slechts weinig of aanmeldingen. Azure Security Center kunt bepalen basislijn Aanmeldingsactiviteit voor deze virtuele machines en machine zelf bepalen welke buiten de normale Aanmeldingsactiviteit leren gebruiken. Als het aantal aanmeldingen of het tijdstip van de dag van de aanmeldingen, of de locatie waar de aanmeldingen worden aangevraagd of andere kenmerken met betrekking tot aanmelding sterk afwijkt van de basislijn zijn, kan een waarschuwing worden gegenereerd. Machine learning bepaalt, wat belangrijk is.

## <a name="continuous-threat-intelligence-monitoring"></a>Voortdurende dreiging intelligence controleren

Azure Security Center werkt beveiliging onderzoeks- en wetenschappelijke teams die continu worden gecontroleerd op wijzigingen in het landschap bedreiging. Dit omvat de volgende initiatieven:

- **Threat intelligence monitoring**: Threat intelligence bevat mechanismen, indicatoren, gevolgen en uitvoerbaar advies over bestaande of opkomende bedreigingen. Deze informatie wordt gedeeld in de beveiliging Gemeenschap en threat intelligence-feeds van interne en externe bronnen wordt voortdurend gecontroleerd door Microsoft.
- **Signaal delen**: security teams uit breed scala van Microsoft van de wolk en op ruimten services, servers en clients eindpunt inzichten worden gedeeld en geanalyseerd. 
- **Microsoft security specialisten**: voortdurende betrokkenheid bij teams uit Microsoft dat werkt in de beveiliging van speciale velden, zoals forensics en web-aanval detectie.
- **Tuning detectie**: algoritmen voor echte klant gegevenssets worden uitgevoerd en beveiliging onderzoekers samen met klanten de resultaten te valideren. True en false positieven worden verfijnen machine learning algoritmen gebruikt.

Deze gecombineerde inspanningen moet resulteren in nieuwe en verbeterde detectie, die u direct van profiteren kunt: Er is geen actie u moet ondernemen.

## <a name="see-also"></a>Zie ook
In dit document, hebt u geleerd in Beveiligingscentrum Azure detectie mogelijkheden werking. Voor meer informatie over de Security Center, raadpleegt u het volgende:

- [Beveiligingscentrum Azure Planning en Operations Guide](security-center-planning-and-operations-guide.md)
- [Beheren van en het reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md)
- [Waarschuwingen op Type in Azure Security Center](security-center-alerts-type.md)
- [Security health monitoring in Beveiligingscentrum Azure](security-center-monitoring.md) , informatie over het controleren van de gezondheid van uw resources Azure.
- [Monitoring oplossingen van partners met Azure Security Center](security-center-partner-solutions.md) , informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het Azure Security Center](security-center-faq.md) , zoeken, veelgestelde vragen over het gebruik van de service.
- [Beveiligingsblog voor Azure](http://blogs.msdn.com/b/azuresecurity/) — vinden over Azure beveiliging en naleving van blogberichten.
