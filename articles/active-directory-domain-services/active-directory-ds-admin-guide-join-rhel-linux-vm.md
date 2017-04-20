<properties
    pageTitle="Azure Active Directory Domain Services: Een VM RHEL toevoegen aan een beheerde domein | Microsoft Azure"
    description="Lid worden van een virtuele machine van Red Hat Enterprise Linux Azure AD Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Een Red Hat Enterprise Linux 7 virtuele machine aan een beheerde domein toevoegen
In dit artikel wordt beschreven hoe u een virtuele machine van Red Hat Enterprise Linux (RHEL) 7 toevoegen aan een beheerde Azure AD Domain Services-domein.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Inrichten van een virtuele machine van Red Hat Enterprise Linux
Voer de volgende stappen uit om een RHEL 7 virtuele machine met de Azure portal inrichten.

1. Log in om de [Azure portal](https://portal.azure.com).

    ![Azure portal dashboard](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. Klik op **Nieuw** in het linkerdeelvenster en **Red Hat** in de zoekbalk typt, zoals in de volgende schermafdruk. Vermeldingen voor Red Hat Enterprise Linux worden weergegeven in de zoekresultaten. Klik op **Red Hat Enterprise Linux 7.2**.

    ![Selecteer RHEL in resultaten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. De zoekresultaten in het deelvenster **Alles** moeten de Red Hat Enterprise Linux 7.2 afbeelding weergeven. Klik op **Red Hat Enterprise Linux 7.2** voor meer informatie over de afbeelding van de virtuele machine.

    ![Selecteer RHEL in resultaten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. In het deelvenster met **Red Hat Enterprise Linux 7.2** ziet u meer informatie over de afbeelding van de virtuele machine. In de vervolgkeuzelijst **deployment model selecteren** , selecteer **Classic**. Klik vervolgens op de knop **maken** .

    ![Details van de afbeelding weergeven](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. Voer in het deelvenster **VM maken** de **Hostnaam** voor de nieuwe virtuele machine. Ook een lokale beheerder gebruikersnaam opgeven in het veld **gebruikersnaam** en een **wachtwoord**. U kunt ook een SSH-sleutel gebruikt voor de verificatie van de gebruiker van de lokale beheerder. Selecteer ook een **Reeks prijzen** voor de virtuele machine.

    ![VM - basisgegevens maken](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. Klik op **optionele configuratie**. Klik op **netwerk**in het deelvenster met **optionele config** .

    ![VM maken - virtueel netwerk configureren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. Verschijnt een venster met de titel **netwerk**. Klik in het venster **netwerk** **Virtueel netwerk** , schakelt u het virtuele netwerk waarmee de Linux VM moet worden geïmplementeerd. Hiermee opent u het deelvenster **Virtueel netwerk** . Kies de optie **een bestaande virtuele netwerk gebruiken** in het deelvenster **Virtueel netwerk** . Selecteer vervolgens het virtuele netwerk waarin Azure AD Domain Services beschikbaar is. In dit voorbeeld kiezen we het virtuele netwerk 'MyPreviewVNet'.

    ![Maken van VM - virtueel netwerk selecteren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. Klik op de knop **OK** op het venster **optioneel config** .

    ![VM - virtueel netwerk geselecteerd maken](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. U bent nu gereed voor het maken van de virtuele machine. Klik op de knop **maken** in het deelvenster **VM maken** .

    ![VM - gereed maken](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. Implementatie van de nieuwe virtuele machine op basis van de RHEL 7.2 image te starten.

  ![VM - implementatie gestart maken](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. Na een paar minuten moet de virtuele machine worden geïmplementeerd is en klaar voor gebruik.

  ![VM - geïmplementeerd maken](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Extern verbinding maken met de nieuw ingerichte Linux virtuele machine
De virtuele machine van RHEL 7.2 is in Azure ingericht. De volgende taak is extern verbinding maken met de virtuele machine.

**Verbinding maken met de virtuele machine van RHEL 7.2** Volg de instructies in het artikel [hoe aan te melden op een virtuele machine waarop Linux wordt uitgevoerd](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

De rest van de stappen wordt aangenomen dat u de PuTTY SSH-client verbinding maken met de virtuele machine van RHEL. Voor meer informatie, Zie de [pagina voor het downloaden van stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Open het programma stopverf.

2. Voer de **Hostnaam** voor de zojuist gemaakte RHEL virtuele machine. In dit voorbeeld is onze virtuele machine de host-naam 'contoso rhel.cloudapp .net'. Als u niet zeker van de host-naam van uw VM bent, raadpleegt u het dashboard VM in Azure portal.

    ![Stopverf verbinding](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. Meld u op de virtuele machine met de lokale administrator-referenties opgegeven waarop de virtuele machine is gemaakt. In dit voorbeeld hebben we de lokale administrator-account 'mahesh' gebruikt.

    ![Stopverf aanmelding](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Vereiste pakketten te installeren op de virtuele machine van Linux
Na het maken van een verbinding met de virtuele machine, wordt de volgende taak pakketten vereist voor domeinlidmaatschap op de virtuele machine installeren. Voer de volgende stappen uit:

1. **Realmd installeren:** Het pakket realmd wordt gebruikt voor domeinlidmaatschap. Typ de volgende opdracht in de stopverf terminal:

    sudo yum installeren realmd

    ![Realmd installeren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Na een paar minuten moet het realmd-pakket geïnstalleerd op de virtuele machine.

    ![realmd geïnstalleerd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **Sssd installeren:** Het pakket realmd is afhankelijk van de sssd domein join-bewerkingen uit te voeren. Typ de volgende opdracht in de stopverf terminal:

    sudo yum install sssd

    ![Sssd installeren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Na een paar minuten moet de sssd-pakket geïnstalleerd op de virtuele machine.

    ![realmd geïnstalleerd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **Installatie van kerberos:** Typ de volgende opdracht in de stopverf terminal:

    sudo yum install krb5 workstation krb5-libs

    ![Kerberos installeren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Na een paar minuten moet het realmd-pakket geïnstalleerd op de virtuele machine.

    ![Kerberos geïnstalleerd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>De Linux virtuele machine aan het beheerde domein toevoegen
Nu de vereiste pakketten worden geïnstalleerd op de virtuele machine van Linux, de volgende taak is de virtuele machine toevoegen aan de beheerde domein.

1. Ontdek het beheerde AAD Domain Services-domein. Typ de volgende opdracht in de stopverf terminal:

    sudo realm CONTOSO100.COM ontdekken

    ![Ontdek realm](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Als **realm ontdekken** kan uw beheerde domein te vinden is, ervoor te zorgen dat het domein van de virtuele machine (probeer ping) bereikbaar is. Ook voor zorgen dat de virtuele machine is inderdaad geïmplementeerd met hetzelfde virtuele netwerk waarin de beheerde domein beschikbaar is.

2. Kerberos initialiseren. Typ de volgende opdracht in de stopverf terminal. Zorg ervoor dat u een gebruiker die deel uitmaakt van de groep 'Beheerders AAD DC' opgeven. Alleen deze gebruikers kunnen computers aan de beheerde domein toevoegen.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Zorg ervoor dat u de domeinnaam opgeven in hoofdletters, anders kinit mislukt.

3. De computer toevoegen aan het domein. Typ de volgende opdracht in de stopverf terminal. De gebruiker die u hebt opgegeven in de vorige stap (kinit) opgeven.

    sudo realm join--verbose CONTOSO100.COM -U'bob@CONTOSO100.COM'

    ![Realm-join](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Krijgt u een bericht ("met succes ingeschreven machine in realm') wanneer de machine is geworden van het domein beheerd.


## <a name="verify-domain-join"></a>Controleer of aan domein toevoegen
U kunt snel controleren of de computer is toegevoegd aan het domein beheerd. Verbinding maken met het domein zijn toegevoegd RHEL VM SSH en een gebruikersaccount voor domein en vervolgens gebruiken om te controleren of de gebruikersaccount juist is opgelost.

1. In de stopverf terminal, typ de volgende opdracht om verbinding met het domein zijn toegevoegd RHEL virtuele machine met behulp van SSH. Gebruik een domeinaccount die deel uitmaakt van de beheerde domein (bijvoorbeeld 'bob@CONTOSO100.COM' in dit geval.)

    SSH -l bob@CONTOSO100.COM contoso rhel.cloudapp.net

2. Typ de volgende opdracht om te zien als de basismap correct is geïnitialiseerd in de stopverf terminal.

    pwd

3. Typ de volgende opdracht om te zien als de groepslidmaatschappen worden correct wordt omgezet in de stopverf terminal.

    ID

Er volgt een voorbeeld van uitvoer van deze opdrachten:

![Controleer of aan domein toevoegen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## <a name="troubleshooting-domain-join"></a>Het oplossen van problemen aan domein toevoegen
Raadpleeg het artikel [Troubleshooting domeinlidmaatschap](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) .


## <a name="related-content"></a>Verwante inhoud
- [Azure AD Domain Services - handleiding aan de slag](./active-directory-ds-getting-started.md)

- [Lid worden van een Windows Server virtuele machine aan een beheerde Azure AD Domain Services-domein](active-directory-ds-admin-guide-join-windows-vm.md)

- [Hoe aan te melden op een virtuele machine waarop Linux wordt uitgevoerd](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

- [Installatie van Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)

- [Red Hat Enterprise Linux 7 - handleiding voor Windows-integratie](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
