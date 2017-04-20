<properties 
   pageTitle="Azure SDK voor .NET 2.5.1 Release-opmerkingen" 
   description="Azure SDK voor .NET 2.5.1 Release-opmerkingen" 
   services="app-service" 
   documentationCenter=".net,nodejs,java" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/10/2016"
   ms.author="juliako"/>


# <a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK voor .NET 2.5.1 Release-opmerkingen

Dit document bevat de release-opmerkingen voor Azure SDK voor .NET 2.5.1 release. 

##<a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK voor .NET 2.5.1 release-opmerkingen

De volgende zijn nieuwe functies en updates in de SDK voor .NET 2.5.1 van Azure.

- Nieuwe features\scenarios die betrekking hebben op **Web Tools Extensions**. 

    - Azure Websites is Azure App Service gewijzigd. Zie voor meer informatie, [Azure App Service en bestaande Azure Services](app-service-changes-existing-services.md).
    - Azure API Apps (voorbeeld)-ondersteuning toegevoegd zodat klanten publiceren van ASP.NET-projecten als API Apps en gebruik vervolgens de toevoegen > Azure API App Client beweging in C# projecten voor het genereren van code op basis van de structuur van de geïmplementeerde API App. 
    - Het knooppunt Websites in Server Explorer is afgeschaft in plaats van het knooppunt Azure App-Service met ondersteuning voor de Resource groep gebaseerde groepering van Azure API Apps en Apps Mobile Web Apps.
    - Mobiele Apps controllers Azure Mobile Apps (voorbeeld)-ondersteuning toegevoegd zodat klanten kunnen maken van nieuwe mobiele Apps projecten toevoegen, de projecten publiceren en foutopsporing op afstand toepassingen.
    - Toevoegen > penbeweging Azure API App-Client biedt nu ondersteuning voor lokale Swagger JSON bestanden, zodat ontwikkelaars Web API NuGets van derden zoals Swashbuckle kunt Swagger genereren of het handmatig maken. Op deze manier client ontwikkelaars kunnen de functies voor het genereren van code te verbruiken eindpunt Swagger in C# projecten gebruiken. 
    - Web App en API App publishing dialoogvensters zijn uitgebreid ter ondersteuning van het begrip Azure Portal resources groeperen en selectie maken van resourcegroepen Azure en serviceplannen voor App in het nieuwe Web App en API App provisioning dialoogvenster worden weergegeven. 
    - Azure API App Server Explorer knooppunten bevatten koppelingen naar de API Apps diepe link in de Portal Azure, evenals andere functies zoals Streaming logboek en foutopsporing op afstand.

    Voor bekende problemen en de huidige beperkingen in [Azure SDK .NET 2.5.1 onderstaande gedeelte](app-service-release-notes.md#known_issues_2_5_1) .


- Nieuwe features\scenarios met betrekking tot **HDInsight-hulpprogramma's** in Visual Studio zijn beschikbaar in deze release. 
    - Validatie van lokale component scripts. Klik op de knop valideren script in de werkbalk om te zien of er eventuele fouten in uw script. 
    - Verbetering van de taken van de component foutopsporing. Nu kunt u fouten opsporen component taken via Logboeken garen in Visual Studio. Als uw toepassing prestatieproblemen, biedt onderzoekt GAREN logboeken nuttige informatie...
    - (Public Preview) Ondersteuning voor component trefwoord automatisch aanvullen en IntelliSense. Om te helpen bij het schrijven van scripts component, HDInsight Tools voor Visual Studio toegevoegd trefwoord automatisch aanvullen en IntelliSense-ondersteuning voor component.
    - Storm-ondersteuning. Nu kunt u HDInsight hulpprogramma's voor Visual Studio voor het ontwikkelen van Storm topologieën/Spouts/Bouten in C#. U kunt vervolgens dienen de ontwikkelde topologie naar een cluster met Storm en de bout-topologie/spout status zien. Kunt u het systeemlogboek in Logboeken en logboeken voor klant oplossen van problemen met uw Storm topologieën/bouten/Spouts. U kunt ook bestaande JAVA-elementen in de Storm op HDInsight.
    
    Zie [aan de slag met HDInsight Hadoop Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md)voor meer informatie.



##<a id="known_issues_2_5_1"></a>Azure SDK voor .NET 2.5.1 bekende problemen en beperkingen

- Azure API Apps wordt weergegeven als een implementatiedoel voor mobiele toepassingen. Web Apps moet de enige bestemming voor mobiele Apps tot een latere versie. 
- Azure API App-aanbod kan leiden tot succes maar af en toe niet de voortgang in het venster activiteit Azure App bijwerken. Tijdelijke oplossing is om de status van de nieuwe Azure API App in de Portal Azure. 
- Bestand > Nieuw Project > API App > F5 ervaring resulteert in een HTTP-fout omdat er geen default/index.html. Tijdelijke oplossing is Blader naar de URL /api/values. 
- Server Explorer pictogrammen weergegeven met tussenpozen, afgevlakte. Dit probleem wordt opgelost VS opnieuw te starten. 
- Als een uitzondering is opgetreden tijdens het inrichten van Web App of API App (zoals fouten quotum overschreden of dubbele Azure API App gateway naam), weergeven de fouten JSON onbewerkte tekst 
- Maken van regelmatig terugkerende projecten problemen bij het inzicht van de toepassing wordt gecontroleerd bij het project maken.
- Soms ontbreekt in de gegenereerde code voor Azure API App Client naamruimten, ze moeten worden opgenomen handmatig (of automatisch geïmporteerd via Visual Studio hints) voor de code te compileren. 
- Mobiele App projecten dienen te worden gepubliceerd op het web apps, maar u moet een site die u hebt gemaakt als een mobiele App in de Portal Azure omdat mobiele App projecten is vereist voor een database kiezen. 
- De startpagina voor mobiele Apps gebruikt de term 'mobiele service' in plaats van "mobiele toepassingen" 
- Mobiele App maken van het project kan tot een minuut duren te maken. 
- Tijdens de API App weer ingericht (in sommige gevallen) een fout terug is van de Azure API als gevolg dat de machtigingen kunnen niet worden ingesteld, terwijl de App API correct is ingericht en klaar voor gebruik is. U kunt met behulp van de Portal Azure machtigingen handmatig instellen.
- Inzichten van de toepassing wordt niet ondersteund op sjablonen API App en mobiele App-sjablonen.
- API App projecten kunnen niet worden gebruikt in combinatie met de Service Cloud projecten.
- API App project-sjablonen zijn alleen beschikbaar in C#.
- Verbruik API App via het menu 'Toevoegen Azure API App Client' context wordt alleen ondersteund in C#.

 
