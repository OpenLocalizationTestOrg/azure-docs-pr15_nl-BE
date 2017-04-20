<properties
    pageTitle="3D Hubs met Analytics logboek controleren | Microsoft Azure"
    description="De oplossing oppervlak Hub gebruiken voor het bijhouden van de gezondheid van de Hubs oppervlak en begrijpen hoe ze worden gebruikt."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="monitor-surface-hubs-with-log-analytics"></a>Oppervlak Hubs met logboek Analytics monitor

In dit artikel wordt beschreven hoe u kunt de oplossing oppervlak Hub in logboek Analytics Microsoft Surface Hub-apparaten met Microsoft Operations Management Suite (OMS) controleren. Logboek Analytics helpt u bij het bijhouden van de gezondheid van uw oppervlak Hubs goed begrijpen hoe ze worden gebruikt.

Elke Hub oppervlak heeft Microsoft Monitoring Agent geïnstalleerd. De door de agent u gegevens kunt verzenden vanaf uw Hub-oppervlak aan OMS. Logboekbestanden worden gelezen uit het oppervlak Hubs en worden vervolgens naar de OMS-service worden verzonden. Factoren, zoals servers die off line is, wordt de agenda niet synchroniseren, of als de rekening van het apparaat kan niet aan te melden bij Skype in OMS in het dashboard oppervlak Hub worden weergegeven. Met behulp van de gegevens in het dashboard, kunt u de apparaten die niet worden uitgevoerd of die andere problemen zijn en potentieel gelden correcties voor de gedetecteerde problemen identificeren.


## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing

Gebruik de volgende informatie te installeren en configureren van de oplossing. Om het oppervlak Hubs van Microsoft Operations Management Suite (OMS) beheren, hebt u het volgende nodig:

- Een geldig abonnement op [OMS](http://www.microsoft.com/oms).
- Een abonnement voor [OMS](https://azure.microsoft.com/pricing/details/log-analytics/) die ondersteuning voor het aantal apparaten bieden dat u wilt controleren. OMS prijsstelling hangt af van hoeveel apparaten zijn ingeschreven en over hoeveel gegevens deze processen. U zult Hiermee rekening bij het plannen van de implementatie van uw Hub oppervlak.

Vervolgens wordt u een OMS-abonnement toevoegen aan uw bestaande Microsoft Azure-abonnement of maak een nieuwe werkruimte rechtstreeks via de portal OMS. Gedetailleerde instructies voor het gebruik van beide methoden is [aan de slag met Analytics logboek](log-analytics-get-started.md). Zodra het abonnement OMS is ingesteld, zijn er twee manieren inschrijven voor uw apparaten oppervlak Hub:

- Automatisch via InTune
- Handmatig via de **Instellingen** op uw apparaat oppervlak.

## <a name="set-up-monitoring"></a>Controle instellen

U kunt de gezondheid en de activiteit van uw Hub-oppervlak met Analytics logboek in OMS controleren. U kunt de Hub oppervlak in OMS inschrijven via InTune of lokaal via **Instellingen** op de Hub oppervlak.

## <a name="connect-surface-hubs-to-oms-through-intune"></a>3D Hubs sluit aan OMS via InTune

U moet de werkruimte-ID en de sleutel van de werkruimte voor de OMS werkruimte waarmee uw Hubs oppervlak. U kunt ophalen uit het Kantoorbeheersysteem portal.

InTune is een Microsoft-product waarmee u de OMS configuratie-instellingen die worden toegepast op een of meer van uw apparaten centraal te beheren. Ga als volgt te werk om uw apparaten via InTune:

1. Aanmelden bij InTune.
2. Ga naar **Instellingen** > **bronnen verbonden**.
3. Maak of bewerk een beleid op basis van de sjabloon oppervlak Hub.
4. Ga naar de sectie OMS (operationele inzichten Azure) van het beleid en de *ID van de werkruimte* en de *Werkruimte sleutel* toevoegen aan het beleid.
5. Sla het beleid.
6. Het beleid koppelen aan de juiste groep van apparaten.

  ![InTune beleid](./media/log-analytics-surface-hubs/intune.png)

De instellingen voor OMS InTune vervolgens gesynchroniseerd met de apparaten in de doelgroep ze in uw werkruimte OMS inschrijven.

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>Verbinding maken met Hubs oppervlak OMS met behulp van de app instellingen

U moet de werkruimte-ID en de sleutel van de werkruimte voor de OMS werkruimte waarmee uw Hubs oppervlak. U kunt ophalen uit het Kantoorbeheersysteem portal.

Als u geen InTune gebruikt voor het beheren van uw omgeving, kunt u apparaten handmatig via de **Instellingen** op elk oppervlak Hub inschrijven:

1. Open de **Instellingen**van uw Hub oppervlak.
2. Voer de referenties van de admin apparaat wanneer dat wordt gevraagd.
3. Klik op **Dit apparaat**, en de **controle**, klik op **Instellingen voor OMS configureren**.
4. Selecteer **controle inschakelen**.
6. Typ de **ID van de werkruimte** in het dialoogvenster Instellingen van OMS en typ de **Sleutel van de werkruimte**.  
  ![Instellingen](./media/log-analytics-surface-hubs/settings.png)
7. Klik op **OK** om de configuratie te voltooien.

Wordt een bevestiging weergegeven dat u al dan niet de OMS-configuratie is toegepast op het apparaat. Als dat het geval is, wordt er een bericht weergegeven dat de agent is verbonden met de OMS-service. Vervolgens start het apparaat verzenden van gegevens naar OMS kunt u weergeven en erop reageren.

## <a name="monitor-surface-hubs"></a>3D Hubs monitor

Toezicht op de Hubs oppervlak lijkt met behulp van OMS op veel andere geregistreerde apparaten controleren.

1. Log in om de OMS-portal.
2. Ga naar het oppervlak Hub oplossing pack dashboard.
3. De gezondheid van uw apparaat wordt weergegeven.

  ![Surface Hub-dashboard](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

U kunt [waarschuwingen](log-analytics-alerts.md) op basis van bestaande of aangepaste zoekacties voor logboek maken. Het Kantoorbeheersysteem van het oppervlak Hubs verzamelt gegevens gebruikt, kunt u zoeken naar problemen en een waarschuwing voor de voorwaarden die u voor uw apparaten definieert.


## <a name="next-steps"></a>Volgende stappen

- Met [zoekacties in Analytics logboek logboek](log-analytics-log-searches.md) kunt u gedetailleerde gegevens van de Hub oppervlak.
- [Waarschuwingen](log-analytics-alerts.md) om u te waarschuwen wanneer er problemen optreden met uw oppervlak Hubs maken.
