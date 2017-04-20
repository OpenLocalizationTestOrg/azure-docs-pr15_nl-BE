<properties 
    pageTitle="Vloeiend Streaming Windows Store App zelfstudie | Microsoft Azure" 
    description="Leren werken met Azure Media Services een C# Windows Store-toepassing inhoud maken met een XML-MediaElement-besturingselement naar goede stroom afspelen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>



#<a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Het bouwen van een soepele toepassing Windows Store Streaming

De soepele Streaming Client SDK voor Windows 8 kunnen ontwikkelaars archief van Windows-toepassingen maken die op verzoek en live-Streaming van goede inhoud kunnen afspelen. Naast het afspelen van de basis van goede Streaming inhoud, dat de SDK biedt ook uitgebreide functies, zoals Microsoft PlayReady bescherming, kwaliteit niveau beperking, DVR Live audio stream overschakelt, luisteren naar statusupdates (zoals kwaliteit verandert) en fouten, enzovoort. Zie de [release-opmerkingen](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes)voor meer informatie van de ondersteunde functies. Voor meer informatie, Zie [kader Player voor Windows 8](http://playerframework.codeplex.com/). 

Deze zelfstudie bevat vier lessen:

1. Een goede Streaming winkel basistoepassing maken
2. Een schuifbalk bepalen de voortgang Media toevoegen
3. Selecteer vloeiend Streaming Streams
4. Goede Streaming Tracks selecteren

##<a name="prerequisites"></a>Vereisten

- 32-bits of 64-bits Windows 8. U kunt [Windows 8 Enterprise evaluatie](http://msdn.microsoft.com/evalcenter/jj554510.aspx) van MSDN krijgen.
- Visual Studio 2012 of 2012 Visual Studio Express (of een latere versie). Vanaf [hier](http://www.microsoft.com/visualstudio/11/downloads)kunt u de proefversie krijgen.
- [Vloeiend Microsoft Streaming Client SDK voor Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).


De voltooide oplossing voor elke les kan worden gedownload van MSDN Developer codevoorbeelden (galerie Code): 

- [Les 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - een eenvoudige Windows 8 vloeiend Streaming MediaPlayer 
- [Les 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - een eenvoudige Windows 8 soepele Streaming MediaPlayer met een schuifbalk bepalen, 
- [Les 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - een Windows 8 vloeiend Streaming MediaPlayer met Stream-selectie  
- [Les 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - een Windows 8 vloeiend Streaming MediaPlayer met Track selectie.

##<a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Les 1: Een goede Streaming winkel basistoepassing maken

In deze les maakt u een archief van Windows-toepassing met een besturingselement MediaElement spelen goede stroom inhoud.  De actieve toepassing ziet:

![Voorbeeld van de toepassing Windows Store voor soepele Streaming][PlayerApplication]
 
Zie voor meer informatie over het ontwikkelen van Windows Store-toepassing [ontwikkelen geweldige Apps voor Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). Deze les bevat de volgende procedures:

1.  Maak een project voor de Windows Store
2.  Ontwerp van de gebruikersinterface (XAML)
3.  De code achter het bestand wijzigen
4.  Compileren en de toepassing testen

**Voor het maken van een project voor de Windows Store**

1.  Visual Studio 2012 of hoger worden uitgevoerd.
2.  Klik op **Nieuw**in het menu **bestand** en klik vervolgens op **Project**.
3.  In het dialoogvenster Nieuw Project, typt of selecteert u de volgende waarden:

Naam|Waarde
---|---
Voor sjabloongroep|Geïnstalleerd/sjablonen/Visual C# / Windows archief
Sjabloon|Lege App (XAML)
Naam|SSPlayer
Locatie|C:\SSTutorials
Oplossingsnaam|SSPlayer
Map voor oplossing maken|(geselecteerd)

4.  Klik op **OK**.

**Een verwijzing naar de goede Streaming Client SDK toevoegen**

1.  In de Solution Explorer met de rechtermuisknop op **SSPlayer**en klik vervolgens op **Add Reference**.
2.  Typ of Selecteer de volgende waarden:

Naam|Waarde
---|---
Verwijzingsgroep|Uitbreidingen voor Windows /
Referentie|Selecteer Microsoft vloeiend Streaming Client SDK voor Windows 8 en Microsoft Visual C++ Runtime-pakket
    
3.  Klik op **OK**. 

Na het toevoegen van verwijzingen, moet u het gerichte platform (x64 of x86), verwijzingen toevoegen voor elke CPU platformconfiguratie werkt.  In solution explorer ziet u gele waarschuwing is ingeschakeld voor deze referenties heeft toegevoegd.

**De player-gebruikersinterface ontwerpen**

1.  In Solution Explorer dubbelklikt u op **MainPage.xaml** om deze te openen in de ontwerpweergave.
2.  Zoek de ** &lt;raster&gt; ** en ** &lt;/Grid&gt; ** de XAML bestand tags en plak de volgende code tussen de twee codes:

        <Grid.RowDefinitions>
            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
        </Grid.RowDefinitions>
        
        <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
        </StackPanel>

        <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
        </StackPanel>

        <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
        </StackPanel>

    Het besturingselement MediaElement wordt gebruikt voor het afspelen van media. De schuifregelaar met de naam sliderProgress wordt in de volgende les worden gebruikt om de voortgang van de media.

3.  Druk op **CTRL + S** om het bestand opslaan.

Het besturingselement MediaElement biedt geen ondersteuning voor soepele Streaming content out-of-box. Om de goede Streaming-ondersteuning inschakelt, moet u de handler bytestream goede Streaming door de bestandsextensie en het MIME-type registreren.  Als u wilt registreren, moet u de methode MediaExtensionManager.RegisterByteStremHandler van de naamruimte Windows.Media gebruiken.

Sommige gebeurtenishandlers worden in dit bestand XAML gekoppeld aan de besturingselementen.  U moet de gebeurtenis-handlers definiëren.

**De code achter het bestand wijzigen**

1.  In Solution Explorer met de rechtermuisknop op **MainPage.xaml**en klikt u vervolgens op **Programmacode weergeven**.
2.  Aan de bovenkant van het bestand, voeg de volgende instructie:

        using Windows.Media;

3.  Toevoegen aan het begin van de klasse **MainPage** de gegevenslid van de volgende:

        private MediaExtensionManager extensions = new MediaExtensionManager();

4.  Voeg de volgende twee regels aan het einde van de constructor **MainPage** :

        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
        
5.  Aan het einde van de klasse **MainPage** voorbij de volgende code:

        #region UI Button Click Events
        private void btnPlay_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Play();
            txtStatus.Text = "MediaElement is playing ...";
        }
        
        private void btnPause_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Pause();
            txtStatus.Text = "MediaElement is paused";
        }
        
        private void btnSetSource_Click(object sender, RoutedEventArgs e)
        {
            sliderProgress.Value = 0;
            mediaElement.Source = new Uri(txtMediaSource.Text);
        
            if (chkAutoPlay.IsChecked == true)
            {
                txtStatus.Text = "MediaElement is playing ...";
            }
            else
            {
                txtStatus.Text = "Click the Play button to play the media source.";
            }
        }
        
        private void btnStop_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Stop();
            txtStatus.Text = "MediaElement is stopped";
        }
        
        private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            txtStatus.Text = "Seek to position " + sliderProgress.Value;
            mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
        }
        #endregion

    Hier definieert u de gebeurtenishandler sliderProgress_PointerPressed.  Er zijn meer works doen om deze werkt, worden behandeld in de volgende les van deze zelfstudie.
6.  Druk op **CTRL + S** om het bestand opslaan.

Het voltooide de code achter het bestand moet er zo uit:

![CodeView in Visual Studio van goede Streaming Windows Store-toepassing][CodeViewPic]

**Om te compileren en de toepassing testen**

1.  Klik in het menu **BUILD** **Configuration Manager**.
2.  **Platform voor actieve oplossing** overeenkomt met uw ontwikkelplatform wijzigen.
3.  Druk op **F6** voor het compileren van het project. 
4.  Druk op **F5** om de toepassing te starten.
5.  Aan de bovenkant van de toepassing, moet u de standaard URL voor soepele Streaming of voer een andere. 
6.  Klik op **bron**. Omdat standaard **Automatisch afspelen** is ingeschakeld, wordt het medium automatisch afgespeeld.  U kunt het medium met het **afspelen**, **onderbreken** en **stoppen van** de knoppen bepalen.  U kunt het Mediavolume met de verticale schuifbalk bepalen.  Maar de horizontale schuifregelaar voor het besturen van de voortgang van de media is nog niet volledig geïmplementeerd. 

Lesson1 is voltooid.  In deze les gebruikt u een besturingselement MediaElement goede Streaming inhoud afspelen.  In de volgende les voegt u een schuifregelaar om de voortgang van de goede Streaming inhoud bepalen.


##<a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Les 2: Een schuifbalk bepalen de voortgang Media toevoegen
In les 1, kunt u een toepassing voor Windows Store gemaakt met een besturingselement MediaElement XAML goede Streaming media-inhoud afspelen.  Het wordt geleverd met sommige algemene media functies zoals starten, stoppen en onderbreken.  In deze les zal u een schuifregelaar opdrachtbalk-besturingselement toevoegen aan de toepassing.

In deze zelfstudie gebruiken we een timer voor het bijwerken van de positie van de schuifregelaar op basis van de huidige positie van het besturingselement MediaElement.  De schuifregelaar begin- en tijd ook moeten worden bijgewerkt in het geval van live inhoud.  Dit kan beter worden verwerkt in de gebeurtenis bron adaptieve update.

Mediabronnen zijn objecten die mediagegevens genereren.  De bron-resolver een URL of een byte-stroom neemt en maakt de juiste mediabron voor die inhoud.  De bronoplossing is de standaardmethode voor de toepassingen voor het maken van media-bronnen. 

Deze les bevat de volgende procedures:

1.  Goede Streaming-handler registreren 
2.  Manager niveau gebeurtenis-handlers adaptieve bron toevoegen
3.  De adaptieve bron niveau gebeurtenis-handlers toevoegen
4.  Gebeurtenis-handlers MediaElement toevoegen
5.  Schuifregelaar gerelateerde code toevoegen
6.  Compileren en de toepassing testen

**De soepele Streaming bytestream handler registreren en doorgeven van de propertyset**

1.  In Solution Explorer, **MainPage.xaml**Klik met de rechtermuisknop en klik vervolgens op **Programmacode weergeven**.
2.  Aan het begin van het bestand, voeg de volgende instructie:

        using Microsoft.Media.AdaptiveStreaming;

3.  Toevoegen aan het begin van de klasse MainPage, de gegevensleden van de volgende:

        private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
        private IAdaptiveSourceManager adaptiveSourceManager;
    
4.  Binnen de constructor **MainPage** toevoegen de volgende code na de **. Initialiseren Components();** regels in de vorige les geschreven regel en de registratie-code:
    
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
    
5.  Wijzig de twee methoden voor RegisterByteStreamHandler toe te voegen binnen de constructor **MainPage** de vermelde parameters:

        // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
        // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
        // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
        propertySet);

6.  Druk op **CTRL + S** om het bestand opslaan.

**De gebeurtenishandler adaptieve bron manager niveau**

1.  In Solution Explorer, **MainPage.xaml**Klik met de rechtermuisknop en klik vervolgens op **Programmacode weergeven**.
2.  Voeg de volgende gegevenslid binnen de klasse **MainPage** :

        private AdaptiveSource adaptiveSource = null;

3.  Toevoegen aan het einde van de klasse **MainPage** wordt de volgende gebeurtenis-handler:
    
        #region Adaptive Source Manager Level Events
        private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
        }
        #endregion Adaptive Source Manager Level Events

4.  Voeg de volgende regel te abonneren op de gebeurtenis openen van adaptieve bron aan het einde van de constructor **MainPage** :
    
    adaptiveSourceManager.AdaptiveSourceOpenedEvent += nieuwe AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);

