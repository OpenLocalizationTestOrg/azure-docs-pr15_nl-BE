Met behulp van Visual Studio **Server Explorer**kunt u wachtrijen Azure opslag.

![Server Explorer BLOB 's][Image1]

1. Kies **Server Explorer**in het menu **Beeld** .
2. Vouw het knooppunt **Azure** voor uw abonnement in Server Explorer, vouw het knooppunt van de **opslag** en het knooppunt voor de opslag-account die u hebt opgegeven in de verbonden Azure-Storage-service.
3. Selecteer het knooppunt voor **wachtrijen** en **Wachtrij maken** te kiezen in het contextmenu.
4. Voer een naam voor de container en klik op **OK**.   

Standaard is de nieuwe container privé en moet u de toegangstoets opslag voor het BLOB's downloaden van deze container. Als u wilt dat de bestanden in de container om openbaar te maken, selecteert u de container in **Server Explorer** en druk op `F4` om het venster **Eigenschappen** weer te geven. Stel de **openbare leestoegang** op de **Blob**. Iedereen op Internet BLOB's in een openbare container kunt zien, maar u kunt wijzigen of ze alleen verwijderen als u de juiste sleutel hebt.


[Image1]: ./media/vs-create-blob-container-in-server-explorer/vs-storage-create-blob-containers-in-Server-Explorer.png