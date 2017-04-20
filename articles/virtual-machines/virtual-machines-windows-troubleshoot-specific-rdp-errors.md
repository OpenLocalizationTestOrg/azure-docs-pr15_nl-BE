<properties
    pageTitle="Specifieke foutberichten RDP voor Azure VMs | Microsoft Azure"
    description="Specifieke foutberichten die kunnen worden weergegeven wanneer u probeert om een virtuele Windows-computer verbinding met extern bureaublad gebruiken in Azure begrijpen"
    keywords="Extern bureaublad fout, fout, verbinding met extern bureaublad, kan geen verbinding met de VM, probleemoplossing extern bureaublad"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/14/2016"
    ms.author="iainfou"/>

# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Voor een Windows VM in Azure specifieke RDP-foutberichten oplossen
Er wordt een specifiek foutbericht wanneer u verbinding met extern bureaublad met een Windows virtuele machine (VM) in Azure. In dit artikel vindt u details van de meest voorkomende foutberichten tegenkomt, samen met de stappen voor het oplossen van deze problemen. Als u verbinding maakt met uw VM met RDP problemen ondervindt maar niet optreedt ten gevolge van een specifiek foutbericht, Zie de [handleiding voor extern bureaublad oplossen](virtual-machines-windows-troubleshoot-rdp-connection.md).

Zie de volgende onderwerpen voor meer informatie over bepaalde foutberichten:

