<properties
    pageTitle="Web-Apps op Azure Stack - bekende problemen en het oplossen van problemen | Microsoft Azure"
    description="Gedetailleerde richtlijnen voor het implementeren van Web Apps in Azure stapel"
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
    
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Web Apps Resource Provider - bekende problemen en het oplossen van problemen

> [AZURE.NOTE] De volgende informatie geldt alleen voor Azure Stack TP1-implementaties.

Als u problemen ondervindt bij het implementeren van of werken met Web Apps op Azure Stack Zie de onderstaande richtlijnen.

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure Stack Web Apps niet beschikbaar in de portal

![Azure Stack Web Apps maken nieuwe Web App][1]

Na de [registratie van de Azure Stack Web Apps-Provider](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm) als u niet kunt met het Web + mobiele blade vinden en vervolgens probeert u het volgende:
* **Meld u af van de portal** en **sluit u de browser** en klik vervolgens in een **nieuwe browser venster aanmelden op de portal** en probeer het opnieuw.

Als u nog steeds niet zien het web en mobiele blade, probeert u het volgende:

1.  Zoek de **xRPVM** en de **verbinding** met de VM op de **Hostcomputer Azure-Stack** in **Hyper-V-beheer** .
2.  Open een **opdrachtprompt als Administrator** en **IISRESET** uitvoeren
3.  Terug naar de **ClientVM** en open een **nieuw browservenster**, **Meld u aan bij de portal** en probeer het opnieuw.

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>Implementatie mislukt bij het maken van een Web App in een nieuwe resourcegroep

In het eerste voorbeeld van Web Apps, moeten **Alle Web Apps** worden gemaakt in dezelfde bronnengroep als Web Apps Resource Provider (**AppService-lokale**).  Dit is een bekend probleem en worden opgelost in een toekomstige voorbeeld.

## <a name="other-issues"></a>Andere problemen

Als er andere problemen met Web Apps op Azure Stack boek in [Azure Stack Forum] (https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) waar de leden van ons team berichten wilt controleren.


<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
