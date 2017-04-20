<properties 
    pageTitle="Overzicht van X12 en de Enterprise Integration Pack | Microsoft Azure App Service | Microsoft Azure" 
    description="Informatie over het gebruik van X12 overeenkomsten logica apps maken" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-x12"></a>Enterprise integratie met X12 

>[AZURE.NOTE]Deze pagina omvat de X12 functies van Apps logica. Klik voor informatie over EDIFACT [hier](app-service-logic-enterprise-integration-edifact.md).

## <a name="create-an-x12-agreement"></a>Maak een X12 overeenkomst 
Voordat u de X12 kunt uitwisselen berichten, moet u voor het maken van een X12 overeenkomst en deze opslaan in uw account integratie. De volgende stappen begeleidt u stapsgewijs door het proces van het maken van een X12 overeenkomst.

### <a name="heres-what-you-need-before-you-get-started"></a>Hier is wat u nodig hebt voordat u begint
- Een [account voor integratie](./app-service-logic-enterprise-integration-accounts.md) in uw abonnement Azure  
- Ten minste twee [partners](./app-service-logic-enterprise-integration-partners.md) al gedefinieerd in uw account integratie  

>[AZURE.NOTE]Bij het maken van een overeenkomst, de inhoud van het bestand van de overeenkomst, moet overeenkomen met het Overeenkomstsoort.    


Nadat u [gemaakt van een account integratie](./app-service-logic-enterprise-integration-accounts.md) en [partners toegevoegd hebt](./app-service-logic-enterprise-integration-partners.md), kunt u een X12 overeenkomst door de volgende stappen:  

### <a name="from-the-azure-portal-home-page"></a>Vanaf de introductiepagina van Azure portal

