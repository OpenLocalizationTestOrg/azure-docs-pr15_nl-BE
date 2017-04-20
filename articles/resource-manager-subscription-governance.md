<properties
   pageTitle="Aanbevolen procedures voor ondernemingen verplaatsen naar Azure | Microsoft Azure"
   description="Beschrijving van een scaffold die ondernemingen gebruiken kunnen om een veilige en beheerbare omgeving te creëren."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure enterprise scaffold - uitgebreide abonnement governance

Ondernemingen worden steeds vaker tot vaststelling van de openbare cloud voor wendbaarheid en flexibiliteit. Ze zijn met behulp van sterke om inkomsten te genereren of resources voor het optimaliseren van de wolk. Microsoft Azure biedt een groot aantal services dat ondernemingen zoals bouwstenen om tal van werkbelasting en toepassingen kunnen samenstellen. 

Maar weet waar u moet beginnen is het vaak moeilijk. Nadat zij tot Azure gebruiken, ontstaan een paar vragen vaak:

- "Hoe ik voldoen aan onze wettelijke vereisten voor gegevens-soevereiniteit in bepaalde landen?"
- "Hoe kan ik ervoor zorgen dat iemand per ongeluk niet een essentiële systeem verandert?"
- "Hoe weet ik wat elke resource ondersteunt zodat ik rekening gehouden met er het en bill nauwkeurig weer?"

Het vooruitzicht van een lege abonnement met geen rails guard is een lastige. Deze lege ruimte kan uw overstap naar Azure belemmeren.

Dit artikel biedt een beginpunt voor de technische medewerkers van de behoefte voor governance en het in evenwicht met de noodzaak van flexibiliteit. Het geeft het concept van een onderneming scaffold die organisaties helpt bij de tenuitvoerlegging en het beheer van hun Azure abonnementen. 

## <a name="need-for-governance"></a>Behoefte aan deugdelijk bestuur

Als u naar Azure, kunt u het onderwerp van bestuur vroeg om ervoor te zorgen het succesvolle gebruik van de wolk binnen de onderneming moet verhelpen. Helaas betekent de tijd en bureaucracy voor het maken van een uitgebreide governance dat enkele business groepen gaat u rechtstreeks naar leveranciers zonder tussenkomst van IT-onderneming. Deze aanpak laat de onderneming openen beveiligingslekken als de resources niet goed worden beheerd. De kenmerken van de openbare cloud - wendbaarheid, flexibiliteit en prijzen op basis van consumptie - zijn belangrijk voor bedrijfsgroepen die moet snel te voldoen aan de vereisten van klanten (zowel intern als extern). Maar enterprise IT nodig heeft om ervoor te zorgen dat gegevens en systemen effectief beschermd zijn.

Steiger wordt in de praktijk gebruikt om de basis van de structuur. De scaffold begeleidt algemeen overzicht en kunt ankerpunten voor meer permanente systemen te koppelen. Een onderneming scaffold is hetzelfde: een reeks flexibele besturingselementen en Azure mogelijkheden structuur voor het milieu en ankers voor de openbare cloud-services. Het biedt de builders (IT en business groepen) een basis te maken en nieuwe services toevoegen.

De scaffold is gebaseerd op de procedures die we hebben verzameld van vele betrekkingen met klanten van verschillende grootte. Deze clients kunnen variëren van kleine organisaties ontwikkelen van oplossingen in de cloud Fortune 500-ondernemingen en onafhankelijke softwareleveranciers die worden gemigreerd en ontwikkelen van oplossingen in de cloud. De onderneming scaffold is "doel-bewust gebouwde" flexibel ter ondersteuning van zowel traditionele IT werkbelasting en flexibele werkbelasting; zoals ontwikkelaars maken van toepassingen in software-as-a-service (SaaS) gebaseerd op Azure mogelijkheden.

