<properties
   pageTitle="Veelgestelde vragen over de catalogus Azure gegevens | Microsoft Azure"
   description="Veelgestelde vragen over de catalogus met Azure gegevens, met inbegrip van mogelijkheden voor gegevensbron te zoeken, aantekeningen en beheer."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-frequently-asked-questions"></a>Veelgestelde vragen over de catalogus Azure gegevens

Dit artikel bevat antwoorden op veelgestelde vragen met betrekking tot de service Microsoft **Azure gegevenscatalogus** .

## <a name="q-what-is-azure-data-catalog"></a>V: Wat is Azure gegevenscatalogus?

A: Microsoft Azure gegevenscatalogus is een volledig beheerde service gehost in het Microsoft Azure cloud die als een systeem van registratie en het systeem van discovery voor gegevensbronnen van de onderneming fungeert. Catalogus Azure gegevens biedt mogelijkheden waarmee een gebruiker – van analisten en wetenschappers gegevens voor ontwikkelaars – te registreren, ontdekken, te begrijpen en gegevensbronnen in beslag nemen.

## <a name="q-what-customer-challenges-does-azure-data-catalog-solve"></a>V: welke klant uitdagingen Azure gegevenscatalogus heeft opgelost?

De uitdaging van gegevensbron te zoeken en 'donkere data' adressen Azure gegevenscatalogus doordat gebruikers ontdekken en begrijpen van de gegevensbronnen van de onderneming.

## <a name="q-who-are-the-target-audiences-for-azure-data-catalog"></a>V: wie zijn de doelgroepen voor de catalogus met Azure gegevens?

Catalogus Azure gegevens biedt mogelijkheden voor technische en niet-technische gebruikers, waaronder:

- Ontwikkelaars van gegevens, BI en Analytics Professionals: die verantwoordelijk zijn voor het maken van data en analytics inhoud voor anderen te verbruiken
-   Gegevens Stewards: Degenen die de kennis over de gegevens, wat het betekent en hoe deze is bedoeld om te worden gebruikt en voor welk doel
- Gebruikers van gegevens: Degenen die kunnen gemakkelijk opsporen moeten, begrijpen en verbinding maken met gegevens die nodig zijn voor hun werk met het gereedschap van hun keuze
- Centrale IT: degenen die honderden gegevensbronnen detecteerbaar maken voor zakelijke gebruikers nodig hebben en die nodig zijn om toezicht te houden over hoe de gegevens wordt gebruikt en door wie

## <a name="q-what-is-the-azure-data-catalog-region-availability"></a>V: Wat is de beschikbaarheid van de catalogus gegevens Azure regio?

Azure services voor catalogus gegevens zijn beschikbaar in de volgende datacenters:

- West-Verenigde Staten
- Oost-Verenigde Staten
- West-Europa
- Noord-Europa
- Australië, Oost
- Zuidoost-Azië

## <a name="q-what-are-the-limits-on-the-number-of-data-assets-in-azure-data-catalog"></a>V: wat zijn de beperkingen van het aantal gegevens activa in Azure gegevenscatalogus?

De gratis editie van Azure catalogus gegevens is beperkt tot 5.000 geregistreerde gegevens activa.

De Standard Edition van Azure catalogus gegevens ondersteunt maximaal 100.000 geregistreerde gegevens activa.

## <a name="q-what-are-the-supported-data-source-and-asset-types"></a>V: wat zijn de ondersteunde bron en activa gegevenstypen?

Zie [Gegevens catalogus DSR](data-catalog-dsr.md) voor de lijst met ondersteunde gegevensbronnen.

## <a name="q-how-do-i-request-support-for-another-data-source"></a>V: hoe ik ondersteuning voor een andere gegevensbron aanvragen?

