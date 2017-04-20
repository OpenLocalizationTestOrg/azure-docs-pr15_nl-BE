<properties
   pageTitle="Logica Apps op-premises gegevensverbinding gateway | Microsoft Azure"
   description="Informatie over hoe u een verbinding te maken met de gegevens van ruimten gateway van een app logica."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>Verbinding maken met de gegevens van ruimten gateway voor logica Apps

Ondersteunde logica apps verbindingslijnen kunnen u voor het configureren van uw verbinding met toegang op ruimten gegevens via de gateway van de gegevens van ruimten.  De volgende stappen helpt u bij het installeren en configureren van de gateway van de gegevens op gebouwen om te werken met een app logica.

## <a name="prerequisites"></a>Vereisten

* Met behulp van een werk of school e-mailadres in Azure, de gateway op ruimten gegevens koppelen aan uw account (account Azure Active Directory gebaseerd)
    * Als u een Microsoft-Account (bijv. @outlook.com, @live.com) kunt u uw Azure-account voor het maken van een werk of school e-mailadres door [de stappen hier](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal)

> [AZURE.WARNING] Er is een beperking op dit moment die gateway installeren wordt alleen voltooid als met een account die is geregistreerd voor Power BI op gebouwen.  In de tussentijd Registreer een account met 'Power BI gratis' om de installatie te voltooien.

* Moet zijn op ruimten gegevens gateway [op een lokale computer is geïnstalleerd](app-service-logic-gateway-install.md).
* Gateway moet niet zijn aangevraagd door een andere Azure op ruimten gegevensgateway ([claim gebeurt met maken van stap 2 hieronder](#2-create-an-azure-on-premises-data-gateway-resource)) - een installatie kan alleen worden gekoppeld aan de bron een gateway.

## <a name="installing-and-configuring-the-connection"></a>Installeren en configureren van de verbinding

### <a name="1-install-the-on-premises-data-gateway"></a>1. de gegevens van ruimten gateway installeren

[In dit artikel](app-service-logic-gateway-install.md)vindt u informatie over het installeren van de gateway van de gegevens van ruimten.  De gateway moet zijn geïnstalleerd op een computer op gebouwen voordat u kunt doorgaan met de rest van de stappen.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. Maak een Azure op ruimten gegevens gateway-bron

Eenmaal is geïnstalleerd, moet u uw abonnement Azure koppelen aan de gegevens in de lokalen gateway.

1. Meld u aan bij het hetzelfde werk of school e-mailadres dat is gebruikt tijdens de installatie van de gateway met Azure
1. Klik op de knop resource **Nieuw**
1. Zoek en selecteer de **On-premises gegevensgateway**
1. De informatie voor het koppelen van de gateway aan uw account - Selecteer de juiste **Naam van de installatie** voltooien

    ![On-Premises Gateway gegevensverbinding][1]
1. Klik op de knop **maken** om het maken van de bron

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3. een logica app verbinding maken in de ontwerpfunctie voor

Nu uw Azure abonnement is gekoppeld aan een exemplaar van de gateway van de gegevens in de lokalen, kunt u een verbinding met dit uit in een app logica.

1. Opent een app logica en kiest u een verbindingslijn die op gebouwen-(met ingang van dit schrijven, SQL Server connectiviteit)
1. Vink het selectievakje **verbinding maken via op ruimten gegevensgateway**

    ![Logica App Designer-Gateway maken][2]
1. Selecteer de **Gateway** verbinding maken met en voer eventuele andere informatie nodig
1. Klik op **maken** om de verbinding te maken

De verbinding moet nu worden geconfigureerd voor gebruik in uw app logica.  

## <a name="next-steps"></a>Volgende stappen
- [Algemene voorbeelden en scenario's voor apps logica](app-service-logic-examples-and-scenarios.md)
- [Enterprise integration-functies](app-service-logic-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/app-service-logic-gateway-connection/createblade.PNG
[2]: ./media/app-service-logic-gateway-connection/blankconnection.PNG
[3]: ./media/app-service-logic-gateway-connection/checkbox.PNG