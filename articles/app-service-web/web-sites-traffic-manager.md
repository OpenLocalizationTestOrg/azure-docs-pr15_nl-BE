<properties
    pageTitle="Azure app webverkeer met Azure verkeer Manager beheren"
    description="Dit artikel bevat samenvattingsgegevens voor Azure verkeer Manager met betrekking tot Azure web apps."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Azure app webverkeer met Azure verkeer Manager beheren

> [AZURE.NOTE] Dit artikel bevat de samenvattingsinformatie voor Microsoft Azure verkeer Manager met betrekking tot Azure App Service Web Apps. Via de koppelingen aan het einde van dit artikel vindt u meer informatie over Azure verkeer Manager zelf.

## <a name="introduction"></a>Inleiding
Azure verkeer Manager kunt u instellen hoe aanvragen van webclients worden verdeeld naar web apps in Azure App-Service. Wanneer web app eindpunten worden toegevoegd aan een serviceprofiel Azure verkeer, Azure verkeer Manager houdt van de status van uw web-apps (wordt uitgevoerd, beëindigd of verwijderd) zodat u kunt zelf bepalen welke van deze eindpunten verkeer moet ontvangen.

## <a name="load-balancing-methods"></a>Tegenrekening methoden laden
Azure verkeer Manager gebruikt drie verschillende load balancing methoden. Deze worden beschreven in de volgende lijst als zij betrekking op Azure web apps hebben.

* **Failover**: hebt u web app klonen in verschillende regio's, u kunt deze methode gebruiken voor het configureren van een web app alle webverkeer client-service en een andere web app configureren in een andere regio om dat verkeer in het geval de eerste web-app niet meer beschikbaar is.

* **Round Robin**: hebt u web app klonen in verschillende regio's, kunt u deze methode verkeer gelijkmatig verdelen over het web apps in verschillende regio's.

* **Prestaties**: prestaties van de methode verkeer op basis van de kortste tijd retour naar clients distribueert. De prestaties-methode kan worden gebruikt voor web apps binnen dezelfde regio of in verschillende regio's.

##<a name="web-apps-and-traffic-manager-profiles"></a>Web Apps en verkeer profielen
Een profiel maken in Azure verkeer Manager dat wordt gebruikt een van de drie balancing methoden die eerder zijn beschreven laden voor het configureren van het besturingselement web app verkeer, en vervolgens de eindpunten (in dit geval web apps) waarvoor u om verkeer aan het profiel wilt toevoegen. Uw web app-status (actief, beëindigd of verwijderd) wordt regelmatig doorgegeven aan het profiel zodat Azure verkeer Manager dienovereenkomstig verkeer kunt sturen.

Als u beheer van Azure verkeer met Azure, houd u rekening met de volgende punten:

* Web app alleen implementaties binnen dezelfde regio biedt Web Apps al voor failover en round robin-functionaliteit, ongeacht de webmodus-app.

* Voor installaties in dezelfde regio met Web Apps in combinatie met een andere Azure cloud-service gebruiken, kunt u beide typen eindpunten aan de hybride scenario's combineren.

* U kunt slechts één eindpunt van web app per regio in een profiel opgeven. Wanneer u een web app als een eindpunt voor één regio selecteert, worden de resterende web apps in dat gebied niet beschikbaar voor selectie voor dit profiel.

* De eindpunten van web app die u in een serviceprofiel Azure verkeer opgeeft wordt weergegeven onder het gedeelte **Domeinnamen** op de pagina configureren voor het web app in het profiel, maar wordt er niet zijn geconfigureerd.

* Nadat u een web app aan een profiel toevoegt, wordt de **URL van de Site** op het Dashboard van de web-app portal-pagina aangepaste domein URL van de web app weergegeven als u een hebt ingesteld. Anders wordt het verkeer Manager profiel URL wordt weergegeven (bijvoorbeeld, `contoso.trafficmgr.com`). Zowel de directe domeinnaam van de web app en de URL verkeer Manager zijn zichtbaar op de pagina van de web-app configureren in de sectie **Domeinnamen** .

* Uw aangepaste domeinnamen werkt zoals verwacht, maar naast deze toe te voegen aan uw web-apps, moet u ook de DNS-kaart om te verwijzen naar de URL van verkeer Manager configureren. Zie [een aangepaste domeinnaam voor een Azure website configureren](web-sites-custom-domain-name.md)voor meer informatie over het instellen van een aangepast domein voor een Azure web app.

* U kunt alleen web apps die in de standaardmodus naar een Azure verkeer Manager profiel toevoegen.

## <a name="next-steps"></a>Volgende stappen

Zie [Beheer van netwerkverkeer-overzicht](../traffic-manager/traffic-manager-overview.md)voor een overzicht conceptueel en technisch Manager van Azure verkeer.

Zie blogberichten [Met behulp van Azure verkeer Manager met Azure websites](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) en [Azure verkeer Manager kan nu worden geïntegreerd met Azure websites](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/)voor meer informatie over het gebruik van beheer van verkeer met Web Apps.
