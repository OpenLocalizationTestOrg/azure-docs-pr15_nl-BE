<properties
    pageTitle="Azure AD verbinden sync: functies, naslag | Microsoft Azure"
    description="Verwijzing van declaratieve provisioning expressies in Azure AD verbinden sync."
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
    ms.date="08/23/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD verbinden sync: functies, naslag

Functies worden gebruikt in Azure AD verbinding maken, een waarde van het kenmerk bewerken tijdens de synchronisatie.  
De syntaxis van de functies wordt uitgedrukt in de volgende notatie:  
`<output type> FunctionName(<input type> <position name>, ..)`

Als een functie wordt overbelast en syntaxis van meerdere accepteert, worden alle geldige syntaxis weergegeven.  
De functies worden sterk getypeerd en ze controleren dat het doorgegeven type komt overeen met het type beschreven.  
Als het type niet overeenkomt, wordt een fout gegenereerd.

De typen worden uitgedrukt met de volgende syntaxis:

- **opslaglocatie** : Binary
- **bool** – Boolean
- **dt** -UTC-datum/tijd
- **enum** -opsomming van bekende-constanten zijn
- **exp** -expressie die een Boolean-waarde als resultaat wordt verwacht
- **mvbin** – binaire meerdere waarden
- **mvstr** : String met meerdere waarden
- **mvref** – verwijzing met meerdere waarden
- **num** – numerieke
- **ref** : referentie
- **str** -tekenreeks
- **var** – een variant van het (bijna) een ander type
- **void** – geen waarde als resultaat

De functies met de typen **mvbin**, **mvstr**en **mvref** kunnen alleen werken op kenmerken met meerdere waarden. **Opslaglocatie** **str**en **ref** -functies werken op kenmerken met één waarde zowel met meerdere waarden.

## <a name="functions-reference"></a>Functies, naslag

