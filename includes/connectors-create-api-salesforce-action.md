Nu dat u een voorwaarde hebt toegevoegd, wordt de tijd om iets te doen met de gegevens die worden gegenereerd door de trigger interessant. Volg deze stappen uit om de actie **televergaderingen - object ophalen** . Hierdoor krijgt de gegevens telkens wanneer die een nieuwe lead is gemaakt. U wordt ook een tweede actie met de gegevens van de televergaderingen - een actie bij object ophalen voor het verzenden van een e-mailbericht met de Office 365-connector toevoegen.  

Voor het configureren van deze actie, moet u de volgende informatie te verstrekken. U ziet dat het gemakkelijk te gebruiken gegevens die zijn gegenereerd door de trigger als invoer voor een aantal van de eigenschappen voor het nieuwe bestand:

|Eigenschap file maken|Beschrijving|
|---|---|
|Objecttype|Dit is het type object dat u geïnteresseerd bent in televergaderingen. Voorbeelden zijn de Lead, Account, enz.|
|Object-ID|Hiermee wordt een id voor het object.|


1. Selecteer de koppeling **een actie toevoegen** . Dit opent het zoekvak waarin u naar elke actie u zoeken kunt wilt uitvoeren. In dit voorbeeld zijn televergaderingen acties van belang.      
![Afbeelding van de actie televergaderingen 1](./media/connectors-create-api-salesforce/action-1.png)  
- *Televergaderingen* om te zoeken naar acties met betrekking tot de televergaderingen invoeren.
- Selecteer de te ondernemen actie **televergaderingen - object ophalen** .   **Opmerking**: u wordt gevraagd uw logica app toegang tot uw account televergaderingen als u dat nog niet gedaan eerder toe te staan.    
![Afbeelding van de actie televergaderingen 2](./media/connectors-create-api-salesforce/action-2.png)    
- Het control **-object ophalen** wordt geopend.  
- Selecteer *leiden* als het objecttype.
- Selecteer het besturingselement voor de **Object-ID** .
- **...** Selecteren om de lijst met tokens die worden gebruikt als invoer voor acties weer te geven.       
![Afbeelding van de actie televergaderingen 3](./media/connectors-create-api-salesforce/action-3.png)    
- Selecteer de **Lead-ID** -beheer wordt geopend.   
![Afbeelding van de actie televergaderingen 4](./media/connectors-create-api-salesforce/action-4.png)     
- U ziet het token Lead-ID wordt nu in het besturingselement Object-ID, die aangeeft dat de actie bij object ophalen voor een potentiële klant met een ID die gelijk is aan de potentiële klant-ID van potentiële klanten die deze logica app geactiveerd wordt gezocht.  
![Afbeelding van de actie televergaderingen 5](./media/connectors-create-api-salesforce/action-5.png)  
- Sla uw werk op. Dat is het, u kunt de actie bij object ophalen hebt toegevoegd aan uw app logica. De controle-object ziet er zo uit:    
![Afbeelding van de actie televergaderingen 6](./media/connectors-create-api-salesforce/action-6.png)  

Nu dat u een actie als u een lead hebt toegevoegd, kunt u kunt doen iets interessants aan de nieuwe potentiële klant. In een onderneming kunt u een e-mail sturen naar een distributielijst met de melding dat er een nieuwe lead is gemaakt. We gebruiken de Office 365-connector te sturen een e-mail met een aantal relevante gegevens van het nieuwe object met lood in de televergaderingen.  

1. Selecteer **een actie toevoegen** en vervolgens *e-mailadres* invoeren in het besturingselement zoeken gemarkeerd. Deze filters voor de acties die betrekking hebben op het verzenden en ontvangen van e-mail.  
- Selecteer het item in de lijst **Office 365 Outlook - een e-mailbericht verzenden** . Als u al een *verbinding* met uw Office 365-account hebt gemaakt, wordt u gevraagd uw referenties voor Office 365 nu maken als u wilt invoeren. Nadat u klaar bent, wordt het besturingselement voor het **verzenden van een e-mailbericht** wordt geopend.        
![Afbeelding van de actie televergaderingen 7](./media/connectors-create-api-salesforce/action-7.png)  
- Voer het e-mailadres dat u graag om e-mail te verzenden naar in het besturingselement **aan** .
-  In het besturingselement **onderwerp** *gemaakt nieuwe Lead* - invoeren en selecteer vervolgens het token *bedrijf* . Het veld *bedrijf* uit de nieuwe potentiële klant gemaakt in de televergaderingen wordt weergegeven.  
-  U kunt kiezen uit de tokens uit de nieuwe lead-object en u kunt ook opgeven welke tekst u wilt weergeven in de hoofdtekst van het e-mailbericht in **de hoofdtekst** . Hier volgt een voorbeeld:  
![Televergaderingen actie afbeelding 8](./media/connectors-create-api-salesforce/action-8.png)   
- Sla uw werkstroom.  

Dat is. Uw app logica is nu voltooid.  

Nu kunt u uw app logica testen: in televergaderingen, maakt u een nieuwe lead die voldoet aan de voorwaarde die u hebt gemaakt.  Als u dit overzicht volledig uitgevoerd, alleen een lead maken met een e-mailadres met *amazon.com* erin. Na een paar seconden uw app logica moet worden gestart en de resultaten kunnen uitzien:  
![Afbeelding van de actie televergaderingen 9](./media/connectors-create-api-salesforce/action-9.png)  

