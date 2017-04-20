<properties
    pageTitle="Een Machine Learning werkruimte beheren | Microsoft Azure"
    description="Toegang tot de werkruimten Azure Machine Learning, beheren en implementeren en beheren van webservices API ML"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="manage-an-azure-machine-learning-workspace"></a>Een werkruimte Azure Machine Learning beheren

>[AZURE.NOTE] De procedures in dit artikel zijn relevant voor de Azure Machine Learning klassieke webservices. Zie voor informatie over het beheren van webservices in de portal Machine Learning Web Services, [een webservice met behulp van de portal Azure Machine Learning Web-Services beheren](machine-learning-manage-new-webservice.md).

De klassieke Azure portal kunt u uw werkruimten Machine Learning te beheren:

- Controleren hoe de werkruimte wordt gebruikt
- Configureren van de werkruimte wilt toestaan of weigeren van toegang
- Webservices die zijn gemaakt in de werkruimte beheren
- De werkruimte verwijderen

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Het tabblad dashboard biedt bovendien een overzicht van het gebruik van uw werkruimte en een overzicht van de werkruimtegegevens van uw.  

> [AZURE.TIP] In Azure Machine Learning Studio op het tabblad **WEB SERVICES** kunt u toevoegen, bijwerken of verwijderen van een webservice Machine Learning.

Een werkruimte beheren:

1.  Aanmelden bij de [Azure klassieke portal](https://manage.windowsazure.com/) met uw account Microsoft Azure - account die is gekoppeld aan het abonnement Azure gebruiken.
2.  Klik in het deelvenster services Microsoft Azure **MACHINE LEARNING**.
3.  Klik op de werkruimte die u wilt beheren.

De pagina van de werkruimte bevat drie tabbladen:

- **DASHBOARD** - kunt u voor het gebruik van de werkruimte weergeven en informatie
- **Configureren** - kunt u toegang tot de werkruimte beheren
- **WEB SERVICES** - kunt u Web-services die zijn gepubliceerd vanuit deze werkruimte beheren

## <a name="to-monitor-how-the-workspace-is-being-used"></a>Om te controleren hoe de werkruimte wordt gebruikt

Klik op het tabblad **DASHBOARD** .

U kunt van het dashboard, algemene gebruik van uw werkruimte weergeven en een kijkje van de werkruimte.

- De grafiek **berekenen** weergegeven compute resources wordt gebruikt door de werkruimte. U kunt de weergave weer te geven de relatieve of absolute waarden wijzigen en kunt u de periode in de grafiek weergegeven.
- **Overzicht gebruik** weergegeven Azure opslag wordt gebruikt door de werkruimte.
- **Overzicht** biedt een overzicht van de werkruimte-informatie en nuttige links.

> [AZURE.NOTE] De koppeling **aanmelden bij ML Studio** opent Machine Learning Studio met behulp van het Microsoft-Account u momenteel bent aangemeld. Het Microsoft-Account waarmee u aan te melden de klassieke Azure portal voor het maken van een werkruimte is niet automatisch gemachtigd om die werkruimte te openen. Om een werkruimte opent, u moet zijn aangemeld bij het Microsoft-Account die is gedefinieerd als de eigenaar van de werkruimte, of moet u een uitnodiging ontvangen van de eigenaar te nemen aan de werkruimte.


## <a name="to-grant-or-suspend-access-for-users"></a>Verlenen of toegang voor gebruikers te onderbreken ##

Klik op het tabblad **configureren** .

U kunt op het tabblad configuratie:

- Toegang tot de werkruimte Trainingen Machine onderbreken door te klikken op weigeren. Gebruikers langer niet de werkruimte openen in Machine Learning Studio. Als u access herstellen, klikt u op toestaan.

Extra om accounts te beheren die toegang tot de werkruimte in de Studio van Machine Learning hebben, klikt u op **aanmelden bij ML Studio** op het tabblad **DASHBOARD** (Zie de voorgaande opmerking over **aanmelden bij Studio ML**). Hiermee opent u de werkruimte in de Studio van Machine Learning. Vanaf hier, klikt u op het tabblad **Instellingen** en **gebruikers**. U kunt **Meer gebruikers UITNODIGEN** om gebruikers toegang geven tot de werkruimte, of Selecteer een gebruiker en klik op **verwijderen**klikken.


## <a name="to-manage-web-services-in-this-workspace"></a>Webservices in deze werkruimte beheren

Klik op het tabblad **WEB SERVICES** .

Er verschijnt een lijst van webservices is gepubliceerd vanuit deze werkruimte.
Klik op de naam in de lijst te openen van de webpagina van de service voor het beheren van een webservice.

Een webservice is mogelijk een of meer eindpunten worden gedefinieerd.

- U kunt meer eindpunten, naast de 'Standaard' eindpunt definiëren. Klik op **Eindpunten beheren** onder aan het dashboard de webservices van Azure Machine Learning portal openen als u het eindpunt.

- Verwijderen van een eindpunt (u kunt het eindpunt 'Standaard' niet verwijderen), klik op het selectievakje aan het begin van de rij van het eindpunt en klik op **verwijderen**. Hiermee verwijdert u het eindpunt van de webservice.

    > [AZURE.NOTE] Als een toepassing de web-service-eindpunt wanneer het eindpunt wordt verwijderd, ontvangt de toepassing een fout wanneer die wordt geprobeerd toegang te krijgen tot de service.

Klik op de naam van een Web-service-eindpunt om het te openen. 

Van het dashboard, kunt u algemene gebruik van de webservice in een bepaalde tijdsperiode weergeven. Kunt u de periode in het vervolgmenu van de periode in de rechterbovenhoek van het gebruik van grafieken. Het dashboard ziet u de volgende informatie:

- **Aanvragen verloop van tijd** wordt een stap grafiek van het aantal aanvragen in de geselecteerde periode weergegeven. Kunt identificeren als er pieken in gebruik.
- **Aanvragen van de verzoek-antwoord** geeft de totale verzoek-antwoord oproepen die de service heeft ontvangen over de geselecteerde periode en hoeveel ze is mislukt.
- **Gemiddelde aanvraag berekenen reactietijd van** het gemiddelde weergegeven van de tijd die nodig is voor het uitvoeren van de ontvangen verzoeken.
- **Batch aanvragen** geeft het totale aantal partij verzoeken de service gedurende de geselecteerde tijdsperiode en hoeveel ze niet heeft ontvangen.
- **Gemiddelde latentie van de functie** weergegeven gemiddelde van de tijd die nodig is voor het uitvoeren van de ontvangen verzoeken.
- **Fouten** geeft het totale aantal fouten dat zich heeft voorgedaan op het aanroepen van de webservice.
- **Services-kosten** weergegeven de kosten voor de facturering plan dat is gekoppeld aan de service.

U kunt de volgende eigenschappen bijwerken vanuit de pagina configureren:

* **Omschrijving** kunt u een omschrijving invoeren voor de webservice. Omschrijving is een verplicht veld.
* **Logboekregistratie** kunt u in- of uitschakelen van de foutregistratie op het eindpunt. Voor meer informatie over logboekregistratie Zie Enable [logging voor webservices van Machine Learning](machine-learning-web-services-logging.md).
* **Voorbeeldgegevens inschakelen** kunt u voorbeeldgegevens die u gebruiken kunt voor het testen van de verzoek-antwoord-service bieden. Als u de webservice in Studio van Machine Learning gemaakt, is de voorbeeldgegevens genomen uit de gegevens die uw gebruikte trainen uw model. Als u de service via programmacode hebt gemaakt, worden de gegevens worden opgehaald uit de voorbeeldgegevens die u als onderdeel van het pakket JSON verstrekt.

[consume]: machine-learning-consume-web-services.md
[marketplace]: machine-learning-publish-web-service-to-azure-marketplace.md