5.  Druk op **CTRL + S** om het bestand opslaan.

**Adaptieve bron niveau gebeurtenis-handlers toevoegen**

1.  In Solution Explorer, **MainPage.xaml**Klik met de rechtermuisknop en klik vervolgens op **Programmacode weergeven**.
2.  Voeg de volgende gegevenslid binnen de klasse **MainPage** :
    
        private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
        private Manifest manifestObject;
    
3.  Voeg de volgende gebeurtenis-handlers aan het einde van de klasse **MainPage** :

        #region Adaptive Source Level Events
        private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
        }
        
        private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
        {
            adaptiveSourceStatusUpdate = args;
        }
        
        private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
        {
            txtStatus.Text = "Error: " + args.HttpResponse;
        }
        #endregion Adaptive Source Level Events

4.  Voeg de volgende code om u te abonneren op de gebeurtenissen aan het eind van de methode **mediaElement AdaptiveSourceOpened** :
    
        adaptiveSource.ManifestReadyEvent +=
                    mediaElement_ManifestReady;
        adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 
            mediaElement_AdaptiveSourceStatusUpdated;
        adaptiveSource.AdaptiveSourceFailedEvent += 
            mediaElement_AdaptiveSourceFailed;
    
5.  Druk op **CTRL + S** om het bestand opslaan.

