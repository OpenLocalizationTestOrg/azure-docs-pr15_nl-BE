<properties
    pageTitle="Werken met betrouwbare collecties | Microsoft Azure"
    description="Informatie over de aanbevolen procedures voor het werken met betrouwbare collecties."
    services="service-fabric"
    documentationCenter=".net"
    authors="JeffreyRichter"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="03/28/2016"
    ms.author="jeffreyr" />

# <a name="working-with-reliable-collections"></a>Werken met betrouwbare collecties

Fabric-service biedt een stateful programmeermodel beschikbaar voor .NET ontwikkelaars via betrouwbare collecties. Met name biedt Fabric-Service betrouwbare woordenboek en betrouwbare wachtrij klassen. Wanneer u deze klassen, de staat is gepartitioneerd (voor schaalbaarheid), gerepliceerd (voor beschikbaarheid) en verwerkt in een partitie (voor ZURE semantiek). We bekijken een normaal gebruik van een object dictionary betrouwbare en zien wat het daadwerkelijk doen.

~~~
retry:
try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to  
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) { 
   await Task.Delay(100, cancellationToken); goto retry; 
}
~~~

Alle bewerkingen op betrouwbare dictionary-objecten (met uitzondering van ClearAsync die niet ongedaan maken), moet een ITransaction-object. Dit object is gekoppeld aan een en alle wijzigingen die u probeert te maken aan de woordenlijst van een betrouwbare en/of betrouwbare wachtrij-objecten binnen een enkele partitie. Verwerving van een ITransaction object door het aanroepen van de partitie de StateManager de CreateTransaction-methode.
 
In de code, wordt het ITransaction-object doorgegeven aan een betrouwbare woordenlijst AddAsync methode. Intern, nemen woordenlijst methoden die een sleutel accepteert een lezer/schrijver vergrendelen die zijn gekoppeld aan de sleutel. Als de methode de waarde van de sleutel wijzigt, wordt een vergrendeling voor schrijven op de sleutel en als de methode alleen van de waarde van de sleutel wordt gelezen, wordt een leesvergrendeling genomen op de sleutel. Aangezien AddAsync de waarde van de sleutel naar de nieuwe doorgegeven waarde wijzigt, wordt van de sleutel schrijven lock genomen. Dus als threads 2 (of meer) probeert om waarden te voegen met dezelfde sleutel op hetzelfde moment, worden één thread wordt vergrendelen voor het schrijven en de andere threads. Standaard methoden blokkeren gedurende 4 seconden te vergrendelen; de methoden genereren na 4 seconden, een TimeoutException. Methode overbelastingen bestaan zodat u een time-outwaarde expliciete doorgeven als u liever.
 
Meestal u uw code schrijven om te reageren op een TimeoutException door het vangen en het gehele opnieuw (zoals weergegeven in de bovenstaande code). In mijn eenvoudige code net gebeld doorgeven van 100 milliseconden als Task.Delay. Maar in werkelijkheid u wellicht beter een soort exponentiële back-uit-vertraging te gebruiken.
 
Zodra de vergrendeling is verkregen, wordt AddAsync de sleutel en de waarde objectverwijzingen naar een interne tijdelijke woordenlijst die is gekoppeld aan het ITransaction-object. Dit is gedaan om lezen-uw-eigen-schrijfbewerkingen semantiek. Dat wil zeggen na het aanroepen van AddAsync, een aanroep van TryGetValueAsync (met hetzelfde object ITransaction) later de waarde als resultaat zelfs als de transactie nog niet zijn doorgevoerd. Vervolgens AddAsync serialiseert uw sleutel en waarde objecten met byte-matrices en worden deze matrices met bytes toegevoegd aan een bestand op het lokale knooppunt. AddAsync verzendt ten slotte de byte-matrices naar de secundaire replica's zodat ze dezelfde sleutel/waarde-informatie hebben. Ook al de sleutel/waarde-informatie naar een logboekbestand geschreven is, de informatie wordt niet beschouwd als onderdeel van de woordenlijst totdat de transactie die ze gekoppeld vastgelegd is. 

In de code voert het aanroepen van CommitAsync alle bewerkingen van de transactie. Met name commit informatie toegevoegd aan het bestand op het lokale knooppunt en stuurt ook de doorvoerrecord naar de secundaire replica's. Zodra een quorum (meerderheid) van de replica's heeft beantwoord, alle wijzigingen in gegevens worden beschouwd als permanente en alle vergrendelingen die zijn gekoppeld aan sleutels die zijn gemanipuleerd via het ITransaction-object worden vrijgegeven, zodat andere threads/transacties de dezelfde toetsen en de bijbehorende waarden bewerken kunt.

