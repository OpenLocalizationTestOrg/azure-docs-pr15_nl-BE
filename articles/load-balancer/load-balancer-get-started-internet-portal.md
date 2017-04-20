<properties
   pageTitle="Maak een internetgerichte taakverdeling in Resource Manager, met behulp van de portal Azure | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding taakverdeling in Resource Manager, met behulp van de portal Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="anavinahar"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="annahar" />

# <a name="creating-an-internet-facing-load-balancer-using-the-azure-portal"></a>Maken van een internetverbinding taakverdeling met behulp van de portal Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op het implementatiemodel Resource Manager. U kunt ook [informatie over het maken van een internetverbinding taakverdeling met klassieke implementatie](load-balancer-get-started-internet-classic-portal.md)

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

Dit heeft betrekking op de volgorde van afzonderlijke taken worden uitgevoerd moet voor het maken van een load balancer en wordt uitgelegd in detail de werkzaamheden om het doel te bereiken.

## <a name="what-is-required-to-create-an-internet-facing-load-balancer"></a>Wat is vereist voor het maken van een internetverbinding taakverdeling?

U wilt maken en configureren van de volgende objecten als u wilt een load balancer implementeren.

- IP-configuratie van front-end - bevat openbare IP-adressen voor binnenkomende netwerkverkeer.

