<properties
    pageTitle="Azure Diagnostics versiegeschiedenis"
    description="Verklaring van wijzigingen in de verschillende versies van Azure diagnostics als verzonden met verschillende versies van Microsoft Azure SDK's."
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="robb"/>


# <a name="microsoft-azure-diagnostics-version-history"></a>Microsoft Azure Diagnostics versiegeschiedenis

Nieuwe Azure Diagnostics? Zie [overzicht van Azure diagnostische gegevens](azure-diagnostics.md).

Elke versie van Azure SDK wordt meestal geleverd met een nieuwe versie van Azure diagnostische gegevens. De volgende tabel bevat de Azure SDK en Azure diagnose-versies die zijn gekoppeld aan de SDK release.



Azure versie van SDK | Azure Diagnostics versie | Model
--- | --- | ---
1.x      | 1.0 | invoegtoepassing
2.0-2.4| 1.0 | "
2.5      | 1.2 | extensie
2.6      | 1.3 | "
2.7      | 1.4 | "
2.8      | 1.5 | "


De meest recente versie is 1.5, dat wordt geleverd bij Azure SDK 2.8. De versie van Azure Diagnostics-extensie die wordt geleverd met de SDK wordt alleen gebruikt voor scenario's voor lokale emulator. Een gedistribueerde toepassing wordt automatisch de meest recente versie gebruikt waarop in Azure, ongeacht welke versie van de toepassing van de SDK is gebouwd met. Echter, tenzij u de meest recente Azure SDK installeert, u hebt niet alle nieuwe functies maken gebruik van de tools die u helpen.

Verschillende functies en wijzigingen die hieronder worden beschreven.

## <a name="azure-sdk-28"></a>Azure SDK 2.8
Azure SDK 2.8 diagnostische gegevens verzenden naar [Toepassing inzichten](./application-insights/app-insights-cloudservices.md) makkelijker problemen op te sporen in uw toepassing, alsmede het niveau van de infrastructuur en het systeem toegevoegd.

## <a name="azure-26-diagnostics-changes"></a>Azure 2.6 diagnostics wijzigingen

Azure SDK 2.6 Cloud Service projecten in Visual Studio, zijn de volgende wijzigingen aangebracht. (Deze wijzigingen ook van toepassing op nieuwere versies van Azure SDK.)

- De lokale emulator biedt nu ondersteuning voor diagnostische gegevens. Dit betekent dat u kunt diagnostische gegevens te verzamelen en dat uw toepassing de juiste traces maakt tijdens het ontwikkelen en testen in Visual Studio. De verbindingsreeks `UseDevelopmentStorage=true` kunt u het verzamelen van diagnostische gegevens terwijl u uw cloud service-project in Visual Studio werkt met behulp van de emulator Azure opslag. Alle diagnostische gegevens worden verzameld in de opslag (opslag ontwikkeling).

- Diagnostische gegevens opslag account verbindingsreeks (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) opnieuw opgeslagen in het configuratiebestand van service (.cscfg). In Azure SDK 2.5 is de diagnostische gegevens opslag rekening opgegeven in het bestand diagnostics.wadcfgx.

Er zijn enkele belangrijke verschillen tussen hoe de verbindingsreeks in Azure SDK 2.4 en eerder heeft gewerkt en hoe het werkt in Azure SDK 2.6 en hoger.

- In Azure SDK 2.4 en eerder is de verbindingsreeks gebruikt als een runtime door de invoegtoepassing diagnostische gegevens om de gegevens van de opslag voor het overbrengen van diagnostische logboeken.

- In Azure SDK 2.6 en hoger wordt de verbindingsreeks van de diagnostische gegevens van de uitbreiding van diagnostische gegevens configureren met de gegevens van de juiste opslag tijdens publicatie door Visual Studio gebruikt. De verbindingsreeks, kunt u verschillende opslag instellen voor verschillende configuraties die Visual Studio wordt gebruikt bij het publiceren. Omdat de invoegtoepassing diagnostics (na Azure SDK 2.5) niet langer beschikbaar is, inschakelen niet het bestand .cscfg op zichzelf echter de extensie Diagnostics. U moet de extensie via hulpprogramma's zoals Visual Studio of PowerShell afzonderlijk inschakelen.

- De uitvoer van het pakket van Visual Studio bevat ook de openbare configuratie XML voor de uitbreiding van de diagnostische gegevens voor elke rol ter vereenvoudiging van het proces van de uitbreiding van diagnostische gegevens configureren met PowerShell. Visual Studio gebruikt de verbindingsreeks diagnostische gegevens aanwezig zijn in de configuratie van openbare gegevens van de opslag te vullen. De openbare configuratiebestanden worden gemaakt in de map extensies en PaaSDiagnostics van het patroon volgen. <RoleName>. PubConfig.xml. Alle implementaties op basis van PowerShell kunnen dit patroon gebruiken om elke configuratie toewijzen aan een rol.

