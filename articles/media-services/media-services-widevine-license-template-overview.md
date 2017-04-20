<properties 
    pageTitle="Widevine licentie sjabloon overzicht | Microsoft Azure" 
    description="Dit onderwerp geeft een overzicht van een sjabloon voor Widevine waarmee Widevine licenties configureren." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="widevine-license-template-overview"></a>Widevine licentie sjabloon overzicht

##<a name="overview"></a>Overzicht

Azure Media Services kunt u nu configureren en Widevine licenties aanvragen. Wanneer de eindgebruiker wordt geprobeerd uw Widevine beveiligde inhoud af te spelen, wordt een aanvraag verzonden naar de bezorgservice licentie om een licentie te verkrijgen. Als de licentie-service de aanvraag heeft goedgekeurd, problemen het met de licentie die wordt verzonden naar de client en kan worden gebruikt voor het decoderen en afspelen van de opgegeven inhoud.

Licentieaanvraag Widevine is opgemaakt als een JSON-bericht.  

Houd er rekening mee dat u kunt kiezen voor het maken van een leeg bericht zonder waarden alleen "{}" en een sjabloon met alle standaardinstellingen gemaakt wordt.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

##<a name="json-message"></a>JSON-bericht

Naam | Waarde | Beschrijving
---|---|---
Payload |Base64-gecodeerde string |De licentieaanvraag van een client. 
content_id | Base64-gecodeerde string|ID gebruikt voor het afleiden van KeyId(s) en inhoud (s) voor elke content_key_specs.track_type.
provider |tekenreeks |Gebruikt om te zoeken naar inhoud toetsen en beleid. Vereist.
Naam_van_beleid | tekenreeks |De naam van een eerder geregistreerde beleid. Optioneel
allowed_track_types | Enum  | SD_ONLY of SD_HD. Besturingselementen voor inhoud sleutels moeten worden opgenomen in een licentie
content_key_specs | matrix van JSON structuren, Zie onderstaande **Inhoud sleutel specificaties** | Een fijnere gegreineerd besturingselement op welke inhoud toetsen om terug te keren. Zie inhoud sleutel specificaties hieronder voor meer informatie.  Er kan slechts één van de allowed_track_types en content_key_specs worden opgegeven. 
use_policy_overrides_exclusively | Boole-waarde. waar of ONWAAR | Gebruik Groepsbeleid kenmerken opgegeven door policy_overrides en alle eerder opgeslagen beleid weglaten.
policy_overrides | JSON structuur, Zie hieronder worden **Overschreven** | De instellingen voor deze licentie.  In het geval van deze activa heeft een vooraf gedefinieerd beleid, worden deze waarden opgegeven wordt gebruikt. 
session_init | JSON structuur, Zie onderstaande **Sessie-initialisatie** | Optionele gegevens doorgegeven aan de licentie.
parse_only | Boole-waarde. waar of ONWAAR | De licentieaanvraag wordt geparseerd, maar geen licentie is afgegeven. Echter de waarden voor de licentieaanvraag worden geretourneerd in het formulier.  

##<a name="content-key-specs"></a>Belangrijkste specificaties van de inhoud 

Als u een bestaand beleid bestaat, behoeft niet opnieuw op te geven van de waarden in de sleutel-specificatie van inhoud.  Het bestaande beleid dat is gekoppeld aan deze inhoud wordt gebruikt voor de bescherming van de uitvoer zoals HDCP en CGMS bepalen.  Als u een bestaand beleid is niet geregistreerd bij de licentieserver Widevine, kan de inhoudsprovider injecteren de waarden de licentieaanvraag.   


Elke content_key_specs moet worden opgegeven voor alle tracks, ongeacht de optie use_policy_overrides_exclusively. 


Naam | Waarde | Beschrijving
---|---|---
content_key_specs. track_type | tekenreeks | Naam van een track. Als content_key_specs is opgegeven in de licentieaanvraag, zorg ervoor dat alle typen bijhouden expliciet opgeven. Niet doet, resulteert in fout om het afspelen te afgelopen 10 seconden. 
content_key_specs  <br/> security_level | UInt32 | Definieert de robuustheid clientvereisten voor afspelen. <br/> 1 - whitebox softwarematige codering is vereist. <br/> 2 - crypto-software en een betekenisloze decoder is vereist. <br/> 3 - het sleutelmateriaal en crypto bewerkingen moeten worden uitgevoerd binnen een hardware back vertrouwde omgeving. <br/> 4 - de codering en decodering van inhoud moeten worden uitgevoerd binnen een hardware back vertrouwde omgeving.  <br/> 5 - de crypto, decoderen en alle verwerkingstijd van de media (gecomprimeerd en gedecomprimeerd) moeten worden verwerkt in een hardware back vertrouwde omgeving.  
content_key_specs <br/> required_output_protection.hDC | de tekenreeks - een van: HDCP_NONE, HDCP_V1, HDCP_V2 | Geeft aan of de HDCP is vereist
content_key_specs <br/>sleutel | Base64 <br/>gecodeerde tekenreeks|Inhoud sleutel voor dit nummer. Als u opgeeft, is de track_type of key_id vereist.  Met deze optie kunt de aanbieder aan de sleutel voor dit nummer in plaats van een licentieserver Widevine genereren of het opzoeken van een sleutel te laten invoeren.
content_key_specs.key_id| Met base64 gecodeerde tekenreeks binair, 16 bytes | De unieke id voor de sleutel. 