Dezelfde gebeurtenissen zijn beschikbaar voor geavanceerde bron Manager niveau, die kan worden gebruikt voor het verwerken van functionaliteit die gemeenschappelijk zijn voor alle media-elementen in de app. Elke AdaptiveSource bevat een eigen gebeurtenissen en gebeurtenissen voor alle AdaptiveSource worden trapsgewijs onder AdaptiveSourceManager.

**Gebeurtenis-handlers voor Media-Element toevoegen**

1.  In Solution Explorer, **MainPage.xaml**Klik met de rechtermuisknop en klik vervolgens op **Programmacode weergeven**.
2.  Voeg de volgende gebeurtenis-handlers aan het einde van de klasse **MainPage** :
    
        #region Media Element Event Handlers 
        private void MediaOpened(object sender, RoutedEventArgs e)
        {
            txtStatus.Text = "MediaElement opened";
        }
        
        private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
        {
            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
        }
        
        private void MediaEnded(object sender, RoutedEventArgs e)
        {
            txtStatus.Text ="MediaElement ended.";
        }
        #endregion Media Element Event Handlers

3.  Voeg de volgende code op subscript op de gebeurtenissen aan het eind van de constructor **MainPage** :
    
        mediaElement.MediaOpened += MediaOpened;
        mediaElement.MediaEnded += MediaEnded;
        mediaElement.MediaFailed += MediaFailed;

