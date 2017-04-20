<properties
   pageTitle="Op locatie gegevensgateway | Microsoft Azure"
   description="Een On-premises-gateway is nodig als u de Analysis Services-server in Azure verbinding maken met gegevensbronnen voor gebouwen."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="on-premises-data-gateway"></a>Gegevens-gateway op de gebouwen

De gegevens in de lokalen gateway fungeert als een brug, die veilige gegevensoverdracht tussen gegevensbronnen op gebouwen en uw Azure Analysis Services-server in de cloud.

Een gateway is geïnstalleerd op een computer in uw netwerk. Een gateway moet worden geïnstalleerd voor elke Azure Analysis Services-server die u in uw abonnement Azure hebt. Bijvoorbeeld als er twee servers in uw abonnement Azure verbinding met gegevensbronnen voor lokalen maken, moet een gateway geïnstalleerd op twee afzonderlijke computers in het netwerk.

## <a name="requirements"></a>Vereisten

**Minimale systeemvereisten:**

- 4.5 .NET framework
- 64-bits versie van Windows 7 / Windows Server 2008 R2 (of hoger)

**Aanbevolen:**

- 8 core CPU
- 8 GB geheugen
- 64-bits versie van Windows 2012 R2 (of hoger)

**Belangrijke overwegingen:**

- De gateway kan niet worden geïnstalleerd op een domeincontroller.

- Slechts één gateway kan worden geïnstalleerd op één computer.

- De gateway installeren op een computer die blijft op en gaat niet naar de slaapstand. Als de computer zich niet op, kan uw Azure Analysis Services-server kan geen verbinding maken met uw op gebouwen-gegevensbronnen om gegevens te vernieuwen.

- De gateway niet installeert op een computer draadloos verbonden met het netwerk. Prestaties kan afnemen.

- Als u wilt wijzigen de naam van de server voor een gateway die al zijn geconfigureerd, moet u opnieuw installeren en configureren van een nieuwe gateway.

- In sommige gevallen kunnen in tabelvorm modellen verbinding maken met gegevensbronnen native providers zoals SQL Server Native Client (SQLNCLI11) met een fout geretourneerd. Voor meer informatie, Zie de [Datasource-verbindingen](analysis-services-datasource.md).

## <a name="supported-on-premises-data-sources"></a>Gegevensbronnen ondersteunde in gebouwen
Als voorbeeld ondersteunt de gateway verbindingen tussen uw Azure Analysis Services-server en de volgende gegevensbronnen op ruimten:

- SQL Server
- SQL datawarehouse
- APS
- Oracle
- Teradata


