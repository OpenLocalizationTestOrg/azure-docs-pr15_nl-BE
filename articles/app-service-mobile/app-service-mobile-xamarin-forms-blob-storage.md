<properties
    pageTitle="Verbinding maken met Azure opslag in uw app Xamarin.Forms"
    description="Afbeeldingen toevoegen aan de todo lijst Xamarin.Forms mobiele app door verbinding te maken met Azure blob-opslag"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="connect-to-azure-storage-in-your-xamarinforms-app"></a>Verbinding maken met Azure opslag in uw app Xamarin.Forms

## <a name="overview"></a>Overzicht

Ondersteuning voor off line synchronisatie van gestructureerde gegevens met CRUD bewerkingen op het eindpunt van de /tables de Azure Mobile Apps client en server SDK. In het algemeen worden deze gegevens opgeslagen in een database of een vergelijkbare winkel en kunnen deze opgeslagen gegevens grote binaire gegevens in het algemeen efficiënt opslaan. Sommige toepassingen hebben ook gerelateerde gegevens die zich elders bevinden (bijvoorbeeld blob storage, bestandsshares) en het is handig om te kunnen maken van koppelingen tussen records in het eindpunt van de /tables en andere gegevens.

In dit onderwerp wordt beschreven hoe u ondersteuning voor afbeeldingen toevoegen aan de quickstart Mobile Apps todo-lijst. U moet eerst de zelfstudie voor [maken van een app Xamarin.Forms]voltooien.

In deze zelfstudie, u maakt een account voor de opslag en een verbindingsreeks toevoegen aan uw backend Mobile App. U wilt toevoegen een nieuw overgenomen van het nieuwe mobiele Apps type `StorageController<T>` voor uw serverproject.

