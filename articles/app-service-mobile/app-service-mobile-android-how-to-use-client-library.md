<properties
    pageTitle="Het gebruik van de Android Mobile Apps Client Library"
    description="Het gebruik van Android client SDK voor Azure Mobile Apps."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>


# <a name="how-to-use-the-android-client-library-for-mobile-apps"></a>Het gebruik van de clientbibliotheek Android voor mobiele toepassingen

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Deze handleiding wordt beschreven hoe u de Android client SDK voor mobiele toepassingen gebruiken voor het implementeren van algemene scenario's, zoals:

- Opvragen van gegevens (invoegen, bijwerken en verwijderen).
- Verificatie.
- Fouten afhandelen.
- Aanpassen van de client. 

Ook wordt een uitvoerige in gemeenschappelijke clientcode gebruikt in de meeste mobiele apps.

Deze handleiding is gericht op de client-side Android SDK.  Zie [werken met end .NET SDK] voor meer informatie over de server-side SDK's voor mobiele Apps,[ 10] of [het gebruik van de backend Node.js SDK][11].

## <a name="reference-documentation"></a>Documentatie

U vindt de [Javadocs API reference] [ 12] voor de clientbibliotheek Android op GitHub.

## <a name="supported-platforms"></a>Ondersteunde Platforms

Azure Mobile Apps Android SDK ondersteunt API 19 tot en met 24 (KitKat via noga).  

Een webweergave de "server-flow"-verificatie gebruikt voor de gebruikersinterface weergegeven. Als het apparaat niet kunt presenteren een WebView UI, zijn vervolgens andere methoden voor verificatie nodig die buiten het bereik van het product.  Deze SDK is niet geschikt voor controle-type of ook beperkte apparaten.

## <a name="setup-and-prerequisites"></a>Installatie en vereisten

Voltooi de zelfstudie [quickstart Mobile Apps](app-service-mobile-android-get-started.md) .  Deze taak garandeert dat alle noodzakelijke voorwaarden voor de ontwikkeling van mobiele Apps in Azure, is voldaan.  De Quickstart helpt u bij het configureren van uw account en maak uw eerste mobiele App back-end.

Als u niet de Quickstart zelfstudie af te handelen, de volgende taken uitvoeren:

