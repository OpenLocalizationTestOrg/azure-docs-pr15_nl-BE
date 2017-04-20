<properties
   pageTitle="Beheer van oplossingen van partners in Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document helpt u bij hoe Azure Security Center kunt controleren in een oogopslag de status van uw partneroplossingen is geïntegreerd met uw abonnement Azure."
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

# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Monitoring oplossingen van partners met Azure Security Center

Dit document helpt u bij het controleren van de status van uw oplossingen van partners in Azure Security Center.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie. Dit is een stapsgewijze handleiding.

## <a name="monitoring-partner-solutions"></a>Monitoring oplossingen van partners

De **oplossingen van partners** naast elkaar op het blad **Security Center** kunt controleren in een oogopslag de status van uw partneroplossingen is geïntegreerd met uw abonnement Azure.
![Oplossingen van partners naast elkaar][1]

De **oplossingen van partners** tegel wordt het aantal oplossingen van partners en een samenvatting van deze oplossingen status.

De **STATUS** van de partneroplossing van een kan zijn:

- Beveiligd (groen) - er is geen probleem voor de gezondheid.
- Wat de netwerkverbindingsmogelijkheden ervan (rood) - er is een probleem voor de gezondheid die onmiddellijke aandacht vereist.
- Gestopt (oranje) rapportage - oplossing voor het melden van de gezondheid is gestopt.
- Onbekende beschermingsstatus (oranje) - de gezondheid van de oplossing is onbekend op dit moment als gevolg van een mislukt proces van een nieuwe resource toevoegen aan de bestaande oplossing.
- Niet gemeld (grijs) - de oplossing is niet alles gemeld maar de status van de oplossing worden niet-aangegeven als het net is aangesloten en is nog steeds implementeren.

Als er geen oplossingen op te nemen met uw abonnement wordt de tegel staat dat er geen oplossingen beschikbaar zijn. De tegel **oplossingen van partners** te selecteren, kunt u de blade **aanbevelingen** voor de implementatie van oplossingen van partners beveiliging openen.
![Geen partner solutions][2]

De gezondheid van uw partneroplossingen bekijken:

1. Selecteer de tegel **oplossingen van partners** . Een blade wordt geopend met een lijst van de oplossingen van partners verbonden met Security Center.
![Oplossingen van partners][3]

2. Selecteer de partneroplossing van een. In dit voorbeeld kunt de oplossing **F5 WAF2** selecteren.  Een blade wordt geopend met de status van de partneroplossing en van de oplossing die resources. Selecteer **oplossing console** openen de partner management ervaring voor deze oplossing.
![Partner-oplossing details][4]

3. Ga terug naar de blade **F5-WAF2** en selecteer **koppeling app**. Hiermee opent u het blad **Koppeling toepassingen** . Hier kunt u resources aan de partneroplossing.
![Koppeling resources solution partner][5]

## <a name="see-also"></a>Zie ook
In dit document, hebt u kennisgemaakt met de **Oplossingen van partners** tegel in Beveiligingscentrum. Voor meer informatie over de Security Center, raadpleegt u het volgende:

- [Het instellen van beveiligingsbeleid in Beveiligingscentrum Azure](security-center-policies.md) , informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Azure Security Center aanbevelingen voor beveiliging te beheren](security-center-recommendations.md) , informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Security health monitoring in Beveiligingscentrum Azure](security-center-monitoring.md) , informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Beveiligingscentrum Azure](security-center-managing-and-responding-alerts.md) , informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Veelgestelde vragen over het Azure Security Center](security-center-faq.md) , zoeken, veelgestelde vragen over het gebruik van de service.
- [Beveiliging Azure blog](http://blogs.msdn.com/b/azuresecurity/) , de meest recente Azure beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
