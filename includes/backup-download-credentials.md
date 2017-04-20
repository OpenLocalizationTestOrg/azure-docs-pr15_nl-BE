## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Met behulp van kluis referenties voor de verificatie van de back-up Azure service

De server op locatie (Windows-client of Windows Server of Data Protection Manager-server) moet worden geverifieerd met een back-up kluis voordat deze kan back-up van gegevens naar Azure. De verificatie wordt bereikt met behulp van 'kluis referenties'. Het concept van de referenties van de kluis is vergelijkbaar met het concept van een "instellingen"-bestand publiceren die wordt gebruikt in Azure PowerShell.

### <a name="what-is-the-vault-credential-file"></a>Wat is de referentie kluis bestand?

Het bestand kluis referenties is een certificaat dat is gegenereerd door de portal voor elke back-vault. De portal uploads van de openbare sleutel vervolgens de Access Control Service (ACS). De persoonlijke sleutel van het certificaat wordt beschikbaar gesteld aan de gebruiker als onderdeel van de workflow die als input in de werkstroom machine registratie wordt gegeven. Hierdoor wordt de machine back-up om gegevens te verzenden naar een geïdentificeerde kluis in de Azure back-service geverifieerd.

De referentie van de kluis wordt alleen gebruikt tijdens de registratie-workflow. Het is de verantwoordelijkheid van de gebruiker om ervoor te zorgen dat het bestand kluis referenties niet meer veilig. Als het valt in de handen van een rogue-gebruiker, kan de kluis referenties-bestand kan worden gebruikt voor het registreren van andere computers tegen de dezelfde kluis. Als u de gegevens van de back-up is gecodeerd met behulp van een wachtwoordzin die bij de klant hoort, kunnen bestaande back-ups echter kan niet worden aangetast. Om te verhelpen dit probleem, kluis referenties ingesteld 48hrs verloopt. Kunt u de referenties van de kluis van een back-up kluis een willekeurig aantal malen – downloaden, maar alleen de meest recente kluis referentie bestand tijdens de workflow van de registratie van toepassing is.

### <a name="download-the-vault-credential-file"></a>Download het bestand kluis referentie

De kluis referentie-bestand wordt gedownload via een beveiligd kanaal vanaf de portal Azure. De back-up Azure-service is niet op de hoogte van de persoonlijke sleutel van het certificaat en de persoonlijke sleutel wordt niet in de portal of de service. Gebruik de volgende stappen de kluis referentie-bestand te downloaden naar een lokale computer.

1.  Aanmelden bij de [Portal beheren](https://manage.windowsazure.com/)
2.  Klik op de **Recovery Services** in het linkernavigatievenster en selecteert u de back-kluis die u hebt gemaakt. Klik op het pictogram van de wolk naar de weergave snel starten van de back-kluis.

    ![Snel weergeven](./media/backup-download-credentials/quickview.png)

3.  Klik op de pagina Quick Start **Download kluis referenties**. De portal genereert de kluis referentie-bestand, dat wordt beschikbaar gesteld voor download.

    ![Downloaden](./media/backup-download-credentials/downloadvc.png)

4.  De portal, de referentie van een kluis met een combinatie van de naam van de kluis en de huidige datum wordt gegenereerd. Klik op **Opslaan** om de referenties van de kluis te downloaden naar de lokale account downloadmap, of selecteer OpslaanAls in het menu opslaan om een locatie voor de referenties van de kluis.

### <a name="note"></a>Opmerking
- Zorg ervoor dat de referenties van de kluis is opgeslagen op een locatie die toegankelijk is vanaf uw computer. Als het is opgeslagen in een bestand delen/SMB, Controleer de toegangsmachtigingen.
- De referenties kluis bestand wordt alleen gebruikt tijdens de registratie-workflow.
- De referenties kluis bestand verloopt na 48hrs en kan worden gedownload vanaf de portal.
- Raadpleeg de Azure back-up [Veelgestelde vragen](../articles/backup/backup-azure-backup-faq.md) voor eventuele vragen over de werkstroom.
