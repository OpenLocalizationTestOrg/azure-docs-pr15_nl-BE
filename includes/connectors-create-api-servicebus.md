### <a name="prerequisites"></a>Vereisten

U moet een [Bus Service](https://azure.microsoft.com/services/service-bus/) -account hebben.  

Voordat u uw account Azure Service Bus in een logica app gebruiken kunt, moet u de logica app verbinding maken met uw account-bus machtigen. U kunt dit gemakkelijk uit gelukkig doen binnen uw app logica op de Azure portal.  

Hier zijn de stappen te machtigen uw app logica voor verbinding met uw Service Bus-account:  

1. Een verbinding wilt maken op Service Bus, in de ontwerpfunctie voor logica app, selecteer **weergeven Microsoft beheerde API's** in de vervolgkeuzelijst. **Bus service** vervolgens invoeren in het zoekvak. Selecteer de trigger of de actie die u wilt gebruiken.  
    ![Service Bus verbinding afbeelding 1](./media/connectors-create-api-servicebus/servicebus-1.png)  

2. Als u nog geen verbindingen met de Service-Bus eerder wordt gemaakt, wordt u gevraagd uw referenties Service Bus op te geven. Deze referenties worden gebruikt om te machtigen uw logica app verbinding met en toegang tot de gegevens van uw account Service Bus. De Service Bus connector moet de verbindingsreeks voor de naamruimte Service Bus. Ook moet machtigingen **beheren** . Een goede manier om te weten als de verbindingsreeks voor de naamruimte of een specifieke entiteit als bevat de `EntityPath` parameter. Als dit het geval is, is het niet de juiste verbindingsreeks voor een app logica.  
    ![De verbindingsreeks Service Bus](./media/connectors-create-api-servicebus/connectionstring.png)

1. Nadat u de verbindingsreeks voor de naamruimte hebt ontvangen, kunt u deze voor de verbinding van de API in logica Apps.  
    ![Service Bus verbinding afbeelding 2](./media/connectors-create-api-servicebus/servicebus-2.png)  

3. U ziet de verbinding is gemaakt en u bent nu vrij om verder te gaan met de overige stappen in uw app logica.  
    ![Service Bus verbinding afbeelding 3](./media/connectors-create-api-servicebus/servicebus-3.png)   
