<properties 
    pageTitle="Inschakelen, extern bureaublad voor cloud services (Node.js)" 
    description="Informatie over het inschakelen van extern bureaublad-toegang voor de virtuele machines die als host fungeert voor uw toepassing Node.js Azure." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="enabling-remote-desktop-in-azure"></a>Inschakelen, extern bureaublad in Azure

Extern bureaublad kunt u toegang tot het bureaublad van een rol exemplaar dat wordt uitgevoerd in Azure. U kunt een verbinding met extern bureaublad configureren van de virtuele machine of het oplossen van problemen met uw toepassing.

> [AZURE.NOTE] Dit artikel is van toepassing op Node.js toepassingen als Azure Cloud Service gehost.


## <a name="prerequisites"></a>Vereisten

- Installeer en configureer [Azure Powershell](../powershell-install-configure.md).
- Een app Node.js implementeren op een Azure Cloud-Service. Zie voor meer informatie, [bouwen en implementeren van een toepassing Node.js een Azure Cloud-service](cloud-services-nodejs-develop-deploy-app.md).


## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>Stap 1: Gebruik Azure PowerShell service voor externe bureaubladverbinding configureren

Voor het gebruik van extern bureaublad, moet u de definitie van Azure service en de configuratie bijwerken met een gebruikersnaam, wachtwoord en certificaat. 

De volgende stappen uitvoeren vanaf een computer met de bronbestanden voor uw app.

1. **Windows PowerShell** als Administrator uitvoeren. (Vanuit het **Menu Start** of het **Scherm Start**, zoekt **Windows PowerShell**.)

2.  Navigeer naar de map waarin de definitie van service (.csdef) en service-configuratiebestanden (.cscfg).

3. Voer de volgende PowerShell-cmdlet:

        Enable-AzureServiceProjectRemoteDesktop

4. Geef een gebruikersnaam en wachtwoord bij de opdrachtprompt.

    ![inschakelen azureserviceprojectremotedesktop][enable-rdp]

3.  Voer de volgende PowerShell-cmdlet om de wijzigingen te publiceren:

        Publish-AzureServiceProject

    ![publiceren azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>Stap 2: Verbinding maken met het exemplaar van de rol

Nadat u de definitie van de update service publiceert, kunt u het exemplaar van de rol.

1.  In de [Azure klassieke portal] **Cloud Services** selecteren en selecteer vervolgens uw service.

    ![Azure klassieke portal][cloud-services]

2.  Klik op **instanties**, en klik vervolgens op **productie** of **klaarzetten** om te zien de exemplaren voor uw service. Selecteer een instantie en klik op **verbinding maken met** onderaan de pagina.

    ![De pagina exemplaren][3]

2.  Wanneer u klikt op **verbinden**, wordt de webbrowser vraagt u een RDP-bestand op te slaan. Open dit bestand. (Bijvoorbeeld als u Internet Explorer, klik op **openen**.)

    ![vragen om het RDP-bestand opent of opslaat][4]

3.  Wanneer het bestand wordt geopend, wordt de volgende beveiligingsprompt weergegeven:

    ![Windows security prompt][5]

4.  Klik op **verbinding maken**, en wordt een beveiligingsprompt weergegeven voor het invoeren van referenties voor toegang tot het exemplaar. Voer het wachtwoord dat u hebt gemaakt in [stap 1] [stap 1: Configureer de service voor externe bureaubladverbinding met Azure PowerShell], en klik vervolgens op **OK**.

    ![gebruikersnaam en wachtwoord gevraagd][6]

Als de verbinding is gemaakt, wordt verbinding met extern bureaublad het bureaublad van het exemplaar in Azure. 

![Extern bureaublad-sessies][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>Stap 3: Configureer de service voor extern bureaublad uitschakelen 

Wanneer u verbindingen met extern bureaublad op de rol van instanties in de cloud niet meer nodig hebt, schakelt u toegang tot extern bureaublad met [Azure PowerShell].

1.  Voer de volgende PowerShell-cmdlet:

        Disable-AzureServiceProjectRemoteDesktop

2.  Voer de volgende PowerShell-cmdlet om de wijzigingen te publiceren:

        Publish-AzureServiceProject

## <a name="additional-resources"></a>Aanvullende bronnen

- [Op afstand toegang krijgen tot de rol van exemplaren in Azure] 
- [Met behulp van extern bureaublad met Azure rollen]
- [Node.js Developer Center](/develop/nodejs/)

  [Azure PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure klassieke portal]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
[Op afstand toegang krijgen tot de rol van exemplaren in Azure]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Met behulp van extern bureaublad met Azure rollen]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 