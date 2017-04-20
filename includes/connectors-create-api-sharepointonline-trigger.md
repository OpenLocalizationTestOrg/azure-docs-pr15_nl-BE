In dit voorbeeld leest u hoe de trigger **SharePoint Online - wanneer een nieuw item wordt gemaakt** met een werkstroom logica app starten wanneer een nieuw item wordt gemaakt in een SharePoint Online-lijst.

>[AZURE.NOTE]Zal krijgen moet u inloggen op uw SharePoint-account als u nog geen *verbinding* met de SharePoint Online gemaakt.  

1. *Sharepoint* invoeren in het zoekvak op de logica apps designer en selecteer vervolgens de trigger **SharePoint Online - wanneer een nieuw item wordt gemaakt** .  
![SharePoint online triggerafbeelding](./media/connectors-create-api-sharepointonline/trigger-1.png)  
- Het besturingselement **wanneer een nieuw item wordt gemaakt** , wordt weergegeven.  
![SharePoint online triggerafbeelding 2](./media/connectors-create-api-sharepointonline/trigger-2.png)   
- Selecteer een **Site-URL**. Dit is de online SharePoint-site die u controleren op nieuwe items wilt voor het starten van de werkstroom.  
![SharePoint online triggerafbeelding 3](./media/connectors-create-api-sharepointonline/trigger-3.png)   
- Selecteer een **naam**. Dit is de lijst op de SharePoint Online-site wilt controleren op nieuwe items waarmee de werkstroom wordt geactiveerd.  
![SharePoint online triggerafbeelding 4](./media/connectors-create-api-sharepointonline/trigger-4.png)   

Op dit moment is uw app logica geconfigureerd met een trigger die een uitvoering van de andere triggers en acties in de werkstroom wordt gestart. Dit gebeurt telkens wanneer die een nieuw item wordt gemaakt in SharePoint Online lijst die u hebt geselecteerd.  