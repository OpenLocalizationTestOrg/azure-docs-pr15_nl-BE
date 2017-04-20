<properties
   pageTitle="Meer informatie over en maken een BizTalk-regels API-app in de App logica | Microsoft Azure"
   description="Dit onderwerp omvat de functies van de regels van de BizTalk-connector en vindt u instructies voor het gebruik ervan"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="andalmia"/>

#<a name="biztalk-rules"></a>BizTalk-regels

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Bedrijfsregels kapselt het beleid en de besluiten die bedrijfsprocessen te beheren. Dit beleid formeel in procedure handleidingen, contracten of overeenkomsten kunnen worden gedefinieerd, of als kennis of expertise die zijn vervat in de werknemers bestaat. Deze beleidsregels zijn dynamisch en worden gewijzigd via tijd toe te schrijven aan wijzigingen in de bedrijfsplannen, regelgeving of andere redenen.

Tenuitvoerlegging van dit beleid in de traditionele programmeertalen vereist veel tijd en coördinatie en niet-programmeurs deel te nemen aan het maken en onderhouden van bedrijfsbeleid wordt niet ingeschakeld. BizTalk-zakelijke regels kunt u snel implementeren van dit beleid en het loskoppelen van de rest van het bedrijfsproces. Hierdoor kunnen wijzigingen vereist bedrijfsbeleid zonder invloed op de rest van het bedrijfsproces.

##<a name="why-rules"></a>Waarom regels

Er zijn 3 belangrijke redenen BizTalk bedrijfsregels in bedrijfsproces te gebruiken:

* Loskoppelen van de bedrijfslogica van de toepassingscode
- Analisten hebben meer controle over het beheer van zakelijke logica toestaan
+ Wijzigingen in de bedrijfslogica Ga sneller naar productie

##<a name="rules-concepts"></a>Regels concepten

##<a name="vocabulary"></a>Woordenlijst

Een _woordenlijst_ is een verzameling van definities die bestaat uit een beschrijvende naam voor de computer-objecten gebruikt in regelvoorwaarden en acties. Vocabulaire definities gemakkelijker de regels te lezen, te begrijpen en te delen met mensen in een bepaalde business-domein.

Woordenlijsten zijn ontworpen om de kloof tussen de zakelijke semantiek en uitvoering. Een gegevensbinding voor de goedkeuringsstatus kan bijvoorbeeld verwijzen naar een bepaalde kolom in een bepaalde rij in een bepaalde database weergegeven als een SQL-query. In plaats van dit soort complexe vertegenwoordiging in een regel, maakt u in plaats daarvan een vocabulaire definiëren, die is gekoppeld aan die gegevensbinding met een beschrijvende naam 'Status'. U kunt vervolgens 'Status' opnemen in een onbeperkt aantal regels en de regel-engine de bijbehorende gegevens kunt ophalen uit de tabel.

##<a name="rule"></a>Regel

Bedrijfsregels zijn declaratieve instructies voor het uitvoeren van bedrijfsprocessen. Een regel bestaat uit een voorwaarde en acties. De voorwaarde is geëvalueerd en als dit in waar resulteert, de motor van de regel een of meer acties gestart.
Regels in het kader van zakelijke regels worden gedefinieerd met behulp van de volgende notatie:

_IF_ _voorwaarde_ _Vervolgens_ _actie_

Houd rekening met het volgende voorbeeld:

*Als bedrag kleiner is dan of gelijk aan beschikbare middelen*  
*Uitvoeren van transactie en een ontvangstbewijs afdrukken*

Deze regel bepaalt of een transactie wordt uitgevoerd door het toepassen van de bedrijfslogica in de vorm van een vergelijking van twee monetaire waarden in de vorm van een transactiebedrag en de beschikbare middelen.
U kunt de bedrijfsregel maken, aanpassen en implementeren. Ook kunt u de voorafgaande taken via de programmacode uitvoeren.