- [maken van een mobiele App back-end] [ 13] voor gebruik met uw Android app.
- In Android Studio [de build Gradle bestanden bijwerken](#gradle-build).
- [Internet machtiging inschakelen](#enable-internet).

###<a name="gradle-build"></a>Het bestand Gradle versie bijwerken

Beide **build.gradle** -bestanden wijzigen:

1. Deze code toevoegen aan *het projectbestand op **build.gradle** in de *buildscript* -tag* :

        buildscript {
            repositories {
                jcenter()
            }
        }

2. Deze code toevoegen aan de *Module app* niveau **build.gradle** bestand in de tag *afhankelijkheden* :

        compile 'com.microsoft.azure:azure-mobile-android:3.1.0'

    Op dit moment is de meest recente versie 3.1.0. De ondersteunde versies staan [hier][14].

###<a name="enable-internet"></a>Internet machtiging inschakelen

Voor toegang tot Azure, moet uw app gemachtigd zijn het INTERNET ingeschakeld. Als deze nog niet ingeschakeld, voegen de volgende regel code aan het bestand **AndroidManifest.xml** :

    <uses-permission android:name="android.permission.INTERNET" />

## <a name="the-basics-deep-dive"></a>De grondbeginselen van diepgaande Kennismaking

In deze sectie worden enkele van de code in de Quickstart app die betrekking heeft op het gebruik van Azure Mobile Apps besproken.  

###<a name="data-object"></a>Client gegevensklassen definiëren

Definiëren voor toegang tot gegevens uit tabellen SQL Azure, client gegevensklassen die overeenkomen voor de tabellen in de back-end mobiele App. Voorbeelden in dit onderwerp wordt ervan uitgegaan dat een tabel met de naam **ToDoItem**, met de volgende kolommen:

- ID
- tekst
- voltooien

De bijbehorende client-side-object worden opgegeven:

    public class ToDoItem {
        private String id;
        private String text;
        private Boolean complete;
    }

De code is opgeslagen in een bestand met de naam **ToDoItem.java**.

Als uw Azure SQL-tabel meer kolommen bevat, kunt u de corresponderende velden zou toevoegen aan deze klasse.  Bijvoorbeeld, als de DTO (object data transfer) had een geheel getal-kolom prioriteit en vervolgens kunt u in dit veld, samen met de methoden getter en setter toevoegen:

    private Integer priority;

    /**
    * Returns the item priority
    */
    public Integer getPriority() {
        return mPriority;
    }
    
    /**
    * Sets the item priority
    *
    * @param priority
    *            priority to set
    */
    public final void setPriority(Integer priority) {
        mPriority = priority;
    }

Zie voor informatie over extra tabellen maken in de backend Mobile Apps, [hoe: definiëren een tabel controller] [ 15] (.NET back-end) of [tabellen definiëren met behulp van een dynamische Schema] [ 16] (Node.js backend). U kunt ook de instelling van de **eenvoudige tabellen** in [Azure portal]voor een backend Node.js gebruiken.

###<a name="create-client"></a>Procedure: de clientcontext te maken

Deze code maakt het **MobileServiceClient** -object dat wordt gebruikt voor toegang tot uw backend Mobile App. De code wordt de `onCreate` methode van de klasse **activiteit** is opgegeven in *AndroidManifest.xml* als **belangrijkste** actie- en categorie **starten** . Het gaat in de Quickstart-code in het bestand **ToDoActivity.java** .

        MobileServiceClient mClient = new MobileServiceClient(
            "MobileAppUrl", // Replace with the Site URL
            this)

Vervang in deze code, `MobileAppUrl` met de URL van uw mobiele App backend, die kan worden gevonden in de [Azure portal] in het blad voor uw mobiele App backend. Voor deze regel van de code te compileren, moet u ook toevoegen met de volgende instructie voor **importeren** :

    import com.microsoft.windowsazure.mobileservices.*;

###<a name="instantiating"></a>Procedure: een tabelverwijzing maken

De eenvoudigste manier of wijzigen van gegevens in de back-end is via *programmeermodel wordt getypt*, aangezien Java een sterk getypeerde taal is. Dit model biedt naadloze JSON serialisatie en deserialisatie met behulp van de [gson] [ 3] bibliotheek bij het verzenden van gegevens tussen de clientobjecten en -tabellen in de back-end SQL Azure.

Als u een tabel, maakt u eerst een [MobileServiceTable] [ 8] object door het aanroepen van de **getTable** methode op de [MobileServiceClient][9].  Deze methode heeft twee overbelastingen:

    public class MobileServiceClient {
        public <E> MobileServiceTable<E> getTable(Class<E> clazz);
        public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
    }

**MClient** is in de volgende code wordt een verwijzing naar het object MobileServiceClient.  De eerste overbelasting wordt gebruikt wanneer de klassenaam en de naam van de tabel hetzelfde zijn en wordt het gebruikt in de Quickstart:

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);

De tweede overbelasting wordt gebruikt als naam van de tabel van de naam van de klasse verschilt: de eerste parameter is de naam van de tabel.

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

###<a name="binding"></a>Procedure: gegevens binden aan de gebruikersinterface

Gegevensbinding bestaat uit drie onderdelen:

- De gegevensbron
- De schermindeling
- De adapter die de twee samen bindt.

In ons voorbeeld van code retourneren we de gegevens uit de tabel Mobile Apps SQL Azure **ToDoItem** in een matrix. Deze activiteit is een gemeenschappelijk patroon voor gegevenstoepassingen.  Databasequery's geven vaak een collectie van rijen die de client in een lijst of een matrix als resultaat gegeven. In dit voorbeeld wordt is de matrix de gegevensbron.

De code wordt een schermindeling waarmee de weergave van de gegevens die wordt weergegeven op het apparaat.  De twee zijn verbonden met een adapter die in deze code een uitbreiding is van de **ArrayAdapter&lt;ToDoItem&gt; ** klasse.

#### <a name="layout"></a>Procedure: de indeling definiëren

De lay-out wordt gedefinieerd door de verschillende fragmenten van XML-code. Uitgaande van een bestaande indeling, vertegenwoordigt de volgende code de **Lijstweergave** wilt vullen met gegevens van onze server.

    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>

Hiermee geeft u de id van de lay-out voor een afzonderlijke rij in de lijst in de voorgaande code op het kenmerk *listitem* . Deze code geeft een selectievakje en de bijbehorende tekst en eenmaal voor elk item in de lijst wordt gemaakt. Deze indeling wordt niet weergegeven voor de veld **-id** en een complexe indeling geeft extra velden in de weergave. Deze code wordt in het bestand **row_list_to_do.xml** .

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">
        <CheckBox
            android:id="@+id/checkToDoItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/checkbox_text" />
    </LinearLayout>


#### <a name="adapter"></a>Procedure: de adapter definiëren

Omdat de gegevensbron van de weergave een matrix van **ToDoItem is**, we subklasse onze adapter van een **ArrayAdapter&lt;ToDoItem&gt; ** klasse. Deze subcategorie produceert een weergave voor elke **ToDoItem** met de indeling **row_list_to_do** .

In onze code definiëren we de volgende klasse die is een uitbreiding van de **ArrayAdapter&lt;E&gt; ** klasse:

    public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {

Overschrijf de methode adapters **getView** . Bijvoorbeeld:

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }

        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });


        return row;
    }

