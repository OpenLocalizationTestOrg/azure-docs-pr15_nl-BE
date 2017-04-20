<properties
   pageTitle="Gegevens in DocumentDB met TTL verlopen | Microsoft Azure"
   description="Met TTL biedt Microsoft Azure DocumentDB de mogelijkheid om documenten automatisch verwijderd uit het systeem na een periode van tijd."
   services="documentdb"
   documentationCenter=""
   keywords="TTL"
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2016"
   ms.author="kipandya"/>

# <a name="expire-data-in-documentdb-collections-automatically-with-time-to-live"></a>Gegevens in de collecties van de DocumentDB automatisch met TTL verlopen

Toepassingen kunnen maken en opslaan van grote hoeveelheden gegevens. Sommige van deze gegevens, zoals de computer gegenereerde gegevens, logboeken en gebruiker logboekregistratiesessie informatie is alleen nuttig voor een beperkte periode. Zodra de gegevens wordt overtollige aan de behoeften van de toepassing is het veilig verwijderen van deze gegevens en vermindert de vraag naar de opslag van een toepassing.

Met "time to live" of TTL, biedt Microsoft Azure DocumentDB de mogelijkheid om documenten automatisch verwijderd uit de database na een periode van tijd. De standaardtijd voor live kunt instellen op het niveau en op basis van per document overschreven. Wanneer TTL is ingesteld als een standaard collectie of op het documentniveau van een, wordt DocumentDB automatisch verwijderen documenten die bestaan na die periode van tijd, in seconden, omdat ze het laatst zijn gewijzigd.

TTL in DocumentDB gebruikt een verschuiving ten opzichte van het document het laatst is gewijzigd. Hiervoor die wordt het veld _ts dat bestaat in elk document gebruikt. Het veld _ts is een unix-stijl epoch tijdstempel die de datum en tijd. Het _ts-veld wordt bijgewerkt telkens wanneer een document is gewijzigd. 

## <a name="ttl-behavior"></a>TTL-gedrag

De TTL-functie wordt bepaald door de eigenschappen op twee niveaus - het niveau en het niveau van de TTL-waarde. De waarden worden ingesteld in seconden en worden behandeld als een delta van de _ts die het document het laatst is gewijzigd op.

 1.  DefaultTTL voor de collectie
  * Als ontbreekt (of is ingesteld op null) documenten worden niet automatisch verwijderd.
  
  * Indien aanwezig en de waarde '-1' = oneindig – documenten standaard verloopt niet
  
  * Indien aanwezig en de waarde is een getal ("n") – documenten verlopen seconden "n" na de laatste wijziging

 2.  TTL-waarde van de documenten: 
  * De eigenschap is alleen van toepassing als DefaultTTL voor de bovenliggende collectie aanwezig is.
  
  * Heft de DefaultTTL waarde voor de bovenliggende collectie.

Zodra het document is verlopen (ttl + _ts > = huidige servertijd), het document wordt gemarkeerd als "verlopen". Na deze tijd geen bewerking is toegestaan op deze documenten en ze worden uitgesloten van de resultaten van de query's uitgevoerd. Documenten fysiek in het systeem worden verwijderd en worden verwijderd op de achtergrond optioneel wordt op een later tijdstip. Dit verbruikt geen eventuele [Vragen eenheden (RUs)](documentdb-request-units.md) uit de begroting van de collectie.

De bovenstaande logica kan worden weergegeven in de volgende matrix:

