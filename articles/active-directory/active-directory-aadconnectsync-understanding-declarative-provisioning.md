<properties
    pageTitle="Azure AD verbinden sync: wat declaratieve inrichten | Microsoft Azure"
    description="Declaratieve provisioning configuratiemodel in Azure AD verbinding uitgelegd."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD verbinden sync: wat declaratieve inrichten
Dit onderwerp wordt uitgelegd dat het configuratiemodel in Azure AD verbinden. Het model wordt genoemd declaratieve ingericht en kunt u een configuratie gemakkelijk wijzigen. Veel dingen die in dit onderwerp worden beschreven zijn geavanceerde en niet vereist voor de meeste scenario's van klanten.

## <a name="overview"></a>Overzicht
Declaratieve ingericht afkomstig uit een bronmap verbonden objecten wordt verwerkt en bepaalt hoe het object en de kenmerken moeten worden omgezet van een bron naar een doel. Een object wordt verwerkt in een pijplijn sync en de pijpleiding is hetzelfde voor binnenkomende en uitgaande regels. Een binnenkomende regel de metaverse is uit een connector en een uitgaande regel uit de metaverse is een connector-ruimte.

![Sync-pipeline](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

De pijplijn heeft meerdere, verschillende modules. Elk is verantwoordelijk voor een begrip in object-synchronisatie.

![Sync-pipeline](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

- Bron, het bronobject
- [Scope](#scope), vindt u alle synchronisatie regels die binnen het bereik vallen
- [Deelnemen](#join), hiermee verband tussen connector ruimte en de metaverse
- [Transformeren](#transform), berekent hoe kenmerken moeten worden omgezet en flow
- [Prioriteit](#precedence), conflicterende kenmerk bijdragen worden opgelost
- Doel van het target-object

## <a name="scope"></a>Toepassingsgebied
De module scope een object wordt beoordeeld, en bepaalt de regels die binnen het bereik vallen en moeten worden opgenomen in de verwerking. Afhankelijk van de waarden van de kenmerken van het object, synchroniseren met verschillende regels in het bereik worden geëvalueerd. Een uitgeschakelde gebruiker met geen Exchange-postbus heeft bijvoorbeeld andere regels dan een ingeschakelde gebruiker met een postvak.  
![Toepassingsgebied](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

Het bereik is gedefinieerd als groepen en componenten. De componenten zijn in een groep. Een logische en wordt tussen alle componenten in een groep gebruikt. Bijvoorbeeld (afdeling = en land = Denemarken). Een logische OR wordt gebruikt tussen groepen.

![Toepassingsgebied](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
Als de scope in deze afbeelding worden gelezen (afdeling = en land = Denemarken) of (land = Zweden). Als groep 1 of groep 2 wordt geëvalueerd als true, wordt de regel is in het bereik.

De scope-module ondersteunt de volgende bewerkingen.

Bewerking | Beschrijving
--- | ---
GELIJK, NOTEQUAL | Het vergelijken van een tekenreeks die wordt geëvalueerd als de waarde gelijk is aan de waarde in het kenmerk. Voor kenmerken met meerdere waarden, Zie ISIN en ISNOTIN.
LESSTHAN, LESSTHAN_OR_EQUAL | Een tekenreeks vergelijken die wordt geëvalueerd als waarde is dan de waarde in het kenmerk.
BEVAT, NOTCONTAINS | Het vergelijken van een tekenreeks die wordt geëvalueerd als waarde kan worden gevonden ergens in de waarde in het kenmerk.
STARTSWITH, NOTSTARTSWITH | Het vergelijken van een tekenreeks die wordt geëvalueerd als de waarde in het begin van de waarde in het kenmerk.
ENDSWITH, NOTENDSWITH | Het vergelijken van een tekenreeks die wordt geëvalueerd als de waarde in het einde van de waarde in het kenmerk.
GROTER DAN, GREATERTHAN_OR_EQUAL | Het vergelijken van een tekenreeks die wordt geëvalueerd als de waarde groter is dan de waarde in het kenmerk.
ISNULL, ISNOTNULL | Als het kenmerk niet aanwezig is van het object. Als het kenmerk niet aanwezig en daarom null is, vervolgens is de regel in het bereik.
ISIN, ISNOTIN | Als de waarde in het opgegeven kenmerk is geëvalueerd. Deze bewerking is de variatie met meerdere waarden van gelijk- en NOTEQUAL. Het kenmerk moet een kenmerk met meerdere waarden zijn en als de waarde kan worden gevonden in een van de waarden van het kenmerk, de regel is in het bereik.
ISBITSET, ISNOTBITSET | Wordt geëvalueerd als een bepaalde bit is ingesteld. Zo kan worden gebruikt voor de evaluatie van de bits in userAccountControl te zien als een gebruiker is ingeschakeld of uitgeschakeld.
ISMEMBEROF, ISNOTMEMBEROF | De waarde moet een DN-naam aan een groep in de connector ruimte bevatten. Als het object een lid van de groep die is opgegeven is, wordt de regel in het bereik.

## <a name="join"></a>Join
De join-module in de pijplijn sync is verantwoordelijk voor de relatie tussen het object in de bron en een object in het doel te zoeken. Deze relatie is een binnenkomende regel voor een object in de ruimte van een verbindingslijn zoeken van een relatie met een object in de metaverse.  
![Join tussen cs en mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
Het doel is om te zien dat als een object al in de metaverse, gemaakt door een andere Connector, moet het worden gekoppeld. In een forest account resource moet bijvoorbeeld de gebruiker uit het accountforest worden gekoppeld met de gebruiker van het bronforest.

Joins worden voornamelijk in binnenkomende regels gebruikt om de connector ruimteobjecten samenvoegen naar hetzelfde object metaverse.

De joins worden gedefinieerd als een of meer groepen. In een groep, kunt u componenten hebt. Een logische en wordt tussen alle componenten in een groep gebruikt. Een logische OR wordt gebruikt tussen groepen. De groepen worden verwerkt in de volgorde van boven naar beneden. Wanneer een groep precies één overeenkomst met een object in het doeldomein gevonden heeft, worden geen andere join regels geëvalueerd. Als nul of meer dan één object wordt aangetroffen, blijft verwerking de volgende groep met regels. Daarom moeten de regels gemaakt in de volgorde van de meeste expliciete eerste en meer bij benadering aan het einde.  
![Deelnemen aan de definitie](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
De joins in deze afbeelding worden verwerkt van boven naar beneden. Eerst ziet de pijpleiding synchroniseren als er een overeenkomst is voor de werknemer-id is. Als dit niet het geval is, wordt de tweede regel ziet als de naam van de account kan worden gebruikt om de objecten samenvoegen. Als dat niet een overeenkomst ofwel, is de derde en laatste regel meer bij benadering overeenkomen met de naam van de gebruiker.

Als alle regels van de join wordt geëvalueerd en er niet precies één overeenkomst, wordt het **Type koppeling** op de pagina **Beschrijving** gebruikt. Als deze optie is ingesteld om in te **richten**, wordt een nieuw object in het doeldomein gemaakt.  
![Bepaling of een join](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Een object mag alleen één enkel sync regel met join regels hebben in het bereik. Als er meerdere regels voor synchronisatie waarbij join is gedefinieerd, treedt een fout op. Prioriteit wordt niet gebruikt join conflicten op te lossen. Een object moet een join-regel in het bereik voor kenmerken met dezelfde richting inkomende/uitgaande vloeien. Als u kenmerken stroom inkomende en uitgaande naar hetzelfde object moet, moet u zowel een binnenkomend als een regel voor uitgaande synchroniseren met join hebben.

Uitgaande join is een speciaal gedrag bij het inrichten van een object naar een doel-connector ruimte. Het kenmerk DN-naam wordt gebruikt voor de eerste keer probeert een reverse-join. Als er al een object in de ruimte van de doel-connector met de dezelfde DN, worden de objecten gekoppeld.

De join-module wordt alleen geëvalueerd wanneer wanneer een nieuwe regel voor synchronisatie in bereik komen. Wanneer een object is gekoppeld, is het niet verwijderen zelfs als de samenvoegcriteria niet meer wordt voldaan. Als u wilt dat een object Meld, moet de sync-regel die deel uitmaakt van de objecten vallen buiten bereik.

### <a name="metaverse-delete"></a>Metaverse verwijderen
Een object metaverse blijft zolang is een sync-regel in een scope met het **Type koppeling** instellen op **bepaling** of **StickyJoin**. Een StickyJoin wordt gebruikt wanneer een verbindingslijn is niet toegestaan om het inrichten van een nieuw object aan de metaverse, maar wanneer het lid geworden, deze in de bron moet worden verwijderd voordat de metaverse-object wordt verwijderd.

Wanneer een object metaverse wordt verwijderd, worden alle objecten die zijn gekoppeld aan een regel voor uitgaande sync gemarkeerd voor het **inrichten van** zijn gemarkeerd voor verwijderen.

## <a name="transformations"></a>Transformaties
De transformaties worden gebruikt om te definiëren hoe kenmerken moeten de stroming van de bron naar het doel. De stroom kunnen een van de volgende **stroomtypen**hebben: Direct, constante of expressie. Een directe stroom loopt als een waarde van het kenmerk-is met geen extra transformaties. Een constante waarde ingesteld voor de opgegeven waarde. Een expressie wordt de declaratieve taal voor sjabloonexpressies provisioning express hoe de transformatie moet worden. In het onderwerp [Wat declaratieve provisioning taal voor sjabloonexpressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) vindt u de details voor de expressietaal.

![Bepaling of een join](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

Het selectievakje **eenmaal van toepassing** wordt gedefinieerd dat het kenmerk moet alleen worden ingesteld wanneer het object oorspronkelijk is gemaakt. Deze configuratie kan bijvoorbeeld een eerste wachtwoord instellen om een nieuw object worden gebruikt.

### <a name="merging-attribute-values"></a>Samenvoegen van waarden van kenmerk
In de stromen kenmerk is een instelling om te bepalen als kenmerken met meerdere waarden moeten worden samengevoegd uit verschillende andere Connectors. De standaardwaarde is de **Update**, waarmee wordt aangegeven dat de regel van de synchronisatie met de hoogste prioriteit moet winnen.

![Typen samenvoegen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

Er is ook **samenvoegen** en **MergeCaseInsensitive**. Met deze opties kunt u waarden uit verschillende bronnen samenvoegen. Het kan bijvoorbeeld worden gebruikt het kenmerk lid of proxyAddresses uit diverse verschillende forests samenvoegen. Wanneer u deze optie gebruikt, moeten alle regels in het bereik van een object voor synchronisatie hetzelfde type samenvoeging gebruiken. U kunt geen **Update** van een Connector en **samenvoegen** van de andere definiëren. Als u probeert foutbericht u een.

Het verschil tussen **samenvoegen** en **MergeCaseInsensitive** is het verwerken van dubbele waarden. De synchronisatie-engine wordt gecontroleerd of dubbele waarden niet worden ingevoegd in het kenmerk target. Met **MergeCaseInsensitive**, dubbele waarden met alleen een verschil in het geval zullen aanwezig zijn. Bijvoorbeeld niet ziet u beide "SMTP:bob@contoso.com" en "smtp:bob@contoso.com" in het target-kenmerk. **Samenvoegen** wordt alleen kijken naar de exacte waarden en meerdere waarden wanneer er alleen een verschil in kwestie aanwezig kan zijn.

De optie **vervangen** is hetzelfde als de **Update**echter niet wordt gebruikt.

### <a name="control-the-attribute-flow-process"></a>Het kenmerk stroom proces beheren
Wanneer u meerdere regels voor inkomende synchronisatie bij te dragen tot hetzelfde kenmerk metaverse zijn geconfigureerd, wordt voorrang gebruikt om te bepalen van de winnaar. Contribute de waarde gaat de regel synchroniseren met de hoogste prioriteit (de laagste numerieke waarde). Hetzelfde gebeurt voor uitgaande regels. Synchronisatie van de regel met de hoogste prioriteit wins en bijdragen van de waarde in de gekoppelde map.

In sommige gevallen, in plaats van een waarde, dragen moet de sync-regel bepalen hoe andere regels gedragen. Er zijn enkele speciale letterlijke waarden gebruikt voor deze kwestie.

Voor binnenkomende regels voor synchronisatie, kan de letterlijke **NULL** men om aan te geven dat de gegevensstroom heeft geen waarde bij te dragen. Een andere regel met een lagere prioriteit kan bijdragen een waarde. Als er geen regel een waarde heeft, wordt het kenmerk metaverse verwijderd. Voor een uitgaande regel als **NULL** de uiteindelijke waarde is nadat alle synchronisatie regels zijn verwerkt, wordt worden de waarde verwijderd in de gekoppelde map.

De letterlijke **AuthoritativeNull** is gelijk aan **NULL** , maar met het verschil dat er geen lagere prioriteitsregels een waarde kunnen bijdragen.

Een kenmerk stroom kunt ook **IgnoreThisFlow**gebruiken. Lijkt op NULL in die zin dat betekent dit dat er niets bij te dragen. Het verschil is dat wordt niet verwijderd een reeds bestaande waarde in het doel. Het is net als de stroom kenmerk is er nog nooit geweest.

Hier volgt een voorbeeld:

In *Out naar AD - gebruiker Exchange hybride* kunt de volgende stroom vinden:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Deze expressie moet worden gelezen als: als het postvak in Azure AD, vervolgens het kenmerk van Azure AD-AD stromen. Als dat niet het geval is, terug naar Active Directory niet doorlopen. In dit geval zou het de bestaande waarde in AD behouden.

### <a name="importedvalue"></a>ImportedValue
De functie ImportedValue is anders dan alle andere functies, omdat de naam van het kenmerk moet tussen aanhalingstekens in plaats van vierkante haken:  
`ImportedValue("proxyAddresses")`.

Een kenmerk gebruikt tijdens de synchronisatie meestal de verwachte waarde, zelfs als deze nog niet is geëxporteerd of een fout is ontvangen tijdens het exporteren ("top van de toren"). Een inkomende synchronisatie wordt ervan uitgegaan dat een kenmerk dat nog niet is bereikt een gekoppelde map uiteindelijk het bereikt. In sommige gevallen is het belangrijk dat u alleen een waarde die is bevestigd door de verbonden directory ("hologram en delta tower importeren") worden gesynchroniseerd.

Een voorbeeld van deze functie kunt u vinden in de out-of-box-synchronisatie regel *In uit Active Directory – algemene gebruiker van Exchange*. In Exchange hybride, moet de waarde die is toegevoegd door Exchange online alleen worden gesynchroniseerd als deze is bevestigd dat de waarde is geëxporteerd:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Prioriteit
Wanneer verschillende regels voor sync bij te dragen aan het doel de dezelfde waarde van het kenmerk, wordt de prioriteit gebruikt om te bepalen van de winnaar. De regel met de hoogste prioriteit, de laagste numerieke waarde, zal contribute het kenmerk in een conflict.

![Typen samenvoegen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

Deze volgorde kan men definiëren nauwkeuriger kenmerk stromen voor een kleine subset van objecten. Bijvoorbeeld zorg de uit-van-vak-regels dat de kenmerken van een ingeschakelde account (**Gebruiker AccountEnabled**) voorrang van andere accounts hebben.

Prioriteit kan worden gedefinieerd tussen verbindingslijnen. Waarmee u verbindingslijnen met betere gegevens waarden eerst bijdragen.

### <a name="multiple-objects-from-the-same-connector-space"></a>Meerdere objecten van dezelfde ruimte connector
Als u meerdere objecten in dezelfde connector-ruimte die deel uitmaakt van hetzelfde object metaverse, moet voorrang worden aangepast. De synchronisatie-engine is niet kunnen bepalen voorrang als meerdere objecten in een bereik van de regel met dezelfde synchronisatie. Is het onduidelijk welke bronobject moet bijdragen tot de waarde in de metaverse. Deze configuratie wordt gerapporteerd als dubbelzinnig, zelfs als de kenmerken in de bron dezelfde waarde hebben.  
![Meerdere objecten die zijn gekoppeld aan hetzelfde object mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

In dit scenario moet u de werkingssfeer van de regels voor synchronisatie wijzigen zodat de bronobjecten sync verschillende regels in het bereik. Waarmee u verschillende voorrang te definiëren.  
![Meerdere objecten die zijn gekoppeld aan hetzelfde object mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Volgende stappen

- Lees meer over de taal voor sjabloonexpressies in [Wat declaratieve inrichten van expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
- Zie hoe declaratieve inrichting gebruikte out-of-box inzicht te verwerven in [de standaardconfiguratie](active-directory-aadconnectsync-understanding-default-configuration.md)wordt.
- Zie praktische wijzigen met behulp van declaratieve ingericht in [het maken van een wijziging in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md).
- Verder lezen hoe gebruikers en contactpersonen samen in [wat gebruikers en contactpersonen werken](active-directory-aadconnectsync-understanding-users-and-contacts.md).

**Van overzichtsonderwerpen**

- [Azure AD verbinden sync: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)

**Naslagonderwerpen**

- [Azure AD verbinden sync: functies, naslag](active-directory-aadconnectsync-functions-reference.md)