Als CommitAsync (meestal als gevolg van een uitzondering wordt gegenereerd) niet wordt aangeroepen, wordt het ITransaction-object verwijderd. Bij de buitengebruikstelling van een niet-doorgevoerde ITransaction-object, Fabric-Service afbreken informatie toegevoegd aan het logboekbestand van het lokale knooppunt en er niets hoeft te worden verzonden naar een van de secundaire replica's. En vervolgens alle vergrendelingen die zijn gekoppeld aan sleutels die zijn bewerkt via de transactie worden vrijgegeven.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Veelvoorkomende valkuilen en hoe u deze kunt vermijden
Nu dat u hoe de betrouwbare collecties intern werken begrijpt, laten we op sommige gemeenschappelijke misbruik van deze. Zie de volgende code:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes, 
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
~~~

Als u werkt met een gewone .NET woordenboek, kunt u een sleutel/waarde toevoegen aan de woordenlijst en wijzig de waarde van een eigenschap (bijvoorbeeld LastLogin). Echter, deze code werkt niet goed met een betrouwbare woordenlijst. Vergeet niet uit de eerdere discussie, het aanroepen van AddAsync de sleutel/waarde-objecten met byte-matrices serialiseert en de matrices worden opgeslagen in een lokaal bestand en ook doorgestuurd naar de secundaire replica's. Als u een eigenschap wijzigt, verandert dit de waarde van de eigenschap in het geheugen. Dit heeft geen gevolgen voor het lokale bestand of de gegevens die worden verzonden met de replica's. Als het proces is vastgelopen, wordt wat zich in het geheugen weggegooid. Bij het starten van een nieuw proces of een andere replica primaire wordt, is de waarde van de eigenschap oude wat er beschikbaar is. 

Ik kan niet genoeg benadrukken genoeg hoe gemakkelijk is om de aard van de fout hierboven weergegeven. En alleen leert u over de fout als of wanneer dat het proces wordt afgesloten. De juiste manier om de code te schrijven is gewoon omkeren van de twee regels:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync(); 
}
~~~

Hier is een ander voorbeeld is een algemene fout weergegeven:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync(); 
   }
}
~~~

Nogmaals, met regelmatige .NET woordenboeken, de bovenstaande code werkt prima en is een vast patroon: de ontwikkelaar een sleutel wordt gebruikt om een waarde op te zoeken. Als de waarde aanwezig is, wijzigt de ontwikkelaar de waarde van een eigenschap. Met betrouwbare collecties, maar vertoont deze code hetzelfde probleem, zoals reeds besproken: __moet u een object niet wijzigen nadat u deze hebt verleend tot een verzameling van betrouwbare.__
 
De juiste manier voor het bijwerken van een waarde in een betrouwbare collectie is een verwijzing naar de bestaande waarde ophalen en kunt u het object waarnaar wordt verwezen door deze verwijzing onveranderbaar. Maak vervolgens een nieuw object is een exacte kopie van het oorspronkelijke object. Nu kunt u de status van dit nieuwe object wijzigen en schrijf het nieuwe object in de collectie zodat deze haalt geserialiseerd met byte-matrices toegevoegd aan het lokale bestand en de replica's. Nadat de wijzigingen zijn doorgevoerd, zijn de objecten in het geheugen, het lokale bestand en de replica's exact dezelfde staat. Alles is goed!

De volgende code ziet u hoe u een waarde in een verzameling van betrouwbare bijwerken:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire 
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync(); 
   }
}
~~~

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Onveranderbare gegevenstypen om te voorkomen dat programmeurs fout definiëren

Willen we graag de compiler om fouten te melden in het ideale geval wanneer u per ongeluk code die de status van een object die u moeten overwegen onveranderbare mutates produceren. Maar de C#-compiler beschikt niet over de mogelijkheid om dit te doen. Dus, om te voorkomen dat potentiële programmeur bugs, wordt ten zeerste aanbevolen dat u de typen definieert u met betrouwbare collecties te typen onveranderbaar zijn. Dit houdt in dat u blijven aan core waardetypen (zoals nummers [Int32, UInt64, enz.], DateTime, Guid, TimeSpan en dergelijke vasthouden). En natuurlijk kunt u ook tekenreeks gebruiken. Het is raadzaam om te voorkomen dat de collectie eigenschappen als het serialiseren van en deserialisatie ze kan vaak kunnen schaden prestaties. Echter, als u gebruik wilt maken collectie-eigenschappen, wordt ten zeerste aanbevolen het gebruik van. NET van onveranderbare verzamelingen library (System.Collections.Immutable). Deze bibliotheek is beschikbaar voor downloaden van http://nuget.org. Ook wordt aangeraden uw klassen sluiting en de velden die alleen-lezen zoveel mogelijk.