4.  Druk op **CTRL + S** om het bestand opslaan.

**Gerelateerde code schuifbalk toevoegen**

1.  In Solution Explorer, **MainPage.xaml**Klik met de rechtermuisknop en klik vervolgens op **Programmacode weergeven**.
2.  Aan het begin van het bestand, voeg de volgende instructie:
    
        using Windows.UI.Core;

3.  Voeg de volgende gegevensleden binnen de klasse **MainPage** :
    
        public static CoreDispatcher _dispatcher;
        private DispatcherTimer sliderPositionUpdateDispatcher;
    
4.  Voeg de volgende code aan het einde van de constructor **MainPage** :

        _dispatcher = Window.Current.Dispatcher;
        PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
        sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
    
5.  Voeg de volgende code aan het einde van de klasse **MainPage** :
    
        #region sliderMediaPlayer
        private double SliderFrequency(TimeSpan timevalue)
        {
            long absvalue = 0;
            double stepfrequency = -1;
        
            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }
        
            TimeSpan totalDVRDuration = new TimeSpan(absvalue);
        
            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }
        
            return stepfrequency;
        }
        
        void updateSliderPositionoNTicks(object sender, object e)
        {
            sliderProgress.Value = mediaElement.Position.TotalSeconds;
        }
        
        public void setupTimer()
        {
            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
        }

        public void startTimer()
        {
            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderStartTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderEndTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
        }
        #endregion sliderMediaPlayer

    **Opmerking:** CoreDispatcher wordt wijzigingen aanbrengen in de UI-thread van UI-Thread niet gebruikt. Bij een knelpunt op dispatcher-thread kunt ontwikkelaar gebruiken verzender geleverd door gebruikersinterface-element hij voornemens is om bij te werken.  Bijvoorbeeld:
    
        await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 
          timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
        double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 
          sliderProgress.Maximum = absvalue; }); 
        

