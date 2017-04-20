<properties
   pageTitle="Beperken van toegang via Internet facing eindpunten in Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document wordt beschreven hoe u voor de uitvoering van de aanbeveling Azure Beveiligingscentrum **Restrict access via Internet gerichte eindpunt**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Beperken van toegang via Internet facing eindpunten in Azure Security Center

Azure Beveiligingscentrum wordt aanbevolen dat u de toegang via Internet facing eindpunten beperken als een van uw netwerk-beveiligingsgroepen (NSGs) heeft een of meer regels voor binnenkomende verbindingen die toegang van 'elk' IP-adres. Openstellen van "willekeurig" kan kwaadwillende gebruikers toegang krijgen tot de bronnen inschakelen. Beveiligingscentrum wordt aanbevolen dat u deze regels voor binnenkomende verbindingen beperken de toegang tot de bron-IP-adressen die daadwerkelijk toegang nodig hebben als u wilt bewerken.

Deze aanbeveling wordt gegenereerd voor elke niet--poort die "willekeurig" als bron is.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie. Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer **toegang beperken via Internet gerichte eindpunt**in de **aanbevelingen blade**.
![Beperken van toegang via Internet facing eindpunt][1]

2. Hiermee opent u **de toegang beperken via Internet gerichte eindpunt**van het blad. Deze blade geeft een overzicht van de virtuele machines (VMs) met regels voor binnenkomende verbindingen die een potentieel beveiligingsprobleem te maken. Selecteer een VM.
![Selecteer een VM][2]

3. De **NSG** -blade geeft informatie, verwante regels voor binnenkomende verbindingen en de bijbehorende VM netwerk Security Group. Selecteer **de binnenkomende regels bewerken** doorgaan met het bewerken van een binnenkomende regel.
![Network Security Group blade][3]

4. Selecteer de binnenkomende regel bewerken op de **regels voor binnenkomende** blade. In dit voorbeeld gaan we **AllowWeb**selecteren.
![Regels voor inkomende][4]

  Opmerking u kunt ook de **standaardregels** voor een overzicht van de set standaard-voorschriften door alle NSGs selecteren. De standaard-regels kunnen niet worden verwijderd, maar omdat deze een lagere prioriteit zijn toegewezen, kunnen ze worden overschreven door de regels die u maakt. Meer informatie over de [standaardregels](../virtual-network/virtual-networks-nsg.md#default-rules).
![Standaardregels][5]

5. Op het blad **AllowWeb** door de eigenschappen van de binnenkomende regel te bewerken zodat de **bron** een IP-adres of het blokkeren van IP-adressen is. Zie voor meer informatie over de eigenschappen van de binnenkomende regel, [NSG-regels](../virtual-network/virtual-networks-nsg.md#nsg-rules).

  ![Binnenkomende regel bewerken][6]

## <a name="see-also"></a>Zie ook

In dit artikel hebt u geleerd hoe implementeert u de aanbeveling Security Center 'Toegang beperken via Internet gerichte eindpunt'. Voor meer informatie over het inschakelen van NSGs en -regels, raadpleegt u het volgende:

- [Wat is een Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [NSGs met de Azure portal beheren](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md)--informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Azure Security Center aanbevelingen voor beveiliging te beheren](security-center-recommendations.md), informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Security health monitoring in Azure Security Center](security-center-monitoring.md)--informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md)--informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring van partneroplossingen met Azure Security Center](security-center-partner-solutions.md) --informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md)--zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/)--laatste Azure beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
