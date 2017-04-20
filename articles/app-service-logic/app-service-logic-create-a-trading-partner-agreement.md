<properties 
   pageTitle="Maak een Trading partnerovereenkomst in Azure App Service | Microsoft Azure" 
   description="Trading Partner overeenkomsten maken" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
    ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2016"
   ms.author="rajram"/>

# <a name="creating-a-trading-partner-agreement"></a>Maken van een Trading partnerovereenkomst   

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Handelspartners zijn de entiteiten die betrokken zijn bij B2B (Business-to-Business) communicatie. Als twee partners tot stand brengen van een relatie, is dit bedoeld als een *overeenkomst*. De overeenkomst is gedefinieerd op basis van de mededeling van de twee partners wil bereiken en protocol of specifieke transport is. De verschillende protocollen B2B en transporten ondersteund door Azure App-Service omvatten:

- AS2 (toepasselijkheid Statement 2)
- EDIFACT (VN/Electronic Data Interchange voor beheer, handel en Transport (UN/EDIFACT))
- X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>BizTalk-API Apps die ondersteuning bieden voor B2B-scenario 's
De volgende API Apps inschakelen deze mogelijkheden met een rijke en intuïtieve ervaring in de Portal Azure:


## <a name="biztalk-trading-partner-management-tpm"></a>BizTalk-Partner Management (TPM) handel
- Maken en beheren van Partners, profielen en identiteiten
- Opslag en het beheer van schema's voor EDI
- Opslag en beheer van certificaten (gebruikt in AS2-protocol)
- Maken en beheren van AS2-overeenkomsten
- Maken en beheren van de EDIFACT-overeenkomsten (inclusief batchen aan de kant verzenden)
- Maken en beheren van X12 overeenkomsten (inclusief batchen aan de kant verzenden)

![][1]


## <a name="as2-connector"></a>AS2-Connector
- AS2 overeenkomsten wordt uitgevoerd zoals gedefinieerd in de verwante TPM API App-exemplaar
- Oppervlakken AS2 verwerking/controle-informatie voor het oplossen van problemen


## <a name="biztalk-edifact"></a>BizTalk-EDIFACT
- EDIFACT overeenkomsten wordt uitgevoerd zoals gedefinieerd in de verwante TPM API App-exemplaar
- Oppervlakken EDIFACT verwerking/controle-informatie voor het oplossen van problemen
- Statusbeheer van batches (starten en stoppen) biedt, zoals gedefinieerd in EDIFACT overeenkomst(en) in de verwante TPM API App-exemplaar


## <a name="biztalk-x12"></a>BizTalk-X12
- X12 voert overeenkomsten als omschreven in de bijbehorende TPM API App-exemplaar 
- X12 verwerking/tracking-informatie voor het oplossen van oppervlakken
- Statusbeheer van batches (starten en stoppen) biedt, zoals gedefinieerd in X12 overeenkomst(en) in de verwante TPM API App-exemplaar

Als eerder vermeld vereisen de AS2, X 12 en EDIFACT API Apps een TPM API App werkt zoals verwacht.


## <a name="getting-started"></a>Aan de slag
Trading partner overeenkomsten maken:

1. Een exemplaar van de **BizTalk-Trading Partner beheer** verbindingslijn maken. U moet hiervoor een lege Database SQL functie. Voordat u begint moet u een lege database beschikbaar en gereed voor gebruik.
2. Schema's en certificaten als vereist door de overeenkomsten te uploaden. Dit doen door te bladeren door het exemplaar van de TPM gemaakt en in het gedeelte 'Schema' en/of 'Certificaten' stepping
3. Ga naar het TPM-exemplaar gemaakt en stap in het gedeelte **Partners**
4. Partners naar wens maken. Ook de profiel(en) desgewenst bewerken en de vereiste identiteiten toevoegen
5. Gebruik nu de **overeenkomsten** deel te maken van overeenkomsten. Wanneer u een overeenkomst hebt gemaakt, moet u het protocol dat wordt gebruikt. De overige configuratieopties zijn gebaseerd op het protocol dat u hebt geselecteerd.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png
 