De onderneming scaffold is bedoeld als de basis van elk nieuw abonnement in Azure. Hiermee kunnen beheerders waarborgen workloads voldoen aan minimale bestuur van een organisatie zonder te voorkomen dat bedrijven en ontwikkelaars snel hun eigen doelstellingen.

> [AZURE.IMPORTANT] Governance is van cruciaal belang voor het succes van Azure. Dit artikel is bedoeld voor de technische uitvoering van een onderneming scaffold, maar alleen het perfectioneren van het bredere proces en de relaties tussen de onderdelen. Governance beleid van boven naar beneden loopt en wordt bepaald door het bedrijf wil bereiken. Natuurlijk, het maken van een model van governance voor Azure omvat vertegenwoordigers van de IT, maar belangrijker is deze sterke vertegenwoordiging van de groep leidinggevenden en beveiliging en risicobeheer moet hebben. Uiteindelijk is een onderneming scaffold over beperkende bedrijfsrisico ter vergemakkelijking van de missie en doelstellingen van een organisatie.

De volgende afbeelding worden de onderdelen van de scaffold beschreven. De Stichting is gebaseerd op een solide plan voor afdelingen, rekeningen en abonnementen. De stijlen bestaan uit Resource Manager beleid en sterke naamgevingsstandaarden. De rest van de scaffold afkomstig van core Azure mogelijkheden en functies die een veilige en beheerbare omgeving.

![scaffold onderdelen](./media/resource-manager-subscription-governance/components.png)

> [AZURE.NOTE] Azure is snel gegroeid sinds de introductie in 2008. Deze groei vereist Microsoft engineering-teams na te denken over hun aanpak voor het beheren en implementeren van services. De bronnenbeheerder Azure model werd geïntroduceerd in 2014 en wordt vervangen door het implementatiemodel klassiek. Resource Manager kunnen organisaties gemakkelijker implementeren, organiseren en Azure bronnen beheren. Resource Manager bevat parallelization bij het maken van bronnen voor een snellere implementatie van oplossingen voor complexe en onderling afhankelijk zijn. Bevat tevens gedetailleerde toegangscontrole en de mogelijkheid tot code bronnen met metagegevens. Microsoft raadt u aan alle resources via het objectmodel Resource Manager maken. De onderneming scaffold is expliciet ontworpen voor het model Resource Manager.

## <a name="define-your-hierarchy"></a>De hiërarchie definiëren

De basis van de scaffold is de inschrijving Azure Enterprise (Enterprise Portal). De inschrijving voor de onderneming definieert de vorm en gebruik van Azure services binnen een bedrijf is de bestuursstructuur core. In de enterprise agreement kunnen klanten het milieu in afdelingen, accounts en ten slotte abonnementen verder onderverdelen. Een abonnement op Azure is de basiseenheid waarin alle bronnen zijn opgenomen. Het definieert ook verschillende limieten in Azure, zoals het aantal cores, resources, enz.

![hiërarchie](./media/resource-manager-subscription-governance/agreement.png)

Elke onderneming is anders en de hiërarchie in de vorige afbeelding kunt u aanzienlijke flexibiliteit in hoe Azure is georganiseerd binnen het bedrijf. Voordat u de instructies in dit document opgenomen, model van de hiërarchie en u de impact op de factuur, de toegang tot bronnen en de complexiteit te begrijpen.

De drie algemene patronen voor Azure inschrijvingen zijn:

- Het **functionele** patroon

    ![functionele](./media/resource-manager-subscription-governance/functional.png)

- Het patroon van de **business unit** 

    ![Business](./media/resource-manager-subscription-governance/business.png)

- Het **geografisch** patroon

    ![geografische](./media/resource-manager-subscription-governance/geographic.png)

U de scaffold op het niveau van het abonnement uit te breiden de governance van de eisen van de onderneming in het abonnement van toepassing.

## <a name="naming-standards"></a>Naamgevingsstandaarden

