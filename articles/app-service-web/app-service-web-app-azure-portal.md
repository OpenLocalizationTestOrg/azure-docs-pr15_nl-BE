<properties
    pageTitle="Verwijzing voor het navigeren in de Azure portal"
    description="Meer informatie over de ervaringen van andere gebruikers voor Service-Web App tussen de portal management en de Azure Portal"
    services="app-service"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/26/2016"
    ms.author="jaime-espinosa"/>

# <a name="reference-for-navigating-the-azure-portal"></a>Verwijzing voor het navigeren in de Azure portal

Azure Websites heten nu [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). We bijwerkt al onze documentatie aan deze naamswijziging en instructies te geven voor de Portal Azure. Totdat dit proces is voltooid, kunt u dit document als een leidraad voor het werken met Web Apps in Azure portal.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 
 
## <a name="the-future-of-the-azure-classic-portal"></a>De toekomst van de klassieke Azure-Portal

Ziet u de huisstijl wijzigingen op de klassieke Azure-Portal, is die portal het proces wordt vervangen door de Portal Azure. Als u de klassieke portal geleidelijk, wordt de focus op de ontwikkeling van nieuwe verschuiven naar de Portal Azure. Alle toekomstige nieuwe functies voor Web Apps komen in de Portal Azure. Start de Azure-Portal gebruiken om te profiteren van de nieuwste en beste dat Web Apps te bieden hebben.

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>Verschillen tussen de Azure klassieke Portal en Portal Azure lay-out

In de klassieke portal worden de Azure services vermeld op de linker kant. Navigatie in de portal voor klassieke volgt waar u van de service starten en navigeren in elk element in een boomstructuur. Deze structuur werkt goed wanneer de onafhankelijke onderdelen beheren. Echter toepassingen gebaseerd op Azure zijn een verzameling van onderling verbonden services en deze structuur is niet echt geschikt voor het werken met verzamelingen van services. 

De Azure portal kunt u eenvoudig toepassingen end-to-end met onderdelen uit meerdere services bouwen. De portal is ingedeeld als *ritten*. Een *reis* is een reeks *blades*, containers voor de verschillende onderdelen. Bijvoorbeeld voor een web app *rit* waarmee u meerdere bladen zoals in het volgende voorbeeld wordt automatische schaling instellen: de **website** blade (dat blade-titel nog niet zijn bijgewerkt voor het gebruik van de nieuwe terminologie), het blad van de **Instellingen** en de **schaal van** blade. In het voorbeeld wordt wordt automatisch schalen ingesteld afhankelijk van CPU-gebruik, dus er ook een **CPU Percentage** -blade is. De onderdelen binnen de *blades* heten *onderdelen*, die er uitzien zoals tegels. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Voorbeeld van de navigatie: maken van een web app.

Nieuwe web apps maakt nog steeds net zo eenvoudig als 1-2-3. De volgende afbeelding toont de klassieke portal en de portal voor naast elkaar om aan te tonen dat niet veel is gewijzigd in het aantal stappen die u nodig hebt voor een web app van en uitgevoerd. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

In de portal kunt u kiezen uit de meest voorkomende web apps, met inbegrip van de galerie met populaire toepassingen, zoals WordPress. Ga naar de [Azure Marketplace]voor een volledige lijst met beschikbare toepassingen.

Wanneer u een webtoepassing maakt, geeft u URL, App serviceplan en locatie in de portal, net als in de klassieke portal. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Bovendien kunt de portal u andere algemene instellingen definiëren. Bijvoorbeeld u [resourcegroepen](../azure-resource-manager/resource-group-overview.md) eenvoudig bekijken en beheren van de verwante bronnen voor Azure. 

## <a name="navigation-example-settings-and-features"></a>Voorbeeld van de navigatie: instellingen en functies

Alle instellingen en functies zijn nu logisch zijn gegroepeerd in een enkele blade, waarin u kunt navigeren.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

U kunt bijvoorbeeld aangepaste domeinen maken door te klikken op **aangepaste domeinen en SSL** in de blade **Instellingen** .

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

Als u een waarschuwing voor een controle instellen, klikt u op **verzoeken en fouten** en vervolgens op **Waarschuwing toevoegen**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Klik op **Diagnostische logboeken** in de blade **Instellingen** zodat diagnostische gegevens.

![](./media/app-service-web-app-azure-portal/Diagnostics.png)
 
Om toepassingsinstellingen te configureren, klikt u op **Instellingen voor toepassing** in de blade **Instellingen** . 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Dan de merknaam, zijn een paar dingen in de portal verwijderd of hernoemd gegroepeerd anders gemakkelijker terugvinden. Hieronder is bijvoorbeeld een schermafbeelding van de desbetreffende pagina voor de app instellingen (**configureren**) in de klassieke portal.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>Meer bronnen

[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er veranderd
* Zie voor een handleiding voor het wijzigen van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)
 
