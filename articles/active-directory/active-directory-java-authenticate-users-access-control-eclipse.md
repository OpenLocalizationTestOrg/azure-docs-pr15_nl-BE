<properties
    pageTitle="Het gebruik van Access Control (Java) | Microsoft Azure"
    description="Informatie over het ontwikkelen en gebruiken van Access Control met Java in Azure."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-authenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Hoe webgebruikers worden geverifieerd met Azure Access Control-Service met behulp van de Eclips

Deze handleiding leert hoe u de Azure Access Control Service (ACS) binnen de Azure-Toolkit voor Eclips. Zie de sectie [volgende stappen](#next_steps) voor meer informatie over de ACS.

> [AZURE.NOTE]
> Het besturingselement Azure Access Services-Filter is een voorbeeld van de technologie Gemeenschap. Als pre-release software, is het niet officieel ondersteund door Microsoft.

## <a name="what-is-acs"></a>Wat de ACS is?

De meeste ontwikkelaars zijn niet identiteit deskundigen en over het algemeen niet wilt besteden tijd ontwikkelen verificatie en autorisatie mechanismen voor hun toepassingen en services. ACS is een Azure service een eenvoudige manier biedt van verificatie van gebruikers die toegang krijgen tot uw webtoepassingen en -services moeten zonder factor authentication complexe logica in uw code.

De volgende functies zijn beschikbaar in de ACS:

-   Integratie met Windows Identity Foundation (WIF).
-   Ondersteuning voor populaire web identiteit aanbieders (IPs), zoals Windows Live ID, Google, Yahoo! en Facebook.
-   Ondersteuning voor Active Directory Federation Services (AD FS) 2.0.
-   Een Open Data Protocol (OData)-management-service die toegang op programmeerniveau tot ACS-instellingen biedt.
-   Een Portal Management administratieve toegang tot de ACS-instellingen.

Zie [Access Control Service 2.0][]voor meer informatie over de ACS.

## <a name="concepts"></a>Concepten

Azure ACS is gebaseerd op de beveiligings-principals op claims gebaseerde identiteit - een consistente benadering voor het maken van verificatiemechanismen voor toepassingen die worden uitgevoerd in ruimten of in de cloud. Op claims gebaseerde identiteit biedt een algemene manier voor toepassingen en services te verkrijgen van de identiteit de benodigde informatie over gebruikers binnen hun organisatie, in andere organisaties en op het Internet.

Om de taken in deze handleiding hebt voltooid, moet u de volgende concepten begrijpen:

**Client** - In het kader van deze procedures handleiding, dit is een browser die probeert toegang te krijgen tot uw webtoepassing.

**Toepassing Relying (RP)** - toepassing een RP is een website of service die verificatie van een externe autoriteit heeft. Identiteit jargon zeggen we dat de RP die instantie vertrouwt. Deze handleiding wordt uitgelegd hoe u uw toepassing configureren voor vertrouwde ACS.

**Token** - token is een verzameling van van beveiligingsgegevens die meestal na een geslaagde verificatie van een gebruiker is uitgegeven. Het bevat een aantal *claims*, kenmerken van de geverifieerde gebruiker. Een claim kan de naam van een gebruiker vertegenwoordigen, een id voor een rol van een gebruiker behoort tot de leeftijd van de gebruiker en enzovoort. Een token is meestal digitaal ondertekend, wat betekent dat die kan altijd terug naar de uitgever afkomstig en de inhoud ervan kan worden geknoeid. Een gebruiker krijgt toegang tot een toepassing RP door overlegging van een geldige token is uitgegeven door een instantie die door de toepassing RP wordt vertrouwd.

**Identity Provider (IP)** - een IP is een instantie die de identiteit van gebruikers worden geverifieerd en beveiligingstokens. De werkelijke hoeveelheid werk van het uitgeven van tokens wordt geïmplementeerd als een speciale service Security Token Service (STS) genoemd. Typische voorbeelden van IP-adressen zijn Windows Live ID, Facebook en opslagplaatsen van de zakelijke gebruiker (zoals Active Directory).
ACS is geconfigureerd voor vertrouwen IP, zal het systeem accepteren als tokens die zijn uitgegeven door die IP valideren. ACS kan meerdere IP-adressen in een keer, wat betekent dat wanneer uw toepassing ACS vertrouwt, u direct uw toepassing op alle geverifieerde gebruikers uit alle IP-adressen bieden kunt dat ACS-vertrouwensrelaties namens u kunt vertrouwen.

**Federation-Provider (FP)** - directe kennis hebben van gebruikers, en ze met hun referenties verifiëren en uitgeven claims over wat ze over deze weten IP-adressen. Een Federation-Provider (FP) is een ander soort dienst: in plaats van rechtstreeks verifiëren van gebruikers, het fungeert als een tussenpersoon en makelaars-verificatie tussen een RP en een of meer IP-adressen. Zowel de IP-adressen en FPs beveiligingstokens, dus ze beide Security Token Services (STS) gebruiken. ACS is een LCD-Schermen.

**ACS regel Engine** - de logica voor het transformeren van binnenkomende tokens van vertrouwde IP-adressen met tokens die zijn bedoeld om te worden gebruikt door de RP is overgegaan in de vorm van regels voor eenvoudige claims transformatie. ACS is uitgerust met een motor regel die zorgt voor het toepassen van de transformatie logica die u hebt opgegeven voor uw RP.

De **Namespace van de ACS** - een naamruimte is een partitie van het hoogste niveau van ACS die u gebruikt voor het ordenen van uw instellingen. Een naamruimte bevat een lijst met IP-adressen u vertrouwt de RP-toepassingen die u wilt gebruiken, de regels waarop u verwacht dat de regel voor het verwerken van binnenkomende tokens met de motor, enzovoort. Een naamruimte beschrijft verschillende eindpunten die wordt gebruikt door de toepassing en de ontwikkelaar van de ACS voor het uitvoeren van de functie opvragen.

In de volgende afbeelding ziet u hoe de ACS-verificatie werkt met een webtoepassing:

![ACS-stroomdiagram][acs_flow]

1.  De client (in dit geval een browser) wordt een pagina opvraagt bij de RP.
2.  Omdat de aanvraag nog niet zijn geverifieerd, de RP wordt de gebruiker omgeleid naar de instantie die de, wordt ACS. De ACS biedt de gebruiker de keuze van de IP-adressen die zijn opgegeven voor deze RP. De gebruiker selecteert de juiste IP.
3.  De client bladert naar de pagina verificatie van de IP- en vraagt de gebruiker aan te melden.
4.  Nadat de client is geverifieerd (bijvoorbeeld de identiteit referenties worden ingevoerd), problemen met het OT een beveiligingstoken.
5.  Na het verzenden van een beveiligingstoken het OT, wordt de client omgeleid naar de ACS en de client stuurt het beveiligingstoken uitgegeven door de IP aan ACS.
6.  ACS valideert het beveiligingstoken uitgegeven door de IP, inputs de identiteit vorderingen in deze token in de ACS-regels-engine, berekent de output identiteitsclaims en een nieuwe beveiligingssleutel deze claims uitvoer met problemen.
7.  ACS wordt de client omgeleid naar de RP. De client stuurt het nieuwe beveiligingstoken is uitgegeven door ACS aan het RP. De RP valideert de handtekening op het beveiligingstoken uitgegeven door ACS, evalueert de vorderingen in dit token en wordt de pagina die aanvankelijk was aangevraagd.

## <a name="prerequisites"></a>Vereisten

Als u de taken in deze handleiding wilt nodig u het volgende:

- Een Java Developer Kit (JDK), v 1.6 of hoger.
- Voor ontwikkelaars van Java EE, IDE-Eclips Indigo of hoger. Dit kan worden gedownload van <http://www.eclipse.org/downloads/>. 
- Een verdeling van een Java-webserver of een toepassing zoals Apache Tomcat, GlassFish, JBoss-toepassingsserver of Jetty.
- een Azure-abonnement, die kan worden verkregen uit <http://www.microsoft.com/windowsazure/offers/>.
- De Toolkit Azure voor Eclips, April 2014 vrijgeven of hoger. Zie [de Azure Toolkit voor Eclips installeren](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx)voor meer informatie.
- Een x.509-certificaat te gebruiken met uw toepassing. U moet dit certificaat in openbare certificaat (CER) en Personal Information Exchange (. (PFX)-indeling. (De opties voor het maken van dit certificaat wordt verderop in deze handleiding beschreven).
- Bekendheid met de Azure berekenen emulator en deployment technieken besproken in het [maken van een Hello World-toepassing voor Azure in Eclips](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx).

## <a name="create-an-acs-namespace"></a>Maken van een ACS-Namespace

Om te beginnen met behulp van Access Control-Service (ACS) in Azure, maakt u een naamruimte ACS. De naamruimte biedt een unieke bereik voor het adresseren van ACS-bronnen uit binnen de toepassing.

1. Meld u aan bij de [Azure Management Portal][].
2. Klik op **Active Directory**. 
3. Wilt maken van een nieuwe naamruimte voor toegangsbeheer, klikt u op **Nieuw**, **App Services**klikt u op, klikt u op **Toegangsbeheer**en klik op **Snelle invoer**. 
4. Voer een naam voor de naamruimte. Azure wordt gecontroleerd of de naam uniek is.
5. Selecteer het gebied waarin de naamruimte wordt gebruikt. Gebruik voor de beste prestaties, de regio waarin u uw toepassing distribueert.
6. Als u meer dan één abonnement hebt, selecteert u het abonnement dat u wilt gebruiken voor de ACS-naamruimte.
7. Klik op **maken**.

Azure wordt gemaakt en de naamruimte wordt geactiveerd. Wacht totdat de status van de nieuwe naamruimte **actief** voordat u verdergaat is. 

## <a name="add-identity-providers"></a>Id-providers toevoegen

In deze taak voegt u IP-adressen voor gebruik met uw toepassing RP voor verificatie. Deze taak wordt aangegeven hoe Windows Live toevoegen als een IP-ter demonstratie, maar kunt u de IP-adressen die worden vermeld in de ACS-Management Portal gebruiken.


1.  Klik op **Active Directory**in de [Azure Management Portal][], selecteer een naamruimte toegangsbeheer en klik op **beheren**. De Portal ACS-beheer wordt geopend.
2.  Klik op **id-providers**in het linkernavigatievenster van de ACS-Management-Portal.
3.  Windows Live ID is standaard ingeschakeld en kan niet worden verwijderd. Voor de doeleinden van deze zelfstudie, alleen Windows Live ID gebruikt. Dit scherm is echter waar u andere IP-adressen toevoegen door te klikken op de knop **toevoegen** .

Windows Live ID is nu ingeschakeld als een IP-voor de ACS-naamruimte. Geef vervolgens uw Java webtoepassing (later worden gemaakt) als een RP.

## <a name="add-a-relying-party-application"></a>De toepassing van een gebruikmakende partij toevoegen

In deze taak configureert u ACS herkent uw webtoepassing van Java als geldige RP-toepassing.

1.  Klik op de Portal Management ACS **Relying partij toepassingen**.
2.  Klik op **toevoegen**op de pagina **Toepassingen leveranciers vertrouwen** .
3.  Klik op de pagina **Toepassing vertrouwen toevoegen** door het volgende te doen:
    1.  Typ in het vak **naam**de naam van de RP. Typ voor de doeleinden van deze zelfstudie, **Azure Web App**.
    2.  Selecteer in de **modus** **Voer de instellingen handmatig**.
    3.  Typ de URI die het beveiligingstoken uitgegeven door ACS is van toepassing in de **Realm**. Voor deze taak is, typt u **http://localhost:8080 /**.
        ![Gebruik realm partij voor gebruik in de compute-emulator][relying_party_realm_emulator]
    4.  Typ de URL waaraan ACS het beveiligingstoken retourneert in **Retour-URL** . Voor deze taak is, typt u **http://localhost:8080/MyACSHelloWorld/index.jsp**
        ![Relying partij retour-URL voor gebruik in de compute-emulator][relying_party_return_url_emulator]
    5.  Accepteer de standaardwaarden in de rest van de velden.

4.  Klik op **Opslaan**.

Nu is geconfigureerd uw webtoepassing Java wanneer deze wordt uitgevoerd in de emulator Azure compute (bij http://localhost:8080 /) om een RP in de ACS-naamruimte. Maak vervolgens de regels die de ACS wordt gebruikt voor het verwerken van aanvragen om de RP.

## <a name="create-rules"></a>Regels maken

In deze taak definieert u de regels die drive hoe aanvragen van IP-adressen worden doorgegeven aan de RP. Voor de toepassing van deze handleiding configureert we gewoon ACS invoer typen en waarden rechtstreeks in het token uitvoer kopiëren zonder filteren of wijzigen van deze bestanden.

1.  Klik op de hoofdpagina ACS Management Portal **regel groepen**.
2.  Klik op de pagina **Groepen regel** **Standaard regelgroep voor Azure Web App**.
3.  Klik op de pagina **Groep van regel bewerken** op **genereren**.
4.  Op de **regels genereren: regelgroep voor Azure Web App standaard** pagina, Windows Live ID is ingeschakeld en klik op **genereren**.    
5.  Klik op **Opslaan**op de pagina **Groep van regel bewerken** .

## <a name="upload-a-certificate-to-your-acs-namespace"></a>Een certificaat uploaden naar de ACS-naamruimte

In deze taak, die u uploadt een. PFX-certificaat dat wordt gebruikt voor het ondertekenen van token aanvragen die door de ACS-naamruimte gemaakt.

1.  Klik op de hoofdpagina ACS Management Portal, **certificaten en sleutels**.
2.  Klik op de pagina **certificaten en sleutels** **toevoegen** boven **Tokenhandtekeningen**.
3.  Op de pagina **certificaat toevoegen Token-ondertekening of de sleutel** :
    1. In het gedeelte **gebruikt voor** **Toepassing vertrouwt** op en selecteer **Azure Web App** (die u eerder hebt ingesteld als de naam van uw toepassing gebruikmakende).
    2. Selecteer in de sectie **Type** **X.509-certificaat**.
    3. Klik op de bladerknop en navigeer naar het bestand van x.509-certificaat dat u wilt gebruiken in de sectie **certificaat** . Dit is een. PFX-bestand. Selecteer het bestand, klik op **openen**en voer het wachtwoord voor het certificaat in het vak **wachtwoord** . Houd er rekening mee dat een zelfstandige-signed-certificaat voor testdoeleinden kunnen gebruiken. Gebruik de knop **Nieuw** in het dialoogvenster voor **ACS Filter bibliotheek** (verderop beschreven) om een zelfondertekend certificaat maken, of gebruik van het hulpprogramma **encutil.exe** van de [project-website][] van het startpakket Azure voor Java.
    4. Zorg ervoor dat **Primair maken** is ingeschakeld. **Certificaat toevoegen Token-ondertekening of sleutel** ziet de pagina er ongeveer zo uit.
        ![Certificaat voor token-ondertekening toevoegen][add_token_signing_cert]
    5. Klik op **Opslaan** om uw instellingen opslaan en sluit de pagina **certificaat toevoegen Token-ondertekening of sleutel** .

Vervolgens Lees de informatie op de pagina Application Integration en kopieer de URI die u uw Java webtoepassing configureren moet voor gebruik van de ACS.

## <a name="review-the-application-integration-page"></a>Controleer de pagina van de integratie van toepassingen

U vindt alle informatie en de code nodig voor het configureren van uw webtoepassing van Java (de RP toepassing) om te werken met de ACS op de pagina Application integratie van de ACS Management Portal. U moet deze gegevens bij het configureren van uw webtoepassing van Java voor federatieve verificatie.

1.  Klik op de Portal ACS-Management **Application integration**.  
2.  Klik op de pagina **Application Integration** **Aanmeldingspagina's**.
3.  Klik op de pagina **Login pagina integratie** **Azure Web App**.

In de **Login pagina integratie: Azure Web App** pagina, wordt de URL weergegeven **optie 1: koppeling naar een aanmeldingspagina die door de ACS-host** wordt gebruikt in uw webtoepassing van Java. U moet deze waarde wanneer u de bibliotheek Azure Access Control Services-Filter aan uw Java-toepassing toevoegen.

## <a name="create-a-java-web-application"></a>Een Java webtoepassing maken
1. Binnen de Eclips, op het menu **bestand**klikt u op **Nieuw**en klik op **Dynamische Web-Project**. (Als er geen **Dynamische Web Project** weergegeven als een project beschikbaar na het klikken op **bestand**, **Nieuw**, vervolgens als volgt: klik op **bestand**, klikt u op **Nieuw**klikt u op **Project**, **webpagina**uitvouwen, klikt u op **Dynamic Web Project**en klik op **volgende**.) Geef het project de **MyACSHelloWorld**voor de doeleinden van deze zelfstudie. (Zorg ervoor u deze naam gebruiken, de opeenvolgende stappen in deze zelfstudie verwacht het WAR-bestand de naam MyACSHelloWorld). Het scherm weergegeven met de volgende strekking:

    ![Maak een project HelloWorld voor ACS-exampple][create_acs_hello_world]

    Klik op **Voltooien**.
2. Vouw in de weergave Project Explorer Eclips van **MyACSHelloWorld**. **Webinhoud**met de rechtermuisknop op **Nieuw**en klik vervolgens op **JSP-bestand**.
3. Naam in het dialoogvenster **Nieuwe JSP-bestand** , het bestand **index.jsp**. Houd de bovenliggende map als MyACSHelloWorld/webinhoud, zoals in het volgende:

    ![Een JSP-bestand bijvoorbeeld ACS toevoegen][add_jsp_file_acs]

    Klik op **volgende**.

4. **Nieuwe JSP-bestand (html)** selecteren in het dialoogvenster **Selecteer JSP-sjabloon** en klik op **Voltooien**.
5. Als het bestand index.jsp wordt geopend in de Eclips, in weer te geven tekst toevoegen **ACS HelloWorld!** binnen de bestaande `<body>` element. Uw bijgewerkte `<body>` inhoud moet worden weergegeven als de volgende:

        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
    
    Index.jsp opslaan.
  
## <a name="add-the-acs-filter-library-to-your-application"></a>De bibliotheek van de ACS-Filter toevoegen aan uw toepassing

1. In de Projectverkenner van de Eclips, met de rechtermuisknop op **MyACSHelloWorld**, klikt u op **Pad maken**en klik op **Opbouwen pad configureren**.
2. Klik op het tabblad **bibliotheken** in het dialoogvenster **Pad naar Java bouwen** .
3. Klik op de **bibliotheek toevoegen**.
4. **Azure Access Control Services-Filter (door MS Open Tech)** en klik op **volgende**. Het dialoogvenster **Filter Azure Access Control-Services** wordt weergegeven.  (Het veld **locatie** kan een ander pad, afhankelijk van waar u Eclips, geïnstalleerd hebben en het versienummer kan afwijken, afhankelijk van software-updates).

    ![Bibliotheek ACS-Filter toevoegen][add_acs_filter_lib]

5. Met behulp van een browser is geopend op de pagina **Login pagina integratie** van de Portal Management Kopieer de URL in de **optie 1: koppeling naar een aanmeldingspagina die door de ACS-host** veld en plak deze in het veld **Eindpunt voor ACS-verificatie** van het dialoogvenster Eclips.
6. Met behulp van een browser is geopend op de pagina **Toepassing vertrouwen bewerken** van de Portal voor beheer, de URL in het veld **Realm** kopiëren en plakken in het veld **Partij Realm vertrouwen** van het dialoogvenster Eclips.
7. In de sectie **beveiliging** van het dialoogvenster Eclips als u wilt een bestaand certificaat te gebruiken, klikt u op **Bladeren**, navigeer naar het certificaat dat u wilt gebruiken, selecteert u deze en klikt u op **openen**. Of als u een nieuw certificaat maken en klik op **Nieuw** om het dialoogvenster **Nieuw certificaat** weer te geven, geef het wachtwoord, de naam van het cer-bestand en de naam van het pfx-bestand voor het nieuwe certificaat.
8. Controleer of **het certificaat in het WAR-bestand insluiten**. Het certificaat op deze manier te Embedding omvat het in uw implementatie zonder dat u handmatig toe te voegen als een onderdeel. (Als in plaats daarvan moet u uw certificaat extern opslaan vanuit het WAR-bestand, u kan het certificaat worden toegevoegd als onderdeel van een rol en schakelt u **het certificaat in het WAR-bestand insluiten**.)
9. [Optioneel] Keep **vereisen HTTPS-verbindingen** ingeschakeld. Als u deze optie instelt, moet u toegang hebben tot uw toepassing met behulp van het HTTPS-protocol. Schakel deze optie uit als u niet wilt dat HTTPS-verbindingen vereist.
10. Voor een implementatie van de compute-emulator de filterinstellingen **Azure ACS** ziet er ongeveer als volgt.

    ![Azure ACS filterinstellingen voor een implementatie van de compute-emulator][add_acs_filter_lib_emulator]

11. Klik op **Voltooien**.
12. Klik op **Ja** wanneer wordt aangeboden met een dialoogvenster waarin wordt vermeld dat een web.xml-bestand wordt gemaakt.
13. Klik op **OK** om het dialoogvenster **Java bouwen pad** te sluiten.

## <a name="deploy-to-the-compute-emulator"></a>Implementatie van de compute-emulator

1. Met de rechtermuisknop op **MyACSHelloWorld**in de Projectverkenner van de Eclips naar **Azure**en klik vervolgens op **pakket voor Azure**.
2. Voor de **naam van het Project**, typ **MyAzureACSProject** en klik op **volgende**.
3. Selecteer een JDK en application server. (Deze stappen uitgebreid worden beschreven in de zelfstudie voor het [maken van een Hello World-toepassing voor Azure in Eclips](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) ).
4. Klik op **Voltooien**.
5. Klik op de knop **uitvoeren in Azure-Emulator** .
6. Nadat uw webtoepassing van Java wordt gestart in de compute-emulator, sluit u alle exemplaren van de browser (zodat alle huidige browsersessies niet met uw aanmelding ACS test interfereren).
7. Uw toepassing uitvoeren via <> http://localhost:8080/MyACSHelloWorld/in uw browser ( <> of/https://localhost:8080/MyACSHelloWorld/als u **HTTPS vereisen verbindingen**gecontroleerd). U moet worden gevraagd om een Windows Live ID-aanmelding en klik vervolgens u moet worden genomen om de retour-URL voor uw gebruikmakende partij toepassing opgegeven.
99.  Klik op **Azure-Emulator opnieuw instellen** wanneer u klaar bent met het bekijken van uw toepassing.

## <a name="deploy-to-azure"></a>Implementeren op Azure

Als u wilt implementeren op Azure, moet u de gebruikmakende partij realm wijzigen en het retour-URL voor de ACS-naamruimte.

1. Wijzigen in Azure Management Portal, **Vertrouwen partij toepassing bewerken** op de pagina **Realm** om de URL van uw site geïmplementeerd. **Voorbeeld** vervangen door de DNS-naam die u hebt opgegeven voor uw implementatie.

    ![Gebruik realm partij voor gebruik in productie][relying_party_realm_production]

2. **Retour-URL** de URL van uw toepassing wilt wijzigen. **Voorbeeld** vervangen door de DNS-naam die u hebt opgegeven voor uw implementatie.

    ![Vertrouwen partij retour-URL voor gebruik in productie][relying_party_return_url_production]

3. Klik op **Opslaan** om uw bijgewerkte beantwoorden partij realm opslaan en terugkeren van URL-wijzigingen.
4. De pagina **Login pagina integratie** geopend laten in uw browser, moet u het kopiëren van het kort.
5. In de Projectverkenner van de Eclips, met de rechtermuisknop op **MyACSHelloWorld**, klikt u op **Pad maken**en klik op **Opbouwen pad configureren**.
6. Klik op het tabblad **bibliotheken** **Azure Access Control Services-Filter**en klik vervolgens op **bewerken**.
7. Met behulp van een browser is geopend op de pagina **Login pagina integratie** van de Portal Management Kopieer de URL in de **optie 1: koppeling naar een aanmeldingspagina die door de ACS-host** veld en plak deze in het veld **Eindpunt voor ACS-verificatie** van het dialoogvenster Eclips.
8. Met behulp van een browser is geopend op de pagina **Toepassing vertrouwen bewerken** van de Portal voor beheer, de URL in het veld **Realm** kopiëren en plakken in het veld **Partij Realm vertrouwen** van het dialoogvenster Eclips.
9. In de sectie **beveiliging** van het dialoogvenster Eclips als u wilt een bestaand certificaat te gebruiken, klikt u op **Bladeren**, navigeer naar het certificaat dat u wilt gebruiken, selecteert u deze en klikt u op **openen**. Of als u een nieuw certificaat maken en klik op **Nieuw** om het dialoogvenster **Nieuw certificaat** weer te geven, geef het wachtwoord, de naam van het cer-bestand en de naam van het pfx-bestand voor het nieuwe certificaat.
10. Houd **het certificaat in het WAR-bestand insluiten** ingeschakeld, ervan uitgaande dat het certificaat in het WAR-bestand is ingesloten.
11. [Optioneel] Keep **vereisen HTTPS-verbindingen** ingeschakeld. Als u deze optie instelt, moet u toegang hebben tot uw toepassing met behulp van het HTTPS-protocol. Schakel deze optie uit als u niet wilt dat HTTPS-verbindingen vereist.
12. Voor een implementatie van Azure de filterinstellingen Azure ACS ziet er ongeveer als volgt.

    ![Azure ACS filterinstellingen voor een productie-implementatie][add_acs_filter_lib_production]

13. Klik op **Voltooien** om het dialoogvenster **Bibliotheek bewerken** te sluiten.
14. Klik op **OK** om te sluiten van het dialoogvenster **Eigenschappen voor MyACSHelloWorld** .
15. Eclips, klik op de knop **publiceren op Azure Cloud** . Reageren op de vragen, ongeveer zoals in de sectie **voor de implementatie van de toepassing naar Azure** van het onderwerp voor het [maken van een Hello World-toepassing voor Azure in Eclips](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) uitgevoerd. 

Nadat uw webtoepassing is geïmplementeerd, sluit alle geopende browsersessies uw webtoepassing uitvoeren en u aan te melden met een Windows Live ID-referenties, gevolgd door die worden verzonden naar de retour-URL van uw toepassing met gebruikmakende partij moet worden gevraagd.

Als u klaar bent met ACS Hello World-toepassing moet u de implementatie (u kunt informatie over het verwijderen een implementatie in het onderwerp voor het [maken van een Hello World-toepassing voor Azure in Eclips](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) ) te verwijderen.


## <a name="next_steps"></a>Volgende stappen

Zie voor een onderzoek van de Security Assertion Markup Language (SAML) door ACS terug naar de toepassing [die wordt geretourneerd door de Azure Access Control Service SAML weergeven][]. Verder verkennen van ACS-functionaliteit en om te experimenteren met meer geavanceerde scenario's, Zie [Access Control Service 2.0][].

In dit voorbeeld wordt ook de optie **insluiten het certificaat in het WAR-bestand** gebruikt. Met deze optie kunt u eenvoudig het certificaat te implementeren. Als u in plaats daarvan uw handtekeningcertificaat gescheiden blijven van het WAR-bestand wilt, kunt u de volgende methode:

1. Typ in de sectie **beveiliging** van het dialoogvenster **Azure Access Control Services Filter** $ **{env. JAVA_HOME}/mycert.cer** en schakelt u **het certificaat in het WAR-bestand insluiten**. (Mijncert.cer aanpassen als de bestandsnaam van het certificaat anders is). Klik op **Voltooien** om het dialoogvenster te sluiten.
2. Kopieer het certificaat als onderdeel van uw implementatie: Projectverkenner In Eclips uitvouwen van **MyAzureACSProject**, met de rechtermuisknop op de **WorkerRole1**klikt u op **Eigenschappen**, vouw **Azure rol**en klik op **onderdelen**.
3. Klik op **toevoegen**.
4. In het dialoogvenster **Component toevoegen** :
    1. In de sectie **importeren** :
        1. De knop **bestand** gebruiken om te navigeren naar het certificaat dat u wilt gebruiken. 
        2. Voor **methode**, selecteert u **kopie**.
    2. **Als de naam**en klikt u op het tekstvak en accepteer de standaardnaam.
    3. In de sectie **implementeren** :
        1. Voor **methode**, selecteert u **kopie**.
        2. Voor een **map**, typ **% JAVA_HOME %**.
    4. Het dialoogvenster **Component toevoegen** ziet er ongeveer als volgt.

        ![Certificaatonderdeel toevoegen][add_cert_component]

    5. Klik op **OK**.

Op dit moment zou uw certificaat worden opgenomen in uw implementatie. Houd er rekening mee dat ongeacht of het certificaat in het WAR-bestand insluiten of als een onderdeel toevoegen aan uw implementatie, moet u het certificaat uploaden naar de naamruimte als beschreven in de sectie voor het [uploaden van een certificaat aan de ACS-naamruimte][] .

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Een certificaat uploaden naar de ACS-naamruimte]: #upload-certificate
[Review the Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add the ACS Filter library to your application]: #add_acs_filter_library
[Deploy to the compute emulator]: #deploy_compute_emulator
[Deploy to Azure]: #deploy_azure
[Next steps]: #next_steps
[Project-website]: http://wastarterkit4java.codeplex.com/releases/view/61026
[Het weergeven van SAML die wordt geretourneerd door de Azure Access Control-Service]: /en-us/develop/java/how-to-guides/view-saml-returned-by-acs/
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Azure Management Portal]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png
 
