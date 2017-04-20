<properties
    pageTitle="Optimalisatie van uw omgeving met de Service Fabric-oplossing in logboek Analytics | Microsoft Azure"
    description="U kunt de Service Fabric-oplossing voor de beoordeling van de risico's en de gezondheid van uw Fabric Service toepassingen, micro-services, knooppunten en clusters."
    services="log-analytics"
    documentationCenter=""
    authors="niniikhena"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="nini"/>



# <a name="service-fabric-solution-in-log-analytics"></a>Fabric-oplossing in logboek Analytics service

> [AZURE.SELECTOR]
- [Resource Manager](log-analytics-service-fabric-azure-resource-manager.md)
- [PowerShell](log-analytics-service-fabric.md)

In dit artikel wordt beschreven hoe de Service Fabric-oplossing in logboek Analytics gebruiken om u te kunnen identificeren en oplossen van problemen in uw cluster Service Fabric.

De Service Fabric-oplossing gebruikt Azure diagnostische gegevens van uw Service Fabric VMs, door het verzamelen van deze gegevens uit uw tabellen Azure af. Logboek Analytics leest vervolgens Fabric Service framework gebeurtenissen, met inbegrip van **Betrouwbare servicegebeurtenissen**, **Gebeurtenissen Actor** **Operationele gebeurtenissen**en **ETW aangepaste gebeurtenissen**. Met het dashboard oplossing bent u belangrijke kwesties en relevante gebeurtenissen bekijken in uw omgeving Fabric-Service.

Om te beginnen met de oplossing, moet u verbinding maken met uw Service Fabric '-cluster een logboek Analytics werkruimte. Hier volgen drie scenario's te overwegen:

1. Gebruik de stappen in ***een Fabric '-Cluster Service implementeren die zijn verbonden met een werkruimte Analytics logboek*** als u niet uw Service Fabric '-cluster hebt geïmplementeerd, een nieuw cluster implementeren en dit programma hebben geconfigureerd op rapport op logboek Analytics.

2. Volg de stappen in als u nodig hebt voor het verzamelen van prestatiemeteritems van de hosts gebruiken andere OMS oplossingen, zoals beveiliging op uw Fabric '-Cluster Service, ***implementeren een Fabric '-Cluster-Service die is verbonden met een werkruimte OMS VM extensie geïnstalleerd.***

3. Als u uw Service Fabric '-cluster en u wilt verbinden met een Log Analytics al hebt geïmplementeerd, volgt u de stappen in ***een bestaande opslag account toe te voegen aan logboek Analytics.***


##<a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a>Implementatie van een Service Fabric '-Cluster verbonden met een werkruimte logboek Analytics.
Deze sjabloon biedt het volgende:


1. Een Azure Service Fabric '-cluster al verbonden aan een werkruimte logboek Analytics implementeert. U hebt de optie een nieuwe werkruimte maken tijdens het implementeren van de sjabloon of invoer van de naam van een bestaand logboek Analytics werkruimte.
2. De diagnostische opslag account toegevoegd aan de werkruimte logboek Analytics.
3. Hiermee kunt de Service Fabric-oplossing in uw werkruimte logboek Analytics.