De eerste pijler van de scaffold is naamgevingsvereisten. Goed ontworpen naamgevingsstandaarden kunnen u om resources in de portal op een rekening en in scripts. Waarschijnlijk hebt u al naamgevingsvereisten voor lokale infrastructuur. Azure toevoegen aan uw omgeving, moet u deze naamgevingsstandaarden uitbreiden naar Azure resources. Efficiënter beheer van het milieu op alle niveaus naamgevingsstandaard vergemakkelijken.

> [AZURE.TIP]
>
> - Controleren en vaststellen waar mogelijk de [richtlijnen voor patronen en procedures](./guidance/guidance-naming-conventions.md). Deze richtlijnen kunt u bepalen op een zinvolle naamgevingsstandaard.
> - CamelCasing gebruiken voor namen van resources (zoals myResourceGroup en vnetNetworkName). Opmerking: Er zijn bepaalde bronnen, zoals accounts, opslag, waarbij de enige optie is om te gebruiken in kleine letters (en geen speciale tekens).
> - Kunt u afdwingen naamgevingsstandaarden met Azure Resource Manager-beleid (beschreven in de volgende sectie).
 
## <a name="policies-and-auditing"></a>Beleid en controle

De tweede pijler van de scaffold moeten worden gemaakt van [Azure Resource Manager-beleid](resource-manager-policy.md) en de [controle van het logboek voor faxactiviteit](resource-group-audit.md). Resource Manager-beleid bieden de mogelijkheid tot beheer van risico's in Azure. U kunt beleid dat de soevereiniteit van de gegevens te garanderen door te beperken, af te dwingen, controle of bepaalde acties definiëren. 

- Beleid is een **toestaan** van het systeem. U bepalen acties door te definiëren en beleid toewijzen aan resources die weigeren of acties op bronnen controleren.
- Beleid worden beschreven door de beleidsdefinities in een beleid definition language (als-dan-voorwaarden).
- U maakt beleid met JSON (Javascript Object Notation) opgemaakte bestanden. Na het definiëren van een beleid voor u deze toewijzen aan een bepaald bereik: abonnement, resourcegroep. of resource.

Beleidsregels hebben meerdere acties waarmee een toegesneden aanpak voor uw scenario's. De acties zijn:

-   **Weigeren**: de aanvraag worden geblokkeerd
-   **Controle**: kunnen aanvragen, maar wordt een lijn toegevoegd aan het logboek voor faxactiviteit (die kan worden gebruikt om waarschuwingen of voor het starten van runbooks)
-   **Toevoegen**: opgegeven gegevens worden toegevoegd aan de bron. Als er geen label voor een 'CostCenter' op een resource, bijvoorbeeld dat label met een standaardwaarde toevoegen.

### <a name="common-uses-of-resource-manager-policies"></a>Gemeenschappelijk gebruik van Resource Manager-beleid

Azure Resource Manager-beleid is een krachtig hulpmiddel in de Azure toolkit. Hiermee kunt u voorkomen dat onverwachte kosten een kostenplaats voor bronnen via codering te identificeren en om ervoor te zorgen dat in overeenstemming is eisen wordt voldaan. Wanneer beleid worden gecombineerd met de ingebouwde functies voor controle, kunt u complexe en flexibele oplossingen fashion. Beleidsregels kunnen bedrijven voor besturingselementen van 'traditionele IT-werklasten en werkbelasting "Agile"; zoals ontwikkelen klanttoepassingen. De meest voorkomende patronen we voor een beleid zien zijn:

