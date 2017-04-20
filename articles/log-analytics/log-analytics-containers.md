<properties
    pageTitle="Oplossing voor containers in logboek Analytics | Microsoft Azure"
    description="De oplossing van de Containers in logboek Analytics helpt u bij het weergeven en beheren van uw Docker container hosts op één locatie."
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
    ms.date="10/10/2016"
    ms.author="banders"/>



# <a name="containers-preview-solution-log-analytics"></a>Containers (voorbeeld) oplossing logboek Analytics

Dit artikel wordt beschreven hoe u kunt instellen en gebruiken van de Containers oplossing in logboek Analytics waarmee u kunt bekijken en uw hosts Docker container op een centrale locatie beheren. Docker is een software virtualisatie-systeem gebruikt voor het maken van de containers die de implementatie van software om hun IT-infrastructuur te automatiseren.

Met de oplossing, kunt u zien welke containers op de container hosts worden uitgevoerd en welke afbeeldingen in de containers worden uitgevoerd. U kunt de opdrachten voor de containers waarin gedetailleerde controle-informatie weergeven. En als u containers weer te geven en gecentraliseerde logboeken zoeken Docker hosts op afstand bekijken zonder problemen. Hier vindt u containers die mogelijk ruis en beslag overtollige middelen op een host. En u kunt bekijken, gecentraliseerde CPU, geheugen, opslag en gebruik en de prestaties gegevens voor containers.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing

Gebruik de volgende informatie te installeren en configureren van de oplossing.

De Containers-oplossing toevoegen aan uw Kantoorbeheersysteem werkruimte met behulp van het proces beschreven in [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).

Er zijn twee manieren om te installeren en gebruiken van Docker met OMS:

- Op ondersteunde besturingssystemen van Linux installeren en uitvoeren van Docker installeren en OMS-Agent configureren voor Linux
- CoreOS, installeren en uitvoeren van Docker en configureert u de OMSAgent uit te voeren binnen een container

