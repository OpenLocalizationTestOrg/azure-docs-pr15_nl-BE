<properties
    pageTitle="Gebeurtenissen weergeven en controlelogboekbestanden"
    description="Informatie over het bekijken van alle gebeurtenissen die in uw abonnement Azure plaatsvinden."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/28/2015"
    ms.author="robb"/>

# <a name="view-events-and-audit-logs"></a>Gebeurtenissen weergeven en controlelogboekbestanden

Alle bewerkingen die worden uitgevoerd op Azure bronnen worden volledig gecontroleerd door de Azure Resource Manager, van het maken en verwijderen te verlenen of intrekken van toegang. Kunt u deze logboeken in de portal Azure en kun je de [REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx) of de [SDK voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Insights/) via programmering toegang krijgen tot de volledige reeks gebeurtenissen.

## <a name="browse-the-events-impacting-your-azure-subscription"></a>De gebeurtenissen die invloed op uw abonnement Azure bladeren

1. Aanmelden bij de [Azure Portal](https://portal.azure.com/).
2. Klik op **Bladeren** en selecteer **controlelogboeken**.  
    ![Hub bladeren](./media/insights-debugging-with-events/Insights_Browse.png)
3. Dit wordt een bladeserver met alle gebeurtenissen die van invloed zijn geweest op een van uw abonnementen voor de afgelopen 7 dagen geopend. Boven bevindt zich een grafiek met gegevens op het niveau en onder die de volledige lijst met Logboeken is:  ![alle gebeurtenissen](./media/insights-debugging-with-events/Insights_AllEvents.png)

>[AZURE.NOTE] U kunt alleen de 500 meest recente gebeurtenissen voor een bepaald abonnement bekijken in de Azure portal.

4. U kunt klikken op een vermelding voor een overzicht van de gebeurtenissen die bestaat. Bijvoorbeeld, als u iets aan een resourcegroep, kunnen veel verschillende bronnen worden gemaakt of gewijzigd. Voor elke vermelding kunt u het volgende zien:
    * Het **niveau** van de gebeurtenis - bijvoorbeeld, het kan net iets worden om bij te houden (**ter informatie**) of als er iets is een fout opgetreden die u moet weten over (**fout**).
    * **Status** - de definitieve status wordt in het algemeen zijn **geslaagd** of **mislukt**, maar mogelijk ook **geaccepteerd** voor langdurige bewerkingen.
    * *Wanneer* de gebeurtenis is opgetreden.
    * *Die* de bewerking uitgevoerd als iedereen. Niet alle bewerkingen worden uitgevoerd door gebruikers, sommige door back-end services worden uitgevoerd zodat ze een **beller**niet zou hebben.
    * De **Correlatie-ID** van de gebeurtenis - dit is de unieke id voor deze set bewerkingen.

5. Van daaruit gaat u naar de blade details voor een overzicht van de specifieke kenmerken van de gebeurtenis.

    ![Resourcegroepen](./media/insights-debugging-with-events/Insights_EventDetails.png)

    **Mislukte** gebeurtenissen op bevat deze pagina meestal een **Substatus** en een sectie **Eigenschappen** bevatten nuttige informatie voor foutopsporing.

## <a name="filter-to-specific-logs"></a>Filteren op specifieke Logboeken

Om gebeurtenissen die betrekking hebben op een specifieke entiteit, of van een bepaald type wordt weergegeven, kunt u de Audit logs blade filteren door te klikken op de opdracht **Filter** . Ook u de blade Filter wijzigen de **tijd span** van de Audit logs blade.

Als u op deze opdracht klikt, wordt een nieuwe blade geopend:

![Filter](./media/insights-debugging-with-events/Insights_EventFilter.png)

Er zijn vier typen filters:

1. Abonnement
2. Door een **resourcegroep**
3. Door een **brontype**
4. Door een bepaalde **Resource** - moet dit u voorbij in de volledige *Bron-ID* van de resource die u geïnteresseerd bent in

U kunt daarnaast ook gebeurtenissen filteren door die de gebeurtenis uitgevoerd of door het niveau van de gebeurtenis.

Zodra u klaar bent met kiezen wat u wilt zien, klikt u op de knop **bijwerken** onder aan het blad.

## <a name="monitor-events-impacting-specific-resources"></a>Gebeurtenissen controleren specifieke resources beïnvloeden

1. Klik op **Bladeren** om te zoeken naar de bron die u geïnteresseerd bent in. Ook ziet u alle van de logboeken voor een hele **groep**.
2. Schuif naar beneden totdat u de tegel **gebeurtenissen** op van de resource-blade.  
    ![Gebeurtenissen naast elkaar](./media/insights-debugging-with-events/Insights_EventsTile.png)
3. Klik op de steen voor een overzicht van gebeurtenissen die zijn gefilterd, zodat alleen de resources die u hebt geselecteerd. U kunt de opdracht **Filter** meer specifieke filters toepassen of wijzigen van het tijdsbereik.

## <a name="next-steps"></a>Volgende stappen

* [Meldingen ontvangen](insights-receive-alert-notifications.md) wanneer een gebeurtenis plaatsvindt.
* [Maatstaven voor monitor-service](insights-how-to-customize-monitoring.md) om te controleren of dat de service beschikbaar is en reageert.
* [Track service gezondheid](insights-service-health.md) om erachter te komen wanneer Azure heeft ondervonden prestaties afbraak- of -service-onderbrekingen.  
