<properties
    pageTitle="Azure Active Directory Domain Services: DNS op beheerde domeinen beheren | Microsoft Azure"
    description="DNS in beheerde Azure Active Directory Domain Services-domeinen beheren"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>DNS in een beheerde Azure AD Domain Services-domein beheren
Azure Active Directory Domain Services bevat een DNS (Domain Name resolutie) server van de DNS-omzetting voor het domein beheerd. Soms wilt u DNS configureren in het domein beheerd. Wellicht moet u DNS-records voor computers die geen lid zijn van het domein te maken, het configureren van virtuele IP-adressen voor netwerktaakverdeling of externe DNS-doorstuurservers instellen. Daarom krijgen gebruikers die tot de groep 'Beheerders AAD DC behoren' bevoegdheden voor beheerders in beheerde domein DNS.


## <a name="before-you-begin"></a>Voordat u begint
Voor het uitvoeren van de taken die in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldig **abonnement Azure**.

2. Een **directory Azure AD** - hetzij gesynchroniseerd met een op locatie of map alleen-wolk.

3. **Azure AD Domain Services** moet zijn ingeschakeld voor de map Azure AD. Als u dit nog niet hebt gedaan, voert u alle taken die worden beschreven in de [handleiding aan de slag](./active-directory-ds-getting-started.md).

4. Een **virtuele machine deel uitmaakt van het domein** van waaruit u de beheerde Azure AD Domain Services-domein beheren. Als er geen dergelijke een virtuele machine, voert u alle taken die worden beschreven in het artikel [deelnemen aan een virtuele Windows-computer naar een beheerde domein](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Moet u de referenties van een **gebruikersaccount die deel uitmaken van de groep ' beheerders AAD DC'** in de map voor het beheren van DNS voor het domein beheerd.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Taak 1 - voorziening een domein behoren virtuele machine op afstand beheren van DNS voor het domein beheerd
Azure AD beheerde domeinen kunnen extern worden beheerd via vertrouwde Active Directory-beheerprogramma's zoals de Active Directory Administrative Center (ADAC) of AD PowerShell. DNS voor het domein beheerd kan op dezelfde manier worden beheerd op afstand met behulp van de DNS-Server-beheerprogramma's.

Beheerders in uw directory Azure AD beschikt niet over bevoegdheden op domeincontrollers in het domein beheerd via Extern bureaublad verbinding te maken. Leden van de groep 'Beheerders AAD DC' kunnen beheren van DNS voor beheerde domeinen op afstand met behulp van hulpprogramma's voor DNS-Server vanaf een Windows Server/client-computer die is verbonden met het domein beheerd. Hulpprogramma's voor DNS-Server kunnen worden geïnstalleerd als onderdeel van de optionele functie Remote Server Administration Tools (RSAT) voor Windows Server en clientcomputers die zijn verbonden met het domein beheerd.

De eerste taak is het creëren van een Windows Server virtuele machine die is verbonden met het domein beheerd. Raadpleeg het artikel met de titel [deelnemen aan een Windows Server virtuele machine aan een AD-domein Azure Services beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md)voor instructies.


## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Taak 2 - hulpprogramma's voor DNS-Server installeren op de virtuele machine
Voer de volgende stappen uit om de DNS-beheerprogramma's installeren op de virtuele machine van domein toegevoegd. Zie voor meer informatie over de [installatie en het gebruik van Remote Server Administration Tools](https://technet.microsoft.com/library/hh831501.aspx), Technet.

1. Ga naar het knooppunt van de **virtuele Machines** in de klassieke Azure portal. Selecteer de virtuele machine die u hebt gemaakt in taak 1 en klik op **verbinding maken** in de werkbalk onder in het venster.

    ![Verbinding maken met virtuele Windows-computer](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. De klassieke portal wordt u gevraagd te openen of opslaan van een bestand met de extensie 'RDP-', dat wordt gebruikt om verbinding met de virtuele machine. Wanneer het downloaden is voltooid, klikt u op het bestand.

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

9. Op de pagina **onderdelen** , vouw het knooppunt **Remote Server Administration Tools** en klik vervolgens op Vouw het knooppunt **Beheerprogramma's voor rollen** uit. De functie **Hulpprogramma's voor DNS-Server** selecteren uit de lijst met hulpprogramma's voor functiebeheer.

    ![Pagina functies](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. Klik op de pagina **bevestiging** op **installeren** om de functie van DNS-Server-hulpprogramma's installeren op de virtuele machine. Wanneer de installatie is voltooid, klikt u op **sluiten** om de wizard **functies toevoegen en onderdelen** af te sluiten.

    ![Bevestigingspagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Taak 3 - start de DNS management console voor het beheren van DNS
Nu de functie hulpprogramma's voor DNS-Server is geïnstalleerd op het domein lid virtuele machine, gebruiken we de DNS-hulpprogramma's voor het beheren van DNS voor het domein beheerd.

> [AZURE.NOTE] U moet lid zijn van de groep 'Beheerders AAD DC' DNS op het beheerde domein beheren.

1. Van het startscherm, klik op **Systeembeheer**. Hier ziet u de **DNS-** console op de virtuele machine is geïnstalleerd.

    ![Systeembeheer - DNS-Console](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. Klik op **DNS** om de DNS Management-console te starten.

3. Klik op de optie **de volgende computer**met de titel in het dialoogvenster **verbinding maken met de DNS-Server** en geef de DNS-naam van het beheerde domein (bijvoorbeeld ' contoso100.com').

    ![DNS-Console - verbinding maken met een domein](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. De DNS-Console maakt verbinding met het domein beheerd.

    ![De Console DNS - domein beheren](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. Nu kunt u in de DNS-console DNS-vermeldingen toevoegen voor andere computers binnen het virtuele netwerk waarin u AAD Domain Services hebt ingeschakeld.

> [AZURE.WARNING] Wees voorzichtig bij het beheren van DNS voor het beheerde domein met DNS-beheerprogramma's. Zorg ervoor dat u **niet verwijderen of wijzigen van de ingebouwde DNS-records die worden gebruikt door Services in het domein domein**. Ingebouwde DNS-records zijn domein DNS-records naam serverrecords en andere records voor DC-locatie gebruikt. Als u deze records wijzigt, worden in het virtuele netwerk domeinservices verstoord.


Zie [DNS-hulpprogramma's voor artikel op Technet](https://technet.microsoft.com/library/cc753579.aspx) voor meer informatie over het beheren van DNS.


## <a name="related-content"></a>Verwante inhoud

- [Azure AD Domain Services - handleiding aan de slag](./active-directory-ds-getting-started.md)

- [Lid worden van een Windows Server virtuele machine aan een beheerde Azure AD Domain Services-domein](active-directory-ds-admin-guide-join-windows-vm.md)

- [Een beheerde Azure AD Domain Services-domein beheren](active-directory-ds-admin-guide-administer-domain.md)

- [DNS-beheerprogramma 's](https://technet.microsoft.com/library/cc753579.aspx)
