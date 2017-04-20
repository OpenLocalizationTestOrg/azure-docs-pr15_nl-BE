<properties 
    pageTitle="Advertenties aan de clientzijde invoegen | Microsoft Azure" 
    description="In dit onderwerp wordt beschreven hoe advertenties aan de clientzijde invoegen." 
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


#<a name="inserting-ads-on-the-client-side"></a>Advertenties aan de clientzijde invoegen

Dit onderwerp bevat informatie over het invoegen van verschillende soorten advertenties op de client.

Zie voor meer informatie over de ondersteuning van gesloten captioning en ad in Live streaming video's [ondersteund Closed Captioning en Ad plaatsingskosten normen](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

>[AZURE.NOTE] Azure Media Player ondersteunt momenteel geen advertenties.

##<a id="insert_ads_into_media"></a>Invoegen van advertenties in de Media

Azure Media Services biedt ondersteuning voor ad invoegen via de Windows Media-Platform: Frameworks-speler. Player kaderregelingen met ad-ondersteuning zijn beschikbaar voor Windows 8, Silverlight, Windows Phone 8 en iOS-apparaten. Elke speler framework bevat voorbeeldcode waarmee het implementeren van een toepassing van de speler. Er zijn drie verschillende soorten advertenties die u in uw media: lijst invoegen kunt.

- **Lineair** – volledig frame advertenties die de belangrijkste video pauzeren.
- **Nonlinear** – overlay-advertenties die worden weergegeven als de belangrijkste video wordt afgespeeld, meestal een logo of een andere statische afbeelding in Windows media player worden geplaatst.
- **Companion** – advertenties die buiten Windows media player worden weergegeven.

Advertenties kunnen op elk punt in de tijdlijn van de belangrijkste video worden geplaatst. U moet Windows media player te zien bij het afspelen van de advertentie en welke advertenties af te spelen. Dit wordt gedaan met behulp van een set van standaard XML-bestanden: Video Ad-Service-sjabloon (VAST), digitale Video meerdere Ad afspeellijst (VMAP), Media abstracte volgordebepaling sjabloon (b) en digitale Video Player Ad Interface Definition (VPAID). GROTE bestanden opgeven welke advertenties weer te geven. VMAP bestanden opgeven bij het afspelen van verschillende advertenties en ENORME XML bevatten. MAST-bestanden zijn een andere manier om een reeks advertenties die ook VAST XML kan bevatten. VPAID bestanden definiëren voor een interface tussen de videospeler en het ad of Active Directory-server.

Elke speler framework werkt anders en elk in een eigen onderwerp aan bod. Dit onderwerp beschrijft de fundamentele mechanismen gebruikt voor het invoegen van advertenties. Advertenties aanvragen videospeler toepassingen bij een Active Directory-server. De ad-server kan reageren op een aantal manieren:

- Een groot bestand retourneren
- Retourneren van een VMAP-bestand (met ingesloten VAST)
- Retourneren van een MAST-bestand (met ingesloten VAST)
- Retourneren van een groot bestand met VPAID advertenties

 
###<a name="using-a-video-ad-service-template-vast-file"></a>Met behulp van een Video-advertentie-Service (VAST)-sjabloonbestand

Een groot bestand geeft aan welke ad of advertenties weer te geven. De volgende XML-code is een voorbeeld van een groot bestand voor een lineaire advertentie:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
    
