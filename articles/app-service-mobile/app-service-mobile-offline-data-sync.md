<properties
    pageTitle="Synchronisatie van off line gegevens in Azure mobiele Apps | Microsoft Azure"
    description="Algemene verwijzing en een overzicht van de functie Off line gegevens gesynchroniseerd voor Azure mobiele Apps"
    documentationCenter="windows"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="offline-data-sync-in-azure-mobile-apps"></a>Synchronisatie van off line gegevens in Azure mobiele Apps

## <a name="what-is-offline-data-sync"></a>Wat is synchronisatie van off line gegevens?

Synchronisatie van off line gegevens is een client en server SDK functie van Azure mobiele Apps waarmee u gemakkelijk voor ontwikkelaars om apps die zonder een netwerkverbinding werken te maken.

Wanneer uw app in de off line modus, kunnen gebruikers nog steeds maken en wijzigen van gegevens die worden opgeslagen in een lokale winkel. Wanneer de app weer on line is, kan het lokale wijzigingen synchroniseren met uw backend Azure Mobile App. De functie biedt tevens ondersteuning voor het detecteren van conflicten wanneer dezelfde record wordt gewijzigd op zowel de client als de backend. Conflicten kunnen vervolgens worden verwerkt op de server of de client.

Off line synchronisatie heeft een aantal voordelen:

* App het reactievermogen verbeteren door caching server-gegevens lokaal op het apparaat
* Robuuste toepassingen die momenten wanneer de netwerkverbinding maken
* Dat eindgebruikers maken en wijzigen van gegevens, zelfs wanneer er geen toegang tot het netwerk, ondersteuning van scenario's met weinig of geen connectiviteit
* Gegevens synchroniseren tussen meerdere apparaten en conflicten detecteren wanneer dezelfde record is gewijzigd door twee apparaten
* Netwerkgebruik op netwerken met hoge latentie en gemeten beperken

De volgende zelfstudies weergeven off line synchronisatie toevoegen aan uw mobiele clients met Azure Mobile Apps:

* [Android: Offline synchronisatie inschakelen]
* [iOS: offline synchronisatie inschakelen]
* [Xamarin iOS: offline synchronisatie inschakelen]
* [Xamarin Android: Offline synchronisatie inschakelen]
* [Xamarin.Forms: Off line synchronisatie inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Universal Windows Platform: Offline synchronisatie inschakelen]

## <a name="what-is-a-sync-table"></a>Wat is een synchronisatietabel?

Als u het eindpunt '/ tabellen', bieden de Azure mobiele client SDK's interfaces, zoals `IMobileServiceTable` (.NET client SDK) of `MSTable` (iOS-client). Deze API's rechtstreeks aansluiten op de backend Azure Mobile App en het zal mislukken als de clientapparaat beschikt niet over een netwerkverbinding.

Voor off line gebruik uw app moet in plaats daarvan gebruiken de *synchronisatietabel* -API's, zoals `IMobileServiceSyncTable` (.NET client SDK) of `MSSyncTable` (iOS-client). CRUD bewerkingen (maken, lezen, Update, Delete) werken allemaal dezelfde tegen synchronisatietabel-API's, maar nu ze lezen uit of naar een *lokale winkel schrijven*. Voordat alle bewerkingen van de tabel synchronisatie kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd.

## <a name="what-is-a-local-store"></a>Wat is een lokale winkel?

Een lokale winkel is de laag van de permanente gegevens op het clientapparaat. De mobiele Apps van Azure client SDK's bieden een standaardimplementatie van het lokale archief. Op Windows, Xamarin en Android, is gebaseerd op de SQLite; op iOS, is gebaseerd op gegevens van Core.

Om de implementatie op basis van SQLite op Windows Phone of Windows Store 8.1 gebruikt, moet u een SQLite extensie installeren. Zie voor meer informatie [Universal Windows Platform: offline synchronisatie inschakelen]. Android en iOS geleverd met een versie van SQLite in het besturingssysteem van het apparaat zelf, dus het is niet nodig om te verwijzen naar uw eigen versie van SQLite.

Ontwikkelaars kunnen hun eigen lokale winkel ook geïmplementeerd. Bijvoorbeeld, als u wilt de gegevens opslaan in een versleutelde indeling op de mobiele client, kunt u een lokale winkel met SQLCipher voor codering.

## <a name="what-is-a-sync-context"></a>Wat is een context synchroniseren?

Een *sync-context* is gekoppeld aan een mobiele client-object (zoals `IMobileServiceClient` of `MSClient`) en bijgehouden wijzigingen die zijn aangebracht met tabellen voor synchronisatie. De context van de synchronisatie houdt een *bewerking wachtrij* blijft een geordende lijst van CUD bewerkingen (Create, Update, Delete) die later zal worden verzonden naar de server.

