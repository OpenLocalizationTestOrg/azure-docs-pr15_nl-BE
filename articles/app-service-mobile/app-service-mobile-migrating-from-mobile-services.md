<properties
    pageTitle="Mobiele Services migreren naar een mobiele App App Service"
    description="Meer informatie over het eenvoudig uw toepassing een App Service Mobile App Mobile Services migreren"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="adrianha"/>

# <a name="article-top"></a>Uw bestaande Mobile-Service Azure migreren naar Azure App Service

Met de [algemene beschikbaarheid van Azure App Service]kunnen Azure Mobile Services-sites worden gemakkelijk gemigreerd ter plaatse om te profiteren van de functies van de Service Azure App.  Dit document wordt uitgelegd wat u kunt verwachten bij het migreren van uw site van Azure Mobile Services naar Azure App-Service.

## <a name="what-does-migration-do"></a>Wat doet migratie naar uw site

Migratie van uw Azure Mobile-Service maakt uw mobiele Service zonder gevolgen voor de code van een app [Azure App-Service] .  Uw melding Hubs, SQL gegevensverbinding verificatie-instellingen, geplande taken en domeinnaam blijven ongewijzigd.  Mobiele clients met behulp van uw mobiele Azure Service blijven normaal functioneren.  Migratie wordt uw service opnieuw gestart zodra ze worden overgebracht naar Azure App-Service.

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Waarom moet u uw site migreren

Microsoft wordt geadviseerd uw Azure Mobile-Service om te profiteren van de functies van Azure App-Service, met inbegrip van de te migreren:

  *  Nieuwe host-functies, waaronder [WebJobs] en [aangepaste domeinnamen].
  *  Verbinding met uw lokale resources met behulp van [VNet] naast het [Hybride verbindingen].
  *  Controle en het oplossen van problemen met nieuwe Relic of [Inzichten van toepassing].
  *  Ingebouwde DevOps tooling, met inbegrip van ["slots" staging], terugdraaien, en testen in productie.
  *  [Automatisch schalen], load balancing en [controleren van de prestaties].

Zie het onderwerp [Mobile Services versus App-Service] voor meer informatie over de voordelen van Azure App-Service.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint een belangrijke werk op uw site, moet u scripts [Back-up van uw Mobile-Service] en SQL-database.

## <a name="migrating-site"></a>Uw sites migreren

Het migratieproces worden gemigreerd van alle sites binnen één enkel Azure.

Voor het migreren van uw site:

  1.  Log in op de [Azure klassieke Portal].
  2.  Selecteer een mobiele Service in de regio die u wilt migreren.
  3.  Klik op **migreren naar App-Service** .

    ![De knop migreren][0]

  4.  Lees de migreren naar het dialoogvenster App-Service.
  5.  Voer de naam van uw mobiele Service in het desbetreffende vak.  Bijvoorbeeld, als uw domeinnaam contoso.azure mobile.net is, voert u _contoso_ in het desbetreffende vak.
  6.  Klik op de knop maatstreepjes.

De status van de migratie in de monitor van de activiteit te controleren. Uw site wordt weergegeven als de *migratie* in de klassieke Azure-Portal.

  ![Migratie-activiteiten controleren][1]

Elke migratie kan duren 3 tot 15 minuten per mobiele service wordt gemigreerd.  De site blijft beschikbaar tijdens de migratie.
Uw site opnieuw aan het einde van het migratieproces wordt gestart.  De site is niet beschikbaar tijdens het opstarten, dat enkele seconden duurt.

## <a name="finalizing-migration"></a>De migratie voltooien

Plan uw site vanaf een mobiele client aan het einde van het migratieproces te testen.  Zorg ervoor dat u alle gemeenschappelijke acties voor client zonder wijzigingen in de mobiele client kunt uitvoeren.  

### <a name="update-app-service-tier"></a>Selecteer een geschikte App serviceprijzen laag

