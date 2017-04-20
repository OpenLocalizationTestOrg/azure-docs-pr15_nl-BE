<properties
    pageTitle="Azure Active Directory Domain Services: Een beheerde domein beheert | Microsoft Azure"
    description="Beheerde Azure Active Directory Domain Services-domeinen beheren"
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

# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Een beheerde Azure Active Directory Domain Services-domein beheren
In dit artikel wordt beschreven hoe u een beheerde Azure Active Directory (AD) Domain Services-domein beheren.


## <a name="before-you-begin"></a>Voordat u begint
Voor het uitvoeren van de taken die in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldig **abonnement Azure**.

2. Een **directory Azure AD** - hetzij gesynchroniseerd met een op locatie of map alleen-wolk.

3. **Azure AD Domain Services** moet zijn ingeschakeld voor de map Azure AD. Als u dit nog niet hebt gedaan, voert u alle taken die worden beschreven in de [handleiding aan de slag](./active-directory-ds-getting-started.md).

4. Een **virtuele machine deel uitmaakt van het domein** van waaruit u de beheerde Azure AD Domain Services-domein beheren. Als er geen dergelijke een virtuele machine, voert u alle taken die worden beschreven in het artikel [deelnemen aan een virtuele Windows-computer naar een beheerde domein](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Moet u de referenties van een **gebruikersaccount die deel uitmaken van de groep ' beheerders AAD DC'** in de map voor het beheren van uw domein beheerd.

<br>


## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Administratieve taken die u op een beheerde domein uitvoeren kunt
Leden van de groep 'Beheerders AAD DC' krijgen bevoegdheden op het beheerde domein waarmee deze taken uit te voeren, zoals:

- Computers toevoegen aan het beheerde domein.

- De ingebouwde groepsbeleidsobject voor de containers 'AADDC Computers' en 'AADDC Users' in de beheerde domein configureren.

- DNS gebruiken op het beheerde domein beheren.

- Maken en beheren van aangepaste organisatie-eenheden (OU's) op het domein beheerd.

- Geen administratieve toegang krijgen tot de computers aan de beheerde domein toegevoegd.


## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>U niet in een beheerde domein hoeft beheerdersbevoegdheden
Het domein wordt beheerd door Microsoft, met inbegrip van activiteiten zoals patches, monitoring en back-ups maken. Daarom wordt het domein is vergrendeld en u bent niet bevoegd om bepaalde beheertaken uitvoeren in het domein. Er zijn enkele voorbeelden van taken die u niet kunt uitvoeren onder.

- U kunt domeinbeheerder of Ondernemingsadministrator-bevoegdheden voor het beheerde domein worden niet verleend.

- U kunt het schema van het beheerde domein niet uitbreiden.

- U kan geen verbinding maken met domeincontrollers voor het beheerde domein met behulp van extern bureaublad.

- U kan geen domeincontrollers toevoegen aan het domein beheerd.


## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Taak 1 - voorziening een domein behoren Windows Server virtuele machine op afstand beheren van de beheerde domein
Azure AD beheerde domeinen kunnen worden beheerd met behulp van vertrouwde Active Directory-beheerprogramma's zoals de Active Directory Administrative Center (ADAC) of AD PowerShell. Pachtersbeheerders beschikt niet over bevoegdheden op domeincontrollers in het domein beheerd via Extern bureaublad verbinding te maken. Leden van de groep 'Beheerders AAD DC' kunnen daarom beheerde domeinen op afstand met beheerprogramma's van AD vanaf een computer met Windows Server/client die lid is van de beheerde domein beheren. Beheerprogramma's voor AD kunnen worden geïnstalleerd als onderdeel van de optionele functie Remote Server Administration Tools (RSAT) voor Windows Server en clientcomputers die zijn verbonden met het domein beheerd.

De eerste stap is het instellen van een Windows Server virtuele machine die is verbonden met het domein beheerd. Raadpleeg het artikel met de titel [deelnemen aan een Windows Server virtuele machine aan een AD-domein Azure Services beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md)voor instructies.

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Het beheerde domein extern beheren vanaf een clientcomputer (bijvoorbeeld Windows 10)
De instructies in dit artikel een virtuele machine van Windows Server voor het beheer van de AAD-DS domein beheerd. U kunt echter ook gebruik van een Windows-client (bijvoorbeeld Windows 10) virtuele machine om dit te doen.

U kunt [Remote Server Administration Tools (RSAT) installeren](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) op een virtuele machine voor Windows client door de instructies op TechNet.


## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Taak 2 - installatie van Active Directory-beheerprogramma's op de virtuele machine
Voer de volgende stappen uit om de Active Directory-beheerprogramma's installeren op de virtuele machine van domein toegevoegd. Zie Technet voor meer [informatie over het installeren en gebruiken van Remote Server Administration Tools](https://technet.microsoft.com/library/hh831501.aspx).

1. Ga naar het knooppunt van de **virtuele Machines** in de klassieke Azure portal. Selecteer de virtuele machine die u hebt gemaakt in taak 1 en klik op **verbinding maken** in de werkbalk onder in het venster.

    ![Verbinding maken met virtuele Windows-computer](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. De klassieke portal wordt u gevraagd te openen of opslaan van een bestand met de extensie 'RDP-', dat wordt gebruikt om verbinding met de virtuele machine. Klik op het bestand te openen als het downloaden is voltooid.

3. Gebruik bij de aanmeldingsprompt de referenties van een gebruiker uit de groep 'Beheerders AAD DC'. Zo gebruiken we 'bob@domainservicespreview.onmicrosoft.com' in ons geval.

4. Open **Serverbeheer**in het startscherm. Klik op **functies toevoegen en onderdelen** in het centrale gedeelte van het venster van Serverbeheer.

    ![Starten van Serverbeheer op de virtuele machine](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. Klik op **volgende**op de pagina **Voordat u begint** met de **functies toevoegen en de Wizard functies**.

    ![Voordat u begint met pagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Op de pagina **Type installatie** laat de optie **installatie op basis van de rol of functie** is ingeschakeld en klik op **volgende**.

    ![De pagina Type installatie](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. De huidige virtuele machine te selecteren uit de groep van de server op de pagina **Server selecteren** en klik op **volgende**.

    ![Serverpagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. Klik op **volgende**op de pagina **Serverrollen** . We overslaan deze pagina omdat we niet alle functies wilt op de server installeren.

9. Op de pagina **onderdelen** , vouw het knooppunt **Remote Server Administration Tools** en klik vervolgens op Vouw het knooppunt **Beheerprogramma's voor rollen** uit. Selecteer de functie **AD DS en AD LDS-hulpprogramma's** uit de lijst met hulpprogramma's voor functiebeheer.

    ![Pagina functies](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)

10. Klik op **installeren** voor het installeren de advertentie en de functie van AD LDS-tools op de virtuele machine op **de bevestigingspagina** . Wanneer de installatie is voltooid, klikt u op **sluiten** om de wizard **functies toevoegen en onderdelen** af te sluiten.

    ![Bevestigingspagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)


## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Taak 3 - verbinding en het domein beheerd verkennen
Nu dat de AD-beheerprogramma's zijn geïnstalleerd op het domein lid virtuele machine, we deze hulpprogramma's kunt gebruiken om te verkennen en de beheerde domein beheren.

> [AZURE.NOTE] U moet lid zijn van de groep 'Beheerders AAD DC' om de beheerde domein te beheren.

1. Van het startscherm, klik op **Systeembeheer**. Hier ziet u de beheerprogramma's van AD op de virtuele machine is geïnstalleerd.

    ![Beheerprogramma's zijn geïnstalleerd op de server](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Klik op **Active Directory Administrative Center**.

    ![Active Directory Administrative Center](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Als u wilt weten van het domein, klikt u op de naam van het domein in het linkerdeelvenster (bijvoorbeeld ' contoso100.com'). U ziet twee containers, respectievelijk 'AADDC Computers' en 'AADDC Users' genoemd.

    ![ADAC - domein weergeven](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)

4. Klik op de container **Gebruikers AADDC** om te zien alle gebruikers en groepen die deel uitmaken van het beheerde domein genoemd. Ziet u gebruikersaccounts en groepen uit uw Azure advertentie weergeven van pachters in deze container. Aankondiging in het volgende voorbeeld, een account voor de gebruiker met de naam 'bob' en 'Beheerders AAD DC' de groep zijn in deze container beschikbaar.

    ![ADAC - domeingebruikers](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)

5. Klik op de container **Computers AADDC** overzicht van de computers die deel uitmaken van dit beheerde domein genoemd. Hier ziet u een vermelding voor de huidige virtuele machine die is gekoppeld aan het domein. Computeraccounts voor alle computers die lid zijn van de beheerde Azure AD Domain Services-domein worden opgeslagen in deze container 'AADDC Computers'.

    ![ADAC - domein verbonden computers](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Verwante inhoud

- [Azure AD Domain Services - handleiding aan de slag](./active-directory-ds-getting-started.md)

- [Lid worden van een Windows Server virtuele machine aan een beheerde Azure AD Domain Services-domein](active-directory-ds-admin-guide-join-windows-vm.md)

- [Implementeren van Remote Server Administration Tools](https://technet.microsoft.com/library/hh831501.aspx)
