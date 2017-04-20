<properties
   pageTitle="Een toepassingsgateway voor offload van SSL configureren met behulp van de portal | Microsoft Azure"
   description="Deze pagina bevat instructies voor het maken van een toepassingsgateway met SSL via de portal-offload"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Een toepassingsgateway voor offload van SSL configureren met behulp van de portal

> [AZURE.SELECTOR]
-[Azure portal](application-gateway-ssl-portal.md)
-[Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
-[Klassieke PowerShell Azure](application-gateway-ssl.md)

Azure toepassingsgateway kan worden geconfigureerd om de Secure Sockets Layer (SSL)-sessie op de gateway om te voorkomen dat kostbare SSL-decodering taken te gebeuren op de web-farm te beëindigen. SSL-offload vereenvoudigt ook de front-end-server setup en het beheer van de webtoepassing.

## <a name="scenario"></a>Scenario

In het volgende scenario gaat bij het configureren van SSL op een bestaande toepassingsgateway-offload. Het scenario wordt ervan uitgegaan dat u de stappen voor het [maken van een toepassingsgateway](application-gateway-create-gateway-portal.md)al hebt gevolgd.

## <a name="before-you-begin"></a>Voordat u begint

SSL-offload configureren met een toepassingsgateway, is een certificaat vereist. Dit certificaat is geladen op de toepassingsgateway en wordt gebruikt voor het coderen en decoderen van het verkeer dat wordt verzonden via SSL. Het certificaat moet in de indeling Personal Information Exchange (pfx). Deze bestandsindeling kunt voor de persoonlijke sleutel te exporteren die door de toepassingsgateway is vereist voor het uitvoeren van de codering en decodering van verkeer.

## <a name="add-an-https-listener"></a>Een HTTPS-listener toevoegen

De HTTPS-listener gezocht naar verkeer op basis van de configuratie en helpt het verkeer doorsturen naar de back-end-toepassingen.

### <a name="step-1"></a>Stap 1

Navigeer naar de Azure portal en selecteer een bestaande toepassingsgateway

### <a name="step-2"></a>Stap 2

Listeners en klik op de knop toevoegen als u wilt toevoegen een listener.

![App gateway overzicht blade][1]

### <a name="step-3"></a>Stap 3

Vul de vereiste gegevens voor de listener en upload het .pfx-certificaat na afloop klikt u op OK.

**Naam** - deze waarde is een beschrijvende naam van de listener.

**Frontend IP-configuratie** - deze waarde is de frontend IP-configuratie die wordt gebruikt voor de listener.

**Frontend-poort (naam/poort)** - een beschrijvende naam voor de poort die wordt gebruikt op de voorkant van de toepassingsgateway en de werkelijke poort gebruikt.

**Protocol** - een schakelaar om te bepalen als https- of http wordt gebruikt voor de front-endtoepassing.

**Certificaat (naam en wachtwoord)** - offload als SSL wordt gebruikt, een .pfx-certificaat is vereist voor deze instelling en een beschrijvende naam en wachtwoord zijn vereist.

![listener blade toevoegen][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Een regel maken en deze te koppelen aan de listener

De listener is gemaakt. Het is tijd voor het maken van een regel voor het afhandelen van het verkeer van de listener.

### <a name="step-1"></a>Stap 1

Klik op de **regels** van de toepassingsgateway en klik vervolgens op toevoegen.

![App gateway regels blade][3]

### <a name="step-2"></a>Stap 2

Typt in de beschrijvende naam voor de regel op het blad **basisregel toevoegen** en kiest u de listener in de vorige stap hebt gemaakt. Kies de juiste back-end-toepassingen en de HTTP-instelling en klik op **OK**

![HTTPS-instellingenvenster][4]

De instellingen worden nu bewaard op de toepassingsgateway. Het opslaan, verwerken voor deze instellingen kunnen even duren voordat deze worden weergegeven via de portal of via PowerShell. Zodra de toepassingsgateway opgeslagen zorgt voor het coderen en decoderen van verkeer. Alle verkeer tussen de toepassingsgateway en de back-end webservers wordt via http verwerkt. Elke mededeling terug naar de client als via https gestart wordt geretourneerd naar de client gecodeerd.

## <a name="next-steps"></a>Volgende stappen

Zie meer informatie over het configureren van een aangepaste gezondheid sonde met Azure toepassingsgateway, [maken een sonde met aangepaste gezondheid](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png