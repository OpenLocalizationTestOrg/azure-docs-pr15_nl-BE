<!--author=SharS last changed: 01/15/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>Update 1.2 installeren vanaf de klassieke Azure portal

1. Selecteer het apparaat op de pagina StorSimple service. Ga naar **apparaten** > **onderhoud**.

2. Klik onderaan de pagina op **Updates scannen**. Een taak wordt gemaakt om te zoeken naar beschikbare updates. U krijgt wanneer de taak is voltooid.

3. In de sectie **Software-Updates** op dezelfde pagina ziet u dat er nieuwe software-updates beschikbaar zijn. Wij raden u de release-opmerkingen voordat u Update 1.2 op uw apparaat toepast.

    ![Software-updates installeren](./media/storsimple-install-update-via-portal/InstallUpdate12_11M.png)

4. Klik onderaan de pagina **Updates installeren**.

5. U wordt om bevestiging gevraagd. Klik op **OK**.

6. Het dialoogvenster **Updates installeren** wordt weergegeven. Het apparaat moet voldoen aan de controles die in dit dialoogvenster weergegeven. Deze stappen zijn uitgevoerd voordat de update. Selecteer **ik de bovenstaande eis begrijpen en ben klaar voor het bijwerken van het apparaat**. Klik op het pictogram.

    ![Bevestigingsbericht](./media/storsimple-install-update-via-portal/InstallUpdate12_2M.png)

7. Een reeks automatische voorafgaande controles wordt nu gestart. Deze omvatten:

    - **Controller te controleren** om te controleren of de apparaatcontrollers gezonde en on line zijn.
    
    - **Hardware-onderdeel te controleren** om te controleren of alle hardwareonderdelen op uw apparaat StorSimple in orde zijn.
    
    - **DATA 0 wordt gecontroleerd** om te controleren of gegevens 0 op het apparaat is ingeschakeld. Als deze interface niet is ingeschakeld, moet u deze inschakelen en probeer het opnieuw.
    
    - **Gegevens 2 en 3 van de gegevens wordt gecontroleerd** om te controleren of netwerkinterfaces gegevens 2 en 3 van de gegevens zijn niet ingeschakeld. Als deze interfaces zijn ingeschakeld, moet u uit te schakelen en probeer vervolgens het apparaat bijwerken. Deze controle wordt alleen uitgevoerd als u bijwerkt vanaf een apparaat met NH-software. Apparaten met een versie van 0,1 0,2 of 0,3 nodig deze controle niet.
    
    - **Gateway controleren** op een apparaat waarop een eerdere versie dan 1 Update uitgevoerd. Deze controle wordt uitgevoerd op alle apparaat 1 vóór update-software wordt uitgevoerd, maar niet op de apparaten die geconfigureerd voor een netwerkinterface dan gegevens 0 gateway zijn.
 
    Update 1.2 wordt alleen toegepast als de bovenstaande vóór update controles met succes zijn voltooid. U krijgt vooraf controles worden uitgevoerd.
  
    ![Kennisgeving vooraf controleren](./media/storsimple-install-update-via-portal/InstallUpdate12_3M.png)

    Hier volgt een voorbeeld waarin de upgrade is mislukt. U moet controleren of de apparaatcontrollers gezonde en on line zijn. U moet ook controleren van de status van de hardware-onderdelen. In dit voorbeeld moeten Controller 0 en 1 Controller onderdelen aandacht besteden. Wellicht moet u contact opnemen met Microsoft Support als u niet kunt deze door uzelf belemmeringen.

     ![Voorafgaande controle is mislukt](./media/storsimple-install-update-via-portal/HCS_PreUpgradeChecksFailed-include.png)

    > [AZURE.NOTE] Nadat u Update 1.2 hebt toegepast op uw apparaat StorSimple, langer gegevens 2 en 3 van de gegevens controleert en de controle van de gateway niet nodig zijn voor de toekomstige updates. De voorafgaande controles nog steeds vereist.


8. Nadat de upgrade controles zijn voltooid, wordt een bijwerktaak worden gemaakt. U krijgt wanneer u de taak update met succes is gemaakt.
 
    ![Het maken van taken bijwerken](./media/storsimple-install-update-via-portal/InstallUpdate12_44M.png)

    De update zal vervolgens worden toegepast op het apparaat.
 
9. Klik op **Taak weergeven**om de voortgang van de bijwerktaak. Klik op de pagina **taken** ziet u de voortgang van de update. 

    ![Voortgang van het project bijwerken](./media/storsimple-install-update-via-portal/InstallUpdate12_5M.png)

10. De update zal een paar uur in beslag nemen. U kunt de details van de taak op elk gewenst moment bekijken.

    ![Taakdetails van een bijwerken](./media/storsimple-install-update-via-portal/InstallUpdate12_6M.png)

11. Nadat de taak voltooid is, gaat u naar de pagina **onderhoud** en Ga naar **Software-Updates**.

12. Controleer of het apparaat is **StorSimple 8000 serie Update 1.2 (6.3.9600.17584)**. De **datum voor het laatst bijgewerkt** moet ook worden gewijzigd.

    ![Onderhoudspagina](./media/storsimple-install-update-via-portal/InstallUpdate12_10M.png)

13. U ziet nu dat onderhoud modus updates beschikbaar zijn. Deze updates zijn storend updates die leiden tot uitvaltijd apparaat en kunnen alleen worden toegepast via de Windows PowerShell-interface van uw apparaat. Volg de instructies in het [onderhoud modus updates installeren](storsimple-update-device.md#install-maintenance-mode-updates-via-windows-powershell-for-storsimple) voor het installeren van deze updates via de Windows PowerShell voor StorSimple.

> [AZURE.NOTE] Het bericht onderhoud modus updates beschikbaar zijn kan tot 24 uur nadat de onderhoud modus updates zijn toegepast op het apparaat worden weergegeven in bepaalde gevallen wordt.  


