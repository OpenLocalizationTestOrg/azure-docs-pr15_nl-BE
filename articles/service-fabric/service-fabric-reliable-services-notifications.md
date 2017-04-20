<properties
   pageTitle="Meldingen van betrouwbare diensten | Microsoft Azure"
   description="Conceptuele documentatie voor betrouwbare configuratieservices Service meldingen"
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="reliable-services-notifications"></a>Betrouwbare diensten meldingen

Meldingen kunnen clients bijhouden van de wijzigingen die worden aangebracht in een object dat ze geïnteresseerd zijn. Twee typen objecten ondersteunen meldingen: *Betrouwbare staat Manager* en *Betrouwbaar woordenboek*.

Veelvoorkomende redenen voor het gebruik van meldingen zijn:

- Gebouw gerealiseerd weergaven, zoals secundaire of gefilterde weergaven van de staat van de replica wordt samengevoegd. Een voorbeeld is een gesorteerde index van alle sleutels in betrouwbare woordenlijst.
- Verzendende controlegegevens, zoals het aantal gebruikers in het afgelopen uur toegevoegd.

Meldingen worden gestart als onderdeel van de bedrijfsactiviteiten. Door die worden meldingen zo snel mogelijk en synchrone gebeurtenissen niet moeten zijn dure bewerkingen behandeld.

## <a name="reliable-state-manager-notifications"></a>Betrouwbare staat Manager meldingen

Betrouwbare staat Manager biedt meldingen voor de volgende gebeurtenissen:

- Transactie
    - Doorvoeren
- Regionale manager
    - Opnieuw opbouwen
    - Toevoeging van een betrouwbare staat.
    - Verwijdering van een betrouwbare staat.

Manager betrouwbare staat de huidige inflight-transacties bijgehouden. De enige wijziging in de status van de transactie die ervoor zorgt een melding dat te worden gestart, wordt een transactie wordt doorgevoerd.

Betrouwbare staat Manager houdt een verzameling van betrouwbare Staten als betrouwbaar woordenboek en betrouwbare wachtrij. Manager staat betrouwbare meldingen wordt gestart wanneer deze verzameling wordt gewijzigd: een betrouwbare staat wordt toegevoegd of verwijderd, of de hele collectie opnieuw wordt opgebouwd.
De betrouwbare staat Manager-collectie opnieuw wordt opgebouwd in drie gevallen:

- Herstel: Bij het starten van een replica wordt hersteld de vorige status van de schijf. Aan het einde van het herstel wordt **NotifyStateManagerChangedEventArgs** gebruikt een gebeurtenis die de set herstelde betrouwbare lidstaten bevat.
- Volledige kopie: voordat een replica kan deelnemen aan de configuratieset, er worden opgebouwd. Soms moet hiervoor een volledige kopie van de staat betrouwbare staat van de Manager van de primaire replica moet worden toegepast op de niet-actieve secundaire replica. Betrouwbare staat op de secundaire replica wordt gebruikt **NotifyStateManagerChangedEventArgs** een gebeurtenis met de verzameling van betrouwbare Staten die deze uit de primaire replica verkregen.
- Terugzetten: In herstel na storing scenario's, de status van de replica kan worden hersteld vanaf een back-up via **RestoreAsync**. In dergelijke gevallen betrouwbare staat op de primaire replica wordt gebruikt **NotifyStateManagerChangedEventArgs** een gebeurtenis met de set betrouwbare gemeld dat deze uit de back-up teruggezet.

Om te registreren voor meldingen en/of manager-meldingen staat, moet u registreren bij de **TransactionChanged** of **StateManagerChanged** gebeurtenissen op betrouwbare staat Manager. Een algemene plaats om te registreren bij deze gebeurtenis-handlers is de constructor van de stateful-service. Wanneer u zich voor de constructor registreert, niet u een melding die wordt veroorzaakt door een wijziging gedurende de levensduur van de **IReliableStateManager**naar hun.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

**NotifyTransactionChangedEventArgs** de **TransactionChanged** -gebeurtenis-handler gebruikt voor informatie over de gebeurtenis. De eigenschap action (bijvoorbeeld **NotifyTransactionChangedAction.Commit**) die welk type wijziging aangeeft bevat. Het bevat ook de transactie-eigenschap die een verwijzing naar de transactie die wordt gewijzigd.

>[AZURE.NOTE] Vandaag, **TransactionChanged** -gebeurtenissen treden alleen op als de transactie doorgevoerd is. De actie is vervolgens gelijk aan **NotifyTransactionChangedAction.Commit**. Maar in de toekomst mogelijk gebeurtenissen worden verhoogd voor andere soorten transactie de status verandert. Het is raadzaam om de actie controleren en verwerken van de gebeurtenis alleen als die u verwacht.

Hier volgt een voorbeeld van de gebeurtenis-handler voor **TransactionChanged** .

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

**NotifyStateManagerChangedEventArgs** de **StateManagerChanged** -gebeurtenis-handler gebruikt voor informatie over de gebeurtenis.
**NotifyStateManagerChangedEventArgs** heeft twee subcategorieën: **NotifyStateManagerRebuildEventArgs** en **NotifyStateManagerSingleEntityChangedEventArgs**.
U kunt de eigenschap action in **NotifyStateManagerChangedEventArgs** **NotifyStateManagerChangedEventArgs** geconverteerd naar de juiste subcategorie:

- **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
- **NotifyStateManagerChangedAction.Add** en **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Hier volgt een voorbeeld van de **StateManagerChanged** melding handler.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Betrouwbare meldingen van woordenboek

Betrouwbare woordenlijst biedt meldingen voor de volgende gebeurtenissen:

- Opnieuw opbouwen: Genoemd als **ReliableDictionary** de status van een herstelde of gekopieerde lokale staat of een back-up is hersteld.
- Wissen: Aangeroepen wanneer de status van **ReliableDictionary** via de methode **ClearAsync** is gewist.
- Toevoegen: Wordt aangeroepen wanneer een item is toegevoegd aan de **ReliableDictionary**.
- Update: Wordt aangeroepen wanneer een item in de **IReliableDictionary** is bijgewerkt.
- Verwijderen: Aangeroepen wanneer een item in de **IReliableDictionary** is verwijderd.

Als u meldingen voor betrouwbare woordenlijst, moet u registreren bij de gebeurtenis-handler voor de **DictionaryChanged** op **IReliableDictionary**. De **ReliableStateManager.StateManagerChanged** is een algemene plaats om te registreren bij deze gebeurtenis-handlers melding toevoegen.
Registreren als **IReliableDictionary** wordt toegevoegd aan de **IReliableStateManager** zorgt ervoor dat u geen meldingen niet naar hun.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

>[AZURE.NOTE] **ProcessStateManagerSingleEntityNotification** is de monster-methode die in het voorgaande voorbeeld van de **OnStateManagerChangedHandler** oproept.

De bovenstaande code wordt ingesteld dat de interface **IReliableNotificationAsyncCallback** met **DictionaryChanged**. Omdat het **NotifyDictionaryRebuildEventArgs** bevat een **IAsyncEnumerable** -interface die moet worden geïnventariseerd asynchroon--meldingen opnieuw worden gestart via de **RebuildNotificationAsyncCallback** in plaats van **OnDictionaryChangedHandler**.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

>[AZURE.NOTE] In de voorgaande code, als onderdeel van de verwerking van de melding opnieuw maken, is eerst de geaggregeerde onderhouden staat uitgeschakeld. Omdat de betrouwbare verzameling wordt opnieuw opgebouwd met een nieuwe staat, zijn alle vorige mededelingen irrelevant.

**NotifyDictionaryChangedEventArgs** de **DictionaryChanged** -gebeurtenis-handler gebruikt voor informatie over de gebeurtenis.
**NotifyDictionaryChangedEventArgs** heeft vijf subklassen. Gebruik de eigenschap action in **NotifyDictionaryChangedEventArgs** te **NotifyDictionaryChangedEventArgs** voor de juiste subklasse gieten:

- **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
- **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
- **NotifyDictionaryChangedAction.Add** en **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
- **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
- **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Aanbevelingen

- Berichtgebeurtenissen *hoeft* zo snel mogelijk te voeren.
- *Niet* alle dure bewerkingen uitvoeren (bijvoorbeeld i/o-bewerkingen) als onderdeel van de synchrone gebeurtenissen.
- Het actietype *moet* u inschakelen voordat u de gebeurtenis verwerkt. Nieuwe actietypen kunnen in de toekomst worden toegevoegd.

Hier volgen enkele dingen rekening houden:

- Meldingen worden gestart als onderdeel van de uitvoering van een bewerking. Bijvoorbeeld wordt een melding terugzetten gestart als de laatste stap van een bewerking voor terugzetten. Een herstelbewerking voltooid niet totdat de meldingsgebeurtenis is verwerkt.
- Omdat de berichten worden gestart als onderdeel van de bewerkingen toepassen, Zie clients alleen meldingen voor lokaal doorgevoerde transacties. En omdat de bewerkingen zijn gegarandeerd alleen lokaal toegezegd (met andere woorden bent aangemeld), ze kan of kan niet ongedaan worden gemaakt in de toekomst.
- Op het pad opnieuw gestart één melding voor elke bewerking toegepast. Dit betekent dat als T1 transactie Create(X), Delete(X) en Create(X) omvat, u een melding voor het maken van X, één voor de verwijdering en één voor het maken, in die volgorde krijgt.
- Voor transacties die meerdere bewerkingen bevatten, worden de bewerkingen in de volgorde waarin ze zijn ontvangen op de primaire replica van de gebruiker toegepast.
- Als onderdeel van de verwerking van false voortgang, kunnen sommige bewerkingen ongedaan worden gemaakt. Meldingen worden voor dergelijke bewerkingen ongedaan maken, de status van de replica wordt teruggedraaid naar een stabiel punt verhoogd. Een belangrijk verschil van berichten ongedaan maken is dat gebeurtenissen die dubbele sleutels hebben, worden samengeteld. Bijvoorbeeld als T1 transactie is die ongedaan wordt gemaakt, ziet u één melding naar Delete(X).

## <a name="next-steps"></a>Volgende stappen

- [Betrouwbare collecties](service-fabric-work-with-reliable-collections.md)
- [Betrouwbare diensten snel starten](service-fabric-reliable-services-quick-start.md)
- [Betrouwbare Services back-up en terugzetten (noodherstel)](service-fabric-reliable-services-backup-restore.md)
- [Referentie voor ontwikkelaars voor betrouwbare collecties](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