Het type UserInfo hieronder laat zien hoe een onveranderbare type profiteren van de bovengenoemde aanbevelingen definiëren.

~~~
[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;
 
   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }
 
   [DataMember]
   public readonly String Email;
 
   // Ideally, this would be a readonly field but it can't be because OnDeserialized 
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
~~~

Het type item-id is ook een onveranderbare zoals hier wordt weergegeven:

~~~
[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
~~~

## <a name="schema-versioning-upgrades"></a>Schema versioning (upgrades)

Intern, serialiseren betrouwbare collecties uw objecten via. DataContractSerializer van het NET. Geserialiseerde objecten op de lokale schijf de primaire replica worden doorgevoerd en worden ook doorgegeven aan de secundaire replica's. Als uw service veroudert, is het waarschijnlijk zult u het wijzigen van het type gegevens (schema), die de service is vereist. U kunt versies van uw gegevens met grote zorgvuldigheid moet benadert. Allereerst omdat u moet altijd mogelijk te converteren de oude gegevens. Dit houdt in uw code deserialisatie oneindig achterwaarts compatibel moet zijn: 333-versie van de servicecode moet kunnen worden toegepast op gegevens in een betrouwbare collectie geplaatst door versie 1 van de servicecode 5 jaar geleden.

Bovendien is servicecode bijgewerkte domein voor een upgrade op een tijdstip. Tijdens een upgrade hebt u dus twee verschillende versies van de servicecode die gelijktijdig wordt uitgevoerd. U moet voorkomen dat de nieuwe versie van de servicecode gebruikt het nieuwe schema zoals oude versies van de servicecode mogelijk niet voor het verwerken van het nieuwe schema. Indien mogelijk, moet u elke versie van de service is compatibel met versie 1 ontwerpen. Dit betekent met name V1 van uw servicecode moet kunnen gewoon negeren alle schema-elementen die dit expliciet niet verwerkt. Het moet wel kunnen alle gegevens weet niet expliciet over en gewoon schrijven weer uit bij het bijwerken van een woordenlijst sleutel of waarde op te slaan. 

>[AZURE.WARNING] Terwijl u het schema van een sleutel wijzigen kunt, moet u ervoor zorgen dat uw sleutel van de hash-code en algoritmen is gelijk aan stabiel zijn. Als u een van deze algoritmen werking wijzigt, is het niet mogelijk de sleutel in de woordenlijst voor betrouwbare ooit opnieuw opzoeken.
  
U kunt ook uitvoeren wat doorgaans wordt aangeduid als een upgrade 2-fase. Met een upgrade 2-phase upgrade uw service van V1 naar V2: V2 bevat de code die weet hoe u kunt omgaan met het nieuwe schema wijzigen, maar deze code niet uitgevoerd. Wanneer de code V2 V1 gegevens leest, werkt het en V1-gegevens worden geschreven. Klik, nadat de upgrade voltooid voor alle upgrades van domeinen is, u kunt op signaal naar de actieve exemplaren van de V2 de upgrade is voltooid. (Eén manier om het signaal dat dit naar een upgrade van de configuratie is, dit is wat maakt dit een upgrade 2-fase). Nu kunnen de V2-exemplaren V1-gegevens lezen, converteren naar V2-gegevens worden uitgevoerd en wegschrijven als V2-gegevens. Als u andere exemplaren V2 gegevens lezen, ze hoeven niet te converteren, ze gewoon worden uitgevoerd en V2 gegevens wegschrijven.

## <a name="next-steps"></a>Volgende stappen
Zie meer informatie over het maken van contracten voorwaarts compatibel gegevenstype, [Vooruit-compatibele gegevenscontracten](https://msdn.microsoft.com/library/ms731083.aspx).

Zie aanbevolen procedures voor versiebeheer gegevenscontracten meer [Gegevens Contract versiebeheer](https://msdn.microsoft.com/library/ms731138.aspx). 

Zie meer informatie over het implementeren versie fouttolerante gegevenscontracten, [Versie fouttolerante serialisatie Callbacks](https://msdn.microsoft.com/library/ms733734.aspx). 

Als u wilt weten hoe u een gegevensstructuur die voor de verschillende versies samenwerken kan, Zie [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