- **Soevereiniteit Geo-Nalevingsgegevens** - Azure biedt de regio's over de hele wereld. Ondernemingen vaak zelf wil bepalen wanneer resources worden gemaakt (of om gegevens-soevereiniteit of gewoon zorgen bronnen dicht bij de consument einde van de resources worden gemaakt).
- **Kostenbeheer** - abonnement een Azure bronnen van veel typen en schaal kan bevatten. Bedrijven willen vaak om ervoor te zorgen dat standaard abonnementen niet onnodig groot bronnen die honderden dollars per maand of meer kost.
- **Governance door middel van de vereiste labels standaard** - tags vereisen, is een van de meest voorkomende en zeer gewenste functies. Werken met beleid voor Azure Resource Manager zijn ondernemingen in staat om ervoor te zorgen dat een resource op de juiste manier is gelabeld. De meest voorkomende tags zijn: afdeling, de eigenaar van de Resource en omgeving-type (bijvoorbeeld - productie, testen en ontwikkeling)

**Voorbeelden**

"Traditionele IT ' abonnement van branchespecifieke toepassingen

-   Afdeling afdwingen en de eigenaar van alle bronnen van tags
-   Resource maken op de Noord-Amerikaanse regio beperken
-   Beperkt de mogelijkheid om de G-serie VMs en HDInsight Clusters maken

"Flexibele" omgeving voor een business unit maken van cloud-toepassingen

- Toestaan dat resources gemaakt om te voldoen aan de eisen inzake gegevens-soevereiniteit, alleen in een bepaalde regio.
- Omgeving-code op alle resources afdwingen. Toevoegen als een resource wordt gemaakt zonder een label, de **omgeving: onbekende** tag aan de resource.
- Controle wanneer resources worden gemaakt buiten Noord-Amerika, maar niet belemmeren.
- Controle bij hoge kosten voor resources worden gemaakt.

> [AZURE.TIP]
>
> Het meest voorkomende gebruik van Resource Manager beleid tussen verschillende organisaties is om te bepalen *waar* de resources kunnen worden gemaakt en *welke* typen resources kunnen worden gemaakt. Naast besturingselementen op *waar* en *welke*biedt u vele ondernemingen beleid om middelen zijn met de juiste metagegevens weer voor verbruik in rekening. Wij raden aan om het beleid op het niveau van het abonnement voor:
>
> - Soevereiniteit geo-Nalevingsgegevens
> - Kosten management
> - Vereiste labels (bepaald door de zakelijke behoeften, zoals BillTo, de eigenaar van toepassing)
>
> U kunt extra beleidsregels op lagere niveaus van een bereik toepassen.
 
### <a name="audit---what-happened"></a>Audit - wat is er gebeurd?

Als u wilt zien hoe uw omgeving werkt, moet u controleren van gebruikersactiviteiten. De meeste resourcetypen in Azure maken diagnostische logboeken die u via een log-hulpprogramma of in Azure Operations Management Suite analyseren kunt. U kunt Logboeken voor faxactiviteit verzamelen over meerdere abonnementen voor een afdeling of de weergave van de onderneming. Audit records zijn zowel een belangrijk diagnostisch hulpmiddel en een cruciaal mechanisme voor trigger-gebeurtenissen in de Azure-omgeving.

Logboeken voor faxactiviteit van bronbeheer implementaties kunnen u bepalen de **bewerkingen** die vond plaats en wie welke uitgevoerd. Logboeken voor faxactiviteit kunnen worden verzameld en samengevoegd met behulp van hulpprogramma's zoals Analytics logboek.

## <a name="resource-tags"></a>Bron codes

Als gebruikers in uw organisatie resources aan het abonnement toevoegen, wordt het steeds belangrijker om resources koppelen aan de juiste afdeling, klant en omgeving. U kunt metagegevens toevoegen aan bronnen via [tags](resource-group-using-tags.md). Met tags kunt u informatie over de resource of de eigenaar. Tags kunnen u niet alleen statistische en bronnen op verschillende manieren groeperen, maar die gegevens gebruikt voor de doeleinden van de terugboeking. U kunt resources met maximaal 15 key: value-paren labelen. 

Resource-labels zijn flexibel en moeten worden gekoppeld aan de meeste bronnen. Voorbeelden van veelgebruikte bron codes zijn:

