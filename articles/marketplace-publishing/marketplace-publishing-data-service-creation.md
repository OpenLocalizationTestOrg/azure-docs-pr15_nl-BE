<properties
   pageTitle="Handleiding voor het maken van een Service voor de markt | Microsoft Azure"
   description="Gedetailleerde instructies over het maken, certificeren en implementeren van een Service voor gegevens kopen op de markt Azure."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Gegevensservice publiceren gids voor Azure Marketplace

>[AZURE.IMPORTANT] **Op dit moment we zijn niet langer onboarding alle nieuwe Data Service uitgevers. Nieuwe dataservices zal niet krijgen goedgekeurd voor de aanbieding.** Als u beschikt over een SaaS zakelijke toepassing die u wilt publiceren op AppSource vindt u meer informatie [hier](https://appsource.microsoft.com/partners). Als u een IaaS toepassingen of developer service die u wilt publiceren op Azure Marketplace meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Na het voltooien van stap 1, [registratie en Account maken](marketplace-publishing-accounts-creation-registration.md)we geleid u via de [algemene niet-technische](marketplace-publishing-pre-requisites.md) en [Technische vereisten](marketplace-publishing-data-service-creation-prerequisites.md) van een Data Service aanbod op Azure Marketplace. Nu we u door de stappen begeleidt voor het maken van een aanbieding Service gegevens op de [Portal voor publiceren] [ link-pubportal] voor Azure Marketplace.

## <a name="1---login-to-the-publishing-portal"></a>1. Meld u aan bij het publiceren Portal.

Ga naar [https://publish.windowsazure.com](https://publish.windowsazure.com. )

**Voor de eerste keer aanmelden bij Publishing Portal, dezelfde account gebruiken, waarbij uw bedrijfsprofiel verkoper is geregistreerd in het Developer Center.**  (Later kunt u toevoegen een werknemers van uw bedrijf als een co-beheerder in de Portal voor publiceren).

Klik op de tegel **een Data Services publiceren** als dit de eerste aanmelding in de portal voor publiceren.

## <a name="2---choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2. Kies **Data Services** in het navigatiemenu aan de linkerkant.

  ![tekening](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3---create-a-new-data-service"></a>3. een nieuwe Service maken

Vul de titel voor de nieuwe Data Service aanbieden en klik op "+" aan de rechterkant.

  ![tekening](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4---review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4. Controleer het vervolgmenu onder de Service van nieuwe gegevens in het navigatiemenu.

Klik op het tabblad **Overzicht** en Bekijk alle vereiste stappen goed op de markt Azure Service gegevens publiceren.

> [AZURE.TIP] U kunt altijd klikt u op de koppelingen op de pagina 'Overzicht' of tabs gebruiken in het submenu aan de linkerkant van de aanbieding van de Service van gegevens.

## <a name="5---create-a-new-plan"></a>5. Maak een nieuw Plan.

### <a name="offers-plans-transactions"></a>Aanbiedingen, plannen, transacties.

Elke aanbieding kan meerdere plannen, maar ten minste één (1) het Plan moet hebben. Zij zich abonneren voor een van de aanbieding van Plan als eindgebruikers zich op uw voorstel abonneren. Elk plan wordt gedefinieerd hoe eindgebruikers is het mogelijk om de service te gebruiken.

Momenteel Azure Marketplace alleen maandelijks abonnement transactie gebaseerd model ondersteuning voor Services voor gegevens, d.w.z. eindgebruikers betaalt maandelijkse vergoeding op basis van de prijs van de specifieke regeling die ze geabonneerd op en kunnen elke maand aantal transactie die wordt gedefinieerd door het plan in beslag nemen.

Elke transactie die doorgaans gedefinieerd als het aantal records de Service van uw gegevens retourneert is gebaseerd op de query die wordt verzonden naar de Service. De standaardwaarde is 100. Aantal transacties die aan elke query wordt geretourneerd getal zijn van records die worden gedeeld door 100 en afgerond op het dichtstbijzijnde gehele getal.

Is de Service Marketplace Azure laag verantwoordelijkheid voor het controleren van het aantal transacties worden gebruikt door elke query (meter).

> [AZURE.IMPORTANT] Eindgebruikers die de transactie in de maand bereikt wordt uit het voortgezette gebruik van de service tot het einde van hun maandelijkse abonnement cyclus geblokkeerd.

> Het plan of een van de plannen kan (maar moet niet) zijn onbeperkt aantal transacties.

### <a name="create-a-plan"></a>Maak een plan.
1. Klik op **'+'** naast de "toevoegen een nieuw plan".

2. Kies een van de opties: **onbeperkt** of **beperkt** gebruik voor dit plan.  Als beperkt het aantal transactie geeft het plan mogelijk in een maand in beslag neemt.

    ![tekening](media/marketplace-publishing-data-service-creation/step-5.1.png)  

    'Plan-id", die worden gebruikt om te communiceren met de eindgebruikers de naam van het plan in de gebruikersinterface en ook gebruikt door de Service markt ter identificatie van het Plan wordt ook gevraagd of Portal te publiceren. Als u wilt, kunt u de 'Plan-id' wijzigen.

    > [AZURE.NOTE] De 'Plan id' moet uniek zijn binnen het bereik van elke aanbieding. Zoals veel andere id's in de publicatie plannen voor Portal-id gebruikt wordt vergrendeld na de eerste publicatie in productie en u niet mogelijk deze id wijzigen.

3. Klik hierop om uw keuze te accepteren.

4. Vervolgens wordt u gevraagd enkele aanvullende vragen met betrekking tot het nieuwe Plan.

    ![tekening](media/marketplace-publishing-data-service-creation/step-5.2.png)


|Vraag|Betekenis|
|----|----|
|**Dit Plan is gratis en beschikbaar wereldwijd?**|U kunt een volledig vrij van-gratis-plan maken. Als dit het enige plan voor dit voorstel is – betekent dit dat u "Vrij zijn" in de markt publiceert. Als voor een (van enkele) Plan it biedt u een optie voor het aanbieden van eindgebruikers voor meer informatie over de service met een relatief klein aantal transacties per maand.  Als het antwoord "Ja" is, wordt geen verdere vragen worden gevraagd.|

> [AZURE.NOTE] Eindgebruikers kunnen altijd upgraden naar de betaalde plannen.

|Vraag|Betekenis|
|----|----|
|**Gratis proefversie beschikbaar is?**|U kunt kiezen tussen "Geen proef" op alle of een optie voor het gebruik van uw Plan voor "Een maand" geven. Uitgevers als deze optie wilt gebruiken om eindgebruikers de mogelijkheid om te begrijpen van de voordelen van het voorstel voor één maand gratis.|

> [AZURE.IMPORTANT] Eindgebruikers is alleen mogelijk een gratis proefversie te kopen als ze betalingsmethode bijvoorbeeld creditcard enterprise overeenkomst hebben vastgesteld.

> Na een maand van de gratis proefversie start Azure Marketplace opladen klanten de prijs op de datum van het abonnement, tenzij de klant de annulering abonnement gestart. Geen speciale melding krijgt voor de eindgebruikers.

|Vraag|Betekenis|
|----|----|
|**Dit plan moet een actiecode te kopen?**| Uitgevers hebben een optie voor het beperken van toegang tot hun plannen door middel van een speciale code, genaamd "A Promocode" aan specifieke klanten. Alleen de eindgebruikers die deze Promocode is het mogelijk om u te abonneren op de planning. Als u "No", vervolgens gaat u akkoord dat iedereen uit de regio waar de aanbieding beschikbaar (Zie [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) voor meer informatie is) is het mogelijk om u te abonneren op dit plan. Geen verdere vragen krijgt.|
|**Ook dit plan van iemand die niet beschikt over een geldige promotiecode verbergen?**|Als het antwoord op de vorige vraag is 'Ja' heeft de uitgever een optie voor het verwijderen van dit plan wordt weergegeven in de gebruikersinterface van de markt. Betekent dit, klanten zien dit plan in de pagina details van de aanbieding. Eindgebruikers die een promocode te kopen, zich abonneren op met deze promocode kan worden.|

## <a name="6---create-your-marketplace-marketing-content"></a>6. uw marktplaats marketing inhoud maken
Voor meer informatie te verstrekken informatie die vereist is op de tabbladen **verkoop, prijzen, ondersteuning en categorieën** moet Bezoek [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) gedeeld door alle artefacten wordt in de markt Azure gepubliceerd.  

## <a name="7---connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7. uw voorstel verbinding met uw Service (SQL Azure gebaseerd of webservice op basis).

Klik in het submenu **Van gegevens** .

Op de bovenste helft van de pagina u gevraagd om het aanbod van **Namespace**.  

  ![tekening](media/marketplace-publishing-data-service-creation/step-7.png)

De onderstaande vraag wordt gedefinieerd hoe verloopt de uitgever op voorstel van de nieuw gemaakte expose naar Azure Marketplace. (Zie de [Data Services technische vereiste Guide](marketplace-publishing-data-service-creation-prerequisites.md)voor meer informatie).

  ![tekening](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Publiceren van de Database op basis van service**

Klik op de **Database**. De volgende pagina wordt weergegeven:

  ![tekening](media/marketplace-publishing-data-service-creation/step-7.3.png)

De toewijzing van een CSDL voor de gegevensset die is gebaseerd op de Azure SQL DB maken:

  ![tekening](media/marketplace-publishing-data-service-creation/step-7.4.png)

En vervolgens voor elke tabel

  ![tekening](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![tekening](media/marketplace-publishing-data-service-creation/step-7.6.png)

Als Web-Service

  ![tekening](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [AZURE.IMPORTANT] [Toewijzen van een bestaand webservice aan OData via CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) voor gedetailleerde instructies en voorbeelden voor het maken van een CSDL op Internet lezen.

## <a name="next-steps"></a>Volgende stappen
Nu dat u uw serviceaanbod van gegevens hebt gemaakt, zorg ervoor dat u de instructies in de [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) voltooien voordat u verder gaan naar [uw Service gegevens in Staging testen](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Zie ook
- [Aan de slag: Het publiceren van een aanbod op de markt Azure](marketplace-publishing-getting-started.md)
- Als u geïnteresseerd bent in wat de algehele OData toewijzingsproces en het doel, Lees dit artikel [Gegevenstoewijzing Service OData](marketplace-publishing-data-service-creation-odata-mapping.md) definities, structuren en instructies bekijken.
- Als u geïnteresseerd bent in leren en wat zijn de specifieke knooppunten en hun parameters, Lees dit artikel [Service OData Mapping gegevensknooppunten](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) voor definities en uitleg, voorbeelden en case context gebruikt.
- Als u geïnteresseerd bent in voorbeelden bekijken, lezen [Gegevens Service OData Mapping voorbeelden](marketplace-publishing-data-service-creation-odata-mapping-examples.md) Zie voorbeeldcode en begrijpen de syntaxis en de context van dit artikel.


[link-pubportal]:https://publish.windowsazure.com
