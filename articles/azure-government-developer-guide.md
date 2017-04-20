<properties 
    pageTitle="Azure overheid Developers Guide" 
    description="Dit zorgt voor een vergelijking van functies en hulp op het ontwikkelen van toepassingen voor de overheid Azure" 
    services="" 
    cloud="gov"
    documentationCenter="" 
    authors="Joharve2" 
    manager="Chrisnie" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="azure-government" 
    ms.date="10/29/2015" 
    ms.author="jharve"/>


#  <a name="microsoft-azure-government-developer-guide"></a>Handleiding voor ontwikkelaars van Microsoft Azure overheid 

<p> Microsoft Azure overheid is een fysiek en geïsoleerd netwerk-exemplaar van Microsoft Azure.  In deze handleiding voor ontwikkelaars zal geven informatie over de verschillen die softwareontwikkelaars en beheerders nodig om te communiceren en te werken met deze afzonderlijke gebieden van Azure.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## <a name="in-this-topic"></a>In dit onderwerp


+ [Overzicht](#Overview)
+ [Richtlijnen voor ontwikkelaars](#Guidance)
+ [Functies die momenteel in Microsoft Azure overheid](#Features)
+ [Endpoint-toewijzing](#Endpoint)
+ [Volgende stappen](#next)


## <a name="Overview"></a>Overzicht

Microsoft Azure overheid is een afzonderlijk exemplaar van de Microsoft Azure service adressering van de behoeften voor beveiliging en naleving van de Amerikaanse federale agentschappen, regionale en lokale overheden en hun leveranciers van oplossingen. Azure overheid biedt fysieke en isolatie van niet-Amerikaanse overheid implementaties netwerk en afgeschermde Amerikaanse personeel. 

Microsoft biedt een aantal hulpmiddelen maken en implementeren van cloud-toepassingen van Microsoft global Microsoft Azure service ("Global Service") en Microsoft Azure overheidsdiensten.

Bij het maken en distribueren van toepassingen voor de ontwikkelaars van Azure overheidsdiensten, in plaats van de algemene Service moet op de hoogte van de belangrijkste verschillen van de twee services.  Specifiek rond instellen en configureren van de programmeeromgeving, eindpunten die toepassingen schrijven en deze kunnen als overheid Azure services te configureren.

De informatie in dit document bevat een overzicht van deze verschillen en vormt een aanvulling op de informatie die op de site van de(http://www.azure.com/gov "Overheid Azure") [Azure overheid]en de [Technische bibliotheek van Microsoft Azure],(http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") op MSDN. Officiële gegevens mogelijk ook beschikbaar op andere locaties, zoals de [Microsoft Azure Trust Center] (https://azure.microsoft.com/support/trust-center/ "Microsoft Azure Trust Center" /), [Azure documentatiecentrum](https://azure.microsoft.com/documentation/) en [Azure Blogs] ('Azure Blogs' https://azure.microsoft.com/blog/ /). 

Deze inhoud is bedoeld voor partners en ontwikkelaars die Microsoft Azure overheid levert.



## <a name="Guidance"></a>Richtlijnen voor ontwikkelaars
Omdat de meeste van de technische inhoud die beschikbaar is op dit moment wordt ervan uitgegaan dat toepassingen zijn ontwikkeld voor de wereldwijde Service dan voor Microsoft Azure overheid, is het belangrijk om ervoor te zorgen dat ontwikkelaars bekend zijn met belangrijke verschillen voor toepassingen die zijn ontwikkeld om te worden gehost in Azure regering.

- Eerst, services en de verschillen tussen functies zijn, betekent dit dat bepaalde functies die in bepaalde gebieden van de wereldwijde Service zijn mogelijk niet beschikbaar in Azure regering.

- Ten tweede voor functies die worden aangeboden in Azure regering, zijn er configuratie verschillen van de wereldwijde Service.  U moet daarom controleren de voorbeeldcode, configuraties en om ervoor te zorgen dat u bouwen en binnen de omgeving van Azure Cloud overheidsdiensten uitvoeren.


## <a name="Features"></a>Functies die momenteel in Microsoft Azure overheid
Azure regering heeft momenteel de volgende services beschikbaar in zowel ons finan IOWA en ons finan VIRGINIA regio's:

- Virtuele Machines
- Cloud Services
- Opslag
- Active Directory
- Scheduler
- Virtueel netwerk
- SQL-Database
- Azure-bestanden
- Mediaservices
- Beheer van netwerkverkeer
- Bus service
- StorSimple
- Cache bestand Vgx.
- Azure back-up
- Automatisering
- ExpressRoute
- enz.

Andere services beschikbaar zijn, en doorlopend meer services worden toegevoegd.  Zie de [pagina van de regio's](https://azure.microsoft.com/regions/#services) die elke beschikbare regio en hun diensten wordt gemarkeerd voor de meest recente lijst van services.  

ONS finan Iowa en ons finan Virginia zijn momenteel de gegevenscentra Azure regering ondersteunen.  Raadpleeg de pagina gebieden boven voor huidige datacenters en services die beschikbaar zijn.

## <a name="Endpoint"></a>Endpoint-toewijzing

De volgende tabel gebruiken om u te helpen bij het toewijzen van openbare Microsoft Azure en SQL Database eindpunten naar specifieke eindpunten Azure regering.


Servicetype|Azure publiek|Azure overheid
---|---|---
Beheerportal|Manage.windowsazure.com|Manage.windowsazure.us
Algemeen|*. windows.net|*. usgovcloudapi.net
Core|*. core.windows.net|*. core.usgovcloudapi.net
Berekenen|*. cloudapp.net|*. usgovcloudapp.net
BLOB-opslag|*. blob.core.windows.net|   *. blob.core.usgovcloudapi.net
Queue Storage|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Tabelopslag|*. table.core.windows.net|*. table.core.usgovcloudapi.net
Servicebeheer|Management.Core.Windows.NET|Management.Core.usgovcloudapi.NET
SQL-Database|*. database.windows.net|*. database.usgovcloudapi.net
Eindpunt voor netwerktaakverdeling ARM|https://Management.Windows.NET|https://Management.usgovcloudapi.NET  

* Verwijzen naar [Verificatie Azure Resource Manager aanvragen](https://msdn.microsoft.com/library/azure/dn790557.aspx) voor verificatie via Azure AD ARM

## <a name="next"></a>Volgende stappen

Als u geïnteresseerd bent in het leren meer en over Azure overheid moet gebruikmaken van enkele van de onderstaande koppelingen.

- **[Aanmelden voor een proeflicentie](https://azuregov.microsoft.com/trial/azuregovtrial)**

- **[Aanschaf en toegang tot de overheid Azure](http://azure.com/gov)**

- **[Azure overheid overzicht](/azure-government-overview)**

- **[Azure overheid Blog](http://blogs.msdn.com/b/azuregov/)**

- **[Azure naleving](https://azure.microsoft.com/support/trust-center/compliance/)**

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: storage-whatis-account.md
