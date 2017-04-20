<properties
    pageTitle="Problemen met SSH-verbinding naar een VM | Microsoft Azure"
    description="Informatie over het oplossen van problemen, zoals 'SSH verbinding mislukt' of 'SSH verbinding geweigerd' voor een Azure VM waarop Linux."
    keywords="SSH verbinding geweigerd, ssh fout, azure ssh, SSH-verbinding is mislukt"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>SSH-verbindingen met een Azure Linux VM die wordt geweigerd of mislukt, fouten, problemen oplossen
Er zijn diverse redenen dat u op problemen Secure Shell (SSH), SSH verbinding mislukt stuit, of SSH wordt geweigerd wanneer u probeert verbinding te maken met een Linux virtuele machine (VM). Dit artikel helpt u bij het vinden en corrigeren van de problemen. Azure portal Azure CLI of uitbreiding van de VM toegang voor Linux kunt u verbindingsproblemen oplossen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Als u meer Help-informatie op elk punt in dit artikel, kunt u contact opnemen met de Azure experts op [de MSDN Azure en forums stackoverloop](http://azure.microsoft.com/support/forums/). U kunt ook een incident Azure ondersteuning indienen. Ga naar de [site ondersteuning van Azure](http://azure.microsoft.com/support/options/) en selecteer **ondersteuning krijgen**. Lees de [Veelgestelde vragen over ondersteuning voor Microsoft Azure](http://azure.microsoft.com/support/faq/)voor meer informatie over het gebruik van Azure ondersteunen.


## <a name="quick-troubleshooting-steps"></a>Snelle stappen voor probleemoplossing
Probeer na elke stap opnieuw verbinding maken met de VM.

1. De SSH-configuratie teruggezet.
2. De referenties van de gebruiker hersteld.
3. Controleer of dat de [Beveiligingsgroep netwerk](../virtual-network/virtual-networks-nsg.md) SSH verkeer toegestaan.
    - Zorg ervoor dat er een regel Network Security Group is SSH verkeer toestaan (standaard TCP-poort 22).
    - Poortomleiding kunt u niet gebruiken / koppelen zonder een Azure taakverdeling.
4. Controleer de [VM resource gezondheid](../resource-health/resource-health-overview.md). 
    - Zorg ervoor dat de VM rapporteert dat gezonde.
    - Hebt u boot diagnostics is ingeschakeld, controleert u of dat de VM niet rapporteert fouten in de logboeken te starten.
5. Start de VM.
6. Implementeer de VM.

Verder lezen voor meer gedetailleerde stappen voor probleemoplossing en toelichtingen.


## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>Beschikbare methoden met SSH-verbinding oplossen

U kunt referenties of SSH-configuratie met behulp van een van de volgende manieren herstellen:

- [Azure portal](#using-the-azure-portal) - ideaal als u moet snel opnieuw de SSH-configuratie of SSH-sleutel en u hoeft niet de Azure's hebben geïnstalleerd.
- [Opdrachten voor azure CLI](#using-the-azure-cli) - opnieuw bent u al op de opdrachtregel snel de SSH-configuratie of referenties.
- [Uitbreiding VMAccessForLinux azure](#using-the-vmaccess-extension) - maken en json-definitiebestanden als de SSH-configuratie of de gebruiker referenties opnieuw wilt gebruiken.

Probeer na elke stap opnieuw verbinding te maken met uw VM. Als u nog steeds geen verbinding kunt maken, probeert u de volgende stap.


## <a name="using-the-azure-portal"></a>Met behulp van de portal Azure
De Azure portal biedt een snelle manier om de SSH-configuratie of de gebruiker referenties opnieuw zonder alle hulpprogramma's installeren op de lokale computer.

Selecteer uw VM in Azure portal. Ga naar de sectie **Support + probleemoplossing** en selecteer **wachtwoord opnieuw instellen** zoals in het volgende voorbeeld:

![SSH-configuratie of de referenties in de portal Azure opnieuw instellen](./media/virtual-machines-linux-troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>De SSH-configuratie herstellen
Als eerste stap selecteert u `Reset SSH configuration only` uit de vervolgkeuzelijst **modus** als in de vorige screenshot klik vervolgens op de knop **Beginwaarden** . Als deze actie is voltooid, probeert u opnieuw toegang krijgen tot uw VM.

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-referenties voor een gebruiker opnieuw instellen
Als u de referenties van een bestaande gebruiker wijzigen, selecteer een van de `Reset SSH public key` of `Reset password` uit de vervolgkeuzelijst **modus** als in de vorige screenshot. Geef de gebruikersnaam en een SSH-sleutel of het nieuwe wachtwoord en klik vervolgens op de knop **Beginwaarden** .

U kunt ook een gebruiker maken met sudo-rechten op de VM vanuit dit menu. Voer een nieuwe gebruikersnaam en het bijbehorende wachtwoord of de SSH-sleutel en klik op de knop **Beginwaarden** .


## <a name="using-the-azure-cli"></a>Met behulp van de CLI Azure
Als u nog niet gedaan, [installeert de CLI Azure en verbinding maken met uw abonnement Azure hebt](../xplat-cli-install.md). Zorg ervoor dat u met modus Resource Manager als volgt:

```
azure config mode arm
```

Als u hebt gemaakt en geüpload een aangepaste installatiekopie van Linux, zorg ervoor dat de [Agent voor Microsoft Azure Linux](virtual-machines-linux-agent-user-guide.md) versie 2.0.5 of hoger is geïnstalleerd. VMs gemaakt met behulp van afbeeldingen, is deze extensie toegang al geïnstalleerd en geconfigureerd voor u.

### <a name="reset-ssh-configuration"></a>SSH-configuratie herstellen
De SSHD configuratie zelf mogelijk niet goed geconfigureerd of de service is een fout opgetreden. U kunt instellen dat SSHD om te controleren of dat de configuratie van het SSH zelf geldig is. SSHD opnieuw in te stellen, moet de eerste stap die u nemen.

Het volgende voorbeeld wordt een SSHD op een VM met de naam `myVM` in de bronnengroep met de naam `myResourceGroup`. Uw eigen groepnamen VM en resource als volgt gebruiken:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-referenties voor een gebruiker opnieuw instellen
Als de SSHD goed wordt weergegeven, kunt u het wachtwoord voor de gebruiker van een afzender herstellen. Het volgende voorbeeld worden de referenties voor `myUsername` op de waarde die is opgegeven in `myPassword`, op de VM met de naam `myVM` in `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Als u SSH key-verificatie gebruikt, kunt u de SSH-sleutel voor een bepaalde gebruiker herstellen. In het volgende voorbeeld werkt de SSH-sleutel opgeslagen in `~/.ssh/azure_id_rsa.pub` voor de gebruiker met de naam `myUsername`, op de VM met de naam `myVM` in `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-file ~/.ssh/azure_id_rsa.pub
```


## <a name="using-the-vmaccess-extension"></a>Bestand met de extensie VMAccess
De uitbreiding van de toegang tot VM voor Linux wordt gelezen in een json-bestand waarin u de acties uit te voeren. Deze acties omvatten SSHD opnieuw in te stellen, opnieuw een SSH-sleutel in te stellen of een gebruiker toe te voegen. U nog steeds met de CLI Azure roept de extensie VMAccess, maar u kunt opnieuw gebruiken json bestanden via meerdere VMs indien gewenst. Deze benadering kunt u een opslagplaats van json-bestanden kunnen vervolgens worden aangeroepen voor bepaalde scenario's maken.

### <a name="reset-sshd"></a>SSHD opnieuw instellen
Maak een bestand met de naam `PrivateConf.json` met de volgende inhoud:

```bash
{  
    "reset_ssh":"True"
}
```

Met behulp van de CLI Azure, u roept u vervolgens de `VMAccessForLinux` -extensie voor de SSHD-verbinding resetten als de json-bestand opgeeft. Het volgende voorbeeld wordt een SSHD op de VM met de naam `myVM` in `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```bash
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-referenties voor een gebruiker opnieuw instellen
Als SSHD goed wordt weergegeven, kunt u de referenties voor een gebruiker voor de afzender kunt herstellen. Om het wachtwoord voor een gebruiker opnieuw instelt, maakt u een bestand met de naam `PrivateConf.json`. Het volgende voorbeeld worden de referenties voor `myUsername` op de waarde die is opgegeven in `myPassword`. Voer de volgende regels in de `PrivateConf.json` -bestand, met behulp van uw eigen waarden:

```bash
{
    "username":"myUsername", "password":"myPassword"
}
```

Of als u de SSH-sleutel voor een gebruiker wijzigen, maakt u eerst een bestand met de naam `PrivateConf.json`. Het volgende voorbeeld worden de referenties voor `myUsername` op de waarde die is opgegeven in `myPassword`, op de VM met de naam `myVM` in `myResourceGroup`. Voer de volgende regels in de `PrivateConf.json` -bestand, met behulp van uw eigen waarden:

```bash
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Nadat u hebt uw json-bestand, met de CLI Azure roept de `VMAccessForLinux` uitbreiding van uw gebruikersreferenties SSH opnieuw door de json-bestand op te geven. Het volgende voorbeeld worden de referenties voor de VM met de naam `myVM` in `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```


## <a name="restart-a-vm"></a>Opnieuw een VM
Als u de SSH-configuratie en de gebruiker referenties opnieuw, of heeft een fout aangetroffen in doet, kunt u proberen de VM aan de onderliggende problemen compute adres opnieuw te starten.

### <a name="azure-portal"></a>Azure portal
Een met behulp van de portal voor Azure VM, selecteer uw VM en klik op het * knop**opnieuw opstarten** het volgende voorbeeld:

![Opnieuw een VM in Azure portal](./media/virtual-machines-linux-troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
In het volgende voorbeeld wordt opnieuw gestart met de naam VM `myVM` in de bronnengroep met de naam `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```bash
azure vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Implementeer een VM
U kunt opnieuw een VM naar een ander knooppunt in Azure, die alle onderliggende netwerkproblemen kan corrigeren. Zie [virtuele machine naar nieuwe Azure knooppunt opnieuw implementeren](virtual-machines-windows-redeploy-to-new-node.md)voor meer informatie over het opnieuw distribueren van een VM.

> [AZURE.NOTE] Wanneer deze bewerking is voltooid, is tijdelijke schijfgegevens verloren en dynamische IP-adressen die gekoppeld aan de virtuele machine zijn wordt bijgewerkt.

### <a name="azure-portal"></a>Azure portal
Selecteer uw VM en blader naar de sectie **Support + probleemoplossing** te implementeren in een VM met behulp van de portal Azure. Klik op de knop **implementeren** zoals in het volgende voorbeeld:

![Implementeer een VM in Azure portal](./media/virtual-machines-linux-troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
In het volgende voorbeeld de VM met de naam redeploys `myVM` in de bronnengroep met de naam `myResourceGroup`. Gebruik uw eigen waarden als volgt:

```bash
azure vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>Gemaakt met behulp van het model Klassiek implementatie VMs

Probeer de volgende stappen u kunt oplossen door de meest voorkomende storingen van de SSH-verbinding voor VMs die zijn gemaakt met behulp van het implementatiemodel klassiek. Na elke stap opnieuw verbinding te maken met de VM.

- Externe toegang via de [portal Azure](https://portal.azure.com)opnieuw. Selecteer uw VM in Azure portal, en klik op de knop **Beginwaarden externe...** .

- Start de VM. Selecteer uw VM in [Azure portal](https://portal.azure.com)en klik op de knop **opnieuw opstarten** .

    -OF-

    Selecteer op de [Azure klassieke portal](https://manage.windowsazure.com) **virtuele machines** > **exemplaren** > **opnieuw**.

- Implementeren naar een nieuw knooppunt Azure VM. Zie [virtuele machine naar nieuwe Azure knooppunt opnieuw implementeren](virtual-machines-windows-redeploy-to-new-node.md)voor meer informatie over het implementeren van een VM.

    Wanneer deze bewerking is voltooid, is tijdelijke schijfgegevens verloren en dynamische IP-adressen die gekoppeld aan de virtuele machine zijn wordt bijgewerkt.

- Volg de instructies in [het opnieuw instellen van een wachtwoord of SSH voor Linux-gebaseerde virtuele machines](virtual-machines-linux-classic-reset-access.md) :
    - De SSH-sleutel of het wachtwoord opnieuw.
    - Maak een gebruikersaccount _sudo_ .
    - De SSH-configuratie teruggezet.

- Controleren van de VM resource of er problemen zijn platform.<br>
  Selecteer uw VM en blader omlaag **Instellingen** > **Gezondheid controleren**.


## <a name="additional-resources"></a>Aanvullende bronnen

- Als u nog steeds geen SSH voor uw VM na het volgen van de stappen na, Zie [meer gedetailleerde stappen voor probleemoplossing](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md) te bekijken van aanvullende stappen om uw probleem te verhelpen.

- Zie [problemen met toegang tot een toepassing wordt uitgevoerd op een Azure virtuele machine](virtual-machines-linux-troubleshoot-app-connection.md) voor meer informatie over het oplossen van toegang tot toepassingen

- Zie voor meer informatie over het oplossen van virtuele machines die zijn gemaakt met behulp van het implementatiemodel klassiek, [het opnieuw instellen van een wachtwoord of SSH voor Linux-gebaseerde virtuele machines](virtual-machines-linux-classic-reset-access.md).