De lineaire advertentie wordt beschreven door de **<Linear>** element. Hiermee geeft u aan de duur van de advertentie, het bijhouden van gebeurtenissen, klik op de link klikken bijhouden, en een aantal **<MediaFile>** elementen. Bijhouden van gebeurtenissen zijn opgegeven in de **<TrackingEvents>** element, waardoor een ad-server voor het bijhouden van verschillende gebeurtenissen die plaatsvinden tijdens het bekijken van de advertentie. In dit geval het begin, midden, voltooid, en vouw gebeurtenissen worden bijgehouden. De startgebeurtenis treedt op wanneer de advertentie wordt weergegeven. Het middelpunt gebeurtenis plaats wanneer ten minste 50% van de tijdlijn van de advertentie heeft bekeken. De complete-gebeurtenis treedt op wanneer de advertentie is uitgevoerd naar het einde. De gebeurtenis uitvouwen vindt plaats wanneer de gebruiker de videospeler breidt uit naar volledig scherm. Clickthroughs worden aangeduid met een **<ClickThrough>** -element binnen een **<VideoClicks>** element en geeft u een URI voor een resource weer te geven wanneer de gebruiker op de advertentie klikt. ClickTracking is opgegeven in een **<ClickTracking>** -element, ook in de **<VideoClicks>** element en geeft u een resource bijhouden voor de speler om te vragen wanneer de gebruiker op de advertentie klikt. De **<MediaFile>** elementen geven informatie over een bepaalde codering van een advertentie. Als er meer dan een **<MediaFile>** -element, de videospeler kunt kiezen de beste codering van het platform. 

Lineaire advertenties kunnen worden weergegeven in een opgegeven volgorde. Hiertoe toevoegen extra <Ad> elementen aan de VAST-bestand en de volgorde met behulp van het kenmerk volgorde opgeven. In het volgende voorbeeld illustreert dit:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
    
Niet-lineaire advertenties zijn opgegeven in een <Creative> element ook. Het volgende voorbeeld ziet u een <Creative> -element dat een niet-lineaire advertentie wordt beschreven.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>

 
De **<NonLinearAds>** element kan bevatten een of meer **<NonLinear>** elementen, die elk een niet-lineaire ad kunt beschrijven. De **<NonLinear>** element geeft de bron voor de niet-lineaire advertentie. De resource kan bestaan uit een **<StaticResouce>**, een **<IFrameResource>**, of een **<HTMLResouce>**.**<StaticResource>** Beschrijving van een HTML-bron en definieert een creativeType attribuut geeft aan hoe de resource wordt weergegeven:

Image/gif, image/JPEG-, image/png: de bron wordt weergegeven in een HTML- **<img>** code.

Application/x-javascript: de bron wordt weergegeven in een HTML-code <**script**>-code.

Application/x-shockwave-flash: de bron wordt weergegeven in een Flash-speler.

**<IFrameResource>**Beschrijving van een HTML-bron die kan worden weergegeven in een IFrame. **<HTMLResource>**Beschrijving van een stukje HTML-code die kan worden ingevoegd in een webpagina. **<TrackingEvents>**bijhouden van gebeurtenissen en de URI aan te vragen bij de gebeurtenis opgeven. In dit voorbeeld worden de gebeurtenissen acceptInvitation en samenvouwen bijgehouden. Voor meer informatie over de **<NonLinearAds>** -element en onderliggende, Zie IAB.NET/VAST. Houd er rekening mee dat de **<TrackingEvents>** element zich bevindt binnen de** <NonLinearAds> ** element in plaats van de **<NonLinear>** element.

