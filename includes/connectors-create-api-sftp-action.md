Nu dat u een trigger hebt toegevoegd, wordt de tijd om iets te doen met de gegevens die worden gegenereerd door de trigger interessant. Volg deze stappen om toe te voegen een de **SFTP - uittreksel map** actie. Deze actie wordt de inhoud van een bestand uitgepakt als de omschreven voorwaarden is voldaan. 

Voor het configureren van deze actie, moet u de volgende informatie te verstrekken. U ziet dat het gemakkelijk te gebruiken gegevens die zijn gegenereerd door de trigger als invoer voor een aantal van de eigenschappen voor het nieuwe bestand:

|SFTP - extract folder, eigenschap|Beschrijving|
|---|---|
|Pad naar bron archief|Dit is het pad voor het bestand uitgepakt. U kunt een van de tokens uit een eerdere actie of de SFTP-server om het pad naar het bestand bladeren.|
|Bestemmingspad|Dit is het pad waar u de uitgepakte bestanden wordt geplaatst. U kunt een van de tokens selecteren uit een eerdere actie als het doelpad of SFTP-server bladeren en selecteert u een pad.|
|Overschrijven?|Geeft aan dat als een bestand met dezelfde naam als het uitgepakte bestand wordt gevonden in het doelpad map als het bestaande bestand moet worden overschreven of niet.|

De slag Pak de bestanden als de eerder omschreven voorwaarde in *waar resulteert*de actie toe te voegen. 

1. Selecteer **een actie toevoegen**.        
![SFTP actie voorwaarde afbeelding 6](./media/connectors-create-api-sftp/condition-6.png)   
- Selecteer de actie **SFTP - uittreksel map**      
![SFTP actie voorwaarde afbeelding 7](./media/connectors-create-api-sftp/condition-7.png)   
- Selecteer de **bronpad archiefbestand**              
![SFTP actie voorwaarde afbeelding 9](./media/connectors-create-api-sftp/condition-9.png)   
- Selecteer het **pad naar het bestand** token. Hiermee wordt aangegeven dat u het pad van het bestand dat de trigger gevonden gebruikt als het pad van het bron archief.           
![SFTP actie voorwaarde afbeelding 10](./media/connectors-create-api-sftp/condition-10.png)   
- **Pad naar doel map** selecteren           
![SFTP actie voorwaarde afbeelding 11](./media/connectors-create-api-sftp/condition-11.png)   
- Selecteer het **pad naar het bestand** token. Hiermee wordt aangegeven dat u het pad van het bestand dat de trigger wordt gevonden als het doelpad wilt gebruiken voor de uitgepakte bestanden.   
- *\ExtractedFile* invoeren in het besturingselement voor de **bestemmingspad** . Doe dit direct na het bestand pad token in het besturingselement doel map pad.         
![SFTP actie voorwaarde afbeelding 12](./media/connectors-create-api-sftp/condition-12.png)   
- Voer *de waarde True* in de **overschrijven?* controle om aan te geven dat bestaande bestanden worden overschreven als ze dezelfde naam als de uitgepakte bestanden hebben.      
![SFTP actie voorwaarde afbeelding 13](./media/connectors-create-api-sftp/condition-13.png)   
- Sla de wijzigingen in uw workflow  
