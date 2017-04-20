<properties 
    pageTitle="Overzicht van Enterprise Integration | Microsoft Azure App Service | Microsoft Azure" 
    description="Gebruik van de functies van Enterprise Integration business proces van integratie en scenario's met Logic apps inschakelen" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-the-enterprise-integration-pack"></a>Overzicht van de Enterprise Integration Pack

## <a name="what-is-the-enterprise-integration-pack"></a>Wat is de Enterprise Integration Pack?
De Enterprise Integration Pack is de Microsoft cloud-gebaseerde oplossing voor het naadloos business-to-business (B2B)-communicatie inschakelen. Industriestandaardprotocollen zoals [AS2](./app-service-logic-enterprise-integration-as2.md), [X12](./app-service-logic-enterprise-integration-x12.md)en [EDIFACT](./app-service-logic-enterprise-integration-edifact.md) het pack gebruikt voor het uitwisselen van berichten tussen zakelijke partners. Berichten kunnen eventueel worden beveiligd met behulp van digitale handtekeningen en codering. 

Het pack kan organisaties die gebruikmaken van verschillende protocollen en indelingen voor het uitwisselen van elektronische berichten door verschillende indelingen te transformeren naar een indeling die beide organisaties hebben systemen kunnen interpreteren en actie ondernemen. 

Als u bekend met BizTalk Server of Microsoft Azure BizTalk-Services bent, zult u deze eenvoudig te gebruiken dat omdat de meeste van de concepten vergelijkbaar zijn is uitgerust met de integratie in de onderneming. Een belangrijk verschil is dat Enterprise Integration integratie accounts gebruikt voor het vereenvoudigen van de opslag en het beheer van artefacten in B2B-communicatie wordt gebruikt. 

Conceptueel, is de Enterprise Integration Pack gebaseerd op de **integratie van rekeningen** die de artefacten die kunnen worden gebruikt voor het ontwerpen, implementeren en onderhouden van uw B2B apps opslaan. Een account integratie is in principe een cloud-gebaseerde container waarin u artefacten, zoals schema's, partners, certificaten, kaarten en overeenkomsten wilt opslaan. Deze artefacten kunnen vervolgens worden gebruikt in de logica apps B2B werkstromen te bouwen. Voordat u de onderdelen in een logica app kunt, moet u uw integratie-account te koppelen aan uw app logica. Na koppelen, wordt uw app logica hebben toegang tot artefacten van de integratie-account.  

## <a name="why-should-you-use-enterprise-integration"></a>Waarom zou u de integratie in de onderneming gebruiken?
- Met enterprise integratie kan u alle onderdelen van uw opgeslagen op één locatie uw account integratie wordt. 
- U kunt gebruikmaken van de logica apps engine en alle verbindingslijnen naar B2B werkstromen te bouwen en te integreren met 3de partij SaaS-toepassingen, toepassingen in gebouwen, evenals aangepaste toepassingen
- U kunt ook gebruikmaken van Azure functies

## <a name="how-to-get-started-with-enterprise-integration"></a>Hoe kan ik aan de slag met integratie in de onderneming?
U kunt bouwen en beheren B2B apps met de Enterprise Integration Pack via de ontwerpfunctie logica apps op **Azure portal**.  

U kunt ook [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "logica apps PowerShell onderwerpen") gebruiken voor het beheren van uw apps logica. 

Hier volgt een overzicht van de stappen die u nemen moet voordat u toepassingen in de portal Azure maken kunt: ![afbeelding overzicht](./media/app-service-logic-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Wat zijn enkele algemene scenario's?

Integratie in de onderneming ondersteunt deze industrienormen:   

- EDI - Electronic Data Interchange  
- EAI - Enterprise Application Integration  

## <a name="heres-what-you-need-to-get-started"></a>Hier is wat u nodig hebt om aan de slag
- Met een account integratie een Azure-abonnement
- Visual Studio 2015 voor het maken van plattegronden en schema 's
- [Microsoft Azure logica Apps Enterprise Integration Tools voor Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it"></a>Probeer het nu
[Probeer het nu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) een voorbeeld van een volledig operationele AS2 implementeren & verzenden ontvangen logica app die gebruikmaakt van de B2B-functies van Apps logica.

## <a name="learn-more-about"></a>Meer informatie over:
- [Overeenkomsten] (./app-service-logic-enterprise-integration-agreements.md "Meer informatie over enterprise integratie overeenkomsten")
- [Scenario's voor Business to Business (B2B)] (./app-service-logic-enterprise-integration-b2b.md "Informatie over het maken van apps logica met B2B-functies")  
- [Certificaten] (./app-service-logic-enterprise-integration-certificates.md "Meer informatie over enterprise integration certificaten")
- [Plat bestand codering en decodering] (./app-service-logic-enterprise-integration-flatfile.md "Meer informatie over het coderen en decoderen van platte bestandsinhoud")  
- [Integratie-accounts] (./app-service-logic-enterprise-integration-accounts.md "Meer informatie over de integratie van accounts")
- [Plattegronden] (./app-service-logic-enterprise-integration-maps.md "Meer informatie over de integratie in de onderneming worden toegewezen")
- [Partners] (./app-service-logic-enterprise-integration-partners.md "Meer informatie over enterprise integratiepartners")
- [Schema 's] (./app-service-logic-enterprise-integration-schemas.md "Meer informatie over enterprise integration schema 's")
- [Validatie van XML-bericht] (./app-service-logic-enterprise-integration-xml.md "Meer informatie over het valideren van XML-berichten met logica apps")
- [XML-transformatie] (./app-service-logic-enterprise-integration-transform.md "Meer informatie over de integratie in de onderneming worden toegewezen")
- [Enterprise Integration verbindingslijnen] (../connectors/apis-list.md "Meer informatie over enterprise integration pack verbindingslijnen")



