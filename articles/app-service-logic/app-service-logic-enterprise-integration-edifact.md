<properties 
    pageTitle="Integratie in de onderneming met EDIFACT | Microsoft Azure" 
    description="Informatie over het gebruik van overeenkomsten EDIFACT logica apps maken" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="jonfan"/>

# <a name="enterprise-integration-with-edifact"></a>Integratie in de onderneming met EDIFACT 

> [AZURE.NOTE] Deze pagina omvat de EDIFACT-functies van Apps logica. Klik voor informatie over de X12 [hier](app-service-logic-enterprise-integration-x12.md).

## <a name="create-an-edifact-agreement"></a>Maak een EDIFACT-overeenkomst 
Voordat u EDIFACT berichten uitwisselen kunt, moet u een overeenkomst EDIFACT maken en deze opslaan in uw account integratie. De volgende stappen kunt u bij het maken van een EDIFACT-overeenkomst.

### <a name="heres-what-you-need-before-you-get-started"></a>Hier is wat u nodig hebt voordat u begint
- Een [account voor integratie](./app-service-logic-enterprise-integration-accounts.md) in uw abonnement Azure  
- Ten minste twee [partners](./app-service-logic-enterprise-integration-partners.md) al gedefinieerd in uw account integratie  

>[AZURE.NOTE]Bij het maken van een overeenkomst, de inhoud van de berichten u zal ontvangen/verzenden naar en van de partner, moet overeenkomen met het Overeenkomstsoort.    


Nadat u [gemaakt van een account integratie](./app-service-logic-enterprise-integration-accounts.md) en [partners toegevoegd hebt](./app-service-logic-enterprise-integration-partners.md), kunt u een EDIFACT-overeenkomst door de volgende stappen:  

### <a name="from-the-azure-portal-home-page"></a>Vanaf de introductiepagina van Azure portal

Nadat u zich aanmelden bij de [Azure portal](http://portal.azure.com "Azure portal"):  
1. Selecteer **Bladeren** in het menu aan de linkerkant.  

>[AZURE.TIP]Als u de koppeling **Bladeren** niet ziet, moet u mogelijk eerst het menu uitvouwen. Hiervoor selecteert u de koppeling **menu weergeven** op links boven in het menu samengevouwen.  

![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-0.png)    
2. *Integratie* typt in het vak filter zoeken en selecteer vervolgens **Integratie Accounts** in de lijst met resultaten.       
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-3.png)    
3. In de blade **Integratie Accounts** die wordt geopend, selecteert u de integratie rekening waarin u de overeenkomst maken. Als er geen integratie van alle rekeningen lijsten, [maakt u een eerste](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-4.png)  
4.  Selecteer de tegel **overeenkomsten** . Als u de overeenkomsten naast elkaar weergeven niet ziet, moet u deze eerst toevoegen.   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-5.png)     
5. Selecteer de knop **toevoegen** in het blad overeenkomsten die wordt geopend.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-agreement-2.png)  
6. Voer een **naam** voor de overeenkomst en selecteer vervolgens het **type overeenkomst** voor EDIFACT, **Partner van de Host**, **Identiteit van de Host**, **Partner van de Gast**, **Gast identiteit**, in het blad overeenkomsten die wordt geopend.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1.png)  
7. Nadat u de eigenschappen van de overeenkomst hebt ingesteld, selecteert u **Instellingen voor ontvangen** om te configureren hoe berichten ontvangen via deze overeenkomst moeten worden verwerkt.  
8. Het besturingselement instellingen ontvangen is onderverdeeld in de volgende secties, met inbegrip van id's, bevestiging, schema's, Control Numbers, validatie, interne instellingen en Batch-verwerking. Deze eigenschappen op basis van uw overeenkomst met de partner die u berichten uitwisselt configureren. Hier volgt een overzicht van deze besturingselementen, op basis van hoe u deze overeenkomst te herkennen en verwerken van inkomende berichten wilt configureren:  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-2.png)  
9. Selecteer **OK** om uw instellingen op te slaan.  

### <a name="identifiers"></a>Id 's

|Eigenschap|Beschrijving |
|---|---|
|UNB6.1 (wachtwoord geadresseerde verwijzing)|Een alfanumerieke waarde variërend van 1 tot 14 tekens invoeren.|
|UNB6.2 (kwalificatie geadresseerde verwijzing)|Een alfanumerieke waarde met een minimum van één teken en een maximum van twee tekens invoeren.|

### <a name="acknowledgments"></a>Bevestigingen 