###<a name="conditions"></a>Voorwaarden

Een voorwaarde is een waar/onwaar (Boolean)-expressie die uit een of meer predikaten bestaat.
In dit voorbeeld wordt het predikaat kleiner dan of gelijk aan toegepast op het bedrag en de beschikbare middelen. Deze voorwaarde wordt altijd waar of ONWAAR als resultaat.
Predikaten kunnen worden gecombineerd met de logische operatoren AND, OR en niet op een logische expressie die potentieel erg groot is, maar altijd wordt geëvalueerd als waar of ONWAAR.

###<a name="actions"></a>Acties

Acties zijn de functionele gevolgen van de evaluatie van de voorwaarde. Als aan een voorwaarde wordt voldaan, wordt een overeenkomstige actie of acties gestart.
In ons voorbeeld zijn "verloop transactie" en "ontvangstbewijs afdrukken" acties die worden uitgevoerd als en slechts als de voorwaarde (in dit geval "als bedrag kleiner is dan of gelijk aan beschikbare middelen') is ingesteld op true.
Acties in het kader van zakelijke regels vertegenwoordigd door het uitvoeren van bewerkingen in XML-documenten ingesteld.

##<a name="policy"></a>Beleid

Een beleid is een logische groepering van regels. U een beleid opstellen, opslaan, het testen en, als u tevreden met de resultaten bent gebruiken in een productieomgeving.

###<a name="policy-composition"></a>Samenstelling van beleid

U kunt beleid in de portal regels opstellen. Een beleid kan een willekeurig grote reeks regels bevatten, maar meestal het opstellen van een beleid van regels die betrekking hebben op een specifiek business-domein in het kader van de toepassing die van het beleid gebruikmaakt.

###<a name="policy-testing"></a>Beleid testen
U kunt een testuitvoering van uw beleid doeltreffend uitvoeren voordat het wordt gebruikt in een productieomgeving. De regels Portal kunt u om invoer voor een beleid, uitvoeren van het beleid en de uitvoer weergeven. De uitvoer bevat de logboeken, de uitvoering van de regel, evaluatie, en de uiteindelijke uitvoer.

##<a name="sample-scenario---insurance-claims"></a>Voorbeeldscenario - schadeclaims
We nemen een voorbeeldscenario en wandel door het als we de bedrijfslogica voor dezelfde opstellen.

![ALT-tekst][1]

De aanvrager dient in een echt eenvoudige scenario verzekeringsclaims verzekering vordering (via een client zoals website, telefoon-App, etc). Deze aanvraag voor een Claim wordt verzonden naar het bedrijf van de Claim Processing Unit en op basis van het resultaat van de verwerking, de Claim kan beide goedgekeurd, afgekeurd of verzonden langs voor verdere handmatige verwerking.
De Claim verwerken van eenheid in ons scenario zou zijn dat omvat de bedrijfslogica voor het systeem. Nadere kennismaking met deze eenheid te nemen, kunnen we het volgende zien:

![ALT-tekst][2]

Nu we zakelijke regels gebruiken om deze bedrijfslogica te implementeren.


##<a name="creation-of-rules-api-app"></a>Het maken van regels Api App


1. Meld u aan bij de Azure Portal
2. Selecteer Nieuw -> Marketplace zoekt u naar *BizTalk-regels*
3. Selecteert u BizTalk-regels in de lijst met resultaten. Hiermee opent u de regels van de BizTalk-blade
4. Klik op de knop *maken* ![Alt-tekst][3]
1. In het nieuwe blad dat wordt geopend, voert u de volgende informatie:  
    1. Naam: Geef een naam voor uw regels API App
    1. App Service Plan – Selecteer of maak een nieuwe App Service Plan
    1. Prijzen laag – Kies de prijzen laag die u wilt dat deze toepassing zich bevinden binnen
    1. Resourcegroep: selecteren of maken waarin de toepassing zich moet bevinden in resourcegroep
    2. Abonnement - Selecteer het gewenste abonnement
    1. Locatie: Kies de geografische locatie waar u de toepassing dat wilt moet worden geïmplementeerd.
4.  Selecteer *maken*. Binnen enkele minuten zou uw BizTalk-regels API App worden gemaakt.

##<a name="vocabulary-creation"></a>Woordenlijst maken
De volgende stap zou woordenlijsten maken na het maken van een BizTalk-regels API App. De verwachting is dat de ontwikkelaar de algemene persona voor deze oefening doen. Hier ziet u hoe u dit gedaan:


1. Start uw BizTalk API App regels vanaf de portal via bladeren -> API Apps - ><Your Rules API App>. Hierdoor bent u aan het Dashboard regels API App zoals hieronder:

   ![ALT-tekst][4]

2. Selecteer 'Definities woordenlijst'. Dit wordt aangegeven met de woordenlijst Authoring scherm 3. Select 'toevoegen' om te beginnen met het toevoegen van nieuwe definities van de woordenschat.
Er zijn 2 typen vocabulaire definities die momenteel worden ondersteund: letterlijke en XML.

##<a name="literal-definition"></a>Letterlijke definitie
1.  Na het klikken op 'Toevoegen', een nieuwe 'Definitie toevoegen' Blade wordt geopend. Voer de volgende waarden
  1.    Naam: alleen alfanumerieke tekens zonder speciale tekens worden verwacht. Dit moet ook aan de lijst met bestaande vocabulaire definitie uniek zijn.
  2.    Omschrijving: optioneel veld.
  3.    Definitie: Er zijn 2 typen die worden ondersteund. In dit voorbeeld kiest u letterlijk
  4.    Gegevenstype: Hiermee kunnen gebruikers selecteert u het gegevenstype van de definitie. Op dit moment 4-gegevenstypen worden ondersteund: ik.  String – deze waarden moeten tussen dubbele aanhalingstekens ("voorbeeld String") worden ingevoerd.  
    II. Boole-waarde: dit is waar of ONWAAR  
    III.    Getal-een decimaal getal  
    IV. Datum/tijd: dit betekent dat de def van het type datumtype. Gegevens moeten worden ingevoerd met behulp van deze indeling: dd-mm-jjjj: mm: ss AM\PM  
  5. Input: dit is waar u de waarde van de definitie. De waarden die u hier opgeeft, moeten overeenkomen met het gekozen gegevenstype. U kunt een enkele waarde, een reeks waarden, gescheiden door komma's of een bereik van waarden met het trefwoord *wilt*invoeren. Bijvoorbeeld, kunt u unieke waarde 1; een set 1, 2, 3; of een bereik van 1 tot en met 5. Merk op dat bereik wordt alleen ondersteund voor getallen.
  6. Klik op *OK*.

![ALT-tekst][5]
##<a name="xml-definition"></a>XML-definitie
Als het Type woordenlijst als XML wordt gekozen, de volgende ingangen moet worden opgegeven  
  een.    Schema: klikken op dit opent nieuwe blade zodat gebruikers kiezen uit een lijst met al geüpload schema's of toestaan voor het uploaden van een nieuw.
b.    Deze haalt de XPATH-invoer ontgrendeld na het kiezen van een schema in de vorige stap. Als u op dit wordt het schema dat is geselecteerd en selecteer het knooppunt waarvoor een woordenlijst definitie moet worden gemaakt van de gebruiker kan worden weergegeven.  
  c.    FEIT: deze invoer wordt aangeduid met het gegevensobject zou worden ingevoerd in de regels-engine voor verwerking. Dit is een geavanceerde eigenschappen en is standaard ingesteld op het bovenliggende object van het geselecteerde XPATH. FEIT is met name belangrijk voor scenario's koppelen en de collectie.

![ALT-tekst][6]

### <a name="add-bulk"></a>Bulk toevoegen
De bovenstaande stappen hebt vastgelegd de ervaring voor het maken van definities van de woordenschat. Eenmaal gemaakt, weergegeven de definities woordenlijst gemaakt in het formulier. Zijn er vereisten kunnen meerdere definities genereren uit hetzelfde schema in plaats van telkens één van de bovenstaande stappen herhalen. Dit is waar Bulk toevoegen mogelijkheid wordt erg handig.
"Bulk toevoegen" selecteren, gaat u naar een nieuwe blade. De eerste stap is het selecteren van het schema waarvoor meerdere definities worden gemaakt. Als u deze opent een nieuwe blade zodat u te kiezen uit een lijst van al de geüploade schema's of voor het uploaden van een nieuw toestaan.
De eigenschap XPath haalt nu ontgrendeld. Hierdoor wordt het Schema Viewer geopend waarin meerdere knooppunten kunnen worden geselecteerd.
De namen voor de verschillende definities die gemaakt wordt standaard de naam van het knooppunt dat is geselecteerd. Deze kunnen altijd worden gewijzigd nadat het is gemaakt.

![ALT-tekst][7]

##<a name="policy-creation"></a>Beleid maken
Zodra de ontwikkelaar de vereiste vocabularies gemaakt heeft, is de verwachting dat de bedrijfsanalist één maken bedrijfsbeleid via de Portal Azure zou zijn.  
    1. Klik op de regels App gemaakt, is er een beleid lens op die de gebruiker gaat naar de pagina voor het maken van beleid.  
    2. deze ziet u de lijst met beleidsregels dit App met bepaalde regels heeft. De analist kunt u een nieuw beleid toevoegen door gewoon een naam te typen en tab twee keer raken. Meerdere beleidsregels kunnen zich bevinden in een enkele regels API App.  
    3. de gemaakte beleid te selecteren, gaat de gebruiker naar de pagina beleid waarbij de regels die in het beleid kunnen zien.  
    ![ALT-tekst][8]
 4.  Selecteer 'Toevoegen' om een nieuwe regel toevoegen. Hiermee gaat u naar een nieuwe blade.

##<a name="rule-creation"></a>Regel maken
Een regel is een verzameling instructies voorwaarde en actie. De acties worden uitgevoerd als de voorwaarde in waar resulteert. Geef een unieke naam (voor dat beleid) en beschrijving (optioneel) in het blad regel maken.
Het vak voorwaarde (IF) kan worden gebruikt voor het maken van complexe voorwaardelijke instructies. Hieronder staan de sleutelwoorden ondersteund:  
1.  En – de voorwaardelijke operator  
2.  Of – voorwaardelijke operator  
3.  biedt\_niet\_bestaat  
4.  Er bestaat  
5.  False  
6.  is\_gelijk\_aan  
7.  is\_groter\_dan  
8.  is\_groter\_dan\_gelijk\_aan  
9.  is\_in  
10. is\_minder\_dan  
11. is\_minder\_dan\_gelijk\_aan  
12. is\_niet\_in  
13. is\_niet\_gelijk\_aan  
14. Mod  
15. True  

Het vak Action(THEN) kan bevatten meerdere instructies, één per regel maken de acties die moeten worden uitgevoerd. Hieronder staan de sleutelwoorden ondersteund:  
1.  is gelijk aan  
2.  False  
3.  True  
4.  stoppen  
5.  Mod  
6.  Null  
7.  Update  

De voorwaarde en een actie uit bieden Intellisense waarmee u snel een regel schrijven. Dit kan worden veroorzaakt door kunt u door op ctrl + space of net begint te typen. Trefwoorden overeenkomen met getypte tekens worden omlaag gefilterd en weergegeven. Alle trefwoorden en vocabulaire definities weergegeven de intellisense-venster.
![ALT-tekst][9]

##<a name="explicit-forward-chaining"></a>Expliciete Forward Chaining
BizTalk-regels ondersteunt expliciete, forward chaining als gebruikers regels in reactie op bepaalde acties opnieuw evalueren, ze kunnen resulteren in dit door bepaalde trefwoorden te gebruiken. Dit zijn de sleutelwoorden ondersteund:  
   1.   Update <vocabulary definition> – dit trefwoord opnieuw alle regels die de definitie van de opgegeven woordenlijst in de voorwaarde gebruikt wordt geëvalueerd.  
   2.   Stilstand – dit trefwoord stopt alle uitvoeringen van de regel

##<a name="enabledisable-rules"></a>Enable\Disable regels
Elke regel in het beleid kan worden ingeschakeld of uitgeschakeld. Alle regels zijn standaard ingeschakeld. Uitgeschakelde regels niet worden uitgevoerd tijdens de uitvoering van beleid. Enable\Disable regels kunnen worden gedaan van de bladeserver regel direct – de opdrachten zijn beschikbaar in de werkbalk boven aan het blad of van het beleid, het contextmenu (Klik met de rechtermuisknop op een regel) heeft de optie voor het enable\disable.

##<a name="rule-priority"></a>Regelprioriteit
De regels van een beleid worden in volgorde uitgevoerd. De prioriteit van de uitvoering wordt bepaald door de volgorde waarin ze in het beleid voorkomen. Deze prioriteit kan worden gewijzigd door simpelweg te slepen en neer te zetten van de regel.

##<a name="test-policy"></a>Beleid testen
U kunt uw beleid testen met behulp van de opdracht 'Test Policy' in de blade beleid testen. Deze blade bevat een overzicht van vocabulaire definities die worden gebruikt in het beleid waarvoor invoer van de gebruiker. Gebruikers kunnen de waarden voor deze invoer voor de Testscenario handmatig toevoegen. U kunt ook gebruikers kunt importeren XMLs testen voor invoer. Zodra alle ingangen, de test kan worden uitgevoerd en de resultaten voor de definitie van elke woordenlijst wordt weergegeven in de uitvoerkolom voor een eenvoudige vergelijking. Bedrijfsanalist beschrijvende Logboeken, klikt u op op 'Logboeken' weer te geven van de uitvoering van Logboeken. Als u wilt opslaan in de logboeken, de optie 'Output opslaan' is beschikbaar voor het opslaan van alle verwante gegevens voor onafhankelijke analyse test.

## <a name="using-rules-in-logic-apps"></a>Met behulp van regels in de logica Apps
Nadat het beleid is ontworpen en getest, is het nu klaar zijn voor consumptie. U kunt een nieuwe logica App maken door logica Apps vanaf de linkerkant van de introductiepagina van de portal. Zodra uw App logica is gemaakt, starten en selecteer vervolgens *Triggers en acties*. Vervolgens kunt u de sjabloon *maken* . Volg de stappen om uw BizTalk-regels API App App logica toevoegen. Zodra dit is gebeurd, zal er een optie om te kiezen welke regels API App (actie) naar het doel. Acties omvatten de lijst beleidsregels die moeten worden uitgevoerd. Kies een specifiek beleid na de invoer nodig is voor het beleid moet worden gegeven. Gebruikers kunnen de uitvoer van de downstream regels API-App gebruiken voor verdere besluitvorming.

## <a name="using-rules-via-apis"></a>Met behulp van regels via API 's
De regels API App kunnen ook worden opgeroepen met behulp van een uitgebreide set van API's. Deze manier waarop gebruikers zijn niet beperkt tot alleen maar Apps logica gebruikt en regels in een willekeurige toepassing kunnen gebruiken door het aanroepen van de REST. De exacte REST-API's beschikbaar kunnen worden bekeken door te klikken op de lens "API afgebakend" in het dashboard van de regels.

![ALT-tekst][10]

Hierna volgt een voorbeeld van hoe een gebruik van deze API in C#

            // Constructing the JSON message to use in API call to Rules API App

            // xmlInstance is the XML message instance to be passed as input to our Policy
            string xmlInstance = "<ns0:Patient xmlns:ns0=\"http://tempuri.org/XMLSchema.xsd\">  <ns0:Name>Name_0</ns0:Name>  <ns0:Email>Email_0</ns0:Email>  <ns0:PatientID>PatientID_0</ns0:PatientID>  <ns0:Age>10.4</ns0:Age>  <ns0:Claim>    <ns0:ClaimDate>2012-05-31T13:20:00.000-05:00</ns0:ClaimDate>    <ns0:ClaimID>10</ns0:ClaimID>    <ns0:TreatmentID>12</ns0:TreatmentID>    <ns0:ClaimAmount>10000.0</ns0:ClaimAmount>    <ns0:ClaimStatus>ClaimStatus_0</ns0:ClaimStatus>    <ns0:ClaimStatusReason>ClaimStatusReason_0</ns0:ClaimStatusReason>  </ns0:Claim></ns0:Patient>";

            JObject input = new JObject();

            // The JSON object is to be of form {"<XMLSchemName>_<RootNodeName>":"<XML Instance String>".
            // In the below case, we are using XML Schema - "insruanceclaimsschema" and the root node is "Patient".
            // This is CASE SENSITIVE.
            input.Add("insuranceclaimschema_Patient", xmlInstance);
            string stringContent = JsonConvert.SerializeObject(input);


            // Making REST call to Rules API App
            HttpClient httpClient = new HttpClient();

            // The url is the Host URL of the Rules API App
            httpClient.BaseAddress = new Uri("https://rulesservice77492755b7b54c3f9e1df8ba0b065dc6.azurewebsites.net/");
            HttpContent httpContent = new StringContent(stringContent);
            httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json");

            // Invoking API "Execute" on policy "InsruranceClaimPolicy" and getting response JSON object. The url can be gotten from the API Definition Lens
            var postReponse = httpClient.PostAsync("api/Policies/InsuranceClaimPolicy?comp=Execute", httpContent).Result;

U ziet dat de bovenstaande regels API App beveiligingsinstellingen zijn ingesteld op "Anon openbare". Dit kan worden ingesteld van binnen de App API met - alle instellingen -> Instellingen -> toegangsniveau.

![ALT-tekst][11]

## <a name="editing-vocabulary-and-policy"></a>Beleid en woordenlijst bewerken
Een van de belangrijkste voordelen van het gebruik van zakelijke regels is dat wijzigingen in de zakelijke logica kunnen naar buiten wijzen voor de productie van een stuk sneller. Elke wijziging vocabulaire en het beleid wordt onmiddellijk toegepast in productie. Gebruiker hoeft alleen maar om te bladeren naar de respectieve vocabulaire definitie of het beleid en de wijziging van kracht te.

<!--Image references-->
[1]: ./media/app-service-logic-use-biztalk-rules/InsuranceScenario.PNG
[2]: ./media/app-service-logic-use-biztalk-rules/InsuranceBusinessLogic.png
[3]: ./media/app-service-logic-use-biztalk-rules/CreateRuleApiApp.png
[4]: ./media/app-service-logic-use-biztalk-rules/RulesDashboard.png
[5]: ./media/app-service-logic-use-biztalk-rules/LiteralVocab.PNG
[6]: ./media/app-service-logic-use-biztalk-rules/XMLVocab.PNG
[7]: ./media/app-service-logic-use-biztalk-rules/BulkAdd.PNG
[8]: ./media/app-service-logic-use-biztalk-rules/PolicyList.PNG
[9]: ./media/app-service-logic-use-biztalk-rules/RuleCreate.PNG
[10]: ./media/app-service-logic-use-biztalk-rules/APIDef.PNG
[11]: ./media/app-service-logic-use-biztalk-rules/PublicAnon.PNG
