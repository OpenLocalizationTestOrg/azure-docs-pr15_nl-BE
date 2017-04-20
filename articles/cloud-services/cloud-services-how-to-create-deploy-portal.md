<properties
    pageTitle="Het maken en implementeren van een cloud service | Microsoft Azure"
    description="Informatie over het maken en implementeren van een cloud-service met behulp van de portal Azure."
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




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Het maken en implementeren van een cloud-service

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-create-deploy-portal.md)
- [Azure klassieke portal](cloud-services-how-to-create-deploy.md)

De Azure portal biedt twee manieren om te maken en implementeren van een cloud-service: *Snelle* en *Aangepaste maken*.

In dit artikel wordt uitgelegd hoe u de methode Snelle invoer gebruiken om een nieuwe wolk service maken en vervolgens **uploaden** te uploaden en te implementeren, een cloud servicepakket in Azure. Wanneer u deze methode gebruikt, kunt u de Azure portal beschikbaar handige koppelingen voor het voltooien van alle eisen die u gaat. Als u uw cloud service implementeren wanneer u deze maakt, kunt u beide tegelijkertijd gebruik maken van aangepaste doen.

> [AZURE.NOTE] Als u van plan bent voor het publiceren van uw cloud-service van Visual Studio Team Services (VSTS), snelle invoer gebruiken en vervolgens publiceren VSTS instellen vanuit de Quickstart Azure of het dashboard. Zie voor meer informatie, [Continue levering aan Azure door met behulp van Visual Studio Team Services][TFSTutorialForCloudService], of Zie help voor de pagina **Quick Start** .

## <a name="concepts"></a>Concepten
Drie onderdelen zijn vereist voor het implementeren van een toepassing als een cloud-service in Azure:

- **Definitie van service**  
  Het definitiebestand van cloud-service (.csdef) definieert het service-model, met inbegrip van het aantal rollen.

- **Configuratie van de service**  
  Het configuratiebestand van de cloud-service (.cscfg) bevat configuratie-instellingen voor de cloud service en individuele functies, waaronder het aantal exemplaren van de rol.

- **Servicepakket**  
  Het servicepakket (.cspkg) bevat de code van de toepassing en configuraties en het definitiebestand.

Voor meer informatie over deze en het maken van een pakket [hier](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Uw app voorbereiden
Voordat u een service cloud implementeren kunt, moet u het pakket in de cloud service (.cspkg) van de code van uw toepassing en een wolk service-configuratiebestand (.cscfg). De SDK Azure biedt hulpmiddelen voor het voorbereiden van deze bestanden vereist implementatie. U kunt de SDK installeren vanaf de pagina [Downloads Azure](https://azure.microsoft.com/downloads/) in de taal waarin u de voorkeur geeft voor het ontwikkelen van uw toepassingscode.

Drie cloud servicefuncties vereisen speciale configuraties voordat u een servicepakket exporteren:

- Als u wilt een cloud service implementeren die maakt gebruik van Secure Sockets Layer (SSL) voor gegevenscodering, [uw toepassing configureren](cloud-services-configure-ssl-certificate-portal.md#modify) voor SSL.

- Als u extern bureaublad-verbindingen met exemplaren van de rol, [de rollen configureren](cloud-services-role-enable-remote-desktop.md) voor extern bureaublad configureren. Dit is alleen mogelijk in de klassieke portal.

- Als u configureren uitgebreide controle van uw cloud-service wilt, een Azure diagnoseprogramma inschakelen voor de service cloud. *Minimale controle* (de standaard niveau controleren) maakt gebruik van prestatiemeteritems die worden verzameld door de host-besturingssystemen voor rol exemplaren (virtuele machines). *Uitgebreide controle* verzamelt extra statistieken op basis van prestatiegegevens in de exemplaren van de rol om dichter bij analyse van de problemen die zich tijdens de verwerking van toepassing voordoen. Azure Diagnostics inschakelen, Zie [Diagnostische gegevens inschakelen in Azure](cloud-services-dotnet-diagnostics.md).

Als u wilt een cloud-service maken met implementaties van web-rollen of functies van de werknemer, moet u [het servicepakket maken](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Voordat u begint

- Als u de Azure SDK hebt geïnstalleerd, op **Azure SDK installeren** om de [pagina Downloads Azure](https://azure.microsoft.com/downloads/)te openen en download de SDK voor de taal waarin u wilt uw code te ontwikkelen. (Je hebt u de mogelijkheid om dit te doen later.)

- Als u alle exemplaren van de rol van een certificaat vereist is, maken de certificaten. Cloud services vereist een pfx-bestand met een persoonlijke sleutel. [Dat u kunt de certificaten naar Azure uploaden]() als u maken en implementeren van de cloud-service.

- Als u van plan bent de cloud-service een groep affiniteit implementeren, maakt u de groep affiniteit. U kunt een groep affiniteit uw cloud-service en andere Azure services implementeren op dezelfde locatie in een gebied. U kunt de groep affiniteit in de **netwerken** van de Azure klassieke portal op de pagina **affiniteit groepen** maken.


## <a name="create-and-deploy"></a>Maken en implementeren

1. Log in op de [Azure portal](https://portal.azure.com/).
2. Klik op **Nieuw > virtuele Machines**, en schuif omlaag naar en klik op **De Service Cloud**.

    ![Publiceren van uw cloud-service](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)

3. Onderaan op de pagina die wordt weergegeven, klikt u op **maken**. 
4. Voer een waarde voor de **DNS-naam**in de nieuwe **Wolk Service** -blade.
5. Een nieuwe **Resourcegroep** maken of een bestaande selecteren.
6. Selecteer een **locatie**.
7. Klik op het **pakket**. Hiermee opent u het **uploaden van een pakket** blade. Vul de vereiste velden in.  

     Als een van de rollen een enkel exemplaar bevat, zorg **dat implementeren, zelfs als een of meer rollen een enkel exemplaar bevatten** is ingeschakeld.

8. Zorg ervoor dat **de implementatie Start** is geselecteerd.
9. Klik op **OK** , waardoor het **uploaden van een pakket** blade wordt afgesloten.
10. Als er geen andere certificaten toe te voegen, klikt u op **maken**.

    ![Publiceren van uw cloud-service](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Een certificaat uploaden

Als het installatiepakket [geconfigureerd om certificaten te gebruiken is](cloud-services-configure-ssl-certificate-portal.md#modify), kunt u nu het certificaat uploaden.

1. Selecteer **certificaten**, en vervolgens het **wachtwoord** opgeven voor het certificaat, selecteert u het pfx-bestand van het SSL-certificaat op de bladeserver **certificaten toevoegen**
2. Klik op **certificaat koppelen**en klik vervolgens op **OK** in het blad **certificaten toevoegen** .
3. Klik op **maken** op de **Service Cloud** -blade. Wanneer de implementatie de status **Gereed** bereikt is, kunt u doorgaan naar de volgende stappen.

    ![Publiceren van uw cloud-service](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)


## <a name="verify-your-deployment-completed-successfully"></a>Controleer of de installatie is voltooid

1. Klik op het exemplaar van de wolk.

    De status wordt weergegeven dat de service **uitgevoerd wordt**.

2. Onder **Essentials**, klikt u op de **Site-URL** naar de cloud-service in een webbrowser.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)


[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Volgende stappen

* [Algemene configuratie van de service cloud](cloud-services-how-to-configure-portal.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md)configureren.
* [Uw cloud-service beheren](cloud-services-how-to-manage-portal.md).
* [Ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md)configureren.