|Eigenschap|Beschrijving |
|----|----|
|Ontvangst van bericht (CONTRL)|Schakel dit selectievakje in dat een technische (CONTRL) bevestiging naar de afzender van de uitwisseling. De bevestiging is verzonden naar de afzender van de uitwisseling op basis van de instellingen voor verzenden voor de overeenkomst.|
|Bevestiging (CONTRL)|Schakel dit selectievakje in om een functionele (CONTRL) bevestiging terugsturen naar de afzender van de uitwisseling met de bevestiging wordt verzonden naar de afzender van de uitwisseling op basis van de instellingen voor verzenden voor de overeenkomst.|

### <a name="schemas"></a>Schema 's

|Eigenschap|Beschrijving |
|----|----|
|UNH2.1 (TYPE)|Selecteer een type transactie instellen.|
|UNH2.2 (VERSIE)|Geef het versienummer van het bericht. (Minimum, één teken, maximum, drie tekens).|
|UNH2.3 (INTRODUCTIE)|Voer het nummer van het release. (Minimum, één teken, maximum, drie tekens).|
|UNH2.5 (GEKOPPELD TOEGEWEZEN CODE)|Voer de code toegewezen. (Maximaal zes tekens. Moet alfanumeriek zijn).|
|UNG2.1 (APP AFZENDER-ID)|Een alfanumerieke waarde met een minimum van één teken en maximaal 35 tekens invoeren.|
|UNG2.2 (APP AFZENDER CODE KWALIFICATIE)|Een alfanumerieke waarde, met een maximum van vier tekens invoeren.|
|SCHEMA|Selecteer de eerder geüploade schema dat u wilt gebruiken in uw gekoppelde Account voor integratie.|

### <a name="control-numbers"></a>Control Numbers

|Eigenschap|Beschrijving |
|----|----|
|Controlenummer Interchange duplicaten toestaan|Schakel dit selectievakje in voor het blokkeren van dubbele uitgewisseld. Ingeschakeld, controleert de actie EDIFACT decoderen het controlenummer interchange (UNB5) voor het uitwisselen van ontvangen komt niet overeen met een eerder verwerkte interchange controlenummer. Als een overeenkomst wordt gevonden, wordt het de uitwisseling wordt niet verwerkt.
|Controleren op dubbele UNB5 elke (dagen)|Als u ervoor hebt gekozen om dubbele interchange controlenummers niet toestaan, kunt u het aantal dagen waarop de controle wordt uitgevoerd door middel van de juiste waarde voor de optie **controleren op dubbele UNB5 (alle dagen)** .|
|Groep besturingselement aantal duplicaten toestaan|Schakel dit selectievakje in voor het blokkeren van uitgewisseld met dubbele controlenummers (UNG5).|
|Transactie set besturingselement aantal duplicaten toestaan|Schakel dit selectievakje in voor het blokkeren van uitgewisseld met dubbele transactie set controlenummers (UNH1).|
|EDIFACT besturingselement bevestigingsnummer|Aan te wijzen de transactie set nummers in een bevestiging moet worden gebruikt, moet u een waarde invoeren voor het voorvoegsel, een reeks nummers en een achtervoegsel.|

### <a name="validations"></a>Validaties

|Eigenschap|Beschrijving |
|----|----|
|Berichttype|Geef het berichttype. Elke rij van de validatie is voltooid, wordt een andere automatisch toegevoegd. Als er geen regels zijn opgegeven, wordt de rij gemarkeerd zoals standaard wordt gebruikt voor validatie.|
|EDI-validatie|Schakel dit selectievakje in als u EDI validatie uitvoeren voor gegevenstypen die door de EDI-eigenschappen van het schema, lengte beperkingen, lege elementen en afsluitende scheidingsteken.|
|Uitgebreide validatie|Schakel dit selectievakje in om uitgebreide (XSD)-validatie van het ontvangen van de afzender interchange uitgewisseld. Dit omvat de validatie van de veldlengte en optionaliteit herhalingen naast XSD-type gegevensvalidatie.|
|Regelafstand/navolgende nullen toestaan|Selecteer **toestaan** om regelafstand/nullen achter de komma; **NotAllowed** niet toestaan regelafstand/navolgende nullen of **bijsnijden** voor het bijsnijden van de voorloopspaties en volgspaties nul.|
|Trim nullen regelafstand/volgspaties|Schakel dit selectievakje in als u voorloop- of volgspaties nullen knippen|
|Afsluitende scheidingsteken beleid|Selecteer **Niet toegestaan** als u niet wilt toestaan navolgende scheidingstekens en scheidingstekens in een knooppunt van de afzender interchange ontvangen. Als het knooppunt aan het einde scheidingstekens en scheidingstekens bevat, is deze nietig verklaard. **Optioneel** uitgewisseld met of zonder navolgende scheidingstekens en scheidingstekens accepteren selecteert. Selecteer **verplicht** als de ontvangen uitwisseling moet achteraan scheidingstekens en scheidingstekens bevatten.|

