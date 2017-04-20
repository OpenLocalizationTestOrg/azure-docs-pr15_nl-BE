<properties 
    pageTitle="Het gebruik van de betrokkenheid bij de API op Windows Phone Silverlight" 
    description="Het gebruik van de betrokkenheid bij de API op Windows Phone Silverlight"    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" /> 

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="how-to-use-the-engagement-api-on-windows-phone-silverlight"></a>Het gebruik van de betrokkenheid bij de API op Windows Phone Silverlight

Dit document is een invoegtoepassing voor [het integreren van Mobile Engagement in uw Silverlight voor Windows Phone-app](mobile-engagement-windows-phone-integrate-engagement.md)van het document. Het biedt in diepte details over het gebruik van de API Engagement voor aangifte van de statistieken van uw toepassing.

Als u wilt dat alleen de betrokkenheid van uw toepassing sessies, activiteiten, crashes en technische informatie, is de eenvoudigste manier om alle uw `PhoneApplicationPage` onderliggende klassen overnemen van de `EngagementPage` klasse.

Als u wilt doen, bijvoorbeeld als u wilt de toepassing specifieke gebeurtenissen, fouten en taken, of als u moet de activiteiten van uw toepassing op een andere manier dan de geïmplementeerd in het `EngagementPage` klassen, moet u gebruikmaken van de API Engagement.

De betrokkenheid bij de API wordt geleverd door de `EngagementAgent` klasse. U kunt toegang tot deze methoden via `EngagementAgent.Instance`.

Zelfs als de agent module is niet geïnitialiseerd, wordt elke aanroep van de API is uitgesteld en wordt opnieuw uitgevoerd wanneer de agent beschikbaar is.

##<a name="engagement-concepts"></a>Betrokkenheid bij de concepten

De volgende onderdelen verfijnen de concepten van Mobile Engagement voor het Windows Phone platform.

### <a name="session-and-activity"></a>`Session`en`Activity`

Een *activiteit* wordt meestal veroorzaakt door een pagina van de toepassing, dat wil zeggen de *activiteit* wordt gestart wanneer de pagina wordt weergegeven en stopt wanneer de pagina wordt gesloten: dit is het geval wanneer de betrokkenheid bij de SDK is geïntegreerd met behulp van de `EngagementPage` klasse.

Maar *activiteiten* kunnen ook handmatig met behulp van de API Engagement worden beheerd. Hiermee splitst een bepaalde pagina in verschillende sub-delen voor meer informatie over het gebruik van deze pagina (bijvoorbeeld bekend hoe vaak en hoe lang de dialoogvensters worden gebruikt in deze pagina).

##<a name="reporting-activities"></a>Activiteiten melden

### <a name="user-starts-a-new-activity"></a>Gebruiker start een nieuwe activiteit

#### <a name="reference"></a>Referentie

            void StartActivity(string name, Dictionary<object, object> extras = null)

U moet bellen `StartActivity()` elke keer dat de Gebruikerswijzigingen in activiteiten. De eerste aanroep van deze functie wordt een nieuwe gebruikerssessie gestart.

> [AZURE.IMPORTANT] De SDK automatisch de methode EndActivity aanroepen wanneer de toepassing wordt gesloten. Dus is het raadzaam de methode StartActivity aanroepen wanneer de activiteit van de gebruiker wijzigen en klik op nooit de EndActivity methode aanroepen, aangezien deze methode zorgt ervoor dat de huidige sessie moet worden beëindigd.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Gebruiker eindigt zijn huidige activiteit

#### <a name="reference"></a>Referentie

            void EndActivity()

U moet bellen `EndActivity()` ten minste één keer wanneer de gebruiker zijn laatste activiteit is. Dit meldt de SDK Engagement dat de gebruiker die momenteel niet actief is en dat de sessie van de gebruiker moet worden gesloten zodra de time-out voor sessies verlopen (als u `StartActivity()` voordat de time-out voor sessies is verlopen, de sessie gewoon wordt voortgezet).

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.EndActivity();