|       | DefaultTTL ontbreekt of niet is ingesteld op de collectie | DefaultTTL = -1 collectie | DefaultTTL = "n" collectie|
| ------------- |:-------------|:-------------|:-------------|
| TTL ontbreekt op document| Niets op documentniveau negeren aangezien het document en de collectie geen concept van de TTL-waarde hebben. | Er zijn geen documenten in deze verzameling verloopt. | De documenten in deze verzameling verloopt als n interval is verstreken. |
| TTL = -1 op document | Niets om op te heffen op het niveau van sinds de collectie bevat geen definitie van de eigenschap DefaultTTL die een document kunt vervangen. TTL-waarde voor een document is niet geïnterpreteerd door het systeem. | Er zijn geen documenten in deze verzameling verloopt. | Het document met TTL =-1 in deze verzameling verloopt nooit. Alle andere documenten verloopt na "n" interval. |
|  TTL = n op document | Er is niets te negeren op het niveau. TTL-waarde in een document in niet geïnterpreteerd door het systeem. | Het document met TTL = n verloopt na n interval in seconden. Andere documenten zal overnemen, interval-1 en nooit verloopt. | Het document met TTL = n verloopt na n interval in seconden. Andere documenten worden "n" interval overgenomen uit de collectie. |


## <a name="configuring-ttl"></a>TTL configureren

TTL is standaard uitgeschakeld in alle collecties van DocumentDB en op alle documenten.

## <a name="enabling-ttl"></a>TTL inschakelen

TTL op een collectie of de documenten binnen een collectie is ingeschakeld, moet u de eigenschap DefaultTTL van een collectie ingesteld op -1 of een positief getal dan nul. Als u de DefaultTTL op-1, die door de DocumentDB, maar alle documenten in de collectie wordt altijd live standaard service moet worden gecontroleerd door deze collectie voor documenten die deze standaardinstelling hebt overschreven.

## <a name="configuring-default-ttl-on-a-collection"></a>Standaard-TTL voor een collectie configureren

U zijn kunt configureren een standaardtijd voor live op een niveau. 

De TTL-waarde instellen voor een collectie, moet u een niet-nul positief getal dat aangeeft van de periode, in seconden, alle documenten in de collectie nadat de tijdstempel voor laatste gewijzigd van het document (_ts) verloopt.

Of u kunt de standaard ingesteld op -1, wat betekent alle documenten dat in de aan de collectie ingevoegd voor onbepaalde tijd blijft bestaan standaard.

## <a name="setting-ttl-on-a-document"></a>De instelling TTL-waarde voor een document

Naast het instellen van een standaard-TTL voor een collectie kunt u specifieke TTL-waarde instellen op het documentniveau van een. Dit overschrijft de standaardinstelling van de collectie.

De TTL-waarde instellen op een document, moet u een positief getal dan nul geeft aan de periode, in seconden, verloopt het document na de laatste gewijzigde tijdstempel van het document (_ts).

Stel om deze verschuiving verstrijken, stellen het TTL-veld in het document.

Als een document geen TTL-veld is, wordt de standaardwaarde van de collectie van toepassing zijn.

Als TTL is uitgeschakeld op het niveau van wordt het TTL-veld in het document genegeerd totdat de TTL-waarde weer is ingeschakeld op de collectie.


## <a name="extending-ttl-on-an-existing-document"></a>TTL uitbreiden op een bestaand document

U kunt de TTL voor een document herstellen door een bewerking van het schrijven van het document. Hierdoor wordt de _ts ingesteld op de huidige tijd en begint het aftellen naar het document verstrijken, zoals ingesteld door de TTL-waarde opnieuw.

Als u wilt dat de TTL-waarde van een document wijzigen, kunt u het veld bijwerken zoals u met een Instelbaar veld doen kunt.


## <a name="removing-ttl-from-a-document"></a>TTL-waarde verwijderen uit een document

Als u niet langer wilt dat document verloopt een TTL-waarde is ingesteld op een document kunt vervolgens u het document ophalen, het TTL-veld verwijderen en vervangen het document op de server.

Als het TTL-veld wordt verwijderd uit het document, kunt u de standaardinstelling van de collectie wordt toegepast.

Als u wilt stoppen met een document verloopt en niet wordt overgenomen uit de collectie moet u de TTL-waarde ingesteld op -1.


## <a name="disabling-ttl"></a>TTL uitschakelen

Om te schakelen van TTL volledig op een collectie en de achtergrond stoppen van verlopen documenten zoekt u de eigenschap DefaultTTL in de collectie moeten worden verwijderd.