### <a name="internal-settings"></a>Interne instellingen

|Eigenschap|Beschrijving |
|----|----|
|Lege XML-tags maken als afsluitende scheidingstekens zijn toegestaan.|Schakel dit selectievakje in om de uitwisseling afzender bevatten lege XML-codes voor het afsluitende scheidingsteken.|
|Verwerking van inkomende batchen|Opties omvatten:</br></br>**Interchange splitsen als Sets van transactie - transactie wordt bij fout opschorten**: elke transactie in een knooppunt in een apart XML-document instellen door de juiste envelop tot de transactie wordt geparseerd. Met deze optie als een of meer transacties wordt ingesteld in de uitwisseling validatie mislukt, wordt alleen die transactie sets worden geschorst. Interchange splitsen als Sets van transactie - uitwisseling onderbreken bij fout: elke transactie in een knooppunt in een apart XML-document instellen door de juiste envelop wordt geparseerd. Met deze optie als een of meer transacties wordt ingesteld in de uitwisseling validatie mislukt, wordt de hele uitwisseling zal worden geschorst.</br></br>**Behouden Interchange - onderbreking transactie Sets on Error**: blijft de uitwisseling behouden, maakt een XML-document voor de hele batch uitwisseling. Met deze optie als een of meer transacties wordt ingesteld in de uitwisseling validatie mislukt, wordt alleen deze transactie wordt geschorst, terwijl alle andere sets uit de transactie wordt verwerkt.</br></br>**Behouden Interchange - onderbreking Interchange on Error**: blijft de uitwisseling behouden, maakt een XML-document voor de hele batch uitwisseling. Met deze optie als een of meer transacties wordt ingesteld in de uitwisseling validatie mislukt, wordt de hele uitwisseling is geschorst.|

De overeenkomst is geschikt voor het verwerken van binnenkomende berichten die voldoen aan de instellingen die u hebt geselecteerd.

Configureer de instellingen die u naar partners verzendt berichten verwerken:  
10. Selecteer **Instellingen voor verzenden** om te configureren hoe berichten die worden verzonden via deze overeenkomst moeten worden verwerkt.  

De controle van de instellingen voor verzenden is onderverdeeld in de volgende secties, met inbegrip van id's, bevestiging, schema's, enveloppen, tekensets en scheidingstekens, getallen controle en validatie. 

Hier volgt een overzicht van deze besturingselementen. De selecties op basis van hoe u wilt omgaan met berichten die u naar partners via deze overeenkomst verzendt maken:   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-3.png)    
11. Selecteer **OK** om uw instellingen op te slaan.  

### <a name="identifiers"></a>Id 's
|Eigenschap|Beschrijving |
|----|----|
|UNB1.2 (syntaxis versie)|Selecteer een waarde tussen **1** en **4**.|
|UNB2.3 (Reverse routering afzenderadres)|Een alfanumerieke waarde met een minimum van één teken en maximaal 14 tekens invoeren.|
|UNB3.3 (adres ontvanger Reverse routering)|Een alfanumerieke waarde met een minimum van één teken en maximaal 14 tekens invoeren.|
|UNB6.1 (wachtwoord geadresseerde verwijzing)|Een alfanumerieke waarde met minimaal één en maximaal 14 tekens invoeren.|
|UNB6.2 (kwalificatie geadresseerde verwijzing)|Een alfanumerieke waarde met een minimum van één teken en een maximum van twee tekens invoeren.|
|UNB7 (ID toepassing verwijzing)|Een alfanumerieke waarde met een minimum van één teken en maximaal 14 tekens invoeren|

### <a name="acknowledgment"></a>Bevestiging
|Eigenschap|Beschrijving |
|----|----|
|Ontvangst van bericht (CONTRL)|Schakel dit selectievakje in als de gehoste partner verwacht te ontvangen om een technische (CONTRL) bevestiging ontvangen. Deze instelling geeft aan dat de gehoste partner die het bericht verzendt, een bevestiging van de Gast-partner verzoekt.|
|Bevestiging (CONTRL)|Schakel dit selectievakje in als u de gehoste partner verwacht te ontvangen van de bevestiging van een functionele (CONTRL). Deze instelling geeft aan dat de gehoste partner die het bericht verzendt, een bevestiging van de Gast-partner verzoekt.|
|Lus voor geaccepteerde transactie sets SG1/SG4 genereren|Als u kiest voor het aanvragen van een functionele bevestiging, schakel dit selectievakje generatie SG1/SG4 lussen in functionele CONTRL bevestigingen voor geaccepteerde transactie sets afdwingen.|