Maken we een exemplaar van deze klasse in onze activiteiten als volgt:

    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);

De tweede parameter aan de constructor ToDoItemAdapter is een verwijzing naar de lay-out. We kunnen nu de **Lijstweergave** wordt gestart en de adapter toewijzen aan de **Lijstweergave**.

    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);

### <a name="api"></a>De API-structuur

Bewerkingen van mobiele Apps en aangepaste API-aanroepen zijn asynchroon. De [toekomst] en [AsyncTask] -objecten gebruiken voor de asynchrone methoden waarbij query's, invoegen, bijwerken en verwijderen. Met behulp van futures gemakkelijker meerdere bewerkingen op een thread op de achtergrond uitvoeren zonder omgaan met meerdere, geneste retouraanroepen.

Raadpleeg het bestand **ToDoActivity.java** in het project Android quickstart vanuit [Azure portal] voor een voorbeeld.

#### <a name="use-adapter"></a>Procedure: de adapter

U bent nu klaar voor gebruik gegevensbinding. De volgende code ziet u hoe u de items in de tabel en wordt de lokale adapter gevuld met de geretourneerde artikelen.

    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }

Roep de adapter telkens wanneer die u de tabel **ToDoItem** wijzigen. Aangezien wijzigingen worden gedaan op basis van een andere record, moet u een enkele rij in plaats van een verzameling verwerken. Wanneer u een item wilt invoegen, roept u de methode **toevoegen** op de adapter; bij het verwijderen, roept u de methode **verwijderen** .

##<a name="querying"></a>Procedure: gegevens van uw mobiele App backend opvragen

In deze sectie wordt beschreven hoe u query's op de backend Mobile App, waaronder de volgende taken:

- [Alle items]
- [Gegevens filteren]
- [Gegevens sorteren]
- [Gegevens weergeven op pagina 's]
- [Bepaalde kolommen selecteren]
- [De querymethoden samenvoegen](#chaining)

### <a name="showAll"></a>Procedure: retourneren alle Items in een tabel

De volgende query worden alle items in de tabel **ToDoItem** .

    List<ToDoItem> results = mToDoTable.execute().get();

De variabele *resultaten* retourneert voor de resultaatset van de query als een lijst.

### <a name="filtering"></a>Procedure: Filter gegevens geretourneerd

De uitvoering van de volgende query worden alle items uit de tabel **ToDoItem** waarbij **volledig** gelijk is aan **false**.

    List<ToDoItem> result = mToDoTable.where()
                                .field("complete").eq(false)
                                .execute().get();

**mToDoTable** is de verwijzing naar de servicetabel mobiele die we eerder hebben gemaakt.

Een filter met de methodeaanroep **waar** in de tabelverwijzing definiëren. De methode **waarbij** wordt gevolgd door een **veld** methode gevolgd door een methode waarmee de logische predikaat. Mogelijke predikaat methoden omvatten **eq** (is gelijk aan), **ne** (niet gelijk aan), **gt** (groter dan), **ge** (groter dan of gelijk aan), **lt** (kleiner dan), **le** (kleiner dan of gelijk aan). Deze methoden kunnen u de velden getal en een tekenreeks met specifieke waarden te vergelijken.

U kunt filteren op een datum. De volgende methoden kunnen u vergelijken de hele datumveld of delen van de datum: **jaar**, **maand**, **dag**, **uur**, **minuut**en **tweede**. In het volgende voorbeeld wordt een filter voor artikelen waarvan de *vervaldatum* gelijk is aan 2013 toegevoegd.

    mToDoTable.where().year("due").eq(2013).execute().get();

De volgende methoden ondersteuning voor complexe filters op velden string: **startsWith**, **endsWith** **concat**, **subtekenreeks**, **indexOf**, **vervangen**, **toLower**, **toUpper**, **bijsnijden**en **lengte**. In het volgende voorbeeld worden gefilterd voor rijen waarin *de kolom* begint met 'PRI0'.

    mToDoTable.where().startsWith("text", "PRI0").execute().get();

De volgende methoden van de operator in numerieke velden worden ondersteund: **toevoegen**, **sub**, **mul**, **div**, **mod**, **vloer**, **plafond**en **afronden**. In het volgende voorbeeld filters voor tabelrijen waarbij de **duur** een even getal is.

    mToDoTable.where().field("duration").mod(2).eq(0).execute().get();

Predikaten met deze logische methoden kunnen worden gecombineerd: **en**, **of** en **niet**. In het volgende voorbeeld worden twee van de voorgaande voorbeelden gecombineerd.

    mToDoTable.where().year("due").eq(2013).and().startsWith("text", "PRI0")
                .execute().get();

Groeperen en logische operators nesten:

    mToDoTable.where()
                .year("due").eq(2013)
                    .and
                (startsWith("text", "PRI0").or().field("duration").gt(10))
                .execute().get();

Zie [verkennen van de culturele rijkdom van het model Android client query](http://hashtagfail.com/post/46493261719/mobile-services-android-querying)voor meer informatie over en voorbeelden van filters.

### <a name="sorting"></a>Procedure: sorteren, gegevens geretourneerd

De volgende code retourneert dat alle objecten van een tabel van **ToDoItems** oplopend in *het tekstveld* gesorteerd. *mToDoTable* is de verwijzing naar de back-end-tabel die u eerder hebt gemaakt:

    mToDoTable.orderBy("text", QueryOrder.Ascending).execute().get();

De eerste parameter van de methode **orderBy** is een tekenreeks die gelijk is aan de naam van het veld waarop u wilt sorteren. De tweede parameter wordt de opsomming **QueryOrder** aangeven of u oplopend of Aflopend sorteren.  Als u met behulp van de methode ***waarbij filtert*** , kan de methode ***waarbij*** moet worden aangeroepen voordat de methode ***sorteren op*** .

### <a name="paging"></a>Procedure: gegevens ophalen in pagina's

Het eerste voorbeeld ziet u hoe de bovenste vijf items selecteren uit een tabel. De query retourneert de items in een tabel van **ToDoItems**. **mToDoTable** is de verwijzing naar de back-end-tabel die u eerder hebt gemaakt:

    List<ToDoItem> result = mToDoTable.top(5).execute().get();


Dit is een query die de eerste vijf items worden overgeslagen en geeft als resultaat de volgende vijf:

    mToDoTable.skip(5).top(5).execute().get();

### <a name="selecting"></a>Procedure: specifieke kolommen selecteren

De volgende code ziet u hoe u retourneren alle items in een tabel van **ToDoItems**, maar worden alleen de velden **volledig** en **tekst** . **mToDoTable** is de verwijzing naar de back-end-tabel die we eerder hebben gemaakt.

    List<ToDoItemNarrow> result = mToDoTable.select("complete", "text").execute().get();

De parameters voor de functie select zijn de tekenreeksnamen van de kolommen van de tabel die u wilt retourneren.

De methode **select** moet methoden zoals **waar** en **sorteren op**. Het kan worden gevolgd door het wisselbestand methoden zoals **boven**.

### <a name="chaining"></a>Procedure: de querymethoden samenvoegen

De methoden die worden gebruikt bij het zoeken van de backend-tabellen kunnen worden samengevoegd. Chaining querymethoden kunt u selecteren van specifieke kolommen van de gefilterde rijen gesorteerd en wisselbaar geheugen. U kunt complexe logische filters maken.
De querymethode is een queryobject retourneert. Om te eindigen de reeks van methoden en de query daadwerkelijk uitvoert, roept u de methode **execute** . Bijvoorbeeld:

    mToDoTable.where()
        .year("due").eq(2013)
        .and().startsWith("text", "PRI0")
        .or().field("duration").gt(10)
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .top(20)
        .execute().get();

De aaneengeschakelde querymethoden moeten als volgt worden besteld:

1. Filtermethoden (**waar**).
2. Sorteren op (**orderBy**) methoden.
3. Selectiemethoden (**selecteren**).
4. wisselbestand methoden (**overslaan** en **boven**).

##<a name="inserting"></a>Procedure: gegevens invoegen in de backend

Een instantie van de klasse *ToDoItem* en de eigenschappen ervan instellen.

    ToDoItem item = new ToDoItem();
    item.text = "Test Program";
    item.complete = false;

Gebruik vervolgens **insert()** een object in te voegen:

    ToDoItem entity = mToDoTable.insert(item).get();

De geretourneerde entiteit komt overeen met de gegevens ingevoegd in de tabel backend opgenomen de ID en andere waarden ingesteld op de backend.

Mobiele Apps tabellen moeten een primaire-sleutelkolom met de naam **id**. Deze kolom is standaard een tekenreeks. De standaardwaarde voor de kolom-ID is een GUID.  U kunt andere unieke waarden, zoals e-mailadressen of gebruikersnamen op te geven. Wanneer een tekenreeks-ID-waarde is opgegeven voor een ingevoegde record, wordt een nieuwe GUID gegenereerd in de backend.

Tekenreeks-id-waarden bieden de volgende voordelen:

+ Id's kunnen worden gegenereerd zonder een retour in de database aanbrengen.
+ Records zijn samenvoegen uit verschillende tabellen of databases gemakkelijker.
+ Id-waarden integratie betere met de logica van de toepassing.

Tekenreeks-id-waarden zijn **REQUIRED** voor off line synchronisatie ondersteuning.

##<a name="updating"></a>Procedure: gegevens bijwerken in een mobiele app

Als u wilt de gegevens in een tabel bijwerkt, geeft u het nieuwe object aan de methode **update()** .

    mToDoTable.update(item).get();

*Artikel* is in dit voorbeeld wordt een verwijzing naar een rij in de tabel *ToDoItem* , die enkele wijzigingen aangebracht heeft.
De rij met dezelfde **id** is bijgewerkt.

##<a name="deleting"></a>Procedure: gegevens verwijderen uit een mobiele app

De volgende code toont hoe u gegevens uit een tabel verwijderen door te geven van het object.

    mToDoTable.delete(item);

U kunt een item ook verwijderen door op te geven van de veld **-id** van de rij te verwijderen.

    String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
    mToDoTable.delete(myRowId);

##<a name="lookup"></a>Procedure: een specifiek artikel opzoeken

Zoek een item met een bepaalde **id** -veld met de methode **lookUp()** :

    ToDoItem result = mToDoTable
                        .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
                        .get();

##<a name="untyped"></a>Procedure: werken met gegevens zonder type

Het zonder type programmeermodel kunt u exact bepalen JSON serialisatie.  Er zijn enkele algemene scenario's waar u kunt een typeannotatie programmeermodel te gebruiken. Als bijvoorbeeld uw back-end-tabel veel kolommen bevat en u hoeft alleen te verwijzen naar een subset van de kolommen.  De getypte model moet u alle mobiele apps tabelkolommen in uw klasse definieert.  

De meeste van de API-aanroepen voor toegang tot gegevens zijn vergelijkbaar met de getypte programma-aanroepen. Het belangrijkste verschil is dat in het model zonder type u methoden van het object **MobileServiceJsonTable** , in plaats van het **MobileServiceTable** -object aanroepen.

### <a name="json_instance"></a>Procedure: een exemplaar maken van een tabel zonder typen

Net als het getypte model, begint u met een tabelverwijzing ophalen, maar in dit geval is het een **MobileServicesJsonTable** -object. De verwijzing te verkrijgen door het aanroepen van de **getTable** methode op een instantie van de client:

    private MobileServiceJsonTable mJsonToDoTable;
    //...
    mJsonToDoTable = mClient.getTable("ToDoItem");

Als u een exemplaar van de **MobileServiceJsonTable**hebt gemaakt, is het vrijwel de dezelfde API beschikbaar als met de getypte programmeermodel. In sommige gevallen worden de methoden een typeannotatie parameter in plaats van getypte parameter.

### <a name="json_insert"></a>Procedure: invoegen in een tabel zonder typen

De volgende code toont hoe u een invoegen. De eerste stap is het maken van een [JsonObject][1], die deel uitmaakt van de [gson] [ 3] library.

    JsonObject jsonItem = new JsonObject();
    jsonItem.addProperty("text", "Wake up");
    jsonItem.addProperty("complete", false);

Vervolgens gebruikt u **insert()** het zonder type object invoegen in de tabel.

    mJsonToDoTable.insert(jsonItem).get();

Als u moet de ID van het ingevoegde object wilt ophalen, gebruikt u de methode **getAsJsonPrimitive()** .

    jsonItem.getAsJsonPrimitive("id").getAsInt());

### <a name="json_delete"></a>Procedure: verwijderen uit een tabel zonder typen

De volgende code laat zien hoe een instantie, in dit geval hetzelfde exemplaar van een **JsonObject** die is gemaakt in het voorbeeld van de voorafgaande *Invoegen* verwijderen. De code is hetzelfde als met de getypte case, maar de methode heeft een andere handtekening omdat het verwijst naar een **JsonObject**.

         mToDoTable.delete(jsonItem);

U kunt ook een exemplaar verwijderen via de ID:

         mToDoTable.delete(ID);

### <a name="json_get"></a>Procedure: retourneert alle rijen uit een tabel zonder type

De volgende code toont hoe u een hele tabel ophaalt. Nadat u een tabel JSON gebruikt, kunt u slechts enkele tabelkolommen selectief ophalen.

    public void showAllUntyped(View view) {
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final JsonElement result = mJsonToDoTable.execute().get();
                    final JsonArray results = result.getAsJsonArray();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (JsonElement item : results) {
                                String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                                String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                                Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                                ToDoItem mToDoItem = new ToDoItem();
                                mToDoItem.setId(ID);
                                mToDoItem.setText(mText);
                                mToDoItem.setComplete(mComplete);
                                mAdapter.add(mToDoItem);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
    }

Dezelfde reeks filters, filters en methoden die beschikbaar voor de getypte model zijn van het wisselbestand beschikbaar zijn voor het zonder type model ook.

##<a name="custom-api"></a>Procedure: een aangepaste API aanroepen

Een aangepaste API kunt u aangepaste eindpunten die functionaliteit die niet worden toegewezen aan een invoegen, bijwerken, verwijderen of leesbewerking definiëren. Met behulp van een aangepaste API kunt hebt u meer controle over messaging, inclusief lezen en HTTP-berichtheaders instellen en definiëren van een berichtindeling lichaam dan JSON.

Vanaf een Android client roept u de methode **invokeApi** aanroepen van het aangepaste API-eindpunt. In het volgende voorbeeld ziet u hoe aanroepen van een API-eindpunt met de naam **completeAll**, die als een Collectieklasse met de naam **MarkAllResult resultaat**.

    public void completeItem(View view) {

        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll", MarkAllResult.class );

            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }

                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();
                }
            });
        }