- [De externe sessie is beëindigd omdat er geen extern bureaublad-licentieservers beschikbaar een licentie kunnen verlenen](#rdplicense).
- [Extern bureaublad kan de computer 'naam' niet vinden](#rdpname).
- [Een verificatie fout. De Local Security Authority kan geen contact maken met](#rdpauth).
- [Windows-beveiliging-fout: uw referenties werken niet](#wincred).
- [Deze computer kan geen verbinding maken met de externe computer](#rdpconnect).

<a id="rdplicense"></a>
## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>De externe sessie is beëindigd omdat er geen extern bureaublad-licentieservers beschikbaar een licentie kunnen verlenen.

Oorzaak: De 120 dagen respijtperiode voor de extern bureaublad-serverfunctie is verlopen en moet u licenties installeren.

Als tijdelijke oplossing, een lokale kopie van het RDP-bestand opslaan vanuit de portal en deze opdracht uitvoert vanaf een opdrachtprompt PowerShell om verbinding te maken. Deze stap schakelt de licentieverlening voor alleen die verbinding:

        mstsc <File name>.RDP /admin

Als u meer dan twee gelijktijdige verbindingen met extern bureaublad voor VM eigenlijk niet hoeft, kunt u Server Manager te verwijderen van de functie Extern bureaublad-Server.

Zie de blog boeken [Azure VM mislukt met 'Geen extern bureaublad-licentieservers Servers beschikbaar'](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/)voor meer informatie.

<a id="rdpname"></a>
## <a name="remote-desktop-cant-find-the-computer-name"></a>Extern bureaublad kan de computer 'naam' niet vinden.

Oorzaak: De client voor extern bureaublad op uw computer kan de naam van de computer in de instellingen van het RDP-bestand niet ophalen.

Mogelijke oplossingen:

- Als u op het intranet van een organisatie bent, zorg ervoor dat de computer toegang tot de proxy-server heeft en HTTPS-verkeer naar deze verzenden kan.

- Als u een lokaal opgeslagen RDP-bestand gebruikt, kunt u die wordt gegenereerd door de portal. Deze stap zorgt u ervoor dat u beschikt over de juiste DNS-naam voor de virtuele machine of de cloud-service en poort van het eindpunt van de VM. Hier volgt een voorbeeld van RDP-bestand gegenereerd door de portal:

        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Het gedeelte van het RDP-bestand is:
- De FQDN-naam van de cloud-service met de VM ("tailspin-azdatatier.cloudapp.net' in dit voorbeeld).

- De externe TCP-poort van het eindpunt voor het verkeer voor extern bureaublad (55919).

<a id="rdpauth"></a>
## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Verificatie is fout. Kan geen contact maken met de LSA.

Oorzaak: Het doel VM kan de beveiligingsautoriteit vinden in het gebruikersgedeelte van uw referenties.

Als uw gebruikersnaam in de vorm van *SecurityAuthority*is\\*gebruikersnaam* (voorbeeld: CORP\User1), het *SecurityAuthority* gedeelte is de computernaam van de VM (voor de lokale beveiligingsautoriteit) of een Active Directory-domeinnaam.

Mogelijke oplossingen:

- Als de account lokaal op de VM, zorg dat de VM-naam juist is gespeld.

- Als de account in een Active Directory-domein, de spelling van de naam van het domein.

- Als dit een Active Directory-domein-account is en de domeinnaam juist is gespeld, controleert u of een domeincontroller beschikbaar is in het domein. Het is een algemeen probleem in Azure virtuele netwerken met domeincontrollers die een domeincontroller is niet beschikbaar omdat deze nog niet is gestart. Als tijdelijke oplossing kunt u een lokale administrator-account in plaats van een domeinaccount.

<a id="wincred"></a>
## <a name="windows-security-error-your-credentials-did-not-work"></a>Fout in Windows-beveiliging: uw referenties werken niet.

Oorzaak: Het doel VM kan niet valideren uw accountnaam en wachtwoord.

Een Windows-computer kunt valideren van de referenties van een domeinaccount of een lokale account.

- Gebruik de *computernaam*voor lokale accounts\\*gebruikersnaam* syntaxis (voorbeeld: SQL1\Admin4798).
- Voor domeinaccounts, gebruikt u de *domeinnaam*\\*gebruikersnaam* syntaxis (voorbeeld: CONTOSO\peterodman).

Als uw VM met een domeincontroller in een nieuw Active Directory-forest is gepromoveerd, wordt u aangemeld met lokale administrator-account omgezet in een gelijkwaardige account hetzelfde wachtwoord in het nieuwe forest en domein. De lokale account wordt vervolgens verwijderd.

Als u aangemeld met de lokale DC1\DCAdmin-account en vervolgens de virtuele machine gepromoveerd tot een domeincontroller in een nieuw forest voor het domein corp.contoso.com, bijvoorbeeld de lokale account voor DC1\DCAdmin wordt verwijderd en een nieuwe domeinaccount (CORP\DCAdmin) wordt gemaakt met hetzelfde wachtwoord.

Zorg ervoor dat de naam is een naam die de virtuele machine kunt controleren of een geldige account en het wachtwoord klopt.

Als u het wachtwoord van de lokale administrator-account wijzigen moet, raadpleegt u [het opnieuw instellen van een wachtwoord of de service Extern bureaublad voor Windows virtuele machines](virtual-machines-windows-reset-rdp.md).

<a id="rdpconnect"></a>
## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Deze computer kan geen verbinding maken met de externe computer.

Oorzaak: De account die wordt gebruikt om verbinding te maken heeft geen extern bureaublad aanmelden rechten.

Elke Windows-computer is een extern bureaublad gebruikers lokale groep met de accounts en groepen die u op het op afstand inloggen kunnen. Leden van de groep lokale beheerders hebben ook toegang, zelfs als deze accounts niet in de lokale groep Externe bureaubladgebruikers weergegeven worden. Voor computers die deel uitmaakt van een domein bevat de lokale beheerdersgroep ook de Domeinadministrators voor het domein.

Zorg ervoor dat de account die u via verbinding met extern bureaublad aanmelden rechten heeft. Als tijdelijke oplossing, een domein of de lokale administrator-account via verbinding met extern bureaublad. Om de gewenste account toevoegen aan de lokale groep Externe bureaubladgebruikers, gebruikt u de module Microsoft Management Console (**System Tools > lokale gebruikers en groepen > groepen > Externe bureaubladgebruikers**).


## <a name="next-steps"></a>Volgende stappen
Als geen van deze fouten is opgetreden en u een onbekend probleem hebt met de verbinding te maken met RDP, Zie de [handleiding voor extern bureaublad oplossen](virtual-machines-windows-troubleshoot-rdp-connection.md).

- [Azure diagnostics-pakket IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Zie [problemen met toegang tot een toepassing wordt uitgevoerd op een Azure VM](virtual-machines-linux-troubleshoot-app-connection.md)voor probleemoplossing bij het openen van toepassingen op een VM.
- Als u problemen met behulp van Secure Shell (SSH ondervindt) verbinding maken met een Linux VM in Azure, Zie [problemen met SSH-verbindingen naar een Linux VM in Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).