## <a name="download"></a>Downloaden
 [De gateway downloaden](https://aka.ms/azureasgateway)


## <a name="install-and-configure"></a>Installeren en configureren

1. Voer setup uit.

2. Kies een locatie voor de installatie en accepteer de licentievoorwaarden.

3. Aanmelden bij Azure.

4. Geef uw naam Azure Analysis Server. U kunt slechts één server per gateway opgeven. Klik op **configureren** en u bent goed om te gaan.

    ![aanmelden bij azure](./media\analysis-services-gateway\aas-gateway-configure-server.png)


## <a name="how-it-works"></a>Hoe het werkt
De gateway wordt uitgevoerd als een Windows-service, **On-premises gegevensgateway**, op een computer in het netwerk van uw organisatie. De gateway die u hebt geïnstalleerd voor gebruik met Azure Analysis Services is gebaseerd op de dezelfde gateway die wordt gebruikt voor andere services, zoals Power BI, maar met enkele verschillen in hoe deze geconfigureerd.

![Hoe het werkt](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Query's en gegevens stroom werken als volgt:

1.  Een query is gemaakt door de cloud-service met de gecodeerde referenties voor de gegevensbron op gebouwen. Vervolgens wordt het verzonden naar een wachtrij voor de gateway worden verwerkt.

2.  De gateway cloud-service de query analyseert en duwt de aanvraag naar de [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).

3.  De gateway op ruimten gegevens opgevraagd Azure Service Bus voor aanvragen in behandeling.

4.  De query haalt de gateway, decodeert de referenties en maakt verbinding met gegevensbronnen referenties.

5.  De gateway stuurt de query voor de gegevensbron voor de uitvoering.

6.  De resultaten uit de gegevensbron verzonden naar de gateway en vervolgens naar de cloud-service.

## <a name="windows-service-account"></a>Windows Service-account

De gegevens op het bedrijf gateway is geconfigureerd voor gebruik van *NT SERVICE\PBIEgwService* voor de aanmeldingsreferenties van het Windows-service. Deze heeft standaard het recht Aanmelden als een service; in het kader van de computer die u installeert de gateway op. Deze referentie is niet dezelfde rekening gebruikt voor verbinding met gegevensbronnen voor lokalen of uw account Azure.  

Als u problemen met uw proxyserver door verificatie ondervindt, u kunt de Windows-account voor een domeingebruiker te wijzigen of beheerd serviceaccount.

## <a name="ports"></a>Poorten

De gateway maakt een uitgaande verbinding Azure Service bus. Deze communiceert op uitgaande poorten: TCP-443 (standaard), 5671, 5672, 9350 via 9354.  De gateway is niet vereist voor inkomende poorten.

Het is u aanbevolen "witte" lijst de IP-voor de regio van uw gegevens in uw firewall adressen. U kunt de [lijst met Microsoft Azure Datacenter IP](https://www.microsoft.com/download/details.aspx?id=41653)downloaden. Deze lijst wordt wekelijks bijgewerkt.

> [AZURE.NOTE]  De IP-adressen weergegeven in de lijst IP-Azure-Datacenter zijn in de CIDR-notatie. 10.0.0.0/24 betekent bijvoorbeeld niet 10.0.0.0 tot en met 10.0.0.24. Meer informatie over de [CIDR-notatie](http://whatismyipaddress.com/cidr).

Hieronder vindt u de FQDN-namen die door de gateway wordt gebruikt.

|Domeinnamen|Uitgaande poorten|Beschrijving|
|---|---|---|
|*. powerbi.com|80|HTTP wordt gebruikt voor het downloaden van het installatieprogramma.|
|*. powerbi.com|443|HTTPS|
|*. analysis.windows.net|443|HTTPS|
|*. login.windows.net|443|HTTPS|
|*. servicebus.windows.net|5671 5672|Geavanceerde Message Queuing-Protocol (AMQP)|
|*. servicebus.windows.net|443, 9350 9354|Listeners op Bus-Relay-Service via TCP (443 voor toegangsbeheer token aanschaf vereist)|
|*. frontend.clouddatahub.net|443|HTTPS|
|*. core.windows.net|443|HTTPS|
|Login.microsoftonline.com|443|HTTPS|
|*. msftncsi.com|443|Internet-verbindingen te testen als de gateway onbereikbaar voor de Power BI-service is gebruikt.|
|*.microsoftonline p.com|443|Gebruikt voor verificatie, afhankelijk van de configuratie.|


### <a name="forcing-https-communication-with-azure-service-bus"></a>HTTPS-communicatie met Azure Service Bus forceren

U kunt forceren dat de gateway om te communiceren met Azure Service Bus met HTTPS in plaats van directe TCP; Dit kan prestaties echter aanzienlijk minder. U moet het bestand *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* wijzigen. Wijzig de waarde van `AutoDetect` op `Https`. Dit bestand bevindt zich standaard in *C:\Program Files\On-premises gegevensgateway*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```


## <a name="troubleshooting"></a>Het oplossen van problemen
Onder de motorkap is de op het bedrijf gegevensgateway gebruikt voor verbinding met uw gegevensbronnen op ruimten Azure Analysis Services met Power BI gebruiken dezelfde gateway.

Als u problemen ondervindt bij het installeren en configureren van een gateway, zorg voor een overzicht van [de Power BI-Gateway het oplossen van problemen](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem-tshoot/). Als u denkt dat u een probleem ondervindt met uw firewall dat, Raadpleeg de firewall of proxyserver.

Als u denkt dat u proxy-problemen ontstaan met de gateway, Zie [de proxy-instellingen configureren voor de Power BI-Gateways](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy.md).

## <a name="next-steps"></a>Volgende stappen
- [Analyseservices beheren](analysis-services-manage.md)
- [Gegevens ophalen uit Azure Analysis Services](analysis-services-connect.md)