- BillTo
- Afdeling (of Business Unit)
- Omgeving (productie, Stage, ontwikkeling)
- Laag (Web Tier, Tier toepassing)
- De eigenaar van toepassing
- Projectnaam

![tags](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Zie voor meer voorbeelden van codes [naamgevingsconventies voor Azure resources aanbevolen](./guidance/guidance-naming-conventions.md).

> [AZURE.TIP]
>
> Maak een tagging strategie waarmee via uw abonnementen welke metagegevens nodig is voor de business, finance, beveiliging, risicobeheer en algemeen beheer van het milieu. Maak daarom een beleid dat labelen verplicht voor:
>
> - Resourcegroepen
> - Opslag
> - Virtuele Machines
> - Application Service omgevingen/webservers

## <a name="resource-group"></a>Resourcegroep 

Resource Manager kunt u bronnen plaatsen in zinvolle groepen voor beheer van facturering of natuurlijke affiniteit. Zoals eerder vermeld, heeft Azure twee implementatiemodellen. In het eerdere model Klassiek is de basiseenheid voor het beheer van het abonnement. Was het moeilijk om op te splitsen bronnen binnen een abonnement, die heeft geleid tot het maken van een groot aantal abonnementen. Met het model Resource Manager hebt gezien de invoering van de resource. Resourcegroepen zijn containers voor bronnen die de levensduur van een gemeenschappelijke of een kenmerk zoals 'alle SQL-servers delen of ' A '.

Resourcegroepen kunnen niet worden opgenomen in elkaar en de bronnen kunnen slechts aan één resourcegroep. U kunt bepaalde acties toepassen op alle resources in een resourcegroep. Bijvoorbeeld, een groep verwijdert, wordt alle resources in de resourcegroep. Meestal plaatsen u een volledige toepassing of verwante systeem in dezelfde bronnengroep. Een drie-tier toepassing met de naam Contoso webtoepassing bevat bijvoorbeeld de webserver, toepassingsserver en de SQL server in dezelfde bronnengroep.

> [AZURE.TIP]
>
> Hoe het organiseren van uw resourcegroepen kan afwijken van de "traditionele IT-werklasten om 'flexibele IT-werklasten
>
> - "Traditionele IT ' werklasten worden meestal gegroepeerd op items binnen de dezelfde levenscyclus, zoals een toepassing. Groeperen door toepassing kunt voor afzonderlijke toepassingen beheren.
> - "Flexibele IT ' werklasten richt veelal op externe klantgerichte cloud-toepassingen. De lagen van de implementatie (zoals Web Tier, App Tier) moeten overeenkomen met de resourcegroepen en beheer.

## <a name="role-based-access-control"></a>Toegangsbeheer op basis van rollen

Waarschijnlijk vraagt u uzelf "wie moet toegang hebben tot bronnen?" en 'Hoe bepaal ik deze toegang uit?' Toestaan of verbieden van de toegang tot de portal Azure en toegang tot bronnen in de portal beheren is van cruciaal belang. 

Toen Azure in eerste instantie werd uitgebracht, toegangscontrole op een abonnement basic zijn: beheerder of CO-beheerder. Toegang tot een abonnement in de klassieke model impliciet toegang tot alle bronnen in de portal. Dit gebrek aan nauwgezette controle leidde tot de verspreiding van abonnementen tot een bepaald niveau van redelijke toegangsbeheer voor een inschrijving Azure.

De verspreiding van abonnementen is niet meer nodig. Met op functies gebaseerd toegangsbeheer kunt u gebruikers toewijzen aan de standaardrollen (bijvoorbeeld 'Lezer' en 'schrijver' soorten rollen). U kunt ook aangepaste rollen definiëren.

> [AZURE.TIP]
>  
> - Verbinding Azure Active Directory met behulp van het hulpprogramma verbinding maken met AD uw bedrijfsidentiteit winkel (meest Active Directory).
> - De Admin/Co-beheerder van een abonnement met behulp van een beheerde identiteit besturen. **Geen** toewijzen Co-Admin-beheerder aan de eigenaar van een nieuw abonnement. In plaats daarvan gebruik RBAC-rollen **eigenaar** rechten aan een groep of individueel.
> - Azure gebruikers toevoegen aan een groep (bijvoorbeeld toepassing X eigenaren) in Active Directory. Gebruik de gesynchroniseerde groep leden van de groep de juiste rechten voor het beheren van de bronnengroep met de toepassing.
> - Volg het principe van de toekenning van de **laagste bevoegdheid** nodig is voor de verwachte werkzaamheden. Bijvoorbeeld:
>     - Implementatie-groep: Een groep die zich alleen bij het implementeren van bronnen.
>     - Beheer virtuele Machine: Een groep opnieuw starten VMs (voor bewerkingen)

## <a name="azure-resource-locks"></a>Azure resource sloten

Als uw organisatie kernservices van het abonnement invoert, wordt het steeds belangrijker om ervoor te zorgen dat deze diensten beschikbaar zijn voor verstoring van de zakelijke voorkomen. [Vergrendelingen voor resources](resource-group-lock-resources.md) kunt u voor het beperken van bewerkingen van hoogwaardige bronnen waar wijzigt of verwijdert ze een belangrijke invloed op toepassingen of cloud infrastructuur zou hebben. U kunt vergrendelingen toepassen op een abonnement, resource of resourcegroep. Meestal toepassen u vergrendelingen op basissysteem bronnen, zoals virtuele netwerken en gateways opslag rekeningen. 

Vergrendelingen van de resource momenteel ondersteuning voor twee waarden: CanNotDelete en alleen-lezen. CanNotDelete houdt in dat gebruikers (met de juiste machtigingen) kunnen nog steeds lezen of wijzigen van een resource, maar kan niet worden verwijderd. Alleen-lezen betekent dat gemachtigde gebruikers niet verwijderen of wijzigen van een resource.

Maken of verwijderen van vergrendelingen beheren, hebt u toegang tot `Microsoft.Authorization/*` of `Microsoft.Authorization/locks/*` acties.
Van de ingebouwde functies, alleen de eigenaar en beheerder toegang verleend deze acties.

> [AZURE.TIP] Core netwerkopties moeten worden beveiligd met vergrendelingen. Per ongeluk verwijderen van een gateway VPN van site naar site worden voor een abonnement Azure rampzalige. Azure kunt u niet verwijderen van een virtueel netwerk dat wordt gebruikt, maar meer beperkingen toe te passen, is een nuttige voorzorgsmaatregel. Beleid zijn ook essentieel voor de handhaving van de juiste besturingselementen. Wij raden aan dat u een **CanNotDelete** vergrendeling beleid die in gebruik zijn van toepassing.
>
> - Virtueel netwerk: CanNotDelete
> - Network Security Group: CanNotDelete
> - Beleid: CanNotDelete

## <a name="core-networking-resources"></a>Core networking-bronnen

Toegang tot bronnen kan worden intern (binnen het bedrijfsnetwerk) of extern (via het internet). Het is gemakkelijk voor gebruikers in uw organisatie resources per ongeluk in de verkeerde plaats plaatsen en blootstellen aan schadelijke toegang. Net als bij de lokale apparaten, ondernemingen moeten passende besturingselementen toevoegen om te Azure gebruikers moet ervoor zorgen dat de juiste keuzes. Voor abonnement governance, we core resources identificeren die basisbesturingselementen voor toegang bieden. De belangrijkste resources bestaan uit:

- **Virtuele netwerken** zijn de container-objecten voor subnetten. Hoewel niet strikt noodzakelijk is, wordt het vaak gebruikt als toepassingen op interne bedrijfsnetwerken te verbinden.
- **Netwerk-beveiligingsgroepen** zijn vergelijkbaar met een firewall en bevatten regels voor de manier waarop een resource kan "praten" via het netwerk. Ze bieden gedetailleerde controle over hoe u eventueel een subnet (of virtuele machines) verbinding met Internet of andere subnetten in hetzelfde virtuele netwerk maken kunnen.

![Core networking](./media/resource-manager-subscription-governance/core-network.png)

> [AZURE.TIP]
>  
> - Maak virtuele netwerken gewijd aan de werkbelasting van de externe en interne werkbelasting. Deze methode verkleint u de kans van virtuele machines die bestemd zijn voor interne werklasten in een extern gerichte ruimte per ongeluk te brengen.
> - Netwerk-beveiligingsgroepen zijn essentieel voor deze configuratie. Ten minste een toegang tot het internet blokkeren van interne virtuele netwerken en blokkeert de toegang tot het bedrijfsnetwerk externe virtuele netwerken.

### <a name="automation"></a>Automatisering

Bronnen beheren afzonderlijk is tijdrovend en mogelijk gevoelig voor bepaalde bewerkingen. Azure biedt diverse mogelijkheden voor het automatiseren Azure automatisering, logica Apps en functies Azure. [Azure automatisering](./automation/automation-intro.md) kunnen beheerders maken en runbooks voor het verwerken van algemene taken bij het beheren van resources definiëren. U kunt runbooks maken met behulp van een PowerShell code-editor of een grafische editor. U kunt meerdere fasen werkstromen produceren. Azure automatisering wordt vaak gebruikt voor het verwerken van veelvoorkomende taken, zoals het afsluiten van ongebruikte middelen of middelen in reactie op een trigger zonder menselijke tussenkomst.

> [AZURE.TIP]
>
> - Maak een account Azure automatisering en bekijk de beschikbare runbooks (grafische en command line) beschikbaar in de [Galerie met Runbook](./automation/automation-runbook-gallery.md).
> - Importeren en aanpassen van de belangrijkste runbooks voor eigen gebruik.
>
> Een gebruikelijk scenario is de mogelijkheid om virtuele machines starten en afsluiten van een schema. Er zijn een voorbeeld van de runbooks die beschikbaar zijn in de galerie die zowel in dit scenario verwerken en leren uit te vouwen.


## <a name="azure-security-center"></a>Azure Security Center

Misschien is een van de grootste blokkeringen tot vaststelling van de wolk de bezorgdheid over de beveiliging. IT-managers van risico's en afdelingen van de beveiliging moeten ervoor zorgen dat resources in Azure beveiligd zijn. 

Het [Beveiligingscentrum Azure](./security-center/security-center-intro.md) biedt een centrale weergave van de beveiligingsstatus van bronnen in de abonnementen en aanbevelingen die helpen te voorkomen dat resources ingebroken. Deze kunt meer gedetailleerde beleid (bijvoorbeeld toepassing beleid aan specifieke resourcegroepen waardoor de onderneming aan te passen hun houding aan het risico dat wordt voldaan) inschakelen. Ten slotte Azure Security is Center een open platform waarmee Microsoft-partners en onafhankelijke softwareleveranciers om software aansluit Azure Security Center voor het verbeteren van de mogelijkheden ervan te maken. 

> [AZURE.TIP]
>
> Azure Security Center is standaard ingeschakeld in elk abonnement. Echter, moet u het verzamelen van virtuele machines Azure Beveiligingscentrum de agent installeren en beginnen met het verzamelen van gegevens toestaan inschakelen.
>
> ![het verzamelen van gegevens](./media/resource-manager-subscription-governance/data-collection.png)

## <a name="next-steps"></a>Volgende stappen

- U hebt geleerd over governance abonnement, is het tijd voor een overzicht van deze aanbevelingen in de praktijk. Zie de [voorbeelden van de governance van Azure abonnement](resource-manager-subscription-examples.md).

*[Karl Kuhnhausen](https://github.com/karlkuhnhausen) bijgedragen aan dit onderwerp.*
