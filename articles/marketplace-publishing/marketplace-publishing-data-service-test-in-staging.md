<properties
   pageTitle="Testen van uw Data Service-aanbod voor de markt | Microsoft Azure"
   description="Inzicht in uw Data Service-aanbod voor Azure Marketplace te testen."
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

# <a name="testing-your-data-service-offer-in-staging"></a>Testen van uw aanbod Service gegevens in Staging

>[AZURE.IMPORTANT] **Op dit moment we zijn niet langer onboarding alle nieuwe Data Service uitgevers. Nieuwe dataservices zal niet krijgen goedgekeurd voor de aanbieding.** Als u beschikt over een SaaS zakelijke toepassing die u wilt publiceren op AppSource vindt u meer informatie [hier](https://appsource.microsoft.com/partners). Als u een IaaS toepassingen of developer service die u wilt publiceren op Azure Marketplace meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Na het voltooien van de eerste twee stappen voor het [maken van uw Microsoft Developer-account](marketplace-publishing-accounts-creation-registration.md) en het [maken van uw gegevens Service bieden in de Portal voor publiceren](marketplace-publishing-data-service-creation.md) bent u klaar voor uw aanbieding in de markt Azure beschikbaar maken. In dit onderwerp helpt u bij de eerste, tussenliggende, stap "Staging"

Fasering betekent implementatie van uw aanbieding in een privé "sandbox" kunt u testen en controleren of de functionaliteit ervan voordat het uploaden daarvan naar productie. De aanbieding wordt weergegeven in de staging-net als voor een klant die gebruik heeft genomen.

## <a name="step-1-pushing-your-offer-to-staging"></a>Stap 1. Uw voorstel tot tijdelijke duwen
Uw voorstel tot tijdelijke duwen kunt u de aanbieding testen voordat u deze beschikbaar voor abonnees van toekomstige.  U kunt zien hoe uw aanbieding zal worden weergegeven en functioneren voor degenen die zich abonneren op uw gegevens.  

  ![tekening](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1.  Login in de [Portal te publiceren](https://publish.windowsazure.com)
2.  **Gegevens** selecteren in het venster Navigatie links
3.  Selecteer uw gewenste push naar tijdelijke aanbieding. Het bovenstaande scherm wordt weergegeven.
4.  Klik op de knop **Duwen klaarzetten** .  
5.  Als er problemen met de aanbieding die nodig zijn zijn voor pushen voor gefaseerde installatie voltooid, ziet u een lijst weergegeven.  Deze items oplossen door te klikken op elk item in de lijst. Wanneer alle wijzigingen aangebracht, klikt u nogmaals op **Push naar de Staging** -knop.

Als er geen problemen met uw voorstel zijn ziet u het pop-upvenster hieronder.  

Als u geen planning/niet goedgekeurd om het oppervlak van uw aanbieding in Azure Portal (op dit moment heeft beperkte capaciteit), vervolgens sluit u het pop-upvenster.

Test uw Service Data in Azure Portal (naast de portal DataMarket), moet u een Azure abonnements-ID met testen.  Deze abonnement-ID identificeert de rekening die is toegestaan voor het testen van uw aanbod.  

Kopieer uw abonnements-ID en klik op het selectievakje om door te gaan.

  ![tekening](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [AZURE.NOTE] Deze abonnementen Azure-id's zijn alleen vereist voor het testen en de fasering in de [Azure Management Portal](https://manage.windowsazure.com). Ze hoeven niet te testen in Azure Marketplace.

Het volgende scherm toont dat publicatie plaatsvindt door het pictogram 'In behandeling' weergegeven onder geel gemarkeerd. Duwen naar de staging duurt tussen 10 tot 15 minuten.  Als het langer duurt, moet u eerst uw browser (druk op F5 in IE) vernieuwen.  Klik op de koppeling naar contact in de zeldzame gevallen waarin uw voorstel nog steeds voor gefaseerde installatie na een uur pusht, laat het ons weten dat er een probleem.

  ![tekening](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Als de drukknop in een gefaseerde installatie het pictogram 'In behandeling voltooit' wordt stop verplaatsen en de status wordt bijgewerkt naar 'Klaargezet'.  U bent nu klaar om te testen uw voorstel.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Stap 2. Test uw voorstel klaargezet in DataMarket

Klik op de koppeling die de tekst **"Zie uw service aanbieden bij..."** weergeven op het scherm dat de abonnee zien wanneer uw voorstel gaat naar productie en in de DataMarket wordt weergegeven.

  ![tekening](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Testen of controleren of elk van de 12 items boven gemarkeerd zodat alle logo's, prijzen/transacties, tekst, afbeeldingen, documentatie en koppelingen correct zijn en werken goed.  Dit is een goed moment om ervoor te zorgen u hebt opgegeven bij het maken van uw aanbod testwaarden zijn vervangen door werkelijke waarden.

1. Aanbieding logo
2. Naam van het voorstel
3. Publisher naam/link naar de website van uw bedrijf
4. Categorieën voor uw aanbieding
5. Ondersteuning-koppeling van uw voorstel bij de abonnees
6. Contextuele beschrijving voor uw voorstel
7. Voorstel plan met facturering details
8. Koppelen aan code uitvoering
9. Voorbeeldafbeeldingen die illustreren het gebruik van gegevens van het voorstel
10. Input/Output-metagegevens voor elke service in de aanbieding
11. Voorwaarden voor het gebruik van de aanbieding
12. Voorbeeld van de gegevens van de aanbieding


Controleer ten slotte dat de service werkt via de Datamarket door te klikken op de link "Deze DATASET VERKENNEN".  Een nieuw venster wordt geopend in het hulpprogramma noemen we 'Service Explorer' zodat u de resultaten van een query op uw service bekijken kunt.  In dit venster kunt u de parameters invoeren en ziet de resultaten van een query op de service weergegeven.   Weergegeven is ook de URL voor uw Query.  

> [AZURE.NOTE] Zorg ervoor dat de tekstuele beschrijving van de service weergegeven boven aan het bekijken.  En als uw voorstel bestaat uit meer dan één service call, klik op de tabs onder aan het overschakelen naar de volgende service te bekijken en te testen.



## <a name="next-step"></a>Volgende stap
Als u problemen ondervindt en hulp bij het oplossen van deze Neem contact op met [Ondersteuning van Azure Publisher]( http://go.microsoft.com/fwlink/?LinkId=272975).

U zijn tevreden en publiceren Lees uw voorstel de documentatie van [Push productie en goedkeuring aanvragen](marketplace-publishing-push-to-production.md) .

## <a name="see-also"></a>Zie ook
- [Aan de slag: Het publiceren van een aanbod op de markt Azure](marketplace-publishing-getting-started.md)
