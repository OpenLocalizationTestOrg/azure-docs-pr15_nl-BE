## <a name="download-install-and-register-the-azure-backup-agent"></a>Downloaden, installeren en registreren van de agent Azure back-up

Nadat de kluis Azure back-up is gemaakt, moet een agent worden geïnstalleerd op elk van uw Windows-computers (Windows-client Windows Server System Center Data Protection Manager-server of Azure back-up Server-machine) waarmee een back-up van gegevens en toepassingen op Azure.

1. Aanmelden bij de [Portal beheren](https://manage.windowsazure.com/)

2. Klik op **Recovery Services**en selecteer vervolgens de back-kluis die u wilt registreren met een server. De pagina Quick Start voor deze back-vault wordt weergegeven.

    ![Snel starten](./media/backup-install-agent/quickstart.png)

3. Klik op de optie **voor Windows Server of System Center Data Protection Manager of Windows-client** onder **Agent downloaden**op de pagina Quick Start. Klik op **Opslaan** om het te kopiëren naar de lokale computer.

    ![Agent opslaan](./media/backup-install-agent/agent.png)

4. Zodra de agent is geïnstalleerd, dubbelklikt u op MARSAgentInstaller.exe om de installatie van de agent Azure back-up starten. Kies de installatiemap en de tijdelijke map is vereist voor de agent. De opgegeven locatie van de cache moet vrije ruimte die ten minste 5% van de back-upgegevens.

5.  Als u een proxyserver gebruikt voor verbinding met het internet, voert u de proxy-server-gegevens in het scherm **configuratie van Proxy** . Als u een geverifieerde proxy gebruikt, voert u de gegevens van de gebruiker en het wachtwoord in dit scherm.

6.  De Azure Backup agent installeert u .NET Framework 4.5 en Windows PowerShell (als deze nog niet beschikbaar is) om de installatie te voltooien.

7.  Zodra de agent is geïnstalleerd, klikt u op de knop **Doorgaan om de registratie** te gaan met de workflow.

    ![Registreren](./media/backup-install-agent/register.png)

8. In het scherm kluis referenties, blader naar en selecteer het bestand kluis referenties eerder zijn gedownload.

    ![Vault-referenties](./media/backup-install-agent/vc.png)

    Het bestand kluis referenties is alleen geldig voor 48 uur (na het downloaden vanaf de portal). Als u een fout in dit scherm (bijvoorbeeld ' kluis referenties bestand wordt aangeboden is verlopen'), aanmelding bij de Azure portal optreden en het kluis referenties opnieuw downloaden.

    Zorg ervoor dat het bestand kluis referenties op een locatie die toegankelijk is voor het setup-programma beschikbaar is. Als u toegang tot gerelateerde fouten de kluis referenties bestand kopiëren naar een tijdelijke locatie op deze computer en probeer het opnieuw.

    Als er een fout in kluis ongeldige referenties (bijvoorbeeld "kluis ongeldige referenties geleverd") het bestand is beschadigd of heeft niet de meest recente referenties aan hebt gekoppeld de recovery-service. Probeer het opnieuw na het downloaden van een nieuwe kluis referentie-bestand vanaf de portal. Deze fout wordt meestal weergegeven als de gebruiker op de optie **Download kluis referentie** in de portal Azure snel achter elkaar klikt. In dit geval alleen het tweede kluis referentie bestand is geldig.

9. In het scherm **codering** , moet u een wachtwoordzin genereren of bieden een wachtwoordzin (minimaal 16 tekens). Moet de wachtwoordzin op een veilige locatie opslaan.

    ![Codering](./media/backup-install-agent/encryption.png)

    > [AZURE.WARNING] Als de wachtwoordzin is kwijtgeraakt of vergeten; Microsoft kan niet helpen bij het herstellen van de back-upgegevens. De gebruiker eigenaar is van de wachtwoordzin codering en Microsoft geen inzicht in de wachtwoordzin die wordt gebruikt door de eindgebruiker. Sla het bestand op een veilige plaats als dit nodig tijdens een herstelbewerking is.

10. Zodra u op de knop **Voltooien** klikt, wordt de computer geregistreerd om de en u bent nu klaar om een reservekopie op Microsoft Azure.

11. Als u zelfstandige Microsoft Azure back-up kunt u de instellingen die zijn opgegeven tijdens de registratie workflow door te klikken op de optie **Eigenschappen wijzigen** in de mmc-modules voor Azure back-up in.

    ![Eigenschappen wijzigen](./media/backup-install-agent/change.png)

    U kunt ook bij het gebruik van Data Protection Manager, kunt u de instellingen die zijn opgegeven tijdens de registratie workflow door te klikken op de optie **configureren** door **Online** onder het tabblad **beheer** .

    ![Azure back-up configureren](./media/backup-install-agent/configure.png)
