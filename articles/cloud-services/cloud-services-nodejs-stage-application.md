<properties 
    pageTitle="Fase een cloud service implementeren (Node.js) | Microsoft Azure" 
    description="Informatie over hoe uw toepassing Azure een staging-omgeving te implementeren en vervolgens implementeren in een productieomgeving met behulp van virtuele IP-(VIP) wisselen." 
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



# <a name="staging-an-application-in-azure"></a>Gefaseerde installatie van een toepassing in Azure

Gebundelde toepassingen kan worden geïmplementeerd op de testomgeving in Azure te testen voordat u deze verplaatst naar de productieomgeving waarin de toepassing op het Internet toegankelijk is. De testomgeving is net als de productie-omgeving, met dien verstande dat u alleen toegang tot de gefaseerde toepassing met een betekenisloze URL die wordt gegenereerd door Azure. Nadat u hebt gecontroleerd of de toepassing correct werkt, kan het wisselbestand van het virtuele IP-(VIP) door naar de productieomgeving worden geïmplementeerd.

> [AZURE.NOTE] De stappen in dit artikel zijn alleen van toepassing op het knooppunt toepassingen als Azure Cloud Service gehost.

## <a name="step-1-stage-an-application"></a>Stap 1: Voorbereiden van een toepassing

Deze taak wordt beschreven hoe u een toepassing installeert met behulp van de **Microsoft Azure PowerShell**.

1.  Bij het publiceren van een service, geven de **-sleuf** parameter aan de cmdlet **Publiceren AzureServiceProject** .

    ```powershell
    Publish-AzureServiceProject -Slot staging
    ```

2.  Meld u aan bij de [Azure klassieke portal] en selecteer **Cloud Services**. Nadat de service cloud wordt gemaakt en de **tijdelijke** kolom status **actief**is bijgewerkt, klik op de servicenaam van de.

    ![weergeven van een actieve service portal][cloud-service]

3.  Selecteer het **Dashboard**, en selecteer vervolgens **klaarzetten**.

    ![cloud servicedashboard][cloud-service-dashboard]

4. Noteer de waarde in de vermelding van de **URL van de Site** aan de rechterkant. De DNS-naam is een betekenisloze interne ID die Azure gegenereerd.

    ![url van de site][cloud-service-staging-url]

U kunt nu controleren of de toepassing correct in de testomgeving werkt met behulp van de tijdelijke URL van de site.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Stap 2: Upgrades uitvoeren voor toepassingen in de productie door Swapping VIP 's

Nadat u hebt gecontroleerd dat de bijgewerkte versie van een toepassing in de testomgeving, kunt u snel het beschikbaar maken in de productie door het virtuele IP-adressen (VIP's) van de ontwikkel- en productiecomputers omgevingen te verwisselen.

> [AZURE.NOTE] Deze stap wordt ervan uitgegaan dat u hebt al een toepassing op de productie wordt geïmplementeerd en gefaseerd de bijgewerkte versie van de toepassing.

1.  Log in op de [Azure klassieke portal] **Cloud Services** op en selecteert u de servicenaam.

2.  **Staging**selecteren uit het **Dashboard**en klik op **omwisselen** onderaan de pagina. Hiermee opent u het dialoogvenster VIP Swap.

    ![VIP swap dialoogvenster][vip-swap-dialog]

3.  Lees de informatie en klik vervolgens op **OK**. De twee implementaties beginnen met het bijwerken van de tijdelijke installatie schakelt over naar de productie en de productie-implementatie wordt overgeschakeld naar de staging.

Je hebt een implementatie klaargezet en een productie-implementatie door VIP's te verwisselen met de implementatie in staging bijgewerkt.

## <a name="additional-resources"></a>Aanvullende bronnen

- [Wisselen van VIP's in Azure implementatie Service-Upgrade naar productie]

[Azure klassieke portal]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Wisselen van VIP's in Azure implementatie Service-Upgrade naar productie]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