De methode **invokeApi** wordt aangeroepen op de client, die in een POST-aanvraag naar de nieuwe aangepaste API verzendt. Het resultaat van de aangepaste API wordt weergegeven in een dialoogvenster weergegeven als er fouten zijn. Andere versies van **invokeApi** kunnen u desgewenst een object in het hoofdgedeelte van de aanvraag te verzenden, geeft u de HTTP-methode en query-parameters met de aanvraag verzenden. Zonder type versies van **invokeApi** zijn ook beschikbaar.

##<a name="authentication"></a>Procedure: verificatie toevoegen aan uw app

Zelfstudies beschreven al in detail hoe deze functies toe te voegen.

App-Service ondersteunt [app gebruikers verifiëren](app-service-mobile-android-get-started-users.md) met behulp van een groot aantal externe id-providers: Facebook, Google, Microsoft-Account, Twitter en Azure Active Directory. U kunt machtigingen instellen voor tabellen voor het beperken van toegang voor specifieke bewerkingen tot alleen geverifieerde gebruikers. U kunt ook de identiteit van geverifieerde gebruikers verificatieregels implementeren in uw back-end.

Twee stromen voor verificatie worden ondersteund: een stroom van de **server** en een **client** -stroom. De stroom van de server biedt de meest eenvoudige verificatie ervaring, dit is afhankelijk van de providers web-interface van identiteit.  Geen aanvullende SDK's zijn vereist voor het implementeren van stroom serververificatie. Serververificatie van stroom voorziet niet in een naadloze integratie in het mobiele apparaat en wordt alleen aanbevolen voor het bewijs van concept scenario's.