Hebt u meer flexibiliteit in de prijzen nadat u deze gemigreerd naar Azure App-Service.

  1.  Log in op de [Azure portal].
  2.  Selecteer **alle resources** of **App Services** Klik op de naam van de gemigreerde Mobile-Service.
  3.  De blade instellingen wordt standaard geopend.
  4.  Klik in het menu instellingen op **App Service Plan** .
  5.  Klik op de tegel **Prijzen laag** .
  6.  Klik op de tegel, afhankelijk van uw behoeften en klik vervolgens op **selecteren**.  Mogelijk moet u klikken op **Alles weergeven** voor een overzicht van de beschikbare prijzen lagen.

Als uitgangspunt, raden we aan de volgende niveaus:

| Mobiele Service prijzen laag | De Service App prijzen laag |
| :-------------------------- | :----------------------- |
| Gratis                        | Gratis F1                  |
| Basic                       | Basic B1                 |
| Standaard                    | S1-standaard              |

Er is aanzienlijke flexibiliteit bij de keuze van het recht prijzen laag voor uw toepassing.  Raadpleeg de [Serviceprijzen App] voor alle informatie over de prijs van een nieuwe App-Service.

> [AZURE.TIP]De App Service Standard laag bevat toegang tot veel functies die u gebruiken wilt, met inbegrip van ["slots" staging], automatische back-ups en automatisch schalen.  Bekijk de nieuwe mogelijkheden terwijl u er bent!

### <a name="review-migration-scheduler-jobs"></a>Controleer de gemigreerde plannertaken

Taken van Taakplanner wordt tot ongeveer 30 minuten na de migratie niet meer zichtbaar.  Geplande taken blijven actief op de achtergrond.
Uw geplande taken bekijken nadat ze opnieuw weergegeven worden:

  1.  Log in op de [Azure portal].
  2.  Selecteer **Bladeren >**, **planning** invoeren in het vak _Filter_ en selecteer vervolgens **Scheduler collecties**.

Er zijn een beperkt aantal gratis planner taken beschikbaar na de migratie.  Bekijk uw gebruik en de [Scheduler van plan Azure].

### <a name="configure-cors"></a>CORS configureren, indien nodig

Cross-oorsprong resources delen is een techniek waarmee een website toegang tot een Web-API in een ander domein.  Als u met een koppeling naar een website Azure Mobile Services gebruikt, moet u CORS configureert als onderdeel van de migratie.  Als u uitsluitend uit mobiele apparaten Azure Mobile Services opent, vervolgens hoeft CORS niet te worden geconfigureerd, behalve in uitzonderlijke gevallen.

De gemigreerde CORS-instellingen zijn beschikbaar als de instelling van de **MS_CrossDomainWhitelist** App.  Uw site migreren naar de App Service CORS faciliteit:

  1.  Log in op de [Azure portal].
  2.  Selecteer **alle resources** of **App Services** Klik op de naam van de gemigreerde Mobile-Service.
  3.  De blade instellingen wordt standaard geopend.
  4.  **CORS** Klik in het menu van de API.
  5.  Alle oorsprongen toegestaan invoeren in het desbetreffende vak, drukt u na elke opdracht op Enter.
  6.  Als uw lijst met toegestane oorsprongen correct is, klikt u op de knop opslaan.