### <a name="schemas"></a>Schema 's
|Eigenschap|Beschrijving |
|----|----|
|UNH2.1 (TYPE)|Selecteer een type transactie instellen.|
|UNH2.2 (VERSIE)|Geef het versienummer van het bericht.|
|UNH2.3 (INTRODUCTIE)|Voer het nummer van het release.|
|SCHEMA|Selecteer het schema dat u wilt gebruiken. Schema's bevinden zich in uw account integratie. Als u uw schema's, uw account integratie eerst aan uw app logica te koppelen.|

### <a name="envelopes"></a>Enveloppen
|Eigenschap|Beschrijving |
|----|----|
|UNB8 (prioriteitscode Processing)|Voer een alfabetische waarde, die niet meer dan één teken lang.|
|UNB10 (communicatie-overeenkomst)|Een alfanumerieke waarde met een minimum van één teken en maximaal 40 tekens invoeren.|
|UNB11 (Test Indicator)|Schakel dit selectievakje in om aan te geven dat het knooppunt gegenereerd testgegevens|
|UNA Segment (advies Service String) van toepassing|Schakel dit selectievakje in voor het genereren van een segment UNA voor de uitwisseling moet worden verzonden.|
|UNG segmenten (functie groepskoptekst) van toepassing|Schakel dit selectievakje groeperen om segmenten te maken in de koptekst van de functionele groep in de berichten die zijn verzonden naar de partner van de Gast. De volgende waarden worden gebruikt om de UNG segmenten te maken:</br></br>Voer voor **UNG1**, een alfanumerieke waarde met een minimum van één teken en een maximum van zes tekens.</br></br>Voer voor **UNG2.1**, een alfanumerieke waarde met een minimum van één teken en maximaal 35 tekens.</br></br>Voer voor **UNG2.2**, een alfanumerieke waarde, met een maximum van vier tekens.</br></br>Voer voor **UNG3.1**, een alfanumerieke waarde met een minimum van één teken en maximaal 35 tekens.</br></br>Voer voor **UNG3.2**, een alfanumerieke waarde, met een maximum van vier tekens.</br></br>Voer voor **UNG6**, een alfanumerieke waarde met minimaal één en maximaal drie tekens.</br></br>Voer voor **UNG7.1**, een alfanumerieke waarde met een minimum van één teken en maximaal drie tekens.</br></br>Voer voor **UNG7.2**, een alfanumerieke waarde met een minimum van één teken en maximaal drie tekens.</br></br>Voer voor **UNG7.3**, een alfanumerieke waarde met minimaal 1 teken en maximaal 6 tekens.</br></br>Voer voor **UNG8**, een alfanumerieke waarde met een minimum van één teken en maximaal 14 tekens.|

### <a name="character-sets-and-separators"></a>Teken Sets en scheidingstekens
Andere dan de tekenset kunt u een andere set met scheidingstekens worden gebruikt voor elk berichttype. Als u een tekenset voor een bepaald berichtschema niet wordt opgegeven, wordt de standaardtekenset gebruikt.

|Eigenschap|Beschrijving |
|----|----|
|UNB1.1 (systeem-id)|Selecteer de tekenset EDIFACT moet worden toegepast op de uitgaande uitwisseling.|
|Schema|Selecteer een schema in de vervolgkeuzelijst. Als elke rij is voltooid wordt een nieuwe rij toegevoegd. Schakel de scheidingspagina's instellen om te worden gebruikt voor het geselecteerde schema:</br></br>**Onderdeel element scheidingsteken** : Voer een enkel teken te scheiden van de samengestelde gegevenselementen.</br></br>**Element-scheidingsteken gegevens** – Voer een enkel teken voor het scheiden van eenvoudige gegevenselementen in samengestelde gegevenselementen.</br></br></br></br>**Vervangend teken** : Schakel dit selectievakje in als de nettolading aan gegevens bevat tekens die worden ook gebruikt als gegevens of segment onderdeel scheidingstekens. Vervolgens kunt u een vervangend teken. Als de uitgaande EDIFACT bericht worden gegenereerd, worden alle exemplaren van de scheidingstekens in de nettolading van gegevens vervangen door het opgegeven teken.</br></br>**Segment Terminator** : Voer een enkel teken aan het einde van een segment EDI.</br></br>Het **achtervoegsel** – Selecteer het teken dat wordt gebruikt met de id van het segment. Als u een achtervoegsel opgeeft, kan het segment terminator element leeg zijn. Als de terminator segment leeg laat, moet u een achtervoegsel opgeven.|

