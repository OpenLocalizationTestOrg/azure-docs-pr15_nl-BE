<properties
   pageTitle="De introductiehandleiding Azure Security Center | Microsoft Azure"
   description="Dit artikel helpt u snel aan de slag met Azure Beveiligingscentrum leidt u door de componenten voor monitoring en beleid van beveiliging en koppelt u naar de volgende stappen."
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
   ms.date="10/28/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-quick-start-guide"></a>De introductiehandleiding Azure Security Center

Dit artikel helpt u snel aan de slag met Azure Security Center door leidt u door de security monitoring en beleid beheeronderdelen van Security Center.

> [AZURE.NOTE] Dit artikel introduceert de service met behulp van een voorbeeld van de implementatie. Dit artikel bevat geen stapsgewijze instructies.

## <a name="prerequisites"></a>Vereisten

Om te beginnen met Security Center, hebt u een abonnement op Microsoft Azure. Als u niet een abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

De vrij laag van Security Center wordt uw abonnement automatisch ingeschakeld en biedt inzicht in de beveiligingsstatus van uw resources Azure. Het biedt elementaire beveiligingsbeleid beheren, aanbevelingen voor beveiliging en integratie, beveiligingsproducten en diensten van partners Azure.

U openen Beveiligingscentrum vanaf de [portal Azure](https://azure.microsoft.com/features/azure-portal/). Voor meer informatie over de Azure portal, raadpleegt u de [documentatie van de portal](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="data-collection"></a>Het verzamelen van gegevens

Security Center verzamelt gegevens van uw virtuele machines (VMs) beoordeling van de toestand van hun beveiliging en een waarschuwing voor bedreigingen bevatten aanbevelingen voor beveiliging. Wanneer u eerst Security Center, wordt het verzamelen van gegevens is ingeschakeld op alle VMs in uw abonnement. Het verzamelen van gegevens wordt aanbevolen, maar u kunt opt-out voor het verzamelen van gegevens in het beleid van Beveiligingscentrum uitschakelt.

De volgende stappen wordt beschreven hoe u toegang tot en gebruik van de onderdelen van de Security Center. In deze stappen zien we hoe u het verzamelen van gegevens uitschakelen als u opt-out.

## <a name="access-security-center"></a>Access Security Center

Ga als volgt te werk om Beveiligingscentrum te openen in de portal:

1. Selecteer in het menu **Microsoft Azure** **Security Center**.
![Azure menu][1]

2. Als u de Security Center voor het eerst opent, wordt de **Welkom** blade geopend. Selecteer Ja **! Ik wil starten Azure Security Center** de blade **Beveiligingscentrum** te openen en het verzamelen van gegevens inschakelen.
![Welkom-scherm][10]

3. Nadat u Beveiligingscentrum vanuit het Welkom-blade starten of Security Center in het menu Microsoft Azure selecteert, wordt het blad **Beveiligingscentrum** geopend. Voor eenvoudige toegang tot de blade **Security Center** in de toekomst de optie **pincode blade aan dashboard** (rechtsboven).
![PIN-blade met optie dashboard][2]

## <a name="use-security-center"></a>Met Beveiligingscentrum

U kunt beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen. We gaan een beveiligingsbeleid voor uw abonnement configureren:

1. Selecteer de tegel **beleid** op de bladeserver **Security Center** .
![Beveiligingsbeleid][3]

2. Selecteer een abonnement op het blad **beveiligingsbeleid - beleid per abonnement of de resourcegroep definiëren** .
3. **Het verzamelen van gegevens** is ingeschakeld op de bladeserver **beveiligingsbeleid** voor het automatisch verzamelen van Logboeken. De uitbreiding van de controle is op alle huidige en nieuwe VMs in het abonnement ingericht. (U kunt niet deelnemen aan het verzamelen van gegevens door **het verzamelen van gegevens** op **uit**, maar dit voorkomt dat Security Center zodat u waarschuwingen en aanbevelingen).
4. Selecteer op het **beveiligingsbeleid** blade, **Kies een account voor opslag per regio**. Voor elke regio waarin u met VMs hebt, kiest u de opslag rekening waarop gegevens worden verzameld uit de VMs is opgeslagen. Als u niet voor elke regio een account opslag kiest, wordt deze voor u gemaakt. De gegevens die worden verzameld logisch gescheiden is van de gegevens van andere klanten om veiligheidsredenen.

     > [AZURE.NOTE] U wordt aangeraden het verzamelen van gegevens inschakelen en kiest u eerst een account voor opslag op het niveau van het abonnement. Beveiligingsbeleid kunnen worden ingesteld op het niveau van de resourcegroep en Azure-abonnement, maar de configuratie van het verzamelen van gegevens en account opslag plaatsvindt op het soort abonnement.

5. Selecteer op de bladeserver **beveiligingsbeleid** **preventiebeleid**. Hiermee opent u het **preventiebeleid voor** blade.
![Preventiebeleid][4]

6. Schakel op de bladeserver **preventiebeleid voor** de aanbevelingen die u wilt weergeven als onderdeel van het beveiligingsbeleid. Voorbeelden:

 - Als u **systeemupdates** op **op** scannen alle ondersteunde virtuele machines naar ontbrekende updates voor het besturingssysteem.
 - **OS beveiligingslekken** instellen om **op** scant alle ondersteunde virtuele machines voor de OS-configuraties die ervoor dat de virtuele machine kwetsbaarder zorgen kunnen voor aanvallen.

### <a name="view-recommendations"></a>Aanbevelingen weergeven

1. Terug naar de blade **Security Center** en selecteer de tegel **aanbevelingen** . Security Center analyseert regelmatig de beveiligingsstatus van uw resources Azure. Wanneer Beveiligingscentrum potentiële beveiligingsproblemen geeft, worden op het blad **aanbevelingen** aanbevelingen.
![Aanbevelingen in Azure Security Center][5]

2.  Selecteer een aanbeveling over de blade **aanbevelingen** om meer informatie te bekijken en/of stappen te ondernemen om het probleem kunt oplossen.

### <a name="view-the-health-and-security-state-of-your-resources"></a>De status van gezondheid en veiligheid van uw resources weergeven

1.  Terug naar de blade **Security Center** . De **gezondheid van bronnen beveiliging** tegel bevat indicatoren voor de toestand van de beveiliging voor virtuele machines, netwerk, gegevens en toepassingen.
2.  Selecteer de **virtuele machines** voor meer informatie. De blade **virtuele machines** wordt geopend en geeft een statusoverzicht van antimalware programma's, systeemupdates opnieuw wordt opgestart en OS kwetsbaarheid van het VMs.
![De tegel Resources gezondheid in Azure Security Center][6]

3.  Selecteer een aanbeveling krachtens de **Aanbevelingen van de virtuele MACHINE** voor meer informatie en/of maatregelen benodigde besturingselementen configureren.
4.  Selecteer een VM onder **virtuele machines** om meer details te bekijken.

### <a name="view-security-alerts"></a>Waarschuwingen weergeven

1.  Terug naar de blade **Security Center** en selecteer de tegel **beveiligingswaarschuwingen** . De blade **beveiligingswaarschuwingen** wordt geopend en een lijst met waarschuwingen worden weergegeven. De analyse van de Security Center van uw beveiligingslogboeken en netwerkactiviteit deze waarschuwingen genereert. Meldingen van het voor geïntegreerde partneroplossingen zijn opgenomen.
![Beveiligingswaarschuwingen in Azure Security Center][7]

    > [AZURE.NOTE] Security alerts zijn alleen beschikbaar als de standaard laag van Beveiligingscentrum is ingeschakeld. Er is een gratis proefversie van 90 dagen van de laag standaard beschikbaar. Zie de [volgende stappen](#next-steps) voor meer informatie over hoe u de standaard laag.

2.  Selecteer een signaal voor extra informatie. In dit voorbeeld zullen we selecteren **gewijzigd systeem binaire ontdekt**. Blades die extra informatie over de waarschuwing bevatten wordt geopend.
![Security alert details in Azure Security Center][8]

### <a name="view-the-health-of-your-partner-solutions"></a>De gezondheid van uw partneroplossingen weergeven

1. Terug naar de blade **Security Center** . De **oplossingen van partners** tegel kunt u controleren in een oogopslag de status van uw partner solutions geïntegreerd met uw abonnement Azure.
2. Selecteer de tegel **oplossingen van partners** . Een blade wordt geopend en geeft een overzicht van de oplossingen van partners verbonden met Security Center.
![Oplossingen van partners][9]

3. Selecteer de partneroplossing van een. In dit voorbeeld gaan we de oplossing **F5 WAF** selecteren.  Een blade wordt geopend en geeft u de status van de partneroplossing en de bijbehorende bronnen van de oplossing. Selecteer **oplossing console** openen de partner management ervaring voor deze oplossing.

## <a name="next-steps"></a>Volgende stappen
Dit artikel werd geïntroduceerd tot monitoring en beleid voor beheer van Security Center. Nu u vertrouwd bent met Security Center, proberen de volgende stappen uit:

- Configureer een beveiligingsbeleid voor uw abonnement op Azure. Voor meer informatie, Zie de [instelling voor beveiligingsbeleid in Azure Security Center](security-center-policies.md).
- Met de aanbevelingen in Beveiligingscentrum kunt u uw Azure bronnen beveiligen. Voor meer informatie, Zie [beheren in Azure Security Center aanbevelingen](security-center-recommendations.md).
- Bekijken en beheren van uw huidige beveiligingswaarschuwingen. Zie voor meer informatie kunt [beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md).
- Meer informatie over de [Geavanceerde functies voor detectie van bedreiging](security-center-detection-capabilities.md) die worden geleverd met de [standaard niveaus](security-center-pricing.md) van Security Center. Er is een gratis proefversie van 90 dagen van de laag standaard beschikbaar.
- Hebt u vragen over het gebruik van Security Center, Raadpleeg de [Veelgestelde vragen over het Azure Security Center](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
