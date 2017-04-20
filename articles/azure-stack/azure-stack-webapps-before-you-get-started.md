<properties
    pageTitle="Azure Stack App Service technische Preview 1 voordat u aan de slag | Microsoft Azure"
    description="Stappen voor het implementeren van Web Apps op Azure Stack voltooien"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="before-you-get-started-with-azure-stack-web-apps"></a>Voordat u aan de slag met Azure Stack Web Apps

> [AZURE.NOTE] De volgende informatie geldt alleen voor Azure Stack TP1-implementaties.

U moet een paar items Azure Stack Web Apps te installeren:

- Een [Azure Stack technisch voorbeeld 1](azure-stack-run-powershell-script.md) is geïmplementeerd
- Onvoldoende ruimte in uw systeem Azure Stack voor een kleine implementatie van Azure Stack Web Apps.  De benodigde ruimte is ongeveer 20GB schijfruimte.
- [Een server waarop SQL Server wordt uitgevoerd](#SQL-Server).

>[AZURE.NOTE] De volgende stappen plaats op de Client-VM vindt.

## <a name="before-you-deploy-azure-stack-web-apps"></a>Voordat u Azure Stack Web Apps implementeren

Voor de implementatie van een resource-provider, moet u de PowerShell Integrated Scripting Environment(ISE) als beheerder uitvoeren. Om deze reden moet u cookies en JavaScript in de Internet Explorer-profiel dat u gebruikt voor aanmelding bij Azure Active Directory toestaan.

## <a name="turn-off-internet-explorer-enhanced-security"></a>Verbeterde beveiliging van Internet Explorer uitschakelen

1.  Aanmelden bij de computer Azure Stack bewijs van concept (Implementatiemodel) als **AzureStack/beheerder**en opent u **Server Manager**.
2.  **Internet Explorer Enhanced Security Configuration** uitschakelen voor zowel beheerders als gebruikers.
3.  ClientVM.AzureStack.local virtual machine als beheerder aanmelden en opent u **Server Manager**.
4.  **Internet Explorer Enhanced Security Configuration** uitschakelen voor zowel beheerders als gebruikers.

## <a name="enable-cookies"></a>Cookies inschakelen

1.  Selecteer **Start**> **alle apps**, > **Windows Bureau-accessoires**. Klik met de rechtermuisknop op **Internet Explorer** > **meer** > **uitvoeren als beheerder**.
2.  Als u wordt gevraagd, selecteer **Gebruik aanbevolen beveiliging**en klik vervolgens op **OK**.
3.  Selecteer in Internet Explorer, **Extra** (het vistuig pictogram), > **Internet-opties** > **Privacy** > **Geavanceerd**.
4.  Selecteer **Geavanceerd**. Controleer of beide selectievakjes **accepteren** zijn geselecteerd, en selecteer **OK** en **OK** opnieuw.
5.  Sluit Internet Explorer af en start de PowerShell ISE als beheerder.

## <a name="install-the-latest-version-of-azure-powershell"></a>Installeer de nieuwste versie van Azure PowerShell

1.  Aanmelden bij de computer Azure Stack Implementatiemodel als **AzureStack/beheerder**.
2.  Gebruik verbinding met extern bureaublad aanmelden bij de ClientVM.AzureStack.local virtuele machine als beheerder.
3.  Open het **Configuratiescherm** en selecteer **een programma verwijderen**. 
4.  Klik met de rechtermuisknop op **Microsoft Azure PowerShell - November 2015** en selecteer **verwijderen**.
5.  Na het verwijderen is voltooid, start opnieuw op de virtuele machine van ClientVM.AzureStack.local
6.  Download en installeer de meest recente [Azure PowerShell](http://aka.ms/azstackpsh).


## <a name="sql-server"></a>SQL Server

Het installatieprogramma Azure Stack Web Apps wordt standaard gebruik van de SQL-Server die is geïnstalleerd met de Provider Azure Stack SQL-bron. Zorg ervoor dat u dient u de database beheerder een gebruikersnaam en wachtwoord tijdens de installatie van de Provider voor SQL Server Resource (RP SQL). U moet zowel bij het installeren van Azure Stack Web Apps.
Opmerking: U hebt ook de optie voor het implementeren en gebruiken van een andere server met SQL Server. U kunt de SQL Server-exemplaar te gebruiken als u de opties in het installatieprogramma van Azure Stack Web Apps.