- Back-end-adresgroep - bevat netwerkinterfaces (NIC's) voor de virtuele machines voor het netwerkverkeer ontvangen van de taakverdeling.

- Regels voor taakverdeling - regels een openbare poort op de taakverdeling toe te wijzen aan de poort van de back-end-adresgroep bevat.

- Binnenkomende regels NAT - regels een openbare poort op de taakverdeling toe te wijzen aan een poort voor een specifieke virtuele machine in de back-end-adresgroep bevat.

- Sondes - gezondheid sondes gebruikt om te controleren van de beschikbaarheid van exemplaren van de virtuele machines in de back-end-adresgroep bevat.

U krijgt meer informatie over het laden taakverdeling voor onderdelen met Azure Resource Manager op [Azure Resource Manager ondersteuning voor taakverdeling](load-balancer-arm.md).


## <a name="set-up-a-load-balancer-in-azure-portal"></a>Een load balancer in Azure portal instellen

> [AZURE.IMPORTANT] In dit voorbeeld wordt verondersteld een virtueel netwerk genaamd **myVNet**. Zie [virtuele netwerken maken](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) om dit te doen. Zij neemt ook is er een subnet binnen **myVNet** aangeroepen **LB Subnet worden** en twee VMs **web1** en **web2** respectievelijk in dezelfde reeks beschikbaarheid genoemd met de naam **myAvailSet** in **myVNet**. Verwijzen naar [deze koppeling](../virtual-machines/virtual-machines-windows-hero-tutorial.md) VMs maken.


1. Ga vanuit een browser naar de portal Azure: [http://portal.azure.com](http://portal.azure.com) en meld u aan met uw account Azure.

2. Selecteer **Nieuw**links op de bovenkant van het scherm > **netwerk** > **Load Balancer.**

3. Typ een naam voor de verdeling van de belasting in de blade **maken de belasting voor documentconversies** . Hier wordt het **myLoadBalancer**genoemd.

4. Selecteer **openbare**onder **Type**.

5. Maak een nieuwe openbare IP-adres **myPublicIP**genoemd onder **openbare IP-adres**.

6. Selecteer onder resourcegroep, **myRG**. Selecteer vervolgens de juiste **locatie**en klik vervolgens op **OK**. De taakverdeling start vervolgens te implementeren en duurt een paar minuten implementatie is voltooid.

![Taakverdeling resourcegroep bijwerken](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-a-back-end-address-pool"></a>Een back-end-adresgroep maken

1. Zodra de verdeling van de belasting is geïmplementeerd, selecteert u deze uit binnen uw middelen. Selecteer onder instellingen voor back-end-toepassingen. Typ een naam voor uw back-end-toepassingen. Klik vervolgens op de knop **toevoegen** naar de bovenkant van het blad dat wordt weergegeven.

2. Klik op **een virtuele machine toevoegen** in de **groep met back-end Add** blade.  Selecteer **een set beschikbaarheid kiezen** onder **beschikbaarheid** en selecteer **myAvailSet**. Vervolgens **kiest u de virtuele machines** te selecteren onder de sectie virtuele Machines op het blad en klik op **web1** en **web2**, de twee VMs gemaakt voor taakverdeling. Ervoor zorgen dat beide blauwe vinkjes links zoals in de onderstaande afbeelding. Klik vervolgens op **selecteren** in dat blade gevolgd door OK in het blad **kiezen virtuele machines** en vervolgens op **OK** in het blad **back-end-toepassingen toevoegen** .

    ![Toe te voegen aan de back-end-adresgroep- ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Controleer of uw meldingen vervolgkeuzelijst heeft een update met betrekking tot het opslaan van de load balancer backend van toepassingen niet alleen de netwerkinterface voor VMs **web1** en **web2**.


## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Een sonde, regel LB en NAT-regels maken

1. Maak een sonde met gezondheid.

    Schakel onder instellingen van de load balancer sondes. Klik vervolgens op **toevoegen** te vinden op de bovenkant van het blad.

    Er zijn twee manieren voor het configureren van een sonde: http- of TCP. In dit voorbeeld ziet HTTP, maar de TCP kan worden geconfigureerd op een vergelijkbare manier.
    Werk de nodige informatie. Zoals vermeld, laadt **myLoadBalancer** saldo verkeer op poort 80. Het geselecteerde pad is HealthProbe.aspx-Interval is 15 seconden en beschadigde drempel is 2. Zodra u klaar bent, klikt u op **OK** om het maken van de sonde.

    Laat de aanwijzer op de "i" pictogram voor meer informatie over deze afzonderlijke configuraties en hoe deze kunnen worden gewijzigd om aan uw behoeften tegemoet.

    ![Een sonde toe te voegen](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Maak een regel load balancer.

    Klik op de regels in de sectie instellingen van de verdeling van de belasting voor de taakverdeling. Klik op **toevoegen**in het nieuwe blad. De naam van uw regel. Hier is HTTP. Kies de poort frontend en Backend-poort. 80 is hier gekozen voor beide. **LB backend** kiezen als uw back-end-groep en de eerder gemaakte **HealthProbe** als de sonde. Andere configuraties kunnen worden ingesteld volgens uw vereisten. Klik vervolgens op OK om de regel voor taakverdeling opslaan.

    ![Een regel voor taakverdeling toe te voegen](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. NAT regels voor binnenkomende verbindingen maken

    Klik op binnenkomende NAT regels in de sectie instellingen van de verdeling van de belasting. Het nieuwe blad, klik op **toevoegen**. Geef uw binnenkomende NAT regel de naam. Hier wordt het **inboundNATrule1**genoemd. Het doel moet zijn de openbare IP eerder gemaakt. Selecteer aangepast onder Service en selecteer het protocol dat u wilt gebruiken. TCP is hier geselecteerd. Geef de poort, 3441, en in dit geval wordt de doelpoort 3389. Klik vervolgens op OK om deze regel opslaan.

    Zodra de eerste regel is gemaakt, Herhaal deze stap voor het tweede binnenkomende NAT regel inboundNATrule2 aangeroepen vanuit 3442 poort doel poort 3389.

    ![Toevoegen van een binnenkomende regel voor NAT](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Een Load Balancer verwijderen

Selecteer een load balancer verwijderen, de verdeling van de belasting die u wilt verwijderen. Klik op **verwijderen** zich aan de bovenkant van het blad in de blade *-Taakverdeling* . Selecteer **Ja** wanneer u wordt gevraagd.

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-cli.md)

[Een load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