Lijst met functies | | | | |  
--------- | --------- | --------- | --------- | --------- | ---------
**Conversie** |  
[CBool](#cbool) | [CDate](#cdate) | [CGuid](#cguid) | [ConvertFromBase64](#convertfrombase64)
[ConvertToBase64](#converttobase64) | [ConvertFromUTF8Hex](#convertfromutf8hex) | [ConvertToUTF8Hex](#converttoutf8hex) | [CNum](#cnum)
[CRef](#cref) | [CStr](#cstr) | [StringFromGuid](#StringFromGuid) | [StringFromSid](#stringfromsid)
**Datum / tijd** |  
[DateAdd](#dateadd) | [DateFromNum](#datefromnum) | [FormatDateTime](#formatdatetime) | [Nu](#now)
[NumFromDate](#numfromdate) |  
**Directory** |  
[DNComponent](#dncomponent) | [DNComponentRev](#dncomponentrev) | [EscapeDNComponent](#escapedncomponent)
**Evaluatie** |  
[IsBitSet](#isbitset) | [IsDate](#isdate) | [IsEmpty](#isempty) | [IsGuid](#isguid)
[IsNull](#isnull) | [IsNullOrEmpty](#isnullorempty) | [IsNumeric](#isnumeric) | [IsPresent](#ispresent) |
[IsString](#isstring) |  
**Wiskunde** |  
[BitAnd](#bitand) | [BitOr](#bitor) | [RandomNum](#randomnum)
**Met meerdere waarden** |  
[Bevat](#contains) | [Aantal](#count) | [Artikel](#item) | [ItemOrNull](#itemornull)
[Join](#join) | [RemoveDuplicates](#removeduplicates) | [Splitsen](#split) |
**Programmaverloop** |  
[Fout](#error) | [IIF](#iif)  | [Switch](#switch)
**Tekst** |  
[GUID](#guid) | [InStr](#instr) | [InStrRev](#instrrev) | [LCase](#lcase)
[Links](#left) | [Len](#len) | [LTrim](#ltrim) | [Mid](#mid)
[PadLeft](#padleft) | [PadRight](#padright) | [PCase](#pcase) | [Vervangen](#replace)
[ReplaceChars](#replacechars) | [Rechts](#right) | [RTrim](#rtrim) | [Trim](#trim)
[UCase](#ucase) | [Word](#word)

----------
### <a name="bitand"></a>BitAnd

**Beschrijving:**  
De functie BitAnd wordt opgegeven bits ingesteld op een waarde.

**Syntaxis:**  
`num BitAnd(num value1, num value2)`

- waarde1, waarde2: functiesleutels moeten samen worden numerieke waarden

**Opmerkingen:**  
Deze functie zet beide parameters om de binaire weergave en wordt een beetje op:

- 0 - als een of beide van de overeenkomstige bits in het *masker* en de *vlag* 0 zijn
- 1 - als de overeenkomstige bits 1 zijn.

Met andere woorden, resultaat het 0 in alle gevallen, behalve wanneer de bijbehorende bits van beide parameters 1.

**Voorbeeld:**  
`BitAnd(&HF, &HF7)`  
Resulteert in 7 omdat hexadecimale ' F ' en 'F7' deze waarde als resultaat.

----------
### <a name="bitor"></a>BitOr

**Beschrijving:**  
De functie BitOr wordt opgegeven bits ingesteld op een waarde.

**Syntaxis:**  
`num BitOr(num value1, num value2)`

- waarde1, waarde2: numerieke waarden die samen worden moeten

**Opmerkingen:**  
Deze functie zet beide parameters om de binaire weergave en stelt een beetje 1 als een of beide van de overeenkomstige bits in het masker en de vlag zijn 1 en 0 als de overeenkomstige bits 0 zijn. Met andere woorden, wordt 1 in alle gevallen, behalve wanneer de bijbehorende bits van beide parameters gelijk zijn aan 0.

----------
### <a name="cbool"></a>CBool

**Beschrijving:**  
De functie CBool retourneert een Boolean-waarde op basis van de geëvalueerde expressie

**Syntaxis:**  
`bool CBool(exp Expression)`

**Opmerkingen:**  
Als resultaat van de expressie op een andere waarde dan CBool True retourneert, anders resultaat het ONWAAR.

**Voorbeeld:**  
`CBool([attrib1] = [attrib2])`  

Deze eigenschap retourneert de waarde True als beide kenmerken hebben dezelfde waarde.

----------
### <a name="cdate"></a>CDate

**Beschrijving:**  
De functie CDate geeft als resultaat een DateTime UTC uit een tekenreeks. Datum/tijd is niet een native kenmerktype synchroon, maar wordt gebruikt door sommige functies.

**Syntaxis:**  
`dt CDate(str value)`

- Waarde: Een tekenreeks met een datum, tijd, en eventueel tijdzone

**Opmerkingen:**  
De geretourneerde tekenreeks wordt altijd in UTC.

**Voorbeeld:**  
`CDate([employeeStartTime])`  
Deze eigenschap retourneert die een datum/tijd op basis van de werknemer de begintijd

`CDate("2013-01-10 4:00 PM -8")`  
Deze eigenschap retourneert een DateTime die "2013-01-11-12:00 AM"

----------
### <a name="cguid"></a>CGuid

**Beschrijving:**  
De functie CGuid omgezet de tekenreeksweergave van een GUID in de binaire weergave.

**Syntaxis:**  
`bin CGuid(str GUID)`

- Een tekenreeks die is opgemaakt in dit patroon: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx of {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

----------
### <a name="contains"></a>Bevat

**Beschrijving:**  
De functie bevat een tekenreeks binnen een kenmerk met meerdere waarden worden gevonden

**Syntaxis:**  
`num Contains (mvstring attribute, str search)`-hoofdlettergevoelige  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-hoofdlettergevoelige

- kenmerk: het kenmerk met meerdere waarden te zoeken.
- zoeken: de tekenreeks te vinden in het kenmerk.
- Casetype: CaseInsensitive of CaseSensitive.

Deze eigenschap retourneert index in het kenmerk met meerdere waarden waarin de tekenreeks is gevonden. 0 wordt geretourneerd als de tekenreeks niet wordt gevonden.

**Opmerkingen:**  
Voor kenmerken met meerdere waarden string subreeksen gezocht in de waarden.  
Voor kenmerken van de verwijzing, de gezochte tekenreeks exact overeenkomen met de waarde van een overeenkomst worden beschouwd.

**Voorbeeld:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Als het kenmerk proxyAddresses een primair e-mailadres (aangegeven met hoofdletters ' SMTP: "), retourneert het kenmerk proxyAddress, anders een fout.

----------
### <a name="convertfrombase64"></a>ConvertFromBase64

**Beschrijving:**  
De functie ConvertFromBase64 omgezet de opgegeven base64-gecodeerde waarde in een normale tekenreeks.

**Syntaxis:**  
`str ConvertFromBase64(str source)`-wordt ervan uitgegaan dat Unicode voor codering  
`str ConvertFromBase64(str source, enum Encoding)`

- bron: met Base64 gecodeerde tekenreeks  
- -Codering: ASCII, Unicode UTF8

**Voorbeeld**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Beide voorbeelden terug "*Hello world!*"

----------
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex

**Beschrijving:**  
De functie ConvertFromUTF8Hex omgezet de opgegeven gecodeerde UTF8-Hex-waarde in een tekenreeks.

**Syntaxis:**  
`str ConvertFromUTF8Hex(str source)`

- bron: gecodeerde UTF8 2-byte-String

**Opmerkingen:**  
Het verschil tussen deze functie en ConvertFromBase64([],UTF8) in die het resultaat is van het kenmerk DN beschrijvende.  
Deze indeling wordt gebruikt door Azure Active Directory als de DN-naam.

**Voorbeeld:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Geeft als resultaat "*Hello world!*"

----------
### <a name="converttobase64"></a>ConvertToBase64

**Beschrijving:**  
De functie ConvertToBase64 omgezet een tekenreeks in een Unicode-base64-tekenreeks.  
De waarde van een matrix van gehele getallen omgezet in de overeenkomstige tekenreeks voorstelling die is gecodeerd met base64-cijfers.

**Syntaxis:**  
`str ConvertToBase64(str source)`

**Voorbeeld:**  
`ConvertToBase64("Hello world!")`  
Geeft als resultaat "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

----------
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex

**Beschrijving:**  
De functie ConvertToUTF8Hex omgezet een tekenreeks in een gecodeerde UTF8-Hex-waarde.

**Syntaxis:**  
`str ConvertToUTF8Hex(str source)`

**Opmerkingen:**  
Indeling van de uitvoer van deze functie wordt gebruikt door Azure Active Directory als de indeling van het kenmerk DN-naam.

**Voorbeeld:**  
`ConvertToUTF8Hex("Hello world!")`  
Deze eigenschap retourneert 48656C6C6F20776F726C6421

----------
### <a name="count"></a>Aantal

**Beschrijving:**  
De functie Count wordt het aantal elementen in een kenmerk met meerdere waarden

**Syntaxis:**  
`num Count(mvstr attribute)`

----------
### <a name="cnum"></a>CNum

**Beschrijving:**  
De functie CNum een tekenreeks en retourneert een numeriek gegevenstype.

**Syntaxis:**  
`num CNum(str value)`

----------
### <a name="cref"></a>CRef

**Beschrijving:**  
Zet een tekenreeks met een verwijzing naar kenmerk

**Syntaxis:**  
`ref CRef(str value)`

**Voorbeeld:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

----------
### <a name="cstr"></a>CStr

**Beschrijving:**  
De functie CStr worden geconverteerd naar het gegevenstype string.

**Syntaxis:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

- waarde: een numerieke waarde, een verwijzing kenmerk of een Boolean kan worden.

**Voorbeeld:**  
`CStr([dn])`  
Kan retourneren "cn = Jan, dc = contoso, dc = com"

----------
### <a name="dateadd"></a>DateAdd

**Beschrijving:**  
Deze eigenschap retourneert een datum met een datum waaraan een opgegeven tijdsinterval is toegevoegd.

**Syntaxis:**  
`dt DateAdd(str interval, num value, dt date)`

- interval: een tekenreeksexpressie die het tijdinterval toe te voegen. De tekenreeks moet een van de volgende waarden hebben:
 - Jaar jjjj
 - Kwartaal q
 - Maand m
 - Dag van het jaar y
 - d-dag
 - w-dag van de week
 - ww Week
 - uur uur
 - n minuut
 - s tweede
- waarde: het aantal eenheden dat u wilt toevoegen. Zijn positieve (voor datums in de toekomst) of negatieve (voor datums in het verleden).
- datum: datum/tijd die datum waaraan de interval wordt toegevoegd.

**Voorbeeld:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
3 maanden toegevoegd en geeft als resultaat een DateTime die '01-04-2001'.

----------
### <a name="datefromnum"></a>DateFromNum

**Beschrijving:**  
De DateFromNum functie converteert een waarde in de datum van de advertentie-indeling op het type datum/tijd.

**Syntaxis:**  
`dt DateFromNum(num value)`

**Voorbeeld:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Deze eigenschap retourneert een DateTime die 01-01-2012 23:00:00

----------
### <a name="dncomponent"></a>DNComponent

**Beschrijving:**  
De functie DNComponent retourneert de waarde van de opgegeven DN-naam onderdeel van links.

**Syntaxis:**  
`str DNComponent(ref dn, num ComponentNumber)`

- DN-naam: het kenmerk van de verwijzing te interpreteren
- ComponentNumber: Het onderdeel in de DN-naam om terug te keren

**Voorbeeld:**  
`DNComponent([dn],1)`  
Als de DN-naam is ' cn Joe, ou = =..., "Joe is het resultaat

----------
### <a name="dncomponentrev"></a>DNComponentRev

**Beschrijving:**  
De functie DNComponentRev retourneert de waarde van de opgegeven DN-naam onderdeel van rechts (het einde).

**Syntaxis:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- DN-naam: het kenmerk van de verwijzing te interpreteren
- ComponentNumber - de component in de DN-naam om terug te keren
- Opties: DC: negeren van alle onderdelen met ' dc = '

**Voorbeeld:**  
Als de DN-naam is ' cn Joe, ou = Atlanta, ou = GA, ou = = US, dc = contoso, dc = com "Klik  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
Zowel ons als resultaat.

----------
### <a name="error"></a>Fout

**Beschrijving:**  
De Error-functie wordt gebruikt om een aangepaste fout.

**Syntaxis:**  
`void Error(str ErrorMessage)`

**Voorbeeld:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Als u de accountnaam van het kenmerk niet aanwezig is, genereert een fout op het object.

----------
### <a name="escapedncomponent"></a>EscapeDNComponent

**Beschrijving:**  
De functie EscapeDNComponent duurt één onderdeel van een DN-naam en verlaat u zodat deze kan worden weergegeven in LDAP.

**Syntaxis:**  
`str EscapeDNComponent(str value)`

**Voorbeeld:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Zorgt ervoor dat het object kan worden gemaakt in een LDAP-adreslijst, zelfs als het kenmerk displayName bevat tekens die in LDAP moeten worden voorafgegaan.

----------
### <a name="formatdatetime"></a>FormatDateTime

**Beschrijving:**  
De functie FormatDateTime wordt gebruikt voor het opmaken van een datum/tijd in een tekenreeks met een opgegeven indeling

**Syntaxis:**  
`str FormatDateTime(dt value, str format)`

- waarde: een waarde in de DateTime-indeling
- indeling: een string die de indeling te converteren naar.

**Opmerkingen:**  
De mogelijke waarden voor de indeling vindt u hier: [Door de gebruiker gedefinieerde datum-/ tijdnotaties (functie Format)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Voorbeeld:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
Resulteert in "25-12-2007".

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
Kan resulteren in "20140905081453.0Z"

----------
### <a name="guid"></a>GUID

**Beschrijving:**  
De GUID wordt gegenereerd een nieuwe willekeurige GUID

**Syntaxis:**  
`str GUID()`

----------
### <a name="iif"></a>IIF

**Beschrijving:**  
De IIF-functie retourneert een van de mogelijke waarden op basis van een opgegeven voorwaarde.

**Syntaxis:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- voorwaarde: een waarde of expressie die kan worden geëvalueerd naar waar of ONWAAR.
- WaardeAlsWaar: als de voorwaarde resulteert in true, wordt de geretourneerde waarde.
- WaardeAlsOnwaar: als de voorwaarde in ONWAAR, de geretourneerde waarde resulteert.

**Voorbeeld:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Als de gebruiker een Stage, wordt de alias van een gebruiker met een "t-" toegevoegd aan het begin van deze anders retourneert de alias van de gebruiker is.

----------
### <a name="instr"></a>InStr

**Beschrijving:**  
De functie InStr wordt het eerste exemplaar van een subtekenreeks van een tekenreeks

**Syntaxis:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- reekscontroleren: string moet worden gezocht
- reeksvergelijken: string worden gevonden
- Start: vanaf de positie om te zoeken naar de subtekenreeks
- Vergelijk: vbTextCompare of vbBinaryCompare

**Opmerkingen:**  
Retourneert de positie waar de subtekenreeks is gevonden, of 0 indien niet gevonden.

**Voorbeeld:**  
`InStr("The quick brown fox","quick")`  
Evalues tot en met 5

`InStr("repEated","e",3,vbBinaryCompare)`  
Resulteert in 7

----------
### <a name="instrrev"></a>InStrRev

**Beschrijving:**  
De functie InStrRev vindt u het laatste exemplaar van een subtekenreeks van een tekenreeks

**Syntaxis:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- reekscontroleren: string moet worden gezocht
- reeksvergelijken: string worden gevonden
- Start: vanaf de positie om te zoeken naar de subtekenreeks
- Vergelijk: vbTextCompare of vbBinaryCompare

**Opmerkingen:**  
Retourneert de positie waar de subtekenreeks is gevonden, of 0 indien niet gevonden.

**Voorbeeld:**  
`InStrRev("abbcdbbbef","bb")`  
Geeft 7

----------
### <a name="isbitset"></a>IsBitSet

**Beschrijving:**  
De functie IsBitSet Tests als een bit is ingesteld of niet

**Syntaxis:**  
`bool IsBitSet(num value, num flag)`

- waarde: een numerieke waarde die is evaluated.flag: een numerieke waarde waarvoor de bits te evalueren

**Voorbeeld:**  
`IsBitSet(&HF,4)`  
Het resultaat is True omdat "4" bit is ingesteld in de hexadecimale waarde "F"

----------
### <a name="isdate"></a>IsDate

**Beschrijving:**  
Als de expressie kan worden geëvalueerd als een datum/tijd, en vervolgens de functie IsDate wordt geëvalueerd als True.

**Syntaxis:**  
`bool IsDate(var Expression)`

**Opmerkingen:**  
Gebruikt om te bepalen als CDate() uitgevoerd worden kan.

----------
### <a name="isempty"></a>IsEmpty

**Beschrijving:**  
Als het kenmerk aanwezig in de CS of MV is, maar in een lege tekenreeks, resulteert wordt de functie IsEmpty wordt geëvalueerd als True.

**Syntaxis:**  
`bool IsEmpty(var Expression)`

----------
### <a name="isguid"></a>IsGuid

**Beschrijving:**  
Als de tekenreeks kan worden geconverteerd naar een GUID, wordt de IsGuid-functie geëvalueerd op true.

**Syntaxis:**  
`bool IsGuid(str GUID)`

**Opmerkingen:**  
Een GUID is gedefinieerd als een tekenreeks die na een van deze patronen: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx of {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Gebruikt om te bepalen als CGuid() uitgevoerd worden kan.

**Voorbeeld:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Als de StrAttribute een GUID-indeling heeft, een binaire weergave terugkeren, anders een null-waarde te retourneren.

----------
### <a name="isnull"></a>IsNull

**Beschrijving:**  
Als de expressie de waarde Null oplevert, geeft de functie IsNull als resultaat true.

**Syntaxis:**  
`bool IsNull(var Expression)`

**Opmerkingen:**  
Voor een kenmerk, wordt een null-waarde uitgedrukt door de afwezigheid van het kenmerk.

**Voorbeeld:**  
`IsNull([displayName])`  
Geeft als resultaat waar als het kenmerk niet aanwezig is in de CS of MV.

----------
### <a name="isnullorempty"></a>IsNullOrEmpty

**Beschrijving:**  
Als de expressie null of een lege tekenreeks is, retourneert de functie IsNullOrEmpty de waarde true.

**Syntaxis:**  
`bool IsNullOrEmpty(var Expression)`

**Opmerkingen:**  
Voor een kenmerk, zou dit resulteren in True als het kenmerk niet aanwezig is of aanwezig is maar een lege tekenreeks.  
De inverse van deze functie de naam IsPresent.

**Voorbeeld:**  
`IsNullOrEmpty([displayName])`  
Geeft als resultaat waar als het kenmerk niet aanwezig is of een lege tekenreeks in de CS of MV.

----------
### <a name="isnumeric"></a>IsNumeric

**Beschrijving:**  
De functie IsNumeric retourneert een Boolean-waarde die aangeeft of een expressie kan worden geëvalueerd als een getal.

**Syntaxis:**  
`bool IsNumeric(var Expression)`

**Opmerkingen:**  
Gebruikt om te bepalen als CNum() uitgevoerd worden kan op de expressie niet parseren.

----------
### <a name="isstring"></a>IsString

**Beschrijving:**  
Als de expressie kan worden geëvalueerd als een tekenreeks typt, vervolgens de IsString functie wordt geëvalueerd als True.

**Syntaxis:**  
`bool IsString(var expression)`

**Opmerkingen:**  
Gebruikt om te bepalen als CStr() uitgevoerd worden kan op de expressie niet parseren.

----------
### <a name="ispresent"></a>IsPresent

**Beschrijving:**  
Als de expressie in een tekenreeks die niet Null is en niet leeg is resulteert, zijn de IsPresent-functie retourneert de waarde true.

**Syntaxis:**  
`bool IsPresent(var expression)`

**Opmerkingen:**  
De inverse van deze functie de naam IsNullOrEmpty.

**Voorbeeld:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

----------
### <a name="item"></a>Artikel

**Beschrijving:**  
De Item-functie retourneert een item uit een tekenreeks/kenmerk met meerdere waarden.

**Syntaxis:**  
`var Item(mvstr attribute, num index)`

- kenmerk: een kenmerk met meerdere waarden
- index: index met een item in de tekenreeks met meerdere waarden.

**Opmerkingen:**  
De functie artikel is nuttig in combinatie met de functie bevat sinds de laatste functie resulteert in de index naar een item in het kenmerk met meerdere waarden.

Genereert een fout als de index buiten het bereik.

**Voorbeeld:**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Deze eigenschap retourneert het primaire e-mailadres.

----------
### <a name="itemornull"></a>ItemOrNull

**Beschrijving:**  
De functie ItemOrNull retourneert een item uit een tekenreeks/kenmerk met meerdere waarden.

**Syntaxis:**  
`var ItemOrNull(mvstr attribute, num index)`

- kenmerk: een kenmerk met meerdere waarden
- index: index met een item in de tekenreeks met meerdere waarden.

**Opmerkingen:**  
De functie ItemOrNull is nuttig in combinatie met de functie bevat, aangezien de laatstgenoemde functie resulteert in de index naar een item in het kenmerk met meerdere waarden.

Retourneert een Null-waarde als index buiten bereik is.

----------
### <a name="join"></a>Join

**Beschrijving:**  
De functie Join een meerwaardige tekenreeks en retourneert een tekenreeks één waarde met het opgegeven scheidingsteken tussen elk item ingevoegd.

**Syntaxis:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

- kenmerk: kenmerk met meerdere waarden met tekenreeksen wilt samenvoegen.
- scheidingsteken: een willekeurige tekenreeks, gebruikt voor het scheiden van de subtekenreeksen in de geretourneerde tekenreeks. Als u dit argument weglaat, wordt de spatie ("") wordt gebruikt. Als scheidingsteken een tekenreeks met lengte nul ("") of niets, alle items in de lijst aan elkaar gekoppeld zonder scheidingstekens.

**Opmerkingen**  
Er is pariteit tussen de functies Join en splitsen. De functie Join wordt een matrix van tekenreeksen en toegevoegd, die de waarde voor scheidingsteken, met een enkele tekenreeks. De Split-functie een tekenreeks en bij het scheidingsteken, retourneert een matrix van tekenreeksen wordt gescheiden. Een belangrijk verschil is echter dat de Join tekenreeksen met een willekeurige tekenreeks als scheidingsteken kan samenvoegen, splitsen kunt alleen afzonderlijke tekenreeksen met behulp van een enkel teken als scheidingsteken.

**Voorbeeld:**  
`Join([proxyAddresses],",")`  
Kan retourneren:"SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

----------
### <a name="lcase"></a>LCase

**Beschrijving:**  
De functie LCase omgezet alle tekens in een tekenreeks in kleine letters.

**Syntaxis:**  
`str LCase(str value)`

**Voorbeeld:**  
`LCase("TeSt")`  
Geeft als resultaat "test".

----------
### <a name="left"></a>Links

**Beschrijving:**  
De functie links geeft als resultaat een opgegeven aantal tekens vanaf de linkerkant van een tekenreeks.

**Syntaxis:**  
`str Left(str string, num NumChars)`

- reeks: de tekenreeks te retourneren tekens uit
- NumChars: een nummer voor het aantal tekens op vanaf het begin (links) van de tekenreeks terug

**Opmerkingen:**  
Een tekenreeks met de eerste numChars tekens in de tekenreeks:

- Als numChars = 0, lege tekenreeks als resultaat geven.
- Als de invoerreeks numChars < 0,.
- Als tekenreeks null is, lege tekenreeks als resultaat.

Als tekenreeks minder tekens dan het aantal opgegeven in numChars bevat, wordt een tekenreeks die overeenkomt met de tekenreeks (die is, met alle tekens in parameter 1) geretourneerd.

**Voorbeeld:**  
`Left("John Doe", 3)`  
Deze eigenschap retourneert een 'Joh'.

----------
### <a name="len"></a>Len

**Beschrijving:**  
De functie lengte geeft als resultaat het aantal tekens in een tekenreeks.

**Syntaxis:**  
`num Len(str value)`

**Voorbeeld:**  
`Len("John Doe")`  
Geeft als resultaat 8

----------
### <a name="ltrim"></a>LTrim

**Beschrijving:**  
De functie LTrim verwijdert voorloopspaties wit uit een tekenreeks.

**Syntaxis:**  
`str LTrim(str value)`

**Voorbeeld:**  
`LTrim(" Test ")`  
Geeft als resultaat "Test"

----------
### <a name="mid"></a>Mid

**Beschrijving:**  
De functie Mid geeft als resultaat een bepaald aantal tekens uit een opgegeven positie in een tekenreeks.

**Syntaxis:**  
`str Mid(str string, num start, num NumChars)`

- reeks: de tekenreeks te retourneren tekens uit
- Start: een nummer van de eerste positie in de tekenreeks te retourneren tekens uit
- NumChars: een nummer voor het aantal tekens uit een positie in de tekenreeks

**Opmerkingen:**  
NumChars tekens vanaf de begin positie in de tekenreeks als resultaat.  
Een tekenreeks met numChars tekens vanaf het begin van de positie in de tekenreeks:

- Als numChars = 0, lege tekenreeks als resultaat geven.
- Als de invoerreeks numChars < 0,.
- Als start > de lengte van de tekenreeks, ingevoerde tekenreeks terug.
- Als start < = 0, ingevoerde tekenreeks terug.
- Als tekenreeks null is, lege tekenreeks als resultaat.

Als er geen tekens numChar worden nog in tekenreeks van begin positie, zo veel mogelijk tekens mogelijk geretourneerd.

**Voorbeeld:**  
`Mid("John Doe", 3, 5)`  
Deze eigenschap retourneert een 'hn Do'.

`Mid("John Doe", 6, 999)`  
Geeft als resultaat "De Vries"

----------
### <a name="now"></a>Nu

**Beschrijving:**  
De functie Now retourneert een DateTime die de huidige datum en tijd volgens de systeemdatum en -tijd van uw computer op te geven.

**Syntaxis:**  
`dt Now()`

----------
### <a name="numfromdate"></a>NumFromDate

**Beschrijving:**  
De functie NumFromDate retourneert een datum in de AD-datumnotatie.

**Syntaxis:**  
`num NumFromDate(dt value)`

**Voorbeeld:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Deze eigenschap retourneert 129699324000000000

----------
### <a name="padleft"></a>PadLeft

**Beschrijving:**  
De PadLeft functie links-pads een tekenreeks naar een opgegeven lengte met een teken voor de opgegeven opvulling.

**Syntaxis:**  
`str PadLeft(str string, num length, str padCharacter)`

- reeks: de tekenreeks die moet worden opgevuld.
- lengte: een geheel getal dat de gewenste lengte van de tekenreeks vertegenwoordigt.
- padCharacter: een tekenreeks die bestaat uit een enkel teken als het teken pad

**Opmerkingen:**

- Als de lengte van de tekenreeks kleiner dan de lengte is, vervolgens padCharacter herhaaldelijk toegevoegd aan het begin (links) van de tekenreeks totdat er een lengte gelijk is aan lengte.
- PadCharacter kan een spatie, maar het kan een null-waarde niet.
- Als de lengte van de tekenreeks gelijk aan of groter is dan de lengte is, string ongewijzigd geretourneerd.
- Als tekenreeks een lengte groter dan of gelijk is aan lengte heeft, wordt een tekenreeks die overeenkomt met de tekenreeks geretourneerd.
- Als de lengte van de tekenreeks kleiner dan de lengte is, heeft een nieuwe reeks van de gewenste lengte met geretourneerd, aangevuld met een padCharacter.
- Als tekenreeks null is, retourneert de functie een lege tekenreeks.

**Voorbeeld:**  
`PadLeft("User", 10, "0")`  
Deze eigenschap retourneert een '000000User'.

----------
### <a name="padright"></a>PadRight

**Beschrijving:**  
De PadRight functie rechts-pads een tekenreeks naar een opgegeven lengte met een teken voor de opgegeven opvulling.

**Syntaxis:**  
`str PadRight(str string, num length, str padCharacter)`

- reeks: de tekenreeks die moet worden opgevuld.
- lengte: een geheel getal dat de gewenste lengte van de tekenreeks vertegenwoordigt.
- padCharacter: een tekenreeks die bestaat uit een enkel teken als het teken pad

**Opmerkingen:**

- Als de lengte van de tekenreeks kleiner dan de lengte is, worden padCharacter herhaaldelijk toegevoegd aan het einde van de tekenreeks dat (rechts) totdat er een lengte die gelijk is aan de lengte.
- padCharacter kan een spatie, maar het kan een null-waarde niet.
- Als de lengte van de tekenreeks gelijk aan of groter is dan de lengte is, string ongewijzigd geretourneerd.
- Als tekenreeks een lengte groter dan of gelijk is aan lengte heeft, wordt een tekenreeks die overeenkomt met de tekenreeks geretourneerd.
- Als de lengte van de tekenreeks kleiner dan de lengte is, heeft een nieuwe reeks van de gewenste lengte met geretourneerd, aangevuld met een padCharacter.
- Als tekenreeks null is, retourneert de functie een lege tekenreeks.

**Voorbeeld:**  
`PadRight("User", 10, "0")`  
Deze eigenschap retourneert een 'User000000'.

----------
### <a name="pcase"></a>PCase

**Beschrijving:**  
De functie PCase wordt omgezet in het eerste teken van elk woord door spaties gescheiden in een tekenreeks in hoofdletters en andere tekens worden geconverteerd naar kleine letters.

**Syntaxis:**  
`String PCase(string)`

**Opmerkingen:**

- Deze functie biedt momenteel geen juiste hoofdlettergebruik als een woord dat is geheel in hoofdletters zoals een acroniem wilt converteren.

**Voorbeeld:**  
`PCase("TEsT")`  
Deze eigenschap retourneert een 'Test'.

`PCase(LCase("TEST"))`  
Geeft als resultaat "Test"

----------
### <a name="randomnum"></a>RandomNum

**Beschrijving:**  
De functie RandomNum retourneert een willekeurig getal tussen een opgegeven interval.

**Syntaxis:**  
`num RandomNum(num start, num end)`

- Start: een nummer van de ondergrens van een willekeurige waarde genereren
- einde: het nummer voor het genereren van de bovengrens van een willekeurige waarde

**Voorbeeld:**  
`Random(100,999)`  
734 kunnen worden geretourneerd.

----------
### <a name="removeduplicates"></a>RemoveDuplicates

**Beschrijving:**  
De functie RemoveDuplicates een tekenreeks met meerdere waarden en zorg ervoor dat elke waarde uniek is.

**Syntaxis:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Voorbeeld:**  
`RemoveDuplicates([proxyAddresses])`  
Geeft als resultaat een gezuiverde proxyAddress kenmerk waar alle dubbele waarden zijn verwijderd.

----------
### <a name="replace"></a>Vervangen

**Beschrijving:**  
De functie vervangen vervangt alle exemplaren van een tekenreeks naar een andere tekenreeks.

**Syntaxis:**  
`str Replace(str string, str OldValue, str NewValue)`

- tekenreeks: een tekenreeks te vervangen van waarden in.
- Oude waarde: De tekenreeks zoeken en vervangen.
- NewValue: De tekenreeks te vervangen.

**Opmerkingen:**  
De functie herkent de volgende speciale bijnamen:

- \n – nieuwe regel
- \r – regelterugloop
- \t – tabblad

**Voorbeeld:**  
`Replace([address],"\r\n",", ")`  
CRLF vervangt door een komma en een spatie, en kan leiden tot een 'One Microsoft manier, Redmond, WA, USA'

----------
### <a name="replacechars"></a>ReplaceChars

**Beschrijving:**  
De functie ReplaceChars wordt vervangen door alle exemplaren van de tekens in de tekenreeks ReplacePattern.

**Syntaxis:**  
`str ReplaceChars(str string, str ReplacePattern)`

- tekenreeks: een tekenreeks te vervangen van tekens in.
- ReplacePattern: een tekenreeks met een woordenlijst met tekens te vervangen.

De indeling is {bron1}: {target1}, {bron2}: {target2}, {bronN}, {targetN} bron is waar het teken te zoeken en vervangen door de tekenreeks die als doel.

**Opmerkingen:**

- De functie heeft elk exemplaar van de gedefinieerde gegevensbronnen en vervangt u deze met de doelen.
- De bron moet precies één (unicode)-tekens.
- De bron kan niet leeg is of meer dan één teken (fout bij parseren).
- Het doel kan meerdere tekens, bijvoorbeeld ö:oe, β:ss hebben.
- Het doel kan worden leeg die aangeeft dat het teken moet worden verwijderd.
- De bron is hoofdlettergevoelig en moet exact overeenkomen.
- De, (komma) en: (dubbele punt) zijn gereserveerde tekens en met deze functie kan niet worden vervangen.
- Spaties en andere witte tekens in de tekenreeks ReplacePattern worden genegeerd.

**Voorbeeld:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Deze eigenschap retourneert een Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Geeft als resultaat "ONeil", één stap is bedoeld om te worden verwijderd.

----------
### <a name="right"></a>Rechts

**Beschrijving:**  
De juiste functie geeft als resultaat een opgegeven aantal tekens vanaf de rechterkant (end) van een tekenreeks.

**Syntaxis:**  
`str Right(str string, num NumChars)`

- reeks: de tekenreeks te retourneren tekens uit
- NumChars: een nummer voor het aantal tekens uit het einde van de tekenreeks dat (rechts)

**Opmerkingen:**  
NumChars tekens vanaf de laatste positie van tekenreeks geretourneerd.

Een tekenreeks met de laatste numChars tekens in de tekenreeks:

- Als numChars = 0, lege tekenreeks als resultaat geven.
- Als de invoerreeks numChars < 0,.
- Als tekenreeks null is, lege tekenreeks als resultaat.

Als tekenreeks minder tekens dan het aantal opgegeven in NumChars bevat, wordt een tekenreeks die overeenkomt met de tekenreeks geretourneerd.

**Voorbeeld:**  
`Right("John Doe", 3)`  
Geeft als resultaat "De Vries".

----------
### <a name="rtrim"></a>RTrim

**Beschrijving:**  
De functie RTrim verwijdert spaties aan het einde van een tekenreeks.

**Syntaxis:**  
`str RTrim(str value)`

**Voorbeeld:**  
`RTrim(" Test ")`  
Deze eigenschap retourneert een 'Test'.

----------
### <a name="split"></a>Splitsen

**Beschrijving:**  
De Split-functie een tekenreeks van elkaar gescheiden met een scheidingsteken, waardoor het een tekenreeks met meerdere waarden.

**Syntaxis:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

- waarde: de tekenreeks met een teken als scheidingsteken te scheiden.
- scheidingsteken: enkel teken als scheidingsteken worden gebruikt.
- limiet: maximum aantal waarden die als resultaat kunt geven.

**Voorbeeld:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Retourneert een tekenreeks met meerdere waarden met 2 elementen nuttig voor het kenmerk proxyAddress.

----------
### <a name="stringfromguid"></a>StringFromGuid

**Beschrijving:**  
De functie StringFromGuid nodig is een binair GUID en zet deze om in een tekenreeks

**Syntaxis:**  
`str StringFromGuid(bin GUID)`

----------
### <a name="stringfromsid"></a>StringFromSid

**Beschrijving:**  
De StringFromSid functie converteert een beveiligings-id naar een tekenreeks met byte-matrix.

**Syntaxis:**  
`str StringFromSid(bin ObjectSID)`  

----------
### <a name="switch"></a>Switch

**Beschrijving:**  
De functie Switch wordt gebruikt voor een enkele waarde op basis van voorwaarden geëvalueerd.

**Syntaxis:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- expr: variantexpressie die u wilt evalueren.
- waarde: de waarde moet worden geretourneerd als de bijbehorende expressie waar is.

**Opmerkingen:**  
De lijst met argumenten functie Switch bestaat uit combinaties van expressies en waarden. De expressies worden geëvalueerd van links naar rechts en de waarde die is gekoppeld aan de eerste expressie die True als resultaat wordt gegeven. Als de onderdelen zijn niet goed gekoppeld, treedt er een runtime fout op.

Als expr1 True is, retourneert Switch waarde1. Als expr-1 ingesteld op False is, maar expr-2 waar is, retourneert de schakeloptie waarde 2, enzovoort.

Switch een geretourneerd als:

- Geen van de expressies zijn True.
- De eerste expressie met de waarde True heeft een bijbehorende waarde Null is.

Switch evalueert alle expressies, ook al slechts één van beide wordt. Daarom moet u opletten voor ongewenste neveneffecten. Bijvoorbeeld, als de evaluatie van een expressie in een deling door nul resulteert, treedt een fout op.

Waarde kan ook worden de fout-functie, die een aangepaste tekenreeks als resultaat zou geven.

**Voorbeeld:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Deze eigenschap retourneert de taal wordt gesproken in sommige grote steden, anders wordt een fout geretourneerd.

----------
### <a name="trim"></a>Trim

**Beschrijving:**  
De functie spaties verwijdert u voorloopspaties en volgspaties met wit uit een tekenreeks.

**Syntaxis:**  
`str Trim(str value)`  

**Voorbeeld:**  
`Trim(" Test ")`  
Deze eigenschap retourneert een 'Test'.

`Trim([proxyAddresses])`  
Hiermee verwijdert u voorloopspaties en volgspaties voor elke waarde in het kenmerk proxyAddress.

----------
### <a name="ucase"></a>UCase

**Beschrijving:**  
De functie UCase omgezet alle tekens in een tekenreeks in hoofdletters.

**Syntaxis:**  
`str UCase(str string)`

**Voorbeeld:**  
`UCase("TeSt")`  
Geeft als resultaat "TEST".

----------
### <a name="word"></a>Word

**Beschrijving:**  
De Word-functie retourneert een woord dat voorkomt binnen een andere tekenreeks, op basis van parameters met een beschrijving van de scheidingstekens wilt gebruiken en het nummer word om terug te keren.

**Syntaxis:**  
`str Word(str string, num WordNumber, str delimiters)`

- reeks: de tekenreeks te retourneren van een woord uit.
- WordNumber: een getal dat aangeeft welke word getal moet retourneren.
- scheidingstekens: een string die de delimiter(s) die moet worden gebruikt voor het identificeren van woorden

**Opmerkingen:**  
Elke reeks tekens in string, gescheiden door een van de tekens in de scheidingstekens worden aangeduid als woorden:

- Als getal < 1, het resultaat een lege tekenreeks.
- Als tekenreeks null is, retourneert de lege tekenreeks.

Als tekenreeks minder dan het aantal woorden bevat of tekenreeks bevat geen woorden aangeduid met scheidingstekens, wordt een lege tekenreeks geretourneerd.

**Voorbeeld:**  
`Word("The quick brown fox",3," ")`  
Geeft als resultaat "bruin"

`Word("This,string!has&many separators",3,",!&#")`  
Het resultaat "heeft"

## <a name="additional-resources"></a>Aanvullende bronnen

* [Wat zijn expressies van declaratieve inrichten](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD verbinding Sync: Synchronisatie-opties aanpassen](active-directory-aadconnectsync-whatis.md)
* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
