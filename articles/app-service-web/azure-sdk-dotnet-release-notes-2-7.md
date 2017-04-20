
<properties 
   pageTitle="Azure SDK voor .NET 2.7 en .NET 2.7.1 Release-opmerkingen" 
   description="Azure SDK voor .NET 2.7 en .NET 2.7.1 Release-opmerkingen" 
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

# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Azure SDK voor .NET 2.7 en .NET 2.7.1 Release-opmerkingen

##<a name="overview"></a>Overzicht

Dit document bevat de release-opmerkingen voor Azure SDK voor .NET 2.7 release. 

Het document bevat ook de release-opmerkingen voor Azure SDK voor .NET 2.7.1 release.

Azure SDK 2.7 wordt alleen ondersteund in Visual Studio 2015 en Visual Studio 2013. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) is dat de laatste ondersteunde SDK voor Visual Studio 2012.

Zie voor gedetailleerde informatie over deze release [Azure SDK 2.7 aankondiging boeken](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) en [boeken van Azure SDK 2.7.1 aankondiging](http://go.microsoft.com/fwlink/?LinkId=623850).

##<a name="azure-sdk-for-net-27"></a>Azure SDK voor .NET 2.7

###<a name="sign-in-improvements-for-visual-studio-2015"></a>Verbeteringen voor Visual Studio 2015 aanmelden

2.7 van Azure SDK voor Visual Studio 2015 ondersteunt de nieuwe identiteit management functies in Visual Studio 2015.  Dit omvat ook ondersteuning voor Azure via op rollen gebaseerd toegangsbeheer, Cloud Solution Providers, DreamSpark en andere account en een abonnement toegang tot accounts.

Opgenomen met Azure SDK 2.7 verbeteringen zijn alleen beschikbaar in Visual Studio 2015. Ondersteuning voor Visual Studio 2013 is opgenomen in de SDK van Azure 2.7.1.


###<a name="mobile-sdk"></a>Mobiele SDK

Sjablonen aan de nieuwste [NuGet pakket](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) en installatieprogramma proces **Mobile Apps** bijgewerkt.

###<a name="service-bus"></a>Bus service 

Algemene bugfixes en verbeteringen. Raadpleeg voor informatie over updates en functies, de release van de nieuwste [Service Bus NuGet](http://www.nuget.org/packages/WindowsAzure.ServiceBus/).

###<a name="hdinsight-tools"></a>Extra HDInsight 

In deze release zijn de volgende updates zijn aangebracht. Deze updates worden in de voorvertoning. Zie [deze blog](http://go.microsoft.com/fwlink/?LinkId=619108)voor meer informatie.

- Grafieken component voor component Tez taken
- Volledige component DML IntelliSense-ondersteuning
- Sjabloon voor ondersteuning van varkens
- Storm-sjablonen voor Azure services

####<a name="breaking-changes"></a>Overtredingen van de wijzigingen

- Oude **Storm** project moet worden bijgewerkt wanneer u deze versie van de hulpprogramma's. Zie [deze blog](http://go.microsoft.com/fwlink/?LinkId=619108)voor meer informatie.
- Visual Studio Web Express wordt niet langer ondersteund. Zie [deze blog](http://go.microsoft.com/fwlink/?LinkId=619108)voor meer informatie.

###<a name="azure-app-service-tools"></a>Extra Service van Azure App

In deze release zijn de volgende updates worden aangebracht in Web Tools Extensions. Zie [deze](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blog voor meer informatie. 

- Ondersteuning voor DreamSpark accounts toegevoegd
- Volledige wijziging aangebracht ter ondersteuning van de nieuwe Azure Resource Management-API's Azure-hulpprogramma 's
- Ondersteuning voor Azure App-Service toegevoegd aan [Cloud Explorer](#cloud_explorer)

####<a name="known-issues"></a>Bekende problemen

Web App implementatie sleuf knooppunten worden niet weergegeven onder het knooppunt sleuven in Server Explorer en Web App implementatie sleuf onderliggende knooppunten onder Cloud Explorer niet laden. Dit probleem is opgelost en voorbereid voor de volgende release van de SDK. 


###<a name="cloud_explorer"></a>Cloud Explorer voor Visual Studio 2015

Cloud Explorer bevat Azure SDK 2.7 voor Visual Studio 2015 waarmee u uw Azure resources weergeven, hun eigenschappen te controleren en belangrijke ontwikkelaars acties uitvoeren uit in Visual Studio. 

Cloud explorer ondersteunt de volgende:

- Resourcegroep en het Type Resource weergaven van uw resources Azure 
- Resources zoeken op naam (beschikbaar in de weergave van het Type Resource)
- Ondersteuning voor abonnementen en bronnen die rol op basis van Access Control (RBAC zijn) toegepast 
- Geïntegreerde actie paneel waarin ontwikkelaars gerichte acties specifiek zijn voor de geselecteerde resources. Voorbeeld: externe foutopsporing koppelen voor virtuele Machines die zijn gemaakt met behulp van de Stack Resource Manager diagnostische gegevens voor virtuele Machines, enz weergeven.
- Geïntegreerde deelvenster met eigenschappen die ontwikkelaars zijn gericht eigenschappen vaak nodig tijdens dev/test 
- Snel overschakelen van de account wilt gebruiken bij het inventariseren van resources (Gebruik de opdracht instellingen op de werkbalk) 
- Het filteren van abonnementen wilt gebruiken bij het inventariseren van resources (Gebruik de opdracht instellingen op de werkbalk) 
- Diepe links naar de Portal Azure voor beheer van resources en resourcegroepen 
 
 
###<a name="azure-resource-manager-tools"></a>Azure Resource Manager-hulpprogramma 's 

De bronnenbeheerder Azure's zijn bijgewerkt om te werken met rol op basis van Access Control RBAC () en nieuwe abonnement typen.  Met deze wijzigingen opgenomen is de mogelijkheid om nieuwe accounts naast klassieke opslag, opslag gebruiken voor het opslaan van artefacten tijdens de implementatie.  

Als u een resourcegroep Azure project uit een eerdere versie van de SDK gebruikt met de SDK 2.7, is een nieuwe implementatiescript implementeren met behulp van een nieuwe account voor opslag in plaats van klassieke opslag nodig.  U wordt gevraagd voordat er wijzigingen worden aangebracht in uw project het nieuwe script toevoegen.  Het oude script zal worden gewijzigd en moet u handmatig alle wijzigingen aanbrengen in het nieuwe script.
 
 
###<a name="storage-explorer-tools"></a>Opslag-Explorer Tools 

- Ondersteuning voor het weergeven van BLOB's toevoegen. Meer info in [deze blog](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
- Ondersteuning voor het weergeven van de rekeningen van de premie opslag via Server Explorer. Server Explorer alleen de weergegeven pagina BLOB's voor premium opslag rekeningen zoals ze de enige ondersteunde type opslag voor premium zijn.

###<a name="azure-data-factory-tools-for-visual-studio"></a>Azure Data Factory Tools voor Visual Studio 

Introductie van **Azure Data Factory Tools** voor Visual Studio. Hieronder vindt u de functies zijn ingeschakeld. Zie [deze blog](http://go.microsoft.com/fwlink/?LinkId=617530) voor meer informatie.

- **Op basis van sjabloon ontwerpen**: gebruik geval selecteren op basis van sjablonen, sjablonen voor verplaatsing of sjablonen te implementeren een oplossing end-to-end integratie en aan de slag met Data Factory snel praktische gegevens verwerken. 
- **Integratie met Solution Explorer voor het ontwerpen en implementeren van Data Factory entiteiten**: maken en implementeren van pijpleidingen en verwante diensten als Visual Studio-projecten. 
- **Integratie met het Diagram weergeven voor visuele interactie tijdens het ontwerpen**: visueel ontwerpen van pijpleidingen en gegevenssets met steun van de weergave Netwerkdiagram. 
- **Integratie met Server explorer voor het bekijken en interactie met reeds geïmplementeerde entiteiten**: Maak gebruik van de Server Explorer bladeren al geïmplementeerd gegevens fabrieken en bijbehorende diensten. Een fabriek geïmplementeerde gegevens of een entiteit (Pipeline, gekoppelde Service, Datasets) importeren in uw project. 
- **JSON bewerken met schemavalidatie en rijke intellisense**: efficiënt configureren en JSON-documenten van Data Factory entiteiten met uitgebreide validatie van intellisense en schema bewerken 
- **Omgeving met meerdere publiceren**: publiceren pijpleidingen dev, test of Prod.-order omgeving geschreven door het maken van afzonderlijke configuratiebestanden voor elke omgeving.
- **Ondersteuning voor gegevensverwerking op basis van varkens, component en .net**: ondersteuning voor varkens en Scripts in Data Factory project component. Ondersteuning voor het verwijzen naar C#-Project voor het beheren van .net activiteit.

##<a name="azure-sdk-for-net-271"></a>Azure SDK voor .NET 2.7.1

De volgende sectie bevat updates die zijn geïntroduceerd in de Azure SDK voor .NET 2.7.1 release.
###<a name="hdinsight-tools"></a>Extra HDInsight 

Zie voor gedetailleerde informatie over HDInsight extra updates [deze blog](http://go.microsoft.com/fwlink/?LinkId=623831).

- Component weergeven van taak-Operator (een nieuwe functie)

    U helpen bij uw query component beter, is de functie voor weergave van Operator component toegevoegd. Overzicht van de operatoren in een hoekpunt, dubbelklikt u op het hoekpunt van de grafiek van de taak. Als u meer details van een bepaalde operator, de muisaanwijzer op die operator.
- Component fout markering (een nieuwe functie)

    Als u de grammaticafouten weergeven direct is de functie fout markering component toegevoegd. Ook de foutberichten zijn verbeterd en nu ziet u gedetailleerde grammaticafouten direct (tot deze release, moest u een script component aan het cluster indienen en wacht enige tijd voordat u meer informatie over het foutbericht).  
- Storm-topologie grafiek (een nieuwe functie)

    Visualiseren is erg belangrijk als u zien wilt of uw topologie werkt zoals verwacht. Visualisatie voor Storm grafieken toegevoegd in deze versie. U kunt de belangrijke maatstaven voor uw topologie visualiseren (bijvoorbeeld een kleur geeft weer een bepaalde bout 'bezet' is of niet). U kunt ook dubbelklikken de bout/Spout om meer details weer te geven.

- Ondersteuning voor clusters met HDInsight die zijn gemaakt in de Portal Azure (een bug fix)

    U kunt nu Visual Studio gebruiken voor het weergeven en verzenden van taken naar uw HDInsight clusters, ongeacht waar het cluster zijn gemaakt.

- Meer IntelliSense-ondersteuning & sneller component metagegevens laden van (verbetering)

    We hebben de IntelliSense verbeterd door meer gebruiksvriendelijke suggesties toe te voegen. Bijvoorbeeld kan tabelalias nu ook worden voorgesteld in IntelliSense zodat u gemakkelijker uw query kunt schrijven. We hebben ook de component metagegevens laden zodat het duurt slechts enkele seconden alle databases, tabellen en kolommen van de component metastore verbeterd.

Zie voor gedetailleerde informatie over HDInsight extra updates [deze blog](http://go.microsoft.com/fwlink/?LinkId=623831).

###<a name="improvements-in-visual-studio-2013"></a>Verbeteringen in Visual Studio 2013

- Azure SDK 2.7.1 kan Visual Studio 2013 toegang tot accounts Azure en abonnementen via rollen gebaseerd toegangsbeheer, Cloud Solution Providers en Dreamspark.
- Met Azure SDK 2.7.1 is het nieuwe venster van Cloud Explorer tool nu ook beschikbaar in Visual Studio 2013.

###<a name="known-issues"></a>Bekende problemen

Installatie van de SDK Azure 2.6 of 2.7.1 voor Visual Studio Community 2013 op een niet - Engels besturingssysteem wordt een waarschuwing weergeven dat de Engelse en niet-Engelstalige bronnen van Visual Studio misschien overeen. Deze waarschuwing kan veilig worden genegeerd. Dit gebeurt alleen als de computer een eerdere installatie van Visual Studio Community 2013 bevat heeft en u de SDK op een niet - Engels besturingssysteem installeert. De waarschuwing wordt weergegeven nadat u het language pack geldt de RTM-bronnen voor Visual Studio, maar voordat de Update 4 van toepassing. Ontslaan van de waarschuwing, kunt het language pack en toepassen van de Update 4-versie van de language pack inhoud voltooien.

LightSwitch projecten zijn niet compatibile met deze release. Dit probleem zal worden opgelost met de volgende versie van de SDK.

##<a name="also-see"></a>Zie ook
[Azure SDK 2.7.1 aankondiging boeken](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7 aankondiging boeken](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Ondersteuning en pensioengegevens voor Azure SDK voor .NET en API 's](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
