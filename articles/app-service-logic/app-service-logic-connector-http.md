<properties
   pageTitle="Met de HTTP-Listener en Connector in logica Apps | Microsoft Azure App Service "
   description="Maken en de HTTP-listener- en HTTP-actie Connector of API app configureren en gebruiken in een logica in Azure App Service app"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/31/2016"
   ms.author="prkumar"/>


# <a name="get-started-with-the-http-listener-and-http-action-and-add-it-to-your-logic-app"></a>Aan de slag met de actie in HTTP en HTTP-listener en toevoegen aan uw App logica

> [AZURE.NOTE]We zijn ondersteuning voor deze connector beëindigen omdat de functionaliteit nu standaard als **handmatige trigger opgenomen is** bij het maken van nieuwe logica apps.  We raden u alle apps in de logica die deze connector gebruikt.  
> Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2014-12-01-voorbeeld.

Rechtstreeks aansluiten op http-bronnen voor HTTP-aanvragen luisteren en HTTP-webverzoeken configureren. Er zijn enkele scenario's waarin u werken met directe HTTP-verbindingen moet kan, met inbegrip van:

1.  Voor het ontwikkelen van een App die ondersteuning biedt voor een web of mobiele gebruiker interactieve-front-end-logica.
2.  Voor het ophalen en verwerken van gegevens van een webservice die niet een out van vak connector.
3.  Acties uitvoeren die al weergegeven als een webservice, maar als een API-app niet beschikbaar worden.

Deze scenario's zijn er twee opties:

1. **HTTP-listener**: deze connector fungeert als een trigger en luistert naar HTTP-aanvragen op een geconfigureerde eindpunt. Wanneer een oproep wordt ontvangen op de geconfigureerde eindpunt, wordt een nieuw exemplaar van de stroom en de in de aanvraag voor de stroom voor de verwerking van ontvangen gegevens doorgeeft. Het kan ook worden geconfigureerd om automatisch het binnenkomende verzoek reageren wanneer de stroom is gestart, of kunt u een reactie op basis van de uitvoering van de stroom maken en verzenden van een antwoord aan de aanroepende.
2. **Http-actie**: dit kunt u het configureren van een webaanvraag naar een eindpunt beschikbaar op het internet weer een reactie krijgt en voor aanvullende acties beschikbaar gemaakt in de stroom verbruiken.

Logica apps kunnen worden geactiveerd op basis van verschillende gegevensbronnen en connectors bieden voor het ophalen en verwerken van gegevens als onderdeel van de stroom. De HTTP-connector kunt u toevoegen aan uw zakelijke workflow- en gegevens als onderdeel van deze werkstroom in een App logica. 

## <a name="creating-an-http-listener-for-your-logic-app"></a>Maken van een HTTP-listener voor uw App logica
Een verbindingslijn kan worden gemaakt binnen een logica app of rechtstreeks vanuit de markt Azure worden gemaakt. Een verbindingslijn maakt de markt:  

1. Selecteer in de Azure startboard **Marketplace**.
2. Zoek naar 'HTTP', HTTP-Listener selecteren en selecteer **maken**.
3.  De HTTP-listener als volgt configureren:  
![][1]

4.  Bij het instellen van de pakketinstellingen, ziet u de volgende optie in het automatisch beantwoorden of u van de listener voor het verzenden van een expliciete reactie. Deze optie instellen op **False** om uw eigen antwoord te verzenden:  
![][2]

5.  Klik op **OK** om te maken.
6.  Zodra de API app-exemplaar is gemaakt, opent u de instellingen voor het configureren van de beveiliging. De HTTP-listener ondersteunt momenteel de basisverificatie. U kunt dit configureren met behulp van de optie beveiliging wanneer u de HTTP-listener opent:  
![][3]
  
    **Bekend probleem** *De Security instellingen weergeven 'Geen' als de standaardwaarde, maar niet gedefinieerd is. Moet u de instelling wijzigen om geen voordat het wordt opgeslagen om ervoor te zorgen dat de HTTP-Listener correct is geconfigureerd en Basic.*  

