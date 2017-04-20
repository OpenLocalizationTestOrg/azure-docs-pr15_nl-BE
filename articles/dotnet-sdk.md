<properties
    pageTitle="Wat is de Azure .NET SDK"
    description="Informatie over wat is opgenomen in de SDK voor .NET Azure."
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="what-is-the-azure-sdk-for-net"></a>Wat is de Azure SDK voor .NET?

## <a name="overview"></a>Overzicht

De Azure SDK voor .NET is een set hulpprogramma's van Visual Studio, opdrachtregelprogramma's, runtime binaries en clientbibliotheken die u helpen bij het ontwikkelen, testen en distribueren van toepassingen die worden uitgevoerd in Azure. Dit artikel wordt beschreven wat u tijdens de installatie van de SDK. U kunt de SDK downloaden vanaf de [pagina Downloads Azure](https://azure.microsoft.com/downloads/).

De Azure SDK voor .NET omvat ook de [clientbibliotheken voor beslag Azure services](http://go.microsoft.com/fwlink/?LinkId=510472). Deze bibliotheken zijn geïnstalleerd afzonderlijk met behulp van NuGet.

##<a id="included"></a>Wat opgenomen in de Azure SDK voor .NET

De Azure SDK voor .NET installeert de volgende producten:

- [Visual Studio Community Edition 2015](#vwd)
- [Microsoft Azure opslag Emulator](#stgemulator)
- [Hulpprogramma's voor Microsoft Azure opslag](#stgtools)
- [Microsoft Azure Authoring Tools](#auth)
- [Microsoft Azure-Emulator](#emulator)
- [HDInsight-hulpprogramma's voor ODBC-stuurprogramma voor Visual Studio en Microsoft Component](#hdinsight)
- [Microsoft Azure bibliotheken voor .NET](#libraries)
- [Microsoft Azure mobiele App-SDK](#mobile)
- [Microsoft Azure PowerShell](#ps)
- [Microsoft Azure Tools for Microsoft Visual Studio](#tools)
- [Microsoft ASP.NET en Web Tools voor Visual Studio](#wte)
- [Microsoft Azure gegevens meer hulpprogramma's voor Visual Studio](#datalake)

###<a id="vwd"></a>Visual Studio Community Edition 2015

Als u Visual Studio niet op uw computer hebt, wordt de SDK, [Visual Studio Community Edition 2015](https://www.visualstudio.com/products/visual-studio-community-vs)geïnstalleerd.

###<a id="stgemulator"></a>Microsoft Azure opslag Emulator

De [Azure opslag Emulator](http://msdn.microsoft.com/library/hh403989.aspx) gebruikt een exemplaar van SQL Server en het lokale bestandssysteem te simuleren Azure opslag (wachtrijen, tabellen, BLOB's), zodat u lokaal kunt testen.

###<a id="stgtools"></a>Hulpprogramma's voor Microsoft Azure opslag

Hiermee installeert u [AzCopy](http://aka.ms/AzCopy), een opdrachtregelprogramma dat u gebruiken kunt om gegevens te verzenden naar en vanuit een account Azure opslag.

###<a id="auth"></a>Microsoft Azure Authoring Tools

Dit omvat het volgende:

* Het [opdrachtregelprogramma CSPack](http://msdn.microsoft.com/library/gg432988.aspx) voor het maken van pakketten voor de implementatie.
* het [opdrachtregelprogramma CSEncrypt](http://msdn.microsoft.com/library/hh404001.aspx) voor het versleutelen van wachtwoorden die toegang hebben tot exemplaren van de rol van cloud service via een verbinding met extern bureaublad.
* Runtime binaries die cloud serviceprojecten vereisen voor communicatie met de runtimeomgeving en voor diagnostische gegevens. Deze binaire bestanden zijn niet beschikbaar in de pakketten NuGet.

###<a id="emulator"></a>Microsoft Azure-Emulator

De [Emulator Azure](http://msdn.microsoft.com/library/dn339018.aspx) overeenkomt met de service cloud omgeving zodat u cloud serviceprojecten lokaal op uw computer testen kunt voordat u deze naar Azure distribueert.

###<a id="hdinsight"></a>HDInsight Tools for Visual Studio en de component Microsoft ODBC-stuurprogramma

Extra HDInsight in Server Explorer kunnen u navigeren component, databases en gekoppelde opslag rekeningen voor HDInsight clusters, tabellen, maken en maken en component query's indienen. Zie [aan de slag met HDInsight Hadoop Tools for Visual Studio](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md)voor meer informatie.

###<a id="libraries"></a>Microsoft Azure bibliotheken voor .NET

Dit omvat het volgende:

* NuGet pakketten voor opslag van Azure Service Bus en Caching die op uw computer zijn opgeslagen, zodat Visual Studio nieuwe wolk serviceprojecten maken terwijl u kunt off line.
* Een Visual Studio invoegtoepassing waarmee projecten [In rol Cache](http://msdn.microsoft.com/library/dn386103.aspx) lokaal uitvoeren in Visual Studio.

###<a id="mobile"></a>Microsoft Azure mobiele App-SDK

Hulpmiddelen voor het werken met [Azure App Service Mobile Apps](app-service-mobile/app-service-mobile-value-prop.md).

###<a id="ps"></a>Microsoft Azure PowerShell

Azure PowerShell kunt u automatiseren [Azure-omgeving maken en implementeren](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything).

###<a id="tools"></a>Microsoft Azure Tools for Microsoft Visual Studio

Hierdoor kunt u werken met Azure bronnen, voornamelijk Cloud Services en virtuele Machines:

* [Maken, openen, en cloud serviceprojecten publiceren](cloud-services/cloud-services-dotnet-get-started.md).
* [Pakketten voor de implementatie voor projecten met een cloud service maken](http://msdn.microsoft.com/library/ff683672.aspx).
* [Azure virtuele Machines maken tijdens het maken van nieuwe webprojecten](virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md).
* [Maakt PowerShell scripts tijdens het maken van nieuwe virtuele machines](http://msdn.microsoft.com/library/dn642480.aspx).
* [Weergeven en beheren van instellingen voor cloud service project in Visual Studio-Projecteigenschappen windows](http://msdn.microsoft.com/library/ee405486.aspx).
* Weergeven en beheren van [cloud-services](http://msdn.microsoft.com/library/ff683675.aspx), [virtuele machines](http://msdn.microsoft.com/library/jj131259.aspx)en [Service Bus](http://msdn.microsoft.com/library/jj149828.aspx) in Server Explorer.
* [Uitvoeren in de foutopsporingsmodus op afstand voor cloud-services en virtuele machines](http://msdn.microsoft.com/library/ff683670.aspx).
* [Resource ingericht met Azure Resource groep implementatieprojecten automatiseren](https://msdn.microsoft.com/library/dn872471.aspx)

###<a id="wte"></a>Extra Service van Microsoft App voor Visual Studio

Hierdoor kunt u werken met Azure-Websites:

* [Azure Websites publiceren webprojecten](app-service-web/web-sites-dotnet-get-started.md).
* [Publiceren console projecten Azure WebJobs toepassing](app-service-web/websites-dotnet-deploy-webjobs.md).
* [Bronnen Azure-Website maken en SQL-Database bij het maken van een nieuwe webproject, of tijdens het publiceren van een webproject](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
* [Maak PowerShell implementatie scripts tijdens het maken van nieuwe Websites](http://msdn.microsoft.com/library/dn642480.aspx).
* [Beheren en oplossen van problemen met Websites in Server Explorer Azure](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement).
* [Uitvoeren in de foutopsporingsmodus op afstand voor Websites en WebJobs](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).

>[AZURE.NOTE] U hoeft niet te installeren de Azure SDK voor .NET gebruiken deze voorzieningen; ze zijn ook opgenomen in Updates voor Visual Studio.

##<a id="datalake"></a>Microsoft Azure gegevens meer hulpprogramma's voor Visual Studio

Zie voor meer informatie [Zelfstudie: U SQL scripts schrijven met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

##<a id="notincluded"></a>Wat is niet opgenomen wanneer u de Azure SDK voor .NET installeren

Er zijn een paar dingen die u kunt voor de ontwikkeling van Azure en worden niet meegenomen bij het installeren van de SDK. De belangrijkste hiervan zijn:

* [Client libraries](http://go.microsoft.com/fwlink/?LinkId=510472).

    De Azure-SDK bevat de clientbibliotheken voor het consumeren van Azure services, maar niet alle zijn geïnstalleerd tijdens de installatie van de SDK. Als de toepassing een client library die de SDK niet hebt geïnstalleerd, kun je deze uit [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Als uw toepassing gebruikmaakt van een clientbibliotheek die de SDK wordt geïnstalleerd, is het verstandig deze bij te werken met de huidige versie op NuGet.org.

    **Lokale kopieën van clientbibliotheken.** De Azure SDK voor .NET naar uw computer gekopieerd de NuGet pakketten voor sommige Azure clientbibliotheken, zoals opslag en Service Bus, cache. Deze clientbibliotheken worden automatisch opgenomen in nieuwe projecten voor cloud-service, zodat de lokale NuGet pakketten Visual Studio om projecten te maken zelfs als u niet met het Internet verbonden bent inschakelen. Clientbibliotheken bijgewerkt zijn over het algemeen vaker dan nieuwe versies van de SDK worden vrijgegeven, zodat de client libraries op NuGet.org vaak zijn huidige dan wat u met de SDK.

    **Project-sjablonen met clientbibliotheken.** Alleen [Azure Cloud Service](cloud-services/cloud-services-dotnet-get-started.md) en Azure App Service [Mobile Apps](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) project-sjablonen opnemen automatisch sommige clientbibliotheken. Voor andere bibliotheken of andere sjablonen, installeert de [client library NuGet pakketten](http://go.microsoft.com/fwlink/?LinkId=510472) die u nodig hebt.

* [Mobiele Apps projectsjablonen](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

    Mobiele Apps sjablonen zijn alleen beschikbaar in Visual Studio 2013 Update 2 en hoger. Ze zijn niet beschikbaar in Visual Studio 2012 of eerdere versies, en niet in Visual Studio 2013 Update 1 of lager, zelfs als u de Azure SDK voor .NET hebt geïnstalleerd.

##<a id="faq"></a>Veelgestelde vragen

- [Veel Azure functies zijn al in Visual Studio. Moet ik de Azure SDK voor .NET installeren?](#azinvs)
- [Ik wil een clientbibliotheek. Heb ik de Azure SDK voor .NET voor het installeren?](#clientlib)
- [Waar vind ik oudere versies van de Azure SDK voor .NET](#olderversions)
- [Wat is het support lifecycle-beleid voor versies van de SDK voor .NET, Azure?](#lifecycle)
- [Gast OS versies is de Azure SDK voor .NET compatibel met?](#guestos)
- [Hoe verwijder ik de Azure SDK voor .NET](#uninstall)

###<a id="azinvs"></a>Veel Azure functies zijn al in Visual Studio. Moet ik de Azure SDK voor .NET installeren?

Het is een goede gewoonte om de SDK installeren als u wilt ontwikkelen voor Azure met de meest recente hulpprogramma's. Als u de SDK liever niet installeert zou, kunt u dit doen als de volgende voorwaarden voldaan wordt:

* U kunt de meest recente [Update voor Visual Studio](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5)hebt geïnstalleerd.
* U ontwikkelt uitsluitend voor Azure Websites of mobiele diensten, niet voor Cloud services of virtuele Machines.
* Uw toepassing maakt geen gebruik van opslag, of opslag wordt gebruikt, maar u hoeft niet de Emulator opslag of het hulpprogramma AzCopy.

###<a id="clientlib"></a>Ik wil een clientbibliotheek. Heb ik de Azure SDK voor .NET voor het installeren?

De SDK installeert clientbibliotheken alleen zodat u cloud serviceprojecten maken kunt zelfs als u niet met het Internet verbonden bent. Huidige clientbibliotheken zijn beschikbaar in pakketten NuGet op [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Zie [Wat is niet opgenomen bij het installeren van de Azure SDK voor .NET](#notincluded) eerder in dit document voor meer informatie.

###<a id="olderversions"></a>Waar vind ik oudere versies van de Azure SDK voor .NET

Voor oudere versies, Zie dat de [SDK voor .NET Azure](https://azure.microsoft.com/downloads/archive-net-downloads/) downloads pagina.

###<a id="lifecycle"></a>Wat is het support lifecycle-beleid voor versies van de SDK voor .NET, Azure?

Zie [Microsoft Azure Cloud Services Support Lifecycle-beleid](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq).

###<a id="guestos"></a>Gast OS versies is de Azure SDK voor .NET compatibel met?

Zie [Azure Gast OS Releases en SDK compatibiliteit Matrix](http://msdn.microsoft.com/library/ee924680.aspx).

###<a id="uninstall"></a>Hoe verwijder ik de Azure SDK voor .NET

Elk item weergegeven in dit artikel onder [Wat wordt opgenomen in de SDK voor .NET van Azure](#included) is een afzonderlijk programma in de lijst met geïnstalleerde programma's in het Configuratiescherm van Windows- **programma's en onderdelen**.  Er is geen manier verwijdert u ze als een groep; u moet elk programma afzonderlijk te verwijderen.

Wanneer u de Azure SDK voor .NET al geïnstalleerd hebt en u een nieuwe versie installeert, is meestal niet nodig voor het verwijderen van de oude versie. In de meeste gevallen werkt het SDK-installatie een bestaand programma in plaats van een nieuwe toevoegt en verlaten van de oude.

Echter, als u verwijderen niet langer-nodig restanten van een eerdere versie wilt, verwijder alleen de programma's die het oudere versienummer en alleen verwijderen als u hetzelfde programma met een nieuwere versie aanwezig is. Bijvoorbeeld na het bijwerken van 2.5 naar 2.6 wordt er versies 2.5 en 2.6 van "Microsoft Azure Tools for Microsoft Visual Studio 2013" en de 2.5-versie kunt verwijderen. U ziet alleen de 2.5-versie van 'Microsoft Azure Authoring Tools' maar het zou niet veilig verwijderen die.

Zorg ervoor dat versienummers in programma-titels in de **programma's en functies** kan misleidend.  SDK versie 2.6 bevat bijvoorbeeld "Microsoft Azure Mobile App SDK V1.0" Als u de SDK voor Visual Studio 2013 en 'Microsoft Azure Mobile App SDK V2.0' voor Visual Studio 2015 installeren; de versie is in dit geval niet de versie van SDK maar een indicatie van welke versie van Visual Studio wordt van toepassing op.

In dit artikel niet wordt vermeld in elk programma dat elke eerdere versie van de SDK Azure is opgenomen; Er zijn andere programma's die u uit eerdere versies van de SDK verwijderen kunt, omdat eerdere versies van de SDK soms opgenomen programma's die zijn weggelaten uit de latere versies. Bijvoorbeeld versie 2.5 installeert "Azure Resource Manager-hulpprogramma's voor Visual Studio" maar dat is niet in de lijst van dit artikel omdat het niet langer weergegeven als een afzonderlijk programma in **programma's en onderdelen**.  In dit artikel worden alleen programma's die zijn opgenomen in de Azure SDK voor .NET versie 2.6.

> **Opmerking:** Sommige programma's die de SDK bevat ook in andere contexten afzonderlijk kan worden geïnstalleerd en mogelijk zelfs als u niet de volledige SDK hoeft nodig. Hetzelfde kan worden voldaan van programma's die zijn geïnstalleerd door eerdere versies van SDK maar zijn weggelaten uit de latere versies van de SDK. Wanneer u programma's verwijdert, worden zorgvuldig om te voorkomen dat iets dat nodig is nog steeds op uw computer te verwijderen.



##<a id="versions"></a>Versies

Zie de [SDK voor .NET versiegeschiedenis Azure](https://azure.microsoft.com/downloads/archive-net-downloads/) pagina om te zien welke versie is bijgewerkt of oudere versies te downloaden.

##<a id="resources"></a>Bronnen

Zie de [pagina Downloads Azure](https://azure.microsoft.com/downloads/)om te downloaden van de huidige Azure-SDK voor .NET of een clientbibliotheek.

Zie voor de Azure SDK voor .NET-broncode clientbibliotheken, inclusief [GitHub.com/Azure](https://github.com/azure/).

Zie voor Azure client bibliotheek documentatie, [Azure .NET verwijzing](https://azure.microsoft.com/documentation/api/).