De stroom van de client kunt diepere integratie met apparaatspecifieke mogelijkheden zoals op het afhankelijk is van de SDK's geleverd door de identiteitsprovider.  U kunt bijvoorbeeld de SDK Facebook integreren in uw mobiele toepassing.  De mobiele client naar de Facebook-app swaps en bevestigt uw aanmelding voordat u terug naar uw mobiele app te verwisselen.

Vier stappen zijn vereist om uw app-verificatie inschakelen:

- Registreer uw app voor verificatie met een id-provider.
- Configureer uw backend App-Service.
- Tabelmachtigingen voor geverifieerde gebruikers alleen op de backend-App Service beperken.
- Verificatiecode toevoegen aan uw app.

U kunt machtigingen instellen voor tabellen voor het beperken van toegang voor specifieke bewerkingen tot alleen geverifieerde gebruikers. U kunt ook de SID van een geverifieerde gebruiker om te verzoeken te wijzigen.  Bekijk [aan de slag met verificatie] en de procedure voor Server SDK-documentatie voor meer informatie.

### <a name="caching"></a>Procedure: verificatiecode toevoegen aan uw app

De volgende code begint een stroom aanmelding serverproces met behulp van de Google-provider:

    MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google);

De ID van de aangemelde gebruiker ophalen met een **MobileServiceUser** met behulp van de methode **getUserId** . Zie voor een voorbeeld van hoe de asynchrone aanmelding API's aanroepen met uitstel, [aan de slag met verificatie].

### <a name="caching"></a>Procedure: de Cache-verificatietokens

Caching-verificatietokens, moet u de gebruikers-ID en -verificatietoken lokaal opslaan op het apparaat. De volgende keer dat de toepassing wordt gestart, controleert u de cache en als deze waarden aanwezig zijn, kunt u het logboek in procedure overslaan en de client deze gegevens rehydrate. Maar deze gegevens zijn gevoelig en moeten worden opgeslagen voor veiligheid gecodeerd als de telefoon wordt gestolen.

Ziet u een volledig voorbeeld van hoe u naar de cache-verificatietokens in [Cache authentication tokens sectie][7].

