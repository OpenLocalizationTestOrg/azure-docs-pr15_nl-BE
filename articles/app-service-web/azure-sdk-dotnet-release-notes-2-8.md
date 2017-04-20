
<properties 
   pageTitle="Azure SDK voor .NET 2.8 Release-opmerkingen" 
   description="Azure SDK voor .NET 2.8 Release-opmerkingen" 
   services="app-service\web" 
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
 
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure SDK voor .NET 2.8, punt 2.8.1 en punt 2.8.2

##<a name="overview"></a>Overzicht
 
Dit artikel bevat de release-opmerkingen (met bekende problemen en recente wijzigingen) voor Azure SDK voor .NET 2.8, punt 2.8.1 en punt 2.8.2 vrijgegeven. 

Voor een volledige lijst met nieuwe functies en wijzigingen in deze versie, Zie de aankondiging [Azure SDK 2.8 voor Visual Studio 2013 en Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) . 

##  <a name="azure-sdk-for-net-28"></a>Azure SDK voor .NET 2.8

### <a name="download-azure-sdk-for-net-28"></a>Azure SDK voor .NET 2.8 downloaden

[Azure SDK voor .NET 2.8 voor Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK voor .NET 2.8 voor Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkId=699287)
 
### <a name="net-452-support"></a>4.5.2 .NET ondersteuning 

####<a name="known-issues"></a>Bekende problemen

Azure .NET SDK 2.8 kunt u .NET 4.5.2 Cloud-servicepakketten. Echter 4.5.2 van .NET framework wordt niet geïnstalleerd op de standaard afbeeldingen Gast OS tot januari 2016 Gast OS release. Voordat met 4.5.2 van .NET framework is beschikbaar via een aparte Gast OS releaseversie – November 2015 02. Zie de pagina [Azure Gast OS Releases en SDK Compatibility Matrix](../cloud-services/cloud-services-guestos-update-matrix.md) om bij te houden wanneer de afbeelding wordt uitgebracht.  Nadat de afbeelding November 2015 02 wordt vrijgegeven kunt u de afbeelding te gebruiken die door uw Service Cloud-configuratiebestand (.cscfg)-bestand bij te werken. In de configuratie van de service bestand het kenmerk Versie_besturing van de ServiceConfiguration-element ingesteld op de tekenreeks "WA-GAST-OS-4.26_201511-02 '. Als u ervoor kiest om gebruik te maken van deze afbeelding gebruiken en vervolgens u niet langer automatisch bijwerken van het Gastbesturingssysteem krijgt. Over de automatische updates van de Versie_besturing moet zijn ingesteld op ' * ' en .NET 4.5.2 is alleen beschikbaar via Automatische updates in januari 2016.

###<a name="azure-data-factory"></a>Azure Data Factory

####<a name="known-issues"></a>Bekende problemen 

Tijdens een project **Factory gegevenssjabloon** maken met voorbeeldgegevens, mislukken azure power shellscript als azure power shell-versie op de computer geïnstalleerd na 0.9.8.

Om dit type project maken, moet u [versie van azure power shell 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi)installeren.


### <a name="azure-resource-manager-tools"></a>Azure Resource Manager-hulpprogramma 's 

####<a name="breaking-changes"></a>Overtredingen van de wijzigingen

In deze release voor het werken met de nieuwe Azure PowerShell cmdlets versie 1.0 is die door het project Azure resourcegroep de PowerShell-script bijgewerkt.  Dit nieuwe script werkt niet uit met Visual Studio als u een versie van de SDK voor 2.8.  

Scripts van projecten die zijn gemaakt in eerdere versies van de SDK wordt niet uitvoeren vanuit Visual Studio bij het gebruik van de 2,8 SDK.  Alle scripts blijven werken buiten Visual Studio met de juiste versie van de Azure PowerShell-cmdlets.  

De 2,8 SDK moet versie 1.0 van de Azure PowerShell-cmdlets.  Alle andere versies van de SDK is vereist 0.9.8 van de Azure PowerShell-cmdlets.  Zie [deze](http://go.microsoft.com/fwlink/?LinkID=623011) blog voor meer informatie.

###<a name="web-tools-extensions"></a>Web Tools extensies

####<a name="known-issues"></a>Bekende problemen

De volgende bekende problemen wordt in de volgende release opgelost.

- App Service gerelateerde Cloud en Server Explorer penbeweging voor niet-productieomgevingen (zoals China Azure of Azure Stack klanten) werken niet. Voor klanten in deze gebieden risico, het publicatieprofiel downloaden vanaf de portal Azure zal publiceren mogelijkheid ingeschakeld. Een toekomstige versie herstelt voor Azure China en Stack klanten penbewegingen voor 'Debugger koppelen' en 'Streaming logboeken weergeven'. 
- Klanten kunnen fouten zien tijdens gemaakt wanneer de App inzichten exemplaar waarvan ze implementeren in een ander gebied dan de Oost-Amerikaanse wordt App-Service. In deze scenario's wordt een App Service maken in de portal en het publicatieprofiel downloaden publishing scenario's mogelijk. 

###<a name="azure-hdinsight-tools"></a>Azure HDInsight extra

####<a name="new-updates"></a>Nieuwe updates

- U kunt de component query uitvoeren in het cluster via een HiveServer2 met bijna geen overhead en Zie dat de taak worden vastgelegd in real-time.
- De nieuwe component uitvoering weergave kunt u uw werk verder nader meer informatie vinden en identificeren van potentiële problemen.

Zie voor meer informatie, [Azure SDK 2.8 voor Visual Studio 2013 en Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure SDK voor .NET van punt 2.8.1

### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Bekende problemen met Visual Studio 2013 en Visual Studio 2015
 
1. Geactiveerde WebJob publiceert naar sleuven worden weergeven en de fout en won't set een schema, maar valt de WebJob naar Azure. Klanten die worden van een geplande taak moeten vervolgens kunt de Azure-Portal voor het instellen van het schema voor de WebJob. 
2. Python-klanten kunnen debugger problemen. Service team rolt een correctie voor dit, maar als klanten worden beïnvloed, kunt Microsoft weten in de forums of op de blog van de aankondiging of vrijgeven sectie Opmerkingen. 
3. App Service provisioning fouten treden op klanten in bepaalde gebieden (zoals Zuid-India). Dit komt overeen met de portal en klanten die met dit probleem de Azure portal kunnen gebruiken voor het aanvragen van toegang tot het publiceren naar deze geo-regio's. Zodra ze toegang aanvragen tot deze regio's met behulp van moet de portal inrichten Azure werken. 

##<a name="azure-sdk-for-net-282"></a>Azure SDK voor .NET punt 2.8.2

Na de installatie van de punt 2.8.2 hulpmiddelen voor klanten, kunnen het volgende probleem optreden.         

- Als u van Windows 10 gebruikmaakt en Internet Explorer niet hebt geïnstalleerd, verschijnt het foutbericht 'Internet Explorer kan niet worden gevonden'.
Installeer Internet Explorer met behulp van het dialoogvenster Windows-onderdelen het probleem op te lossen.

Als u dit probleem ziet, gebruikt u de functie verzenden een glimlach om dit te melden.

Zie voor meer informatie [in dit](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) boek.
##<a name="other-updates"></a>Andere updates

Zie voor andere updates, [Azure SDK 2.8 aankondiging boeken](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

##<a name="also-see"></a>Zie ook

[Azure SDK 2.8 aankondiging boeken](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Ondersteuning en pensioengegevens voor Azure SDK voor .NET en API 's](https://msdn.microsoft.com/library/azure/dn479282.aspx)

