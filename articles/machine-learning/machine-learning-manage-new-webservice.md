<properties
    pageTitle="Beheren van een webservice met behulp van de portal Azure Machine Learning Web Serivces | Microsoft Azure"
    description="Toegang tot de werkruimten Azure Machine Learning, beheren en implementeren en beheren van webservices API ML"
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>


# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Een webservice met behulp van de portal Azure Machine Learning Web-Services beheren

U kunt uw Machine Learning nieuwe en klassieke webservices met behulp van de Web Services van Microsoft Azure Machine Learning portal beheren. Aangezien webservices klassieke en nieuwe Web services zijn gebaseerd op verschillende onderliggende technologie, kunt u mogelijkheden voor iets anders hebben voor elk van hen.

U kunt in de webservices van Machine Learning portal:

- Controleren hoe de webservice wordt gebruikt.
- Configureren van de beschrijving, de toetsen bijwerken voor het web service (alleen nieuw), werken uw opslag account sleutel (nieuw alleen) inschakelen logboekregistratie en in- of uitschakelen voorbeeldgegevens.
- De web-service verwijderen.
- Maken, delete of update facturering plannen (alleen nieuw).
- Toevoegen en verwijderen van de eindpunten (alleen klassiek)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="manage-new-web-services"></a>Nieuwe Web-services beheren

De nieuwe Web-services beheren:

