<properties
    pageTitle="Oplossing in Analytics logboek bijhouden | Microsoft Azure"
    description="U kunt de oplossing configuratie wijzigingen bijhouden in logboek Analytics kunt u gemakkelijk identificeren van software en Windows-Services die worden gewijzigd in uw omgeving, bepalen deze wijzigingen in de configuratie, kunt u de operationele problemen lokaliseren."
    services="operations-management-suite"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operations-management-suite"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="change-tracking-solution-in-log-analytics"></a>Oplossing voor het bijhouden in logboekbestand Analytics wijzigen


U kunt de oplossing configuratie wijzigingen bijhouden in logboek Analytics te gemakkelijk identificeren van software en Services van Windows wijzigingen of Linux daemon die zich in uw omgeving voordoen, bepalen deze wijzigingen in de configuratie, kunt u de operationele problemen lokaliseren.

U installeert de oplossing update de agent die u hebt geïnstalleerd. Wijzigingen in de software is geïnstalleerd en Windows-services op de gecontroleerde servers worden gelezen en vervolgens de gegevens worden verzonden naar het logboek voor Analytics-service in de cloud voor verwerking. Logica wordt toegepast op de gegevens ontvangen en de service cloud gegevens. Wanneer er wijzigingen worden gevonden, worden servers met wijzigingen weergegeven in het dashboard wijzigingen bijhouden. Met behulp van de gegevens op het dashboard bijhouden, kunt u de wijzigingen die zijn aangebracht in uw serverinfrastructuur eenvoudig bekijken.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie te installeren en configureren van de oplossing.

- Operations Manager is vereist voor de oplossing van wijzigingen bijhouden.
- Hebt u een Windows- of Operations Manager-agent op elke computer waar u om wijzigingen te volgen.
- De oplossing voor het bijhouden van wijzigingen toevoegen aan uw Kantoorbeheersysteem werkruimte met behulp van het proces beschreven in [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie nodig.


## <a name="change-tracking-data-collection-details"></a>Gegevens bijhouden van de gegevens wijzigen

Het bijhouden van wijzigingen verzamelt software-inventaris en de metagegevens van Windows-Service met behulp van de agenten die u hebt ingeschakeld.

De volgende tabel bevat methoden van de collectie gegevens en andere informatie over de manier waarop gegevens worden verzameld voor het bijhouden van wijzigingen.

| platform | Directe Agent | SCOM agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | frequentie van verzameling |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Nee](./media/log-analytics-change-tracking/oms-bullet-red.png)|            ![Nee](./media/log-analytics-change-tracking/oms-bullet-red.png)|![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png)| per uur|

## <a name="use-change-tracking"></a>Gebruik de functie Wijzigingen bijhouden

Nadat de oplossing is geïnstalleerd, kunt u het overzicht van wijzigingen voor uw gecontroleerde servers bekijken met behulp van de tegel **Bijhouden** op de pagina **Overzicht** van OMS.

![afbeelding van de tegel wijzigingen bijhouden](./media/log-analytics-change-tracking/oms-changetracking-tile.png)

U kunt wijzigingen in uw infrastructuur en inzoomen in de details voor de volgende categorieën bekijken:

- Wijzigingen per configuratie voor de software en services van Windows
- Softwarewijzigingen in toepassingen en updates voor afzonderlijke servers.
- Totaal aantal softwarewijzigingen voor elke toepassing
- Wijzigingen in Windows-service voor afzonderlijke servers

![afbeelding van het dashboard wijzigingen bijhouden](./media/log-analytics-change-tracking/oms-changetracking01.png)

![afbeelding van het dashboard wijzigingen bijhouden](./media/log-analytics-change-tracking/oms-changetracking02.png)

### <a name="to-view-changes-for-any-change-type"></a>Als u wilt weergeven wijzigen voor een type

1. Klik op de pagina **Overzicht** op de tegel **Wijzigingen bijhouden** .
2. Neem de samenvattingsinformatie in een van de type-blades wijzigen en klik op een om gedetailleerde informatie over het op de pagina **logboek** weergeven op het dashboard **Wijzigen bijhouden** .
3. U kunt de resultaten door de tijd, de gedetailleerde resultaten en de zoekgeschiedenis logboek bekijken op een logboek zoekpagina's. U kunt ook filteren op facetten om de resultaten te beperken.

## <a name="next-steps"></a>Volgende stappen

- Met [zoekacties in Analytics logboek logboek](log-analytics-log-searches.md) kunt u gedetailleerde gegevens bijhouden.
