<properties
   pageTitle="Classificatie van gegevens voor Azure | Microsoft Azure"
   description="Dit artikel bevat een inleiding tot de basisbegrippen van de indeling van de gegevens en de waarde ervan, in het bijzonder in de context van cloud computing en het gebruik van Microsoft Azure markeert"
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yurid"/>

# <a name="data-classification-for-azure"></a>Classificatie van gegevens voor Azure

Dit artikel bevat een inleiding tot de basisbegrippen van de indeling van de gegevens en markeert u de waarde ervan, in het bijzonder in de context van cloud computing en het gebruik van Microsoft Azure. 

## <a name="data-classification-fundamentals"></a>Data classificatie fundamentals

Classificatie van succesvolle data in een organisatie moet brede kennis van de behoeften van uw organisatie en een goed begrip van waar uw activa gegevens zich bevinden.  
 
Gegevens voorkomt in een van de drie fundamentele lidstaten: 

- In rust 
- In het proces 
- In transit 
 
Alle drie staten unieke technische oplossingen voor de indeling van gegevens vereist, maar de toegepaste beginselen voor de indeling gegevens moeten identiek zijn voor elk. Gegevens die is aangemerkt als vertrouwelijk moet blijven vertrouwelijk indien bij de rest, in proces en onderweg. 
 
Een gestructureerde of ongestructureerde gegevens ook worden. Normale indeling processen voor de gestructureerde gegevens uit databases en spreadsheets zijn minder ingewikkeld en tijdrovend zijn dan die voor niet-gestructureerde gegevens zoals documenten, broncode en e-mail beheren. 

> [AZURE.TIP] voor meer informatie over de mogelijkheden van Azure en aanbevolen procedures voor gegevens lezen codering [Azure Data Encryption Best Practices](azure-security-data-encryption-best-practices.md)

In het algemeen organisaties hebben meer ongestructureerde gegevens dan gestructureerde gegevens. Ongeacht of gegevens gestructureerde of ongestructureerde, is het belangrijk dat u de gevoeligheid van de gegevens te beheren. Als het goed is geïmplementeerd, zorgt indeling van gegevens dat gevoelige of vertrouwelijke gegevens activa worden beheerd met het toezicht op de groter is dan de gegevens activa die worden beschouwd als openbaar of gratis te distribueren. 

### <a name="controlling-access-to-data"></a>Beheren, toegang tot gegevens 

Verificatie en autorisatie worden vaak verward met elkaar en hun rollen verkeerd begrepen. In werkelijkheid zijn heel anders, zoals in de volgende afbeelding.  

![Toegang tot gegevens en controle](./media/azure-security-data-classification/azure-security-data-classification-fig1.png)

### <a name="authentication"></a>Verificatie 

Verificatie bestaat meestal uit ten minste twee delen: een gebruikersnaam of gebruikers-ID voor het identificeren van een gebruiker en een token, zoals een wachtwoord, om te bevestigen dat de gebruikersnaam in de referenties geldig is. Het proces biedt de geverifieerde gebruiker met toegang tot alle artikelen of diensten; Er wordt gecontroleerd of de gebruiker die ze beweren te zijn.   

> [AZURE.TIP] [Azure Active Directory](../active-directory/active-directory-whatis.md) biedt cloud-gebaseerde identiteit services waarmee u verificatie en machtiging van gebruikers. 

### <a name="authorization"></a>Autorisatie
 
Verificatie is het proces van een geverifieerde gebruiker die toegang tot een toepassing, gegevensverzameling, bestand of een ander object. Geverifieerde gebruikers toewijzen de rechten te gebruiken, wijzigen of verwijderen items die ze toegang tot aandacht voor de indeling van gegevens vereist. 

Succesvolle vergunning vereist de uitvoering van een mechanisme voor de behoeften van individuele gebruikers toegang krijgen tot bestanden en gegevens valideren op basis van een combinatie van functies, beveiligingsbeleid en beleid risico overwegingen. Bijvoorbeeld gegevens uit specifieke line of business (LOB) toepassingen mogelijk niet kunnen worden benaderd door alle werknemers en een klein aantal werknemers moet waarschijnlijk toegang tot bestanden HR (human resources). Maar voor organisaties om te bepalen wie toegang heeft tot gegevens, zoals wanneer en hoe, als een doeltreffend systeem voor het verifiëren van gebruikers uitgevoerd wordt. 

