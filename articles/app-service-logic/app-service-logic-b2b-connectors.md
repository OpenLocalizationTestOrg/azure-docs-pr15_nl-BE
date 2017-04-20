<properties 
    pageTitle="Business-to-Business-Connectors en API Apps in logica Apps | Microsoft Azure" 
    description="Meer informatie over het maken en configureren van EDI, EDIFACT, AS2 en TPM-connectors; microservices architectuur" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/> 

# <a name="business-to-business-connectors-and-api-apps"></a>Business-to-Business-Connectors en API Apps

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Apps logica bevat veel BizTalk-API Apps die cruciaal voor integratie-omgevingen zijn. Deze API Apps zijn gebaseerd op de concepten en hulpprogramma's die worden gebruikt in BizTalk Server, maar zijn nu beschikbaar als onderdeel van de logica Apps. 

Een categorie van deze API Apps zijn de Business-to-Business (B2B) API Apps. Met deze API B2B Apps, kunt u eenvoudig toevoegen van partners, overeenkomsten maken en doen alles wat u zou op-ruimten met behulp van EDI, AS2 en EDIFACT.  

Deze API B2B Apps bieden mogelijkheden voor "de tekst Activeringsmethode" en "Actie". Een Trigger wordt gestart een nieuwe instantie op basis van een bepaalde gebeurtenis, zoals de ontvangst van een X12 bericht van een partner. Een actie is het resultaat, net als na de ontvangst van een X12 bericht, wordt het bericht verzenden met AS2.


## <a name="what-is-a-business-to-business-connector-or-api-apps"></a>Wat is een Business-to-Business Connector of API Apps
De Business-to-Business (B2B)-functie bevat een bestaand, vooraf gebouwde API Apps waarmee verschillende bedrijven, afdelingen, toepassingen enzovoort om te communiceren met AS2, EDI en EDIFACT. 

De API B2B Apps zijn: 

Connector of API Apps | Beschrijving
--- | ---
BizTalk-Trading Partner beheer | Een API-App die business-to-business (B2B) relaties met partners en overeenkomsten worden gemaakt. Deze relaties maken gebruik van de AS2, EDIFACT en X12 protocol.<br/><br/>De TPM API App is de basis behoefte van de connector AS2 en de X12 of EDIFACT API Apps. 
AS2-Connector | Een verbindingslijn die met het vervoer AS2 berichten verzendt en ontvangt. De connector transports gegevens betrouwbaar en veilig via het Internet.
BizTalk-EDIFACT | Een API-App die met EDIFACT berichten verzendt en ontvangt. EDIFACT wordt ook vaak aangeduid als UN/EDIFACT (VN/Electronic Data Interchange For Administration, Commerce en vervoer) en wordt veel gebruikt in de industrie.
BizTalk-X12 | Een API-App die met de X12 berichten verzendt en ontvangt protocol. X12 wordt ook vaak aangeduid als ASC X 12 (geaccrediteerd Standards Committee X12) en wordt veel gebruikt in de industrie. 


Deze API Apps kunt u verschillende EDI berichten taken voltooien. Bijvoorbeeld met behulp van de verbindingslijn AS2, u kunt veilig ontvangen en verschillende typen berichten te verzenden (EDI, XML, Flat-bestand, enzovoort) aan een klant, zoals een afdeling binnen uw bedrijf of iedereen die gebruikmaakt van AS2 Human Resources. 

U kunt zoveel API Apps als u wilt en deze gemakkelijk te maken. U kunt ook een enkele API App binnen meerdere scenario's of workflows hergebruiken.

U kunt dit doen zonder code te schrijven. We gaan aan de slag. 


## <a name="requirements-to-get-started"></a>Eisen aan de slag
Bij het maken van B2B-API Apps, zijn er bepaalde vereiste resources. Deze artikelen moeten worden gemaakt door u, voordat ze kunnen worden gebruikt in andere API Apps. Deze vereisten zijn: 