6.  Voeg de volgende code aan het einde van de methode **mediaElement_AdaptiveSourceStatusUpdated** :
    
        setSliderStartTime(args.StartTime);
        setSliderEndTime(args.EndTime);

7.  Voeg de volgende code aan het einde van de methode **MediaOpened** :
    
    sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan); sliderProgress.Width = mediaElement.Width; setupTimer();

8.  Druk op **CTRL + S** om het bestand opslaan.

**Om te compileren en de toepassing testen**

1. Druk op **F6** voor het compileren van het project. 
2.  Druk op **F5** om de toepassing te starten.
3.  Aan de bovenkant van de toepassing, moet u de standaard URL voor soepele Streaming of voer een andere. 
4.  Klik op **bron**. 
5.  Test de schuifbalk.

Les 2 is voltooid.  In deze les kunt u een schuifbalk toegevoegd aan de toepassing. 

##<a name="lesson-3-select-smooth-streaming-streams"></a>Les 3: Selecteer vloeiend Streaming Streams
Goede Streaming is in staat om inhoud stroomsgewijs verzenden met meerdere taal-audiotracks die kunnen geselecteerd door de kijkers worden.  In deze les kunnen kijkers streams te selecteren. Deze les bevat de volgende procedures:

1. De XAML bestand wijzigen
2. Wijzig het bestand code behand
3. Compileren en de toepassing testen


**De XAML bestand wijzigen**

1. In de Solution Explorer met de rechtermuisknop op **MainPage.xaml**en klik vervolgens op **Ontwerp weergeven**.
2. Ga naar &lt;Grid.RowDefinitions&gt;, en de RowDefinitions wijzigen zodat ze er uitziet als:

        <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
        </Grid.RowDefinitions>

3. In de &lt;raster&gt;&lt;/Grid&gt; -tags, voeg de volgende code definieert een listbox-besturingselement, zodat gebruikers kunnen de lijst met beschikbare streams en streams selecteren:

        <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
        </Grid>

4. Druk op **CTRL + S** om de wijzigingen opslaan.


**De code achter het bestand wijzigen**

1. In Solution Explorer met de rechtermuisknop op **MainPage.xaml**en klikt u vervolgens op **Programmacode weergeven**.
2. Een nieuwe klasse toevoegen in de naamruimte SSPlayer: #region-klasse Stream
    
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
    
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
    
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
    
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream

3. Aan het begin van de klasse MainPage, voeg de volgende variabele definities:

        private List<Stream> availableStreams;
        private List<Stream> availableAudioStreams;
        private List<Stream> availableTextStreams;
        private List<Stream> availableVideoStreams;

4. Binnen de klasse MainPage toevoegen de volgende regio:

        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
        
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
        
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
        
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
        
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
        
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
        
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
        
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
        
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
        
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
        
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
        
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection

