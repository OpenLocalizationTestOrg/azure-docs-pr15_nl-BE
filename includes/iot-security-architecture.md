# <a name="internet-of-things-security-architecture"></a>De beveiligingsarchitectuur van Internet van dingen

Bij het ontwerpen van een systeem, is het belangrijk dat de potentiële bedreigingen voor dat systeem te begrijpen en dienovereenkomstig passende beveiliging toevoegen als het systeem is ontwikkeld en ontworpen. Het is vooral belangrijk voor het ontwerpen van het product vanaf het begin rekening met beveiliging omdat begrijpen hoe een aanvaller mogelijk een systeem kunt u ervoor dat de juiste beperkingen in plaats vanaf het begin. 

## <a name="security-starts-with-a-threat-model"></a>Beveiliging begint met een bedreiging model
 
Microsoft heeft lang bedreiging modellen gebruikt voor haar producten en van het bedrijf bedreiging algemeen beschikbare onlinebronnen process modeling heeft aangebracht. De ervaring van het bedrijf laat zien dat de modellering heeft onverwachte voordelen na het direct inzicht in wat bedreigingen het meest zijn over. Bijvoorbeeld, maakt het ook een avenue voor een open discussie met anderen buiten het ontwikkelteam, wat tot nieuwe ideeën en verbeteringen in het product leiden kan.
  
Het doel van de modellering van bedreigingen is te begrijpen hoe een aanvaller mogelijk een systeem en controleer vervolgens of de juiste beperkende maatregelen zijn genomen. Bedreiging modellering krachten-team, het ontwerp rekening te houden met Beperkende factoren als het systeem is ontworpen in plaats van na een systeem wordt geïmplementeerd. Dit feit is zeer belangrijk omdat onbruikbare, aanpassing van de verdediging van de beveiliging voor een groot aantal apparaten in het veld is foutgevoelig en laat klanten risico.

Veel teams voor productontwikkeling doen een uitstekende job vastleggen van de functionele eisen voor het systeem die ten behoeve van klanten. Identificatie van niet-voor de hand liggende manieren dat iemand het systeem mogelijk misbruik is echter iets ingewikkelder. Threat modeling kunt ontwikkelteams begrijpen wat een aanvaller kan doen en waarom. Threat modeling is een gestructureerd proces dat een discussie over de beveiliging van de ontwerpbesluiten maakt in het systeem, evenals wijzigingen in het ontwerp die langs de manier waarop deze zekerheid gevolgen. Een model van de bedreiging is gewoon een document, vertegenwoordigt deze documentatie ook een ideale manier om te zorgen voor continuïteit van de kennis, het behoud van de lessen hebt geleerd en nieuwe help-team boord snel. Ten slotte is een resultaat van bedreiging modelleren om rekening te houden met andere aspecten van de beveiliging, zoals welke beveiliging verbintenissen aan uw klanten wilt bieden. Deze verplichtingen in combinatie met modelleren bedreiging wordt kennis en testen van uw oplossing Internet dingen (IoT) station.
 

### <a name="when-to-threat-model"></a>Wanneer de dreiging van model

[Threat modeling](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) krijgt u de grootste waarde als het wordt verwerkt in de ontwerpfase. Wanneer u ontwerpt, hebt u de grootste flexibiliteit om wijzigingen in de bedreigingen te elimineren. Bedreigingen te elimineren door het ontwerp is het gewenste resultaat. Het is veel gemakkelijker dan beperkingen toe te voegen, testen en ervoor te zorgen dat de huidige blijven en bovendien die verwijdering is niet altijd mogelijk. Wordt het steeds moeilijker om bedreigingen te elimineren als een product meer volwassen wordt en op zijn beurt uiteindelijk vereist meer werk en veel moeilijker en nadelen dan threat modeling vroeg stadium in de ontwikkeling.

### <a name="what-to-threat-model"></a>Wat de bedreiging model

U moet de thread-model van de oplossing in zijn geheel en ook gericht op de volgende gebieden:

- De functies voor beveiliging en privacy
- De functies waarvan storingen relevante beveiliging zijn
- De functies die de grens van een vertrouwensrelatie raken 

### <a name="who-threat-models"></a>Die dreiging modellen

Threat modeling is een proces net als elke andere.  Het is verstandig om te behandelen het modeldocument bedreiging net als andere onderdelen van de oplossing en valideren. Veel teams voor productontwikkeling doen een uitstekende job vastleggen van de functionele eisen voor het systeem die ten behoeve van klanten. Identificatie van niet-voor de hand liggende manieren dat iemand het systeem mogelijk misbruik is echter iets ingewikkelder. Threat modeling kunt ontwikkelteams begrijpen wat een aanvaller kan doen en waarom.

### <a name="how-to-threat-model"></a>Het model van de dreiging

De bedreiging modelleren proces bestaat uit vier stappen; de stappen zijn:

- Model van de toepassing
- Risico's inventariseren
- Bedreigingen te verminderen
- Valideren van de beperkende maatregelen