##<a name="reporting-jobs"></a>Taken melden

### <a name="start-a-job"></a>Een taak starten

#### <a name="reference"></a>Referentie

            void StartJob(string name, Dictionary<object, object> extras = null)

U kunt de taak opgenomen taken bijhouden in een bepaalde tijdsperiode.

#### <a name="example"></a>Voorbeeld

            // An upload begins...
            
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
            
            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Een taak beëindigen

#### <a name="reference"></a>Referentie

            void EndJob(string name)

Als een taak wordt bijgehouden door een taak is beëindigd, moet u de methode EndJob aanroepen voor deze taak, door het opgeven van de taaknaam van de.

#### <a name="example"></a>Voorbeeld

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
            
            EngagementAgent.Instance.EndJob("uploadData");

##<a name="reporting-events"></a>Voor het melden van gebeurtenissen

Er is drie typen gebeurtenissen:

-   Zelfstandige gebeurtenissen
-   Sessiegebeurtenissen
-   Project-gebeurtenissen

### <a name="standalone-events"></a>Zelfstandige gebeurtenissen

#### <a name="reference"></a>Referentie

            void SendEvent(string name, Dictionary<object, object> extras = null)

Zelfstandig kunnen gebeurtenissen buiten de context van een sessie.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Sessiegebeurtenissen

#### <a name="reference"></a>Referentie

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Sessiegebeurtenissen worden meestal gebruikt om de acties die worden uitgevoerd door een gebruiker tijdens de sessie zijn.

#### <a name="example"></a>Voorbeeld

**Zonder gegevens:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
            
            // or
            
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Met gegevens:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Project-gebeurtenissen

#### <a name="reference"></a>Referentie

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Project-gebeurtenissen worden meestal gebruikt om de acties die worden uitgevoerd door een gebruiker gedurende een project.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

##<a name="reporting-errors"></a>Fouten melden

Er is drie typen fouten:

-   Zelfstandige fouten
-   Sessie-fouten
-   Taakfouten

### <a name="standalone-errors"></a>Zelfstandige fouten

#### <a name="reference"></a>Referentie

            void SendError(string name, Dictionary<object, object> extras = null)

Zelfstandige fouten kunnen met sessie-fouten optreden buiten de context van een sessie.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Sessie-fouten

#### <a name="reference"></a>Referentie

            void SendSessionError(string name, Dictionary<object, object> extras = null)

Sessie-fouten worden meestal gebruikt om de invloed van de gebruiker tijdens de sessie zijn fouten.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Taakfouten

#### <a name="reference"></a>Referentie

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Fouten kunnen worden gerelateerd aan een actieve taak in plaats van te wijten aan de huidige gebruikerssessie.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

##<a name="reporting-crashes"></a>Fouten melden

De agent biedt twee methoden voor de behandeling van crashes.

### <a name="send-an-exception"></a>Een uitzondering verzenden

#### <a name="reference"></a>Referentie

            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Voorbeeld

U kunt een uitzondering op elk gewenst moment verzenden door op te roepen:

            EngagementAgent.Instance.SendCrash(aCatchedException);

U kunt ook een optionele parameter gebruiken om de betrokkenheid bij de sessie op hetzelfde moment dan het verzenden van de crash te beëindigen. Bel hiervoor:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Als u dat doet, wordt de sessie en taken gesloten na het verzenden van de crash.

### <a name="send-an-unhandled-exception"></a>Een niet-verwerkte uitzondering verzenden

#### <a name="reference"></a>Referentie

            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

Engagement biedt ook een methode voor het verzenden van niet-verwerkte uitzonderingen. Dit is vooral handig bij gebruik in de gebeurtenis-handler van silverlight UnhandledException.

Deze methode wordt **altijd** beëindigen engagement sessie en taken nadat deze is aangeroepen.

#### <a name="example"></a>Voorbeeld

