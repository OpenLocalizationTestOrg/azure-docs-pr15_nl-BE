<properties 
   pageTitle="Azure SDK voor .NET 2.6 Release-opmerkingen" 
   description="Azure SDK voor .NET 2.6 Release-opmerkingen" 
   services="app-service/web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

 
# <a name="azure-sdk-for-net-26-release-notes"></a>Azure SDK voor .NET 2.6 Release-opmerkingen

Dit document bevat de release-opmerkingen voor Azure SDK voor .NET 2.6 release. 

Met Azure SDK 2.6 kunt u ontwikkelen cloud service (PaaS) .NET 4.5.2 of .NET 4.6 gericht op voorwaarde dat u het doel van .NET Framework op de rol van Cloud-Service handmatig installeren. Zie [.NET op de rol van een Cloud-Service installeren](http://go.microsoft.com/fwlink/?LinkID=309796).


##<a name="service-bus-updates"></a>Updates van Service Bus

- Hubs van gebeurtenis: 

    - Nu kunt gerichte toegangsbeheer bij het verzenden van gebeurtenissen door extra publisher eindpunt voor gebeurtenis Hubs bloot.
    - Extra stabiliteit en verbetering aan de gebeurtenis Hubs functie toegevoegd.
    - Ondersteuning van Amqp protocol toe te voegen via WebSocket voor messaging en gebeurtenis Hubs.

##<a name="hdinsight-tools-for-visual-studio-updates"></a>Extra HDInsight voor updates voor Visual Studio

- **IntelliSense verbetering**: externe metagegevens suggestie

    HDInsight Tools for Visual Studio ondersteunt nu het ophalen van externe metagegevens wanneer u het onderdeel script bewerkt. Zo kunt u * *selecteren* van*typen * en alle namen van de tabellen worden weergegeven. Ook worden de namen weergegeven nadat u een tabel hebt opgegeven.

- **Ondersteuning voor HDInsight-emulator**

    Extra HDInsight voor Visual Studio ondersteunen nu verbinding maken met de HDInsight-emulator, zodat u kan lokaal component scripts ontwikkelen zonder enige kosten, deze scripts tegen HDInsight clusters wordt uitgevoerd. 

    Raadpleeg [deze handleiding](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409)voor meer informatie.

- **Extra HDInsight voor Visual Studio ondersteuning voor clusters van algemeen Hadoop** (Voorbeeld)

    Extra HDInsight voor Visual Studio ondersteunen nu clusters van algemene Hadoop, zodat HDInsight Tools voor Visual Studio kunt het volgende doen:

    - verbinding maken met uw cluster, 
    - query met verbeterde ondersteuning voor auto-IntelliSense-voltooiing, component schrijven 
    - alle taken weergeven in het cluster met een intuïtieve gebruikersinterface. 

    Raadpleeg [deze handleiding](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409)voor meer informatie.

##<a name="in-role-cache-updates"></a>Updates van de Cache in rol

- **Cache in rol** is gebruikt **Microsoft Azure opslag SDK** versie 4.3 bijgewerkt. Tot nu toe is de **Cache In functie** met behulp van Azure opslag SDK versie 1.7.

    Klanten met Azure SDK 2.5 of hieronder moet bijwerken naar Azure SDK 2.6 en verplaatsen naar de nieuwe versie van Azure opslag SDK. 

    Azure opslag versie 2011-08-18 is op dit moment gepland voor 1 augustus 2016 worden verwijderd. Eventuele migraties van Cache tot de rol van Azure SDK 2.5 of hieronder naar 2.6 moeten voltooid zijn op dit moment. Zie voor meer informatie over het pensioen van Azure opslag versie 2011-08-18, [Microsoft Azure Storage Service versie verwijderen Update: uitbreiding van 2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

>[AZURE.IMPORTANT]Wij zijn 30 November 2016 met pensioen voor Azure-Service beheerd Cache en Azure In rol Cache aankondigen. Wij raden u aan om te migreren naar Azure bestand Vgx. Cache ter voorbereiding van dit pensioen. Zie voor meer informatie over migratie richtlijnen en datums, [aanbieden die Azure Cache is geschikt voor mij?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

##<a name="azure-app-service-tools"></a>Extra Service van Azure App

De volgende items zijn bijgewerkt in de Azure SDK 2.6-versie.

- Azure publicatie uitgebreid met Azure API Apps als doel distributie.
- API Apps inrichten functionaliteit waarmee gebruikers met de functionaliteit voor het maken en inrichten API App.
- Server Explorer aangepast aan de nieuwe App-knooppunt, met het Web, mobiele en API apps gegroepeerd per resourcegroep.
- Penbeweging voor Azure App API-Client toegevoegd aan de meeste C# projecten waarmee automatisch genereren van Swagger ingeschakeld API Apps uitgevoerd in Azure abonnement van de gebruiker toevoegen.
- API Apps tooling en knooppunten in Server Explorer App Service zijn alleen beschikbaar in Visual Studio 2013. 

##<a name="azure-resource-manager-tools-updates"></a>Azure Resource Manager Tools updates

De Azure resource manager tools hebt bijgewerkt met sjablonen voor virtuele Machines, netwerken en opslag. De JSON bewerken ervaring is bijgewerkt met een nieuwe overzichtsweergave voor sjablonen en de mogelijkheid om de JSON-fragmenten met sjablonen bewerken. Sjablonen die worden gedistribueerd vanuit Visual Studio gebruiken een PowerShell script voorzien zijn van het project, zodat alle wijzigingen aangebracht in het script wordt gebruikt door Visual Studio.

##<a name="diagnostics-improvements-for-cloud-services"></a>Verbeteringen van de diagnostische gegevens voor Cloud Services

Azure SDK 2.6 brengt terug ondersteuning voor het verzamelen van diagnostische logboeken in de emulator Azure berekenen en u ze overbrengt naar de opslag van ontwikkeling. Een diagnostische logboeken (met inbegrip van de toepassing trace gebeurtenistracering voor Windows (ETW)-Logboeken, prestatiemeteritems, infrastructuur voor gebeurtenislogboeken in Logboeken en windows-Logboeken) gegenereerd wanneer de toepassing wordt uitgevoerd in de emulator kan worden overgedragen aan ontwikkeling opslag om te controleren of de registratie van diagnostische gegevens op uw lokale computer werkt. 

De diagnostische gegevens opslag account kan nu worden opgegeven in het bestand van de configuratie (.cscfg) om de verschillende diagnostische gegevens opslag rekeningen gebruiken voor verschillende omgevingen te vereenvoudigen. Er zijn enkele belangrijke verschillen tussen hoe de verbindingsreeks in Azure SDK 2.4 en 2.6 van Azure SDK gewerkt. Zie [Diagnostische gegevens voor Azure Cloud Services configureren](http://go.microsoft.com/fwlink/?LinkID=532784)voor meer informatie over het gebruik van de diagnostische gegevens opslag verbinding string en hoe deze van invloed op uw projecten.

##<a name="breaking-changes"></a>Overtredingen van de wijzigingen

###<a name="azure-resource-manager-tools"></a>Azure Resource Manager-hulpprogramma 's 

- Het projecttype **Implementatieprojecten Cloud** beschikbaar in de SDK Azure 2.5 is gewijzigd in **Azure resourcegroep**.
- Type **Wolk implementatieprojecten** van projecten die zijn gemaakt in de SDK Azure 2.5 kan worden gebruikt in de punten 2.6, maar implementeren van de sjabloon vanuit Visual Studio zal mislukken. Echter nog implementeren met de PowerShell-script werkt steeds niet eerder.  Voor meer informatie over het gebruik van Lees **Cloud implementatieprojecten** in 2.6 deze [boeken](http://go.microsoft.com/fwlink/?LinkID=534086).
 
##<a name="known-issues"></a>Bekende problemen

- Het verzamelen van diagnostische logboeken in de emulator is een 64-bits besturingssysteem vereist. Diagnostische logboeken worden niet verzameld wanneer op een 32-bits besturingssysteem wordt uitgevoerd. Dit heeft geen invloed op andere emulator-functionaliteit. 

- Azure SDK 2.6 uitgebracht op 4/29/2015 had twee problemen: 

    - Universal App kan niet worden geladen in Visual Studio 2015 als Azure SDK 2.6 op de computer is geïnstalleerd.
    - Een Cloud Service project foutopsporing zou in Visual Studio 2013 en Visual Studio 2015 waar Visual Studio reageert niet meer en loopt vast bij het weergeven van een dialoogvenster met het bericht "Diagnose configureren voor emulator" mislukken.
    
    Een update van Azure SDK 2.6 is uitgebracht op 18-5/2015. De bijgewerkte versie is 2.6.30508.1601; het bevat oplossingen voor twee problemen die hierboven is beschreven. U kunt aangeven dat het opbouwen van de SDK van het Configuratiescherm -> programma's en onderdelen -> Microsoft Azure-hulpprogramma's voor Microsoft Visual Studio 2013 – v 2.6. De kolom versie wordt het versienummer weergegeven.

    Als u nog steeds de bovenstaande problemen zijn gericht, installeert u de meest recente versie van de SDK Azure 2.6 voor [VS-2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [2013 VS](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) of [VS 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).
 
##<a name="see-also"></a>Zie ook

[Ondersteuning en pensioengegevens voor Azure SDK voor .NET en API 's](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
