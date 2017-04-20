<properties
    pageTitle="Met behulp van meerdere invoerbestanden en eigenschappen met Premium Encoder | Microsoft Azure"
    description="In dit onderwerp wordt uitgelegd hoe u met behulp van setRuntimeProperties met meerdere invoerbestanden en aangepaste gegevens doorgeeft aan de processor van de media Media Encoder Premium Workflow."
    services="media-services"
    documentationCenter=""
    authors="xpouyat"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"  
    ms.author="xpouyat;anilmur;juliako"/>

# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Met behulp van meerdere invoerbestanden en eigenschappen met Premium-Encoder

## <a name="overview"></a>Overzicht

Er zijn scenario's waarin u mogelijk nodig voor het aanpassen van eigenschappen van het onderdeel Clip lijst XML-inhoud opgeven, of meerdere invoerbestanden verzenden wanneer u een taak met de processor van de media **Media Encoder Premium Workflow** indienen. Enkele voorbeelden zijn:

- Tekst op de video en het instellen van de waarde van de (bijvoorbeeld de huidige datum) tijdens runtime voor elke video input bedekken.
- Aanpassen van de XML-lijst Clip (u geeft een of meer bronbestanden, met of zonder bijsnijden, enz.).
- Een logoafbeelding bedekken op de video input, terwijl de video is gecodeerd.

Als u wilt dat de **Media Encoder Premium Workflow** weten dat bepaalde eigenschappen in de werkstroom wilt wijzigen wanneer u de taak maakt of meerdere invoerbestanden verzenden, hebt u een configuratietekenreeks bevat die **setRuntimeProperties** en/of **transcodeSource**. In dit onderwerp wordt uitgelegd hoe u deze kunt gebruiken.


## <a name="configuration-string-syntax"></a>De syntaxis van configuratie

De configuratietekenreeks instellen in de codering taak gebruikt een XML-document ziet:

    <?xml version="1.0" encoding="utf-8"?>
    <transcodeRequest>
      <transcodeSource>
      </transcodeSource>
      <setRuntimeProperties>
        <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      </setRuntimeProperties>
    </transcodeRequest>


Hieronder volgt de C#-code die de XML-configuratie van een bestand wordt gelezen en doorgegeven aan de taak in een project:

    XDocument configurationXml = XDocument.Load(xmlFileName);
    IJob job = _context.Jobs.CreateWithSingleTask(
                                                  "Media Encoder Premium Workflow",
                                                  configurationXml.ToString(),
                                                  myAsset,
                                                  "Output asset",
                                                  AssetCreationOptions.None);


## <a name="customizing-component-properties"></a>Componenteigenschappen aanpassen  

### <a name="property-with-a-simple-value"></a>Eigenschap met een eenvoudige waarde
In sommige gevallen is het handig om een eigenschap van onderdeel samen met het bestand met de workflow die wordt uitgevoerd door de Media Encoder Premium Workflow aanpassen.

Stel dat u een werkstroom die tekst overlays op opgezet uw video's en de tekst (bijvoorbeeld de huidige datum) moet worden ingesteld tijdens runtime. U kunt dit doen door het verzenden van tekst moet worden ingesteld als de nieuwe waarde voor de eigenschap text van het overlay-onderdeel van de taak van de codering. Deze methode kunt u andere eigenschappen wijzigen van een onderdeel in de werkstroom (bijvoorbeeld de positie of kleur van de bedekking, de bitsnelheid van de AVC-encoder, enz.).

**setRuntimeProperties** wordt gebruikt om een eigenschap in de onderdelen van de werkstroom.

Voorbeeld:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Optional Overlay/Overlay/filename" value="MyLogo.png"/>
          <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
      </setRuntimeProperties>
    </transcodeRequest>


### <a name="property-with-an-xml-value"></a>Eigenschap met de waarde van een XML-

Een eigenschap die u verwacht van een XML-waarde wilt instellen, inkapselen met behulp van `<![CDATA[ and ]]>`.