- De verbindingsreeks in het bestand .cscfg wordt ook gebruikt door de Azure portal toegang krijgen tot de diagnostische gegevens, zodat het kan worden weergegeven op het tabblad **controle** . De verbindingsreeks is nodig voor het configureren van de service kunt u uitgebreide controle gegevens weergeven in de portal.

### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migratie projecten op Azure SDK 2.6 en hoger

Bij het migreren van Azure SDK 2.5 naar Azure SDK 2.6 of hoger, als er een diagnostische gegevens opslag account die is opgegeven in het bestand .wadcfgx, vervolgens blijft er. Als u wilt profiteren van de flexibiliteit van opslag van verschillende rekeningen gebruiken voor van verschillende opslagconfiguraties, moet u handmatig de verbindingstekenreeks aan uw project toevoegen. Als u een project van Azure SDK 2.4 en eerder naar Azure SDK 2.6 migreert bent, worden tekenreeksen voor de diagnostische gegevens behouden. Echter, Let op de wijzigingen in hoe tekenreeksen worden behandeld in Azure SDK 2.6 zoals opgegeven in de vorige sectie.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Hoe Visual Studio bepaalt de diagnostische gegevens opslag account

- Als u een verbindingsreeks diagnostics is opgegeven in het bestand .cscfg, Visual Studio wordt gebruikt voor het configureren van de uitbreiding van de diagnostische gegevens publiceren als bij het genereren van de openbare XML-configuratiebestanden tijdens de verpakking.

- Als u geen verbindingsreeks diagnostics is opgegeven in het bestand .cscfg, vervolgens valt Visual Studio terug op de uitbreiding van de diagnostische gegevens publiceren en de openbare XML-configuratiebestanden worden gegenereerd wanneer de verpakking te configureren met behulp van de opslag-account die is opgegeven in het bestand .wadcfgx.

- De verbindingsreeks diagnostische gegevens in het bestand .cscfg heeft voorrang op de rekening van de opslag in het bestand .wadcfgx. Als een verbindingsreeks diagnostics is opgegeven in het bestand .cscfg, Visual Studio die wordt gebruikt en negeert de rekening opslag in .wadcfgx.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>De betekenis van de "Update ontwikkeling opslag verbindingsreeksen..." CheckBox doen?

Het selectievakje voor de **Update ontwikkeling opslag verbindingsreeksen voor diagnostiek en Caching met Microsoft Azure opslag referenties wanneer u publiceert naar Microsoft Azure** biedt u een handige manier om eventuele verbindingstekenreeksen ontwikkeling opslag account bijwerken met de account Azure opslag tijdens publicatie.

Stel bijvoorbeeld dat u dit selectievakje inschakelt en de verbindingsreeks diagnostische gegevens `UseDevelopmentStorage=true`. Wanneer u het project naar Azure publiceren, worden Visual Studio de verbindingsreeks diagnostische gegevens automatisch bijgewerkt met de opslag-account die u hebt opgegeven in de wizard publiceren. Echter als een echte opslag-account als de diagnostische gegevens verbindingsreeks is opgegeven, wordt dat account gebruikt in plaats daarvan.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnostische functies verschillen tussen Azure SDK 2.4 en eerder en Azure SDK 2.5 en hoger

Als u uw project vanuit Azure SDK 2.4 Azure SDK 2.5 of hoger overschakelt, moet u voorzien in het achterhoofd de volgende diagnostische functies verschillen.

- **Configuratie-API's zijn afgeschaft** – programma configuratie van diagnostische gegevens beschikbaar in Azure SDK 2.4 of eerdere versies, maar is afgeschaft in Azure SDK 2.5 en hoger. Als u de configuratie van de diagnostische gegevens die momenteel in code is gedefinieerd, moet u het opnieuw configureren van deze instellingen geheel in het gemigreerde project om diagnostische gegevens blijven werken. Het configuratiebestand van de diagnostische gegevens voor Azure SDK 2.4 is diagnostics.wadcfg en diagnostics.wadcfgx voor Azure SDK 2.5 en hoger.

- **Diagnostische toepassingen voor cloud-service kan alleen worden geconfigureerd op het niveau van de rol niet op het niveau van het exemplaar.**

- **Telkens wanneer u uw app, implementeert de diagnostische configuratie wordt bijgewerkt** : dit kan pariteit problemen veroorzaken als u de configuratie van de diagnostische gegevens van Server Explorer wijzigt en vervolgens implementeren van uw app.

- **Azure SDK 2.5 en hoger, loopt vast dumpen in het configuratiebestand van de diagnostische gegevens niet in de code zijn geconfigureerd** : als u crashdumps geconfigureerd in de code hebt, hebt u de configuratie van code handmatig overbrengen naar het configuratiebestand, omdat de crashdumps worden niet tijdens de migratie naar Azure SDK 2.6 overgebracht.
