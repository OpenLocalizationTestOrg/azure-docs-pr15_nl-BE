#### <a name="prerequisites"></a>Vereisten
- Een account Azure; u kunt een [gratis account](https://azure.microsoft.com/free) maken
- Een [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) -account 

Voordat u uw account OneDrive in een app logica gebruiken kunt, toestaan dat de logica app voor verbinding met uw account OneDrive.  U kunt dit eenvoudig doen binnen uw app logica op de Azure portal. 

Toestaan dat uw logica app verbinding maken met uw OneDrive-account met behulp van de volgende stappen uit:

1. Maak een app logica. In de ontwerpfunctie voor logica Apps **weergeven Microsoft beheerde API's** selecteren in de vervolgkeuzelijst en voer 'onedrive' in het zoekvak. Selecteer een van de triggers of acties:  
  ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Als u geen verbindingen met OneDrive eerder hebt gemaakt, moet u inloggen met uw referenties OneDrive:  
  ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Selecteer **aanmelden**en voer uw gebruikersnaam en wachtwoord. Selecteer **aanmelden**:  
  ![](./media/connectors-create-api-onedrive/onedrive-3.png)   

    Deze referenties worden gebruikt om u te machtigen uw logica app verbinding met en toegang tot de gegevens in uw account OneDrive. 
4. Selecteer **Ja** om te machtigen de logica app je OneDrive-account gebruiken:  
  ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. U ziet dat de verbinding is gemaakt. Nu gaat u verder met de overige stappen in uw app logica:  
  ![](./media/connectors-create-api-onedrive/onedrive-5.png)