[![Implementeren op Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)


Zodra u de knop deploy selecteert, komt u op de Azure portal met de parameters die u kunt bewerken. Zorg ervoor dat u een nieuwe bronnengroep maken wanneer u een nieuw logboekbestand Analytics werkruimte naam: ![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service-Fabric](./media/log-analytics-service-fabric/3.png)

Accepteer de juridische voorwaarden en klik op 'Create' om de installatie te starten. Zodra de installatie is voltooid, ziet u de nieuwe werkruimte en cluster gemaakt en de WADServiceFabric * gebeurtenis, WADWindowsEventLogs en WADETWEvent tabellen toegevoegd:

![Service-Fabric](./media/log-analytics-service-fabric/4.png)

##<a name="deploy-a-service-fabric-cluster-connected-to-an-oms-workspace-with-vm-extension-installed"></a>Implementatie van een Service Fabric '-Cluster verbonden met een werkruimte OMS VM extensie geïnstalleerd.
Deze sjabloon biedt het volgende:

1. Een Azure Service Fabric '-cluster al verbonden aan een werkruimte logboek Analytics implementeert. U kunt een nieuwe werkruimte maken of een bestaande gebruiken.
2. Voegt u de diagnostische opslag aan de werkruimte logboek Analytics.
3. Hiermee kunt de Service Fabric-oplossing in de werkruimte logboek Analytics.
4. De uitbreiding van de agent MMA in elke VM schaal instellen in uw Service Fabric '-cluster geïnstalleerd. Met de MMA agent is geïnstalleerd, bent u prestatiegegevens over de knooppunten weergeven.


[![Implementeren op Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)


De stappen hierboven, de benodigde parameters invoeren en ere van een implementatie. Nogmaals ziet u de nieuwe werkruimte, cluster en alle gemaakte af-tabellen:

![Service-Fabric](./media/log-analytics-service-fabric/5.png)

###<a name="viewing-performance-data"></a>Prestatiegegevens weergeven

Perf om gegevens te bekijken van de knooppunten:
</br>
- De werkruimte logboek Analytics vanaf de portal Azure starten.

![Service-Fabric](./media/log-analytics-service-fabric/6.png)

- Ga naar instellingen in het linkerdeelvenster en selecteer gegevens >> Windows-prestatiemeteritems >> 'De geselecteerde items toevoegen': ![Service Fabric](./media/log-analytics-service-fabric/7.png)

- Met de volgende query's in de zoekfunctie, dieper ingaan op de belangrijke gegevens over de knooppunten:
</br>

    een. De gemiddelde CPU-gebruik op alle knooppunten in de in het afgelopen uur één vergelijken om te zien welke knooppunten ondervindt problemen en met welk tijdsinterval een knooppunt een piek had:

    ``` Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR. ```

    ![Service-Fabric](./media/log-analytics-service-fabric/10.png)


    b. Soortgelijke lijndiagrammen voor het beschikbare geheugen op elk knooppunt met deze query bekijken:

    ```Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.```

    Gebruik deze query om een lijst met alle knooppunten, de exacte waarde van de gemiddelde voor beschikbare megabytes (MB) weergegeven voor elk knooppunt weer te geven:

    ```Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer ```

    ![Service-Fabric](./media/log-analytics-service-fabric/11.png)


    c. In het geval dat u inzoomen op een bepaald knooppunt wilt door het onderzoeken van de per uur, gemiddelde, minimum, maximum en 75 percentiel CPU-gebruik, kun je je dit doen met behulp van deze query (vervang veld Computer):

    ```Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR```

    ![Service-Fabric](./media/log-analytics-service-fabric/12.png)

    Meer informatie over prestatiegegevens in een logboekbestand Analytics [hier]. (https://blogs.technet.microsoft.com/msoms/tag/metrics/)


##<a name="adding-an-existing-storage-account-to-log-analytics"></a>Een bestaande account voor opslag toe te voegen aan Analytics logboek

Deze sjabloon wordt gewoon uw bestaande accounts voor opslag toegevoegd aan een nieuw of bestaand logboek Analytics werkruimte.
</br>

[![Implementeren op Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

>[AZURE.NOTE] Selecteer "Gebruik bestaande" en zoek naar de bronnengroep met de werkruimte OMS in een resourcegroep selecteren als u met een bestaand logboekbestand Analytics werkruimte werkt. Maak een nieuwe als een anders.
![Service-Fabric](./media/log-analytics-service-fabric/8.png)

Nadat u deze sjabloon is geïmplementeerd, kunt u zijn te zien van de opslag-account verbonden met uw werkruimte logboek Analytics. In dit geval ik één meer opslag account toegevoegd aan de Exchange-werkruimte boven maken.
![Service-Fabric](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Service Fabric-gebeurtenissen weergeven

Zodra de implementaties zijn voltooid en de Service Fabric-oplossing in uw werkruimte is ingeschakeld, selecteert u de tegel **Fabric Service** in het logboek Analytics portal aan het dashboard Fabric-Service starten. Het dashboard bestaat uit de kolommen in de volgende tabel. Elke kolom bevat de bovenste tien gebeurtenissen per aantal overeenkomen met de criteria van de kolom voor het opgegeven bereik. U kunt een zoekfunctie waarmee u de hele lijst door te klikken op **alle** onderaan rechts van elke kolom of door te klikken op de kolomkop kunt uitvoeren.

| **Service Fabric, gebeurtenis** | **Beschrijving** |
| --- | --- |
| Belangrijke kwesties | De weergave van kwesties zoals RunAsyncFailures RunAsynCancellations en keuzelijsten knooppunt. |
| Operationele gebeurtenissen | Belangrijke operationele gebeurtenissen zoals toepassing upgrade en implementaties. |
| Gebeurtenissen van betrouwbare Service | Betrouwbare service opvallende gebeurtenissen die een Runasyncinvocations. |
| Actor-gebeurtenissen | Acteur opvallende gebeurtenissen die worden gegenereerd door de micro-services, zoals de uitzonderingen die worden veroorzaakt door een actor-methode, actor activeringen en deactivations. |
| Application-gebeurtenissen | Alle aangepaste ETW gebeurtenissen die worden gegenereerd door uw toepassingen. |

![Dashboard service Fabric](./media/log-analytics-service-fabric/sf3.png)

![Dashboard service Fabric](./media/log-analytics-service-fabric/sf4.png)


In de volgende tabel bevat de methoden van de collectie gegevens en andere details over de manier waarop gegevens worden verzameld voor Service Fabric.

| platform | Directe Agent | SCOM agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | frequentie van verzameling |
|---|---|---|---|---|---|---|
|Windows|![Nee](./media/log-analytics-malware/oms-bullet-red.png)|![Nee](./media/log-analytics-malware/oms-bullet-red.png)| ![Ja](./media/log-analytics-malware/oms-bullet-green.png)|            ![Nee](./media/log-analytics-malware/oms-bullet-red.png)|![Nee](./media/log-analytics-malware/oms-bullet-red.png)|10 minuten |


>[AZURE.NOTE] Door te klikken op de **gegevens op basis van de laatste 7 dagen** aan de bovenkant van het dashboard kunt u het bereik van deze gebeurtenissen in de Service Fabric-oplossing. U kunt ook gebeurtenissen die worden gegenereerd in de laatste 7 dagen, 1 dag of zes uren weergeven. Of u kunt **aangepaste** geeft u een aangepast datumbereik selecteren.


## <a name="next-steps"></a>Volgende stappen

- Met [Zoekacties in Analytics logboek logboek](log-analytics-log-searches.md) kunt u gedetailleerde gegevens Fabric-Service.