Nadat u zich aanmelden bij de [Azure portal](http://portal.azure.com "Azure portal"):  
1. Selecteer **Bladeren** in het menu aan de linkerkant.  

>[AZURE.TIP]Als u de koppeling **Bladeren** niet ziet, moet u mogelijk eerst het menu uitvouwen. Hiervoor selecteert u de koppeling **menu weergeven** op links boven in het menu samengevouwen.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. *Integratie* typt in het vak filter zoeken en selecteer vervolgens **Integratie Accounts** in de lijst met resultaten.       
![](./media/app-service-logic-enterprise-integration-x12/x12-1-3.png)    
3. In de blade **Integratie Accounts** die wordt geopend, selecteert u de integratie rekening waarin u de overeenkomst maken. Als er geen integratie van alle rekeningen lijsten, [maakt u een eerste](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-x12/x12-1-4.png)  
4.  Selecteer de tegel **overeenkomsten** . Als u de overeenkomsten naast elkaar weergeven niet ziet, moet u deze eerst toevoegen.   
![](./media/app-service-logic-enterprise-integration-x12/x12-1-5.png)     
5. Selecteer de knop **toevoegen** in het blad overeenkomsten die wordt geopend.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Voer een **naam** voor de overeenkomst en kies het **soort overeenkomst**, **Host Partner**, **Identiteit van de Host**, **Partner van de Gast**, **Gast identiteit**, in de overeenkomsten blade dat wordt geopend.  
![](./media/app-service-logic-enterprise-integration-x12/x12-1.png)  
7. Nadat u hebt ingesteld de ontvangen instellingen, klik op de knop **OK**  
We gaan verder:  
8. Selecteer **Instellingen voor ontvangen** om te configureren hoe berichten ontvangen via deze overeenkomst moeten worden verwerkt.  
9. Het besturingselement instellingen ontvangen is onderverdeeld in de volgende secties, met inbegrip van id's, bevestiging, schema's, enveloppen, Control Numbers, validatieprocedures en interne instellingen. Deze eigenschappen op basis van uw overeenkomst met de partner die u berichten uitwisselt configureren. Hier volgt een overzicht van deze besturingselementen, op basis van hoe u deze overeenkomst te herkennen en verwerken van inkomende berichten wilt configureren:  
![](./media/app-service-logic-enterprise-integration-x12/x12-2.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-3.png)  
10. Selecteer **OK** om uw instellingen op te slaan.  

### <a name="identifiers"></a>Id 's

|Eigenschap|Beschrijving |
|---|---|
|ISA1 (vergunning kwalificatie)|Selecteer de kwalificatie vergunning waarde uit de vervolgkeuzelijst.|
|ISA2|Dit is optioneel. Autorisatie informatie waarde invoeren. Als de waarde voor ISA1 niet 00 is, Voer ten minste één alfanumeriek teken en een maximum van 10.|
|ISA3 (beveiliging kwalificatie)|Selecteer de waarde voor kwalificatie in de lijst.|
|ISA4|Dit is optioneel. Geef de waarde voor informatie. Als de waarde voor ISA3 niet 00 is, Voer ten minste één alfanumeriek teken en een maximum van 10.|

### <a name="acknowledgments"></a>Bevestigingen 

|Eigenschap|Beschrijving |
|----|----|
|TA1 verwacht|Schakel dit selectievakje in dat een technische (TA1) bevestiging naar de afzender van de uitwisseling. Deze bevestigingen worden verzonden naar de afzender van de uitwisseling op basis van de instellingen voor verzenden voor de overeenkomst.|
|VA-verwacht|Schakel dit selectievakje in dat een functionele (VA) bevestiging naar de afzender van de uitwisseling. Selecteer of u wilt dat de bevestigingen 997 of 999 op basis van de versies van het schema die met werkt. Deze bevestigingen worden verzonden naar de afzender van de uitwisseling op basis van de instellingen voor verzenden voor de overeenkomst.|
|AK2 IK2/lus opnemen|Schakel dit selectievakje inschakelen generatie AK2 lussen in functionele bevestigingen voor geaccepteerde transactie sets. Opmerking: Dit selectievakje is alleen beschikbaar als u het selectievakje VA-verwacht geselecteerd.|

### <a name="schemas"></a>Schema 's

Kies een schema voor elk transactietype (ST1) en toepassing van de afzender (GS2). De pijpleiding ontvangen ontleedt het inkomende bericht door de waarden voor ST1 en GS2 in het inkomende bericht met de waarden die u hier instelt en het schema van het binnenkomende bericht met het schema die u hier instelt.

|Eigenschap|Beschrijving |
|----|----|
|Versie|Selecteer de X12 versie|
|Transactiesoort (ST01)|Selecteer het transactietype|
|Toepassing van de afzender (GS02)|Selecteer de toepassing van de afzender|
|Schema|Het schemabestand dat u ons wilt selecteren. Schemabestanden bevinden zich in uw account integratie.|

### <a name="envelopes"></a>Enveloppen

|Eigenschap|Beschrijving |
|----|----|
|Gebruik ISA11|Gebruik dit veld geeft u het scheidingsteken in een transactie:</br></br>Gebruik de notatie van de standaard-id selecteren '. " in plaats van de notatie van het inkomend document in het EDI ontvangen pijpleiding.</br></br>Selecteer scheidingspagina herhaling geeft het scheidingsteken voor herhaalde exemplaren van een eenvoudige data-element of een herhaalde gegevensstructuur. Bijvoorbeeld wordt (^) meestal gebruikt als scheidingsteken voor herhaling. U kunt alleen (^) voor HIPAA-schema's gebruiken.|

### <a name="control-numbers"></a>Control Numbers

|Eigenschap|Beschrijving |
|----|----|
|Controlenummer Interchange duplicaten toestaan|Schakel deze optie in voor het blokkeren van dubbele uitgewisseld. Ingeschakeld, controleert de BizTalk-Services Portal het controlenummer interchange (ISA13) voor het uitwisselen van ontvangen komt niet overeen met het controlenummer interchange. Als een overeenkomst wordt gevonden, worden de pipeline ontvangen de uitwisseling niet verwerkt.<br/>Als u ervoor hebt gekozen om dubbele interchange controlenummers niet toestaan, kunt u het aantal dagen waarop de controle wordt uitgevoerd door middel van de juiste waarde voor het controleren op dubbele ISA13 om de x dagen opgeven.|
|Groep besturingselement aantal duplicaten toestaan|Schakel deze optie uitgewisseld met dubbele controlenummers blokkeren.|
|Transactie set besturingselement aantal duplicaten toestaan|Schakel deze optie uitgewisseld met dubbele transactie set besturingselement nummers blokkeren.|

### <a name="validations"></a>Validaties

|Eigenschap|Beschrijving |
|----|----|
|Berichttype|EDI-bericht type, zoals 850 aankooporder of bevestiging 999-uitvoering.|
|EDI-validatie|EDI validatie uitvoert op de gegevenstypen zoals gedefinieerd door de EDI-eigenschappen van het schema, lengte beperkingen, lege elementen en afsluitende scheidingsteken.|
|Uitgebreide validatie|Als het gegevenstype niet EDI, is de vereiste gegevens element validatie ingeschakeld en herhaling, opsommingen en element lengte gegevensvalidatie (min/max) toegestaan.|
|Regelafstand/navolgende nullen toestaan|Geen extra ruimte en nul tekens voor voorloop- of volgspaties zijn blijven behouden. Ze worden niet verwijderd.|
|Afsluitende scheidingsteken beleid|Genereert de navolgende scheidingstekens op het knooppunt ontvangen. Opties omvatten NotAllowed, optioneel en verplicht.|

### <a name="internal-settings"></a>Interne instellingen

|Eigenschap|Beschrijving |
|----|----|
|Impliciete decimale notatie Nn basis 10 numerieke waarde converteren|Converteert een getal EDI die is opgegeven met de notatie Nn in een base-10 numerieke waarde in de tussenliggende XML in de Portal BizTalk-Services.|
|Lege XML-tags maken als afsluitende scheidingstekens zijn toegestaan.|Schakel dit selectievakje in om de uitwisseling afzender bevatten lege XML-codes voor het afsluitende scheidingsteken.|
|Verwerking van inkomende batchen|Interchange splitsen als sets van transactie - transactie sets onderbreken bij fout: elke transactie in een knooppunt in een apart XML-document instellen door de juiste envelop tot de transactie wordt geparseerd. Met deze optie als een of meer transacties wordt ingesteld in de uitwisseling validatie mislukt, wordt alleen die transactie sets BizTalk-Services worden onderbroken. </br></br>Interchange splitsen als sets van transactie - uitwisseling onderbreken bij fout: elke transactie in een knooppunt in een apart XML-document instellen door de juiste envelop wordt geparseerd. Met deze optie als een of meer transacties wordt ingesteld in de uitwisseling validatie mislukt, wordt de hele uitwisseling BizTalk-Services worden onderbroken.</br></br>Behouden Interchange - transactie sets onderbreken bij fout: blijft de uitwisseling behouden, maakt een XML-document voor de hele batch uitwisseling. Met deze optie wordt als onAe of meer transacties wordt ingesteld in de uitwisseling validatie mislukt, wordt alleen deze transactie wordt ingesteld, terwijl u doorgaat met het verwerken van alle andere transactie sets BizTalk-Services worden onderbroken.</br></br>Behouden Interchange - uitwisseling onderbreken bij fout: blijft de uitwisseling behouden, maakt een XML-document voor de hele batch uitwisseling. Met deze optie als een of meer transacties wordt ingesteld in de uitwisseling validatie mislukt, wordt de hele uitwisseling BizTalk-Services worden onderbroken.</br></br>|

De overeenkomst is geschikt voor het verwerken van binnenkomende berichten die voldoen aan het schema dat u hebt geselecteerd.

Configureer de instellingen die u naar partners verzendt berichten verwerken:  
11. Selecteer **Instellingen voor verzenden** om te configureren hoe berichten die worden verzonden via deze overeenkomst moeten worden verwerkt.  

De controle van de instellingen voor verzenden is onderverdeeld in de volgende secties, met inbegrip van id's, bevestiging, schema's, enveloppen, Control Numbers, tekensets en scheidingstekens en validatie. 

Hier volgt een overzicht van deze besturingselementen. De selecties op basis van hoe u wilt omgaan met berichten die u naar partners via deze overeenkomst verzendt maken:   
![](./media/app-service-logic-enterprise-integration-x12/x12-4.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-5.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-6.png)  
12. Selecteer **OK** om uw instellingen op te slaan.  

### <a name="identifiers"></a>Id 's
|Eigenschap|Beschrijving |
|----|----|
|Vergunning kwalificatie (ISA1)|Selecteer de kwalificatie vergunning waarde uit de vervolgkeuzelijst.|
|ISA2|Autorisatie informatie waarde invoeren. Als deze waarde niet 00 is, geeft u ten minste één alfanumeriek teken en een maximum van 10.|
|Beveiliging-kwalificatie (ISA3)|Selecteer de waarde voor kwalificatie in de lijst.|
|ISA4|Geef de waarde voor informatie. Als deze waarde dan 00 voor het tekstvak waarde (ISA4 is) geeft u een alfanumerieke waarde minimaal en maximaal 10.|

### <a name="acknowledgment"></a>Bevestiging
|Eigenschap|Beschrijving |
|----|----|
|TA1 verwacht|Schakel dit selectievakje in dat een technische (TA1) bevestiging naar de afzender van de uitwisseling. Deze instelling geeft aan dat de partner van de host die het bericht verzendt een ontvangstbevestiging van de Gast-partner in de overeenkomst vraagt. Deze bevestigingen worden verwacht door de host-partner op basis van de instellingen voor het ontvangen van de overeenkomst.|
|VA-verwacht|Schakel dit selectievakje in om een functionele (VA) bevestiging naar de afzender interchange terug en selecteer vervolgens of u wilt dat de bevestigingen 997 of 999 op basis van het schema versies dat u met werkt. Deze bevestigingen worden verwacht door de host-partner op basis van de instellingen voor het ontvangen van de overeenkomst.|
|VA-versie|Selecteer de VA-versie|

### <a name="schemas"></a>Schema 's
|Eigenschap|Beschrijving |
|----|----|
|Versie|Selecteer de X12 versie|
|Transactiesoort (ST01)|Selecteer het transactietype|
|SCHEMA|Selecteer het schema dat u wilt gebruiken. Schema's bevinden zich in uw account integratie. Als u uw schema's, uw account integratie eerst aan uw app logica te koppelen.|

### <a name="envelopes"></a>Enveloppen
|Eigenschap|Beschrijving |
|----|----|
|Gebruik ISA11|Gebruik dit veld geeft u het scheidingsteken in een transactie:</br></br>Gebruik de notatie van de standaard-id selecteren '. " in plaats van de notatie van het inkomend document in het EDI ontvangen pijpleiding.</br></br>Selecteer scheidingspagina herhaling geeft het scheidingsteken voor herhaalde exemplaren van een eenvoudige data-element of een herhaalde gegevensstructuur. Bijvoorbeeld wordt (^) meestal gebruikt als scheidingsteken voor herhaling. U kunt alleen (^) voor HIPAA-schema's gebruiken.</br>|
|Scheidingsteken voor herhaling|Geef het scheidingsteken voor herhaling|
|Het versienummer Control (ISA12)|Selecteer de versie van de standaard X12 die wordt gebruikt door de BizTalk-Services Portal voor het genereren van een uitgaande interchange.|
|Gebruiksindicator (ISA15)|Opgeven of informatie (I) de productiegegevens (P), de context van een knooppunt of gegevens (T) te testen. De EDI ontvangen pijpleiding bevordert deze eigenschap in op de context.|
|Schema|U kunt opgeven hoe de BizTalk-Services Portal genereert de GS en ST-segmenten voor een X12 gecodeerd uitwisselen die stuurt het naar de pijpleiding verzenden.</br></br>U kunt de waarden van de elementen met de waarden van het Type transactie GS1, GS2, GS3, GS4, GS5, GS7 en GS8 en gegevenselementen versie/koppelen. Wanneer u de BizTalk-Services Portal bepaalt dat een XML-bericht is de ingestelde waarden voor de transactietype en versie /-elementen in een rij van het raster en vervolgens het vult de gegevenselementen GS1, GS2, GS3, GS4, GS5, GS7 en GS8 in de envelop van de uitgaande uitwisseling door de waarden in dezelfde rij van het raster. De waarde van het Type transactie en versie/elementen moeten uniek zijn.</br></br>Dit is optioneel. Voor GS1, een waarde voor de functionele code te selecteren uit de vervolgkeuzelijst.</br></br>Vereist. Voer voor GS2, een alfanumerieke waarde voor de afzender toepassing met een minimum van twee tekens en maximaal 15 tekens.</br></br>Vereist. Voer voor GS3, een alfanumerieke waarde voor de ontvanger van de toepassing met een minimum van twee tekens en maximaal 15 tekens.</br></br>Dit is optioneel. Selecteer voor GS4, CCYYMMDD of JJMMDD.</br></br>Dit is optioneel. Selecteer voor GS5, UUMM, UUMMSS of HHMMSSdd.</br></br>Dit is optioneel. Voor GS7, een waarde voor het Agentschap verantwoordelijk te selecteren uit de vervolgkeuzelijst.</br></br>Dit is optioneel. Voer voor GS8, een alfanumerieke waarde voor het document aangeduid met ten minste één teken en een maximum van 12 tekens.</br></br>**Opmerking**: dit zijn de waarden die de BizTalk-Services Portal worden ingevoerd in de velden GS van de uitwisseling bouwt als de transactie en versie/elementen in dezelfde rij zijn een overeenkomst voor degenen die zijn gekoppeld aan het knooppunt.|

### <a name="control-numbers"></a>Control Numbers
|Eigenschap|Beschrijving |
|----|----|
|Interchange controlenummer (ISA13)|Vereist. Een bereik van waarden invoeren voor het controlenummer interchange is gebruikt door de BizTalk-Services Portal bij het genereren van een uitgaande interchange. Voer een numerieke waarde met minimaal 1 en maximaal 999999999.|
|Groep controlenummer (GS06)|Vereist. Geef het bereik van getallen die de BizTalk-Services Portal voor het nummer van het besturingselement moet worden gebruikt. Voer een numerieke waarde met een minimum van één teken en een maximum van negen tekens.|
|Transactie Set controlenummer (ST02)|Voer voor transactie instellen controlenummer (ST02), een reeks numerieke waarden voor de vereiste velden in de middelste en alfanumerieke waarden voor een optioneel voorvoegsel en achtervoegsel. De maximale lengte van alle vier velden is negen tekens.|
|Voorvoegsel|Voer waarden in de velden van het ACK nummer (ST02) aan te wijzen in het bereik van de transactie set besturingselement getallen die worden gebruikt in een bevestiging. Voer een numerieke waarde voor de middelste twee velden en een alfanumerieke waarde (indien gewenst) voor de velden voor voor- en achtervoegsel. De middelste zijn vereist en de minimale en maximale waarden bevatten voor het controlenummer; het voorvoegsel en achtervoegsel zijn optioneel. De maximale lengte voor alle drie de velden is negen tekens.|
|Achtervoegsel|Voer waarden in de velden van het ACK nummer (ST02) aan te wijzen in het bereik van de transactie set besturingselement getallen die worden gebruikt in een bevestiging. Voer een numerieke waarde voor de middelste twee velden en een alfanumerieke waarde (indien gewenst) voor de velden voor voor- en achtervoegsel. De middelste zijn vereist en de minimale en maximale waarden bevatten voor het controlenummer; het voorvoegsel en achtervoegsel zijn optioneel. De maximale lengte voor alle drie de velden is negen tekens.|

### <a name="character-sets-and-separators"></a>Teken Sets en scheidingstekens
Andere dan de tekenset kunt u een andere set met scheidingstekens worden gebruikt voor elk berichttype. Als u een tekenset voor een bepaald berichtschema niet wordt opgegeven, wordt de standaardtekenset gebruikt.

|Eigenschap|Beschrijving |
|----|----|
|Tekenset wordt gebruikt|Selecteer de X12 tekenset voor het valideren van de eigenschappen die u voor de overeenkomst invoert.</br></br>**Opmerking**: alleen gebruikt deze instelling voor het valideren van de waarden voor de eigenschappen van de verwante overeenkomst de BizTalk-Services Portal. De pijpleiding ontvangen of verzenden pijpleiding negeert deze eigenschap tekenset tijdens runtime-verwerking.|
|Schema|Selecteer het (+) symbool en selecteer een schema uit de vervolgkeuzelijst. Schakel de scheidingspagina's instellen om te worden gebruikt voor het geselecteerde schema:</br></br>Onderdeel element scheidingsteken: Voer een enkel teken te scheiden van de samengestelde gegevenselementen.</br></br>Data-Element scheidingsteken: Voer een enkel teken voor het scheiden van eenvoudige gegevenselementen in samengestelde gegevenselementen.</br></br></br></br>Vervangingsteken: Schakel dit selectievakje in als de nettolading aan gegevens bevat tekens die worden ook gebruikt als gegevens of segment onderdeel scheidingstekens. Vervolgens kunt u een vervangend teken. Bij het genereren van de uitgaande bericht X12, alle exemplaren van de scheidingstekens in de payload gegevens worden vervangen door het opgegeven teken.</br></br>Terminator in segmenten: Voer een enkel teken te geven aan het einde van een segment EDI.</br></br>Achtervoegsel – Selecteer het teken dat wordt gebruikt met de id van het segment. Als u een achtervoegsel opgeeft, kan het segment terminator element leeg zijn. Als de terminator segment leeg laat, moet u een achtervoegsel opgeven.|

### <a name="validation"></a>Validatie
|Eigenschap|Beschrijving |
|----|----|
|Berichttype|Als u deze optie selecteert, kunt validatie op de ontvanger interchange. Deze validatie worden gevalideerd EDI transactie set elementen, gegevenstypen, lengterestricties en lege elementen valideren en het afsluitende scheidingsteken.|
|EDI-validatie||
|Uitgebreide validatie|Als u deze optie selecteert, kunt uitgebreide validatie van het ontvangen van de afzender interchange uitgewisseld. Dit omvat de validatie van de veldlengte en optionaliteit herhalingen naast XSD-type gegevensvalidatie. Kunt u validatie extensie inschakelen zonder validatie EDI, inschakelen of vice versa.|
|Toonaangevende / naloopspaties nullen toestaan|Met deze optie geeft aan dat een EDI uitwisseling heeft ontvangen van de partij wordt geen failback uitgevoerd validatie als een gegevenselement in een EDI uitwisseling niet aan de vereiste lengte vanwege of volgspaties voldoet, maar met de vereiste lengte overeen komt wanneer ze worden verwijderd.|
|Afsluitende scheidingsteken|Als wordt deze optie een EDI uitwisseling van de partij ontvangen mislukt validatie niet als een gegevenselement in een EDI uitwisseling niet aan de vereiste lengte (of achteraan) nullen of volgspaties voldoet, maar met de vereiste lengte overeen komt wanneer ze worden verwijderd.</br></br>Selecteer niet toegestaan als u niet wilt toestaan navolgende scheidingstekens en scheidingstekens in een knooppunt van de afzender interchange ontvangen. Als het knooppunt aan het einde scheidingstekens en scheidingstekens bevat, is deze nietig verklaard.</br></br>Optioneel uitgewisseld met of zonder navolgende scheidingstekens en scheidingstekens accepteren selecteert.</br></br>Selecteer verplicht als de ontvangen uitwisseling moet achteraan scheidingstekens en scheidingstekens bevatten.|

Klik op **OK** nadat u op de open-blades:  
13. Selecteer de tegel **overeenkomsten** op de integratie rekening blade en ziet u de nieuwe overeenkomst vermeld.  
![](./media/app-service-logic-enterprise-integration-x12/x12-7.png)   

## <a name="learn-more"></a>Meer informatie
- [Meer informatie over de Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack")  
