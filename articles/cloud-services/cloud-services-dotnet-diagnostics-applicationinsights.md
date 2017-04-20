<properties
   pageTitle="Problemen met Cloud-Services met behulp van inzichten van toepassing | Microsoft Azure"
   description="Informatie over het cloud-service-problemen oplossen met behulp van toepassing inzichten om gegevens te verwerken in Azure diagnostische gegevens."
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />


# <a name="troubleshoot-cloud-services-using-application-insights"></a>Toepassing inzichten met Cloud-Services oplossen

[Azure SDK 2.8](https://azure.microsoft.com/downloads/) en Azure diagnostics extensie 1,5 u kunt nu verzenden Azure diagnostische gegevens voor uw Service Cloud rechtstreeks naar de inzichten van toepassing. De verschillende typen logboekbestanden verzameld door Azure diagnostiek, met inbegrip van de toepassingslogboeken van de windows-gebeurtenislogboeken ETW meldt en prestatiemeteritems kunnen nu worden verzonden naar de inzichten van toepassing en weergegeven in de portal Application inzichten UI. Wanneer gebruikt samen met de toepassing inzichten SDK kunt u inzicht te krijgen in de statistieken en die afkomstig zijn van uw toepassing als de systeem- en infrastructuur niveau gegevens afkomstig van Azure diagnostische logboeken bekijken en nu krijgen.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Azure diagnostische functies voor het verzenden van gegevens naar inzichten toepassing configureren

Volg deze stappen om uw cloud service project Azure diagnostische gegevens om gegevens te verzenden naar de inzichten van de toepassing in te stellen.

1) In Visual Studio Solution Explorer met de rechtermuisknop op een rol en selecteer **Eigenschappen** voor het openen van de ontwerper van de rol

![Eigenschappen van Solution Explorer][1]

2) Selecteer het selectievakje **verzenden van diagnostische gegevens toepassing inzichten** in designer rol onder de sectie diagnostische gegevens

![Rol de ontwerper diagnostische gegevens verzenden naar de inzichten van toepassing][2]

3) Klik in het dialoogvenster dat wordt weergegeven de toepassing inzichten Resource selecteren die u wilt de Azure diagnostische gegevens te verzenden. Het dialoogvenster kunt u een bestaande toepassing inzichten bron selecteren uit uw abonnement of handmatig een instrumentation sleutel opgeven voor een resource inzichten van toepassing. Als u een bestaande toepassing inzichten bron geen kunt vervolgens u op door te klikken op de koppeling **een nieuwe bron maken** dat wordt geopend met een browser-venster naar de klassieke Azure portal waar u een toepassingsbron inzichten kunt maken. Zie voor meer informatie over het maken van een bron toepassing inzichten [maken een nieuwe toepassing inzichten resource](../application-insights/app-insights-create-new-resource.md)

![Selecteer inzichten toepassingsbron][3]

4) Nadat u de bron van de inzichten van de toepassing hebt toegevoegd, wordt de sleutel instrumentation voor die resource wordt opgeslagen als een configuratie-instelling voor de service met de naam **APPINSIGHTS_INSTRUMENTATIONKEY**. U kunt deze configuratie-instelling voor elke configuratie van de service of de omgeving wijzigen door een andere configuratie te selecteren in de vervolgkeuzelijst Service-configuratie naar beneden en een nieuwe sleutel instrumentation voor die configuratie op te geven.

![Selecteer de configuratie van de service][4]

