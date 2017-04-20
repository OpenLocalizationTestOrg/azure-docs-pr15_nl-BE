<properties
   pageTitle="Het inschakelen van toegang van het publiek aan een ACS-app | Microsoft Azure"
   description="Het publiek toegang tot een Service Azure Container."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="timlt"/>

# <a name="enable-public-access-to-an-azure-container-service-application"></a>Publieke toegang te krijgen tot een toepassing Azure Container Service

De DC-en OS-container in de ACS- [groep openbare agent](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) wordt automatisch met het internet blootgesteld. Door standaard, poorten **80**en **443**, **8080** worden geopend en luisteren op poorten (openbare) containers toegankelijk zijn. In dit artikel wordt beschreven hoe u meer om poorten te openen voor uw toepassingen in Azure Container Service.

## <a name="open-a-port-portal"></a>Open een poort (portal) 

Eerst moet de poort moet openen.

1. Log in op de portal.
2. Zoek de resourcegroep die u de Container Azure Service geïmplementeerd.
3. Selecteer de agent-taakverdeling (die met de naam lijkt op **XXXX-agent-lb-XXXX**).

    ![Azure container service taakverdeling](media/container-service-dcos-agents/agent-load-balancer.png)

4. Klik op de **sondes** en vervolgens op **toevoegen**.

    ![Azure container service load balancer sondes](media/container-service-dcos-agents/add-probe.png)

5. De sonde formulier invullen en klik op **OK**.

  	| Veld | Beschrijving |
  	| ----- | ----------- |
  	| Naam  | Een beschrijvende naam van de sonde. |
  	| Poort  | De poort van de container om te testen. |
  	| Pad  | (Wanneer in de HTTP-modus) Het pad relatief website te zoeken. HTTPS wordt niet ondersteund. |
  	| Interval | De hoeveelheid tijd tussen de sonde probeert in seconden. |
  	| Beschadigde drempel | Het aantal opeenvolgende sonde pogingen voordat de container beschadigd. | 
    

6. Klik op **laden regels voor netwerktaakverdeling** en klik vervolgens op **toevoegen**aan de eigenschappen van de taakverdeling agent weer.

    ![Azure container load balancer regels](media/container-service-dcos-agents/add-balancer-rule.png)

7. De load balancer formulier invullen en klik op **OK**.

  	| Veld | Beschrijving |
  	| ----- | ----------- |
  	| Naam  | Een beschrijvende naam voor de taakverdeling. |
  	| Poort  | De binnenkomende openbare poort. |
  	| Back-end-poort | De interne openbare poort van de container voor het routeren van verkeer. |
  	| Back-end-toepassingen | Het doel voor deze taakverdeling worden de containers in deze groep. |
  	| Sonde | De sonde gebruikt om te bepalen of een doel in de **groep met back-end** in orde is. |
  	| Persistentie van de sessie | Hiermee wordt bepaald hoe verkeer vanaf een client voor de duur van de sessie moet worden verwerkt.<br><br>**None**: opeenvolgende aanvragen van dezelfde client kunnen worden verwerkt door de container.<br>**Client-IP-**: opeenvolgende aanvragen van hetzelfde client-IP-worden afgehandeld door dezelfde container.<br>**Client-IP- en protocol**: opeenvolgende aanvragen van dezelfde client IP en protocol combinatie worden afgehandeld door dezelfde container. |
  	| Time-out voor inactiviteit | (Alleen TCP) In minuten, de tijd als u een TCP-/ HTTP-client openen zonder *keepalive -* berichten. |

## <a name="add-a-security-rule-portal"></a>Toevoegen van een regel (portal)

Vervolgens moet een regel waarmee verkeer van onze geopende poorten via de firewall wordt doorgestuurd toevoegen.

1. Log in op de portal.
2. Zoek de resourcegroep die u de Container Azure Service geïmplementeerd.
3. Selecteer de **openbare** agent netwerkgroep (die met de naam lijkt op **XXXX-agent-publiek-nsg-XXXX**).

    ![Azure container service netwerk beveiligingsgroep](media/container-service-dcos-agents/agent-nsg.png)

4. Selecteer de **binnenkomende regels** en klik vervolgens op **toevoegen**.

    ![Azure container service groep beveiligingsregels voor netwerken](media/container-service-dcos-agents/add-firewall-rule.png)

5. Vul de firewallregel uw openbare poort toestaan en klik op **OK**.

  	| Veld | Beschrijving |
  	| ----- | ----------- |
  	| Naam  | Een beschrijvende naam van de firewallregel. |
  	| Prioriteit | De positie van de prioriteit van de regel. Hoe lager het getal hoe hoger de prioriteit. |
  	| Bron | Het binnenkomende IP-adresbereik te worden toegestaan of geweigerd door deze regel te beperken. Gebruik **een** geeft u een beperking niet. |
  	| Service | Selecteer een set vooraf gedefinieerde services is bestemd voor deze regel. **Aangepaste** anders gebruiken voor het maken van uw eigen. |
  	| Protocol | Op basis van **TCP** of **UDP-**verkeer beperken. Gebruik **een** geeft u een beperking niet. |
  	| Poortbereik | Wanneer **Service** **aangepast is**wordt, het bereik van poorten die deze regel geldt voor. U kunt een enkele poort, zoals **80**of een bereik zoals **1024-1500**. |
  	| Actie | Toestaan of weigeren netwerkverkeer dat voldoet aan de criteria. |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het verschil tussen [openbare en particuliere DC-en OS-agenten](container-service-dcos-agents.md).

Lees meer informatie over [uw DC-en OS-containers beheren](container-service-mesos-marathon-ui.md).