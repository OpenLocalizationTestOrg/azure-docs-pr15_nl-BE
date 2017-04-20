# <a name="internet-of-things-security-best-practices"></a>Aanbevolen procedures voor beveiliging van Internet van dingen

Beveiliging van een Internet van dingen (IoT) infrastructuur, moet een strenge beveiliging-verdedigingsstrategie. Deze strategie moet u de beveiliging van gegevens in de cloud, integriteit van gegevens tijdens de overdracht beveiligen via het openbare internet en apparaten veilig te stellen. Elke laag wordt meer beveiliging zekerheid in de algemene infrastructuur.

## <a name="secure-an-iot-infrastructure"></a>Een IoT-infrastructuur beveiligen

Deze beveiliging verdedigingsstrategie kan worden ontwikkeld en uitgevoerd met de actieve deelname van de verschillende actoren die betrokken zijn bij de productie, ontwikkeling en implementatie van infrastructuur en IoT-apparaten. Hier volgt een beschrijving van deze spelers.  

- **IoT hardware fabrikant/integrator**: dit zijn de fabrikanten van IoT hardware wordt geïmplementeerd, integrators montage hardware van verschillende fabrikanten of leveranciers leveren hardware voor de implementatie van een IoT vervaardigd of door andere leveranciers is geïntegreerd.
- **IoT solution developer**: de ontwikkeling van een oplossing voor IoT wordt meestal gedaan door een ontwikkelaar van de oplossing. Deze ontwikkelaar kan deel van een in-house team of een system integrator (SI) is gespecialiseerd in deze activiteit. De IoT solution developer kunt ontwikkelen van verschillende onderdelen van de oplossing IoT geheel verschillende gebruiksklaar of open source componenten te integreren of vooraf geconfigureerde oplossingen met een kleine aanpassing.
- **Implementatie van IoT-oplossing**: na een IoT-oplossing is ontwikkeld, moet deze worden geïmplementeerd in het veld. Dit omvat de implementatie van hardware, koppeling van apparaten en de implementatie van oplossingen in de, hardwareapparaten of de cloud.
- **Operator voor IoT oplossing**: na IoT oplossing wordt geïmplementeerd, het langdurige bewerkingen, bewaking, upgrades en onderhoud vereist. Dit kan worden gedaan door een in-house team dat bestaat uit informatie technologiespecialisten, hardwarebewerkingen en onderhoud teams en domein-specialisten die de juiste werking van de algemene infrastructuur voor IoT controleren.

De volgende gedeelten vindt u aanbevolen procedures voor elk van deze spelers te ontwikkelen, implementeren en beheren van een veilige infrastructuur voor IoT.

## <a name="iot-hardware-manufacturerintegrator"></a>IoT hardware fabrikant/integrator

Hieronder volgen de aanbevolen procedures voor IoT hardwarefabrikanten en integrators hardware.

- **Scope-hardware aan de minimumvereisten**: minimale functies die nodig zijn voor de werking van de hardware, en niets meer moet worden opgenomen in het ontwerp van hardwareconfiguratie. Een voorbeeld is het USB-poorten zijn alleen indien nodig voor de werking van het apparaat. Deze extra functies opent u het apparaat voor ongewenste aanvalsvectoren die moeten worden vermeden.
- **Maken hardware knoeien bewijs**: bouwen in de mechanismen voor het detecteren van fysieke sabotagepogingen, zoals het openen van de dekking van het apparaat of een deel van het apparaat verwijderen. Deze signalen kunnen knoeien maakt mogelijk deel uit van de gegevensstroom geüpload naar de cloud, die de exploitanten van deze gebeurtenissen kan een waarschuwing.
- **Bouwen om veilig hardware**: als KPV toestaat, beveiligingsfuncties, zoals beveiligde en gecodeerde opslag bouwen of functionaliteit op Trusted Platform Module (TPM) gebaseerd opstart. Deze voorzieningen maken apparaten beveiligen en beschermen de algehele IoT-infrastructuur.
- **Beveiligde opwaarderen**: Firmware-upgrades tijdens de levensduur van het apparaat onvermijdelijk zijn. Bouwen van apparaten met veilige paden voor upgrades en cryptografische assurance van firmware-versies kunt het apparaat veilig te zijn tijdens en na de upgrade.

## <a name="iot-solution-developer"></a>IoT solution developer

De best practices voor ontwikkelaars van IoT oplossingen zijn:

- **Volg secure software development methodologie**: ontwikkeling van veilige software vereist a t/m na te denken over de beveiliging van het begin van het project aan de implementatie, testen en implementeren. De keuzen van platforms, talen en hulpprogramma's worden beïnvloed met deze methodiek. De Microsoft Security Development Lifecycle biedt een stapsgewijze aanpak voor het bouwen van veilige software.
- **Kies open source software met zorg**: Open-source-software biedt de mogelijkheid om snel oplossingen te ontwikkelen. Bij het kiezen van open source software, kunt u het activiteitenniveau van de Gemeenschap voor elk onderdeel van open source. Een actieve community zorgt ervoor dat de software wordt ondersteund en dat problemen worden ontdekt en behandeld. Ook een onduidelijke en niet-actieve open source software mogelijk niet ondersteund en problemen moeten waarschijnlijk niet worden gevonden.
- **Integreren met zorg**: veel software beveiligingsfouten aanwezig zijn op de grens van API's en bibliotheken. Functionaliteit die niet nodig zijn voor de huidige implementatie mogelijk nog steeds beschikbaar via een API-laag. Algehele beveiliging, zorg ervoor om te controleren van alle interfaces van onderdelen voor beveiligingsfouten worden geïntegreerd.      

