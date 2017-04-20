### <a name="prerequisites"></a>Vereisten
- Een Twilio-account
- Een gecontroleerde Twilio telefoonnummer dat SMS kan ontvangen
- Een gecontroleerde Twilio telefoonnummer dat SMS kunt verzenden

>[AZURE.NOTE] Als u een evaluatieversie Twilio-account gebruikt, kunt u alleen SMS verzenden naar telefoonnummers **gecontroleerd** .  

Voordat u uw Twilio account in een logica app gebruiken kunt, moet u de logica app voor verbinding met uw account Twilio machtigen. U kunt dit gemakkelijk uit gelukkig doen binnen uw app logica op de Azure-Portal. 

Hier volgen de stappen voor het toestaan van uw app logica voor verbinding met uw account Twilio:

1. **Weergeven Microsoft beheerde API's** selecteren in de vervolgkeuzelijst om een verbinding maken met Twilio, in de ontwerpfunctie voor logica app, vervolgens *Twilio* invoert in het zoekvak. Selecteer de actie die u zult willen gebruiken of trigger:  
  ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Als u geen verbindingen met Twilio voordat u hebt gemaakt, zult u wordt gevraagd uw referenties Twilio op te geven. Deze referenties worden gebruikt voor het machtigen uw logica app verbinding met en toegang tot de gegevens van uw Twilio-account:  
  ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. U moet de **account-id van Twilio** en **Twilio-toegangstoken** van het dashboard in Twilio, dus Meld u aan bij uw account Twilio nu deze twee stukjes informatie ophaalt:  
  ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio en logica apps gebruiken verschillende namen voor deze twee soorten informatie. Hier ziet u hoe u ze moet toewijzen aan het dialoogvenster logica apps:![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Klik op de knop **verbinding maken** :  
  ![](./media/connectors-create-api-twilio/twilio-4.png)
6. U ziet de verbinding is gemaakt en u bent nu vrij om verder te gaan met de overige stappen in uw app logica:  
  ![](./media/connectors-create-api-twilio/twilio-5.png)