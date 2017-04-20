<properties
    pageTitle="Azure Active Directory Domain Services: Een VM Windows Server toevoegen aan een beheerde domein | Microsoft Azure"
    description="Lid worden van een Windows Server virtuele machine Azure AD Domain Services"
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

# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Een Windows Server virtuele machine aan een beheerde domein toevoegen

> [AZURE.SELECTOR]
- [Klassieke Azure portal - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

In dit artikel wordt beschreven hoe u deelnemen aan een virtuele machine met Windows Server 2012 R2 naar een beheerde domein Azure AD Domain Services, de klassieke Azure portal gebruiken.


## <a name="step-1-create-the-windows-server-virtual-machine"></a>Stap 1: De Windows Server virtuele machine maken
Volg de aanwijzingen in de zelfstudie voor het [maken van een virtuele machine Windows in de klassieke Azure portal wordt uitgevoerd](../virtual-machines/virtual-machines-windows-classic-tutorial.md) . Het is belangrijk om ervoor te zorgen dat de nieuwe virtuele machine is verbonden met hetzelfde virtuele netwerk waarin u Azure AD Domain Services ingeschakeld. De optie Snelle invoer laten deelnemen aan de virtuele machine met een virtueel netwerk. Daarom moet u de optie uit galerie gebruiken voor het maken van de virtuele machine.

Voer de volgende stappen voor het maken van een virtuele Windows-computer deel uitmaakt van het virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld.

1. In de klassieke Azure portal, in de werkbalk onder in het venster, klikt u op **Nieuw**.

2. Klik onder **berekenen**, **virtuele Machine**Klik op **Uit galerie**.

3. Het eerste scherm kunt u **een afbeelding kiezen** voor uw virtuele machine in de lijst met beschikbare kopieën. Kies de juiste afbeelding.

    ![Selecteer afbeelding](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. Het tweede scherm kunt u een naam, grootte, en administratieve gebruikersnaam en wachtwoord kiezen. Gebruik de tier en grootte is vereist voor het uitvoeren van de app of de werkbelasting. De gebruikersnaam die u hier kiest, is een lokale beheerder op de computer. Geef geen referenties voor een domeingebruikersaccount hier.

    ![Virtuele machine configureren](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)

5. Het derde scherm kunt u bronnen voor networking, storage en beschikbaarheid te configureren. Zorg ervoor dat u het virtuele netwerk waarin u Azure AD Domain Services in de vervolgkeuzelijst **Regio affiniteit/groep/virtueel netwerk** ingeschakeld selecteren. Een **Cloud Service DNS-naam** opgeven voor de virtuele machine.

    ![Selecteer virtueel netwerk voor virtuele machine](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [AZURE.WARNING]
    Zorg ervoor dat u lid worden van de virtuele machine met hetzelfde virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld. Als gevolg hiervan, de virtuele machine Zie het domein en taken uitvoeren, zoals lid worden van het domein. Als u de virtuele machine maken in een ander virtueel netwerk, verbinding maken met virtuele netwerk met het virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld.

6. Het vierde scherm kunt u de VM-Agent installeren en configureren van de beschikbare extensies.

    ![Gedaan](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)

7. Nadat u de virtuele machine hebt gemaakt, worden de klassieke portal voor de nieuwe virtuele machine onder het knooppunt **virtuele Machines** . Zowel de virtuele machine en een cloud-service automatisch gestart en de status wordt weergegeven als **actief**.

    ![Virtuele machine is actief](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Stap 2: Verbinding maken met de Windows Server virtuele machine met behulp van de lokale administrator-account
Nu we verbinding maken met de nieuwe Windows Server virtuele machine, aan het domein toevoegen. Gebruik de lokale administrator-referenties die u hebt opgegeven bij het maken van de virtuele machine, tot stand te brengen.

Voer de volgende stappen uit om verbinding met de virtuele machine.

1. Ga naar het knooppunt van de **virtuele Machines** in de klassieke portal. Selecteer de virtuele machine die u in stap 1 hebt gemaakt en klik op **verbinding maken** in de werkbalk onder in het venster.

    ![Verbinding maken met virtuele Windows-computer](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. De klassieke portal wordt u gevraagd te openen of opslaan van een bestand met de extensie 'RDP-', dat wordt gebruikt om verbinding met de virtuele machine. Klik op het bestand te openen als het downloaden is voltooid.

3. Geef bij de aanmeldingsprompt de **lokale administrator-referenties**die u hebt opgegeven tijdens het maken van de virtuele machine. We hebben bijvoorbeeld 'localhost\mahesh' gebruikt in dit voorbeeld.

Op dit punt moet u zijn aangemeld om de zojuist gemaakte virtuele Windows-computer met lokale Administrator-referenties. De volgende stap is de virtuele machine toevoegen aan het domein.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Stap 3: Join de Windows Server virtuele machine aan de beheerde AAD-DS-domein
Voer de volgende stappen uit om de Windows Server virtuele machine aan het beheerde AAD DS-domein toevoegen.

1. Verbinding maken met de Windows-Server zoals in stap 2. Open **Serverbeheer**in het startscherm.

2. Klik op **Lokale Server** in het linkerdeelvenster van het venster van Serverbeheer.

    ![Starten van Serverbeheer op de virtuele machine](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. Klik op **werkgroep** onder de sectie **-Eigenschappen** . Klik op **wijzigen** als u wilt deelnemen aan het domein in het eigenschappenvenster van **Het dialoogvenster Systeemeigenschappen** .

    ![De pagina eigenschappen van systeem](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

4. Geef de domeinnaam van uw Azure AD Domain Services beheerd domein in het tekstvak van het **domein** en klik op **OK**.

    ![Geef het domein te worden gekoppeld](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

5. U wordt gevraagd uw referenties als lid van het domein op te geven. Ervoor zorgen dat u **geeft u de referenties voor een gebruiker die deel uitmaken van de beheerders van de DC AAD** groep. Alleen leden van deze groep hebben bevoegdheden machines toevoegen aan de beheerde domein.

    ![Geef de referenties voor aan domein toevoegen](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

6. U kunt referenties opgeven in een van de volgende manieren:

    - UPN-indeling: het UPN-achtervoegsel voor de gebruikersaccount opgeven zoals geconfigureerd in Azure AD. In dit voorbeeld wordt het UPN-achtervoegsel van de gebruiker 'bob' is 'bob@domainservicespreview.onmicrosoft.com'.

    - Indeling SAMAccountName: kunt u de accountnaam in de notatie SAMAccountName. In dit voorbeeld moet de gebruiker 'bob' invoeren 'CONTOSO100\bob'.

        > [AZURE.NOTE] **Wij raden u aan met behulp van de UPN-indeling kunt u referenties opgeven.** De SAMAccountName kan worden automatisch gegenereerd als een gebruiker het UPN-voorvoegsel is veel te lang (bijvoorbeeld 'joereallylongnameuser'). Als meerdere gebruikers hetzelfde UPN-voorvoegsel (bijvoorbeeld "bob") in uw huurder Azure AD, kan hun indeling SAMAccountName worden automatisch gegenereerd door de service. In deze gevallen kan de UPN-indeling op betrouwbare wijze worden gebruikt voor aanmelding bij het domein.

7. Als aan domein toevoegen voltooid is, ziet u de volgende welkomstbericht aan het domein. Start de virtuele machine voor het domein join-bewerking te voltooien.

    ![Welkom bij het domein](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Het oplossen van problemen aan domein toevoegen
### <a name="connectivity-issues"></a>Verbindingsproblemen
Als de virtuele machine niet het domein te vinden is, raadpleegt u de volgende stappen:

- Zorg ervoor dat de virtuele machine is verbonden met hetzelfde virtuele netwerk als u Services in domein hebt ingeschakeld. Zo niet, de virtuele machine kan geen verbinding maken met het domein en is dus geen lid van het domein.

- Als de virtuele machine is verbonden met een ander virtueel netwerk, moet u ervoor zorgen dat dit virtuele netwerk is verbonden met het virtuele netwerk waarin u Domain Services hebt ingeschakeld.

- Probeer te pingen van het domein met de naam van het domein van de beheerde domein (bijvoorbeeld: ping contoso100.com). Als het niet lukt om dit te doen, probeert u te pingen van de IP-adressen voor het domein op de pagina weergegeven waarop u AD-Domain Azure Services ingeschakeld (bijvoorbeeld ' ping adres 10.0.0.4'). Als u kunt pingen, maar niet het domein het IP-adres, DNS mogelijk onjuist geconfigureerd. Mogelijk hebt u de IP-adressen van het domein als DNS-servers voor het virtuele netwerk niet geconfigureerd.

- Probeer op de virtuele machine (' ipconfig/flushdns') de DNS-omzettingscache leegmaken.

Als u het dialoogvenster waarin u wordt gevraagd om referenties voor een lid van het domein, er geen verbindingsproblemen.


### <a name="credentials-related-issues"></a>Problemen met de referenties
Ga naar de volgende stappen uit als u problemen met referenties ondervindt en kan niet deelnemen aan het domein.

- Probeer de UPN-indeling kunt u referenties opgeven. De SAMAccountName voor uw account mogelijk automatisch gegenereerd als er meerdere gebruikers met hetzelfde UPN-voorvoegsel in de huurder zijn of als uw UPN-voorvoegsel veel te lang is. Daarom kan de SAMAccountName indeling voor uw account afwijken van wat u verwacht of gebruiken in uw domein op gebouwen.

- Probeert u de referenties van een gebruikersaccount die tot de groep 'AAD DC Administrators behoort' computers toevoegen aan het beheerde domein.

- Zorg ervoor dat [Wachtwoordsynchronisatie ingeschakeld](active-directory-ds-getting-started-password-sync.md) volgens de stappen in de handleiding aan de slag.

- Zorg ervoor dat u de UPN van de gebruiker zoals ingesteld in Azure advertentie (bijvoorbeeld, 'bob@domainservicespreview.onmicrosoft.com') aan te melden.

- Zorg ervoor dat u lang genoeg Wachtwoordsynchronisatie gewacht hebt voltooid zoals is opgegeven in de handleiding aan de slag.


## <a name="related-content"></a>Verwante inhoud

- [Azure AD Domain Services - handleiding aan de slag](./active-directory-ds-getting-started.md)

- [Een beheerde Azure AD Domain Services-domein beheren](./active-directory-ds-admin-guide-administer-domain.md)