De **APPINSIGHTS_INSTRUMENTATIONKEY** configuratie-instelling wordt gebruikt door Visual Studio de extensie van diagnostische gegevens configureren met de juiste toepassing inzichten resourcegegevens tijdens publicatie. De configuratie-instelling is een handige manier van het definiëren van verschillende instrumenten toetsen voor verschillende configuraties. Visual Studio wordt die instelling vertalen en invoegen in de openbare configuratie van diagnostische extensie bij het publiceren. Ter vereenvoudiging van het proces van de uitbreiding van diagnostische gegevens configureren met PowerShell, bevat het pakket ook uitvoeren vanuit Visual Studio de openbare configuratie XML met de juiste toepassing inzichten instrumentatie sleutel opgenomen. De openbare configuratiebestanden worden gemaakt in de map extensies en PaaSDiagnostics van het patroon volgen. <RoleName>. PubConfig.xml. Alle implementaties op basis van PowerShell kunnen dit patroon gebruiken om elke configuratie toewijzen aan een rol.

5) Inschakelen, **verzenden van diagnostische gegevens toepassing inzichten** zal automatisch configureren voor Azure diagnostische functies voor het verzenden van alle prestatiemeteritems en niveau foutenlogboeken die worden verzameld door de agent Azure diagnostische gegevens naar inzichten van toepassing. Als u verder configureren welke gegevens worden verzonden naar de inzichten van toepassing wilt, moet u handmatig bewerken van het bestand *diagnostics.wadcfgx* voor elke rol. Zie [Azure diagnostische gegevens configureren voor het verzenden van gegevens naar inzichten van toepassing](../azure-diagnostics-configure-applicationinsights.md) voor meer informatie over het handmatig bijwerken van de configuratie.

Zodra de Cloud-Service is geconfigureerd voor het verzenden van Azure diagnostische gegevens voor toepassing inzichten die u kan deze implementeren in Azure zoals u gewend bent is en zorg dat de diagnostische gegevens van Azure-extensie ingeschakeld. Zie [een Cloud-service met behulp van Visual Studio](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Azure diagnostische gegevens bekijken in inzichten toepassing
De Azure diagnostische telemetrie wordt weergegeven in de toepassing inzichten bron voor uw cloud-service geconfigureerd.

Hieronder ziet u hoe verschillende Azure diagnostische gegevens over de typen hyperlinks zich bij de toepassing inzichten concepten:  

-  Prestatiemeteritems worden weergegeven als aangepast Metrics in inzichten van toepassing
-  Windows-gebeurtenislogboeken worden weergegeven als de sporen en aangepaste gebeurtenissen in inzichten van toepassing
-  Toepassing Logboeken, logboeken ETW en alle infrastructuur diagnostische logboeken worden weergegeven als de sporen in inzichten van toepassing.

Azure diagnostische gegevens weergeven in de inzichten van toepassing:

- [Metrics Verkenner](../application-insights/app-insights-metrics-explorer.md) gebruiken om te visualiseren eventuele aangepaste prestatiemeters of tellingen van verschillende typen gebeurtenissen voor windows-gebeurtenislogboek.

![Aangepaste Metrics in Explorer Metrics][5]

- Gebruik de [zoekfunctie](../application-insights/app-insights-diagnostic-search.md) kunt u zoeken in de verschillende traceerlogboeken verzonden door Azure diagnostische gegevens. Voor het voorbeeld als u een niet-verwerkte uitzondering in een rol die de rol van vastlopen en recycle die informatie veroorzaakt zou worden weergegeven in het kanaal van de *toepassing* van het *gebeurtenislogboek van Windows*. De zoekfunctie kunt u kijken naar het gebeurtenislogboek van Windows-fout en de volledige stacktrace ophalen voor de uitzondering om te zoeken naar de oorzaak van het probleem.

![Sporen zoeken][6]

## <a name="next-steps"></a>Volgende stappen

- [De SDK toepassing inzichten met uw cloud-service toevoegen](../application-insights/app-insights-cloudservices.md) aan gegevens over aanvragen, uitzonderingen, afhankelijkheden en eventuele aangepaste telemetrie verzenden vanuit uw toepassing. In combinatie met de Azure diagnostische gegevens krijgt u een compleet overzicht van uw toepassing en systeem in dezelfde toepassing inzicht resource.  


<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
