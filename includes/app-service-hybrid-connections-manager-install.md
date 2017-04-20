
1. De blade **hybride verbindingen** de hybride-verbinding die u zojuist hebt gemaakt, klik op **Setup Listener**.
    
    ![Klik op Setup Listener](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
    
4. Hiermee opent u de **verbindingseigenschappen hybride** blade. Onder **On-premises hybride Verbindingsbeheer** **downloaden**en handmatig configureren, sla het gedownloade pakket met HybridConnectionManager.msi en de verbindingsreeks gateway kopiëren.
    
    ![Klik hier om te installeren](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
    
5. Typ de volgende opdracht om het installatieprogramma te starten vanaf een opdrachtprompt beheerder:

        start HybridConnectionManager.msi
 
7. Nadat het installatieprogramma wordt uitgevoerd, klik op **niet nu**, en vervolgens bladert u naar de map %ProgramFiles%\Microsoft\HybridConnectionManager HCMConfigWizard.exe uitvoeren en klikt u op **Ja** in het dialoogvenster **Gebruikersaccountbeheer** .
        
7. De verbindingsreeks hybride die u eerder hebt gekopieerd te plakken en klik op **OK**. 
    
    ![Installeren](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
    
8. Wanneer de installatie is voltooid, klikt u op **sluiten**.
    
    ![Klik op sluiten](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
    
    De kolom **Status** wordt nu **verbonden**op het blad **hybride verbindingen** . 
    
    ![Status verbonden](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)