## <a name="iot-solution-deployer"></a>Implementatie van IoT-oplossing

Aanbevolen procedures voor IoT oplossing personen zijn:

- **Hardware veilig implementeren**: IoT-implementaties vereisen hardware te implementeren in een niet-beveiligde locaties, zoals in openbare ruimten of werkende landinstellingen. In dergelijke gevallen ervoor te zorgen dat hardware implementatie fraudebestendig voor zover. Als USB- of andere poorten beschikbaar op de hardware zijn, ervoor te zorgen dat ze veilig worden behandeld. Veel aanvalsvectoren kunnen gebruiken deze ingangspunten.
- **Verificatiesleutels veilig te houden**: tijdens de implementatie elk apparaat vereist apparaat-id's en verificatiesleutels die zijn gegenereerd door de cloud-service is gekoppeld. Deze sleutels fysiek veilig blijven ook na de implementatie. Een eventueel ontcijferde sleutel kan worden gebruikt door een kwaadwillende apparaat om zich voordoen als een bestaand apparaat.

## <a name="iot-solution-operator"></a>IoT solution operator

Dit zijn de aanbevolen procedures voor IoT oplossing operators:

- **Het systeem up-to-date te houden**: er zorg voor dat apparaat, besturingssystemen en alle stuurprogramma's zijn bijgewerkt naar de nieuwste versies. Als u automatische updates in Windows 10 (IoT of andere SKU's), blijft Microsoft up-to-date met een beveiligd besturingssysteem voor IoT-apparaten. Andere besturingssystemen (zoals Linux) houden ervoor up-to-date zorgt dat zij ook beschermd tegen schadelijke aanvallen.
- **Beveiligen tegen schadelijke activiteit**: als het besturingssysteem dat toestaat, de meest recente antivirus- en antimalware-mogelijkheden op elk apparaat besturingssysteem installeren. Dit kan helpen bij de meeste externe bedreigingen te verminderen. Door passende maatregelen te nemen, kunt u de meeste moderne besturingssystemen tegen bedreigingen beschermen.
- **Regelmatig controle**: controle IoT infrastructuur voor beveiligingsproblemen is sleutel bij het reageren op beveiligingsincidenten. De meeste besturingssystemen bieden ingebouwde logboekregistratie die regelmatig moet worden herzien om ervoor te zorgen dat er geen inbreuk op de beveiliging opgetreden. Controle-informatie kan worden verzonden als een afzonderlijke telemetrie stroom naar de cloud service waar kunnen worden geanalyseerd.
- **De IoT infrastructuur fysiek beveiligen**: de slechtste aanvallen tegen IoT infrastructuur met fysieke toegang tot apparaten worden gestart. Een wordt belangrijke veiligheid beschermen tegen kwaadaardig gebruik van USB-poorten en andere fysieke toegang. Een sleutel op uncovering overtredingen die hebben plaatsgevonden is de registratie van fysieke toegang, zoals een USB-poort gebruiken. Windows 10 (IoT en andere SKU's) kunnen ook gedetailleerde registratie van deze gebeurtenissen.
- **Protect wolk referenties**: wolk verificatiereferenties gebruikt voor het configureren en gebruiken van een IoT implementatie mogelijk zijn de eenvoudigste manier om toegang en krijgen een IoT-systeem. De referenties te beschermen door het wachtwoord regelmatig wijzigen en zich onthouden van het gebruik van deze referenties op openbare computers.

Mogelijkheden van verschillende IoT apparaten verschillen. Sommige apparaten mogelijk computers met gangbare desktop besturingssysteem en sommige apparaten mogelijk zeer licht gewicht-besturingssystemen worden uitgevoerd. De aanbevolen procedures voor beveiliging beschreven eerder mogelijk van toepassing op deze apparaten in verschillende mate. Als u opgeeft, moeten extra beveiligings- en aanbevelingen van de fabrikanten van deze apparaten worden gevolgd.

Sommige oudere en beperkte apparaten mogelijk niet zijn ontworpen voor IoT-implementatie. Deze apparaten mogelijk niet over de mogelijkheid om gegevens te coderen, verbinding maken met het Internet of bieden geavanceerde controle. In deze gevallen wordt een veld van moderne en veilige gateway statistische gegevens van oude apparaten en beveiliging vereist voor het verbinden van deze apparaten via het Internet. Veld-gateways bieden beveiligde verificatie, onderhandeling van codering, ontvangst van de opdrachten van de wolk en veel andere beveiligingsfuncties.