1.  Aanmelden bij de [Web Services van Microsoft Azure Machine Learning](https://services.azureml.net/quickstart) portal met uw account Microsoft Azure - account die is gekoppeld aan het abonnement Azure gebruiken.
2.  Klik in het menu **Web Services**.

Er verschijnt een lijst van Web-services geïmplementeerd voor uw abonnement. 

Klik op Web Services voor het beheren van een webservice. U kunt op de pagina Web Services:

- Klik op de webservice beheren.
- Klik op de facturering plannen voor de webservice te werken.
- Verwijderen van een webservice.
- Een webservice kopiëren en deze implementeren in een andere regio.

Wanneer u op een webservice, opent u de pagina met Quickstart. De pagina met Quickstart heeft twee opties waarmee u uw web-service beheren:

- **DASHBOARD** - kunt u gebruik van de Web service weergeven.
- **Configureren** - kunt u beschrijvende tekst toevoegen, bijwerken van de sleutel voor de opslag die is gekoppeld aan de webservice en inschakelen of uitschakelen van voorbeeldgegevens.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Controleren hoe de webservice wordt gebruikt ###

Klik op het tabblad **DASHBOARD** .

Van het dashboard, kunt u algemene gebruik van de webservice in een bepaalde tijdsperiode weergeven. Kunt u de periode in het vervolgmenu van de periode in de rechterbovenhoek van het gebruik van grafieken. Het dashboard ziet u de volgende informatie:

- **Aanvragen verloop van tijd** wordt een stap grafiek van het aantal aanvragen in de geselecteerde periode weergegeven. Kunt identificeren als er pieken in gebruik.
- **Aanvragen van de verzoek-antwoord** geeft de totale verzoek-antwoord oproepen die de service heeft ontvangen over de geselecteerde periode en hoeveel ze is mislukt.
- **Gemiddelde aanvraag berekenen reactietijd van** het gemiddelde weergegeven van de tijd die nodig is voor het uitvoeren van de ontvangen verzoeken.
- **Batch aanvragen** geeft het totale aantal partij verzoeken de service gedurende de geselecteerde tijdsperiode en hoeveel ze niet heeft ontvangen.
- **Gemiddelde latentie van de functie** weergegeven gemiddelde van de tijd die nodig is voor het uitvoeren van de ontvangen verzoeken.
- **Fouten** geeft het totale aantal fouten dat zich heeft voorgedaan op het aanroepen van de webservice.
- **Services-kosten** weergegeven de kosten voor de facturering plan dat is gekoppeld aan de service.

### <a name="configuring-the-web-service"></a>De webservice configureren ###

Klik op de optie **configureren** .

U kunt de volgende eigenschappen bijwerken:

* **Omschrijving** kunt u een omschrijving invoeren voor de webservice.
* **Titel** kunt u een titel invoeren voor de webservice
* **Sleutels** kunt u de API primaire en secundaire sleutels draaien.
* **Opslag account toets** kunt u de sleutel voor de opslag die is gekoppeld aan de wijzigingen van de service Web bijwerken. 
* **Voorbeeldgegevens inschakelen** kunt u voorbeeldgegevens die u gebruiken kunt voor het testen van de verzoek-antwoord-service bieden. Als u de webservice in Studio van Machine Learning gemaakt, is de voorbeeldgegevens genomen uit de gegevens die uw gebruikte trainen uw model. Als u de service via programmacode hebt gemaakt, worden de gegevens worden opgehaald uit de voorbeeldgegevens die u als onderdeel van het pakket JSON verstrekt.

### <a name="managing-billing-plans"></a>Factuuradres plannen beheren ###

Klik op de optie **plannen** van de pagina services Quickstart. U kunt ook klikken op het plan dat is gekoppeld aan een specifieke Web-service voor het beheren van dat plan.

* **Nieuw** kunt u een nieuw plan maken.
* **Plan voor software-exemplaar** kunt u scale-"een bestaand schema capaciteit toe te voegen.
* **Upgrade/DownGrade** kunt u "scale-up een bestaand schema capaciteit toe te voegen.
* **Verwijderen** , kunt u een schema verwijderen.

Klik op een plan om het dashboard weer te geven. Het dashboard biedt u een snapshot of plan gebruik gedurende een geselecteerde periode. Klik op de vervolgkeuzelijst in de rechterbovenhoek van dashboard voor **periode** de periode weer te geven. 

Het dashboard plan bevat de volgende informatie:

* **Beschrijving van het plan** wordt informatie weergegeven over de kosten en de capaciteit die is gekoppeld aan het plan.
* **Gebruik van het plan** wordt het aantal transacties en compute-uren worden gebracht tegen het plan.
* **Web Services** bevat het nummer van de webservices die van dit plan gebruikmaken.
* **Top met aanroepen naar een webservice** worden weergegeven boven vier webservices aanroepen die in rekening worden gebracht tegen het plan.
* **Top Web Services per uur berekenen** geeft de top vier webservices die gebruikmaken van compute bronnen die in rekening worden gebracht tegen het plan.

## <a name="manage-classic-web-services"></a>Klassieke webservices beheren

> [AZURE.NOTE] De procedures in deze sectie zijn relevant zijn voor het beheren van webservices klassieke via de portal Azure Machine Learning Web-Services. Zie voor meer informatie over het beheren van klassieke Web services via de Studio Machine Learning en de klassieke Azure portal [beheren een werkruimte Azure Machine Learning](machine-learning-manage-workspace.md).

De klassieke Web services beheren:

1.  Aanmelden bij de [Web Services van Microsoft Azure Machine Learning](https://services.azureml.net/quickstart) portal met uw account Microsoft Azure - account die is gekoppeld aan het abonnement Azure gebruiken.
2.  Klik op het menu, **Klassieke Web Services**.

Klik op **Klassieke Web Services**voor het beheren van een klassieke webservice. U kunt via de klassieke Web Services-pagina:

- Klik op de web-service om de bijbehorende eindpunten weer te geven.
- Verwijderen van een webservice.

Bij het beheren van een klassieke webservice beheren u elk van de eindpunten afzonderlijk. Wanneer u op een web-service op de pagina Web Services, opent u de lijst van eindpunten is gekoppeld aan de service. 

U kunt toevoegen en verwijderen van de eindpunten van de service op de pagina klassieke Web-Service-eindpunt. Zie voor meer informatie over het toevoegen van eindpunten [Eindpunten maken](machine-learning-create-endpoint.md).

Klik op een van de eindpunten op de pagina met Quickstart openen. Op de pagina Quickstart zijn er twee opties waarmee u uw web-service beheren:

- **DASHBOARD** - kunt u gebruik van de Web service weergeven.
- **Configureren** - kunt u beschrijvende tekst toevoegen, foutregistratie in- en uitschakelen, update de sleutel voor de opslag die is gekoppeld aan de webservice rekening en in- en uitschakelen voorbeeldgegevens.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Controleren hoe de webservice wordt gebruikt ###

Klik op het tabblad **DASHBOARD** .

Van het dashboard, kunt u algemene gebruik van de webservice in een bepaalde tijdsperiode weergeven. Kunt u de periode in het vervolgmenu van de periode in de rechterbovenhoek van het gebruik van grafieken. Het dashboard ziet u de volgende informatie:

- **Aanvragen verloop van tijd** wordt een stap grafiek van het aantal aanvragen in de geselecteerde periode weergegeven. Kunt identificeren als er pieken in gebruik.
- **Aanvragen van de verzoek-antwoord** geeft de totale verzoek-antwoord oproepen die de service heeft ontvangen over de geselecteerde periode en hoeveel ze is mislukt.
- **Gemiddelde aanvraag berekenen reactietijd van** het gemiddelde weergegeven van de tijd die nodig is voor het uitvoeren van de ontvangen verzoeken.
- **Batch aanvragen** geeft het totale aantal partij verzoeken de service gedurende de geselecteerde tijdsperiode en hoeveel ze niet heeft ontvangen.
- **Gemiddelde latentie van de functie** weergegeven gemiddelde van de tijd die nodig is voor het uitvoeren van de ontvangen verzoeken.
- **Fouten** geeft het totale aantal fouten dat zich heeft voorgedaan op het aanroepen van de webservice.
- **Services-kosten** weergegeven de kosten voor de facturering plan dat is gekoppeld aan de service.

### <a name="configuring-the-web-service"></a>De webservice configureren ###

Klik op de optie **configureren** .

U kunt de volgende eigenschappen bijwerken:

* **Omschrijving** kunt u een omschrijving invoeren voor de webservice. Omschrijving is een verplicht veld.
* **Logboekregistratie** kunt u in- of uitschakelen van de foutregistratie op het eindpunt. Voor meer informatie over logboekregistratie Zie Enable [logging voor webservices van Machine Learning](machine-learning-web-services-logging.md).
* **Voorbeeldgegevens inschakelen** kunt u voorbeeldgegevens die u gebruiken kunt voor het testen van de verzoek-antwoord-service bieden. Als u de webservice in Studio van Machine Learning gemaakt, is de voorbeeldgegevens genomen uit de gegevens die uw gebruikte trainen uw model. Als u de service via programmacode hebt gemaakt, worden de gegevens worden opgehaald uit de voorbeeldgegevens die u als onderdeel van het pakket JSON verstrekt.

## <a name="grant-or-suspend-access-to-web-services-for-users-in-the-portal"></a>Toekennen of opschorten van toegang tot webservices voor gebruikers in de portal

Met de klassieke Azure portal, kunt u toestaan of weigeren van toegang tot specifieke gebruikers.

### <a name="access-for-users-of-new-web-services"></a>Toegang voor gebruikers van nieuwe Web services

Als u wilt dat andere gebruikers werken met webservices in de portal Azure Machine Learning Web Services, moet u deze toevoegen als co-beheerders op uw abonnement Azure.

Aanmelden bij de [Azure klassieke portal](https://manage.windowsazure.com/) met uw account Microsoft Azure - account die is gekoppeld aan het abonnement Azure gebruiken.

1. In het navigatiedeelvenster klikt u op **Instellingen**, klik op **Administrators**.
2. Onderaan in het venster, klikt u op **toevoegen**. 
3. Typ het e-mailadres van de persoon die u wilt toevoegen als co-beheerder en selecteer vervolgens het abonnement dat u wilt dat de CO-beheerder toegang tot in het dialoogvenster ADD A CO-ADMINISTRATOR.
4. Klik op **Opslaan**.

### <a name="access-for-users-of-classic-web-services"></a>Toegang voor gebruikers van klassieke webservices

Een werkruimte beheren:

Aanmelden bij de [Azure klassieke portal](https://manage.windowsazure.com/) met uw account Microsoft Azure - account die is gekoppeld aan het abonnement Azure gebruiken.

1. Klik in het deelvenster services Microsoft Azure **MACHINE LEARNING**.
1. Klik op de werkruimte die u wilt beheren.
1. Klik op het tabblad **configureren** .

U kunt via het tabblad configuratie, toegang tot de werkruimte Machine Learning uitstellen door te klikken op **weigeren**. Gebruikers langer niet de werkruimte openen in Machine Learning Studio. Als u access herstellen, klikt u op **toestaan**.

Aan specifieke gebruikers:

Extra om accounts te beheren die toegang tot de werkruimte in de Studio van Machine Learning hebben, klikt u op **aanmelden bij ML Studio** op het tabblad **DASHBOARD** . Hiermee opent u de werkruimte in de Studio van Machine Learning. Vanaf hier, klikt u op het tabblad **Instellingen** en **gebruikers**. U kunt **Meer gebruikers UITNODIGEN** om gebruikers toegang geven tot de werkruimte, of Selecteer een gebruiker en klik op **verwijderen**klikken.

> [AZURE.NOTE] De koppeling **aanmelden bij ML Studio** opent Machine Learning Studio met behulp van het Microsoft-Account u momenteel bent aangemeld. Het Microsoft-Account waarmee u aan te melden de klassieke Azure portal voor het maken van een werkruimte is niet automatisch gemachtigd om die werkruimte te openen. Om een werkruimte opent, u moet zijn aangemeld bij het Microsoft-Account die is gedefinieerd als de eigenaar van de werkruimte, of moet u een uitnodiging ontvangen van de eigenaar te nemen aan de werkruimte.
