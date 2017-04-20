<properties 
    pageTitle="Het configureren van een cloud-service (portal) | Microsoft Azure" 
    description="Informatie over het cloud-services configureren in Azure. Informatie over de configuratie van de service cloud bijwerken en configureren van externe toegang tot de rol van exemplaren. Deze voorbeelden gebruiken de Azure portal." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="how-to-configure-cloud-services"></a>Cloud-Services configureren

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-configure-portal.md)
- [Azure klassieke portal](cloud-services-how-to-configure.md)

In de Azure portal kunt u de meestgebruikte instellingen voor een cloud-service. Of als u direct bijwerken van de configuratiebestanden, downloaden van een bestand van de service-configuratie wilt bijwerken, het bijgewerkte bestand uploaden en de service cloud bijwerken met wijzigingen in de configuratie. In beide gevallen is de configuratie-updates naar buiten wijzen voor alle exemplaren van de rol.

U kunt ook de exemplaren van uw cloud service rollen of extern bureaublad naar deze beheren.

Azure alleen ervoor 99.95% beschikbaarheid van de service tijdens het bijwerken van de configuratie hebt u ten minste twee exemplaren van de rol voor elke rol. Waarmee een virtuele machine voor het verwerken van clientaanvragen terwijl het andere wordt bijgewerkt. Zie voor meer informatie, [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Een cloud-service wijzigen

Na het openen van de [portal Azure](https://portal.azure.com/), Ga naar de cloud-service. Hier kunt u vele aspecten van het beheren. 

![Pagina-instellingen](./media/cloud-services-how-to-configure-portal/cloud-service.png)

De **Instellingen** of **alle** koppelingen wordt geopend de blade **Instellingen** kunt u de **Eigenschappen**wijzigen, wijzigt de **configuratie**, **certificaten**, setup **waarschuwingsregels**beheren en **gebruikers** die toegang hebben tot deze cloud-service beheren.

![Azure cloud service instellingen blade](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

>[AZURE.NOTE]
>Het besturingssysteem dat wordt gebruikt voor de cloud-service kan niet worden gewijzigd met behulp van de **portal Azure**, kunt u deze instelling via het [portal voor Azure klassieke](http://manage.windowsazure.com/)alleen wijzigen. Dit is gedetailleerd [hier](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file).

## <a name="monitoring"></a>Monitoring

U kunt waarschuwingen toevoegen aan uw cloud-service. Klik op **Instellingen** > **Waarschuwing regels** > **melding toevoegen**. 

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Hier kunt u instellen dat een waarschuwing. U kunt een waarschuwing voor de volgende typen gegevens kunt instellen met de vervolgkeuzelijst **Mertic** .

- Schijf lezen
- Schijf schrijven
- Het netwerk
- Uit het netwerk
- CPU-percentage 

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Controle van een metrische tegel configureren

In plaats van met behulp van **Instellingen voor** > **Waarschuwingsregels**, kunt u klikken op een van de metrische tegels in de sectie **bewaking** van de bladeserver **Cloud-service** .

![Cloud-Service controleren](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Hier kunt u de grafiek gebruikt in combinatie met de tegel aanpassen of toevoegen van een waarschuwingsregel.


## <a name="reboot-reimage-or-remote-desktop"></a>Opnieuw opstarten, reimage of extern bureaublad

Op dit moment kunt u extern bureaublad met behulp van de **portal Azure**niet configureren. Echter, u kunt instellen dat via de [portal voor Azure klassieke](cloud-services-role-enable-remote-desktop.md) [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), of via [Visual Studio](../vs-azure-tools-remote-desktop-roles.md). 

Klik eerst op het exemplaar van de wolk.

![Cloud Service-exemplaar](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Van het blad dat wordt geopend uou kan tot stand brengen van een verbinding met extern bureaublad, de sessie op afstand opnieuw op te starten of op afstand reimage (met een verse afbeelding) het exemplaar start.

![Cloud Service-exemplaar knoppen](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)



## <a name="reconfigure-your-cscfg"></a>De .cscfg configureren

Wellicht moet u opnieuw configureren van de wolk-service via het bestand [config service (cscfg)](cloud-services-model-and-package.md#cscfg) . U moet eerst het bestand .cscfg downloaden, wijzigen en vervolgens uploaden.

1. Klik op **het pictogram** of de koppeling van **alle instellingen** om de **Instellingen voor** blade te openen.

    ![Pagina-instellingen](./media/cloud-services-how-to-configure-portal/cloud-service.png)

2. Klik op de **configuratie** -item.

    ![Configuratie Blade](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)

3. Klik op de knop **downloaden** .

    ![Downloaden](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

4. Na het bijwerken van het configuratie-bestand uploaden en de configuratie-updates:

    ![Uploaden](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
5. Selecteer het bestand .cscfg en klik op **OK**.

            
## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [implementeren van een cloud-service](cloud-services-how-to-create-deploy-portal.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md)configureren.
* [Uw cloud-service beheren](cloud-services-how-to-manage-portal.md).
* [Ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md)configureren.