> [AZURE.TIP] Microsoft Azure, zorg in Azure Role-Based Access Control RBAC () om de hoeveelheid toegang verlenen die gebruikers nodig hebben om hun werk te benutten. Lees [de roltoewijzingen gebruiken voor het beheren van toegang tot uw Azure Active Directory-bronnen](../active-directory/role-based-access-control-configure.md) voor meer informatie. 

### <a name="roles-and-responsibilities-in-cloud-computing"></a>Rollen en verantwoordelijkheden in cloud computing 

Hoewel cloud providers kunnen helpen bij het beheren van risico's, klanten nodig hebben om ervoor te zorgen dat Classificatiebeheer en tenuitvoerlegging correct is geïmplementeerd om het juiste niveau van data management-services.  
 
Data classificatie verantwoordelijkheden zijn afhankelijk van welk model cloud service geplaatst is, zoals aangegeven in de volgende afbeelding. De drie primaire cloud servicemodellen zijn infrastructuur als een service (IaaS), een platform als service (PaaS) en software als service (SaaS). Implementatie van data classificatie mechanismen verschilt ook op basis van het vertrouwen op en de verwachtingen van de cloud provider. 

![Rollen](./media/azure-security-data-classification/azure-security-data-classification-fig2.png)

Hoewel u verantwoordelijk bent voor het classificeren van uw gegevens, moet de cloud providers schriftelijke toezeggingen over hoe zij veilig en onderhouden van de privacy van de klantgegevens opgeslagen in de cloud.  

- **IaaS providers** zijn beperkt om ervoor te zorgen dat de virtuele omgeving geschikt voor data classificatie mogelijkheden en wensen van de klant voldoet. IaaS providers hebben een kleinere rol bij de indeling van gegevens omdat ze alleen nodig om ervoor te zorgen dat de gegevens van de klant aan vereisten adressen. Providers moeten echter nog steeds zorgen dat hun virtuele omgeving adres classificatie gegevensvereisten naast het beveiligen van hun datacenters.
- **PaaS aanbieders** verantwoordelijkheden mogen worden vermengd, omdat het platform kan worden gebruikt in een gelaagde benadering van beveiliging voor een productiekalender voor de indeling. PaaS-providers kunnen die verantwoordelijk is voor verificatie en mogelijk enkele verificatieregels en moeten bieden voor beveiliging en voorzieningen voor classificatie op de toepassingslaag. Veel als aanbieders van IaaS moeten PaaS aanbieders ervoor zorgen dat hun platform aan alle eisen van de indeling relevante gegevens voldoet.
- **SaaS-aanbieders** vaak worden beschouwd als onderdeel van een keten van de vergunning en zal nodig om ervoor te zorgen dat de gegevens die zijn opgeslagen in de SaaS toepassingen kan worden beheerd door type indeling. SaaS-toepassingen kunnen worden gebruikt voor LOB-toepassingen, en door hun aard nodig om de middelen te verifiëren en autoriseren van gegevens die wordt gebruikt en opgeslagen. 

## <a name="classification-process"></a>Classificatieproces 

Veel organisaties die begrijpen de noodzaak voor de indeling van de gegevens en deze implementeren een fundamentele uitdaging geconfronteerd: waar u moet beginnen?

