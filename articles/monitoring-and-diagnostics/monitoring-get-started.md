<properties
    pageTitle="Aan de slag met Azure Monitor | Microsoft Azure"
    description="Aan de slag met Azure Monitor inzicht in de werking van uw middelen en maatregelen op basis van de gegevens."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-azure-monitor"></a>Aan de slag met Azure Monitor

Azure Monitor is de service platform één bron biedt voor het controleren van Azure bronnen. Azure en monitor kunt u visualiseren, opvragen, route, archiveren en statistieken en logboeken die afkomstig zijn van bronnen in Azure en actie ondernemen. U kunt werken met deze gegevens met behulp van de Monitor portal blade, [Monitor PowerShell Cmdlets](./insights-powershell-samples.md), [Cross-Platform CLI](insights-cli-samples.md)of [Azure Monitor REST API's](https://msdn.microsoft.com/library/dn931943.aspx). In dit artikel bekijken we enkele van de belangrijkste onderdelen van Azure Monitor, via de portal voor demonstratie.

1. Ga naar **meer services** en de optie **Beeldscherm** in de portal. Klik op de ster pictogram Deze optie toevoegen aan uw lijst met Favorieten zodat deze altijd gemakkelijk toegankelijk via de linkernavigatiebalk.

    ![In de servicelijst controleren](./media/monitoring-get-started/monitor-more-services.png)

2. Klik op de optie **Monitor** om de bladeserver **Monitor** te openen. Deze blade combineert de controle-instellingen en gegevens in één geconsolideerde weergave. Eerst wordt geopend naar het gedeelte van het **logboek voor faxactiviteit** .

    ![Monitor blade navigatie](./media/monitoring-get-started/monitor-blade-nav.png)

    > [AZURE.WARNING] De **servicemeldingen** en **kennisgeving groepen** bovenstaande opties worden alleen weergegeven voor gebruikers die lid zijn van de particuliere Preview van deze functies.

    Azure Monitor heeft drie basiscategorieën van bewakingsgegevens: het **logboek voor faxactiviteit**, **Statistieken**en **Diagnostische logboeken**.

3. Klik op de **activiteit registreren** om ervoor te zorgen dat de activiteit logboek sectie wordt weergegeven.

    ![Activiteit logboek blade](./media/monitoring-get-started/monitor-act-log-blade.png)

    Het [**logboek voor faxactiviteit**](./monitoring-overview-activity-logs.md) beschrijft alle bewerkingen die worden uitgevoerd op de resources in uw abonnement. Met het logboek voor faxactiviteit, kunt u bepalen de ' wat, wie, en wanneer ' voor elke maken, bijwerken of verwijderen van bewerkingen op de resources in uw abonnement. Bijvoorbeeld, het logboek voor faxactiviteit geeft aan wanneer een web app is gestopt en die is gestopt. Activiteitsgebeurtenissen worden opgeslagen in het platform en de query voor 90 dagen beschikbaar.
   
    Maken en opslaan van query's voor algemene filters, wordt de belangrijkste query's naar een portal dashboard vastmaken zodat u altijd weet als gebeurtenissen die voldoen aan uw criteria hebben plaatsgevonden.

4. De weergave aan een resourcegroep in de laatste week filteren en klik vervolgens op de knop **Opslaan** .

    ![Activiteit logboek query opslaan](./media/monitoring-get-started/monitor-act-log-save.png)

5. Klik nu op de knop **Pin** .

    ![Klik op pincode voor logboek met faxactiviteit](./media/monitoring-get-started/monitor-act-log-pin.png)

    De meeste van de weergaven in dit scenario kan worden vastgemaakt aan een dashboard. Hiermee maakt u één bron van informatie voor operationele gegevens op uw diensten. 

6. Terug naar het dashboard. Nu kunt u zien dat de query (en het aantal resultaten) wordt weergegeven in het dashboard. Dit is handig als u snel wilt bekijken hoogwaardig acties die onlangs in uw abonnement, bv. een nieuwe rol is toegewezen of een VM is verwijderd.

    ![Logboek voor faxactiviteit vastgemaakt aan het dashboard](./media/monitoring-get-started/monitor-act-log-db.png)

7. Terug naar het venster **Monitor** en klik op de sectie **parameters** . U moet eerst een resource selecteren door te filteren en te selecteren met behulp van de vervolgkeuzelijst opties boven aan het blad.

    ![Resource filter voor statistieken](./media/monitoring-get-started/monitor-met-filter.png)

    Alle bronnen van Azure uitstralen [**metrics**](./monitoring-overview-metrics.md). Deze weergave combineert alle parameters in één venster van glas zodat u gemakkelijk kan begrijpen hoe uw resources wilt uitvoeren.

8. Wanneer u een resource hebt geselecteerd, verschijnen alle beschikbare parameters aan de linkerzijde van het blad. U kunt grafiek meerdere parameters in één keer door te selecteren, parameters en het type en bereik voor grafiek wijzigen. U kunt ook alle metrische waarschuwingen instellen voor deze resource bekijken.

    ![Metrische blade](./media/monitoring-get-started/monitor-metric-blade.png)

    > [AZURE.NOTE] Sommige parameters zijn alleen beschikbaar door te schakelen [Toepassing inzichten](../application-insights/app-insights-overview.md) en/of Windows of Linux Azure diagnostische gegevens op de bron.

9. Als u tevreden met uw diagram bent, kunt u de knop **Pin** vastmaken aan uw dashboard.

10. Terug naar de bladeserver **controleren** en klik op **Diagnostische logboeken**.

    ![Diagnoselogboeken blade](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**Diagnostische logboeken**](monitoring-overview-of-diagnostic-logs.md) worden logboeken gegenereerd *door* een resource die gegevens over de werking van die resource. Network Security groep regel items en logica App workflowlogboeken worden bijvoorbeeld beide typen diagnostische logboeken. Deze logboeken kunnen worden opgeslagen in een opslagruimte-account, stroomsgewijs verzonden naar een gebeurtenis Hub en/of verzonden naar [Logboek Analytics](../log-analytics/log-analytics-overview.md). Logboek Analytics is operationele intelligence-product van Microsoft voor Geavanceerd zoeken en waarschuwingen.
   
    U kunt in de portal weergeven en filteren van een lijst van alle bronnen in uw abonnement te identificeren als ze beschikken over diagnostische logboeken ingeschakeld.

11. Klik op een resource in de diagnostische logboeken blade. Als diagnostische logboeken worden opgeslagen in een rekening voor opslag, ziet u een lijst van ieder uur logboekbestanden die u direct kunt downloaden.

    ![Diagnostische logboeken voor één resource](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    U kunt ook klikken op **Diagnostische instellingen**waarmee u kunt instellen of wijzigen van uw instellingen voor archiveren naar een opslag, streaming met Hubs gebeurtenis of het verzenden van een werkruimte logboek Analytics.

    ![Diagnostische logboeken inschakelen](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Als u een diagnostische logboeken logboek Analytics hebt ingesteld, kunt u deze zoeken in de sectie **logboekbestand zoeken** van de portal.

12. Ga naar de sectie **waarschuwingen** van de bladeserver controleren.

    ![blade waarschuwingen voor openbare](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Hier kunt u alle [**waarschuwingen**](./monitoring-overview-alerts.md) beheren op uw Azure bronnen. Dit geldt ook voor waarschuwingen op maatstaven, activiteitsgebeurtenissen (in persoonlijke voorbeeld), inzichten toepassing web tests (locatie) en proactieve diagnostics inzichten van toepassing. Waarschuwingen kunnen resulteren in een e-mailbericht moet worden verzonden of een HTTP POST naar een URL webhook.
   
13. Klik op de **metrische waarschuwing toevoegen** om een waarschuwing maken.

    ![metrische waarschuwing toevoegen](./media/monitoring-get-started/monitor-alerts-add.png)

    Vervolgens kunt u een waarschuwing op het dashboard voor een overzicht van de staat gemakkelijk op elk gewenst moment vastmaken.

14. De Monitor sectie bevat ook koppelingen naar toepassingen [Toepassing inzichten](../application-insights/app-insights-overview.md) en [Logboek Analytics](../log-analytics/log-analytics-overview.md) oplossingen. Deze Microsoft-producten hebben een naadloze integratie met Azure Monitor.

15. Als u geen toepassing inzichten of logboek Analytics, zijn kans dat Azure Monitor een partnerschap met de huidige controle, logboekregistratie en waarschuwingsmethoden producten heeft. Zie onze [partners pagina](./monitoring-partners.md) voor een volledige lijst en instructies voor het integreren.

Door deze stappen te volgen en alle relevante elementen aan een dashboard vastzetten, kunt u uitgebreide weergaven van uw toepassing en de infrastructuur zoals deze:

![Azure Monitor dashboard](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Volgende stappen
- Lees het [overzicht van Azure Monitor](./monitoring-overview.md)