7. Ten slotte stelt u de beveiligingsinstellingen van de API-App publiek (anoniem) zodat externe clients toegang tot het eindpunt. Deze instelling is beschikbaar onder "alle instellingen > Toepassingsinstellingen ' van de HTTP-Listener API App:![][10]

Zodra u dat hebt gedaan, kunt u nu een app logica voor het gebruik van de HTTP-listener.

## <a name="using-the-http-listener-in-your-logic-app"></a>Met behulp van de HTTP-listener in uw App logica
Als uw app API is gemaakt, nu kunt u de HTTP-listener als een trigger voor uw App logica. Hiervoor moet u:

4.  Maak een nieuwe logica App.
5.  Open 'Triggers en acties' als de ontwerper van de logica Apps openen en configureren van de stroom. De HTTP-Listener wordt vermeld in de galerie. Selecteer deze.
6.  Nu kunt u de HTTP-methode en de relatieve URL waarop u de listener voor het starten van de stroom nodig instellen:  
![][4]  
![][5]

7.  Als u de volledige URI, dubbelklikt u op de HTTP-Listener om de configuratie-instellingen weergeven en kopieer de URL voor de 'Host' van uw app API:  
![][6]
8.  Nu kunt u de gegevens als volgt in de HTTP-aanvraag in andere acties in de stroom ontvangen:  
![][7]  
![][8]
9.  Ten slotte, om een antwoord te verzenden, een andere HTTP-Listener toevoegen en selecteert u de actie HTTP-antwoord verzenden. De aanvraag-ID ingesteld op de aanvraag-id van de HTTP-Listener verkregen en het hoofdgedeelte van de response en HTTP-status te geven terug te vullen:  
![][9]

## <a name="using-the-http-action"></a>Met behulp van de HTTP-actie
De actie HTTP wordt ondersteund door logica Apps en u hoeft een API app eerst om ermee te kunnen worden gemaakt. U kunt een HTTP-actie op elk punt in uw App logica invoegen en kies de URI, kopteksten en hoofdtekst voor het gesprek.
De actie HTTP ondersteunt meerdere opties voor de beveiliging van client-side. Zie de [Beveiligingsopties voor client-side](../scheduler/scheduler-outbound-authentication.md).

De uitvoer van de HTTP-actie is koppen en hoofdtekst, waarmee u kunt verder stroomafwaarts in de stroom is vergelijkbaar met hoe de uitvoer van andere acties en verbindingslijnen wordt verbruikt.

## <a name="do-more-with-your-connector"></a>Doe meer met uw Connector
Nu dat de verbindingslijn is gemaakt, kunt u deze toevoegen aan een zakelijke werkstroom met een App logica. Zie [Wat zijn Apps logica?](app-service-logic-what-are-logic-apps.md).

De Swagger REST API reference op [verbindingslijnen en API-Naslaggids voor Apps](http://go.microsoft.com/fwlink/p/?LinkId=529766)bekijken.

U kunt ook statistieken en beheer van zekerheid aan de verbindingslijn bekijken. Zie [de ingebouwde API Apps en de verbindingen controleren en beheren](app-service-logic-monitor-your-connectors.md).

> [AZURE.NOTE] Als u aan de slag met Azure logica Apps wilt voor het aanmelden voor een account met Azure, gaat u naar de [Logica App proberen](https://tryappservice.azure.com/?appservice=logic). U kunt direct een tijdelijk starter logica app in App-Service maken. Geen creditcard vereist; geen verplichtingen.

<!--Image references-->
[1]: ./media/app-service-logic-connector-http/1.png
[2]: ./media/app-service-logic-connector-http/2.png
[3]: ./media/app-service-logic-connector-http/3.png
[4]: ./media/app-service-logic-connector-http/4.png
[5]: ./media/app-service-logic-connector-http/5.png
[6]: ./media/app-service-logic-connector-http/6.png
[7]: ./media/app-service-logic-connector-http/7.png
[8]: ./media/app-service-logic-connector-http/8.png
[9]: ./media/app-service-logic-connector-http/9.png
[10]: ./media/app-service-logic-connector-http/10.png
