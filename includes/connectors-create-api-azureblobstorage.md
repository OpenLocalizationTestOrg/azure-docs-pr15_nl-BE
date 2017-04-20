### <a name="prerequisites"></a>Vereisten
- Een account Azure; u kunt een [gratis account](https://azure.microsoft.com/free) maken
- Een [account Azure Blob-opslag](../articles/storage/storage-create-storage-account.md) met inbegrip van de naam van de opslag en de toegangstoets. Deze informatie wordt weergegeven in de eigenschappen van de account van de opslag in de portal Azure. Lees meer over [Azure opslag](../articles/storage/storage-introduction.md).

Voordat u uw account Azure Blob-opslag in een app logica, verbinding maken met uw account Azure Blob-opslag. U kunt dit eenvoudig doen binnen uw app logica op de Azure portal.  

Verbinding maken met uw account Azure Blob-opslag is als volgt te werk:  

1. Maak een app logica. In de ontwerpfunctie voor Apps logica een trigger toevoegen en vervolgens een actie toevoegen. **Weergeven Microsoft beheerde API's** selecteren in de vervolgkeuzelijst en voer 'blob' in het zoekvak. Selecteer een van de acties:  

    ![Azure Blob-opslag verbinding maken stap](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  

2. Als u eerder geen verbindingen met Azure opslag hebt gemaakt, wordt u gevraagd de verbindingsdetails:   

    ![Azure Blob-opslag verbinding maken stap](./media/connectors-create-api-azureblobstorage/connection-details.png)  

3. Geef de details van de opslag. Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
|---|---|
| Naam van de verbinding * | Voer een naam voor de verbinding. |
| Azure opslag accountnaam * | Voer de naam van de opslag. De naam van de opslag wordt weergegeven in de opslageigenschappen in de portal Azure. |
| Azure Storage Account Access Key * | Geef de sleutel opslag account. De toegangstoetsen worden weergegeven in de opslageigenschappen in de portal Azure. |

    Deze referenties worden gebruikt om te machtigen uw logica app verbinding maken en toegang tot uw gegevens. 

4. Selecteer **maken**.

5. U ziet dat de verbinding is gemaakt. Nu gaat u verder met de overige stappen in uw app logica: 

    ![Azure Blob-opslag verbinding maken stap](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  