U kunt het implementeren van uw eigen handler UnhandledException (vooral als u de automatische crash rapportage functie van betrokkenheid hebt uitgeschakeld). Bijvoorbeeld, in de `Application_UnhandledException` methode van de `App.xaml.cs` bestand:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code
            
              EngagementAgent.Instance.SendCrash(e);
            }

##<a name="onactivated"></a>OnActivated

### <a name="reference"></a>Referentie

            void OnActivated(ActivatedEventArgs e)

Wanneer de gebruiker naar voren, niet op een toepassing, navigeert nadat de gebeurtenis gedeactiveerd wordt geactiveerd, probeert het besturingssysteem de toepassing in een inactieve toestand zetten. De toepassing is vervolgens tombstones. In dit proces een aanvraag is afgesloten, maar sommige gegevens over de status van de toepassing en de afzonderlijke pagina's in de toepassing blijft behouden.

U hebt te plaatsen `EngagementAgent.Instance.OnActivated(e)` in de `Application_Activated` methode uit het bestand App.xaml.cs de betrokkenheid bij de Agent opnieuw instellen wanneer de toepassing Tombstoned is.

### <a name="example"></a>Voorbeeld

            // Inside your App.xaml.cs file
            
            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

##<a name="device-id"></a>Apparaat-Id

            String GetDeviceId()

Door het aanroepen van deze methode kunt u de betrokkenheid bij de apparaat-id krijgen.

##<a name="extras-parameters"></a>Extra parameters

Willekeurige gegevens kunnen worden gekoppeld aan een gebeurtenis, een fout, een activiteit of taak. Deze gegevens kunnen gestructureerd worden met behulp van een woordenboek. Sleutels en waarden kunnen van een willekeurig type worden.

Extra gegevens worden geserialiseerd dus als u wilt uw eigen type invoegen in extra's hebt u een gegevenscontract voor dit type toevoegen.

### <a name="example"></a>Voorbeeld

We maken een nieuwe klasse "Persoon".

            using System.Runtime.Serialization;
            
            namespace Engagement.Agent
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
            
                // Properties
            
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
            
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Wij zullen vervolgens toevoegen een `Person` een extra exemplaar.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
            
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] Als u andere typen objecten plaatst, moet dat de methode ToString() wordt geïmplementeerd om een menselijk leesbare tekenreeks als resultaat geven.

### <a name="limits"></a>Limieten

#### <a name="keys"></a>Toetsen

Elke sleutel in het object moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Betekent dit dat de sleutels met ten minste één letter beginnen moeten, gevolgd door de letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Extra's zijn beperkt tot **1024** tekens per oproep.

##<a name="reporting-application-information"></a>Toepassingsgegevens

### <a name="reference"></a>Referentie

            void SendAppInfo(Dictionary<object, object> appInfos)

Handmatig kunt u de functie voor het bijhouden van gegevens (of andere toepassing specifieke gegevens) met behulp van de SendAppInfo() rapporteren.

Merk op dat deze informatie kan stapsgewijs worden verzonden: alleen de meest recente waarde voor een bepaalde sleutel worden opgeslagen voor een bepaald apparaat. Een woordenlijst gebruiken als gebeurtenis extra's,\<-object, object\> voor het koppelen van gegevens.

### <a name="example"></a>Voorbeeld

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Limieten

#### <a name="keys"></a>Toetsen

Elke sleutel in het object moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Betekent dit dat de sleutels met ten minste één letter beginnen moeten, gevolgd door de letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Informatie over de toepassingen zijn beperkt tot **1024** tekens per oproep.

In het vorige voorbeeld is de JSON naar de server verzonden 44 tekens bevatten:

            {"subscription":"2013-12-07","premium":"true"}
 
##<a name="logging"></a>Logboekregistratie
###<a name="enable-logging"></a>Logboekregistratie inschakelen

De SDK kan produceren test Logboeken in de IDE-console worden geconfigureerd.
Deze logboekbestanden zijn standaard niet geactiveerd. De eigenschap voor bijwerken als u wilt aanpassen, `EngagementAgent.Instance.TestLogEnabled` op een van de waarde van de `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