>[AZURE.TIP] Deze zelfstudie is een [companion monster](https://azure.microsoft.com/documentation/samples/app-service-mobile-dotnet-todo-list-files/) beschikbaar, dat kan worden geïmplementeerd op uw eigen account Azure. 

## <a name="prerequisites"></a>Vereisten

* Voltooi de zelfstudie [een app Xamarin.Forms maken] , waarin de andere vereisten. In dit artikel wordt de voltooide toepassing uit deze handleiding.

>[AZURE.NOTE] Als u aan de slag met Azure App-Service wilt voordat u zich voor een account met Azure aanmeldt, gaat u naar de [App-Service probeert](https://tryappservice.azure.com/?appServiceName=mobile). Daar kunt u direct maken een tijdelijk starter mobiele app in App-Service — geen creditcard vereist en geen verplichtingen.

## <a name="create-a-storage-account"></a>Maak een account voor opslag

1. Maak een account opslag door de zelfstudie [een Azure opslag Account maken]. 

2. Navigeer naar de account van de zojuist gemaakte opslag en klikt u op het pictogram van de **sleutels** in het portal voor Azure. Kopieer de **primaire verbinding String**.

3. Ga naar uw mobiele app backend. **Alle**instellingen -> **Toepassingsinstellingen** -> **Verbindingsreeksen**, maakt u een nieuwe registersleutel met de naam `MS_AzureStorageAccountConnectionString` en de waarde gekopieerd uit uw account opslag gebruiken. **Aangepast** als het sleuteltype gebruiken.

## <a name="add-a-storage-controller-to-the-server"></a>Een opslagcontroller aan de server toevoegen

U moet een nieuwe domeincontroller toevoegen aan uw serverproject die zal reageren op verzoeken om een SAS-token voor opslag in Azure, alsmede een lijst met bestanden die overeenkomen met een record terug:

- [Een opslagcontroller toevoegen aan uw serverproject](#add-controller-code)
- [Routes die zijn geregistreerd door de opslagcontroller](#routes-registered)
- [Communicatie van client en server](#client-communication)

###<a name="add-controller-code"></a>Een opslagcontroller toevoegen aan uw serverproject

1. Open uw .NET server-project in Visual Studio. Het pakket Nuget [Microsoft.Azure.Mobile.Server.Files]toevoegen. Zorg ervoor dat **zijn voorlopige versie**selecteren.

2. Open uw .NET server-project in Visual Studio. Klik met de rechtermuisknop op de map **Controllers** en selecteer **toevoegen** -> **Controller** -> **Web API 2 Controller - leeg**. Naam van de domeincontroller `TodoItemStorageController`.

3. Voeg de volgende instructies:

        using Microsoft.Azure.Mobile.Server.Files;
        using Microsoft.Azure.Mobile.Server.Files.Controllers;

4. De basisklasse aan `StorageController`:
    
        public class TodoItemStorageController : StorageController<TodoItem>

5. De volgende methoden toevoegen aan de klasse:

        [HttpPost]
        [Route("tables/TodoItem/{id}/StorageToken")]
        public async Task<HttpResponseMessage> PostStorageTokenRequest(string id, StorageTokenRequest value)
        {
            StorageToken token = await GetStorageTokenAsync(id, value);

            return Request.CreateResponse(token);
        }

        // Get the files associated with this record
        [HttpGet]
        [Route("tables/TodoItem/{id}/MobileServiceFiles")]
        public async Task<HttpResponseMessage> GetFiles(string id)
        {
            IEnumerable<MobileServiceFile> files = await GetRecordFilesAsync(id);

            return Request.CreateResponse(files);
        }

        [HttpDelete]
        [Route("tables/TodoItem/{id}/MobileServiceFiles/{name}")]
        public Task Delete(string id, string name)
        {
            return base.DeleteFileAsync(id, name);
        }

6. De configuratie van de Web-API voor het instellen van het kenmerk routering bijwerken. Voeg de volgende regel op in **Startup.MobileApp.cs**, de `ConfigureMobileApp()` methode na de definitie van de `config` variabele:

        config.MapHttpAttributeRoutes();

7. Uw serverproject publiceren naar uw mobiele app backend.

###<a name="routes-registered"></a>Routes die zijn geregistreerd door de opslagcontroller

De nieuwe `TodoItemStorageController` beschrijft twee onderliggende bronnen onder de record beheert:

- StorageToken

    + HTTP POST: Hiermee maakt u een token opslag
    
        `/tables/TodoItem/{id}/MobileServiceFiles`
    
- MobileServiceFiles

    + HTTP GET: Haalt een lijst van bestanden die zijn gekoppeld aan de record
    
        `/tables/TodoItem/{id}/MobileServiceFiles`

    + HTTP verwijderen: Hiermee verwijdert u het opgegeven bestand in de resource-id voor bestand
    
        `/tables/TodoItem/{id}/MobileServiceFiles/{fileid}`

###<a name="client-communication"></a>Communicatie van client en server

Houd er rekening mee dat `TodoItemStorageController` is *niet* een route voor het uploaden of downloaden van een blob hebben. Dat komt doordat een mobiele client communiceert met blob storage *direct* om deze bewerkingen uitvoeren, na eerst aan een SAS-token (gedeelde toegang handtekening) veilig toegang krijgen tot een bepaalde blob of container. Dit is een belangrijke architectonische anderszins toegang tot opslag beperkt door de schaalbaarheid en beschikbaarheid van de mobiele back-end. In plaats daarvan kunt rechtstreeks verbinding maken met Azure opslag, de mobiele client profiteren van de functies voor automatisch partitioneren en geo-verdeling.

Een gedeelde toegang handtekening biedt gedeelde toegang tot bronnen in uw account opslag. Dit betekent dat u een client beperkte machtigingen op objecten in uw account opslag gedurende een bepaalde tijd en met een opgegeven reeks machtigingen, zonder dat u voor het delen van uw account toegangstoetsen kunt verlenen. Zie voor meer informatie, [Informatie over gedeelde toegang handtekeningen].

Het onderstaande diagram ziet u de interacties van de client en server. Voordat u een bestand uploadt, vraagt de client een SAS-token van de service. De service gebruikt de verbindingsreeks voor de opslag voor het genereren van een nieuwe SA's, geeft als aan de client resultaat. De SA's is beperkt in de tijd en machtigingen beperkt tot alleen een bepaald bestand of een container. De mobiele client gebruikt deze SAS en de opslag van Azure client SDK het bestand te uploaden naar een blobopslag.

![Een SAS-token aanvragen](./media/app-service-mobile-xamarin-forms-blob-storage/storage-token-diagram.png)

## <a name="update-your-client-app-to-add-image-support"></a>Uw app client om toe te voegen ondersteuning voor afbeeldingen bijwerken

Open het Xamarin.Forms quickstart-project in Visual Studio of Xamarin Studio. U Nuget pakketten installeren en bijwerken van de draagbare library-project en de iOS, Android en Windows client projecten:

- [Nuget pakketten toevoegen](#add-nuget)
- [IPlatform-interface toevoegen](#add-iplatform)
- [Klasse FileHelper toevoegen](#add-filehelper)
- [Een bestand sync-handler toevoegen](#file-sync-handler)
- [TodoItemManager bijwerken](#update-todoitemmanager)
- [Een detailweergave toevoegen](#add-details-view)
- [Update de hoofdweergave](#update-main-view)
- [Het Android project bijwerken](#update-android), [iOS-project](#update-ios), [project voor Windows](#update-windows)

>[AZURE.NOTE] Deze zelfstudie bevat alleen instructies voor Android, iOS en Windows Store, niet Windows Phone-platforms.

###<a name="add-nuget"></a>Nuget pakketten toevoegen

De oplossing met de rechtermuisknop en selecteer **beheren Nuget pakketten voor oplossing**. De volgende Nuget pakketten toevoegen aan **alle** projecten in de oplossing. Zorg ervoor dat controleren **zijn voorlopige versie**.

  - [Microsoft.Azure.Mobile.Client.Files]

  - [Microsoft.Azure.Mobile.Client.SQLiteStore]

  - [PCLStorage]

In dit voorbeeld gebruikt de bibliotheek [PCLStorage] voor het gemak, maar is niet verplicht de Azure Mobile Apps client SDK.

[PCLStorage]: https://www.nuget.org/packages/PCLStorage/

###<a name="add-iplatform"></a>IPlatform-interface toevoegen

Maak een nieuwe interface voor `IPlatform` in de hoofdbibliotheek draagbare project. Dit komt overeen met het patroon van de [Xamarin.Forms DependencyService] voor het laden van de rechts-platform-specifieke klasse tijdens runtime. U wordt later platform-specifieke implementaties in elk van de client-projecten toevoegen.

1. Voeg de volgende instructies:

        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Sync;

2. De uitvoering wordt vervangen door het volgende:

        public interface IPlatform
        {
            Task <string> GetTodoFilesPathAsync();

            Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata);

            Task<string> TakePhotoAsync(object context);

            Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename);
        }

###<a name="add-filehelper"></a>Klasse FileHelper toevoegen

1. Maak een nieuwe klasse `FileHelper` in de hoofdbibliotheek draagbare project. Voeg de volgende instructies:

        using System.IO;
        using PCLStorage;
        using System.Threading.Tasks;
        using Xamarin.Forms;

2. De klassendefinitie toevoegen:

        public class FileHelper
        {
            public static async Task<string> CopyTodoItemFileAsync(string itemId, string filePath)
            {
                IFolder localStorage = FileSystem.Current.LocalStorage;

                string fileName = Path.GetFileName(filePath);
                string targetPath = await GetLocalFilePathAsync(itemId, fileName);

                var sourceFile = await localStorage.GetFileAsync(filePath);
                var sourceStream = await sourceFile.OpenAsync(FileAccess.Read);

                var targetFile = await localStorage.CreateFileAsync(targetPath, CreationCollisionOption.ReplaceExisting);

                using (var targetStream = await targetFile.OpenAsync(FileAccess.ReadAndWrite)) {
                    await sourceStream.CopyToAsync(targetStream);
                }

                return targetPath;
            }

            public static async Task<string> GetLocalFilePathAsync(string itemId, string fileName)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();

                string recordFilesPath = Path.Combine(await platform.GetTodoFilesPathAsync(), itemId);

                    var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(recordFilesPath);
                    if (checkExists == ExistenceCheckResult.NotFound) {
                        await FileSystem.Current.LocalStorage.CreateFolderAsync(recordFilesPath, CreationCollisionOption.ReplaceExisting);
                    }

                return Path.Combine(recordFilesPath, fileName);
            }

            public static async Task DeleteLocalFileAsync(Microsoft.WindowsAzure.MobileServices.Files.MobileServiceFile fileName)
            {
                string localPath = await GetLocalFilePathAsync(fileName.ParentId, fileName.Name);
                var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(localPath);

                if (checkExists == ExistenceCheckResult.FileExists) {
                    var file = await FileSystem.Current.LocalStorage.GetFileAsync(localPath);
                    await file.DeleteAsync();
                }
            }
        }

###<a name="file-sync-handler"></a>Een bestand sync-handler toevoegen

Maak een nieuwe klasse `TodoItemFileSyncHandler` in de hoofdbibliotheek draagbare project. Deze klasse bevat retouraanroepen uit de SDK Azure uw code waarschuwen wanneer een bestand wordt toegevoegd of verwijderd.

De Azure mobiele Client SDK bestandsgegevens niet werkelijk opgeslagen: de client-SDK wordt de implementatie van `IFileSyncHandler` die op zijn beurt bepaalt of en hoe bestanden worden opgeslagen op het lokale apparaat.

1. Voeg de volgende instructies:

        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Xamarin.Forms;

2. De klassendefinitie van de vervangen door het volgende: 

        public class TodoItemFileSyncHandler : IFileSyncHandler
        {
            private readonly TodoItemManager todoItemManager;

            public TodoItemFileSyncHandler(TodoItemManager itemManager)
            {
                this.todoItemManager = itemManager;
            }

            public Task<IMobileServiceFileDataSource> GetDataSource(MobileServiceFileMetadata metadata)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();
                return platform.GetFileDataSource(metadata);
            }

            public async Task ProcessFileSynchronizationAction(MobileServiceFile file, FileSynchronizationAction action)
            {
                if (action == FileSynchronizationAction.Delete) {
                    await FileHelper.DeleteLocalFileAsync(file);
                }
                else { // Create or update. We're aggressively downloading all files.
                    await this.todoItemManager.DownloadFileAsync(file);
                }
            }
        }

###<a name="update-todoitemmanager"></a>TodoItemManager bijwerken

1. Opmerkingen bij de lijn in **TodoItemManager.cs**, `#define OFFLINE_SYNC_ENABLED`.

2. In **TodoItemManager.cs**, voeg de volgende instructies:

        using System.IO;
        using Xamarin.Forms;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Eventing;

3. In de constructor van `TodoItemManager`, voeg de volgende na de aanroep van `DefineTable()`:

        // Initialize file sync
        this.client.InitializeFileSyncContext(new TodoItemFileSyncHandler(this), store);

4. Vervang in de constructor, de aanroep van `InitializeAsync` met de volgende stappen. Dit zal zorgen ervoor dat er retouraanroepen wanneer records worden gewijzigd in het lokale archief. Deze retouraanroepen de sync-functie gebruikt voor het starten van de bestands-handler voor synchronisatie.

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

5. In `SyncAsync()`, voeg de volgende na de aanroep van `PushAsync()`:

        await this.todoTable.PushFileChangesAsync();

6. Toevoegen van de volgende methoden om te `TodoItemManager`:

        internal async Task DownloadFileAsync(MobileServiceFile file)
        {
            var todoItem = await todoTable.LookupAsync(file.ParentId);
            IPlatform platform = DependencyService.Get<IPlatform>();

            string filePath = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name); 
            await platform.DownloadFileAsync(this.todoTable, file, filePath);
        }

        internal async Task<MobileServiceFile> AddImage(TodoItem todoItem, string imagePath)
        {
            string targetPath = await FileHelper.CopyTodoItemFileAsync(todoItem.Id, imagePath);
            return await this.todoTable.AddFileAsync(todoItem, Path.GetFileName(targetPath));
        }

        internal async Task DeleteImage(TodoItem todoItem, MobileServiceFile file)
        {
            await this.todoTable.DeleteFileAsync(file);
        }

        internal async Task<IEnumerable<MobileServiceFile>> GetImageFilesAsync(TodoItem todoItem)
        {
            return await this.todoTable.GetFilesAsync(todoItem);
        }

###<a name="add-details-view"></a>Een detailweergave toevoegen

In deze sectie kunt toevoegen u een nieuwe detailweergave voor een item todo. De weergave wordt gemaakt wanneer de gebruiker een item todo selecteert en nieuwe afbeeldingen wilt toevoegen aan een item maakt.

1. Een nieuwe klasse **TodoItemImage** toevoegen aan het project draagbare bibliotheek met de volgende uitvoering:

        public class TodoItemImage : INotifyPropertyChanged
        {
            private string name;
            private string uri;

            public MobileServiceFile File { get; private set; }

            public string Name
            {
                get { return name; }
                set
                {
                    name = value;
                    OnPropertyChanged(nameof(Name));
                }
            }

            public string Uri
            {
                get { return uri; }      
                set
                {
                    uri = value;
                    OnPropertyChanged(nameof(Uri));
                }
            }

            public TodoItemImage(MobileServiceFile file, TodoItem todoItem)
            {
                Name = file.Name;
                File = file;

                FileHelper.GetLocalFilePathAsync(todoItem.Id, file.Name).ContinueWith(x => this.Uri = x.Result);
            }

            public event PropertyChangedEventHandler PropertyChanged;

            private void OnPropertyChanged(string propertyName)
            {
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
            }
        }

2. **App.cs**bewerken. Vervang de initialisatie van `MainPage` met het volgende:
    
        MainPage = new NavigationPage(new TodoList());

3. **App.cs**, voeg de volgende eigenschap:

        public static object UIContext { get; set; }

4. Klik met de rechtermuisknop op het project draagbare bibliotheek en selecteer **toevoegen** -> **Nieuw Item** -> **Cross-platform** -> **Formulieren Xaml-pagina**. Naam van de weergave `TodoItemDetailsView`.

5. Open **TodoItemDetailsView.xaml** en de hoofdtekst van de ContentPage vervangen door het volgende:

          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Button Clicked="OnAdd" Text="Add image"></Button>
            <ListView x:Name="imagesList"
                      ItemsSource="{Binding Images}"
                      IsPullToRefreshEnabled="false"
                      Grid.Row="2">
              <ListView.ItemTemplate>
                <DataTemplate>
                  <ImageCell ImageSource="{Binding Uri}"
                             Text="{Binding Name}">
                  </ImageCell>
                </DataTemplate>
              </ListView.ItemTemplate>
            </ListView>
          </Grid>

6. **TodoItemDetailsView.xaml.cs** bewerken en voeg de volgende instructies:

        using System.Collections.ObjectModel;
        using Microsoft.WindowsAzure.MobileServices.Files;

7. Vervangen van de uitvoering van de `TodoItemDetailsView` met het volgende:

        public partial class TodoItemDetailsView : ContentPage
        {
            private TodoItemManager manager;

            public TodoItem TodoItem { get; set; }        
            public ObservableCollection<TodoItemImage> Images { get; set; }

            public TodoItemDetailsView(TodoItem todoItem, TodoItemManager manager)
            {
                InitializeComponent();
                this.Title = todoItem.Name;

                this.TodoItem = todoItem;
                this.manager = manager;

                this.Images = new ObservableCollection<TodoItemImage>();
                this.BindingContext = this;
            }

            public async Task LoadImagesAsync()
            {
                IEnumerable<MobileServiceFile> files = await this.manager.GetImageFilesAsync(TodoItem);
                this.Images.Clear();

                foreach (var f in files) {
                    var todoImage = new TodoItemImage(f, this.TodoItem);
                    this.Images.Add(todoImage);
                }
            }

            public async void OnAdd(object sender, EventArgs e)
            {
                IPlatform mediaProvider = DependencyService.Get<IPlatform>();
                string sourceImagePath = await mediaProvider.TakePhotoAsync(App.UIContext);

                if (sourceImagePath != null) {
                    MobileServiceFile file = await this.manager.AddImage(this.TodoItem, sourceImagePath);

                    var image = new TodoItemImage(file, this.TodoItem);
                    this.Images.Add(image);
                }
            }
        }

###<a name="update-main-view"></a>Update de hoofdweergave 

Werk de hoofdweergave van de om detailweergave te openen wanneer een todo-item wordt geselecteerd.

Vervang in de **TodoList.xaml.cs**, de uitvoering van de `OnSelected` met het volgende:

    public async void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var todo = e.SelectedItem as TodoItem;

        if (todo != null) {
            var detailsView = new TodoItemDetailsView(todo, manager);
            await detailsView.LoadImagesAsync();
            await Navigation.PushAsync(detailsView);
        }

        todoList.SelectedItem = null;
    }

###<a name="update-android"></a>Het Android project bijwerken

Platformspecifieke code toevoegen aan de Android-project, inclusief code voor het downloaden van een bestand en het gebruik van de camera voor het vastleggen van een nieuwe afbeelding. 

Deze code wordt de Xamarin.Forms [DependencyService](https://developer.xamarin.com/guides/xamarin-forms/dependency-service/) de klasse rechts platformspecifieke tijdens runtime wordt geladen.

1. De component **Xamarin.Mobile** toevoegen aan het project Android.

2. Een nieuwe klasse toevoegen `DroidPlatform` met de volgende implementatie. "YourNamespace" vervangen door de belangrijkste naamruimte van uw project.

        using System;
        using System.IO;
        using System.Threading.Tasks;
        using Android.Content;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.Droid.DroidPlatform))]
        namespace YourNamespace.Droid
        {
            public class DroidPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var uiContext = context as Context;
                        if (uiContext != null) {
                            var mediaPicker = new MediaPicker(uiContext);
                            var photo = await mediaPicker.TakePhotoAsync(new StoreCameraMediaOptions());

                            return photo.Path;
                        }
                    }
                    catch (TaskCanceledException) {
                    }

                    return null;
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string appData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
                    string filesPath = Path.Combine(appData, "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. **MainActivity.cs**bewerken. In `OnCreate`, voeg de volgende voor de aanroep naar `LoadApplication()`:

        App.UIContext = this;

###<a name="update-ios"></a>Het iOS-project bijwerken

Platformspecifieke code toevoegen aan het project iOS.

1. De component **Xamarin.Mobile** toevoegen aan het project iOS.

2. Een nieuwe klasse toevoegen `TouchPlatform` met de volgende implementatie. "YourNamespace" vervangen door de belangrijkste naamruimte van uw project.

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.iOS.TouchPlatform))]
        namespace YourNamespace.iOS
        {
            class TouchPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var mediaPicker = new MediaPicker();
                        var mediaFile = await mediaPicker.PickPhotoAsync();
                        return mediaFile.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string filesPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. **AppDelegate.cs** bewerken en opmerkingen bij het aanroepen van `SQLitePCL.CurrentPlatform.Init()`.

###<a name="update-windows"></a>Het project voor Windows Update

1. De uitbreiding van Visual Studio [SQLite 8.1 voor Windows](http://go.microsoft.com/fwlink/?LinkID=716919)te installeren. Zie de zelfstudie voor [off line synchronisatie van uw Windows-app inschakelen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)voor meer informatie. 

2. **Package.appxmanifest** bewerken en controleren van de mogelijkheid voor de **Webcam** .

3. Een nieuwe klasse toevoegen `WindowsStorePlatform` met de volgende implementatie. "YourNamespace" vervangen door de belangrijkste naamruimte van uw project.

        using System;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Windows.Foundation;
        using Windows.Media.Capture;
        using Windows.Storage;
        using YourNamespace;

        [assembly: Xamarin.Forms.Dependency(typeof(WinApp.WindowsStorePlatform))]
        namespace WinApp
        {
            public class WindowsStorePlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> GetTodoFilesPathAsync()
                {
                    var storageFolder = ApplicationData.Current.LocalFolder;
                    var filePath = "TodoItemFiles";

                    var result = await storageFolder.TryGetItemAsync(filePath);

                    if (result == null) {
                        result = await storageFolder.CreateFolderAsync(filePath);
                    }

                    return result.Name; // later operations will use relative paths
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        CameraCaptureUI dialog = new CameraCaptureUI();
                        Size aspectRatio = new Size(16, 9);
                        dialog.PhotoSettings.CroppedAspectRatio = aspectRatio;

                        StorageFile file = await dialog.CaptureFileAsync(CameraCaptureUIMode.Photo);
                        return file.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }
            }
        }

##<a name="summary"></a>Samenvatting

In dit artikel beschreven hoe u het nieuwe bestandsondersteuning in Azure mobiele client en server SDK gebruiken om te werken met Azure opslag. 

- Maak een account voor de opslag en de verbindingsreeks toevoegen aan uw mobiele app backend. Alleen de back-end de Azure opslag heeft: de mobiele client vraagt een SAS-token (gedeelde toegang handtekening) wanneer toegang tot opslag Azure. Zie voor meer informatie over tokens in Azure opslag SAS, [Wat zijn gedeelde toegang handtekeningen].

- Een domeincontroller maken die subklassen `StorageController` de SAS token aanvragen verwerken en opvragen van de bestanden die gekoppeld aan een record zijn. Bestanden zijn standaard gekoppeld aan een record met behulp van de record-ID als deel van de containernaam; het gedrag kan worden aangepast door het opgeven van een implementatie van `IContainerNameResolver`. Het beleid van SAS-token kan ook worden aangepast.

- De Azure mobiele Client SDK slaat geen gegevens in een bestand worden opgeslagen. In plaats daarvan wordt de client SDK roept de `IFileSyncHandler`, die vervolgens besluit hoe (en als) bestanden op het lokale apparaat zijn opgeslagen. De sync-handler wordt als volgt geregistreerd:

        client.InitializeFileSync(new MyFileSyncHandler(), store);

      + `IFileSyncHandler.GetDataSource`wordt aangeroepen wanneer de Azure mobiele Client SDK de gegevens in een bestand (bijvoorbeeld als onderdeel van het uploadproces moet). Dit biedt u de mogelijkheid beheren hoe (en als) bestanden op het lokale apparaat zijn opgeslagen en die informatie wanneer dat nodig is.

      + `IFileSyncHandler.ProcessFileSynchronizationAction`Als onderdeel van de stroom bestand synchronisatie wordt gestart. Een verwijzing naar een bestand en een opsommingswaarde FileSynchronizationAction worden geleverd, zodat u hoe uw toepassing die gebeurtenis bepalen kunt (bijvoorbeeld automatisch downloaden van een bestand wanneer deze wordt gemaakt of bijgewerkt, een bestand verwijderen uit het lokale apparaat wanneer dat bestand wordt verwijderd op de server) moet verwerken.

- A `MobileServiceFile` kan worden gebruikt in de on line of off line modus, met behulp van een `IMobileServiceTable` of `IMobileServiceSyncTable`, respectievelijk. In de off line scenario het uploaden treedt op wanneer de toepassing wordt aangeroepen `PushFileChangesAsync`. Dit zorgt ervoor dat de wachtrij off line bewerking moeten worden verwerkt; voor elke bestandsbewerking de Azure mobiele client SDK roept de `GetDataSource` methode van de `IFileSyncHandler` exemplaar voor het ophalen van de inhoud van het bestand voor het uploaden.

- Om bestanden van een item ophalen, roept de '' GetFilesAsync` method on the  `IMobileServiceTable<T> ` or IMobileServiceSyncTable<T>` exemplaar. Deze methode retourneert een lijst met bestanden die zijn gekoppeld aan het item met gegevens. (Opmerking: dit is een *lokale* bewerking en levert de bestanden op basis van de status van het object wanneer dit voor het laatst is gesynchroniseerd. Als u een bijgewerkte lijst van bestanden vanaf de server, moet u starten een synchronisatiebewerking eerst.)

        IEnumerable<MobileServiceFile> files = await myTable.GetFilesAsync(myItem);

- De sync-functie gebruikt meldingen vastleggen op de lokale winkel om de records ophalen die de client heeft ontvangen als onderdeel van een push- of pull-bewerking. Dit wordt bereikt door het inschakelen van meldingen voor lokale en server voor de synchronisatie context met de `StoreTrackingOptions` parameter. 

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

      + Andere opties voor bijhouden zijn beschikbaar, zoals lokaal of server alleen meldingen. U kunt toevoegen of eigen aangepaste terugbellen met behulp van de `EventManager` van de eigenschap `IMobileServiceClient`:

            jobService.MobileService.EventManager.Subscribe<StoreOperationCompletedEvent>(StoreOperationEventHandler);

- Het kan toevoegen of verwijderen van bestanden uit een record doordat blob-opslag direct, omdat de koppeling wordt bereikt door een naamgevingsconventie. Echter, in dit geval moet u altijd **bijwerken van de tijdstempel van de record als de bijbehorende BLOB's worden gewijzigd**. De Azure mobiele client SDK werkt altijd een record bij het toevoegen of verwijderen van een bestand. 

    De reden voor deze vereiste is dat sommige mobiele clients, al de record in de lokale opslag. Wanneer deze clients een incrementele pull uitvoert, wordt deze record niet geretourneerd en de client vraagt niet voor de nieuwe gekoppelde bestanden. Als u wilt voorkomen dat dit probleem, is het raadzaam dat u de tijdstempel van record bijwerken tijdens het uitvoeren van een blob storage wijziging die geen van de Azure mobiele client SDK gebruikmaakt.

- De client wordt gebruikt het patroon van [Xamarin.Forms DependencyService] de klasse rechts platformspecifieke laden tijdens runtime. In dit voorbeeld hebben we een interface gedefinieerd `IPlatform` met implementaties in elk van de platform-specifieke projecten.

<!-- URLs. -->

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
[Maak een Xamarin.Forms-app]: app-service-mobile-xamarin-forms-get-started.md
[Xamarin.Forms DependencyService]: https://developer.xamarin.com/guides/xamarin-forms/dependency-service/
[Microsoft.Azure.Mobile.Client.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.Files/
[Microsoft.Azure.Mobile.Client.SQLiteStore]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[Microsoft.Azure.Mobile.Server.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Files/
[Wat is gedeelde toegang handtekeningen]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[Maak een Account Azure opslag]:  ../storage/storage-create-storage-account.md#create-a-storage-account
