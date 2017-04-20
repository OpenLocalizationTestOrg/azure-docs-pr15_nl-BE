#### <a name="prerequisites"></a>Vereisten
- Een account Azure; u kunt een [gratis account](https://azure.microsoft.com/free) maken
- Een account met [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) 

Toestemming voordat u uw account Dynamics in een app logica, de logica app verbinding maken met uw CRM Online-account. U kunt dit eenvoudig doen binnen uw app logica op de Azure portal. 

Toestaan dat uw app logica voor verbinding met uw CRM Online account met behulp van de volgende stappen uit:

1. Maak een app logica. In de ontwerpfunctie voor logica Apps **weergeven Microsoft beheerde API's** selecteren in de vervolgkeuzelijst en voer 'dynamics' in het zoekvak. Selecteer een van de triggers of acties:  
  ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Als u geen verbindingen met Dynamics eerder hebt gemaakt, moet u inloggen met uw Dynamics referenties:  
  ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Selecteer **aanmelden**en voer uw gebruikersnaam en wachtwoord. Selecteer **aanmelden**. 

    Deze referenties worden gebruikt om uw logica app verbinding met en toegang tot de gegevens in uw account Dynamics machtigen. 
4. U ziet dat de verbinding is gemaakt. Nu gaat u verder met de overige stappen in uw app logica:  
  ![](./media/connectors-create-api-crmonline/dynamics-properties.png)