> [AZURE.TIP]Een van de voordelen van het gebruik van een App Azure Service is dat u uw website en mobiele service op de site uitvoeren kunt.  Zie voor meer informatie de sectie [volgende stappen](#next-steps) .

### <a name="download-publish-profile"></a>Een nieuw profiel publiceren downloaden

Profiel voor het publiceren van uw site wordt gewijzigd wanneer u migreert naar Azure App-Service.  Als u van plan bent voor het publiceren van uw site vanuit Visual Studio, moet u een nieuw profiel publiceren.  Het nieuwe profiel publiceren downloaden:

  1.  Log in op de [Azure portal].
  2.  Selecteer **alle resources** of **App Services** Klik op de naam van de gemigreerde Mobile-Service.
  3.  Klik op **Get profiel publiceren**.

Het bestand PublishSettings is gedownload naar uw computer.  _Sitenaam_wordt normaal gesproken genoemd. PublishSettings.  De publicatie-instellingen importeren in een bestaand project:

  1.  Open Visual Studio en het project Azure Mobile Service.
  2.  Uw project in de **Solution Explorer** met de rechtermuisknop en selecteer **publiceren...**
  3.  Klik op **importeren**
  4.  Klik op **Bladeren** en selecteer het gedownloade bestand publiceren.  Klik op **OK**
  5.  Klik op **Valideren verbinding** zodat het werk van de instellingen publiceren.
  6.  Klik op **publiceren** om uw site te publiceren.


## <a name="working-with-your-site"></a>Werken met uw na de sitemigratie

Aan de slag met de nieuwe App-Service in de [portal Azure] na de migratie.  Hier volgen enkele opmerkingen over de specifieke bewerkingen die u gebruikt in de [Klassieke Portal Azure], samen met hun equivalent App-Service uit te voeren.

### <a name="publishing-your-site"></a>Downloaden en publiceren van een gemigreerde site.

De site is beschikbaar via git- of FTP- en met verschillende andere mechanismen, waaronder WebDeploy, TFS volgt, GitHub en FTP opnieuw kan worden gepubliceerd.  De referenties van de implementatie worden gemigreerd met de rest van uw site.  Als u uw referenties implementatie niet ingesteld of als u ze niet meer weet, kunt u ze opnieuw instellen:

  1. Log in op de [Azure portal].
  2. Selecteer **alle resources** of **App Services** Klik op de naam van de gemigreerde Mobile-Service.
  3. De blade instellingen wordt standaard geopend.
  4. Klik op **referenties implementatie** bij het publiceren menu.
  5. Voer de nieuwe implementatie-referenties in de desbetreffende vakken en klik op de knop opslaan.

U kunt deze referenties op de site met git clone of geautomatiseerde implementaties van GitHub, TFS of volgt instellen.  Zie voor meer informatie de [documentatie over de implementatie van Azure App-Service].

### <a name="appsettings"></a>Instellingen voor toepassing

De meeste instellingen voor een gemigreerde mobiele service zijn beschikbaar via de App instellingen.  Een lijst met de instellingen van de app krijgt u vanuit de [portal Azure].
Weergeven of wijzigen van de instellingen van de app:

  1. Log in op de [Azure portal].
  2. Selecteer **alle resources** of **App Services** Klik op de naam van de gemigreerde Mobile-Service.
  3. De blade instellingen wordt standaard geopend.
  4. **Klik in het menu Algemeen.**
  5. Ga naar de sectie instellingen voor toepassing en uw instelling app vinden.
  6. Klik op de waarde van de instelling van de app om de waarde te bewerken.  Klik op **Opslaan** om het opslaan van de waarde.

U kunt meerdere app instellingen tegelijk bijwerken.

> [AZURE.TIP]Er zijn twee instellingen voor toepassing met dezelfde waarde.  Ziet u bijvoorbeeld _ApplicationKey_ en _MS\_ApplicationKey_.  Beide instellingen werken op hetzelfde moment.

### <a name="authentication"></a>Verificatie

Alle verificatie-instellingen zijn beschikbaar als App instellingen in de gemigreerde site.  Om uw verificatie-instellingen bijwerken, moet u de juiste app-instellingen wijzigen.  De volgende tabel ziet u de juiste app-instellingen voor de verificatie:

| Provider          | Client-ID                 | Het geheim van de client                | Andere instellingen             |
| :---------------- | :------------------------ | :--------------------------- | :------------------------- |
| Microsoft-Account | **MS\_MicrosoftClientID**  | **MS\_MicrosoftClientSecret** | **MS\_MicrosoftPackageSID** |
| Facebook          | **MS\_FacebookAppID**      | **MS\_FacebookAppSecret**     |                            |
| Twitter           | **MS\_TwitterConsumerKey** | **MS\_TwitterConsumerSecret** |                            |
| Google            | **MS\_GoogleClientID**     | **MS\_GoogleClientSecret**    |                            |
| Azure AD          | **MS\_AadClientID**        |                              | **MS\_AadTenants**          |

Opmerking: **MS\_AadTenants** wordt opgeslagen als een door komma's gescheiden lijst met huurder domeinen (de 'Huurder toegestaan' velden in de portal mobiele diensten).

> [AZURE.WARNING] **De verificatiemechanismen niet gebruiken in het menu instellingen**
>
> Azure App-Service biedt een aparte "Nee-code" verificatie en machtiging onder de _Authentication / Authorization_ menu instellingen en de (afgeschaft) _Mobile verificatie_ -optie in het menu instellingen.  Deze opties zijn niet compatibel met een gemigreerde Azure Mobile-Service.  U kunt [uw site bijwerken](app-service-mobile-net-upgrading-from-mobile-services.md) om te profiteren van de verificatie Azure App-Service.

### <a name="easytables"></a>Gegevens

Het tabblad _gegevens_ in de mobiele diensten is vervangen door een _Eenvoudige tabellen_ binnen de portal Azure.  Toegang tot eenvoudige tabellen:

  1. Log in op de [Azure portal].
  2. Selecteer **alle resources** of **App Services** Klik op de naam van de gemigreerde Mobile-Service.
  3. De blade instellingen wordt standaard geopend.
  4. **Eenvoudige tabellen** Klik in het menu mobiele.

U kunt een tabel toevoegen door te klikken op de knop **toevoegen** of bestaande tabellen openen door te klikken op de naam van een tabel.  Er zijn diverse bewerkingen u vanuit deze blade doen kunt, met inbegrip van:

* Tabelmachtigingen voor wijzigen
* De operationele scripts bewerken
* De tabelschema beheren
* De tabel verwijderen
* De inhoud van de tabel wissen
* Bepaalde rijen van de tabel verwijderen

### <a name="easyapis"></a>API

Het tabblad _API_ in Mobile Services is vervangen door een _Eenvoudige API's_ binnen de portal Azure.  Toegang tot eenvoudige API's:

  1. Log in op de [Azure portal].
  2. Selecteer **alle resources** of **App Services** Klik op de naam van de gemigreerde Mobile-Service.
  3. De blade instellingen wordt standaard geopend.
  4. Klik op **Eenvoudige API's** in het menu van het mobiele.

De gemigreerde API's staan in het blad.  U kunt ook een API uit deze blade toevoegen.  Klik op de API voor het beheren van een specifieke API.
U kunt de machtigingen aanpassen en bewerken van de scripts voor de API van de nieuwe blade.

### <a name="on-demand-jobs"></a>Taken van Taakplanner

Alle geplande taken zijn beschikbaar via de sectie Scheduler taak collecties.  Toegang tot de geplande taken:

  1. Log in op de [Azure portal].
  2. Selecteer **Bladeren >**, **planning** invoeren in het vak _Filter_ en selecteer vervolgens **Scheduler collecties**.
  3. Selecteer de taak collectie voor uw site.  Het is met de naam _sitenaam_-taken.
  4. Klik op **Instellingen**.
  5. Klik op **Taakplanner** onder beheren.

Geplande taken worden weergegeven met de frequentie die u hebt opgegeven voor de migratie.  Taken op aanvraag worden uitgeschakeld.  Een taak op aanvraag uitvoeren:

  1. Selecteer de taak die u wilt uitvoeren.
  2. Klik op **inschakelen** als u de taak zo nodig.
  3. Klik op **Instellingen**en klik op **schema**.
  4. Een herhaling van **één keer**selecteren en klik vervolgens op **Opslaan**

Uw taken op verzoek bevinden zich in `App_Data/config/scripts/scheduler post-migration`.  Wij raden aan dat u alle aangevraagde taken naar [WebJobs] of [functies converteren].  Nieuwe taken van Taakplanner als [WebJobs] of [functies]schrijven.

### <a name="notification-hubs"></a>Melding Hubs

Melding Hubs Mobile Services gebruikt voor push-meldingen.  De volgende App-instellingen worden gebruikt voor het koppelen van de Hub melding met uw mobiele Service na de migratie:

| Toepassingsinstelling                    | Beschrijving                              |
| :------------------------------------- | :--------------------------------------- |
| **MS\_PushEntityNamespace**             | De kennisgeving Hub Namespace           |
| **MS\_NotificationHubName**             | De naam van de Hub melding                |
| **MS\_NotificationHubConnectionString** | De verbindingsreeks melding Hub   |
| **MS\_NamespaceName**                   | Een alias voor MS_PushEntityNamespace      |

Uw melding Hub wordt beheerd via de [portal Azure].  Noteer de naam van de Hub van de melding (u kunt dit vinden met de App instellingen):

  1. Log in op de [Azure portal].
  2. Selecteer **Bladeren**>, selecteert u **Melding Hubs**
  3. Klik op de naam van de kennisgeving Hub die is gekoppeld aan de mobiele service.

> [AZURE.NOTE]Als uw melding HUb is een 'Mixed', is het niet zichtbaar.  'Gemengd' Typ melding hubs maken gebruik van zowel melding Hubs en oudere voorzieningen voor Service Bus.  [De gemengde naamruimten converteren] voordat u doorgaat.  Zodra de conversie voltooid is, verschijnt de melding hub in [Azure portal].

Bekijk de [Melding Hubs] -documentatie voor meer informatie.

> [AZURE.TIP]Melding Hubs beheerfuncties in [Azure portal] zijn nog steeds in de voorbeeldweergave.  De [Klassieke Portal Azure] blijft beschikbaar voor het beheren van alle Hubs van uw melding.

### <a name="legacy-push"></a>Oude Push instellingen

Als u op uw mobiele service vóór de invoering van kennisgeving Hubs Push hebt geconfigureerd, gebruikt u _oude push_.  Als u met behulp van Push en er geen melding weergegeven in de configuratie van de Hub, is het waarschijnlijk dat u gebruikt _verouderde push_.  Deze functie wordt gemigreerd met de functies.  We raden echter aan om te upgraden naar Hubs melding direct nadat de migratie voltooid is.

In de tussentijd zijn de oude push instellingen (met de opmerkelijke uitzondering van het certificaat APNS) beschikbaar in App instellingen.  Het certificaat APNS bijwerken door het juiste bestand in het bestandssysteem.

### <a name="app-settings"></a>Andere instellingen App

De volgende extra app instellingen worden gemigreerd van uw mobiele Service en beschikbaar onder *Instellingen* > *App-instellingen*:

| Toepassingsinstelling              | Beschrijving                             |
| :------------------------------- | :-------------------------------------- |
| **MS\_MobileServiceName**         | De naam van uw app                    |
| **MS\_MobileServiceDomainSuffix** | Het voorvoegsel van het domein. Internet Explorer Azure-mobile.net |
| **MS\_ApplicationKey**            | De sleutel voor uw toepassing                    |
| **MS\_MasterKey**                 | De hoofdsleutel app                     |

De toepassing en master sleutel zijn gelijk aan de sleutels van de toepassing van uw oorspronkelijke Mobile-Service.  In het bijzonder wordt de toepassingstoets door mobiele clients verzonden voor het valideren van het gebruik van de mobiele-API.

### <a name="cliequivalents"></a>Opdrachtregelopdrachten

Meer kunt u de opdracht _azure mobiel_ voor het beheren van uw Azure Mobile Services-site.  In plaats daarvan, zijn veel functies vervangen door de opdracht _azure site_ .  De volgende tabel gebruiken om equivalenten voor veelgebruikte opdrachten:

| _Azure Mobile_ Opdracht                     | Equivalente _Azure Site_ , opdracht            |
| :----------------------------------------- | :----------------------------------------- |
| mobiele locaties                           | lijst van site-locatie                         |
| mobiele lijst                                | lijst met sites                                  |
| mobiele weergeven _naam_                         | _de naam_ van de site weergeven                           |
| mobiele opnieuw de _naam_                      | _naam_ van de site opnieuw op te starten                        |
| in dat geval mobiele _naam_                     | _commitId_ _naam_ van site-implementatie opnieuw distribueren |
| mobiele sleutel instellen _naam_ _type_ _waarde_       | _toets_ _de naam_ van site appsetting verwijderen <br/> site appsetting _sleutel_toevoegen=_ _naam_ _ |
| mobiele config _naam_                  | site appsetting _naam_                |
| mobiele config _naam_ _sleutel_ ophalen             | _sleutel_ _de naam_ van site appsetting weergeven          |
| mobiele config instellen _naam_ _sleutel_             | _toets_ _de naam_ van site appsetting verwijderen <br/> site appsetting _sleutel_toevoegen=_ _naam_ _ |
| mobiele domein _naam_                  | site domein _naam_                    |
| mobiele domein _naam_ _domein_ toevoegen          | domein van de site _de naam_ van het _domein_ toevoegen            |
| _naam_ van het mobiele domein verwijderen                | domein verwijderen _domein_ _naam_         |
| _naam_ van mobiele schaal weergeven                   | _de naam_ van de site weergeven                           |
| _naam_ van mobiele schaal wijzigen                 | site schaal modus _modus_ _naam_ <br /> schaal exemplaren _exemplaren_ van de site _naam_ |
| mobiele appsetting _naam_              | site appsetting _naam_                |
| mobiele appsetting _naam_ _sleutel_ _waarde_ toevoegen | site appsetting _sleutel_toevoegen=_ _naam_ _   |
| _naam_ _sleutel_ voor mobiele appsetting verwijderen      | _toets_ _de naam_ van site appsetting verwijderen        |
| _naam_ _sleutel_ voor mobiele appsetting weergeven        | _toets_ _de naam_ van site appsetting verwijderen        |

Verificatie-instellingen voor push-meldingen of bijwerken door de juiste instelling van toepassing bij te werken.
Bestanden bewerken en publiceren van uw site via ftp of git.

### <a name="diagnostics"></a>Registratie van diagnostische gegevens en

Registratie van diagnostische gegevens is normaal gesproken in een Azure App-Service uitgeschakeld.  Diagnostische logboekregistratie inschakelen:

  1. Log in op de [Azure portal].
  2. Selecteer **alle resources** of **App Services** Klik op de naam van de gemigreerde Mobile-Service.
  3. De blade instellingen wordt standaard geopend.
  4. **Diagnostische logboeken** in het menu functies selecteren.
  5. Klik **op** de volgende logboekbestanden: **Toepassing registratie (Filesystem)**, **gedetailleerde foutberichten**en **tracering van mislukte aanvragen**
  6. Klik op **Bestandssysteem** voor logboekregistratie van webserver
  7. Klik op **Opslaan**

De logboeken bekijken:

  1. Log in op de [Azure portal].
  2. Selecteer **alle resources** of **App Services** Klik op de naam van de gemigreerde Mobile-Service.
  3. Klik op de knop **Extra**
  4. **Logboek Stream** selecteren in het menu OBSERVE.

Logboeken worden weergegeven in het venster als ze worden gegenereerd.  U kunt ook downloaden van de logboeken voor latere analyse met behulp van de referenties van uw implementatie. Zie voor meer informatie de documentatie bij de [registratie] .

## <a name="known-issues"></a>Bekende problemen

### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Verwijderen van een kloon gemigreerd Mobile App zorgt ervoor dat een site-onderhoud

Als u de gemigreerde mobiele service met Azure PowerShell klonen en verwijder vervolgens de kloon, wordt de DNS-vermelding voor de productieservice verwijderd.  Uw site is niet langer toegankelijk via het Internet.  

Oplossing: Als u uw site te klonen wilt, doen via de portal.

### <a name="changing-webconfig-does-not-work"></a>Web.config wijzigen werkt niet

Als u een ASP.NET-website, worden wijzigingen in de `Web.config` bestand kan niet worden toegepast.  De App Azure Service bouwt een geschikt `Web.config` bestand tijdens het opstarten van de runtime Mobile Services ondersteunen.  U kunt bepaalde instellingen (zoals aangepaste headers) met behulp van een XML-transformatie-bestand overschrijven.  Maak een bestand genoemd in `applicationHost.xdt` -dit bestand moet uiteindelijk de `D:\home\site` op de Azure-Service.  Uploaden van de `applicationHost.xdt` het bestand via een script voor aangepaste implementatie of rechtstreeks via Kudu.  Hier volgt een voorbeelddocument:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Zie voor meer informatie de documentatie [XDT transformeren monsters] op GitHub.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Gemigreerde Mobile Services kan niet worden toegevoegd aan beheer van verkeer

Wanneer u een serviceprofiel verkeer maakt, kunt u niet rechtstreeks een gemigreerde mobiele service om het profiel te kiezen.  Gebruik een 'externe endpoint'.  Het externe endpoint kan alleen worden toegevoegd door middel van PowerShell.  Zie voor meer informatie de [handleiding verkeer Manager](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Volgende stappen

Nu uw toepassing is gemigreerd naar App Service, er zijn nog meer functies die u kunt gebruiken:

  * Implementatie- [staging-sleuven] kunt u fase wijzigingen aan uw site en het uitvoeren van A / B testen.
  * [WebJobs] bieden een vervanging voor bellen op geplande taken.
  * U kunt [implementeren voortdurend] uw site door uw site koppelen aan GitHub, TFS of volgt.
  * [Inzichten van de toepassing] kunt u uw site controleren.
  * Een website en een mobiele-API uit dezelfde code hebben.

### <a name="upgrading-your-site"></a>Uw mobiele Services-site een upgrade uitvoeren naar Azure Mobile Apps SDK

  * Voor serverprojecten met een op basis van Node.js vindt de nieuwe [Mobiele Apps Node.js SDK] u diverse nieuwe functies. Bijvoorbeeld, kunt u nu plaatselijke ontwikkeling en foutopsporing, een Node.js versie boven 0,10 en aanpassen met een Express.js middleware.

  * Voor. Projecten NET gebaseerde server, de nieuwe [mobiele Apps SDK NuGet pakketten](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) hebben meer flexibiliteit voor NuGet afhankelijkheden.  De nieuwe App Service-verificatie ondersteunen deze pakketten en opstellen met een ASP.NET-project. Meer informatie over het upgraden Zie [upgraden uw bestaande .NET Mobile Service App-service](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[Serviceprijsstelling App]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Inzichten van toepassing]: ../application-insights/app-insights-overview.md
[Automatisch schalen]: ../app-service-web/web-sites-scale.md
[Azure App-Service]: ../app-service/app-service-value-prop-what-is.md
[Azure App Service-documentatie over implementatie]: ../app-service-web/web-sites-deploy.md
[Azure klassieke Portal]: https://manage.windowsazure.com
[Azure portal]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure Scheduler plannen]: ../scheduler/scheduler-plans-billing.md
[continu implementeren]: ../app-service-web/app-service-continuous-deployment.md
[Converteren van de gemengde naamruimten]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[aangepaste domeinnamen]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[algemene beschikbaarheid van Azure App Service]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybride verbindingen]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[Logboekregistratie]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobiele Apps Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[Mobiele Services-Service versus App]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Melding Hubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[prestaties controleren]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[Back-up van uw mobiele Service]: ../mobile-services/mobile-services-disaster-recovery.md
[staging-sleuven]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[XDT-transformatie-voorbeelden]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[Functies]: ../azure-functions/functions-overview.md