### <a name="control-numbers"></a>Control Numbers
|Eigenschap|Beschrijving |
|----|----|
|UNB5 (controlenummer Interchange)|Voer een voorvoegsel, een bereik van waarden voor het controlenummer uitwisseling, en een achtervoegsel. Deze waarden worden gebruikt voor het genereren van een uitgaande interchange. Het voorvoegsel en achtervoegsel zijn optioneel. het besturingselement dat is vereist. Het besturingselementnummer wordt verhoogd voor elk nieuw bericht; het voorvoegsel en achtervoegsel blijven hetzelfde.|
|UNG5 (groep controlenummer)|Voer een voorvoegsel, een bereik van waarden voor het controlenummer uitwisseling, en een achtervoegsel. Deze waarden worden gebruikt voor het genereren van het nummer van het besturingselement. Het voorvoegsel en achtervoegsel zijn optioneel. het besturingselement dat is vereist. Het besturingselement dat wordt voor elk nieuw bericht verhoogd tot de maximale waarde is bereikt; het voorvoegsel en achtervoegsel blijven hetzelfde.|
|UNH1 (Message Header nummer)|Voer een voorvoegsel, een bereik van waarden voor het controlenummer uitwisseling, en een achtervoegsel. Deze waarden worden gebruikt voor het genereren van het referentienummer van de bericht-header. Het voorvoegsel en achtervoegsel zijn optioneel. het nummer is vereist. Het referentienummer wordt verhoogd voor elk nieuw bericht; het voorvoegsel en achtervoegsel blijven hetzelfde.|

### <a name="validations"></a>Validaties
|Eigenschap|Beschrijving |
|----|----|
|Berichttype|Als u deze optie selecteert, kunt validatie op de ontvanger interchange. Deze validatie worden gevalideerd EDI transactie set elementen, validatie van gegevenstypen, lengterestricties en lege elementen en scheidingstekens voor opleiding.|
|EDI-validatie|Schakel dit selectievakje in als u EDI validatie uitvoeren voor gegevenstypen die door de EDI-eigenschappen van het schema, lengte beperkingen, lege elementen en afsluitende scheidingsteken.|
|Uitgebreide validatie|Als u deze optie selecteert, kunt uitgebreide validatie van het ontvangen van de afzender interchange uitgewisseld. Dit omvat de validatie van de veldlengte en optionaliteit herhalingen naast XSD-type gegevensvalidatie. Kunt u validatie extensie inschakelen zonder validatie EDI, inschakelen of vice versa.|
|Regelafstand/navolgende nullen toestaan|Met deze optie geeft aan dat een EDI uitwisseling heeft ontvangen van de partij wordt geen failback uitgevoerd validatie als een gegevenselement in een EDI uitwisseling niet aan de vereiste lengte vanwege of volgspaties voldoet, maar met de vereiste lengte overeen komt wanneer ze worden verwijderd.|
|Trim nullen regelafstand/volgspaties|Als u deze optie selecteert, wordt de nullen voorloop- en volgspaties bijsnijden.|
|Afsluitende scheidingsteken|Als wordt deze optie een EDI uitwisseling van de partij ontvangen mislukt validatie niet als een gegevenselement in een EDI uitwisseling niet aan de vereiste lengte (of achteraan) nullen of volgspaties voldoet, maar met de vereiste lengte overeen komt wanneer ze worden verwijderd.</br></br>Selecteer **Niet toegestaan** als u niet wilt toestaan navolgende scheidingstekens en scheidingstekens in een knooppunt van de afzender interchange ontvangen. Als het knooppunt aan het einde scheidingstekens en scheidingstekens bevat, is deze nietig verklaard.</br></br>**Optioneel** uitgewisseld met of zonder navolgende scheidingstekens en scheidingstekens accepteren selecteert.</br></br>Selecteer **verplicht** als de ontvangen uitwisseling moet achteraan scheidingstekens en scheidingstekens bevatten.|

Klik op **OK** nadat u op de open blade:  
12. Selecteer de tegel **overeenkomsten** op de integratie rekening blade en ziet u de nieuwe overeenkomst vermeld.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-4.png)   

## <a name="learn-more"></a>Meer informatie
- [Meer informatie over de Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack")  
