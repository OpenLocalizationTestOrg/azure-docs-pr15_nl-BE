<properties
    pageTitle="Inschakelen controle en diagnose in Microsoft Azure | Microsoft Azure "
    description="Informatie over het instellen van diagnostische gegevens voor uw resources in Azure."
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="enable-monitoring-and-diagnostics"></a>Inschakelen, controle en diagnose

In [Azure Portal](https://portal.azure.com)kunt u uitgebreide, frequente, bewaking en diagnostische gegevens over uw resources. U kunt ook de [REST API](https://msdn.microsoft.com/library/azure/dn931932.aspx) of [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) diagnostische gegevens via programmacode te configureren.

Diagnose, bewaking en metrische gegevens in Azure is opgeslagen in opslag-account van uw keuze. Hiermee kunt u welke gereedschappen u wilt de gegevens lezen uit een Opslagverkenner Power BI aan derden tooling gebruiken.

## <a name="when-you-create-a-resource"></a>Bij het maken van een resource

De meeste services kunnen u een diagnoseprogramma inschakelen wanneer u deze in de [Portal Azure maakt](https://portal.azure.com).

1. Ga naar **Nieuw** en kies de bron die u geïnteresseerd bent.

2. Selecteer **optionele configuratie**.
    ![Diagnostische gegevens blade](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)

3. **Diagnostische gegevens**selecteren en klik **op**. U moet de opslag kiezen dat u wilt dat diagnostische gegevens moeten worden opgeslagen. Wordt gebracht normale gegevenssnelheden voor opslag en transacties wanneer u diagnostische gegevens naar een opslag verzendt.

4. Klik op **OK** en de bron maakt.

## <a name="change-settings-for-an-existing-resource"></a>Instellingen wijzigen voor een bestaande bron

Als u een resource hebt gemaakt en u de diagnostische gegevens wijzigen wilt (om het niveau van het verzamelen van gegevens, bijvoorbeeld), doet u dat recht in de Portal Azure.

1. Ga naar de resource en klikt u op de opdracht **Instellingen** .

2. Selecteer **Diagnostics**.

3. De **Diagnostische gegevens** blade heeft alle mogelijke diagnose en controlegegevens collectie voor die resource. U kunt ook **een bewaarbeleid voor het opschonen van uw account voor de opslag van de gegevens** voor bepaalde resources kiezen.
    ![Diagnostische gegevens van opslag](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)

4. Als u de instellingen hebt gekozen, klikt u op de opdracht **Opslaan** . Het duurt een beetje terwijl voor het controleren van gegevens die worden weergegeven als u deze voor de eerste keer inschakelen wilt.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Categorieën van gegevens verzamelen voor virtuele machines
Voor virtuele machines worden alle maatstaven en logboeken vastgelegd met tussenpozen van één minuut, zodat u kunt altijd de meest actuele informatie over uw computer.

- **Fundamentele parameters** : gezondheid statistieken over de virtuele machine zoals processor en geheugen
- **Netwerk- en statistieken** : statistieken over uw netwerkverbindingen en webservices
- **.NET-statistieken** : statistieken over de .NET en ASP.NET-toepassingen uitvoeren op uw virtuele machine
- **SQL-parameters** : als u werkt met Microsoft SQL Service, de prestatiegegevens
- **Windows-gebeurtenislogboeken toepassing** : Windows-gebeurtenissen die worden verzonden naar het kanaal van toepassing
- **Windows-gebeurtenislogboeken systeem** : Windows-gebeurtenissen die worden verzonden naar het systeemkanaal. Dit omvat ook alle gebeurtenissen van [Microsoft Antimalware](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409).
- **Windows-beveiliging gebeurtenislogboeken** : Windows-gebeurtenissen die worden verzonden naar het beveiligingskanaal
- **Diagnostische infrastructuur logs** : over de infrastructuur van de collectie diagnostics Logging
- **IIS-logboeken** : logboeken over de IIS-server

Houd er rekening mee dat op dit moment worden bepaalde distributies van Linux niet ondersteund, en de Gast-Agent moet worden geïnstalleerd op de virtuele machine.

## <a name="next-steps"></a>Volgende stappen

* [Meldingen ontvangen](insights-receive-alert-notifications.md) wanneer operationele gebeurtenissen gebeuren of metrics cross een drempel.
* [Maatstaven voor monitor-service](insights-how-to-customize-monitoring.md) om te controleren of dat de service beschikbaar is en reageert.
* [Aantal exemplaar automatisch geschaald](insights-how-to-scale.md) om te controleren of de schaal van de service op basis van de vraag.
* [Prestaties van de toepassing controleren](../application-insights/app-insights-azure-web-apps.md) als u wilt weten precies hoe uw code uitvoeren in de cloud.
* [Gebeurtenissen bekijken en controleren van Logboeken](insights-debugging-with-events.md) voor meer informatie over alles dat is er gebeurd met uw service.
* [Track service gezondheid](insights-service-health.md) om erachter te komen wanneer Azure heeft ondervonden prestaties afbraak- of -service-onderbrekingen.