Voorbeeld:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

>[AZURE.NOTE]Zorg ervoor dat niet om een carriage return vlak na `<![CDATA[`.


### <a name="propertypath-value"></a>propertyPath waarde

De propertyPath werd in de vorige voorbeelden "/ Media File Input/bestandsnaam" of "/ inactiveTimeout" of "clipListXml".
Dit kan in het algemeen is de naam van het onderdeel en vervolgens op de naam van de eigenschap. Het pad kan hebben meer of minder niveaus, zoals "/ primarySourceFile" (omdat de eigenschap in de hoofdmap van de werkstroom) of ' / Video verwerking/afbeelding/dekking voor de bedekking "(omdat de Overlay in een groep).    

U kunt de naam en de eigenschap controleren door de actieknop die direct naast elke eigenschap te gebruiken. U kunt deze actie te klikken en selecteer **bewerken**. Hier ziet u de werkelijke naam van de eigenschap en direct boven, de naamruimte.

![Actie/bewerken](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Eigenschap](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Meerdere invoerbestanden

Elke taak die u bij de **Media Encoder Premium Workflow indient** vereist twee activa:

- De eerste is een bestand met een workflow met *Workflow actief* . Met de [Werkstroomontwerper](media-services-workflow-designer.md)kunt u Werkstroombestanden ontwerpen.
- De tweede is een *Actief Media* met de media-bestanden die u wilt coderen.

Wanneer u meerdere media-bestanden met het coderingsprogramma **Media Encoder Premium Workflow** verzendt, gelden de volgende beperkingen:

- Alle mediabestanden moeten zich in dezelfde *Media activa*. Met behulp van meerdere Media-elementen wordt niet ondersteund.
- U moet het primaire bestand in deze Media activa instellen (in het ideale geval is dit het belangrijkste videobestand dat het coderingsprogramma wordt gevraagd om te verwerken).
- Is het doorgeven van configuratiegegevens die bevat van het element **setRuntimeProperties** en/of **transcodeSource** aan de processor nodig.
  - **setRuntimeProperties** wordt gebruikt om te negeren de eigenschap filename of een andere eigenschap in de onderdelen van de werkstroom.
  - **transcodeSource** wordt gebruikt om de inhoud van de XML-lijst van Clip.

Verbindingen in de werkstroom:

 - Als u een of verschillende onderdelen van Media bestandsinvoer en plan de bestandsnaam opgeven via **setRuntimeProperties** , vervolgens sluit niet het primaire bestand onderdeel pin toe. Zorg ervoor dat er geen verbinding tussen het primaire bestand-object en de Media-bestand Input(s).
 - Als u liever Clip lijst XML en een mediabron onderdeel en vervolgens kunt u beide samen.

![Geen verbinding tussen de primaire bronbestand en bestandsinvoer Media](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Er is geen verbinding van het primaire bestand voor bestandsinvoer Media-onderdelen als u setRuntimeProperties gebruikt voor het instellen van de eigenschap filename.*

![Verbinding in Clip lijst van XML naar Clip lijst bron](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*U kunt verbinding maken met XML-lijst van Clip mediabron en transcodeSource.*


### <a name="clip-list-xml-customization"></a>Clip van de XML-lijst aanpassen
Kunt u de Clip-lijst-XML in de workflow tijdens runtime met behulp van **transcodeSource** in de configuratiereeks XML. Hiervoor moet de XML-lijst van Clip pincode moet worden aangesloten op het onderdeel mediabron in de workflow.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <transcodeSource>
          <clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
          </clipList>
        </transcodeSource>
        <setRuntimeProperties>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Als u /primarySourceFile voor het gebruik van deze eigenschap om de output bestanden met behulp van 'Expressies', vervolgens wordt aangeraden de Clip lijst XML als een eigenschap *als* de eigenschap /primarySourceFile om te voorkomen dat de lijst van de Clip worden overschreven door de instelling van de /primarySourceFile doorgeven.

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Met meer nauwkeurige frame opmaak:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
               <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


## <a name="example"></a>Voorbeeld

Bekijk een voorbeeld waarin u bedekken van een logoafbeelding op de video input wilt terwijl de video is gecodeerd. In dit voorbeeld wordt de video input heet "MyInputVideo.mp4" en het logo met de naam 'MyLogo.png'. U moet de volgende stappen uitvoeren:

- Een werkstroom actief maken met het bestand workflow (Zie het volgende voorbeeld).
- Maken van een actief Media, bevat twee bestanden: MyInputVideo.mp4 als het primaire bestand en de MyLogo.png.
- Een taak verzenden naar de Media Encoder Premium Workflow media-processor met de bovenstaande invoer activa en de volgende configuratietekenreeks opgeven.

Configuratie:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="MyLogo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


In het bovenstaande voorbeeld wordt de naam van het FLV-bestand verzonden op het onderdeel Media bestandsinvoer en de eigenschap primarySourceFile. De naam van het logobestand wordt verzonden naar een andere Media bestand Input die is aangesloten op de grafische overlay-component.

>[AZURE.NOTE]De naam van het FLV-bestand wordt verzonden naar de eigenschap primarySourceFile. De reden hiervoor is het gebruik van deze eigenschap in de werkstroom voor het maken van expressies, bijvoorbeeld met naam van het juiste uitvoer.


### <a name="step-by-step-workflow-creation-that-overlays-a-logo-on-top-of-the-video"></a>Stapsgewijze werkstroom maken die als overlay voor een logo op de video     

Hier volgen de stappen voor het maken van een workflow die twee bestanden als invoer nodig: een video en een afbeelding. Het zal de afbeelding op de video overlay.

**Workflow Designer** te openen en selecteer **bestand** > **Nieuwe werkruimte** > **Transcoderen blauwdruk**.

De nieuwe werkstroom bevat drie elementen:

- Primaire bronbestand
- Clip XML-lijst
- Uitvoer bestand-bedrijfsmiddelen  

![Nieuwe codering Workflow](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Nieuwe codering Workflow*


Om het bestand invoermedium accepteert, beginnen met het Media bestand Input onderdelen toevoegen. Als een onderdeel toevoegen aan de workflow, zoekt in het zoekvak van de bibliotheek en sleep de gewenste vermelding naar het deelvenster ontwerp.

Voeg vervolgens het FLV-bestand moet worden gebruikt voor het ontwerpen van uw workflow. Klik in het deelvenster achtergrond in de werkstroomontwerper hiervoor, en zoekt u de eigenschap van het primaire bestand in het deelvenster rechts van de eigenschap. Klik op het mappictogram en selecteer het juiste video-bestand.

![Primaire bronnen](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Primaire bronnen*


Geef vervolgens het videobestand in de component Media bestand Input.   

![Invoerbron voor mediabestanden](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Invoerbron voor mediabestanden*


Zodra dit is gebeurd, wordt het onderdeel Media bestandsinvoer controleren van het bestand en vullen de uitvoerpinnen aan het bestand dat deze gecontroleerd.

De volgende stap is het toevoegen van een "Video Data Type Updater' als u wilt opgeven van de kleurruimte naar Rec.709. Een "Video Format Converter' die is ingesteld op het type gegevensindeling lay-out toevoegen = vlakke worden geconfigureerd. Dit wordt de video-stream converteren naar een indeling die kan worden uitgevoerd als een bron van het onderdeel overlay.

![video Data Type Updater en Format Converter](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Video-Data Type Updater en Format Converter*

![Indelingstype = configureerbare vlakke](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Indelingstype is configureerbaar vlakke*

Vervolgens een component Video-Overlay toevoegen en de video (niet-gecomprimeerd) pin verbinding met de (niet-gecomprimeerd) video pin van de invoer van media-bestand.

Toevoegen van een andere Media bestand Input (voor het laden van het logobestand), klikt u op dit onderdeel en wijzig de naam in 'Media bestand Input-Logo' en selecteer een afbeelding (bijvoorbeeld een PNG-bestand) in de bestandseigenschap. Verbinding maken met de pincode niet-gecomprimeerde afbeelding van de bedekking van de pincode niet-gecomprimeerde afbeelding.

![Bron van onderdeel- en overlay](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Bron van onderdeel- en overlay*


Als u wilt wijzigen van de positie van het logo op de video (bijvoorbeeld u mogelijk wilt plaatsen op 10 procent korting op de linkerbovenhoek van de video), schakel het selectievakje 'Handmatige invoer'. U kunt dit doen omdat u een Media-invoer bestand om het logobestand naar het onderdeel overlay.

![Overlay-positie](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Overlay-positie*


Toevoegen als u wilt coderen van de video-stream met H.264, AVC Video Encoder en AAC encoder onderdelen op het oppervlak van de ontwerpfunctie. Verbinding maken met de pennen.
De AAC encoder instellen en selecteer Audio Format conversie/definitie: 2.0 (L, R).

![Audio- en Video-Encoders](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Audio- en Video-Encoders*


Nu de onderdelen van de **ISO Mpeg-4 Multiplexer** en **Uitvoer in een bestand** toe te voegen en de pinnen zoals verbinding.

![Multiplexer MP4- en uitvoer in een bestand](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*Multiplexer MP4- en uitvoer in een bestand*


U moet de naam voor het uitvoerbestand. Klik op het onderdeel van de **Uitvoer in een bestand** en bewerkt u de expressie voor het bestand:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Output bestandsnaam](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Output bestandsnaam*

De werkstroom lokaal om te controleren dat deze correct wordt uitgevoerd, kunt u uitvoeren.

Nadat dit is voltooid, kunt u deze in Azure Media Services uitvoeren.

Eerst een activum in Azure Media Services met twee bestanden: het FLV-bestand en het logo. U kunt dit doen met behulp van .NET of REST API. U kunt dit ook doen via de portal Azure of [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

In deze zelfstudie wordt beschreven hoe u voor het beheer van activa met AMSE. Er zijn twee manieren bestanden toevoegen aan een actief:

- Maak een lokale map, Kopieer de twee bestanden, en sleept de map naar het tabblad **actief** .
- Upload het videobestand als een actief, geven de informatie over de activa en Ga naar het tabblad bestanden uploaden van een extra bestand (logo).

>[AZURE.NOTE]Zorg ervoor dat een primair bestand instellen in het actief (de belangrijkste video-bestand).

![Actief bestanden in AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Actief bestanden in AMSE*


Selecteer het activum en u deze coderen met Premium-Encoder. Uploaden van de werkstroom en selecteer deze.

Klik op de knop gegevens doorgeven aan de processor en toevoegen van de volgende XML-code om de runtime eigenschappen instellen:

![Premium-Encoder in AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Premium-Encoder in AMSE*


Plak de volgende XML-gegevens. U moet de naam opgeven van het videobestand voor de invoer van Media-bestand en de primarySourceFile. De naam van het bestand voor het logo te geven.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*


Als u de SDK voor .NET maken en uitvoeren van de taak, heeft deze XML-gegevens moeten worden doorgegeven als configuratiereeks.

    public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);

Nadat de taak voltooid is, wordt het MP4-bestand in de activa van de uitvoer de overlay!

![Overlay op de video](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Overlay op de video*


U kunt de voorbeeldwerkstroom downloaden van [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).


## <a name="see-also"></a>Zie ook

- [Introductie van Premium Services in Azure Media-codering](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

- [Het gebruik van Premium codering in Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

- [Codering van inhoud op aanvraag met Azure Media Services](media-services-encode-asset.md#media_encoder_premium_workflow)

- [Media Encoder Premium Workflow-indelingen en -codecs](media-services-premium-workflow-encoder-formats.md)

- [Workflow-voorbeeldbestanden](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)

- [Azure Media Services Explorer tool](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