##<a name="policy-overrides"></a>Beleid negeren 

Naam | Waarde | Beschrijving
---|---|---
policy_overrides. can_play | Boole-waarde. waar of ONWAAR | Hiermee wordt aangegeven dat het afspelen van de inhoud is toegestaan. De standaardwaarde is false.
policy_overrides. can_persist | Boole-waarde. waar of ONWAAR |Hiermee wordt aangegeven dat de licentie kan worden doorgegeven aan de niet-vluchtige opslagruimte voor off line gebruik. De standaardwaarde is false.
policy_overrides. can_renew | Boole-waarde true of false |Hiermee wordt aangegeven dat de verlenging van deze licentie is toegestaan. Als de waarde true is, kan de duur van de licentie kan worden uitgebreid door heartbeat. De standaardwaarde is false. 
policy_overrides. license_duration_seconds | Int64 | Geeft het tijdvenster voor deze specifieke licentie. De waarde 0 geeft aan dat er geen limiet aan de duur. De standaardwaarde is 0. 
policy_overrides. rental_duration_seconds | Int64 | Geeft het tijdvenster tijdens het afspelen is toegestaan. De waarde 0 geeft aan dat er geen limiet aan de duur. De standaardwaarde is 0. 
policy_overrides. playback_duration_seconds | Int64 | Het weergavevenster van tijd wanneer het afspelen begint binnen de geldigheidsduur van de licentie. De waarde 0 geeft aan dat er geen limiet aan de duur. De standaardwaarde is 0. 
policy_overrides. renewal_server_url |tekenreeks | Alle heartbeat (verversings) aanvragen voor deze licentie worden doorgezonden naar de opgegeven URL. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is.
policy_overrides. renewal_delay_seconds |Int64 |Hoeveel seconden na license_start_time, voordat de verlenging wordt eerst geprobeerd. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. De standaardwaarde is 0 
policy_overrides. renewal_retry_interval_seconds | Int64 | Hiermee geeft u de vertraging in seconden tussen opeenvolgende licentie verlenging aanvragen bij een storing. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. 
policy_overrides. renewal_recovery_duration_seconds | Int64 | Het venster van de tijd waarin afspelen is toegestaan om door te gaan bij het vernieuwen is geprobeerd, nog mislukt als gevolg van problemen met de licentieserver backend. De waarde 0 geeft aan dat er geen limiet aan de duur. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is.
policy_overrides. renew_with_usage | Boole-waarde true of false |Geeft aan dat de licentie voor vernieuwing moet worden verzonden wanneer gebruik wordt gestart. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. 

##<a name="session-initialization"></a>Sessie-initialisatie

Naam | Waarde | Beschrijving
---|---|---
provider_session_token | Base64-gecodeerde string |Deze sessietoken in de licentie wordt doorgegeven en in latere vernieuwingen aanwezig.  Het sessietoken wordt niet permanent buiten de sessies. 
provider_client_token | Base64-gecodeerde string | Clienttoken terug in de licentie-antwoord verzenden.  Als de licentieaanvraag een clienttoken bevat, wordt deze waarde wordt genegeerd. Het clienttoken ervan behouden blijven buiten de licentie-sessies.
override_provider_client_token | Boole-waarde. waar of ONWAAR |Als onwaar en de licentieaanvraag bevat een clienttoken, gebruikt u het token van de aanvraag, zelfs als een clienttoken is opgegeven in deze structuur.  Als de waarde true is, altijd het token dat is opgegeven in deze structuur te gebruiken.

##<a name="configure-your-widevine-licenses-using-net-types"></a>Het gebruik van .NET Widevine-licenties configureren

Media Services biedt .NET API's waarmee u uw licenties Widevine configureren. 

###<a name="classes-as-defined-in-the-media-services-net-sdk"></a>Klassen, zoals gedefinieerd in de .NET SDK Media Services

De volgende zijn de definities van deze typen.

    public class WidevineMessage
    {
        public WidevineMessage();
    
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

###<a name="example"></a>Voorbeeld

In het volgende voorbeeld ziet u hoe .NET API's kunt gebruiken voor het configureren van een eenvoudige Widevine licentie.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Zie ook

[PlayReady en/of dynamische Widevine gemeenschappelijke codering gebruiken](media-services-protect-with-drm.md)