#### <a name="the-process-steps"></a>De stappen

Drie vuistregels rekening moet houden bij het maken van een risico-model:

1. Maak een diagram van de referentiearchitectuur. 
2. Breedte-eerst starten. Een overzicht en inzicht in het systeem als geheel, voordat een diepe duik.  Dit zorgt ervoor dat u uitvoerige op de juiste plaatsen.
3. Station tijdens het proces, niet toestaan dat het station dat u proces. Als u een probleem te vinden in de fase van het model wilt verkennen, probeer het maar!  U moet deze stappen slavishly niet voelen.  

#### <a name="threats"></a>Bedreigingen

De vier hoofdonderdelen van een risico-model zijn:

- Processen (webservices, Win32-services * nix daemons, enz. Houd er rekening mee dat sommige complexe entiteiten (bijvoorbeeld veld gateways en sensoren) kunnen worden geabstraheerd als een proces als een technische inzoomen in deze gebieden niet mogelijk is.
- Gegevens worden opgeslagen (plaatsen waar gegevens worden opgeslagen, zoals een bestand of database)
- Gegevensoverdracht (waar de gegevensdoorvoer tussen de andere elementen in de toepassing)
- Externe entiteiten (iets dat samenwerkt met het systeem, maar het is niet onder de controle van de toepassing, zijn bijvoorbeeld gebruikers en satelliet-feeds)

Alle elementen in het architectuurdiagram worden verschillende bedreigingen; We gebruiken de mnemonic stride is niet goed. [Threat Modeling opnieuw, STAPSGEWIJZE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) meer weten over de elementen stride is niet goed lezen.

Verschillende elementen van het diagram toepassing worden bepaalde bedreigingen stride is niet goed:

- Processen worden stride is niet goed
- Gegevensstromen worden TID
- Opgeslagen gegevens zijn TID, soms R, als logboekbestanden opgeslagen gegevens.
- Externe entiteiten worden onderworpen aan SRD

## <a name="security-in-iot"></a>Beveiliging in IoT

Verbonden speciale apparaten hebben een groot aantal mogelijke interactie oppervlakten en interactie patronen, die allemaal een kader te bieden voor het beveiligen van digitale toegang tot de apparaten moet worden beschouwd. De term "digitale toegang" wordt hier gebruikt om te onderscheiden van de bewerkingen die worden uitgevoerd via rechtstreekse apparaat interactie waar beveiliging is beschikbaar via de fysieke toegangsbeheer. Plaatsen van het apparaat bijvoorbeeld in een ruimte met een slot op de deur. Terwijl de fysieke toegang kan niet worden geweigerd, met behulp van software en hardware, kunnen maatregelen worden genomen om te voorkomen dat fysieke toegang ertoe leidt dat voor storing systeem. 

Zoals we de patronen van de interactie weten, gaan we "Apparaatbeheer" en "data apparaat" met hetzelfde niveau van aandacht. "Apparaatbeheer" kan worden geclassificeerd als alle informatie die wordt verstrekt aan een apparaat door een partij met het doel van invloed op het gedrag van de staat of de toestand van het milieu of wijzigen. "Apparaatgegevens" kunnen worden geclassificeerd als alle informatie die een apparaat aan een andere partij over de toestand en de waargenomen toestand van het milieu zendt.
   
Wilt optimaliseren aanbevolen procedures voor beveiliging, is het raadzaam een typische IoT-architectuur worden verdeeld in verschillende onderdelen/zones als onderdeel van de oefening modeling bedreiging. Deze zones volledig in deze sectie worden beschreven en deze omvatten:

-   Apparaat,
-   Veld-Gateway
-   Cloud gateways, en
-   Services.

Zones zijn brede manier om een segment van een oplossing; elke zone heeft vaak een eigen vereisten van gegevens en verificatie en autorisatie. Zones ook worden gebruikt om beschadiging van de isolatie en beperken van de gevolgen van de lage vertrouwen zones op hogere vertrouwde zones.

Elke zone wordt gescheiden door een grens vertrouwen die wordt geregistreerd als de gestippelde rode lijn in het onderstaande diagram. Het vertegenwoordigt een overgang van gegevens/informatie vanuit één bron naar de andere. Tijdens deze overgang, kan de gegevens onderworpen Spoofing, onrechtmatig worden gewijzigd, afwijzing, openbaarmaking van gegevens, Denial of Service en kan leiden tot misbruik van bevoegdheden (stride is niet goed).

![Beveiligingszones IoT](media/iot-security-architecture/iot-security-architecture-fig1.png) 

De onderdelen die binnen de grenzen van elke zijn ook onderworpen aan stride is niet goed, waardoor een volledige 360 threat modeling weergave van de oplossing. De onderstaande secties uitwerken op elk van de onderdelen en bepaalde beveiligingsproblemen en oplossingen die in de plaats moeten worden gebracht.

De volgende secties wordt uitgelegd hoe onderdelen meestal gevonden in deze zones.

### <a name="the-device-zone"></a>De Zone van het apparaat

De omgeving van het apparaat is de directe fysieke ruimte rond het apparaat waar de fysieke toegang en/of 'lokale netwerk' peer-to-peer digitale toegang tot het apparaat mogelijk is. 'Lokale netwerk', wordt aangenomen dat een netwerk dat is afzonderlijk en geïsoleerd van- maar een beperkt bereik draadloze radiotechnologie waarmee peer-to-peer communicatie apparaten bevat mogelijk tot en met het openbare Internet worden overbrugd. Het is *niet* een netwerk virtualisatie-technologie maakt de illusie van een lokaal netwerk en beschikt ook niet over openbare operator netwerken waarvoor twee apparaten communiceren via het openbare netwerk ruimte alsof ze een relatie met peer-to-peer communicatie invoeren.

### <a name="the-field-gateway-zone"></a>Het veld Gateway-Zone

Veld gateway is een apparaat/toestel of server voor computersoftware die als enabler voor communicatie en potentieel, als een systeem apparaat en apparaat gegevensverwerking hub fungeert. Het veld gateway-zone, bevat het veld gateway zelf en alle apparaten die zijn aangesloten op het. Zoals de naam al aangeeft, veld gateways buiten speciale verwerking faciliteiten fungeren, zijn meestal afhankelijk van locatie, worden mogelijk fysieke toegang en beperkte operationele redundantie. Alle als u wilt dat een veld gateway vaak een ding is zeggen kan een touch en sabotage terwijl u weet wat de functie ervan is. 

Een veld gateway is anders dan een loutere verkeer router heeft een actieve rol in het beheer van toegang had en de informatiestroom, wat betekent dat een toepassing is opgelost entiteit en de netwerkverbinding of een sessie van terminal. Een NAT-apparaat of een firewall, daarentegen, komen niet in aanmerking als veld gateways omdat ze niet expliciet verbinding of sessie terminals, maar in plaats daarvan een route (of blok)-verbindingen of via deze sessies. Het veld gateway heeft twee verschillende oppervlakten. Een vlakken van de apparaten die zijn aangesloten en vertegenwoordigt de binnenkant van de zone en de andere vlakken van alle externe partijen en de rand van de zone is.   

### <a name="the-cloud-gateway-zone"></a>De gateway wolk zone

Cloud-gateway is een systeem waarmee de externe communicatie van en naar de apparaten of veld gateways uit meerdere verschillende sites via een openbaar netwerk ruimte, meestal naar een cloud-gebaseerde beheer- en systeem analyse van gegevens, een Federatie van dergelijke systemen. In sommige gevallen vergemakkelijken een wolk gateway onmiddellijk toegang tot speciale apparaten van terminals tablets en telefoons. 'Cloud' is in de context hier besproken, bedoeld om te verwijzen naar een specifieke systeem dat niet is gebonden aan dezelfde site als de aangesloten apparaten of het veld gateways. Ook in een Zone wolk operationele maatregelen gerichte fysieke toegang voorkomen en wordt niet noodzakelijkerwijs naar een 'public cloud' infrastructuur.  

Een wolk gateway kan mogelijk worden toegewezen aan een netwerk virtualisatie-overlay naar cloud gateway en alle aangesloten apparaten of het veld gateways van andere netwerkverkeer kastje. De gateway van de wolk zelf is noch een controlesysteem apparaat noch een verwerking of opslag faciliteit voor apparaatgegevens; deze interface met de cloud-gateway. De gateway wolk zone bevat de wolk gateway zelf en alle veld gateways en apparaten die direct of indirect is gekoppeld. De rand van de zone is een aparte oppervlak waar alle externe partijen via communiceren.

### <a name="the-services-zone"></a>De zone services

Een "service" is gedefinieerd voor deze context als een softwareonderdeel of module die in aanraking met apparaten via een gateway of veld wolk voor gegevensverzameling en -analyse en voor de opdrachten en besturing komt.  Services zijn bemiddelaar. Ze handelen onder hun identiteit naar gateways en andere subsystemen, opslaan en analyseren van gegevens, autonoom opdrachten geven aan apparaten op basis van inzichten die gegevens of schema's en gegevens openbaren en mogelijkheden voor geautoriseerde eindgebruikers bepalen.

### <a name="information-devices-vs-special-purpose-devices"></a>Informatie versus speciale apparaten

Pc's, telefoons en tablets worden voornamelijk interactieve informatie-apparaten. Telefoons en tablets zijn expliciet geoptimaliseerd om de levensduur van de batterij te maximaliseren. Ze bij voorkeur uitschakelen gedeeltelijk als niet onmiddellijk met een persoon, of bij het verrichten van diensten zoals het afspelen van muziek of de eigenaar leidt naar een bepaalde locatie niet. Vanuit een oogpunt van systemen zijn deze apparaten informatie technologie vooral fungeert als proxy's naar mensen. Ze zijn "mensen actuators" voorstellen voor acties en "mensen sensoren' input te verzamelen. 

Speciale apparaten, verschillen van eenvoudige Temperatuursensoren aan complexe fabriek productielijnen met duizenden onderdelen binnen deze. Deze apparaten zijn veel meer binnen het bereik in doel en zelfs als ze een enkele gebruikersinterface bieden, ze zijn grotendeels binnen het bereik van interfacing met of worden geïntegreerd in de activa in de fysieke wereld. Zij meten en rapporteren milieuomstandigheden, kleppen schakelen servos bepalen, klinkt een alarm, lichten schakelen en veel andere taken uit te voeren. Ze helpen te werken waarvoor een informatie-apparaat te algemeen, te duur, te groot of te bros is. Het concrete doel bepaalt het technische ontwerp onmiddellijk als ook de beschikbare monetaire budget voor hun productie en bewerking van de voorziene levensduur. De combinatie van deze twee belangrijke factoren Hiermee beperkt u de beschikbare operationele budget energie fysieke footprint en aldus beschikbare opslagruimte berekenen en -mogelijkheden.  

Als er iets 'elektroplaat verkeerde' met geautomatiseerde of externe bedien apparaten, bijvoorbeeld fysieke gebreken of besturingselement logica gebreken van willful ongeoorloofde indringing en manipulatie. De partijen van de productie kunnen worden vernietigd gebouwen worden looted of worden gebrand omlaag en mensen die gewond raken of zelfs kan worden. Dit is natuurlijk een heel andere klasse van schade dan iemand een gestolen creditcard limiet bezetten. De balk beveiliging voor apparaten die zo zijn verplaatst en voor de sensorgegevens die uiteindelijk tot opdrachten die ervoor zorgen dat de dingen leidt te verplaatsen, moet hoger zijn dan in alle scenario voor een bank of een e-commerce. 


### <a name="device-control-and-device-data-interactions"></a>Apparaatbesturing en apparaat gegevens interacties

Verbonden speciale apparaten hebben een groot aantal mogelijke interactie oppervlakten en interactie patronen, die allemaal een kader te bieden voor het beveiligen van digitale toegang tot de apparaten moet worden beschouwd. De term "digitale toegang" wordt hier gebruikt om te onderscheiden van de bewerkingen die worden uitgevoerd via rechtstreekse apparaat interactie waar beveiliging is beschikbaar via de fysieke toegangsbeheer. Plaatsen van het apparaat bijvoorbeeld in een ruimte met een slot op de deur. Terwijl de fysieke toegang kan niet worden geweigerd, met behulp van software en hardware, kunnen maatregelen worden genomen om te voorkomen dat fysieke toegang ertoe leidt dat voor storing systeem. 
 
Zoals we de patronen van de interactie weten, gaan we "Apparaatbeheer" en "data apparaat" met hetzelfde niveau van aandacht tijdens het modelleren bedreiging. "Apparaatbeheer" kan worden geclassificeerd als alle informatie die wordt verstrekt aan een apparaat door een partij met het doel van invloed op het gedrag van de staat of de toestand van het milieu of wijzigen. "Apparaatgegevens" kunnen worden geclassificeerd als alle informatie die een apparaat aan een andere partij over de toestand en de waargenomen toestand van het milieu zendt. 

## <a name="threat-modeling-the-azure-iot-reference-architecture"></a>Threat modeling de referentiearchitectuur Azure IoT

Microsoft gebruikt het kader voor modellering voor Azure IoT uitgaan die hierboven worden beschreven. In het volgende gedeelte gebruiken we het concrete voorbeeld van Azure IoT referentiearchitectuur daarom om aan te tonen hoe om na te denken over de bedreiging voor IoT modellering en hoe de bedreigingen geïdentificeerd. In ons geval we vier hoofdgebieden van focus geïdentificeerd:

-   Apparaten en bronnen,
-   Gegevenstransport,
-   Apparaat en de verwerking van de gebeurtenis, en
-   Presentatie

![Threat Modeling for IoT Azure](media/iot-security-architecture/iot-security-architecture-fig2.png) 

Het onderstaande diagram biedt een vereenvoudigde weergave van de IoT architectuur die Microsoft met behulp van een gegevensstroomdiagram model dat wordt gebruikt door het hulpprogramma Microsoft Threat Modeling:

![Threat Modeling for Azure IoT het hulpprogramma MS Threat Modeling](media/iot-security-architecture/iot-security-architecture-fig3.png)

Het is belangrijk te weten dat de architectuur de apparaat- en gateway-mogelijkheden scheidt. Hierdoor heeft de gebruiker gebruikmaken van gatewayapparaten die veiliger zijn: ze kan communiceren met de cloud gateway met veilige protocollen, die doorgaans groter verwerkingsoverhead dat een apparaat - zoals een thermostaat - op een eigen bieden kan zijn. In de zone Azure services nemen we aan dat de wolk-Gateway wordt vertegenwoordigd door de Azure IoT Hub service.

### <a name="device-and-data-sourcesdata-transport"></a>Apparaat- en vervoer van bronnen/gegevens

Deze sectie behandelt de architectuur die hierboven worden beschreven door de lens van modellering van bedreigingen en geeft een overzicht van hoe we enkele van de bezorgdheid van de inherente pakken. Gaan we in op de basiselementen van een bedreiging model:

- Processen (die onder onze controle en de externe items)
- Communicatie (ook wel gegevensstromen)
- Opslag (ook wel opgeslagen gegevens)

#### <a name="processes"></a>Processen

In elk van de categorieën die worden beschreven in de architectuur Azure IoT we proberen te beperken van een aantal verschillende bedreigingen via data/informatie bestaat in verschillende stadia: proces, communicatie en opslag. Hieronder geven we een overzicht van de meest voorkomende codes voor de categorie 'proces', gevolgd door een overzicht van hoe deze kunnen worden best beperkt: 

**Spoofing (S)**: een aanvaller cryptografische sleutel materiaal van een apparaat, hetzij op het niveau van de software of hardware kan uitpakken en vervolgens toegang tot het systeem met een ander fysiek of virtueel apparaat onder de identiteit van het apparaat het sleutelmateriaal uit is genomen. Een goede illustratie is afstandsbedieningen die elke TV kunt inschakelen en die zijn populaire prankster-hulpprogramma's.

**Weigering van Service (D)**: een apparaat niet functioneert of communiceren door radiofrequenties of knippen draden verstoren kan worden weergegeven. Bijvoorbeeld verslag een camera toezicht dat de voeding of het netwerk verbinding opzet afgedekte had geen gegevens, op alle.

**Manipulatie (T)**: een aanvaller kan geheel of gedeeltelijk vervangen door de software die wordt uitgevoerd op het apparaat, waardoor de software vervangen om de ware identiteit van het apparaat als het sleutelmateriaal of faciliteiten cryptografische sleutel materialen die beschikbaar voor het illegale programma waren. Een aanvaller kan bijvoorbeeld gebruikmaken van uitgepakte sleutelmateriaal te onderscheppen en gegevens van het apparaat op het communicatiepad onderdrukken vervangen door false gegevens die met de gestolen sleutelmateriaal is geverifieerd.

**Vrijgeven van informatie (I)**: als het apparaat met gezelschapsdieren software, dergelijke gezelschapsdieren software mogelijk gegevens aan onbevoegden kan lekken. Een aanvaller kan bijvoorbeeld gebruikmaken van uitgepakte sleutelmateriaal zelf invoeren in de communicatie tussen het apparaat en een controller of het veld gateway of cloud-gateway naar siphon uit informatie.

**Kan leiden tot misbruik van bevoegdheden (E)**: een apparaat dat een bepaalde functie heeft kan worden gedwongen om iets anders te doen. Bijvoorbeeld kunt een klep die is geprogrammeerd om te openen halverwege laten misleiden om helemaal open.

| **Onderdeel** | **Bedreiging** | **Risicobeperking**                                                                                                                                                | **Risico**                                                                                                                                                                                                    | **Implementatie**                                                                                                                                                                                                                                                                                                                                     |
|---------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Apparaat        | S          | ID toewijzen aan het apparaat en het apparaat te verifiëren                                                                                                | Apparaat of een deel van het apparaat vervangen door een ander apparaat. Hoe weten we nu praten we naar het juiste apparaat?                                                                                           | Het apparaat met behulp van Transport Layer Security (TLS) of IPSec-verificatie. Infrastructuur dient te ondersteunen met behulp van vooraf-gedeelde sleutel (PSK) op apparaten die volledige asymmetrische cryptografie kunnen niet verwerken. Hefboomwerking Azure AD, [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt)                             |
|               | TRID       | Tamperproof mechanismen voor het apparaat bijvoorbeeld van toepassing waardoor het zeer moeilijk tot onmogelijk zijn sleutels en andere cryptografische materiaal van het apparaat. | Het risico is als iemand het apparaat (fysieke storing) is geknoeid. Hoe zijn we zeker, dat apparaat niet gewijzigd.                                                                                 | De meest effectieve oplossing is een vertrouwde platform module (TPM)-mogelijkheid waarmee sleutels op te slaan in een speciale chip op circuits waaruit de toetsen kunnen niet worden gelezen, maar kunnen alleen worden gebruikt voor cryptografische bewerkingen die de sleutel gebruiken, maar de sleutel nooit openbaar te maken. Codering van geheugen van het apparaat. Beheer van sleutels voor het apparaat. Ondertekening van de code. |
|               | E          | Gelet op het toegangsbeheer van het apparaat. Autorisatieschema.                                                                                                    | Als het apparaat kunt u afzonderlijke acties moeten worden uitgevoerd op basis van opdrachten vanaf een externe bron, of zelfs gemanipuleerde sensoren, is het toegestaan de aanval uitvoeren van bewerkingen anders niet toegankelijk. | Schema van de vergunning voor het apparaat dat                                                                                                                                                                                                                                                                                                             |
| Veld Gateway | S          | Verificatie van het veld gateway naar de Cloud-Gateway (cert gebaseerd, PSK, Claim is gebaseerd,..)                                                                           | Als iemand veld Gateway vervalsen kan, vervolgens het kan worden weergegeven als een apparaat.                                                                                                                               | TLS RSA/PSK, IPSe, [RFC 4279](https://tools.ietf.org/html/rfc4279). Dezelfde sleutel opslag en verklaring betrekking heeft op apparaten in het algemeen – beste is gebruik van TPM. De uitbreiding van de 6LowPAN voor IPSec voor de ondersteuning van draadloze Sensor netwerken (WSN).                                                                                                              |
|               | TRID       | Bescherming van het veld Gateway tegen knoeien (TPM)?                                                                                                            | Spoofing-aanvallen die de wolk gateway denken die het communiceert met het veld gateway tuin kan leiden tot vrijgeven van informatie en geknoei met gegevens                                                             | Geheugen codering, TPM van, verificatie.                                                                                                                                                                                                                                                                                                              |
|               | E          | Mechanisme voor toegangsbeheer voor veld Gateway                                                                                                                    |                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                        |

Hier volgen enkele voorbeelden van bedreigingen in deze categorie:

Spoofing: Een aanvaller kan uitpakken cryptografische sleutel materiaal uit een apparaat, hetzij bij de software of hardwareniveau en vervolgens toegang tot die het systeem met een ander fysiek of virtueel apparaat onder de identiteit van het apparaat het sleutelmateriaal onttrokken.

**Denial of Service**: een apparaat niet functioneert of communiceren door radiofrequenties of knippen draden verstoren kan worden weergegeven. Bijvoorbeeld verslag een camera toezicht dat de voeding of het netwerk verbinding opzet afgedekte had geen gegevens, op alle.

**Onrechtmatig worden gewijzigd**: een aanvaller kan geheel of gedeeltelijk vervangen door de software die wordt uitgevoerd op het apparaat, waardoor de software vervangen om de ware identiteit van het apparaat als het sleutelmateriaal of faciliteiten cryptografische sleutel materialen die beschikbaar voor het illegale programma waren.

**Onrechtmatig worden gewijzigd**: een camera toezicht dat wordt weergegeven een afbeelding van het zichtbare spectrum van een gang leeg kan zijn gericht op een foto van een dergelijke gang. Een sensor van rook of brand kan iemand die een lichtere onder het melden. In beide gevallen wordt het apparaat mogelijk technisch volledig betrouwbaar naar het systeem, maar er gezelschapsdieren informatie wordt gerapporteerd.

**Onrechtmatig worden gewijzigd**: een aanvaller kan gebruikmaken van uitgepakte sleutelmateriaal te onderscheppen en gegevens van het apparaat op het communicatiepad onderdrukken vervangen door false gegevens die met de gestolen sleutelmateriaal is geverifieerd.

**Onrechtmatig worden gewijzigd**: een aanvaller kan geheel of gedeeltelijk vervangen door de software die wordt uitgevoerd op het apparaat, waardoor de software vervangen om de ware identiteit van het apparaat als het sleutelmateriaal of faciliteiten cryptografische sleutel materialen die beschikbaar voor het illegale programma waren.
   
**Vrijgeven van informatie**: als het apparaat met gezelschapsdieren software, dergelijke gezelschapsdieren software mogelijk gegevens aan onbevoegden kan lekken.

**Vrijgeven van informatie**: een aanvaller kan gebruikmaken van uitgepakte sleutelmateriaal zelf invoeren in de communicatie tussen de gateway-apparaat en een controller of een veld of een wolk gateway naar siphon uit informatie.

**Denial of Service**: het apparaat kan worden in- of uitgeschakeld in een modus waarin communicatie is niet mogelijk (die opzettelijk is in veel industriële machines).

**Onrechtmatig worden gewijzigd**: het apparaat kan worden geconfigureerd om te werken in een andere staat bekend bij het systeem (dus niet bekende kalibratie parameters) en dus gegevens die kan worden geïnterpreteerd

**Bevoegdheden**: een apparaat dat een bepaalde functie heeft kan worden gedwongen om iets anders te doen. Bijvoorbeeld kunt een klep die is geprogrammeerd om te openen halverwege laten misleiden om helemaal open.

**Denial of Service**: het apparaat kan worden omgezet in een staat waar communicatie niet mogelijk is.

**Onrechtmatig worden gewijzigd**: het apparaat kan worden geconfigureerd om te werken in een andere staat bekend bij het systeem (dus niet bekende kalibratie parameters) en dus gegevens die kan worden geïnterpreteerd.
 
**Onrechtmatig worden gewijzigd-spoofing/weerlegging**: als niet-beveiligd (dat is zelden het geval is bij de consument afstandsbedieningen) een aanvaller anoniem de status van een apparaat kunt manipuleren. Een goede illustratie is afstandsbedieningen die elke TV kunt inschakelen en die zijn populaire prankster-hulpprogramma's.

#### <a name="communication"></a>Communicatie

Bedreigingen rond communicatiepad tussen apparaten, apparaten en gateways veld en gateway-apparaat en de cloud. In de onderstaande tabel heeft bepaalde richtlijnen rond open sockets op het apparaat of aan VPN:

| **Onderdeel**               | **Bedreiging** | **Risicobeperking**                                      | **Risico**                                                                                                      | **Implementatie**                                                                                                                                                                                                                                                                                                                                                               |
|-----------------------------|------------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Apparaat IoT Hub              | TID        | (D) TLS (PSK/RSA) voor het coderen van het verkeer             | Afluisteren of onderbreekt de communicatie tussen het apparaat en de gateway                             | Beveiliging op het protocolniveau. We moeten uitvinden hoe ze beschermen met aangepaste protocollen. In de meeste gevallen plaatsvindt de communicatie van het apparaat op de IoT Hub (apparaat start de verbinding).                                                                                                                                                                 |
| Device apparaat               | TID        | (D) TLS (PSK/RSA) voor het coderen van het verkeer.            | Het lezen van gegevens tijdens de overdracht tussen apparaten. Knoeien met de gegevens. Overbelasting van het apparaat met de nieuwe verbindingen | Beveiliging op het protocolniveau van het (MQTT/AMQP/HTTP/CoAP. We moeten uitvinden hoe ze beschermen met aangepaste protocollen. De oplossing voor de dreiging van DoS is peer-apparaten via een cloud of het veld gateway en hebben ze enige handeling als clients op het netwerk. De peering kan dit leiden tot een directe verbinding tussen de peers na dat brokered is door de gateway |
| Apparaat voor externe entiteit      | TID        | Sterke combinatie van de externe entiteit met het apparaat | De verbinding met het apparaat afluisteren. Het apparaat van de communicatie verstoren                     | De externe entiteit moet het apparaat NFC Bluetooth/LE koppelen veilig. Beheren van het operationele paneel van het apparaat (fysiek)                                                                                                                                                                                                                                                  |
| Veld Gateway wolk Gateway | TID        | TLS (PSK/RSA) voor het coderen van het verkeer.               | Afluisteren of onderbreekt de communicatie tussen het apparaat en de gateway                             | Beveiliging op het protocolniveau van het (MQTT/AMQP/HTTP/CoAP). We moeten uitvinden hoe ze beschermen met aangepaste protocollen.                                                                                                                                                                                                                                                       |
| Cloud-Gateway apparaat        | TID        | TLS (PSK/RSA) voor het coderen van het verkeer.               | Afluisteren of onderbreekt de communicatie tussen het apparaat en de gateway                             | Beveiliging op het protocolniveau van het (MQTT/AMQP/HTTP/CoAP). We moeten uitvinden hoe ze beschermen met aangepaste protocollen.                                                                                                                                                                                                                                                       |

Hier volgen enkele voorbeelden van bedreigingen in deze categorie:

**Denial of Service**: beperkte apparaten zijn over het algemeen DoS bedreigd wanneer ze actief naar binnenkomende verbindingen of ongevraagde datagrammen op een netwerk luisteren, omdat een aanvaller kan openen veel verbindingen parallel en niet deze service ze heel langzaam en/of het apparaat kan worden overspoeld met ongewenst verkeer. In beide gevallen wordt kan het apparaat effectief worden weergegeven in het netwerk niet meer werkt.

**Spoofing, vrijgeven van informatie**: beperkte apparaten en speciale apparaten hebben vaak de beveiliging van een voor alle faciliteiten zoals het wachtwoord of PINCODE beveiliging of ze volledig vertrouwen op het netwerk, wat betekent dat toegang tot informatie worden verleend wanneer een apparaat op hetzelfde netwerk is en dat netwerk vaak alleen wordt beveiligd door een gedeelde sleutel te vertrouwen. Dit betekent dat bij het gedeelde geheim op het netwerk of het apparaat wordt vermeld, is het mogelijk het apparaat beheren of observeren van de gegevens van het apparaat wordt uitgestoten.  

**Spoofing**: een aanvaller kan onderscheppen gedeeltelijk overschrijven van de uitzending of vervalsen van de opdrachtgever (man in het midden)

**Onrechtmatig worden gewijzigd**: een aanvaller kan onderscheppen gedeeltelijk overschrijven van de uitzending of valse informatie verzenden 

**Tot vrijgeven van informatie:** aanvaller kan afluisteren op een uitzending en informatie zonder toestemming verkrijgen **Denial of Service:** aanvaller kan jam uitgezonden signaal en verspreiden van informatie over weigeren

#### <a name="storage"></a>Opslag

Elk apparaat en het veld gateway is een vorm van opslag (tijdelijk voor Queuing-gegevens, foto-opslag os).

| **Onderdeel**                            | **Bedreiging** | **Risicobeperking**                       | **Risico**                                                                                                                                                                                                                                                                                                                | **Implementatie**                                                                                                                                                     |
|------------------------------------------|------------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Opslagcapaciteit op het apparaat                           | TRID       | Codering van opslag, de logboeken te ondertekenen | Het lezen van gegevens uit de opslag (PII-gegevens), geknoei met telemetriegegevens. Knoeien met in de wachtrij geplaatst of in de cache opgeslagen opdrachtgegevens. Geknoei met of de firmware-update-pakketten kan in de cache of lokaal in de wachtrij leiden tot onderdelen van OS en/of het systeem in gevaar wordt gebracht                                         | Codering, message authentication code (MAC) of de digitale handtekening. Waar mogelijk, sterke toegang beheren door middel van toegang tot bronnen (ACL's) of de machtigingen beheren. |
| Afbeelding van de OS-apparaat                          | TRID       |                                      | Knoeien met OS / vervangen van de OS-componenten                                                                                                                                                                                                                                                                         | Alleen-lezen-OS-partitie, ondertekend installatiekopie van het besturingssysteem, codering                                                                                                                    |
| Veld Gateway opslag (de gegevens queuing) | TRID       | Codering van opslag, de logboeken te ondertekenen | Lezen van gegevens uit de opslag (PII-gegevens), geknoei met telemetriegegevens, gemanipuleerd in de wachtrij geplaatst of in de cache opgeslagen opdrachtgegevens. Geknoei met of de firmware-update-pakketten (die bestemd zijn voor de apparaten of het veld gateway) kan in de cache of lokaal in de wachtrij leiden tot onderdelen van OS en/of het systeem in gevaar wordt gebracht | BitLocker                                                                                                                                                              |
| Veld Gateway besturingssysteemkopie                   | TRID       |                                      | Knoeien met OS / vervangen van de OS-componenten                                                                                                                                                                                                                                                                          | Alleen-lezen-OS-partitie, ondertekend installatiekopie van het besturingssysteem, codering                                                                                                                    |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Apparaat en de gebeurtenis verwerken/cloud gateway zone

Een cloud-gateway is een systeem waarmee de externe communicatie van en naar de apparaten of veld gateways uit meerdere verschillende sites via een openbaar netwerk ruimte, meestal naar een cloud-gebaseerde beheer- en systeem analyse van gegevens, een Federatie van dergelijke systemen. In sommige gevallen vergemakkelijken een wolk gateway onmiddellijk toegang tot speciale apparaten van terminals tablets en telefoons. In de context hier besproken, "cloud" is bedoeld om te verwijzen naar een specifieke systeem dat niet is gebonden aan dezelfde site als de aangesloten apparaten of het veld gateways en waarbij operationele maatregelen te voorkomen dat fysieke toegang aangewezen, maar is niet noodzakelijk een infrastructuur "public cloud".  Een wolk gateway kan mogelijk worden toegewezen aan een netwerk virtualisatie-overlay naar cloud gateway en alle aangesloten apparaten of het veld gateways van andere netwerkverkeer kastje. De gateway van de wolk zelf is noch een controlesysteem apparaat noch een verwerking of opslag faciliteit voor apparaatgegevens; deze interface met de cloud-gateway. De gateway wolk zone bevat de wolk gateway zelf en alle veld gateways en apparaten die direct of indirect is gekoppeld.

Cloud-gateway is meestal aangepaste ingebouwd stukje software als een service uitgevoerd met blootgestelde eindpunten die veld gateway en apparaten aansluiten. Het moet als zodanig worden ontworpen met het oog op gegevensbeveiliging. Volg [SDL](http://www.microsoft.com/sdl) -proces voor het ontwerpen en bouwen van deze service. 

#### <a name="services-zone"></a>Services-zone

Systeem (of controller) is een softwareoplossing die in combinatie met een apparaat, of een veld gateway of cloud-gateway voor het besturen van een of meerdere apparaten en/of voor het verzamelen, opslaan of analyseren van apparaatgegevens voor de presentatie of latere controle. Controlesystemen zijn de enige entiteiten in het toepassingsgebied van deze discussie die interactie met mensen onmiddellijk kan vergemakkelijken. De uitzondering zijn tussenliggende fysieke controle oppervlakken op apparaten, zoals een schakelaar waarmee een gebruiker het apparaat uitschakelen of andere eigenschappen wijzigen en waarvoor er geen functioneel equivalent die digitaal toegankelijk is. 

Tussenliggende oppervlakken van fysieke controle zijn die waar elk soort logica voor de functie van de fysieke oppervlak beperkt dat een gelijkwaardige functie op afstand kan worden gestart of invoer conflicten met externe invoer kunnen worden vermeden – dergelijke intermediated control oppervlakken conceptueel zijn aangesloten op een lokaal systeem dat gebruikmaakt van dezelfde onderliggende functies als elke andere afstandsbedieningssysteem dat het apparaat kan worden aangesloten op parallel. Belangrijkste bedreigingen voor de cloud computing kan worden gelezen op de pagina [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) .


## <a name="additional-resources"></a>Aanvullende bronnen

Raadpleeg de volgende artikelen voor meer informatie:

- [SDL bedreiging modellering, gereedschap](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
- [Microsoft Azure IoT referentiearchitectuur](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)
 