Een effectieve en eenvoudige manier voor het implementeren van data classificatie is gebruik van de PLAN, DO, cheque, model van [MOF](https://technet.microsoft.com/solutionaccelerators/dd320379.aspx)FUNGEREN. De taken die nodig zijn voor het succesvol implementeren van de indeling van de gegevens in dit model-grafieken in de volgende afbeelding.  

1. **Plannen**. Identificeren van gegevens activa, een bewaarder van gegevens te implementeren het classificatie-programma en bescherming-profielen te ontwikkelen. 
2. **DO**. Nadat gegevensbeleid classificatie zijn overeengekomen, het programma te implementeren en handhaving technologieën implementeren voor vertrouwelijke gegevens.  
3. **Controleren**. Controleren en valideren rapporten om ervoor te zorgen dat de hulpprogramma's en methoden wordt effectief beleid voor de classificatie pakken. 
4. **ACT**. De status van toegang tot gegevens en bestanden bekijken en gegevens die moeten worden herzien met behulp van een methodologie herindeling en herziening wijzigingen vast te stellen en om nieuwe risico's adres te bekijken.  

![Plannen,, controleren, handelen](./media/azure-security-data-classification/azure-security-data-classification-fig3.png)
 
###<a name="select-a-terminology-model-that-addresses-your-needs"></a>Selecteer een model terminologie die inspelen op uw behoeften
 
Verschillende typen processen bestaan voor het classificeren van gegevens, met inbegrip van handmatige processen, locatie gebaseerde processen die gegevens classificeren op basis van de locatie van een gebruiker of van het systeem, op basis van een toepassing processen, zoals specifieke database-indeling, en geautomatiseerde processen die worden gebruikt door diverse technologieën, waarvan sommige in de sectie "Bescherming van vertrouwelijke gegevens" verderop in dit artikel worden beschreven.  
 
Dit artikel introduceert twee modellen van algemene termen die zijn gebaseerd op modellen goed gebruikt en industrie in acht genomen. Deze terminologie modellen, die beide drie niveaus van gevoeligheid van de indeling biedt, worden in de volgende tabel weergegeven.  

> [AZURE.NOTE] bij de indeling van een bestand of een resource met de gegevens die normaal gesproken zouden worden ingedeeld op verschillende niveaus, het hoogste niveau van de huidige classificatie vast te stellen de algemene indeling. Bijvoorbeeld een bestand met gevoelige en beperkte gegevens dienen te worden ingedeeld als beperkt.  

| **Gevoeligheid**   | **Terminologie model 1**   | **Terminologie model 2** |
|--------------------|---------------------------|-------------------------|
| Hoog               | Vertrouwelijke              | Beperkt              |
| Gemiddeld             | Alleen voor intern gebruik     | Gevoelige               |
| Laag                | Publiek                    | Onbeperkt            |

#### <a name="confidential-restricted"></a>Vertrouwelijk (beperkt) 

Gegevens die zijn ingedeeld als vertrouwelijk of beperkte bevat gegevens die desastreuze gevolgen hebben voor een of meer personen en/of organisaties kunnen worden als er beschadigde of verloren. Deze informatie wordt vaak geleverd op basis van "moet weten" en kan bevatten: 

- Persoonlijke gegevens, waaronder persoonlijke gegevens, zoals sociale zekerheid of nationale ID-nummers, passport, creditcardnummers, bestuurder licentienummers, medische gegevens en ziektekostenverzekering beleid id-nummers.  
- Financiële records, waaronder financiële rekeningnummers, zoals het controleren of de rekeningnummers investering. 
- Business-materiaal, zoals documenten of gegevens die zijn unieke of specifieke rechten op intellectueel eigendom.  
- Juridische gegevens, met inbegrip van mogelijke advocaat beschermde materiaal. 
- Verificatiegegevens, met inbegrip van persoonlijke cryptografiesleutels, gebruikersnaam wachtwoord paren of andere reeksen identificatie zoals particuliere biometrische belangrijke bestanden. 

Gegevens die vaak als vertrouwelijk is aangemerkt is regelgeving en aan de vereisten voor het verwerken van gegevens. 

#### <a name="for-internal-use-only-sensitive"></a>Voor intern gebruik alleen (gevoelig)
 
Informatie die is ingedeeld als zijnde van Gemiddeld Gevoeligheid bevat bestanden en gegevens die zou geen ernstige gevolgen voor een persoon of organisatie hebben als verloren of vernietigd. Deze informatie kan omvatten: 

- E-mailadres, waarvan de meeste kunnen worden verwijderd of gedistribueerd zonder dat een crisis (met uitzondering van postbussen of e-mailadres van de personen die worden geïdentificeerd in de classificatie van vertrouwelijke).  
- Documenten en bestanden die geen vertrouwelijke gegevens bevatten.
 
Deze indeling omvat in het algemeen alles wat niet vertrouwelijk is. Deze indeling kan de meeste zakelijke gegevens, bevatten, omdat de meeste bestanden die worden beheerd of dagelijkse gebruikt kunnen worden geclassificeerd als gevoelig. Met uitzondering van de gegevens die openbaar wordt gemaakt of vertrouwelijk is, kunnen alle gegevens binnen een organisatie business worden geclassificeerd als gevoelige standaard. 

#### <a name="public-unrestricted"></a>Publiek (onbeperkte)
 
Informatie die wordt geclassificeerd als een openbare bevat gegevens en bestanden die niet essentieel zijn voor de behoeften van het bedrijf of bewerkingen. Deze indeling is ook omvatten gegevens die opzettelijk is vrijgegeven aan het publiek voor het gebruik ervan, zoals marketingmateriaal of persberichten. Deze indeling kan ook gegevens zoals ongewenste e-mailberichten opgeslagen door een e-service bevatten. 

### <a name="define-data-ownership"></a>Gegevens eigenaar definiëren
 
Het is belangrijk vast te stellen een duidelijke vrijheidsstraf keten van eigendom voor alle activa van de gegevens. De volgende tabel bevat de verschillende gegevens eigendom rollen in data classificatie inspanningen en hun respectievelijke rechten.  

| **Rol**        | **Maken**    | **Wijzigen/verwijderen**   | **Gemachtigde**  | **Lezen**    | **Archief/herstellen**   |
|-----------------|---------------|---------------------|---------------|-------------|-----------------------|
| Eigenaar           | X             | X                   | X             | X           | X                     |
| Beheerder       |               |                     | X             |             |                       |
| Beheerder   |               |                     |               |             | X                     |
| Gebruiker\*          |               | X                   |               | X           |                       |
**Gebruikers kunnen aanvullende rechten, zoals bewerken en verwijderen door een beheerder worden verleend* 

> [AZURE.NOTE] Deze tabel biedt geen uitputtende lijst van rollen en rechten, maar slechts een representatieve steekproef. 

De **eigenaar van de gegevens actief** is de oorspronkelijke maker van de gegevens die u kunt eigendom overdragen en toewijzen van een beheerder. Wanneer een bestand wordt gemaakt, moet de eigenaar mogelijk zijn voor het toewijzen van een classificatie, wat betekent dat ze hebben een verantwoordelijkheid om te begrijpen wat er moet als vertrouwelijk worden geclassificeerd op basis van het beleid van de organisatie. Alle gegevens van activa eigenaar gegevens kan worden automatisch ingedeeld voor intern gebruik alleen (gevoelig) tenzij zij verantwoordelijk zijn voor de eigenaar of vertrouwelijk (beperkte) gegevenstypen maken. De rol van de eigenaar wordt vaak wijzigen nadat de gegevens ingedeeld. De eigenaar kan bijvoorbeeld een database van gerubriceerde informatie maken en afstaat van hun rechten aan de beheerder van de gegevens.  

> [AZURE.NOTE] gegevens actief eigenaren gebruiken vaak een combinatie van services, apparaten en media, waarvan sommige zijn persoonlijke en waarvan een aantal deel uitmaken van de organisatie. Schakel organisatiebeleid kunt ervoor te zorgen dat gebruik van apparaten zoals laptops en slimme apparaten overeenkomstig de richtlijnen voor classificatie van gegevens.  

De **gegevens activa bewaarder** wordt toegewezen door de eigenaar van de activa (of hun gemachtigde) voor het beheer van de activa volgens de overeenkomsten met de eigenaar van de activa of overeenkomstig de voorschriften van toepassing beleid. In het ideale geval kan de rol beheerder worden geïmplementeerd in een geautomatiseerd systeem. De bewaarder van een activum zorgt ervoor dat nodig voor toegangsbeheer worden geleverd en is verantwoordelijk voor het beheer en de bescherming van de activa die worden overgedragen aan de bewaring. De verantwoordelijkheden van de bewaarder actief kunnen zijn:  

- Bescherming van de activa volgens de richting van de eigenaar van de activa of in overleg met de eigenaar van de activa 
- Ervoor te zorgen dat classificatie-beleid wordt voldaan 
- Asset eigenaren melding van wijzigingen in de overeengekomen controles en/of procedures voor beveiliging voor deze wijzigingen die van kracht 
- Rapportage aan de eigenaar van de activa over wijzigingen of het verwijderen van de verantwoordelijkheden van de beheerder van de activa 
- Een **beheerder** vertegenwoordigt een gebruiker die verantwoordelijk is voor het waarborgen dat de integriteit wordt gehandhaafd, maar ze zijn niet een gegevens activa eigenaar, beheerder of gebruiker. In feite bieden veel beheerdersrollen gegevens container management services zonder toegang tot de gegevens. De rol van beheerder back-up en herstel van de gegevens, het beheren van records van de activa, en kiezen, verwerving, en gebruik de apparaten en de opslag waarop de activa bevat. 
- De gebruiker actief omvat iedereen die toegang heeft tot gegevens of een bestand. Access-toewijzing is vaak overgedragen door de eigenaar aan de bewaarder van het activum.  

### <a name="implementation"></a>Implementatie
  
Overwegingen bij toepassing op alle classificatie methodologieën. Deze overwegingen moeten bestaan over wie, wat, waar, wanneer en waarom een asset gegevens zouden worden gebruikt, geopend, gewijzigd of verwijderd. Beheer van alle activa moet worden uitgevoerd met een goed begrip van hoe een organisatie haar risico's bekijkt, maar een eenvoudige methode kan worden toegepast, zoals gedefinieerd in het classificatieproces van gegevens. Aanvullende overwegingen voor de indeling van de gegevens omvatten de invoering van nieuwe toepassingen en hulpprogramma's en wijzigingen beheren nadat een indelingsmethode is geïmplementeerd.  

### <a name="reclassification"></a>Herindeling
 
Herindelen of wijzigt de status van de classificatie van een activum gegevens moet worden uitgevoerd wanneer een gebruiker of het systeem wordt bepaald dat de gegevens van het vaste activum belang of risico-profiel is gewijzigd. Deze inspanning is het belangrijk om ervoor te zorgen dat de status van de classificatie blijft geldig en actueel zijn. De meeste inhoud die niet handmatig wordt ingedeeld kan worden automatisch ingedeeld of op basis van gebruik door een beheerder van de gegevens of gegevens eigenaar. 

### <a name="manual-data-reclassification"></a>Gegevens handmatig opnieuw indelen
 
In het ideale geval deze inspanning dat garandeert dat de details van een wijziging zijn vastgelegd en gecontroleerd. De meest waarschijnlijke reden voor het handmatig herindelen zou omwille van de gevoeligheid, of voor de administratie in papier of een vereiste gegevens die oorspronkelijk werd verkeerd geclassificeerd zijn. Omdat deze paper indeling van gegevens en het verplaatsen van gegevens naar de cloud beschouwt, zou handmatige herindeling inspanningen zou per geval aandacht vereisen en een risico evaluatie ideaal adres classificatie eisen. Een dergelijke inspanning namelijk over het algemeen beleid van de organisatie over wat er moet worden ingedeeld, de standaardstatus classificatie (alle gegevens en worden vertrouwelijke, maar niet tot vertrouwelijke bestanden), en de uitzonderingen voor hoog-risicomateriaal gegevens nemen. 

### <a name="automatic-data-reclassification"></a>Automatisch opnieuw indelen
 
Automatische herindeling gebruikt dezelfde algemene regel als handmatige classificatie. De uitzondering is dat geautomatiseerde oplossingen ervoor te zorgen dat de regels worden gevolgd en toegepast wanneer dat nodig is. Indeling van de gegevens kan worden uitgevoerd als onderdeel van een classificatie handhavingsbeleid van gegevens, die kan worden afgedwongen wanneer gegevens worden opgeslagen in gebruik en doorgang met behulp van technologie van de vergunning.

- Op basis van toepassing. Bepaalde toepassingen gebruikt, stelt u het niveau van een classificatie. Gegevens uit de software customer relationship management (CRM), HR en de gezondheid van recordbeheer hulpmiddelen is bijvoorbeeld standaard vertrouwelijk. 
- Afhankelijk van de locatie. Gegevenslocatie kunt bepalen van de gevoeligheid van de gegevens. Gegevens die zijn opgeslagen op een HR- of financiële afdeling is bijvoorbeeld waarschijnlijk vertrouwelijk karakter.  
 
### <a name="data-retention-recovery-and-disposal"></a>Gegevens bewaren, terugwinning en verwijdering 

Herstel van gegevens en verwijdering, zoals een herindeling van gegevens, is een wezenlijk aspect van de gegevens activa te beheren. De beginselen voor het herstel van gegevens en verwijdering zou worden gedefinieerd door een bewaarbeleid gegevens en toegepast op dezelfde manier als een herindeling van gegevens; een dergelijke inspanning zou worden uitgevoerd door de rollen beheerder en beheerder als een gezamenlijke taak.  

Wanneer u niet over een bewaarbeleid gegevens kan betekenen verlies van gegevens of het niet voldoen aan de vereisten voor detectie van wet- en regelgeving. De meeste organisaties die niet een duidelijk omschreven gegevens bewaarbeleid vaak gebruiken voor een bewaarbeleid standaard "alles bijhouden". Een bewaarbeleid heeft echter extra risico's in de cloud services-scenario's. 

Bijvoorbeeld worden een bewaarbeleid gegevens voor cloud-providers beschouwd als 'de duur van het abonnement' (als de service wordt betaald voor de gegevens wordt bewaard). Die een salaris voor behoud overeenkomst kan geen betrekking op bedrijfs- of regelgevende bewaarbeleid. Een beleid definieert voor vertrouwelijke gegevens kan ervoor zorgen dat gegevens opgeslagen en verwijderd op basis van beste praktijken. Bovendien een beleid voor archivering om te begrijpen over welke gegevens moeten worden afgestoten controlemethodologie kan worden gemaakt en wanneer. 

Beleid voor het bewaren van gegevens moet de vereiste adres regelgevende en conformiteitsvereisten, alsmede corporate juridische bewaarplicht. Gerubriceerde gegevens kan leiden tot vragen over behoud duur en uitzonderingen voor gegevens die is opgeslagen bij een provider; deze vragen zijn waarschijnlijk gegevens die niet correct is geclassificeerd. 

> [AZURE.TIP] meer informatie over bewaarbeleid Azure gegevens en meer van de [Abonnementsovereenkomst van Microsoft on line](https://azure.microsoft.com/support/legal/subscription-agreement/) lezen

## <a name="protecting-confidential-data"></a>Bescherming van vertrouwelijke gegevens
  
Nadat de gegevens wordt ingedeeld, wordt zoeken en implementeren van manieren om vertrouwelijke gegevens te beveiligen een integraal onderdeel van een strategie voor gegevensbescherming implementatie. Bescherming van vertrouwelijke gegevens, moet extra aandacht aan hoe gegevens worden opgeslagen en verzonden in conventionele architectuur ook in de cloud. 

In deze sectie vindt u informatie over bepaalde technologieën die handhaving inspanningen ter bescherming van gegevens die als vertrouwelijk zijn geclassificeerd kunt automatiseren. 
 
Zoals in de volgende afbeelding kunt zien, deze technologieën kunnen worden geïmplementeerd als voor lokalen of cloud-gebaseerde oplossingen, of op een wijze hybride, met enkele van deze geïmplementeerd op-gebouwen en sommige in de cloud. (Sommige technologieën, zoals codering en het rechtenbeheer, ook uitbreiden naar Gebruikersapparaten.)  

![Technologieën](./media/azure-security-data-classification/azure-security-data-classification-fig4.png)

### <a name="rights-management-software"></a>Software voor het beheer van rechten  

Een oplossing voor het voorkomen van gegevensverlies is software voor het beheer van rechten. In tegenstelling tot de benaderingen die proberen te onderbreken van de stroom van informatie op uitvoerhavens in een organisatie, werkt rights management-software op diepe niveaus in data storage technologies. Documenten worden gecodeerd en besturen die kunnen worden gedecodeerd wordt gebruikt voor toegangsbeheer die zijn gedefinieerd in een oplossing zoals een directory-service verificatie.  

> [AZURE.TIP] Azure Rights Management (Azure RMS) kunt u als de oplossing voor de bescherming van gegevens om gegevens in verschillende scenario's te beschermen. Lees [Wat is Azure Rights Management?](https://docs.microsoft.com/rights-management/understand-explore/what-is-azure-rms) voor meer informatie over deze oplossing Azure.

Enkele van de voordelen van software voor het beheer van rechten zijn: 

- Beveiligde gevoelige informatie. Gebruikers kunnen hun gegevens rechtstreeks met rights management-toepassingen beveiligen. Er zijn geen extra stappen vereist, documenten schrijven, e-mail en publiceren van gegevens de ervaring van een consistente gegevens bescherming bieden. 
- Bescherming reist met de gegevens. Klanten blijven in controle over wie toegang tot de gegevens heeft in de cloud, bestaande IT-infrastructuur, of op het bureaublad van de gebruiker. Organisaties kunnen voor hun gegevens coderen en beperk de toegang op basis van hun zakelijke behoeften kiezen. 
- Standaardbeleid informatie bescherming. Beheerders en gebruikers kunt standaardbeleid voor vele gangbare bedrijfsscenario's, zoals ' Bedrijf vertrouwelijk – is alleen-lezen' en "Niet doorsturen." Een groot aantal gebruik rechten worden ondersteund, zoals lezen, kopiëren, afdrukken, opslaan, bewerken en flexibiliteit bij het definiëren van aangepaste gebruiksrechten toe te sturen. 

> [AZURE.TIP] gegevens in Azure opslag kunt u beveiligen met behulp van [Azure Storage Service-codering](../storage/storage-service-encryption.md) voor gegevens in rust. U kunt ook [Azure schijf codering](azure-security-disk-encryption.md) ter bescherming van gegevens van virtuele schijven gebruikt voor Azure virtuele Machines.

### <a name="encryption-gateways"></a>Codering gateways

Codering gateways werken in hun eigen lagen codering om diensten te verlenen door het omleiden van alle toegang tot de gegevens op basis van een wolk. Deze aanpak moet niet worden verward met die van een virtueel particulier netwerk (VPN). Codering gateways zijn ontworpen om een transparante laag cloud-gebaseerde oplossingen te bieden.   

Codering gateways kunnen bieden een middel om te beheren en beveiligde gegevens waarvan de codeert de gegevens tijdens de overdracht en de gegevens in rust als vertrouwelijk zijn geclassificeerd.  
 
Codering gateways worden geplaatst in de gegevensstroom tussen Gebruikersapparaten, en toepassingsgegevens centers voor codering/decodering services. Deze oplossingen, zoals VPN's, zijn voornamelijk op gebouwen oplossingen. Ze zijn ontworpen om u te voorzien van een derde partij controle over coderingssleutels, die helpt het risico verminderen van het beheer van de gegevens en de sleutel met één provider te brengen. Deze oplossingen zijn ontworpen, net als de codering, probleemloos en transparant tussen gebruikers en de service. 

> [AZURE.TIP] Azure ExpressRoute kunt u uw netwerken op ruimten uitbreiden naar de cloud met Microsoft via een specifieke persoonlijke verbinding. Lees [technische ExpressRoute-overzicht](../expressroute/expressroute-introduction.md) voor meer informatie over deze mogelijkheid. Andere opties voor meerdere ruimten connectiviteit tussen uw netwerk op gebouwen en [Azure is een VPN website](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

### <a name="data-loss-prevention"></a>Gegevensverlies voorkomen 
Verlies van gegevens (ook wel gegevens lekken genoemd) is een belangrijke overweging, en de preventie van het verlies van externe gegevens via insiders schadelijk en onbedoeld is uitermate belangrijk voor vele organisaties.  
 
Gegevensverlies te voorkomen (DLP) technologieën ervoor dat oplossingen, zoals e-services niet verstrekken de gegevens die als vertrouwelijk zijn geclassificeerd. Organisaties kunnen profiteren van de DLP in bestaande producten om te voorkomen dat gegevens verloren gaan. Dergelijke functies met een beleid dat gemakkelijk gemaakt kunnen zelf of met behulp van een sjabloon die wordt geleverd door de softwareleverancier.  
 
DLP-technologie kunnen analyse diep inhoud via trefwoord resultaten, woordenboek komt overeen met evaluatie van de reguliere expressie en andere inhoud onderzoek voor het detecteren van inhoud die organisatiebeleid DLP schendt. DLP kan bijvoorbeeld helpen voorkomen dat het verlies van de volgende typen gegevens: 

- Sociale zekerheid en de nationale ID-nummers 
- Bankgegevens 
- Creditcardnummers  
- IP-adressen 

Sommige DLP-technologieën bieden ook de mogelijkheid om op te heffen de DLP-configuratie (bijvoorbeeld, als een organisatie nodig heeft voor het verzenden van informatie aan een processor salaris sofi-nummer). Het is bovendien mogelijk DLP zodanig configureren dat gebruikers worden gewaarschuwd voordat ze zelfs voor het verzenden van gevoelige informatie die niet moet worden overgebracht. 

> [AZURE.TIP] Office 365 DLP mogelijkheden kunt u uw documenten beveiligen. Lees [Office 365 naleving besturingselementen: gegevensverlies voorkomen](https://blogs.office.com/2013/10/28/office-365-compliance-controls-data-loss-prevention/) voor meer informatie.

## <a name="see-also"></a>Zie ook

- [Aanbevolen procedures voor Azure Data codering](azure-security-data-encryption-best-practices.md)
- [Azure identiteits- en toegang regelen best practices voor beveiliging](azure-security-identity-management-best-practices.md)
- [Azure Security teamblog](http://blogs.msdn.com/b/azuresecurity/)
- [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

