<properties
   pageTitle="Knooppunttypen Fabric service en VM schaal Sets | Microsoft Azure"
   description="Beschrijft hoe Service Fabric-knooppunttypen betrekking hebben op VM schaal wordt en hoe op afstand verbinding maken met een exemplaar VM schaal instellen of een clusterknooppunt."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>De relatie tussen knooppunttypen Fabric-Service en de virtuele Machine schaal Sets

Virtuele Machine schaal Sets zijn een middel Azure berekenen die kunt u implementeren en beheren van een collectie als een set virtuele machines. Elk knooppunttype dat is gedefinieerd in een cluster Service Fabric is ingesteld als een aparte VM schaal ingesteld. Elk knooppunttype kan vervolgens worden vergroot of omlaag onafhankelijk, hebben verschillende sets van poorten open kan, en andere capaciteit metrics.

De volgende schermafbeelding ziet u een cluster met twee knooppunttypen: FrontEnd en BackEnd.  Elk knooppunttype heeft vijf knooppunten.

![Schermafdruk van een cluster met twee knooppunttypen][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>VM schaal ingesteld exemplaren toewijzen aan knooppunten

Zoals u hierboven zien kunt, starten de exemplaren VM schaal instellen van exemplaar 0 en vervolgens gaan omhoog. De nummering wordt weerspiegeld in de namen. BackEnd_0 is bijvoorbeeld 0 exemplaar van de back-end VM schaal ingesteld. Deze bijzondere VM schaal ingesteld heeft vijf exemplaren, met de naam BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 en BackEnd_4.

Wanneer u de schaal van een VM schaal ingesteld wordt een nieuw exemplaar gemaakt. De instantienaam nieuwe VM schaal ingesteld wordt doorgaans de naam van de VM schaal ingesteld + het nummer van de volgende sessie. In ons voorbeeld is het BackEnd_5.


## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Netwerktaakverdeling instellen VM schaal toewijzing voor elk type knooppunt/VM schaal instellen

Als u het cluster via de portal hebben geïmplementeerd of de Resource Manager voorbeeldsjabloon die we, vervolgens wanneer u een lijst met alle resources in een resourcegroep hebt gebruikt vervolgens ziet u de netwerktaakverdeling voor elk type VM schaal instellen of het knooppunt.

De naam zou ongeveer als volgt: **LB -&lt;naam NodeType&gt;**. Bijvoorbeeld: LB-sfcluster4doc-0, zoals in dit screenshot:


![Bronnen][Resources]


## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Extern verbinding maken met een exemplaar VM schaal instellen of een clusterknooppunt
Elk type knooppunt dat is gedefinieerd in een cluster is geconfigureerd als een aparte VM schaal ingesteld.  Dit betekent dat de knooppunttypen kunnen worden geschaald omhoog of onafhankelijk van elkaar en kan bestaan uit verschillende VM-SKU's. In tegenstelling tot een enkel exemplaar VMs krijg de exemplaren VM schaal ingesteld niet een virtueel IP-adres van hun eigen. Dus kan het lastig zijn een beetje wanneer u naar een IP-adres zoekt en poort kunt u op afstand verbinding maken met een specifieke instantie.

Hier zijn de stappen die u kunt volgen om de instellingen te vinden.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Stap 1: Ontdek het virtuele IP-adres voor het knooppunttype en vervolgens NAT binnenkomende regels voor RDP

U moet die u, de binnenkomende NAT regels waarden die zijn gedefinieerd als onderdeel van de definitie van de bron voor **Microsoft.Network/loadBalancers**.

Ga naar de Load balancer blade en **Instellingen**in de portal.

![LBBlade][LBBlade]


Klik op **regels voor binnenkomende NAT**- **Instellingen**. Deze nu geeft u het IP-adres en poort kunt u op afstand verbinding maken met het eerste exemplaar van VM schaal ingesteld. In de onderstaande schermafbeelding is **104.42.106.156** en **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>Stap 2: Zoeken buiten de poort die u kunt gebruiken om extern verbinding maken met de specifieke VM schaal ingesteld exemplaar van knooppunt

Eerder in dit document, dat ik besproken hoe de VM schaal ingesteld exemplaren worden toegewezen aan de knooppunten. We gebruiken die om de exacte poort te berekenen.

De poorten worden toegewezen in oplopende volgorde van het exemplaar VM schaal ingesteld. in mijn voorbeeld voor het knooppunttype FrontEnd, de poorten die voor elk van de vijf exemplaren zijn de volgende. u moet nu doen dezelfde toewijzing voor uw exemplaar van VM schaal ingesteld.

|**VM schaal exemplaar instellen**|**Poort**|
|-----------------------|--------------------------|
|FrontEnd_0|3389|
|FrontEnd_1|3390|
|FrontEnd_2|3391|
|FrontEnd_3|3392|
|FrontEnd_4|3393|
|FrontEnd_5|3394|


### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>Stap 3: Extern verbinding maken met het specifieke exemplaar van VM schaal instellen

Ik gebruik verbinding met extern bureaublad verbinding maken met de FrontEnd_1 in het screenshot hieronder:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>De waarden voor het RDP-poort wijzigen

### <a name="before-cluster-deployment"></a>Voordat de cluster implementeren

Als u het cluster met behulp van de sjabloon voor een Resource Manager instellen wilt, geeft u het bereik in de **inboundNatPools**.

Ga naar de definitie van de bron voor **Microsoft.Network/loadBalancers**. Onder die vindt u de beschrijving van de **inboundNatPools**.  Vervang de waarden *frontendPortRangeStart* en *frontendPortRangeEnd* .

![InboundNatPools][InboundNatPools]


### <a name="after-cluster-deployment"></a>Na de implementatie van cluster
Dit is iets gecompliceerder en kan leiden tot het VMs gerecycled ophalen. U hebt nu de nieuwe waarden instellen met Azure PowerShell. Zorg ervoor dat de Azure PowerShell 1.0 of hoger op uw computer is geïnstalleerd. Als u dit nog niet hebt gedaan, ik raden dat u de stappen die worden beschreven in [installeren en configureren van Azure PowerShell.](../powershell-install-configure.md)

Aanmelden bij uw account Azure. Als deze PowerShell-opdracht om een of andere reden mislukt, moet u controleren of u beschikt over Azure PowerShell correct geïnstalleerd.

```
Login-AzureRmAccount
```

Voer het volgende als u informatie over de verdeling van de belasting en ziet u de waarden voor de beschrijving van **inboundNatPools**:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Nu *frontendPortRangeEnd* en *frontendPortRangeStart* ingesteld op de gewenste waarden.

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Volgende stappen

- [Overzicht van de functie 'Overal implementeren' en een vergelijking met Azure beheerd clusters](service-fabric-deploy-anywhere.md)
- [Serverclusters, beveiliging](service-fabric-cluster-security.md)
- [Service Fabric SDK en aan de slag](service-fabric-get-started.md)


<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