Companion-advertenties zijn gedefinieerd binnen een <CompanionAds> element. De <CompanionAds> element kan bevatten een of meer <Companion> elementen. Elke <Companion> element beschrijft een companion ad en kan bestaan uit een <StaticResource>, <IFrameResource>, of <HTMLResource> die zijn opgegeven op dezelfde manier als in een niet-lineaire ad. Een groot bestand kan meerdere companion advertenties bevatten en de player-toepassing kunt kiezen de meest geschikte advertentie weer te geven. Zie voor meer informatie over VAST [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

###<a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Met behulp van een digitale Video meerdere door Ad afspeellijstbestand (VMAP)

Een VMAP-bestand kunt u opgeven wanneer ad-einden, hoe lang elk einde is, hoeveel advertenties kunnen worden weergegeven binnen een einde en welke soorten advertenties kunnen worden weergegeven tijdens een pauze. In een voorbeeld van een VMAP bestand dat een enkele ad-einde definieert het volgende:
    
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
     
Een VMAP-bestand begint met een <VMAP> element een of meer bevat <AdBreak> elementen, die elk een ad-einde definieert. Elk ad-einde bevat een type pauze, einde-ID en time-offset. Het kenmerk breakType geeft het type van advertentie die kan worden afgespeeld tijdens de pauze: lineaire, niet-lineaire, of weer te geven. Advertenties kaart VAST companion advertenties weergeven. Meer dan één type van advertentie kan worden opgegeven in een lijst gescheiden door een komma (zonder spaties). De breakID is een optionele aanduiding voor de advertentie. De timeOffset geeft aan wanneer de advertentie moet worden weergegeven. Deze u kunt op een van de volgende manieren opgeven:

1. Tijd: mm: ss of hh:mm:ss.mmm indeling waarbij .mmm wordt in milliseconden. De waarde van dit kenmerk geeft de tijd vanaf het begin van de video-tijdlijn aan het begin van het einde van het ad.
1. Percentage-indeling n % is waarbij n het percentage van de tijdlijn van de video af te spelen voordat het afspelen van de advertentie
1. Begin/einde: Hiermee geeft u aan dat een advertentie moet worden weergegeven vóór of na de video is weergegeven.
1. Plaats: geeft de volgorde van de ad-einden als de timing van de ad-einden onbekend zijn, zoals live streaming is. De volgorde van elk einde ad is opgegeven in de #n indeling waarbij n een geheel getal 1 of groter is. 1 geeft aan dat de advertentie moet worden afgespeeld bij de eerste gelegenheid, 2 geeft aan dat de advertentie moet worden afgespeeld bij de tweede gelegenheid enzovoort.

Binnen het element <**AdBreak**> kan er één <**AdSource**>-element. Het <**AdSource**>-element bevat de volgende kenmerken:

1. -ID: Hiermee geeft u een id voor de ad-bron
1. allowMultipleAds – een Booleaanse waarde die aangeeft of meerdere advertenties kunnen worden weergegeven tijdens de pauze ad
1. followRedirects – een optionele Booleaanse waarde die opgeeft als moet ingaan op de videospeler leidt binnen een respons ad

Het element <**AdSource**> biedt de speler een inline ad antwoord of een verwijzing naar een ad-antwoord. Dit kan een van de volgende elementen bevatten:

- <VASTAdData>Geeft aan dat een OVERGROTE ad antwoord is ingesloten in het bestand VMAP
- <AdTagURI>een URI die verwijst naar een ad-reactie van een ander systeem
- <CustomAdData>-een willekeurige tekenreeks die respresents een antwoord niet VAST

In dit voorbeeld een antwoord in de regel ad is opgegeven met een <VASTAdData> element met een grote ad-antwoord. Zie [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap)voor meer informatie over de andere elementen.

Het element <**AdBreak**> kan ook één <**TrackingEvents**>-element bevatten. Het element <**TrackingEvents**> kunt u het begin of einde van een ad-einde of of is een fout opgetreden tijdens de pauze ad bijhouden. Het <**TrackingEvents**>-element bevat een of meer <**Tracking**>-elementen, die elk een gebeurtenis bijhouden en een tracking URI bevat. Er zijn de gebeurtenissen mogelijk bijhouden:

1. breakStart – het begin van een ad-einde wordt bijgehouden
1. breakEnd – de voltooiing van een ad-einde bijhouden
1. Fout: een fout die is opgetreden tijdens de pauze ad worden bijgehouden

In het volgende voorbeeld ziet u een VMAP-bestand dat de gebeurtenissen bijhouden

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

Zie voor meer informatie over het element <**TrackingEvents**> en de onderliggende http://iab.org/VMAP.pdf

###<a name="using-a-media-abstract-sequencing-template-mast-file"></a>Met behulp van een Media-Abstract volgordebepaling sjabloonbestand (b)

Een bestand MAST kunt u opgeven triggers die bepalen wanneer een advertentie wordt weergegeven. Hier volgt een voorbeeld van de MAST bestand met triggers voor een rol voor ad, een advertentie halverwege roll en een advertentie na roll.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>

 

Een MAST-bestand begint met een **<MAST>** element met een **<triggers>** element. De <triggers> -element bevat een of meer **<trigger>** elementen die bepalen wanneer een advertentie moet worden afgespeeld. 

De **<trigger>** -element bevat een **<startConditions>** element opgeven waarop een advertentie moet beginnen te spelen. De **<startConditions>** -element bevat een of meer <condition> elementen. Bij elke <condition> resulteert in waar een trigger is gestart of ingetrokken afhankelijk van of u de <condition> zich in een **< startConditions**> of **<endConditions>** element respectievelijk. Wanneer meerdere <condition> elementen aanwezig zijn, maar worden behandeld als een impliciete OR, een willekeurige voorwaarde evalueren op true, wordt de trigger starten. <condition>elementen kunnen worden genest. Als kind <condition> elementen zijn vooraf ingesteld, maar worden behandeld als een impliciete en alle voorwaarden moeten resulteren in waar de trigger starten. De <condition> -element bevat de volgende kenmerken die de voorwaarde te definiëren: 

1. **type** : Hiermee wordt het type voorwaarde, gebeurtenis of eigenschap
1. **naam** : de naam van de eigenschap of de gebeurtenis moet worden gebruikt tijdens de beoordeling
1. **waarde** : de waarde die een eigenschap vergeleken
1. **exploitant** : de bewerking moet worden gebruikt tijdens de evaluatie: EQ (equal), NEQ (niet gelijk aan), GTR (groter), GEQ (groter of gelijk), LT (kleiner dan), LEQ (kleiner dan of gelijk aan), MOD (modulo)

**<endConditions>**ook bevatten <condition> elementen. Wanneer een voorwaarde waar de trigger wordt opnieuw ingesteld. De <trigger> -element bevat ook een <sources> element een of meer bevat <source> elementen. De <source> elementen bepalen de URI naar de ad-reactie en het type reactie ad. In dit voorbeeld wordt een URI aan een VAST antwoord gegeven. 


    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
 

###<a name="using-video-player-ad-interface-definition-vpaid"></a>Met behulp van Video Player-Ad interfacedefinitie (VPAID)

VPAID is een API voor het inschakelen van uitvoerbare ad eenheden om te communiceren met een videospeler. Hiermee kunt interactieve ad ervaringen. De gebruiker kan communiceren met de advertentie en de advertentie kan reageren op acties van de viewer. Een advertentie kan bijvoorbeeld knoppen waarmee de gebruiker om meer informatie of een langere versie van de advertentie weergegeven. De videospeler ondersteunt de API VPAID en de uitvoerbare advertentie moet de API worden geïmplementeerd. Als een speler vraagt om dat een advertentie van een ad-server de server reageert met een VAST antwoord met een VPAID ad.

Een advertentie voor uitvoerbare wordt code die moet worden uitgevoerd in een runtimeomgeving zoals Adobe Flash™ of JavaScript die kan worden uitgevoerd in een webbrowser gemaakt. Wanneer u een ad-server retourneert een VAST antwoord met een VPAID ad, de waarde van de apiFramework van het kenmerk de <MediaFile> element moet "VPAID". Dit kenmerk geeft aan dat de advertentie die een uitvoerbare ad VPAID. Het typekenmerk moet worden ingesteld op het MIME-type van het uitvoerbare bestand, zoals "application/x-shockwave-flash" of "application/x-javascript". De volgende XML-fragment wordt getoond de <MediaFile> -element uit een VAST antwoord met een uitvoerbare ad VPAID. 

    
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
 

Een uitvoerbaar ad worden geïnitialiseerd met behulp van de <AdParameters> -element in de <Linear> of <NonLinear> -elementen in een VAST antwoord. Voor meer informatie over de <AdParameters> -element, Zie [3.0 VAST](http://www.iab.net/media/file/VASTv3.0.pdf). Zie voor meer informatie over de API VPAID [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

##<a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Implementatie van een Windows- of Windows Phone 8 Player met ondersteuning voor Ad

Het Platform Microsoft Media: Player Framework voor Windows 8 en Windows Phone 8 bevat een collectie voorbeeldtoepassingen waarin wordt beschreven hoe u een videospeler-toepassing met behulp van het kader te implementeren. Het kader van de speler en de monsters kunt u downloaden van de [speler Framework voor Windows 8 en Windows Phone 8](https://playerframework.codeplex.com).

Bij het openen van de oplossing Microsoft.PlayerFramework.Xaml.Samples ziet u een aantal mappen binnen het project. De reclame-map bevat de voorbeeldcode die relevant zijn voor het maken van een video-speler met ondersteuning voor ad. In de reclame is map een aantal XAML/cs bestanden weergeven waarvan het invoegen van advertenties op een andere manier. De volgende lijst worden beschreven:

- AdPodPage.xaml laat zien hoe een pod ad weergeven.
- AdSchedulingPage.xaml geeft het plannen van advertenties.
- FreeWheelPage.xaml laat zien hoe de FreeWheel-invoegtoepassing gebruiken voor het plannen van advertenties.
- MastPage.xaml laat zien hoe advertenties met een MAST bestand plannen.
- ProgrammaticAdPage.xaml geeft het plannen van advertenties via programmacode in een video.
- ScheduleClipPage.xaml laat zien hoe een advertentie zonder een groot bestand te plannen.
- VastLinearCompanionPage.xaml geeft een lineaire invoegen en aanvullende ad.
- VastNonLinearPage.xaml laat zien hoe een niet-lineaire ad invoegen.
- VmapPage.xaml laat zien hoe advertenties met een VMAP-bestand opgeven.

Elk van deze monsters wordt gebruikt voor de klasse MediaPlayer is gedefinieerd in het kader van de speler. De meeste monsters gebruiken plugins die ondersteuning voor verschillende ad antwoord indelingen toevoegen. Het monster ProgrammaticAdPage communiceert via programmacode met een exemplaar met MediaPlayer.

###<a name="adpodpage-sample"></a>Voorbeeld AdPodPage

In dit voorbeeld gebruikt de AdSchedulerPlugin om te bepalen wanneer een advertentie wordt weergegeven. In dit voorbeeld wordt een advertentie halverwege vorig gepland na 5 seconden wordt afgespeeld. De ad-pod (een groep van advertenties in volgorde) is opgegeven in een groot bestand geretourneerd van een ad-server. De URI voor het bestand VAST die is opgegeven in de <RemoteAdSource> element.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
    
        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>
    
                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>
    
                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

Zie voor meer informatie over de AdSchedulerPlugin, [reclame in het kader van de Player op Windows 8 en Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

###<a name="adschedulingpage"></a>AdSchedulingPage

In dit voorbeeld wordt ook gebruikt voor de AdSchedulerPlugin. Hiermee plant u drie advertenties, een advertentie vóór roll een advertentie halverwege roll en een advertentie na roll. De URI voor het VAST voor elke advertentie die is opgegeven in een <RemoteAdSource> element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>
    
                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


###<a name="freewheelpage"></a>FreeWheelPage

In dit voorbeeld gebruikt de FreeWheelPlugin die een bron-kenmerk waarmee een URI die verwijst naar een bestand SmartXML waarmee ad inhoud zoals ad planningsgegevens.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="mastpage"></a>MastPage

In dit voorbeeld gebruikt de MastSchedulerPlugin waarmee u een bestand MAST te gebruiken. Het attribuut bron geeft de locatie van het bestand MAST.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="programmaticadpage"></a>ProgrammaticAdPage

In dit voorbeeld wordt via een programma communiceert met de MediaPlayer. Het bestand ProgrammaticAdPage.xaml wordt de Media Player:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

Het ProgrammaticAdPage.xaml.cs maakt een AdHandlerPlugin, voegt u een TimelineMarker op te geven wanneer een advertentie moet worden weergegeven en wordt vervolgens een handler voor de gebeurtenis MarkerReached die wordt geladen een RemoteAdSource een URI in een groot bestand op te geven en de advertentie wordt afgespeeld.
    
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;
    
            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }
    
            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

###<a name="scheduleclippage"></a>ScheduleClipPage

Dit voorbeeld gebruikt de AdSchedulerPlugin een advertentie halverwege roll plannen door te geven van een WMV-bestand met de advertentie.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearcompanionpage"></a>VastLinearCompanionPage

In dit voorbeeld ziet u hoe de AdSchedulerPlugin gebruiken voor het plannen van een lineaire ad halverwege roll met een advertentie companion. De <RemoteAdSource> element geeft de locatie van het bestand VAST.
    
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage

In dit voorbeeld wordt de AdSchedulerPlugin voor het plannen van een lineaire en een niet-lineaire ad. De locatie van de ENORME is opgegeven met de <RemoteAdSource> element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
                            
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vmappage"></a>VMAPPage

Deze monsters maakt gebruik van de VmapSchedulerPlugin voor het plannen van advertenties met een VMAP-bestand. De URI naar het bestand VMAP is opgegeven in het kenmerk van de bron van de <VmapSchedulerPlugin> element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

##<a name="implementing-an-ios-video-player-with-ad-support"></a>Implementatie van een iOS-Player Video met ondersteuning voor Ad


Het Platform Microsoft Media: Framework voor iOS-speler bevat een collectie voorbeeldtoepassingen waarin wordt beschreven hoe u een videospeler-toepassing met behulp van het kader te implementeren. Het kader van de speler en de monsters kunt u downloaden van [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework). De pagina github heeft een koppeling naar een Wiki met aanvullende informatie in het kader van de speler en een inleiding tot het monster player: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).


###<a name="scheduling-ads-with-vmap"></a>Advertenties met VMAP plannen

In het volgende voorbeeld ziet u hoe advertenties met een VMAP plannen.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest
    
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

###<a name="scheduling-ads-with-vast"></a>Advertenties met VAST plannen

In het volgende voorbeeld ziet u hoe een late binding VAST ad plannen.
    
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
         
   In het volgende voorbeeld ziet u hoe een vroege binding VAST ad plannen.
Voorbeeld: 4-schema als een vroege binding VAST ad //Download de VAST-bestand (! [ framework.adResolver downloadManifest: & manifest withURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[self logFrameworkError];} else {adLinearTime.startTime = 7; adLinearTime.duration = 0;
        
        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

In het volgende voorbeeld ziet u hoe een advertentie met ruwe knippen bewerken (RCE) invoegen

    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

In het volgende voorbeeld ziet u hoe een ad-pod plannen.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;
    
    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

In het volgende voorbeeld ziet u hoe een niet-sticky halverwege roll ad plannen. Een niet-sticky ad alleen afgespeeld zodra ongeacht eventuele op zoek naar de viewer worden uitgevoerd.
    
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

In het volgende voorbeeld ziet u hoe een sticky halverwege roll ad plannen. Een sticky advertentie wordt weergegeven telkens wanneer die het opgegeven punt op de tijdlijn van de video is bereikt.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


In het volgende voorbeeld ziet u hoe een advertentie na roll plannen.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

In het volgende voorbeeld ziet u hoe een advertentie roll vooraf plannen.
    
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

In het volgende voorbeeld ziet u hoe een halverwege roll-overlay-advertentie plannen.
    
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
##<a name="see-also"></a>Zie ook

[Video player-toepassingen ontwikkelen](media-services-develop-video-players.md)
