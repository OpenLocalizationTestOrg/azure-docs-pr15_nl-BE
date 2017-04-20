<properties
    pageTitle="Azure App Service en de gevolgen voor bestaande Azure services"
    description="Wordt uitgelegd hoe de nieuwe Azure App Service en de bijbehorende functies van invloed op bestaande services in Azure."
    services="app-service"
    documentationCenter=""
    authors="yochay"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="yochayk"/>


# <a name="azure-app-service-and-existing-azure-services"></a>Azure App Service en bestaande Azure services

In dit artikel worden de wijzigingen in bestaande Azure services als onderdeel van de samen te brengen verschillende Azure services in [Azure App-Service](https://azure.microsoft.com/services/app-service/), een nieuwe geïntegreerde aanbod.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Overzicht

[Azure](https://azure.microsoft.com/services/app-service/) is App een nieuwe en unieke cloud service waarmee ontwikkelaars voor het maken van web- en mobiele toepassingen voor elk platform en elk apparaat. App-Service is een geïntegreerde oplossing die is ontworpen om herhaalde coding functies te stroomlijnen, onderneming en SaaS-systemen integreren en automatiseren van bedrijfsprocessen bij het voldoen aan uw wensen voor beveiliging, betrouwbaarheid en schaalbaarheid.

App-Service combineert de volgende bestaande Azure services - [Websites](https://azure.microsoft.com/services/websites/), [Mobiele Services](https://azure.microsoft.com/services/mobile-services/)en [BizTalk-Services](https://azure.microsoft.com/services/biztalk-services/) in één gecombineerde service tijdens het toevoegen van krachtige nieuwe mogelijkheden.  App-Service kunt u voor het hosten van de volgende app-typen:

-   Web Apps
-   Mobiele toepassingen
-   API Apps
-   Logica Apps

De volgende tabel wordt uitgelegd hoe bestaande Azure services toewijzen aan de App Service en de app typen beschikbaar zijn binnen het.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Bestaande Azure Service</th>
<th align="left", style="width:10%">Azure App-Service</th>
<th align="left", style="width:80%">Wat is gewijzigd</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure Websites</td>
<td align="left">Web Apps</td>
<td align="left"><li>Voor Azure Websites is App-Service strikt beperkt tot het wijzigen van de naam van de Websites in Web Apps.
<p><li>Alle bestaande exemplaren van Websites zijn nu Web-Apps in App-Service.</p>
<p><li>Toegang tot uw bestaande websites via het <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Portal Azure</a>, waar u alle bestaande sites onder <em>Web Apps</em>kunt vinden.</p>
<p><li><em>Web Hosting Plan</em> is nu <em>App Service Plan</em>. Een <em>App Service Plan</em> kan elk type app App Service, zoals Web, mobiele, Logic of API apps host.</p>
<p><li>Azure App Service Web Apps is in het algemeen beschikbaar.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Meer informatie over het Web Apps</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure mobiele Services</td>
<td align="left">Mobiele toepassingen</td>
<td align="left"><p><li>Mobile-Services blijven beschikbaar zijn als een zelfstandige service zijn en blijven volledig wordt ondersteund.</p>
<p><li>Mobiele Apps is een app in de App-Service die de functionaliteit van mobiele diensten en nog veel meer integreert.</p>
<p><li>Het is eenvoudig om te <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">migreren van mobiele Services voor mobiele Apps</a>.</p>
<p><li>Als onderdeel van de App Service krijgen mobiele Apps nieuwe mogelijkheden buiten mobiele Services, zoals integratie met lokale en SaaS-systemen staging-sleuven, WebJobs en schaalopties beter.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Meer informatie over mobiele Apps</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API Apps</td>
<td align="left">
<p><li>API Apps is een nieuwe app in de App-Service waarmee u gemakkelijk bouwen en gebruiken van API's in de cloud.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">Meer informatie over API Apps</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Logica Apps</td>
<td align="left">
<p><li>Logica Apps is een nieuwe app in App-Service waarmee u gemakkelijk automatiseren bedrijfsproces.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">Meer informatie over Logic Apps</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Azure BizTalk-Services</td>
<td align="left">BizTalk-API Apps</td>
<td align="left">
<li><p>BizTalk-Services blijven beschikbaar zijn als een zelfstandige service zijn en blijven volledig wordt ondersteund.</p>
<li><p>De mogelijkheden van BizTalk-Services zijn geïntegreerd in de App-Service als API Apps waardoor gebruikers voor het uitvoeren van enterprise application integration en scenario's B2B-integratie met de typen app in de App-Service</p>
<li><p>Met logica Apps kunt u nu workflows maken met behulp van een visueel ontwerp bedrijfsprocessen automatiseren</p></td>
</tr>
</tbody>
</table>

Ga voor meer informatie, [documentatie van App-Service](https://azure.microsoft.com/documentation/services/app-service/).
