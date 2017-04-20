<properties
    pageTitle="Hoe u Blob-opslag van Xamarin | Microsoft Azure"
    description="Azure opslag-Client naar de bibliotheek Xamarin kunnen ontwikkelaars maken iOS, Android en Windows Store apps met hun eigen gebruikersinterface. Deze zelfstudie laat zien hoe een toepassing die gebruikmaakt van Azure Blob-opslag maken met Xamarin."
    services="storage"
    documentationCenter="xamarin"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-xamarin"></a>Hoe u Blob-opslag van Xamarin

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Overzicht

Xamarin kunnen ontwikkelaars gebruik van een gedeelde C# codebase iOS, Android en Windows Store apps maken met hun eigen gebruikersinterface. In deze zelfstudie wordt beschreven hoe u Azure Blob-opslag met een Xamarin-toepassing gebruiken. Zie [Inleiding tot Microsoft Azure opslag](storage-introduction.md)als u weten over Azure opslag wilt, vooraleer de code.

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Maak een nieuwe toepassing van Xamarin

Voor deze aan de slag, maakt we een app die gericht is op Android, iOS en Windows. Dit app zal gewoon maken van een container en uploaden van een blob in deze container. We zullen worden met Visual Studio op Windows voor deze aan de slag, maar de dezelfde geleerde lessen kunnen worden toegepast bij het maken van een app met Xamarin Studio in Mac OS.

Ga als volgt te werk om uw toepassing te maken:

1. Als u nog niet gedaan hebt, download en installeer [Xamarin voor Visual Studio](https://www.xamarin.com/download).
2. Visual Studio openen en maken van een lege App (Native gedeeld): **Bestand > Nieuw > Project > platforms > lege App(Native Shared)**.
3. Uw oplossing in het deelvenster Solution Explorer met de rechtermuisknop en selecteer **Beheren NuGet pakketten voor oplossing**. Zoeken naar **WindowsAzure.Storage** en installeer de nieuwste stabiele versie voor alle projecten in de oplossing.
4. Bouwen en uitvoeren van uw project.

U hebt nu een toepassing waarmee u op een knop die een teller opgehoogd.

> [AZURE.NOTE] Azure opslag-Client naar de bibliotheek Xamarin ondersteunt momenteel de volgende projecttypen: Native gedeelde gedeelde Xamarin.Forms, Xamarin.Android en Xamarin.iOS.

## <a name="create-container-and-upload-blob"></a>Container maken en uploaden van blob

Vervolgens voegt u code aan de gedeelde klasse `MyClass.cs` die een container maakt en uploadt een blob in deze container. `MyClass.cs`ziet er als volgt uit:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;

    namespace XamarinApp
    {
        public class MyClass
        {
            public MyClass ()
            {
            }

            public static async Task createContainerAndUpload()
            {
                // Retrieve storage account from connection string.
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

                // Create the blob client.
                CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

                // Retrieve reference to a previously created container.
                CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

                // Create the container if it doesn't already exist.
                await container.CreateIfNotExistsAsync();

                // Retrieve reference to a blob named "myblob".
                CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

                // Create the "myblob" blob with the text "Hello, world!"
                await blockBlob.UploadTextAsync("Hello, world!");
            }
        }
    }

Zorg ervoor dat "your_account_name_here" en "your_account_key_here" vervangen door de werkelijke accountnaam en de sleutel van de account. U kunt deze gedeelde klasse in uw iOS, Android en Windows Phone-toepassing. U kunt alleen toe te voegen `MyClass.createContainerAndUpload()` aan elk project. Bijvoorbeeld:

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

    using Android.App;
    using Android.Widget;
    using Android.OS;

    namespace XamarinApp.Droid
    {
        [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
        public class MainActivity : Activity
        {
            int count = 1;

            protected override async void OnCreate (Bundle bundle)
            {
                base.OnCreate (bundle);

                // Set our view from the "main" layout resource
                SetContentView (Resource.Layout.Main);

                // Get our button from the layout resource,
                // and attach an event to it
                Button button = FindViewById<Button> (Resource.Id.myButton);

                button.Click += delegate {
                    button.Text = string.Format ("{0} clicks!", count++);
                };

              await MyClass.createContainerAndUpload();
            }
        }
    }

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

    using System;
    using UIKit;

    namespace XamarinApp.iOS
    {
        public partial class ViewController : UIViewController
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.createContainerAndUpload();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Navigation;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

    namespace XamarinApp.WinPhone
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.createContainerAndUpload();
            }
        }
    }


## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

U kunt deze toepassing nu uitvoeren in een Android of Windows Phone emulator. U kunt deze toepassing ook uitvoeren in een iOS-emulator, maar dit vereist een Mac. Lees de documentatie bij [Visual Studio om een Mac te verbinden](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/) voor specifieke instructies over hoe u dit doet,

Zodra u uw toepassing uitvoert, maakt zij de container `mycontainer` in uw account voor opslag. Erin de blob, `myblob`, die de tekst, is `Hello, world!`. U kunt dit controleren met behulp van de [Microsoft Azure Opslagverkenner](http://storageexplorer.com/).

## <a name="next-steps"></a>Volgende stappen

In deze Introductie, hebt u geleerd hoe een toepassing voor meerdere platforms in Xamarin met Azure opslag maken. Deze aan de slag met name gericht op een scenario in de Blob-opslag. Echter, kunt u doen veel meer met niet alleen de Blob-opslag, maar ook met tabel-, bestands- en wachtrij-opslag. Kijk eens op de volgende artikelen voor meer informatie:
- [Aan de slag met Azure Blob-opslag met behulp van .NET](storage-dotnet-how-to-use-blobs.md)
- [Aan de slag met Azure tabelopslag met behulp van .NET](storage-dotnet-how-to-use-tables.md)
- [Aan de slag met Azure Queue Storage met .NET](storage-dotnet-how-to-use-queues.md)
- [Aan de slag met Azure opslaan van bestanden in Windows](storage-dotnet-how-to-use-files.md)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]
