Hier ziet u hoe de trigger **Service Bus - wanneer een bericht wordt ontvangen in een wachtrij** met een werkstroom logica app starten wanneer een nieuw item wordt verzonden naar een wachtrij Service Bus.  

>[AZURE.NOTE]U wordt gevraagd aan te melden met uw Service Bus verbindingsreeks als u nog geen verbinding met de Service Bus gemaakt.  

1. Voer in het zoekvak op de logica apps designer **service bus**. Selecteer vervolgens de trigger **Service Bus - wanneer een bericht wordt ontvangen in een wachtrij** .  
![Triggerafbeelding Service Bus 1](./media/connectors-create-api-servicebus/trigger-1.png)   
- Het dialoogvenster **wanneer een bericht wordt ontvangen in een wachtrij** wordt weergegeven.  
![Triggerafbeelding Service Bus 2](./media/connectors-create-api-servicebus/trigger-2.png)   
- Voer de naam van de Service Bus-wachtrij die u de trigger dat wilt te controleren.   
![Triggerafbeelding Service Bus 3](./media/connectors-create-api-servicebus/trigger-3.png)   

Op dit moment is uw app logica geconfigureerd met een trigger. Wanneer een nieuw item in de geselecteerde wachtrij wordt ontvangen, wordt de trigger een uitvoering van de andere triggers en acties in de werkstroom gestart.    
