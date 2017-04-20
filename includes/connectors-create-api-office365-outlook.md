#### <a name="prerequisites"></a>Vereisten
- Een account Azure; u kunt een [gratis account](https://azure.microsoft.com/free) maken
- Een [Office 365](https://office365.com) -account  

Voordat u uw account voor Office 365 in een app logica, de logica app verbinding maken met uw account voor Office 365 te autoriseren. U kunt dit eenvoudig doen binnen uw app logica op de Azure portal.  

Toestaan dat uw logica app verbinding maken met uw Office 365-account met behulp van de volgende stappen uit:

1. Maak een app logica. In de ontwerpfunctie voor logica Apps **weergeven Microsoft beheerde API's** selecteren in de vervolgkeuzelijst en voer 'office 365' in het zoekvak. Selecteer een van de triggers of acties:  
    ![Stap voor Office 365-verbinding maken](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  

2. Als u eerder geen verbindingen met Office 365 hebt gemaakt, moet u inloggen met uw Office 365-referenties:  
    ![Stap voor Office 365-verbinding maken](./media/connectors-create-api-office365-outlook/office365-signin.png)  

3. Selecteer **aanmelden**en voer uw gebruikersnaam en wachtwoord. Selecteer **aanmelden**:  
    ![Stap voor Office 365-verbinding maken](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)

    Deze referenties worden gebruikt om u te machtigen uw logica app verbinding met en toegang tot uw account voor Office 365. 

4. U ziet dat de verbinding is gemaakt. Nu gaat u verder met de overige stappen in uw app logica:   
    ![Stap voor Office 365-verbinding maken](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  
  