Functie-aanvragen en andere feedback kunnen worden ingediend in het [forum catalogus Azure-gegevens](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-do-i-get-started-with-azure-data-catalog"></a>V: hoe ga ik aan de slag met Azure gegevenscatalogus?

De beste plaats om te beginnen is door de instructies in [Aan de slag met de catalogus met gegevens](data-catalog-get-started.md). Dit artikel is een end-to-end-overzicht van de mogelijkheden van de service.

## <a name="q-how-do-i-register-my-data"></a>V: hoe kan ik mijn gegevens registreren?

Als u wilt uw gegevens in de catalogus van Azure gegevens registreren, het registratiehulpprogramma Azure catalogus met gegevens uit het gebied "Publiceren" van de catalogus met Azure gegevens portal te starten. Meld u aan met dezelfde referenties voor toegang tot de portal catalogus Azure-gegevens in de catalogus van Azure gegevens publiceren toepassing, en selecteert u de gegevensbron en de specifieke activa die u wilt registreren.

## <a name="q-what-properties-are-extracted-for-data-assets-that-are-registered"></a>V: welke eigenschappen worden opgehaald voor de gegevens activa die zijn geregistreerd?

De specifieke eigenschappen van gegevensbron gegevensbron verschillen, maar in het algemeen de volgende informatie wordt opgehaald door de catalogus van Azure gegevens publishing-service:

- Activumnaam
- Activatype
- Activumbeschrijving
- Kenmerk/kolomnamen
- Kenmerk/kolomgegevenstypen
- Beschrijving van het kenmerk/kolom

> [AZURE.IMPORTANT] Registreren van gegevens activa met Azure gegevenscatalogus niet verplaatsen of kopiëren van gegevens naar de cloud. Metagegevens van de activa registreren van activa van een gegevensbron worden gekopieerd naar Azure, maar blijven de gegevens in de locatie van bestaande gegevensbron. De enige uitzondering op deze regel is als een gebruiker wil uploaden voorvertoning records of een profiel gegevens bij het registreren van activa. Wanneer met inbegrip van een voorbeeld van, tot en met 20 records van alle activa gekopieerd en worden opgeslagen als een momentopname in de catalogus gegevens Azure. Wanneer, met inbegrip van een profiel gegevens worden statistische gegevens (zoals de grootte van tabellen, het percentage null-waarden per kolom en de minimale, maximale en gemiddelde waarden voor de kolommen) berekend en opgenomen in de opgeslagen metagegevens in de catalogus.

<br/>

> [AZURE.NOTE] Voor gegevensbronnen, zoals SQL Server Analysis Services met een eersteklas eigenschap **Beschrijving** , wordt de catalogus met Azure gegevens publiceren van toepassing opgehaald door de waarde van die eigenschap. Voor SQL Server-relationele databases, die niet over een eersteklas eigenschap **Beschrijving** , wordt de catalogus van Azure gegevens publiceren toepassing de waarde van de uitgebreide eigenschap voor objecten en de kolommen ms_description uitgepakt. Zie TechNet [Met behulp van uitgebreide eigenschappen van objecten in de Database](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx)voor meer informatie.

## <a name="q-how-long-should-it-take-for-newly-registered-assets-to-appear-in-azure-data-catalog"></a>V: hoe lang moet het duren nieuw ingeschreven activa moet worden weergegeven in de catalogus van Azure gegevens?

Na registratie van activa met een catalogus met Azure gegevens kan er een periode van 5-10 seconden voordat ze worden weergegeven in de catalogus van Azure gegevens portal.

## <a name="q-how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>V: hoe ik aantekeningen en verrijking van de metagegevens voor mijn geregistreerde gegevens activa?

De eenvoudigste manier metagegevens op te geven voor ingeschreven activa is het activum selecteren in de portal Azure catalogus met gegevens en voert de metagegevenswaarden voor het geselecteerde object in het deelvenster Eigenschappen of schema.

U kunt ook bepaalde metadata zoals de tags en deskundigen geven tijdens de registratie. De waarden in de catalogus van Azure gegevens publishing-service van toepassing op alle activa die op dat moment wordt geregistreerd. De recent geregistreerd als objecten wilt weergeven in de portal voor meer commentaar, klik op de knop **Weergave Portal** op het laatste scherm van de catalogus met Azure gegevens publiceren toepassing.

## <a name="q-how-do-i-delete-my-registered-data-objects"></a>V: hoe kan ik mijn geregistreerde gegevensobjecten verwijderen?

U kunt een object verwijderen uit catalogus Azure-gegevens door het object in de portal te selecteren en vervolgens te klikken op de knop **verwijderen** . De metagegevens van het object wordt verwijderd uit de catalogus gegevens Azure, maar heeft geen invloed op de werkelijke onderliggende gegevensbron.

## <a name="q-what-is-an-expert"></a>V: Wat is een expert?

Een expert is een persoon met een perspectief op de hoogte over een gegevensobject. Een object kan meerdere deskundigen hebben. Een expert hoeft niet te worden van de 'eigenaar' van een object; de expert is iemand die weet hoe de gegevens kunnen en moeten worden gebruikt.

## <a name="q-how-do-i-share-information-with-the-azure-data-catalog-team-if-i-encounter-problems"></a>V: hoe moet ik informatie delen met het team catalogus Azure-gegevens als ik problemen?

Gebruik het forum catalogus Azure-gegevens om problemen te rapporteren, informatie delen en vragen stellen. Het forum is te vinden op http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409

##<a name="q-does-azure-data-catalog-work-with-this-other-data-source-im-interested-in"></a>V: Azure catalogus met gegevens werkt met deze andere gegevensbron die ik ben?
We proberen actief op meer gegevensbronnen toe te voegen aan de catalogus gegevens Azure. Als er een gegevensbron die u wilt Zie ondersteund, moet deze voorstellen (of ondersteuning van uw stem als heeft al voorgesteld) in het [forum catalogus Azure-gegevens](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>V: hoe is Azure catalogus met gegevens gerelateerd aan de catalogus gegevens in Power BI voor Office 365?

U kunt catalogus met Azure gegevens beschouwen als een evolutie van de catalogus met gegevens. Catalogus Azure gegevens biedt vergelijkbare mogelijkheden voor gegevensbron publiceren en opsporen, maar is gericht op bredere scenario's en niet afhankelijk zijn van Office 365. Kort nadat de catalogus Azure gegevens over het algemeen beschikbaar zal twee catalogussen samenvoegen in een enkele service.

## <a name="q-what-permissions-does-a-user-need-to-register-assets-with-azure-data-catalog"></a>V: welke machtigingen een gebruiker nodig heeft activa met de catalogus met Azure gegevens registreren?

De gebruiker die de catalogus met Azure gegevens registratiehulpprogramma moet machtigingen voor de gegevensbron waarmee hij voor het lezen van de metagegevens van de bron. Als de gebruiker ook naar een voorbeeld, moet hebben de gebruiker ook machtigingen waarmee hij lezen in de gegevens van de objecten wordt geregistreerd.

## <a name="q-will-azure-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>V: Azure catalogus van gegevens beschikbaar wordt gesteld voor alsmede implementatie voor bedrijfsruimten?

Azure gegevenscatalogus is een cloud service die kan werken met gegevensbronnen wolk zowel op lokalen, een hybride bron discovery oplossing leveren. Er zijn momenteel geen plannen voor een versie van de catalogus met Azure gegevens-service die wordt uitgevoerd op de ruimten.

##<a name="q-can-we-extract-more--richer-metadata-from-the-data-sources-we-register"></a>V: kunnen we meer / rijkere metagegevens uitpakken uit de gegevensbronnen die wij registreren?

We proberen actief om de mogelijkheden van Azure catalogus met gegevens weer te geven. Als er aanvullende metagegevens die u zou willen zien uitgepakte uit de gegevensbron tijdens de registratie, neem voorgesteld dat (of stem voor dit als het al is voorgesteld) in het [forum catalogus Azure-gegevens](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). In de toekomst mag derden toe te voegen nieuwe typen gegevensbronnen via de API voor uitbreidbaarheid.

## <a name="q-how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>V: hoe beperk ik de zichtbaarheid van de activa van de geregistreerde gegevens, zodat alleen bepaalde mensen deze kunnen vinden?

A: de activa van de gegevens in de catalogus Azure gegevens selecteren en klik op de knop 'Eigenaar'. Eigenaars van activa in Azure gegevenscatalogus gegevens kunnen de zichtbaarheidsinstellingen ofwel alle catalogus als gebruikers wilt toestaan om de activa eigendom of de zichtbaarheid beperken tot specifieke gebruikers wijzigen.

## <a name="q-how-do-i-update-the-registration-for-a-data-asset-to-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>V: hoe kan ik de registratie voor een activum gegevens bijwerken die in de gegevensbron wordt gewijzigd, worden doorgevoerd in de catalogus?

A: om te werken in de metagegevens voor de gegevens activa die al zijn geregistreerd in de catalogus, gewoon opnieuw registreren de gegevensbron met de activa. Wijzigingen in de gegevensbron, zoals kolommen worden toegevoegd of verwijderd uit de tabellen of weergaven, worden bijgewerkt in de catalogus, maar alle aantekeningen die door gebruikers worden onderhouden.

## <a name="q-my-question-isnt-answered-here--what-should-i-do"></a>V: mijn vraag hier niet wordt beantwoord – wat moet ik doen?

Hoofd op over aan de [catalogus met Azure gegevens forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Hier hun manier vindt er vragen.