Bekijk de ondersteunde Docker en Linux besturingssystemen voor uw host container op [GitHub](https://github.com/Microsoft/OMS-docker).

>[AZURE.IMPORTANT] Docker moet zijn uitgevoerd **voordat** u de [OMS Agent voor Linux](log-analytics-linux-agents.md) op uw container hosts installeren. Als u de agent voor de installatie van Docker al hebt geïnstalleerd, moet u opnieuw installeren van de Agent OMS voor Linux. Zie de [website van Docker](https://www.docker.com)voor meer informatie over Docker.

U moet de volgende instellingen geconfigureerd op de hosts container voordat u containers kunt controleren.

## <a name="configure-settings-for-the-linux-container-host"></a>Instellingen voor de container Linux host configureren

Nadat u Docker hebt geïnstalleerd, gebruik de volgende instellingen voor de container-host de agent voor gebruik met Docker configureren. Deze configuratiemethode ondersteunt geen CoreOS.

### <a name="to-configure-settings-for-the-container-host---systemd-suse-opensuse-centos-7x-rhel-7x-and-ubuntu-15x-and-higher"></a>Instellingen configureren voor de container host - systemd (SUSE, openSUSE, CentOS 7.x, RHEL 7.x en Ubuntu 15.x en hoger)

1. Docker.service om toe te voegen de volgende bewerken:

    ```
    [Service]
    ...
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ...
    ```

2. Toevoegen van $DOCKER\_KIEST &quot;ExecStart = / usr/bin/docker daemon&quot; in het bestand docker.service. In het volgende voorbeeld gebruikt.

    ```
    [Service]
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ExecStart=/usr/bin/docker daemon -H fd:// $DOCKER_OPTS
    ```

3. Start de service Docker. Bijvoorbeeld:

    ```
    sudo systemctl restart docker.service
    ```

### <a name="to-configure-settings-for-the-container-host---upstart-ubuntu-14x"></a>Voor het configureren van instellingen voor de host container - Upstart (Ubuntu 14.x)

1. /Etc/default/docker bewerken en voeg het volgende toe:

    ```
    DOCKER_OPTS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Sla het bestand op en start de services Docker en OMS.

    ```
    sudo service docker restart
    ```

### <a name="to-configure-settings-for-the-container-host---amazon-linux"></a>Voor het configureren van instellingen voor de host container - Amazon Linux

1. /Etc/sysconfig/docker bewerken en voeg het volgende toe:

    ```
    OPTIONS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Sla het bestand op en start de service Docker.

    ```
    sudo service docker restart
    ```

## <a name="configure-settings-for-coreos-containers"></a>Configureren van instellingen voor CoreOS containers

Nadat u Docker hebt geïnstalleerd, gebruikt u de volgende instellingen voor CoreOS Docker uitvoeren en het maken van een container. U kunt een ondersteunde versie van Linux, met inbegrip van CoreOS, met deze configuratie. U moet uw [Kantoorbeheersysteem werkruimte-ID en sleutel](log-analytics-linux-agents.md).

### <a name="to-use-oms-for-all-containers-with-coreos"></a>OMS gebruiken voor alle containers met CoreOS

- Start de OMS-container die u wilt controleren. Wijzigen en gebruik het volgende voorbeeld.

  ```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25224:25224/udp -p 127.0.0.1:25225:25225 --name="omsagent" --log-driver=none --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-agent-to-one-in-a-container"></a>Overschakelen van het gebruik van een agent is geïnstalleerd op een in een container

Als u eerder hebt gebruikt de agent direct geïnstalleerd en wilt gebruiken in plaats daarvan een agent wordt uitgevoerd in een container, moet u eerst de OMSAgent verwijderen. Zie de [stappen voor het installeren van de Agent OMS voor Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md).

## <a name="containers-data-collection-details"></a>Containers-collectie detailgegevens

De Containers oplossing verzamelt verschillende maatstaven en logboek prestatiegegevens van container-hosts en containers met de agenten OMS voor Linux die u hebt ingeschakeld en OMSAgent uitgevoerd in containers.

De volgende tabel bevat methoden van de collectie gegevens en andere informatie over de manier waarop gegevens worden verzameld voor Containers.

| platform | OMS Agent voor Linux | SCOM agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | frequentie van verzameling |
|---|---|---|---|---|---|---|
|Linux|![Ja](./media/log-analytics-containers/oms-bullet-green.png)|![Nee](./media/log-analytics-containers/oms-bullet-red.png)|![Nee](./media/log-analytics-containers/oms-bullet-red.png)|            ![Nee](./media/log-analytics-containers/oms-bullet-red.png)|![Nee](./media/log-analytics-containers/oms-bullet-red.png)| elke 3 minuten|


De volgende tabel ziet u voorbeelden van gegevenstypen zijn verzameld door de oplossing van Containers:

| Gegevenstype | Velden |
| --- | --- |
| Prestaties voor hosts en containers | Computer, objectnaam, CounterName & #40; % processortijd, schijf leest MB schijf schrijft MB, MB geheugen gebruik, netwerk ontvangen Bytes, netwerk verzonden Bytes, de Processor gebruik sec, netwerk & #41; tegenwaarde, TimeGenerated, itempad, SourceSystem |
| Container voorraad | TimeGenerated, de Computer, de naam van de container, ContainerHostname, Image, ImageTag, ContinerState, ExitCode, EnvironmentVar, opdracht, CreatedTime, StartedTime, FinishedTime, SourceSystem, IdContainer, ImageID |
| Container afbeelding voorraad | TimeGenerated, Computer, afbeelding, ImageTag, ImageSize, VirtualSize, actief, onderbroken, stopgezet, is mislukt, SourceSystem, ImageID, TotalContainer |
| Container aanmelden | TimeGenerated, Computer, afbeeldings-ID, naam van de container, LogEntrySource, LogEntry, SourceSystem, IdContainer |
| Container-logboek | TimeGenerated, Computer, TimeOfCommand, Image, opdracht, SourceSystem, IdContainer |

## <a name="monitor-containers"></a>Monitor containers

Nadat u de oplossing in de portal OMS ingeschakeld hebt, ziet u de **Containers** naast elkaar weergegeven samenvattingsgegevens over uw container-hosts en de containers met hosts.

![Containers naast elkaar](./media/log-analytics-containers/containers-title.png)

De tegel geeft een overzicht van het aantal containers die u hebt in het milieu en of ze nu is mislukt, uitgevoerd of is gestopt.

### <a name="using-the-containers-dashboard"></a>Met behulp van het dashboard Containers

Klik op de tegel **Containers** . Daar ziet u de weergaven die worden georganiseerd door:

- Container-gebeurtenissen
- Fouten
- Status van containers
- Container afbeelding voorraad
- Prestaties van CPU en geheugen

Elk deelvenster in het dashboard is een visuele weergave van een zoekactie die wordt uitgevoerd op de verzamelde gegevens.

![Dashboard containers](./media/log-analytics-containers/containers-dash01.png)

![Dashboard containers](./media/log-analytics-containers/containers-dash02.png)

In de **Container Status** blade, klik op bovenste gebied, zoals hieronder wordt weergegeven.

![Status van containers](./media/log-analytics-containers/containers-status.png)

Logboek zoeken met informatie over de hosts en uitgevoerd in deze containers wordt geopend.

![Zoekfunctie voor containers](./media/log-analytics-containers/containers-log-search.png)

Hier kunt u de query wilt wijzigen om te zoeken naar specifieke informatie waarin je geïnteresseerd bent. Voor meer informatie over het logboek zoeken Zie [zoekacties in Analytics logboek logboek](log-analytics-log-searches.md).

U kunt bijvoorbeeld de query aanpassen zodat gestopt containers in plaats van de actieve containers wordt door te wijzigen in **actief** **gestopt** in de query.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Problemen oplossen door te zoeken naar een container is mislukt

OMS geeft een container **mislukt** als deze is afgesloten met een niet-nul afsluitcode. U kunt een overzicht van de fouten en tekortkomingen in het milieu in de blade **Containers is mislukt** .

### <a name="to-find-failed-containers"></a>Kunnen geen containers te vinden.

1. Klik op de **Container gebeurtenissen** blade.  
  ![containers-gebeurtenissen](./media/log-analytics-containers/containers-events.png)
2. Logboek zoeken met de status van de containers met de volgende strekking wordt geopend.  
  ![containers staat](./media/log-analytics-containers/containers-container-state.png)
3. Klik vervolgens op de waarde om aanvullende informatie zoals de grootte en aantal gestopt en mislukte afbeeldingen weer te geven. Vouw **meer** om de id van de afbeelding weergeven.  
  ![mislukte containers](./media/log-analytics-containers/containers-state-failed.png)
4. Zoek vervolgens de container die deze installatiekopie is uitgevoerd. Typ het volgende in de query.
  `Type=ContainerInventory <ImageID>`De logboeken worden weergegeven. U kunt schuiven om te zien van de container is mislukt.  
  ![mislukte containers](./media/log-analytics-containers/containers-failed04.png)


## <a name="search-logs-for-container-data"></a>Zoeken in Logboeken voor container-gegevens

Als u een specifieke fout oplossen wilt, kan het helpen om te zien waar deze zich voordoet in uw omgeving. De volgende logboek typen kunt u query's om terug te keren de informatie die dat u wilt maken.

- **ContainerInventory** : Gebruik dit type als u informatie over de locatie van de container wilt, wat hun namen zijn en wat beelden ze uitgevoerd.
- **ContainerImageInventory** : Gebruik dit type als u wilt zoeken naar informatie georganiseerd door afbeelding en afbeelding informatie zoals afbeelding-id's of formaten weergeven.
- **ContainerLog** : Gebruik dit type als u wilt zoeken naar specifieke fout logboek en posten.
- **ContainerServiceLog** : Gebruik dit type als u wilt zoeken naar gegevens voor audittrail voor de daemon Docker zoals start, stop, verwijderen of pull-opdrachten.

### <a name="to-search-logs-for-container-data"></a>Logboeken voor container gegevens zoeken

- Kies een afbeelding waarvan u weet dat onlangs is mislukt en de foutenlogboeken voor het zoeken. Begin met het zoeken naar de containernaam van een met die afbeelding met een **ContainerInventory** zoeken. Bijvoorbeeld: zoeken`Type=ContainerInventory ubuntu Failed`  
    ![Zoeken naar Ubuntu containers](./media/log-analytics-containers/search-ubuntu.png)

  Noteer de naam van de container naast **de naam**en zoek naar deze logboeken. In dit voorbeeld is `Type=ContainerLog adoring_meitner`.

**Informatie over prestaties weergeven**

Wanneer u begint bent met het maken van query's, kan het helpen om te zien wat eerst mogelijk is. Probeer bijvoorbeeld een uitgebreide query alle als prestatiegegevens wilt zien, typt u de volgende query.

```
Type=Perf
```

![prestaties van containers](./media/log-analytics-containers/containers-perf01.png)



Ziet u dit in een meer grafische vorm wanneer u klikt op het woord **parameters** in de resultaten.

![prestaties van containers](./media/log-analytics-containers/containers-perf02.png)



U kunt het bereik van de prestatiegegevens speld voor een specifieke container door de naam van het aan de rechterkant van de query te typen.

```
Type=Perf <containerName>
```

Dat ziet u de lijst van prestatiegegevens die zijn verzameld voor een afzonderlijke container.

![prestaties van containers](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Voorbeeld logbestand zoekquery 's

Het is vaak nuttig voor het samenstellen van query's beginnen met een voorbeeld of twee en vervolgens aanpassen aan uw omgeving. Als uitgangspunt, kunt u experimenteren met het blad **Opvallende query's** waarmee u kunt uitgebreidere query's maken.

![Containers-query 's](./media/log-analytics-containers/containers-queries.png)

## <a name="saving-log-search-queries"></a>Zoekquery's opslaan van logboek

Query's opslaan is een standaardfunctie in logboek Analytics. Als u deze opslaat, hebt u die u hebt gevonden nuttig bij de hand voor toekomstig gebruik.

Nadat u een query die nuttig voor u maken, door te klikken op **Favorieten** boven aan de pagina logboek opslaan. Vervolgens kan u gemakkelijk het daarna openen vanaf de pagina **Mijn Dashboard** .

## <a name="next-steps"></a>Volgende stappen

- [Zoeken in Logboeken](log-analytics-log-searches.md) om gedetailleerde container gegevensrecords te bekijken.