Een lokale winkel is gekoppeld aan de sync-context met een initialize-methode, zoals `IMobileServicesSyncContext.InitializeAsync(localstore)` in de [.NET client SDK].

## <a name="how-sync-works"></a>Hoe off line synchronisatie werkt

Wanneer u tabellen synchroniseren, bepaalt de clientcode als lokale wijzigingen worden gesynchroniseerd met een end Azure Mobile App. Niets wordt verzonden naar de back-end tot er een oproep aan lokale *doorgeven* van wijzigingen. Ook het lokale archief gevuld met de nieuwe gegevens als er een aanroep naar de *pull* -gegevens.

* **Push**: Push is een bewerking van de context, synchronisatie en alle CUD wijzigingen sinds de laatste push verzendt. Houd er rekening mee dat het is niet mogelijk om alleen een afzonderlijke tabel wijzigingen, omdat anders bewerkingen kunnen worden verzonden buiten bedrijf. Push voert een reeks aanroepen van de REST aan uw backend Azure Mobile App, die op zijn beurt de server-database worden gewijzigd.

* **Pull**: Pull per tabel wordt uitgevoerd en kan worden aangepast met een query voor het ophalen van alleen een subset van de servergegevens. De resulterende gegevens plaats de Azure mobiele client SDK's in de lokale winkel.

* **Impliciete duwt**: als een pull is uitgevoerd op een tabel met lokale updates, de pull eerst een push in de context van de synchronisatie wordt uitgevoerd. Dit helpt conflicten tussen wijzigingen die al in de wachtrij en nieuwe gegevens van de server.

* **Incrementele synchronisatie**: de eerste parameter voor een pull-bewerking is een *naam van een query* die alleen op de client wordt gebruikt. Als u de querynaam van een niet-null-gebruikt, zal de Azure Mobile SDK een *incrementele synchronisatie*uitvoeren.
  Elke keer dat een pull-bewerking retourneert een van de meest recente resultaten `updatedAt` tijdstempel op dat resultaat wordt opgeslagen in de tabellen SDK lokaal systeem. Opeenvolgende pull-bewerkingen worden alleen records ophalen nadat de tijdstempel.

  Voor het gebruik van incrementele sync uw server moet retourneren zinvolle `updatedAt` waarden en moet ondersteunen op dit veld te sorteren. Echter, aangezien de SDK een eigen sorteren op het veld updatedAt voegt, u query niet gebruiken een pull met een eigen `$orderBy$` component.

  Naam van de query kan een willekeurige tekenreeks die u kiest, maar deze moet uniek zijn voor elke logische query in uw app.
  Anders verschillende pull-bewerkingen, kunnen u de tijdstempel met dezelfde incrementele synchronisatie overschrijven en query's kunnen geven onjuiste resultaten.

  Als de query een parameter heeft, is een manier voor het maken van een met de unieke querynaam op te nemen van de waarde van de parameter.
  Als u op gebruikers-id filtert, kan de naam van de query als volgt (in C#) zijn:

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Als u niet deelnemen aan de incrementele sync geeft `null` als de query-ID. In dit geval alle records opgehaald bij elke aanroep naar `PullAsync`, die mogelijk niet efficiënt is.

* **Purging**: U kunt uitschakelen dat de inhoud van het lokale archief met `IMobileServiceSyncTable.PurgeAsync`.
  Dit kan nodig zijn als u verouderde gegevens in de client-database hebt, of als u wilt alle wachtende wijzigingen negeren.

  Een tabel uit het lokale archief, wordt een permanent gewist. Als er bewerkingen in afwachting van synchronisatie met de server-database, genereert het wissen een uitzondering als de parameter *force wissen* is ingesteld.

  Als een voorbeeld van verouderde gegevens op de client, Stel dat in het voorbeeld "todo lijst" Device1 haalt alleen items die niet zijn voltooid. Vervolgens een todoitem "Melk kopen" is gemarkeerd als voltooid op de server door een ander apparaat. Echter steeds Device1 nog de todoitem "Melk kopen" in het lokale archief omdat deze alleen trekt aan artikelen die niet als voltooid zijn gemarkeerd. Dit object verlopen, wordt een permanent gewist.

## <a name="next-steps"></a>Volgende stappen

* [iOS: offline synchronisatie inschakelen]
* [Xamarin iOS: offline synchronisatie inschakelen]
* [Xamarin Android: Offline synchronisatie inschakelen]
* [Universal Windows Platform: Offline synchronisatie inschakelen]

<!-- Links -->
[.NET client SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Offline synchronisatie inschakelen]: app-service-mobile-android-get-started-offline-data.md
[iOS: offline synchronisatie inschakelen]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: offline synchronisatie inschakelen]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Offline synchronisatie inschakelen]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Universal Windows Platform: Offline synchronisatie inschakelen]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
