<properties
   pageTitle="PowerShell Connector | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe Microsoft Windows PowerShell-Connector configureren."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="windows-powershell-connector-technical-reference"></a>Technische naslaginformatie over Windows PowerShell-Connector
Dit artikel beschrijft de Windows PowerShell-Connector. Het artikel is van toepassing op de volgende producten:

- Microsoft Identity Manager 2016 (MIM2016)
- Forefront Identiteitenbeheer 2010 R2 (FIM2010R2)
    -   Moet hotfix 4.1.3671.0 of later [KB3092178](https://support.microsoft.com/kb/3092178)gebruiken.

De Connector is voor MIM2016 en FIM2010R2, worden gedownload vanaf het [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Overzicht van de PowerShell-Connector
De PowerShell-Connector kunt u de synchronisatieservice integreren met externe systemen met dat Windows PowerShell op basis van API's. De connector biedt een brug tussen de mogelijkheden van de extensible connectiviteit op basis van oproep management agent 2 framework (ECMA2) en Windows PowerShell. Zie [Extensible connectiviteit 2.2 Management Agent Reference](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx)voor meer informatie over de ECMA-framework.

### <a name="prerequisites"></a>Vereisten
Voordat u de verbindingslijn gebruiken, moet u het volgende op de server voor de synchronisatie:

- 4.5.2 van Microsoft .NET Framework of later
- Windows PowerShell 2.0, 3.0 of 4.0

Het beleid kan worden uitgevoerd op de server met de synchronisatie moet worden geconfigureerd als u wilt dat de connector Windows PowerShell-scripts worden uitgevoerd. Tenzij de scripts de connector wordt uitgevoerd, zijn digitaal ondertekend, het uitvoeringsbeleid configureren met behulp van deze opdracht:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Een nieuwe verbindingslijn maken
Om een Windows PowerShell-connector in de tijdsynchronisatie-service maakt, moet u een reeks van Windows PowerShell-scripts die worden uitgevoerd door de synchronisatieservice wordt verzocht stappen opgeven. Afhankelijk van de gegevensbron waarmee u verbinding met maakt en de functionaliteit die u nodig hebt, is de scripts die u moet implementeren. In deze sectie vindt u een overzicht van de scripts die kunnen worden geïmplementeerd en wanneer deze nodig zijn.

De Windows PowerShell-connector is ontworpen voor het opslaan van elk van de scripts in de synchronisatie-database. Tijdens het uitvoeren van scripts die zijn opgeslagen op het bestandssysteem mogelijk is, is het gemakkelijker om de hoofdtekst van elk script rechtstreeks in de configuratie van de verbindingslijn.

Selecteer een PowerShell om connector te maken, in de **Tijdsynchronisatie-Service** **Management Agent** en **maken**. Selecteer de verbindingslijn **PowerShell (Microsoft)** .

![Verbindingslijn maken](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Connectiviteit
Configuratieparameters voor de verbinding met een extern systeem opgeven. Deze waarden worden veilig opgeslagen door de synchronisatieservice en beschikbaar gemaakt voor de Windows PowerShell-scripts als de verbindingslijn wordt uitgevoerd.

![Connectiviteit](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

U kunt de volgende parameters voor de verbinding configureren:

**Connectiviteit**

Parameter | Standaardwaarde | Doel
--- | --- | ---
Server | <Blank> | De connector moet worden verbonden met servernaam.
Domein | <Blank> | Domein van de referenties op te slaan voor gebruik wanneer de connector wordt uitgevoerd.
Gebruiker | <Blank> | De gebruikersnaam van de referenties op te slaan voor gebruik wanneer de connector wordt uitgevoerd.
Wachtwoord | <Blank> | Het wachtwoord van de referenties op te slaan voor gebruik wanneer de connector wordt uitgevoerd.
Imiteren Connector-Account | False | Waar is, wordt de synchronisatieservice Windows PowerShell-scripts uitgevoerd in de context van de opgegeven referenties. Indien mogelijk, wordt aanbevolen dat de **$Credentials** -parameter wordt doorgegeven aan elk script wordt gebruikt in plaats van imitatie. Zie voor meer informatie over aanvullende machtigingen die vereist zijn voor deze optie wilt gebruiken, [Aanvullende configuratie voor imitatie](#additional-configuration-for-impersonation).
Gebruikersprofiel laden als u imitatie | False | Hiermee geeft u Windows het gebruikersprofiel van de referenties van de verbindingslijn laden tijdens de imitatie. Als de geïmiteerde gebruiker een zwervend profiel heeft, wordt de verbindingslijn zwervend profiel niet geladen. Zie voor meer informatie over aanvullende machtigingen die nodig zijn om deze parameter gebruikt, [Aanvullende configuratie voor imitatie](#additional-configuration-for-impersonation).
Aanmeldingstype te gebruiken als u imitatie | Geen | Aanmeldingstype tijdens de imitatie. Raadpleeg de [dwLogonType] [ dw] documentatie.
Alleen ondertekende Scripts | False | Als de waarde true is, wordt de Windows PowerShell-connector gevalideerd dat elk script een geldige digitale handtekening heeft. Als de waarde false is, zorgen ervoor dat de tijdsynchronisatie-Service-server Windows PowerShell-uitvoeringsbeleid RemoteSigned of onbeperkt.

**Algemene Module**  
De connector kunt u een gedeelde Windows PowerShell-module in de configuratie opslaan. Als de verbindingslijn een script wordt uitgevoerd, wordt de Windows PowerShell-module wordt uitgepakt naar het bestandssysteem, zodat deze kan worden geïmporteerd door elk script.

Voor scripts importeren, exporteren en Wachtwoordsynchronisatie, wordt de gemeenschappelijke module uitgepakt in de map MAData van de connector. Voor Schema, validatie, hiërarchie en partitie discovery scripts, wordt de gemeenschappelijke module uitgepakt in de map % TEMP %. In beide gevallen het uitgepakte gemeenschappelijke Module-script is een naam met de instelling van de gemeenschappelijke Module scriptnaam.

Voor het laden van een module met de naam FIMPowerShellConnectorModule.psm1 uit de map MAData, kunt u de volgende instructie gebruiken:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Voor het laden van een module met de naam FIMPowerShellConnectorModule.psm1 uit de map % TEMP %, moet u de volgende instructie gebruiken:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Parametervalidatie**  
Het Script voor validatie is een optionele Windows PowerShell-script dat kan worden gebruikt om ervoor te zorgen dat connector configuratieparameters zijn opgegeven door de beheerder geldig zijn. Zijn het gemeenschappelijk gebruik van het validatiescript verificatieserver verbindingsreferenties parameters, en connectiviteit. De validatiescript wordt aangeroepen na de volgende tabbladen en dialoogvensters worden aangepast:

- Connectiviteit
- Algemene Parameters
- Partitieconfiguratie

Het validatiescript ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameterPage | [ConfigParameterPage][cpp] | Het tabblad configuratie of dialoogvenster die de validatie van aanvraag geactiveerd.
ConfigParameters | [KeyedCollection] [ keyk] [tekenreeks, [ConfigParameter][cp]] | Tabel van de configuratieparameters voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Referenties die zijn opgegeven door de beheerder op het tabblad verbinding bevat.

Het validatiescript moet een ParameterValidationResult-object om de pijpleiding te retourneren.

**Schema Discovery**  
Het script Schema Discovery is verplicht. Dit script geeft als resultaat het objecttypen, kenmerken en kenmerk beperkingen die door de synchronisatieservice wordt gebruikt bij het configureren van regels voor stroom van kenmerken. Het Schema Discovery-script wordt uitgevoerd tijdens het maken van de connector en schema van de verbindingslijn wordt gevuld. Het wordt ook gebruikt door de actie vernieuwen Schema synchronisatie Service Manager.

De discovery schemascript ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection] [ keyk] [tekenreeks, [ConfigParameter][cp]] | Tabel van de configuratieparameters voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Referenties die zijn opgegeven door de beheerder op het tabblad verbinding bevat.

Het script moet resulteren in een enkel [Schema] [ schema] object op de pijpleiding. De Schema-object bestaat uit een [SchemaType] [ schemaT] -objecten die objecttypen vertegenwoordigen (bijvoorbeeld: gebruikers en groepen). Het SchemaType-object bevat een verzameling van [SchemaAttribute] [ schemaA] -objecten met daarin de kenmerken (bijvoorbeeld: voornaam, achternaam en postadres) van het type.

**Extra Parameters**  
Naast de standaard configuratie-instellingen, kunt u aanvullende aangepaste configuratie-instellingen die specifiek voor het exemplaar van de verbindingslijn zijn definiëren. Deze parameters kunnen worden opgegeven op de partitie-connector of de stap uitvoeren niveaus en toegankelijk vanuit het desbetreffende Windows PowerShell-script. Aangepaste configuratie-instellingen kunnen worden opgeslagen in de synchronisatie-database in de indeling tekst zonder opmaak of ze worden gecodeerd. De tijdsynchronisatie-Service automatisch versleuteld en ontsleuteld beveiligde configuratie-instellingen als dat nodig is.

Als u aangepaste configuratie-instellingen, de naam van elke parameter te scheiden met een komma (,).

Als u wilt aangepaste configuratie-instellingen opent vanuit een script, moet u de naam met een onderstrepingsteken achtervoegsel ( \_ ) en de omvang van de parameter (Global, partitie of RunStep). Bijvoorbeeld, als u de parameter FileName globale, gebruiken dit codefragment:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Mogelijkheden
Het tabblad mogelijkheden van de ontwerper Management Agent bepaalt het gedrag en de functionaliteit van de verbindingslijn. De selecties op dit tabblad kunnen niet worden gewijzigd wanneer de verbindingslijn is gemaakt. Deze tabel bevat de mogelijkheid instellingen.

![Mogelijkheden](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

Mogelijkheden | Beschrijving |
--- | --- |
[DN-naam stijl][dnstyle] | Geeft aan of de connector ondersteunt de DN-namen en indien Ja, welke stijl.
[Type exporteren][exportT] | Bepaalt het type van de objecten die worden aangeboden aan het script exporteren. <li>AttributeReplace – bevat de volledige set van waarden voor een kenmerk met meerdere waarden wanneer het kenmerk wordt gewijzigd.</li><li>AttributeUpdate – bevat alleen de delta's aan een kenmerk met meerdere waarden wanneer het kenmerk wordt gewijzigd.</li><li>MultivaluedReferenceAttributeUpdate - bevat een volledige set van waarden voor de meerwaardige kenmerken niet verwijzing en alleen delta voor kenmerken met meerdere waarden verwijzing.</li><li>ObjectReplace – bevat alle kenmerken voor een object wanneer u alle wijzigingen van kenmerken</li>
[Gegevensnormalisatie][DataNorm] | Hiermee geeft u de synchronisatieservice anker kenmerken normaliseren voordat ze zijn bedoeld om scripts.
[Object bevestigen][oconf] | Hiermee configureert u het gedrag van de import in behandeling in de tijdsynchronisatie-Service. <li>Normaal-standaard die alle uitgevoerde wijzigingen worden bevestigd via invoer verwacht</li><li>NoDeleteConfirmation – is wanneer een object wordt verwijderd, er geen wachtende importeren gegenereerd.</li><li>NoAddAndDeleteConfirmation – is wanneer een object wordt gemaakt of verwijderd, er geen wachtende importeren gegenereerd.</li>
DN-naam gebruikt als anker | Als LDAP de onderscheidende naam stijl is, is het ankerkenmerk voor de connector ruimte ook de DN-naam.
Gelijktijdige werking van meerdere verbindingslijnen | Wanneer het selectievakje is ingeschakeld, kunnen meerdere Windows PowerShell verbindingslijnen gelijktijdig uitgevoerd.
Partities | Wanneer het selectievakje is ingeschakeld, wordt de verbindingslijn ondersteunt meerdere partities en een partitie discovery.
Hiërarchie | Wanneer het selectievakje is ingeschakeld, ondersteunt de verbindingslijn een hiërarchische structuur voor LDAP-stijl.
Invoer inschakelen | Wanneer het selectievakje is ingeschakeld, worden de connector gegevens via scripts voor het importeren.
Delta-invoer inschakelen | Wanneer het selectievakje is ingeschakeld, kan de connector delta's aanvragen bij de scripts importeren.
Exporteren inschakelen | Wanneer het selectievakje is ingeschakeld, exporteert de connector gegevens via de export scripts.
Volledige exporteren inschakelen | Wanneer het selectievakje is ingeschakeld, ondersteuning voor de export scripts de verbindingslijn volledig ruimte exporteren. Als u deze optie gebruikt, wilt moet exporteren inschakelen ook worden gecontroleerd.
Geen verwijzing waarden In de eerste uitvoer Pass | Wanneer het selectievakje is ingeschakeld, worden in een tweede keer voor export verwijzing kenmerken geëxporteerd.
Inschakelen van Object wijzigen | Wanneer het selectievakje is ingeschakeld, kunnen de DN-namen worden gewijzigd.
Delete-Add vervangen | Wanneer het selectievakje is ingeschakeld, delete-toevoegen bewerkingen worden geëxporteerd als een enkele vervanging.
Wachtwoord bewerkingen inschakelen | Wanneer het selectievakje is ingeschakeld, worden wachtwoord synchronisatiescripts ondersteund.
Uitvoer-wachtwoord in eerste keer inschakelen | Wanneer het selectievakje is ingeschakeld, worden wachtwoorden ingesteld tijdens het inrichten geëxporteerd wanneer het object is gemaakt.

### <a name="global-parameters"></a>Algemene Parameters
Het tabblad globale Parameters in de ontwerpfunctie voor Management Agent kunt u de Windows PowerShell-scripts die worden uitgevoerd door de connector configureren. U kunt ook globale waarden voor aangepaste configuratie-instellingen die zijn gedefinieerd op het tabblad verbinding.

**Detectie van partitie**  
Een partitie is een aparte naamruimte binnen één gedeelde schema. In Active Directory bijvoorbeeld is elk domein een partitie binnen een forest. Een partitie is de logische groepering voor importeren en exporteren. Import en Export hebben partitie wat een context en alle bewerkingen in deze context. Partities moeten vertegenwoordigen een hiërarchie in LDAP. De DN-naam van een partitie wordt gebruikt in importeren om te controleren of alle geretourneerde objecten binnen het bereik van een partitie. De DN-naam van partitie wordt ook gebruikt tijdens het inrichten van de metaverse aan de verbindingslijn om te bepalen van de partitie die een object gekoppeld aan tijdens het exporteren worden moet.

Het script partitie discovery ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters  | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel van de configuratieparameters voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Referenties die zijn opgegeven door de beheerder op het tabblad verbinding bevat.

Het script moet resulteren in een op een enkele [partitie] [ part] object of een lijst [T] van partitie-objecten op de pijpleiding.

**Detectie van hiërarchie**  
Het script hiërarchie discovery wordt alleen gebruikt als LDAP de onderscheidende naam stijl mogelijkheid. Het script wordt gebruikt om kunt u bladeren en selecteert u een reeks containers die wordt beschouwd als in of buiten het bereik voor het importeren en exporteren. Het script moet bieden alleen een lijst met knooppunten die directe onderliggende van het hoofdknooppunt geleverd aan het script elementen.

Het script hiërarchie discovery ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel van de configuratieparameters voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Referenties die zijn opgegeven door de beheerder op het tabblad verbinding bevat.
ParentNode | [HierarchyNode][hn] | Het hoofdknooppunt van de hiërarchie waarin het script direct onderliggende elementen weer.

Het script moet terugkeren een hetzij een HierarchyNode één onderliggend object of een lijst [T] van de onderliggende HierarchyNode objecten naar de pijplijn.

#### <a name="import"></a>Importeren
Verbindingslijnen die ondersteuning bieden voor importbewerkingen moeten drie scripts implementeren.

**Beginnen met importeren**  
Het begin import script wordt uitgevoerd aan het begin van een stap importeren uitvoeren. Tijdens deze stap kunt u verbinding maken met het bronsysteem en voorbereidende stappen uitvoeren voordat u gegevens importeert uit het systeem verbonden.

Het begin import script ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel van de configuratieparameters voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Referenties die zijn opgegeven door de beheerder op het tabblad verbinding bevat.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Het script informeert over het soort importeren uitvoeren (delta of volledig) partitie, hiërarchie, watermerk en verwachte paginaformaat.
Typen | [Schema][schema] | Schema voor de verbindingslijn die wordt geïmporteerd.

Het script moet resulteren in een enkele [OpenImportConnectionResults] [ oicres] object toe aan de pipeline, bijvoorbeeld:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Gegevens importeren**  
Het importeren van gegevensscript wordt aangeroepen door de connector totdat het script geeft aan dat er geen gegevens meer importeren. De Windows PowerShell-connector heeft een formaat van 9.999 objecten. Als uw script meer dan 9.999 objecten om te importeren geeft, moet u paginering ondersteunen. De connector gegarandeerd een aangepaste eigenschap waarmee u naar een winkel een watermerk kunt zodat telkens wanneer het script voor importeren van gegevens wordt genoemd, het script wordt hervat waar deze gebleven was objecten importeren.

Het script importeren gegevens ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel van de configuratieparameters voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Referenties die zijn opgegeven door de beheerder op het tabblad verbinding bevat.
GetImportEntriesRunStep | [ImportRunStep][irs] | Het watermerk (CustomData) die kan worden gebruikt tijdens de invoer wisselbaar geheugen en de delta invoer bevat.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Het script informeert over het soort importeren uitvoeren (delta of volledig) partitie, hiërarchie, watermerk en verwachte paginaformaat.
Typen | [Schema][schema] | Schema voor de verbindingslijn die wordt geïmporteerd.

Het importeren van gegevensscript moet schrijven een lijst [[CSEntryChange][csec]] de pipeline-object. Deze collectie bestaat uit CSEntryChange-kenmerken voor elk object dat wordt geïmporteerd. Tijdens een volledige importbewerking uitvoeren, moet deze collectie hebben een volledige reeks CSEntryChange-objecten die alle kenmerken voor elk object. Tijdens een Delta importeert, moet het CSEntryChange-object het kenmerk niveau delta's voor elk object dat u wilt importeren of een volledige weergave van de objecten die zijn gewijzigd (de modus vervangen) bevatten.

**Einde importeren**  
Aan het einde van de importbewerking uitvoeren, wordt het importeren van einde script uitgevoerd. Dit script moet opruimen taken nodig zijn (bijvoorbeeld nauwe verbindingen met systemen) en reageren op fouten uitvoeren.

Het importeren van einde script ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel van de configuratieparameters voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Referenties die zijn opgegeven door de beheerder op het tabblad verbinding bevat.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Het script informeert over het soort importeren uitvoeren (delta of volledig) partitie, hiërarchie, watermerk en verwachte paginaformaat.
CloseImportConnectionRunStep | [CloseImportConnectionRunStep][cecrs] | Het script informeert over de reden dat het importeren is beëindigd.

Het script moet resulteren in een enkele [CloseImportConnectionResults] [ cicres] object toe aan de pipeline, bijvoorbeeld:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Exporteren
Identiek aan de architectuur van het importeren van de verbindingslijn, verbindingslijnen die ondersteuning bieden voor Export hanteert drie scripts.

**Beginnen met het exporteren**  
Het begin exportscript wordt uitgevoerd aan het begin van een stap exporteren uitvoeren. Tijdens deze stap kunt u verbinding maken met het bronsysteem en eventuele voorbereidende stappen uitvoeren voordat u gegevens exporteert met het systeem verbonden.

Het begin exportscript ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel van de configuratieparameters voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Referenties die zijn opgegeven door de beheerder op het tabblad verbinding bevat.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Het script informeert over het type partitie exporteren uitvoeren (delta of volledig), hiërarchie en verwachte paginaformaat.
Typen | [Schema][schema] | Schema voor de verbindingslijn die wordt geëxporteerd.

Het script moet de pijpleiding niet uitvoer weer.

**Gegevens exporteren**  
De synchronisatieservice roept het script exporteert gegevens zo vaak als nodig is voor het verwerken van alle uitvoer van in behandeling is. Als de verbindingslijn ruimte meer in behandeling uitvoer dan het paginaformaat van de verbindingslijn, kan het script voor gegevens exporteren naam meerdere keren en mogelijk meerdere malen voor hetzelfde object.

Het script exporteren gegevens ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel van de configuratieparameters voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Referenties die zijn opgegeven door de beheerder op het tabblad verbinding bevat.
CSEntries | IList[CSEntryChange][csec] | Lijst van alle de connector ruimteobjecten in afwachting van de uitvoer moet worden verwerkt tijdens deze fase.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Het script informeert over het type partitie exporteren uitvoeren (delta of volledig), hiërarchie en verwachte paginaformaat.
Typen | [Schema][schema] | Schema voor de verbindingslijn die wordt geëxporteerd.

De gegevens exporteren script moet resulteren in een [PutExportEntriesResults] [ peeres] object op de pijpleiding. Dit object hoeft niet op te nemen informatie voor elke geëxporteerde connector resultaat tenzij er een fout of een wijziging in het ankerkenmerk optreedt. Als u bijvoorbeeld een PutExportEntriesResults-object als resultaat gegeven met de pijpleiding:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Einde exporteren**  
Aan het einde van de export uitvoeren, het exporteren van einde script wilt uitvoeren. Dit script moet opruimen taken nodig zijn (bijvoorbeeld nauwe verbindingen met systemen) en reageren op fouten uitvoeren.

De export einde script ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel van de configuratieparameters voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Referenties die zijn opgegeven door de beheerder op het tabblad verbinding bevat.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Het script informeert over het type partitie exporteren uitvoeren (delta of volledig), hiërarchie en verwachte paginaformaat.
CloseExportConnectionRunStep | [CloseExportConnectionRunStep][cecrs] | Het script informeert over de reden dat het exporteren is beëindigd.

Het script moet de pijpleiding niet uitvoer weer.

#### <a name="password-synchronization"></a>Wachtwoordsynchronisatie
Windows PowerShell verbindingslijnen kunnen worden gebruikt als een doel voor wachtwoorden wijzigingen.

Het script wachtwoord ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel van de configuratieparameters voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Referenties die zijn opgegeven door de beheerder op het tabblad verbinding bevat.
Partitie | [Partitie][part] | Active Directory-partitie die de CSEntry in.
CSEntry | [CSEntry][cse] | Connector ruimte gegeven voor het object dat is ontvangen op een wachtwoord wijzigen of opnieuw instellen.
OperationType | Tekenreeks | Geeft aan of de bewerking een reset (**SetPassword**) of een wijziging (**ChangePassword is**).
PasswordOptions | [PasswordOptions][pwdopt] | Vlaggen die aangeven van het bedoelde wachtwoord opnieuw instellen van gedrag. Deze parameter is alleen beschikbaar als OperationType **SetPassword**.
OudWachtwoord | Tekenreeks | Gevuld met het oude wachtwoord voor het wijzigen van wachtwoorden van het object. Deze parameter is alleen beschikbaar als OperationType **ChangePassword**.
NewPassword | Tekenreeks | Gevuld met het nieuwe wachtwoord van het object die het script moet worden ingesteld.

Het script van het wachtwoord wordt niet verwacht zoekresultaten met de Windows PowerShell-pijplijn. Als een fout in het script wachtwoord optreedt, moet een van de volgende uitzonderingen op de synchronisatieservice informeren over het probleem door het script genereren:

- [PasswordPolicyViolationException] [ pwdex1] – als het wachtwoord niet voldoet aan het wachtwoordbeleid in het verbonden systeem veroorzaakt.
- [PasswordIllFormedException] [ pwdex2] – gegenereerd als het wachtwoord niet geschikt voor het verbonden systeem is.
- [PasswordExtension] [ pwdex3] – voor alle fouten in het script wachtwoord gegenereerd.

## <a name="sample-connectors"></a>Voorbeeld-Connectors
Zie [Windows PowerShell Connector monster Connector-collectie]voor een volledig overzicht van de beschikbare sample-connectors,[samp].

## <a name="other-notes"></a>Andere opmerkingen

### <a name="additional-configuration-for-impersonation"></a>Aanvullende configuratie voor imitatie
Verleen de gebruiker die wordt geïmiteerd voor de tijdsynchronisatie-Service-server de volgende machtigingen:

Leestoegang tot de volgende registersleutels:

- HKEY_USERS\\\Software\Microsoft\PowerShell [SynchronizationServiceServiceAccountSID]
- HKEY_USERS\\\Environment [SynchronizationServiceServiceAccountSID]

Om te bepalen van de SID (Security Identifier) van de tijdsynchronisatie-Service-account, voert u de volgende PowerShell-opdrachten:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Leestoegang tot de volgende mappen van het bestandssysteem:

- %ProgramFiles%\Microsoft forefront identiteit Manager\2010\Synchronization Service\Extensions
- %ProgramFiles%\Microsoft forefront identiteit Manager\2010\Synchronization Service\ExtensionsCache
- %ProgramFiles%\Microsoft forefront identiteit Manager\2010\Synchronization Service\MaData\\{ConnectorName}

De naam van de Windows PowerShell-connector vervangen door een tijdelijke aanduiding voor de {ConnectorName}.

## <a name="troubleshooting"></a>Het oplossen van problemen

-   Zie voor meer informatie over het inschakelen van logboekregistratie voor het oplossen van de connector [hoe inschakelen ETW-tracering voor verbindingslijnen](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
