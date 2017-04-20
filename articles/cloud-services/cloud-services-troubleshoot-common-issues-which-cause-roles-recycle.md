<properties
   pageTitle="Veelvoorkomende oorzaken van Cloud Service rollen recycling | Microsoft Azure"
   description="Een cloud service rol die plotseling wordt gerecycled kan leiden tot aanzienlijke uitvaltijd. Hier volgen enkele problemen die ervoor zorgen dat de rollen om te worden gerecycled, zodat u minder uitval."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="common-issues-that-cause-roles-to-recycle"></a>Veelvoorkomende problemen die ervoor zorgen dat de rollen te recyclen

In dit artikel worden enkele van de voorkomende oorzaken van problemen met implementatie beschreven en vindt u tips voor het oplossen van deze problemen. Een indicatie dat er een probleem met een toepassing is is wanneer de rol-exemplaar niet start, of deze cycli tussen de lidstaten bij het initialiseren, bezet en stoppen.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Ontbrekende runtime afhankelijkheden

Als een rol in uw toepassing afhankelijk van een is assembly die geen deel uitmaakt van .NET Framework of de Azure beheerde bibliotheek, moet u expliciet assembly opnemen in het toepassingspakket. Houd er rekening mee dat andere kaders van Microsoft zijn niet standaard beschikbaar op Azure. Als een dergelijk kader is afhankelijk van uw rol, moet u de assembly's toevoegen aan het toepassingspakket.

Voordat u samenstellen en verpakken van uw toepassing, Controleer het volgende:

- Als u Visual studio, moet dat de **Lokale kopie** van de eigenschap is ingesteld op **True** voor elke assembly waarnaar wordt verwezen in uw project die geen deel uitmaakt van de SDK Azure of het .NET Framework.

- Zorg ervoor dat het web.config-bestand verwijst niet naar alle ongebruikte assembly's in het element compilatie.

- De **Build Action** van elk bestand .cshtml is ingesteld op de **inhoud**. Dit zorgt ervoor dat de bestanden correct worden weergegeven in het pakket en kunnen andere bestanden waarnaar wordt verwezen in het pakket weergegeven.

## <a name="assembly-targets-wrong-platform"></a>Assembly doelen onjuist platform

Azure is een 64-bits omgeving. .NET-assembly's opgesteld voor het doel van een 32-bits werkt daarom niet op Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Rol van niet-verwerkte uitzonderingen genereert tijdens het initialiseren of stoppen

Eventuele uitzonderingen die worden gegenereerd door de methoden van de klasse [RoleEntryPoint] , inclusief de [OnStart], [OnStop]en methoden [worden uitgevoerd] , zijn niet-verwerkte uitzonderingen. Als een niet-verwerkte uitzondering in een van deze methoden optreedt, wordt de rol van recyclen. Als de functie herhaaldelijk is recycling, activerende deze kan een niet-afgehandelde uitzondering wanneer die deze probeert te starten.

## <a name="role-returns-from-run-method"></a>Functie geeft als resultaat van de methode Run

De methode [Run] is bedoeld voor onbepaalde tijd worden uitgevoerd. Als de programmacode de methode [Run] overschrijft, moet het voor onbepaalde tijd slapen. Als de methode [Run] retourneert, wordt de rol wordt herhaald.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Onjuiste instelling van DiagnosticsConnectionString

Als toepassing gebruikmaakt van Azure diagnostische gegevens, het bestand met de service moet opgeven de `DiagnosticsConnectionString` configuratie-instelling. Deze instelling moet een HTTPS-verbinding met uw account voor opslag in Azure opgeven.

Om ervoor te zorgen dat uw `DiagnosticsConnectionString` instelling juist is voordat u het pakket van uw toepassing implementeert in Azure, controleert u het volgende:  

- De `DiagnosticsConnectionString` punten in te stellen op een geldige account in Azure.  
  Deze instelling verwijst standaard naar de rekening geëmuleerde opslag zodat u deze instelling expliciet wijzigen moet voordat u het pakket van uw toepassing implementeert. Als u deze instelling niet wijzigt, wordt een uitzondering gegenereerd wanneer het exemplaar rol probeert te starten van de diagnostische monitor. Hierdoor kan het exemplaar van de rol te recyclen voor onbepaalde tijd.

- De verbindingsreeks is opgegeven in de volgende [indeling](../storage/storage-configure-connection-string.md). (Het protocol moet worden opgegeven als HTTPS). Vervang *MyAccountName* door de naam van uw account voor opslag en *MyAccountKey* met de toegangstoets:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Als u uw toepassing ontwikkelt met behulp van Azure Tools voor Microsoft Visual Studio, kunt u de [eigenschappenpagina's](https://msdn.microsoft.com/library/ee405486) voor het instellen van deze waarde.

## <a name="exported-certificate-does-not-include-private-key"></a>Geëxporteerd certificaat bevat geen persoonlijke sleutel

De rol van een website onder SSL wordt uitgevoerd, moet u ervoor zorgen dat uw geëxporteerde certificaat de persoonlijke sleutel bevat. Als u het certificaat exporteren via het *Certificaatbeheer van Windows* , moet u **Ja** selecteert voor de optie **de persoonlijke sleutel exporteren** . Het certificaat moet worden geëxporteerd in de PFX-indeling de enige indeling die momenteel worden ondersteund.

## <a name="next-steps"></a>Volgende stappen

Bekijk meer [artikelen probleemoplossing](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) voor cloud services.

Bekijk meer rol recycling van scenario's op de [blog-serie van Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Uitvoeren]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
