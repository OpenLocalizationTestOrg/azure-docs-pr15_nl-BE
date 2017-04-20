<properties
   pageTitle="Afhandelen van beveiligingsincidenten in Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document helpt u bij het gebruik van Beveiligingscentrum Azure mogelijkheden beveiligingsincidenten."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="handling-security-incident-in-azure-security-center"></a>Afhandelen van beveiligingsincidenten in Azure Security Center 
Gesorteerd en beveiligingswaarschuwingen onderzoeken kunnen tijdrovend voor zelfs de meest ervaren analisten beveiliging en voor een groot deel niet weet waar u moet beginnen is. Beveiligingscentrum kan bieden die één weergave van een campagne aanval en alle verwante signalen via [analytics](security-center-detection-capabilities.md) de gegevens tussen verschillende [beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md)verbinden – u snel inzicht in welke acties de aanvaller heeft genomen en welke bronnen zijn veranderd.

Dit document wordt beschreven hoe u beveiliging alert mogelijkheden in Beveiligingscentrum gebruiken bij het afhandelen van beveiligingsincidenten.


## <a name="what-is-a-security-incident"></a>Wat is een veiligheidsincident?

Een beveiligingsincident is in Security Center, een samenvoeging van alle waarschuwingen voor een bron uitgelijnd met patronen [kill-keten](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) . Incidenten worden weergegeven in de tegel [Beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md) en blade. De lijst met verwante waarschuwingen, waarmee u meer informatie verkrijgen over elk exemplaar onthullen een Incident.

## <a name="managing-security-incidents"></a>Beheer van beveiligingsincidenten

U kunt uw huidige veiligheidsincidenten door te kijken naar de tegel security waarschuwingen bekijken. Toegang tot de Portal Azure en volg de stappen hieronder om meer details over elke veiligheidsincident:

1. Op het dashboard Security Center ziet u de tegel **beveiligingswaarschuwingen** .

    ![Beveiligingswaarschuwingen in Beveiligingscentrum naast elkaar weergeven](./media/security-center-incident/security-center-incident-fig1.png)

2.  Klik op deze tegel uit te vouwen en als een veiligheidsincident wordt gedetecteerd, wordt deze weergegeven bij de security waarschuwingen grafiek zoals hieronder wordt weergegeven:

    ![Security incident](./media/security-center-incident/security-center-incident-fig2.png)

3.  U ziet dat de beschrijving van de security incident een ander pictogram in vergelijking met andere waarschuwingen instellen. Klik op het weergeven van meer informatie over dit incident.

    ![Security incident](./media/security-center-incident/security-center-incident-fig3.png)

4.  Op het **incident** blade u meer ziet gedetailleerde informatie over deze veiligheidsincident, inclusief de volledige beschrijving, de ernst (dat in dit geval hoog), de huidige status (in dit geval is dit nog steeds *actief*, wat betekent dat de gebruiker een actie is niet uitgevoerd om te *sluiten* - dit gebeuren door rechts te klikken op het incident in de **beveiligingswaarschuwingen** blade) , de resource aangevallen (in dit geval *VM1*), de stappen van de oplossing voor het incident en in het onderste deelvenster hebt u de waarschuwingen die zijn opgenomen in dit incident. Als u meer informatie over elke waarschuwing krijgen wilt, wordt Klik op deze en andere blade geopend, zoals hieronder wordt weergegeven:

    ![Security incident](./media/security-center-incident/security-center-incident-fig4.png)

De informatie op deze blade zijn afhankelijk van de waarschuwing. [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) voor meer informatie over het beheren van deze berichten lezen. Enkele belangrijke overwegingen met betrekking tot deze mogelijkheid:

- Een nieuw filter kunt u de weergave met alleen Incident of alleen waarschuwingen kunt aanpassen. 
- De dezelfde waarschuwing kan als onderdeel van een Incident (indien van toepassing), en worden weergegeven als een waarschuwing voor een zelfstandige bestaan. 
- Ontslaan van een incident, wordt de bijbehorende waarschuwingen niet negeren.

## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe het incident mogelijkheid beveiliging in Beveiligingscentrum. Voor meer informatie over de Security Center, raadpleegt u het volgende:

- [Beheren van en het reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md)
- [Detectiemogelijkheden Azure Security Center](security-center-detection-capabilities.md)
- [Beveiligingscentrum Azure Planning en Operations Guide](security-center-planning-and-operations-guide.md)
- [Beheren van en het reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md)
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md)--zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--blog zoeken naar berichten over Azure beveiliging en naleving.
