<properties
    pageTitle="Het maken en implementeren van een cloud service | Microsoft Azure"
    description="Informatie over het maken en implementeren van een cloud-service met behulp van de methode voor snelle invoer in Azure."
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
    ms.date="09/06/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Het maken en implementeren van een Cloud-Service

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-create-deploy-portal.md)
- [Azure klassieke portal](cloud-services-how-to-create-deploy.md)

De Azure klassieke portal biedt twee manieren om te maken en implementeren van een cloud-service: **Snelle** en **Aangepaste maken**.

In dit onderwerp wordt uitgelegd hoe u de methode Snelle invoer gebruiken om een nieuwe wolk service maken en vervolgens **uploaden** te uploaden en te implementeren, een cloud servicepakket in Azure. Wanneer u deze methode gebruikt, kunt u de klassieke Azure portal beschikbaar handige koppelingen voor het voltooien van alle eisen die u gaat. Als u uw cloud service implementeren wanneer u deze maakt, kunt u beide tegelijkertijd gebruik **Maken van aangepaste**doen.

> [AZURE.NOTE] Als u van plan bent voor het publiceren van uw cloud-service van Visual Studio Team Services (VSTS), snelle invoer gebruiken en vervolgens publiceren VSTS instellen uit **Snel starten** of het dashboard. Zie voor meer informatie, [Continue levering aan Azure door met behulp van Visual Studio Team Services][TFSTutorialForCloudService], of Zie help voor de pagina **Quick Start** .

## <a name="concepts"></a>Concepten
Implementeren van een toepassing als een cloud-service in Azure zijn drie onderdelen nodig:

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

- Als u wilt een cloud service implementeren die maakt gebruik van Secure Sockets Layer (SSL) voor gegevenscodering, [uw toepassing configureren](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) voor SSL.

- Als u extern bureaublad-verbindingen met exemplaren van de rol, [de rollen configureren](cloud-services-role-enable-remote-desktop.md) voor extern bureaublad configureren.

- Als u configureren uitgebreide controle van uw cloud-service wilt, een Azure diagnoseprogramma inschakelen voor de service cloud. *Minimale controle* (de standaard niveau controleren) maakt gebruik van prestatiemeteritems die worden verzameld door de host-besturingssystemen voor rol exemplaren (virtuele machines). "Uitgebreide monitoring * extra statistieken op basis van prestatiegegevens in de exemplaren van de rol om dichter bij analyse van de problemen die zich tijdens de verwerking van de toepassing voordoen worden verzameld. Azure Diagnostics inschakelen, Zie [Diagnostische gegevens inschakelen in Azure](cloud-services-dotnet-diagnostics.md).

Als u wilt een cloud-service maken met implementaties van web-rollen of functies van de werknemer, moet u [het servicepakket maken](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Voordat u begint

- Als u de Azure SDK hebt geïnstalleerd, op **Azure SDK installeren** om de [pagina Downloads Azure](https://azure.microsoft.com/downloads/)te openen en download de SDK voor de taal waarin u wilt uw code te ontwikkelen. (Je hebt u de mogelijkheid om dit te doen later.)

- Als u alle exemplaren van de rol van een certificaat vereist is, maken de certificaten. Cloud services vereist een pfx-bestand met een persoonlijke sleutel. U kunt [de certificaten naar Azure uploaden](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) bij het maken en implementeren van de cloud-service.

- Als u van plan bent de cloud-service een groep affiniteit implementeren, maakt u de groep affiniteit. U kunt een groep affiniteit uw cloud-service en andere Azure services implementeren op dezelfde locatie in een gebied. U kunt de groep affiniteit in de **netwerken** van de Azure klassieke portal op de pagina **affiniteit groepen** maken.


## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Procedure: een cloud service met snelle invoer maken

1. Klik op **Nieuw**in [Azure klassieke portal](http://manage.windowsazure.com/)>**berekenen**>**Cloud Service**>**Snelle invoer**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)

2. Voer in de **URL**, een subdomeinnaam gebruiken in de openbare URL voor toegang tot uw cloud-service in de productie-implementaties. De URL-notatie voor de productie-implementaties: http://*myURL*. cloudapp.net.

3. Selecteer in het **gebied of de groep affiniteit**, geografisch gebied of de groep affiniteit voor de implementatie van de cloud-service. Selecteer een groep affiniteit als u wilt uw cloud service implementeren op dezelfde locatie als andere Azure diensten binnen een regio.

4. Klik op **de Service Cloud maken**.

    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)

    U kunt de status van het proces in het berichtgebied onder in het venster controleren.

    Het gebied van **Cloud-Services** wordt geopend met de nieuwe cloud-service weergegeven. Wanneer de status op gemaakt verandert, is maken van de cloud-service voltooid.

    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)


## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Procedure: een certificaat voor een service cloud uploaden

1. In [Azure klassieke portal](http://manage.windowsazure.com/) **Cloud Services**klikt u op, klikt u op de naam van de cloud-service en klik vervolgens op **certificaten**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)


2. Klik op **een certificaat uploaden** of **uploaden**.

3. In **bestanden**, gebruik **Bladeren** om het certificaat (.pfx-bestand) te selecteren.

4. Voer in het vak **wachtwoord**voor de persoonlijke sleutel voor het certificaat.

5. Klik op **OK** (vinkje).

    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)

    U kunt de voortgang van het uploaden in het gedeelte bericht hieronder bekijken. Wanneer de upload is voltooid, wordt het certificaat toegevoegd aan de tabel. Klik op OK om het bericht te sluiten in het berichtengebied.

    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Procedure: een cloud-service implementeren

1. In de [Azure klassieke portal](http://manage.windowsazure.com/) **Cloud Services**klikt u op, klikt u op de naam van de cloud-service en klik op **Dashboard**.

2. Klik op **uploaden van een nieuwe productie-implementatie** of **uploaden**.

3. Voer in de **implementatie-label**, een naam voor de nieuwe implementatie - bijvoorbeeld MyCloudServicev4.

3. In het **pakket**, gebruik **Bladeren** om het pakketbestand service (.cspkg) te gebruiken.

4. Gebruik **Bladeren** om service configureren bestand (.cscfg) te gebruiken in een **configuratie**.

5. Als de cloud-service alle functies met slechts één exemplaar omvat het selectievakje **zelfs als een of meer rollen één exemplaar bevatten te implementeren** om de implementatie.

    Azure alleen garantie 99.95 procent toegang tot de service cloud tijdens onderhoud en service-updates als ten minste twee exemplaren van elke rol heeft. Indien nodig, kunt u op de pagina **schaal** rol extra exemplaren toevoegen nadat u de cloud-service implementeert. Zie voor meer informatie, [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

6. Klik op **OK** (vinkje) om te beginnen met de implementatie van de service cloud.

    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)

    U kunt de status van de implementatie in het berichtgebied controleren. Klik op OK om het bericht te verbergen.

    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Controleer of de installatie is voltooid

1. Klik op het **Dashboard**.

    De status wordt weergegeven dat de service **uitgevoerd wordt**.

2. Onder een **vluchtige blik**, klikt u op de site-URL naar de cloud-service in een webbrowser.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


[TFSTutorialForCloudService]: cloud-services-continuous-delivery-use-vso.md
 
## <a name="next-steps"></a>Volgende stappen

* [Algemene configuratie van de service cloud](cloud-services-how-to-configure.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name.md)configureren.
* [Uw cloud-service beheren](cloud-services-how-to-manage.md).
* [Ssl-certificaten](cloud-services-configure-ssl-certificate.md)configureren.