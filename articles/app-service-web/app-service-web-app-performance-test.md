<properties
   pageTitle="Test de prestaties van uw Azure web app | Microsoft Azure"
   description="Azure web app prestatietests uitvoeren om te controleren hoe uw app omgaat met laden van de gebruiker worden uitgevoerd. Reactietijd meten en fouten die op problemen duiden mogelijk te vinden."
   services="app-service\web"
   documentationCenter=""
   authors="ecfan"
   manager="douge"
   editor="jimbe"/>

<tags
   ms.service="app-service-web"
   ms.workload="web"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="05/25/2016"
   ms.author="estfan; manasma; ahomer"/>

# <a name="performance-test-your-azure-web-app-under-load"></a>Prestaties testen uw Azure web app onder belasting

De prestaties van uw web app controleren voordat u deze starten of implementeren van updates voor de productie. Op die manier kunt u beter beoordelen of uw app klaar voor release is. Erop vertrouwen dat uw app kan omgaan met het verkeer tijdens de piek of op de volgende marketingcampagne.

Tijdens de openbare preview kunt u prestatietest uw app gratis in de Portal Azure.
Deze tests belasting op uw app gebruiker gedurende een bepaalde periode simuleren en meten van uw app reactie. De testresultaten tonen bijvoorbeeld hoe snel uw app reageert op een specifiek aantal gebruikers. Ook geven ze niet hoeveel aanvragen, die mogelijk duiden op problemen met de app.      

![Zoeken naar problemen met de prestaties in uw web app](./media/app-service-web-app-performance-test/azure-np-perf-test-overview.png)

## <a name="before-you-start"></a>Voordat u begint