Vereiste | Beschrijving
--- | ---
Azure SQL-Database | B2B-items zoals partners, schema's, certificaten en agreeements worden opgeslagen. Elk van de B2B-API Apps zijn eigen Azure SQL-Database is vereist. <br/><br/>**Opmerking** De verbindingsreeks voor deze database kopiëren.<br/><br/>[Azure SQL-Database maken](../sql-database/sql-database-get-started.md)
Azure Blob-opslag container | Winkels berichteigenschappen wanneer AS2 is ingeschakeld. Als u geen AS2 bericht archiveren, wordt een opslag container is niet nodig. <br/><br/>**Opmerking** Als u van archivering inschakelen, kopieert u de verbindingsreeks naar deze Blob-opslag.<br/><br/>[Over Azure opslag Accounts](../storage/storage-create-storage-account.md)
Service Bus Namespace en de waarden van de sleutel | X12 en EDIFACT batchen van gegevens worden opgeslagen. Als u geen batchen, wordt de naamruimte van een Bus-Service is niet nodig.<br/><br/>**Opmerking** Als u de batchverwerking inschakelen wilt, moet u deze waarden kopiëren.<br/><br/>[Maken van een Service Bus Namespace](http://msdn.microsoft.com/library/azure/hh690931.aspx)
TPM-exemplaar | Een exemplaar van de BizTalk-Trading Partner Management (TPM) is vereist voor het maken van een AS2-connector en X12 of EDIFACT API App. Als u de TPM API App maakt, maakt u een exemplaar van de TPM. <br/><br/>**Opmerking** De naam weten van uw App TPM-API. 


## <a name="create-the-api-apps"></a>De API Apps maken
B2B-API Apps kunnen worden gemaakt met behulp van de portal Azure of met REST API's. 


### <a name="create-the-api-apps-using-rest-apis"></a>De API Apps met REST API's maken
[Zie de documentatie over het gebruik van de REST API's.](http://go.microsoft.com/fwlink/p/?LinkId=529766)


### <a name="create-the-b2b-api-apps-in-the-azure-portal"></a>De API B2B Apps maken in de Portal voor Azure
In de Azure portal kunt u B2B API Apps als logica Apps, Web Apps of mobiele Apps te maken. Of u kunt maken met een eigen blade. Beide manieren zijn gemakkelijk het is afhankelijk van uw behoeften en voorkeuren. Sommige gebruikers liever eerst alle B2B API Apps maken met hun specifieke eigenschappen. Vervolgens maakt de logica Apps/Web Apps/Mobile Apps en het B2B-API Apps die u hebt gemaakt toevoegen.  

De volgende stappen maakt het B2B-API Apps met behulp van de API Apps blade.


#### <a name="create-the-biztalk-trading-partner-management-tpm-api-apps"></a>De BizTalk Partner Management (TPM) API Apps handel maken

> [AZURE.NOTE] Een exemplaar van de BizTalk-Trading Partner Management (TPM) is vereist voor het maken van een AS2-connector en X12 of EDIFACT API App. Als u de TPM API App maakt, maakt u een exemplaar van de TPM.

De volgende stappen maakt het TPM-exemplaar:

1. Selecteer de **Marketplace**in Azure portal Startboard (startpagina). **API Apps** geeft een overzicht van de bestaande API Apps en connectoren. U kunt ook **Zoeken** voor de specifieke B2B API Apps.
2. Selecteer **het beheer van handelspartners BizTalk**. Selecteer in het nieuwe blad **maken**. 
3. Geef de eigenschappen: 

    Eigenschap | Beschrijving
--- | ---
Naam | Voer een naam voor het exemplaar van de TPM. Noem het adresboek bijvoorbeeld *AccountsPayableTPM*.
Pakketinstellingen | Voer de ADO.NET- **Verbindingsreeks** met de Azure SQL-Database die u hebt gemaakt. <br/><br/>Als u de verbindingsreeks kopieert, wordt het wachtwoord niet toegevoegd aan de verbindingsreeks. Zorg ervoor dat het wachtwoord opgeven in de verbindingsreeks.
App-serviceplan | Geeft een overzicht van uw betalingsplan. U kunt deze wijzigen als u meer of minder bronnen nodig.
Prijzen laag | Alleen-lezen eigenschap die de categorie prijzen binnen uw abonnement Azure staan. 
Resourcegroep | Maak een nieuwe of een bestaande groep gebruiken. Alle API Apps en aansluitingen voor uw logica Apps, Web Apps en mobiele Apps moeten zich in dezelfde bronnengroep. <br/><br/>[Met behulp van de resourcegroepen](../azure-resource-manager/resource-group-overview.md) wordt deze eigenschap uitgelegd. 
Abonnement | Alleen-lezen eigenschap die de lijsten van je huidige abonnement.
Locatie | De geografische locatie waarop uw Azure service. 
Toevoegen aan Startboard | Met deze optie op de B2B-API App toevoegen aan uw Starboard (startpagina).

4. Selecteer **maken**. 

Nadat de TPM API APP (TPM-instantie) is gemaakt, kunt u vervolgens de verbindingslijn AS2 en/of de X12 of EDIFACT API Apps maken. 


#### <a name="create-the-as2-connector"></a>Maak de AS2-connector

1. Selecteer de **Marketplace**in Azure portal Startboard (startpagina). **API Apps** geeft een overzicht van de bestaande API Apps en connectoren. U kunt ook **Zoeken** voor de specifieke B2B API Apps.
2. Selecteer **de verbindingslijn AS2**. Selecteer in het nieuwe blad **maken**. 
3. Geef de eigenschappen: 

    Eigenschap | Beschrijving
--- | ---
Naam | Voer een naam voor de AS2-connector. Noem het adresboek bijvoorbeeld *AS2Connector*.
Pakketinstellingen | Voer de specifieke instellingen aan die API App, zoals de naam van het exemplaar van de TPM. <br/><br/>Zie [Instellingen voor AS2 pakket toevoegen](#AddAS2Conn) in dit onderwerp voor de specifieke eigenschappen. 
App-serviceplan | Geeft een overzicht van uw betalingsplan. U kunt deze wijzigen als u meer of minder bronnen nodig.
Prijzen laag | Alleen-lezen eigenschap die de categorie prijzen binnen uw abonnement Azure staan. 
Resourcegroep | Maak een nieuwe of een bestaande groep gebruiken. [Met behulp van de resourcegroepen](../azure-resource-manager/resource-group-overview.md) wordt deze eigenschap uitgelegd. 
Abonnement | Alleen-lezen eigenschap die de lijsten van je huidige abonnement.
Locatie | De geografische locatie waarop uw Azure service. 
Toevoegen aan Startboard | Met deze optie op de B2B-API App toevoegen aan uw Starboard (startpagina).

    **<a name="AddAS2Conn"></a>Pakketinstellingen AS2-connector**

    Eigenschap | Beschrijving
--- | --- 
De verbindingsreeks | Geef de ADO.NET-verbindingsreeks met de Azure SQL-Database die u hebt gemaakt. Als u de verbindingsreeks kopieert, wordt het wachtwoord niet toegevoegd aan de verbindingsreeks. Zorg ervoor dat het wachtwoord in de verbindingsreeks opgeven voordat u plakt.
Archivering inschakelen voor binnenkomende berichten | Dit is optioneel. Deze eigenschap voor het opslaan van eigenschappen van het bericht van een binnenkomend AS2-bericht ontvangen van een partner inschakelen. 
Verbindingsreeks Azure Blob-opslag  | Geef de verbindingsreeks met de Azure Blob-opslag container die u hebt gemaakt. Bij het archiveren is ingeschakeld, worden de berichten gecodeerd en gedecodeerd worden opgeslagen in deze container opslag.
TPM-exemplaarnaam | Voer de naam van het **BizTalk-Trading Partner beheer** API App u eerder hebt gemaakt. Als u de verbindingslijn AS2 maakt, voert deze verbindingslijn alleen de AS2 overeenkomsten binnen dit specifieke exemplaar van de TPM.

4. Selecteer **maken**. 


#### <a name="create-the-x12-or-edifact-api-apps"></a>De API X12 of EDIFACT Apps maken

1. Selecteer de **Marketplace**in Azure portal Startboard (startpagina). **API Apps** geeft een overzicht van de bestaande API Apps en connectoren. U kunt ook **Zoeken** voor de specifieke B2B API Apps.
2. Selecteer **BizTalk X 12** of **EDIFACT BizTalk**. Selecteer in het nieuwe blad **maken**. 
3. Geef de eigenschappen: 

    Eigenschap | Beschrijving
--- | ---
Naam | Voer een naam voor de B2B-API App. Noem het adresboek bijvoorbeeld *EDI850APIApp*.
Pakketinstellingen | Voer de specifieke instellingen aan die API App, zoals de naam van het exemplaar van de TPM. <br/><br/>Zie [X12 of EDIFACT pakketinstellingen](#AddX12) in dit onderwerp voor de specifieke eigenschappen. 
App-serviceplan | Geeft een overzicht van uw betalingsplan. U kunt deze wijzigen als u meer of minder bronnen nodig.
Prijzen laag | Alleen-lezen eigenschap die de categorie prijzen binnen uw abonnement Azure staan. 
Resourcegroep | Maak een nieuwe of een bestaande groep gebruiken. [Met behulp van de resourcegroepen](../azure-resource-manager/resource-group-overview.md) wordt deze eigenschap uitgelegd. 
Abonnement | Alleen-lezen eigenschap die de lijsten van je huidige abonnement.
Locatie | De geografische locatie waarop uw Azure service. 
Toevoegen aan Startboard | Met deze optie op de B2B-API App toevoegen aan uw Starboard (startpagina).

    **<a name="AddX12"></a>X12 en EDIFACT API Apps pakket instellingen**  

    Eigenschap | Beschrijving
--- | --- 
De verbindingsreeks | Geef de ADO.NET-verbindingsreeks met de Azure SQL-Database die u hebt gemaakt. Als u de verbindingsreeks kopieert, wordt het wachtwoord niet toegevoegd aan de verbindingsreeks. Zorg ervoor dat het wachtwoord in de verbindingsreeks opgeven voordat u plakt.
Service Bus Namespace | Voer de Service Bus-naamruimte die u hebt gemaakt. Vereist slechts wanneer batchen is ingeschakeld. 
Service Bus Namespace gedeelde toegang sleutelnaam | Geef de naamruimte Service Bus sneltoets die u hebt gemaakt. Vereist slechts wanneer batchen is ingeschakeld. 
Bus Namespace gedeelde toegangssleutel waarde | Geef de naamruimte Service Bus toegangstoets waarde die u hebt gemaakt. Vereist slechts wanneer batchen is ingeschakeld. 
TPM-exemplaarnaam | Voer de naam van het **BizTalk-Trading Partner beheer** API App u eerder hebt gemaakt. Bij het maken van de X12 of EDIFACT API Apps voert dit App API alleen de X12/EDFIACT-overeenkomsten binnen dit specifieke exemplaar van de TPM.

4. Selecteer **maken**. 


## <a name="add-your-partners-agreements-certificates-and-schemas"></a>Uw partners, overeenkomsten, certificaten en schema's toevoegen 
Open uw TPM-API App in de Azure portal. In de sectie **onderdelen** toevoegen uw Partners, overeenkomsten, certificaten en schema's. 

U kunt ook overeenkomsten toevoegen aan uw AS2-connectors, X12 API Apps en Apps van EDIFACT-API. 


## <a name="monitor-your-api-apps"></a>De API Apps controleren
Open uw TPM-API App in de Azure portal. In **de sectie** ziet u verschillende beheertaken uit te voeren. U kunt bijvoorbeeld:

- Gebeurtenissen weergeven ter informatie en fout
- Gebruiks- en thread count geheugen van het werkproces (w3wp) weergeven
- De toepassings- en server-logboekbestanden weergeven

Bij [Netwerkcontrole de logica Apps](app-service-logic-monitor-your-logic-apps.md).


## <a name="add-the-api-apps-to-your-application"></a>De API Apps toevoegen aan uw toepassing 
Microsoft Azure App Service beschrijft de verschillende toepassingstypen die deze Apps B2B-API kunnen gebruiken. U kunt een nieuwe maken of uw bestaande B2B API Apps toevoegen aan logica Apps, mobiele Apps of een Web-Apps. 

Binnen de App, simpelweg selecteren van de B2B-API Apps uit de galerie automatisch toegevoegd aan uw App.  

> [AZURE.IMPORTANT] Verbindingslijnen en API Apps die u eerder hebt gemaakt wilt toevoegen, de logica Apps, mobiele Apps of Web Apps in dezelfde bronnengroep te maken. 

De volgende stappen uit toevoegen het B2B-API Apps aan logica Apps of Apps Mobile Web Apps: 

1. Ga naar de **markt**en zoeken naar de logica, mobiele of Web Apps in Azure portal Startboard (startpagina). 

    Als u een nieuwe toepassing maakt, kunt u zoeken naar logica Apps of Apps Mobile Web Apps. Selecteer de App en selecteer **maken**in het nieuwe blad. [Maken van een App logica](app-service-logic-create-a-logic-app.md) bevat de stappen. 

2. Open de App en selecteer **Triggers en acties**. 

3. Selecteer de App B2B-API, die automatisch toegevoegd aan uw toepassing uit de **Galerie**. Ook kunt u een nieuwe toepassing van de B2B-API.

    > [AZURE.IMPORTANT] De connector AS2 en X12, vereisen EDIFACT API Apps een exemplaar van de TPM. Dus als u maakt nieuwe B2B API Apps, maakt u eerst de TPM API App en maak vervolgens de verbindingslijn AS2, X12 API App of EDIFACT API App. 

4. Selecteer **OK** om de wijzigingen opslaan. 

>[AZURE.NOTE] Aan de slag met Azure logica Apps voor het aanmelden voor een Azure [Probeer logica Apps](https://tryappservice.azure.com/?appservice=logic)-account. U kunt direct een tijdelijk starter logica app maken. Geen creditcard vereist; geen verplichtingen.

## <a name="more-b2b-resources"></a>Meer bronnen voor B2B

[Maken van een B2B-proces](app-service-logic-create-a-b2b-process.md)<br/>
[Maken van een Trading partnerovereenkomst](app-service-logic-create-a-trading-partner-agreement.md)<br/>
[Wat zijn verbindingslijnen en BizTalk-API Apps](app-service-logic-what-are-biztalk-api-apps.md)


## <a name="read-about-logic-apps-and-web-apps"></a>Meer informatie over Apps logica en Web Apps
[Wat zijn logica Apps?](app-service-logic-what-are-logic-apps.md)<br/>
[Websites en Web Apps in Azure App-Service](../app-service-web/app-service-web-overview.md)


## <a name="more-connectors"></a>Meer verbindingslijnen

[Verbindingslijnen en API Apps lijst](app-service-logic-connectors-list.md)<br/><br/>
[Wat zijn verbindingslijnen en BizTalk-API Apps](app-service-logic-what-are-biztalk-api-apps.md) 