Wanneer u een verlopen token gebruikt probeert, ontvangt u een reactie *401 geen toestemming* . Verificatiefouten met behulp van filters kunt u verwerken.  Filters onderscheppen aanvragen op de backend-App Service. De filtercode het antwoord voor een 401-tests de aanmeldingsprocedure activeert en hervat het verzoek dat de 401 wordt gegenereerd. 

## <a name="adal"></a>Procedure: verificatie van gebruikers met de Active Directory-verificatie-bibliotheek

U kunt Active Directory verificatie bibliotheek (ADAL) gebruikers inloggen op uw toepassing met Azure Active Directory. Met de aanmelding van een client stroom is het handiger om met behulp van de `loginAsync()` methoden als het biedt een meer native UX feel en zorgt voor extra aanpassingen.

1. Uw mobiele app back-end voor aanmelding bij AAD configureren aan de hand van de zelfstudie voor [het configureren van App-Service voor Active Directory-aanmelding](app-service-mobile-how-to-configure-active-directory-authentication.md) . Zorg ervoor dat de optionele stap van het registreren van een native clienttoepassing.

2. Installeer ADAL doordat het bestand build.gradle de volgende definities:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

3. De volgende code toevoegen aan uw toepassing, waardoor de volgende vervangingen:

* **INSERT-hier-instantie** vervangen door de naam van de huurder waarin u uw toepassing hebt ingericht. De https://login.windows.net/contoso.onmicrosoft.com moet worden. Deze waarde kan worden gekopieerd vanaf het tabblad domein in uw Azure Active Directory in de [Azure klassieke portal].

* **INSERT RESOURCE-ID hier** vervangen door de client-ID voor uw mobiele app backend. Op het tabblad **Geavanceerd** onder **Azure Active Directory-instellingen** in de portal kunt u de client-ID.

* **INSERT CLIENT-ID hier** vervangen door de client-ID die u hebt gekopieerd uit de native clienttoepassing.

* **INSERT REDIRECT-URI-hier -** vervangen van uw site _/.auth/login/done_ eindpunt, met behulp van het HTTPS-schema. Deze waarde moet zijn vergelijkbaar met _https://contoso.azurewebsites.net/.auth/login/done_.

        private AuthenticationContext mContext;

        private void authenticate() {
            String authority = "INSERT-AUTHORITY-HERE";
            String resourceId = "INSERT-RESOURCE-ID-HERE";
            String clientId = "INSERT-CLIENT-ID-HERE";
            String redirectUri = "INSERT-REDIRECT-URI-HERE";
            try {
                mContext = new AuthenticationContext(this, authority, true);
                mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
            } catch (Exception exc) {
                exc.printStackTrace();
            }
        }

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    Log.d(TAG, "Cancelled");
                } else {
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    Log.d(TAG, "Token is empty");
                } else {
                    try {
                        JSONObject payload = new JSONObject();
                        payload.put("access_token", result.getAccessToken());
                        ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                            @Override
                            public void onFailure(Throwable exc) {
                                exc.printStackTrace();
                            }
                            @Override
                            public void onSuccess(MobileServiceUser user) {
                                Log.d(TAG, "Login Complete");
                            }
                        });
                    }
                    catch (Exception exc){
                        Log.d(TAG, "Authentication error:" + exc.getMessage());
                    }
                }
            }
        };

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
            }
        }

## <a name="how-to-add-push-notification-to-your-app"></a>Procedure: push-bericht toevoegen aan uw app

U kunt [een overzicht van] [ 6] die hier wordt beschreven hoe Microsoft Azure melding Hubs ondersteunt een groot aantal push-meldingen.  In [deze zelfstudie][5], een push-bericht wordt verzonden naar alle apparaten telkens wanneer een record wordt ingevoegd.

## <a name="how-to-add-offline-sync-to-your-app"></a>Procedure: off line synchronisatie toevoegen aan uw app

De Quickstart handleiding bevat code die off line synchronisatie wordt geïmplementeerd. Zoek de code, voorafgegaan door opmerkingen:

    // Offline Sync

U kunt off line synchronisatie implementeren door de volgende regels code uncommenting en kunt u vergelijkbare code toevoegen aan andere code Mobile Apps.

##<a name="customizing"></a>Procedure: de client aanpassen

Er zijn verschillende manieren voor het aanpassen van het standaardgedrag van de client.

### <a name="headers"></a>Procedure: aanpassen headers aanvragen