* Als u geen sjabloon hebt, moet je een [abonnement op Azure](https://account.windowsazure.com/subscriptions). Informatie over hoe u kunt [gratis een Azure account openen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

* Je hebt een [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) rekening te houden de geschiedenis van de test. Een geschikte account wordt automatisch gemaakt bij het instellen van de proef. Of maak een nieuwe account of een bestaande account gebruiken als u de rekeninghouder bent. 

* Implementeer uw app voor het testen in een testomgeving. Hebben uw app gebruik van een App Service plan dan het plan dat wordt gebruikt in de productie. Op die manier die u niet van invloed zijn op alle bestaande klanten of vertragen van uw app in de productie. 

## <a name="set-up-and-run-your-performance-test"></a>Instellen en uitvoeren van de proef

0.  Aanmelden bij de [Azure Portal](https://portal.azure.com). Log in om een Visual Studio Team Services gebruikt die u bezit, als eigenaar van de account.

0.  Ga naar uw web app.

    ![Ga naar bladeren alle, Web Apps, uw web app](./media/app-service-web-app-performance-test/azure-np-web-apps.png)

0.  Ga naar de **proef**.

    ![Ga naar extra, prestatietest](./media/app-service-web-app-performance-test/azure-np-web-app-details-tools-expanded.png)
 
0. Nu zult u een [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) -account zodat uw prestaties test geschiedenis koppelen.

    Als er een Team Services-account te gebruiken, selecteert u die account. Als u dit niet doet, moet u een nieuwe account maken.

    ![Selecteer bestaande Team Services-account of maak een nieuwe account](./media/app-service-web-app-performance-test/azure-np-no-vso-account.png)

0.  Maak uw test waarbij de prestaties. De gegevens instellen en uitvoeren van de test. 

Tijdens de test wordt uitgevoerd, kunt u de resultaten in realtime bekijken.

Stel dat we een app die van coupons op vakantie-Verkoop vorig jaar hebt gegeven. Deze gebeurtenis heeft geduurd 15 minuten met een piekbelasting van 100 gelijktijdige van klanten. We willen het aantal klanten dit jaar verdubbelen. We willen ook klanttevredenheid verbeteren door verkorting van de pagina laden van 5 seconden op 2 seconden. We zullen dus onze bijgewerkte app prestaties met 250 gebruikers gedurende 15 minuten test.

We zullen laden op onze app simuleren door virtuele gebruikers (klanten) genereren die onze website bezoeken op hetzelfde moment. Dit geeft ons hoeveel aanvragen zijn niet goed werkt of reageert traag.

  ![Maken, instellen en uitvoeren van de proef](./media/app-service-web-app-performance-test/azure-np-new-performance-test.png)

   *  De standaard-URL van uw web app wordt automatisch toegevoegd. 
   U kunt de URL voor het testen van andere pagina's (HTTP GET-aanvragen alleen).

   *  Plaatselijke omstandigheden simuleren en latentie te verlagen, selecteert u een locatie die het dichtst bij de gebruikers voor het genereren van belasting.

  Hier wordt de proef uitgevoerd. Tijdens de eerste minuut wordt geladen langzamer dan we willen onze pagina.

  ![Prestatietest uitgevoerd met real-time gegevens](./media/app-service-web-app-performance-test/azure-np-running-perf-test.png)

  Nadat de test is voltooid, weten we dat de pagina sneller wordt geladen na de eerste minuut. Dit helpt identificeren waar we mogelijk wilt laten beginnen met het oplossen van problemen.

  ![Voltooide prestatietest geeft resultaten, met inbegrip van mislukte aanvragen](./media/app-service-web-app-performance-test/azure-np-perf-test-done.png)

## <a name="test-multiple-urls"></a>Meerdere URL's testen

U kunt ook prestatietests met meerdere URL's die een van de end-to-end gebruikersscenario voorstellen door het uploaden van een bestand voor Visual Studio Web Test uitvoeren. Aantal manieren kunt u een Visual Studio Web Test bestand zijn:

* [Verkeer met Fiddler vastleggen en exporteren als een bestand voor Visual Studio Web Test](http://docs.telerik.com/fiddler/Save-And-Load-Traffic/Tasks/VSWebTest)
* [Maak een testbestand laden in Visual Studio](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release)

Uploaden en een bestand voor Visual Studio Web Test uitvoeren:
 
0. De stappen hierboven om het openen van de **nieuwe prestaties testen** blade.
   In deze blade, kiest u de optie CONFIGFURE testen met behulp van de bladeserver **configureren met behulp van test** te openen.  

    ![U opent de configureren belasting blade testen](./media/app-service-web-app-performance-test/multiple-01-authoring-blade.png)

0. Controleren of de proef van TYPE is ingesteld op **Visual Studio Web Test** en selecteer de HTTP-archiefbestand.
    Het pictogram 'map' gebruiken voor het openen van het dialoogvenster bestand selecteren.

    ![Uploaden van een bestand met meerdere URL Visual Studio Web Test](./media/app-service-web-app-performance-test/multiple-01-authoring-blade2.png)

    Nadat het bestand is geüpload, kunt u de lijst met URL's te testen in de sectie URL-informatie bekijken.
 
0. De belasting van de gebruiker en test, duur, en kies vervolgens **test uitvoeren**.

    ![Selecteren van de gebruiker laden en de duur](./media/app-service-web-app-performance-test/multiple-01-authoring-blade3.png)

    Nadat de test is voltooid, ziet u de resultaten in twee deelvensters. Het linker deelvenster bevat de performnace informatie als een reeks van grafieken.

    ![Het deelvenster prestaties](./media/app-service-web-app-performance-test/multiple-01a-results.png)

    Het rechter deelvenster bevat een lijst van mislukte aanvragen, met het soort fout en het aantal keren dat deze is opgetreden.

    ![Het deelvenster van de mislukte aanvraag](./media/app-service-web-app-performance-test/multiple-01b-results.png)

0. Test opnieuw uitvoeren door het pictogram **opnieuw uitvoeren** boven in het rechterdeelvenster.

    ![De test wordt overgedaan](./media/app-service-web-app-performance-test/multiple-rerun-test.png)

##  <a name="q--a"></a>Q & A

#### <a name="q-is-there-a-limit-on-how-long-i-can-run-a-test"></a>V: is er een limiet op hoe lang kan ik een test uitgevoerd? 

**A**: Ja, uitvoeren van de test een uur lang in de Portal Azure.

#### <a name="q-how-much-time-do-i-get-to-run-performance-tests"></a>V: hoeveel tijd krijg ik prestatietests uitvoeren? 

**A**: na de openbare preview, krijgt u 20.000 virtuele gebruiker minuten (VUMs) gratis elke maand met de Visual Studio Team Services-account. Een VUM is het aantal virtuele gebruikers vermenigvuldigd met het aantal minuten in de test. Als uw behoeften de vrije limiet overschrijdt, kunt u meer tijd kopen en betaalt alleen voor wat u gebruikt.

#### <a name="q-where-can-i-check-how-many-vums-ive-used-so-far"></a>V: waar kan ik controleren hoeveel VUMs ik tot nu toe heb gebruikt?

**A**: U kunt dit bedrag in de Portal Azure controleren.

![Ga naar uw account Team Services](./media/app-service-web-app-performance-test/azure-np-vso-accounts.png)

![Gebruikte VUMs controleren](./media/app-service-web-app-performance-test/azure-np-vso-accounts-vum-summary.png)

#### <a name="q-what-is-the-default-option-and-are-my-existing-tests-impacted"></a>V: Wat is de standaardoptie en mijn bestaande tests van invloed?

**A**: de optie voor het laden van prestatietests is een handmatige test - is hetzelfde als het testen voordat u de meerdere URL optie toegevoegd aan de portal.
Uw bestaande tests blijven de geconfigureerde URL gebruiken en werkt als voorheen.

#### <a name="q-what-features-not-supported-in-the-visual-studio-web-test-file"></a>V: welke functies niet worden ondersteund in het bestand Visual Studio Web Test?

**A**: op dit moment deze functie biedt geen ondersteuning voor plug-ins, gegevensbronnen en extractie regels Web Test. U moet het bestand Web Test als u deze wilt bewerken. We hopen dat u aan het toevoegen van ondersteuning voor deze functies in toekomstige updates.

#### <a name="q-does-it-support-any-other-web-test-file-formats"></a>V: het ondersteunt andere bestandsindelingen Web Test?
  
**A**: op aanwezig alleen Visual Studio Web Test format-bestanden worden ondersteund.
We zijn blij te horen van u, als u ondersteuning nodig voor andere bestandsindelingen. Een e-mail sturen naar [vsoloadtest@microsoft.com](mailto:vsoloadtest@microsoft.com).

#### <a name="q-what-else-can-i-do-with-a-visual-studio-team-services-account"></a>V: wat kan ik doen met een Visual Studio Team Services-account?

**A**: uw nieuwe account vindt u op ```https://{accountname}.visualstudio.com```. Uw code delen, bouwen, testen, bijhouden van werk en schip software – alles in de cloud met een hulpprogramma of taal. Meer informatie over hoe uw team gemakkelijker samen te werken en het implementeren van voortdurend helpen [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) -functies en -services.

## <a name="see-also"></a>Zie ook

* [Eenvoudige wolk prestatietests uitvoeren](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-simple-cloud-load-test)
* [Apache Jmeter prestatietests uitvoeren](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-jmeter-test)
* [Record en replay cloud-gebaseerde tests laden](https://www.visualstudio.com/docs/test/performance-testing/getting-started/record-and-replay-cloud-load-tests)
* [Prestaties testen van uw app in de cloud](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing)
