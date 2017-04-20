We gaan nu een trigger toevoegen.

1. *Sftp* invoeren in het zoekvak op de logica apps designer en vervolgens selecteert u de trigger **SFTP - wanneer een bestand wordt toegevoegd of gewijzigd**   
![SFTP triggerafbeelding 1](./media/connectors-create-api-sftp/trigger-1.png)  
- Het besturingselement **wanneer een bestand wordt toegevoegd of gewijzigd** wordt geopend  
![SFTP triggerafbeelding 2](./media/connectors-create-api-sftp/trigger-2.png)  
- Selecteer de **...** zich aan de rechterkant van het besturingselement. Hiermee opent u de map het besturingselement Datumkiezer  
![SFTP triggerafbeelding 3](./media/connectors-create-api-sftp/action-1.png)  
- Selecteer de **SFTP** de hoofdmap als de map om te controleren op nieuwe of gewijzigde bestanden selecteren. U ziet dat de hoofdmap wordt nu weergegeven in de **map** beheer.  
![SFTP triggerafbeelding 4](./media/connectors-create-api-sftp/action-2.png)   

Op dit moment is uw app logica geconfigureerd met een trigger die een uitvoering van de andere triggers en acties in de werkstroom wordt gestart wanneer een bestand wordt gewijzigd of in de specifieke map met SFTP gemaakt. 

>[AZURE.NOTE]Voor een logica app wilt gebruiken, moet ten minste één trigger en één actie bevatten. Volg de stappen in de volgende sectie voor het toevoegen van een actie.  