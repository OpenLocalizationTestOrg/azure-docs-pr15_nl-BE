Nu dat u een trigger hebt toegevoegd, wordt de tijd om iets te doen met de gegevens die worden gegenereerd door de trigger interessant. Volg deze stappen uit om de actie van de **SharePoint Online - bestand maken** . Deze actie maakt een bestand in SharePoint Online elke keer dat het nieuwe item trigger wordt geactiveerd. 

Voor het configureren van deze actie, moet u de volgende informatie te verstrekken. Als u deze gegevens verstrekt, ziet u dat het is gemakkelijk te gebruiken gegevens die zijn gegenereerd door de trigger als invoer voor een aantal van de eigenschappen voor het nieuwe bestand:

|Eigenschap file maken|Beschrijving|
|---|---|
|URL van de site|Dit is de URL van de SharePoint Online-site waar u het nieuwe bestand wilt maken. Selecteer de site in de lijst weergegeven.|
|Pad naar map|Dit is de map (op de URL van de Site die is geselecteerd in de vorige stap) waar het nieuwe bestand wordt geplaatst. Blader naar en selecteer de map.|
|Bestandsnaam|Dit is de naam van het bestand wordt gemaakt.|
|Inhoud van bestand|De inhoud die wordt geschreven naar het bestand.|

1. Selecteer **+ nieuwe stap** om de actie toevoegen.  
![SharePoint online actie afbeelding 1](./media/connectors-create-api-sharepointonline/action-1.png)  
- Selecteer de koppeling **een actie toevoegen** . Dit opent het zoekvak waarin u naar elke actie u zoeken kunt wilt uitvoeren. In dit voorbeeld zijn de SharePoint-acties van belang.    
![SharePoint online actie afbeelding 2](./media/connectors-create-api-sharepointonline/action-2.png)    
- *Sharepoint* voor acties met betrekking tot SharePoint zoeken invoeren.
- Selecteer de **SharePoint Online - bestand maken** als de te ondernemen actie.   **Opmerking**: u wordt gevraagd te machtigen uw logica app voor toegang tot uw SharePoint-account als u een verbinding met de SharePoint Online eerder hebt gemaakt.    
![SharePoint online actie afbeelding 3](./media/connectors-create-api-sharepointonline/action-3.png)    
- Hiermee opent u het besturingselement **maken** .   
![Afbeelding van SharePoint online actie 4](./media/connectors-create-api-sharepointonline/action-4.png)     
- Selecteer de **URL van de Site** en blader naar de locatie waar u het bestand wilt.     
![Afbeelding van SharePoint online actie 5](./media/connectors-create-api-sharepointonline/action-5.png)  
- Selecteer **het pad** en blader naar de map waar u het nieuwe bestand wordt geplaatst.  
![Afbeelding van SharePoint online actie 6](./media/connectors-create-api-sharepointonline/action-6.png)  
- Selecteer het besturingselement met **de naam** en typ de naam van het bestand dat u wilt maken. Hier kunt u kunt de naam van het rechtstreeks invoeren of kunt u de eigenschappen van de trigger die u eerder hebt gemaakt. U doen dit door de eigenschappen te selecteren in de lijst met **resultaten wanneer een nieuw item wordt gemaakt**. Deze lijst is alleen weergegeven nadat u het besturingselement **bestandsnaam** selecteren. In deze walkthough, ik ID (ID van de nieuwe lijst) geselecteerd als de naam van het bestand dat wordt gemaakt door de actie van de **SharePoint Online - bestand maken** .    
![Afbeelding van SharePoint online actie 7](./media/connectors-create-api-sharepointonline/action-7.png)  
- Selecteer het besturingselement dat **de inhoud van bestand** en voer de inhoud wordt weggeschreven naar het bestand dat wordt gemaakt. U ziet dat u kunt de eigenschappen van de trigger die u eerder hebt gemaakt voor de inhoud van het bestand. Selecteer gewoon de eigenschappen in de lijst weergegeven. Ook kunt u de **inhoud van bestand** tekst rechtstreeks in het besturingselement invoeren. In dit voorbeeld ik geselecteerd aantal eigenschappen en ruimten en een streepje tussen elke eigenschap toegevoegd.        
![SharePoint online actie afbeelding 8](./media/connectors-create-api-sharepointonline/action-8.png)  
- Sla de wijzigingen in uw workflow  
- Gefeliciteerd, u hebt nu een app volledig functionele logica die wordt geactiveerd wanneer een nieuw item wordt toegevoegd aan een SharePoint Online-lijst. De app maakt dan een bestand met behulp van enkele van de eigenschappen van het nieuwe item in de lijst.  U kunt nu het formulier testen door een nieuw item maken in de SharePoint-lijst. 