Als u deze eigenschap verschilt van de eigenschap instelt op -1. Instelling op-1 betekent dat nieuwe documenten die worden toegevoegd aan de collectie wordt altijd live, maar u kunt deze waarschuwing negeren voor specifieke documenten in de collectie.

Deze eigenschap verwijderen uit de collectie geheel betekent dat er geen documenten verlopen, zelfs als er zijn documenten die vorige standaard expliciet is overschreven.


## <a name="faq"></a>FAQ

**Wat TTL kost me?**

Er is geen extra kosten aan het instellen van een TTL-waarde voor een document.

**Hoe lang duurt het voordat mijn document verwijderen zodra de TTL-waarde is?**

De betreffende documenten gemarkeerd als niet beschikbaar als het document is verlopen (ttl + _ts > = huidige servertijd). Na deze tijd geen bewerking is toegestaan op deze documenten en ze worden uitgesloten van de resultaten van de query's uitgevoerd. De documenten zijn fysiek verwijderd door het systeem op de achtergrond. Dit zal niet elke RUs uit de begroting van de collectie verbruiken.

**Als het duurt enige tijd om de documenten te verwijderen, ze meetellen voor mijn TARGET (en factuur) totdat ze verwijderd?**

Zodra het document is verlopen niet, u niet gefactureerd voor de opslag van deze documenten en de grootte van de documenten niet meetellen voor de opslaglimiet voor een verzameling.

**Zal TTL voor een document geen invloed hebben op toeslagen RU**

Nee, er worden geen invloed op kosten van de RU voor bewerkingen die worden uitgevoerd op elk document in de DocumentDB.

**Wordt het verwijderen van documenten effect op de doorvoer die ik op Mijn verzameling hebt ingericht?**

Nee, voldoen aan aanvragen tegen uw verzameling ontvangt prioriteit boven de achtergrond gestart om uw documenten te verwijderen. TTL-waarde toe te voegen aan een document is niet van invloed op dit.

**Wanneer een document verloopt, hoe lang deze blijft in Mijn collectie totdat deze wordt verwijderd?**

Zodra het document is verlopen is het niet meer worden gebruikt. De precieze tijd een document blijft in uw verzameling daadwerkelijk verwijderd worden gebaseerd op wanneer de achtergrond wordt uitgevoerd, kan het document verwijderen en niet-deterministisch is.

**Verlopen documenten worden verwijderd op alle knooppunten, of is het "uiteindelijk consistent?"**

Het document wordt niet meer beschikbaar op hetzelfde moment op alle knooppunten en in alle regio's.

**Is er een RU kosten voor TTL controle achtergrondtaken?**

Nee, er is geen kosten RU voor deze.

**Hoe vaak worden de TTL verlopen gecontroleerd?**

TTL verlopen voor controle gebeurt niet als achtergrond. Bij het reageren op een verzoek wordt de backend-service kan de inline controles en uitsluiten van alle documenten die zijn verlopen. Het verwijderen van het fysieke document is het enige proces dat asynchroon op de achtergrond wordt uitgevoerd. De frequentie van dit proces wordt bepaald door de RUs beschikbaar in de collectie.

**De TTL-functie alleen geldt voor hele documenten of kan ik eigenschapswaarden afzonderlijk document verloopt?**

TTL-waarde is van toepassing op het gehele document. Als u slechts een gedeelte van een document laten verlopen wilt, wordt aanbevolen dat u Pak het gedeelte van het hoofddocument in een afzonderlijk document voor het "gekoppelde" en vervolgens TTL op dat document wordt uitgepakt.

**Beschikt over de functie TTL indexing specifieke eisen?**

Ja. De collectie moet [indexeren beleid ingesteld](documentdb-indexing-policies.md) op Lazy of Consistent. Bij het instellen van DefaultTTL in een collectie met indexeren is ingesteld op None resulteert in een fout op als het uitschakelen van de indexering op een verzameling met een DefaultTTL al ingesteld.


## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie over Azure DocumentDB, naar de pagina met [*documentatie*](https://azure.microsoft.com/documentation/services/documentdb/) .