Een **ServiceFilter** als u een aangepaste HTTP-header toevoegen aan elk verzoek wilt configureren:

    private class CustomHeaderFilter implements ServiceFilter {

        @Override
        public ListenableFuture<ServiceFilterResponse> handleRequest(
                    ServiceFilterRequest request,
                    NextServiceFilterCallback next) {

            runOnUiThread(new Runnable() {

                @Override
                public void run() {
                    request.addHeader("My-Header", "Value");                    }
            });

            SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
            try {
                ServiceFilterResponse response = next.onNext(request).get();
                result.set(response);
            } catch (Exception exc) {
                result.setException(exc);
            }
        }

### <a name="serialization"></a>Procedure: serialisatie aanpassen

De client wordt ervan uitgegaan dat de tabelnamen, kolomnamen en gegevens typen in de backend al overeen met precies de DAO-objecten die zijn gedefinieerd in de client. Er zijn een aantal redenen waarom de namen van de server en de client mogelijk niet overeen. In uw geval kunt u de volgende soorten aanpassingen doen:

- De namen die worden gebruikt in de App-servicetabel komen niet overeen met de namen die u in de client gebruikt.
- Gebruik een tabel App Service met een andere naam dan de klasse die het in de client is gekoppeld.
- Schakel automatische eigenschap hoofdlettergebruik.
- Complexe eigenschappen toevoegen aan een object.

### <a name="columns"></a>Procedure: verschillende namen voor client en server

Stel dat uw code van Java-client standaard Java-stijl namen voor de eigenschappen van het object **ToDoItem** , zoals de volgende eigenschappen gebruikt:

- mId
- mText
- mComplete
- Aang

De clientnamen serialiseren in JSON-namen die overeenkomen met de namen van de **ToDoItem** -tabel op de server. De volgende code wordt de [gson] [ 3] bibliotheek om de eigenschappen van aantekeningen voorzien:

    @com.google.gson.annotations.SerializedName("text")
    private String mText;

    @com.google.gson.annotations.SerializedName("id")
    private int mId;

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;

    @com.google.gson.annotations.SerializedName("duration")
    private String mDuration;

### <a name="table"></a>Procedure: namen van de andere tabel tussen de client en de back-end

De naam van de client toewijzen aan een naam voor de verschillende mobiele diensten via een overschrijving van de [getTable()] [ 4] methode:

    mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

### <a name="conversions"></a>Procedure: kolom naamstoewijzingen automatiseren

U kunt opgeven dat een conversie strategie die wordt toegepast op elke kolom met de [gson] [ 3] API. De clientbibliotheek Android gebruikt [gson] [ 3] achter de schermen serialiseren Java-objecten naar JSON-gegevens voordat de gegevens worden verzonden naar Azure App-Service.  De volgende code wordt de methode **setFieldNamingStrategy()** gebruikt voor het instellen van de strategie. In dit voorbeeld wordt het eerste teken (een "m"), verwijderen en vervolgens kleine het volgende teken, voor elke veldnaam. Bijvoorbeeld, zou het zetten "mId" in "id".

    client.setGsonBuilder(
        MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(new FieldNamingStrategy() {
            public String translateName(Field field) {
                String name = field.getName();
                return Character.toLowerCase(name.charAt(1))
                    + name.substring(2);
                }
            });

Deze code moet worden uitgevoerd voordat u de **MobileServiceClient**.

### <a name="complex"></a>Procedure: een eigenschap van een object of array opslaan in een tabel

Tot nu toe hebben onze voorbeelden serialisatie betrokken geweest primitieve typen zoals gehele getallen en tekenreeksen.  Primitieve typen serialiseren gemakkelijk in JSON.  Als we een complexe object dat niet automatisch worden geserialiseerd naar JSON toevoegen, moeten we de methode JSON serialisatie.  U kunt een voorbeeld van hoe u aangepaste JSON serialisatie bekijken het weblogbericht [serialisatie van aanpassen met behulp van de gson-bibliotheek in de mobiele diensten Android client][2].

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[The API structure]: #api
[How to: Query data from a mobile service]: #querying
[Alle items]: #showAll
[Gegevens filteren]: #filtering
[Gegevens sorteren]: #sorting
[Gegevens weergeven op pagina 's]: #paging
[Bepaalde kolommen selecteren]: #selecting
[How to: Concatenate query methods]: #chaining
[How to: Bind data to the user interface]: #binding
[How to: Define the layout]: #layout
[How to: Define the adapter]: #adapter
[How to: Use the adapter]: #use-adapter
[How to: Insert data into a mobile service]: #inserting
[How to: update data in a mobile service]: #updating
[How to: Delete data in a mobile service]: #deleting
[How to: Look up a specific item]: #lookup
[How to: Work with untyped data]: #untyped
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching
[How to: Handle errors]: #errors
[How to: Design unit tests]: #tests
[How to: Customize the client]: #customizing
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next Steps]: #next-steps
[Setup and Prerequisites]: #setup

<!-- Images. -->

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
[Aan de slag met verificatie]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[Toekomst]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html