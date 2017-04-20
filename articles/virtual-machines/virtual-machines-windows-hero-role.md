<properties
    pageTitle="IIS installeren op de eerste Windows VM | Microsoft Azure"
    description="Experimenteren met de eerste virtuele Windows-computer installeren van IIS en het openen van poort 80 met de Azure portal."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cynthn"/>

# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>Experimenteren met het installeren van een functie op uw Windows-VM
    
Zodra u uw eerste virtuele machine (VM) bedrijf te hebben, kunt u op verplaatsen naar het installeren van software en services. Voor deze zelfstudie gaan we IIS installeren met Serverbeheer op de Server Windows VM. Vervolgens maken we een Network Security Group (NSG) de Azure portal met poort 80 voor IIS-verkeer te openen. 

Als u uw eerste VM al hebt gemaakt, moet u teruggaan naar [uw eerste virtuele Windows-computer in de Azure portal maken](virtual-machines-windows-hero-tutorial.md) voordat u verdergaat met deze zelfstudie.

## <a name="make-sure-the-vm-is-running"></a>Controleer of dat de VM wordt uitgevoerd.

1. Open de [Azure portal](https://portal.azure.com).
2. Klik in het menu hub op **virtuele Machines**. Selecteer de virtuele machine in de lijst.
3. Als de status is **gestopt (Deallocated)**, klikt u op de knop **Start** op de bladeserver **Essentials** van VM. Als de status **wordt uitgevoerd**, kunt u op verplaatsen naar de volgende stap.

## <a name="connect-to-the-virtual-machine-and-sign-in"></a>Verbinding maken met de virtuele machine en aanmelden

1.  Klik in het menu hub op **virtuele Machines**. Selecteer de virtuele machine in de lijst.

3. Klik op het blad voor de virtuele machine, **verbinding maken**. Dit maakt en downloadt een bestand met Remote Desktop Protocol (RDP-bestand) dat lijkt op een snelkoppeling voor verbinding met uw computer. U kunt het bestand opslaan op uw bureaublad voor gemakkelijke toegang. **Open** dit bestand met uw VM.

    ![Screenshot van Azure portal waarin wordt beschreven hoe u verbinding maakt met uw VM](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. U krijgt een waarschuwing dat de RDP van een onbekende uitgever. Dit is normaal. Klik in het venster met extern bureaublad **verbinding maken** om door te gaan.

    ![Schermafbeelding van een waarschuwing over een onbekende uitgever](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. Typ in het venster Windows-beveiliging de gebruikersnaam en het wachtwoord voor de lokale account die u hebt gemaakt bij het maken van de VM. De gebruikersnaam is ingevoerd als *vmname*& #92; *username*, klik vervolgens op **OK**.

    ![Schermafbeelding van de VM-naam, gebruikersnaam en wachtwoord invoeren](./media/virtual-machines-windows-hero-tutorial/credentials.png)
    
6.  U krijgt een waarschuwing dat het certificaat kan niet worden gecontroleerd. Dit is normaal. Klik op **Ja** als u wilt controleren of de identiteit van de virtuele machine en klaar bent met het aanmelden.

    ![Schermafbeelding van een bericht, maar de identiteit van de VM](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Als u om problemen op wanneer u probeert verbinding te maken, Zie [problemen met extern bureaublad-verbindingen met een Windows Azure virtuele Machine](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="install-iis-on-your-vm"></a>Het installeren van IIS op uw VM

Nu dat u bent aangemeld bij de VM, zullen we een serverfunctie installeren zodat u meer kunt experimenteren.

1. Open **Serverbeheer** als deze nog niet geopend is. Klik in het menu **Start** en klik vervolgens op **Serverbeheer**.
2. Selecteer **Lokale Server** in het linkerdeelvenster van **Serverbeheer**. 
3. Selecteer **beheren**in het menu > **functies toevoegen en onderdelen**.
4. Kies **installatie op basis van de rol of functie**in de functies toevoegen en de Wizard functies op de pagina **Type installatie** en klik op **volgende**.

    ![Schermafbeelding van het tabblad Functies toevoegen en de Wizard functies voor Type installatie](./media/virtual-machines-windows-hero-tutorial/role-wizard.png)

5. De VM uit de groep van de server en klik op **volgende**.
6. Selecteer op de pagina **Serverfuncties** **Web Server (IIS)**.

    ![Schermafbeelding van het tabblad Functies toevoegen en de Wizard functies voor serverfuncties](./media/virtual-machines-windows-hero-tutorial/add-iis.png)

7. Zorg ervoor dat de **beheerprogramma's opnemen** is geselecteerd en klik vervolgens op **Onderdelen toevoegen**in het pop-upmenu over het toevoegen van functies die nodig zijn voor IIS. Als het pop-upvenster sluit, klikt u op **volgende** in de wizard.

    ![Screenshot met pop-bevestigen de rol van IIS toe te voegen](./media/virtual-machines-windows-hero-tutorial/confirm-add-feature.png)

8. Klik op **volgende**op de pagina onderdelen.
9. Klik op **volgende**op de pagina **Functie van Web-Server (IIS)** . 
10. Klik op **volgende**op de pagina **Rolservices** . 
11. Klik op de pagina **bevestiging** op **installeren**. 
12. Wanneer de installatie voltooid is, klikt u op **sluiten** in de wizard.



## <a name="open-port-80"></a>Open poort 80 

Voor uw VM accepteert inkomend verkeer via poort 80 moet, u een binnenkomende regel toevoegen aan de netwerkgroep. 

1. Open de [Azure portal](https://portal.azure.com).
2. Selecteer de VM die u hebt gemaakt in **virtuele machines** .
3. In de instellingen van virtuele machines **Netwerkinterfaces** te selecteren en selecteer vervolgens de bestaande netwerkinterface.

    ![Schermafbeelding van de instelling van de virtuele machine voor de netwerkinterfaces](./media/virtual-machines-windows-hero-tutorial/network-interface.png)

4. Klik op de **beveiligingsgroep netwerk**in **Essentials** voor de netwerkinterface.

    ![Schermafbeelding van de sectie Essentials voor de netwerkinterface](./media/virtual-machines-windows-hero-tutorial/select-nsg.png)

5. In het blad **Essentials** voor de NSG hebt u een bestaande binnenkomende standaardregel voor **standaard-toestaan rdp** kunt u zich aanmelden bij de VM. Voegt u nog een binnenkomende regel voor IIS-verkeer. Klik op **binnenkomende regel**.

    ![Schermafbeelding van de sectie Essentials voor de NSG](./media/virtual-machines-windows-hero-tutorial/inbound.png)

6. **Binnenkomende regels**en klik op **toevoegen**.

    ![Screenshot met de knop een regel toevoegen](./media/virtual-machines-windows-hero-tutorial/add-rule.png)

7. **Binnenkomende regels**en klik op **toevoegen**. Typ **80** in het poortbereik en controleer of **dat toestaan** is geselecteerd. Klik op **OK**als u klaar bent.

    ![Screenshot met de knop een regel toevoegen](./media/virtual-machines-windows-hero-tutorial/port-80.png)
 
Zie voor meer informatie over NSGs, regels voor binnenkomende en uitgaande, [externe toegang toestaan voor de met behulp van de portal voor Azure VM](virtual-machines-windows-nsg-quickstart-portal.md)
 
## <a name="connect-to-the-default-iis-website"></a>Verbinding maken met de standaard IIS-website

1. Klik op de **virtuele machines** in de Azure portal en selecteer uw VM.
2. Kopieer uw **openbare IP-adres**in de blade **Essentials** .

    ![Waar vind ik het openbare IP-adres van uw VM met Screenshot](./media/virtual-machines-windows-hero-tutorial/ipaddress.png)

2. Open een browser en typ in de adresbalk in uw openbare IP-adres als volgt: http://<publicIPaddress> en klik op **Enter** om naar dat adres.
3. Uw browser moet de standaard IIS-webpagina te openen. Ziet er ongeveer zo uitziet:

    ![Schermafdruk waarin wordt weergegeven hoe de standaard IIS-pagina in een browser uitziet](./media/virtual-machines-windows-hero-tutorial/iis-default.png)

    

## <a name="next-steps"></a>Volgende stappen

- U kunt ook experimenteren met het [toevoegen van een schijf met gegevens](virtual-machines-windows-attach-disk-portal.md) aan uw virtuele machine. Gegevensschijven bieden meer opslagruimte voor uw virtuele machine.
