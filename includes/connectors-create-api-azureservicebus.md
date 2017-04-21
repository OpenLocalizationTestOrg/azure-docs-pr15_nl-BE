Nu dat u een trigger hebt toegevoegd, wordt de tijd om iets te doen met de gegevens die worden gegenereerd door de trigger interessant. Volg deze stappen om toe te voegen een de actie **SharePoint Online - bestand maken** . Deze actie maakt een bestand in SharePoint Online elke keer dat het nieuwe item trigger wordt geactiveerd. 

Voor het configureren van deze actie, moet u de volgende informatie te verstrekken. U ziet dat het gemakkelijk te gebruiken gegevens die zijn gegenereerd door de trigger als invoer voor een aantal van de eigenschappen voor het nieuwe bestand:

|Eigenschap file maken|Beschrijving|
|---|---|
|URL van de site|Dit is de URL van de SharePoint Online-site waar u het nieuwe bestand wilt maken. Selecteer de site in de lijst weergegeven.|
|Pad naar map|Dit is de map (op de Site-URL) waar het nieuwe bestand wordt geplaatst. Blader naar en selecteer de map.|
|Bestandsnaam|Dit is de naam van het bestand wordt gemaakt.|
|Inhoud van bestand|De inhoud die wordt geschreven naar het bestand.|

1. Selecteer **+ nieuwe stap** om de actie toevoegen.  
![](./media/connectors-create-api-sharepointonline/action-1.png)  
- Selecteer de koppeling **een actie toevoegen** . Dit opent het zoekvak waarin u naar elke actie u zoeken kunt wilt uitvoeren. In dit voorbeeld zijn de SharePoint-acties van belang.    
![](./media/connectors-create-api-sharepointonline/action-2.png)    
- *Sharepoint* voor acties met betrekking tot SharePoint zoeken invoeren.
- Selecteer de **SharePoint Online - bestand maken** als de te ondernemen actie.   **Opmerking**: u wordt gevraagd uw logica app toegang tot uw SharePoint-account als u dat nog niet gedaan eerder toe te staan.    
![](./media/connectors-create-api-sharepointonline/action-3.png)    
- Hiermee opent u het besturingselement **maken** .   
![](./media/connectors-create-api-sharepointonline/action-4.png)     
- Selecteer de **URL van de Site** en blader naar de locatie waar u het bestand wilt.     
![](./media/connectors-create-api-sharepointonline/action-5.png)  
- Selecteer **het pad** en blader naar de map waar u het nieuwe bestand wordt geplaatst.  
![](./media/connectors-create-api-sharepointonline/action-6.png)  
- Selecteer het besturingselement met **de naam** en typ de naam van het bestand dat u wilt maken. Voor de naam van het bestand ziet dat u de eigenschappen van de trigger die u eerder hebt gemaakt kunt door deze te selecteren in de lijst weergegeven.     
![](./media/connectors-create-api-sharepointonline/action-7.png)  
- Selecteer het besturingselement dat **de inhoud van bestand** en voer de inhoud wordt weggeschreven naar het bestand dat wordt gemaakt. U ziet dat u kunt de eigenschappen van de trigger die u eerder hebt gemaakt voor de inhoud van het bestand. Selecteer gewoon de eigenschappen in de lijst weergegeven. Ook kunt u de **inhoud van bestand** tekst rechtstreeks in het besturingselement invoeren. In dit voorbeeld ik geselecteerd aantal eigenschappen en ruimten en een streepje tussen elke eigenschap toegevoegd.        
![](./media/connectors-create-api-sharepointonline/action-8.png)  
- Sla de wijzigingen in uw workflow  
