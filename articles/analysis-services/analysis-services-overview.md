<properties
   pageTitle="Wat is Azure Analysis Services | Microsoft Azure"
   description="De grote lijnen van Analysis Services ophalen in Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="what-is-azure-analysis-services"></a>Wat is Azure Analysis Services?
![Azure analyseservices](./media/analysis-services-overview/aas-overview-aas-icon.png)

Gebaseerd op de beproefde analytics engine in Microsoft SQL Server Analysis Services en biedt Azure Analysis Services enterprise-grade gegevensmodellering in de cloud.

> [AZURE.IMPORTANT] Azure Analysis Services is een **voorbeeld**. Er zijn enkele dingen die gewoon nog niet werken. Zorg ervoor dat controleren verwachtingen [voorbeeld](#preview-expectations) verderop in dit artikel. En zorg ervoor dat u in het oog houden op de [Azure Analysis Services-blog](https://go.microsoft.com/fwlink/?linkid=830920) voor de meest recente info.

## <a name="built-on-sql-server-analysis-services"></a>Gebouwd op SQL Server analyseservices
Azure Analysis Services is compatibel met de dezelfde SQL Server 2016 Analysis Services Enterprise Edition die u al kent. Azure Analysis Services ondersteunt in tabelvorm modellen op het compatibiliteitsniveau van 1200. Alle partities, DirectQuery, beveiliging, bi-directionele relaties en vertalingen ondersteund.

## <a name="use-the-tools-you-already-know"></a>Gebruik de hulpprogramma's die u al kent
![BI developer tools](./media/analysis-services-overview/aas-overview-dev-tools.png)

Bij het maken van data-modellen voor Azure Analysis Services, gebruikt u dezelfde hulpmiddelen als voor SQL Server Analysis Services. Ontwerpen en implementeren van modellen in tabelvorm met behulp van de nieuwste versies van [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx) of met behulp van sjablonen [Azure Powershell](../powershell-install-configure.md) en [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connect-to-data-sources"></a>Verbinding maken met gegevensbronnen
Gegevensmodellen geïmplementeerd op servers in Azure ondersteuning verbinding maken met gegevensbronnen op-ruimten in uw organisatie of in de cloud. Gegevens uit beide ruimten op combineren en gegevensbronnen voor een hybride BI-oplossing van de wolk.

![Gegevensbronnen](./media/analysis-services-overview/aas-overview-data-sources.png)

Omdat uw server in de cloud, is naadloze verbinding maken met gegevensbronnen wolk. Wanneer zij verbinding maken met gegevensbronnen voor gebouwen, [On-premises gegevensgateway](analysis-services-gateway.md) zorgt voor snelle, beveiligde verbindingen met de Analysis Services-server in de cloud.  

 \*Sommige gegevensbronnen nog niet in de voorbeeldweergave worden ondersteund. Voor meer informatie, Zie [voorbeeld verwachtingen](#preview-expectations) verderop in dit artikel.

## <a name="explore-your-data-from-anywhere"></a>Ontdek uw gegevens overal
Verbinding maken en [gegevens](analysis-services-connect.md) van uw servers vanaf over overal. Azure Analysis Services ondersteunt verbindingen in Power BI-Desktop, Excel, aangepaste toepassingen en browser gebaseerde hulpprogramma's.

![Gegevensweergaven](./media/analysis-services-overview/aas-overview-visualization.png)

 \*Power BI ingesloten wordt nog niet ondersteund in de voorvertoning.

## <a name="secure"></a>Beveiligen

#### <a name="user-authentication"></a>Verificatie van de gebruiker
Gebruikersverificatie voor Azure Analysis services wordt uitgevoerd door [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md). Wanneer u probeert aan te melden een Azure Analysis Services-database, gebruikers de identiteit van een organisatie account gebruiken met toegang tot de database die deze probeert te openen. Deze gebruikersidentiteiten moeten lid zijn van de standaard Azure Active Directory voor het abonnement waar de Azure Analysis Services-server zich bevindt. [Directory-integratie](https://technet.microsoft.com/library/jj573653.aspx) tussen AAD en een Active Directory op locatie is een uitstekende manier om uw locatie op gebruikerstoegang krijgen tot een Azure Analysis Services-database, maar is niet vereist voor alle scenario's.

Gebruikers zich aanmelden met de UPN (user Principal name) van de account en het wachtwoord. Wanneer gesynchroniseerd met een Active Directory op de gebouwen, is de UPN van de gebruiker het vaak hun organisatie-e-mailadres.

Machtigingen voor het beheer van de bron Azure Analysis Services-server worden verwerkt door het toewijzen van gebruikers aan rollen binnen uw abonnement Azure. Standaard hebben administrators abonnement eigenaarsmachtigingen voor de Serverbron in Azure. Extra gebruikers kunnen worden toegevoegd met behulp van bronbeheer Azure.

#### <a name="data-security"></a>Beveiliging van gegevens
Azure Analysis Services wordt gebruikgemaakt van Azure Blob-opslag persistent storage en metagegevens voor Analysis Services-databases maken. Gegevensbestanden in Blob worden gecodeerd met behulp van Azure Blob Server Side codering (SSE). Wanneer u directe Query-modus, worden alleen metagegevens worden opgeslagen; de feitelijke gegevens is toegankelijk vanuit de gegevensbron op moment van de query.

#### <a name="on-premises-data-sources"></a>Gegevensbronnen voor gebouwen
Veilige toegang tot gegevens tijdelijk op-hoofdkwartier in uw organisatie kunnen worden bereikt door het installeren en configureren van een [On-premises gegevensgateway](analysis-services-gateway.md). Gateways bieden toegang tot gegevens voor zowel directe Query-modi in het geheugen. Wanneer een model Azure Analysis Services verbinding met een gegevensbron op gebouwen maakt, wordt een query gemaakt samen met de gecodeerde referenties voor de gegevensbron in de lokalen. De gateway cloud-service de query analyseert en duwt de aanvraag aan een Azure Service Bus. De gateway op de ruimten worden opgevraagd Azure Service Bus voor aanvragen in behandeling. De gateway vervolgens de query haalt, decodeert de referenties en maakt verbinding met de gegevensbron voor de uitvoering. De resultaten worden vervolgens uit de gegevensbron verzonden naar de gateway en vervolgens op de Azure Analysis Services-database.

Azure Analysis Services is onderworpen aan de [Voorwaarden van Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en de [Privacyverklaring van Microsoft Online Services](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
Zie voor meer informatie over beveiliging in Azure, het [Vertrouwenscentrum van Microsoft](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="get-help"></a>Help-informatie opvragen
Azure Analysis Services is eenvoudig te installeren en te beheren. Hier vindt u alle informatie die u nodig hebt voor het maken en beheren van een server hier. Bij het maken van een gegevensmodel implementeren op uw server is grotendeels hetzelfde als voor het maken van een gegevensmodel implementatie van een server op locatie. Er is een uitgebreide bibliotheek met concepten, procedures, zelfstudies en artikelen van de referentie in [Analysis Services op MSDN](https://msdn.microsoft.com/library/bb522607.aspx).

We hebben ook een aantal nuttige video's op [Azure Analysis Services op Channel 9](https://channel9.msdn.com/series/Azure-Analysis-Services).

Snel verandert dingen. U krijgt altijd de meest recente informatie over de [blog Azure Analysis Services](https://go.microsoft.com/fwlink/?linkid=830920).

## <a name="community"></a>Gemeenschap
Analyseservices heeft een levendige Gemeenschap van gebruikers. Deelnemen aan het gesprek op [Azure Analysis Services-forum](https://aka.ms/azureanalysisservicesforum).

## <a name="feedback"></a>Feedback
Hebt u suggesties of verzoeken functie? Zorg ervoor dat uw reacties op [Azure Analysis Services Feedback](https://aka.ms/azureanalysisservicesfeedback).

Hebt u suggesties over de documentatie? U kunt gegevens toevoegen met Disqus onder aan elk artikel.

## <a name="preview-expectations"></a>Voorbeeld verwachtingen
Azure Analysis Services is momenteel in de voorvertoning. Er zijn een paar dingen die u rekening houden met moet.

##### <a name="server-modes"></a>Modi
Azure Analysis Services ondersteunt momenteel in tabelvorm modus voor in tabelvorm modellen op het compatibiliteitsniveau van 1200. Multidimensionaal en Data Mining en Power Pivot voor SharePoint-modus worden niet ondersteund.

##### <a name="data-sources"></a>Gegevensbronnen
Voor een voorbeeld van worden de volgende gegevensbronnen in tabelvorm 1200 modellen worden geïmplementeerd op een Azure Analysis Services-server ondersteund.

| **Wolk** | **In gebouwen** |
|--------------|------------|
| SQL-database | SQL Server |
| SQL datawarehouse | APS |
|      | Oracle |
|       | Teradata |


### <a name="data-source-providers"></a>Leveranciers van gegevensbronnen
Gegevensmodellen in Azure Analysis Services mogelijk verschillende gegevensproviders verbinding maken met gegevensbronnen dan gegevensmodellen in SQL Server Analysis Services. Vereisten voor de provider gegevens afhankelijk van data source wordt in de cloud of op locatie en het type gegevensmodel; in het geheugen of directe Query. Voor meer informatie, Zie de [Datasource-verbindingen](analysis-services-datasource.md).


### <a name="client-connections"></a>Verbindingen van clients
Power BI ingesloten wordt nog niet ondersteund in de voorvertoning.

Met live verbindingen met een Azure Analysis Services-server en opgeslagen op OneDrive of SharePoint Online Excel-werkmappen worden niet ondersteund.



## <a name="next-steps"></a>Volgende stappen
Nu u meer weten over Azure Analysis Services, is het tijd om aan de slag. Meer informatie over het [maken van een server](analysis-services-create-server.md) in Azure en het [implementeren van een model in tabelvorm](analysis-services-deploy.md) op het.
