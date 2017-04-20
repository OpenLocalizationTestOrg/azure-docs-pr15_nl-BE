<properties
   pageTitle="Het toevoegen van een volgende generatie firewall in Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document wordt beschreven hoe u de Azure Security Center aanbevelingen **toevoegen een volgende generatie Firewall** - en **traffice van de Route via NGFW alleen**implementeren."
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

# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Het toevoegen van een volgende generatie Firewall in Beveiligingscentrum Azure

Azure Beveiligingscentrum kan aanbevelen dat u een volgende generatie firewall (NGFW) van een Microsoft-partner toevoegen om uw beveiligingsinstellingen. Dit document leidt u door een voorbeeld van hoe u dit doet.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.  Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in het blad **aanbevelingen** **een volgende generatie Firewall toevoegen**.
![Een volgende generatie Firewall toevoegen][1]

2. Selecteer een eindpunt in het blad **een volgende generatie Firewall toevoegen** .
![Selecteer een eindpunt][2]

3. Een tweede blade voor **een volgende generatie Firewall toevoegen** wordt geopend. U kunt een bestaande oplossing gebruiken indien beschikbaar of kunt u een nieuwe maken. In dit voorbeeld zijn er geen bestaande oplossingen beschikbaar dus we een nieuwe NGFW maken.
![Nieuw volgende generatie Firewall maken][3]

4. Als u wilt een nieuwe NGFW maken, selecteer een oplossing uit de lijst met geïntegreerde partners. In dit voorbeeld selecteren we **Check Point**.
![Selecteer de volgende generatie Firewall-oplossing][4]

5. De blade **Check Point** wordt geopend zodat u informatie over de partneroplossing. Selecteer **maken** in het blad informatie.
![Firewall informatie blade][5]

6. Hiermee opent u het blad **maken virtuele machine** . Hier kunt u informatie voor het draaien van een virtuele machine (VM) waarmee het NGFW wordt uitgevoerd. Volg de stappen en geef de vereiste informatie voor NGFW. Klik op OK om toe te passen.
![Maken van de virtuele machine voor het uitvoeren van NGFW][6]

## <a name="route-traffic-through-ngfw-only"></a>Route-verkeer via NGFW alleen

Terug naar de blade **aanbevelingen** . Een nieuwe post is gegenereerd nadat u **Route verkeer via NGFW alleen**een NGFW via Beveiligingscentrum toegevoegd genoemd. Deze aanbeveling wordt alleen gemaakt als u uw NGFW via de Security Center geïnstalleerd. Als u de eindpunten internetverbinding hebt, raden Security Center aan netwerk groep regels die zorgen dat binnenkomend verkeer naar uw VM via uw NGFW te configureren.

1. Selecteer in de **aanbevelingen blade** **Route verkeer via NGFW alleen**.
![Route-verkeer via NGFW alleen][7]

2. Hiermee opent u het **doorsturen van verkeer via NGFW alleen** met VMs die u kunt verkeer routeren blade. Selecteer een VM uit de lijst.
![Selecteer een VM][8]

3. Een blade voor de geselecteerde VM met de gerelateerde regels voor binnenkomende verbindingen wordt geopend. Een beschrijving vindt u meer informatie over mogelijke volgende stappen. Selecteer **de binnenkomende regels bewerken** doorgaan met het bewerken van een binnenkomende regel. De verwachting is dat **bron** niet is ingesteld op **alle** voor de internetgerichte eindpunten gekoppeld aan de NGFW. Zie voor meer informatie over de eigenschappen van de binnenkomende regel, [NSG-regels](../virtual-network/virtual-networks-nsg.md#nsg-rules).
![Configureren van regels voor het beperken van toegang tot][9]
![binnenkomende regel bewerken][10]

## <a name="see-also"></a>Zie ook

Dit document werd door het implementeren van de aanbeveling van de Security Center 'Een volgende generatie Firewall toevoegen'. Zie de volgende onderwerpen voor meer informatie over NGFWs en de oplossing van de partner Check Point:

- [Volgende generatie Firewall](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
- [Check Point vSEC](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het beveiligingsbeleid configureren.
- [Aanbevelingen voor de beveiliging beheren in Azure Security Center](security-center-recommendations.md) --informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Statuscontrole voor beveiliging in Azure Security Center](security-center-monitoring.md) --informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring van partneroplossingen met Azure Security Center](security-center-partner-solutions.md) --informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) --blog zoeken naar berichten over Azure beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