5. Zoek de mediaElement_ManifestReady-methode, de volgende code aan het einde van de functie toegevoegd:
    
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();

    Dus wanneer MediaElement manifest klaar is, wordt de code een lijst van de beschikbare streams haalt en de keuzelijst UI met de lijst vult.

6. Zoek de gebruikersinterface in de klasse MainPage knoppen klikt u op gebeurtenissen regio en voeg de volgende functiedefinitie:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Om te compileren en de toepassing testen**

1. Druk op **F6** voor het compileren van het project. 
2.  Druk op **F5** om de toepassing te starten.
3.  Aan de bovenkant van de toepassing, moet u de standaard URL voor soepele Streaming of voer een andere. 
4.  Klik op **bron**. 
5.  De standaardtaal is audio_eng. Probeer om te schakelen tussen audio_eng en audio_es. Telkens wanneer, selecteert u een nieuwe gegevensstroom, moet u de knop verzenden.

Les 3 is voltooid.  In deze les kunt u de functionaliteit om streams toevoegen.

##<a name="lesson-4-select-smooth-streaming-tracks"></a>Les 4: Goede Streaming Tracks selecteren
Een goede Streaming presentatie kan bevatten meerdere FLV-bestanden gecodeerd met verschillende kwaliteitsniveaus (bitsnelheden) en resoluties. In deze les kunt u gebruikers tracks te selecteren. Deze les bevat de volgende procedures:

1. De XAML bestand wijzigen
2. Wijzig het bestand code behand
3. Compileren en de toepassing testen

**De XAML bestand wijzigen**

1. In de Solution Explorer met de rechtermuisknop op **MainPage.xaml**en klik vervolgens op **Ontwerp weergeven**.
2. Zoek de &lt;raster&gt; label met de naam **gridStreamAndBitrateSelection**, de volgende code aan het einde van de code toegevoegd:

        <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
        </StackPanel>

3. Druk op **CTRL + S** hij wijzigingen op te slaan


**De code achter het bestand wijzigen**

1. In Solution Explorer met de rechtermuisknop op **MainPage.xaml**en klikt u vervolgens op **Programmacode weergeven**.
2. In de naamruimte SSPlayer een nieuwe klasse toevoegen:
    
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
    
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
    
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
    
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track

3. Aan het begin van de klasse MainPage, voeg de volgende variabele definities:
    
        private List<Track> availableTracks;

4. Binnen de klasse MainPage toevoegen de volgende regio:
    
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>

        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();

            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;

            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;

                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }

        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }

        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }

        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }

        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection

5. Zoek de mediaElement_ManifestReady-methode, de volgende code aan het einde van de functie toegevoegd:

        getTracks(manifestObject);
        refreshAvailableTracksListBoxItemSource();

6. Zoek de gebruikersinterface in de klasse MainPage knoppen klikt u op gebeurtenissen regio en voeg de volgende functiedefinitie:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Om te compileren en de toepassing testen**

1. Druk op **F6** voor het compileren van het project. 
2.  Druk op **F5** om de toepassing te starten.
3.  Aan de bovenkant van de toepassing, moet u de standaard URL voor soepele Streaming of voer een andere. 
4.  Klik op **bron**. 
5.  Standaard zijn alle tracks van de videostream geselecteerd. U kunt als u wilt experimenteren de bit rate wijzigingen, de laagste beschikbare bitsnelheid selecteren en selecteert de hoogste beschikbare bitsnelheid. Klikt u op indienen na elke wijziging.  Hier ziet u de wijzigingen van de videokwaliteit.

Les 4 is voltooid.  In deze les kunt u de functionaliteit als u nummers wilt toevoegen.


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="other-resources"></a>Andere bronnen:
- [Hoe maakt een soepele Streaming Windows 8 JavaScript-toepassing met geavanceerde functies](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
- [Goede Streaming-technisch overzicht](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png
 
