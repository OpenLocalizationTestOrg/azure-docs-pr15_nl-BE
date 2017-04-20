<properties
   pageTitle="Het toevoegen van een web application firewall in Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document wordt beschreven hoe u de Azure Security Center aanbevelingen **Add web application firewall** en **beveiliging Finalize**implementeren."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Een web application firewall in Beveiligingscentrum Azure toevoegen

Azure Beveiligingscentrum kan aanbevelen dat u een web application firewall (WAF) van een Microsoft-partner wilt beveiligen van uw webtoepassingen toevoegen. Dit document leidt u door een voorbeeld van hoe u dit doet.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.  Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer **beveiligde webtoepassing web application firewall gebruikt**in de blade **aanbevelingen** .
![Beveiligde web Application][1]

2. Selecteer in het blad **beveiligen van uw webtoepassingen met web application firewall** een webtoepassing. Het blad van **een Web Application Firewall toevoegen** wordt geopend.
![Een web application firewall toevoegen][2]
3. U kunt een bestaand web application firewall gebruiken indien beschikbaar of kunt u een nieuwe maken. In dit voorbeeld zijn er geen bestaande WAFs beschikbaar dus we een nieuwe WAF maken.

4. Als u een nieuwe WAF, selecteert een oplossing uit de lijst met geïntegreerde partners. In dit voorbeeld selecteren we **Barracuda Web Application Firewall**.
5. De **Barracuda Web Application Firewall** blade wordt geopend zodat u informatie over de partneroplossing. Selecteer **maken** in het blad informatie.
![Firewall informatie blade][3]

6. Het **Nieuwe Web Application Firewall** blad wordt geopend, waar **VM** configuratiestappen uitvoeren en **WAF**-informatie. Selecteer **VM configuratie**.

7. In het blad **VM configuratie** voert u informatie voor het draaien van de virtuele machine die wordt uitgevoerd de WAF.
![VM-configuratie][4]
8. Terug naar het **Nieuwe Web Application Firewall** blade en **WAF informatie**selecteren. In het blad **WAF informatie** configureert u de WAF zelf. Stap 7 kunt u voor het configureren van de virtuele machine waarop de WAF wordt uitgevoerd en stap 8 kunt u de WAF zelf inrichten.

## <a name="finalize-application-protection"></a>Toepassingsbeveiliging voltooien

1. Terug naar de blade **aanbevelingen** . Een nieuwe post is gegenereerd nadat u de WAF, genaamd **Finalize Toepassingsbeveiliging**gemaakt. Deze vermelding kunt u weten dat u nodig hebt voor het voltooien van het proces van het daadwerkelijk bedrading van de WAF binnen het virtuele netwerk Azure, zodat de toepassing kunnen worden beveiligd.
![Toepassingsbeveiliging voltooien][5]

2. Selecteer **beveiliging voltooien**. Hiermee opent u een nieuwe blade. U ziet dat er een webtoepassing die nodig zijn om het verkeer omgeleid.
3. Selecteer de webtoepassing. Een blade geopend waarmee u stappen voor het voltooien van de web application firewall-instellingen. Voer de stappen uit en selecteer vervolgens **verkeer beperken**. Beveiligingscentrum wordt vervolgens de bedrading tot voor u doen.
![Verkeer beperken][6]

> [AZURE.NOTE] U kunt meerdere webtoepassingen in Beveiligingscentrum beveiligen door deze toepassingen toevoegen aan uw bestaande WAF-implementaties. WAF-toestellen (gemaakt met het implementatiemodel Resource Manager) moeten worden geïmplementeerd op een afzonderlijke virtuele netwerk. WAF-toestellen (gemaakt met het klassieke implementatiemodel) zijn beperkt tot het met behulp van een beveiligingsgroep voor het netwerk. Deze ondersteuning zal in de toekomst worden uitgebreid tot een volledig aangepaste implementatie van een WAF toestel (klassiek). Meer informatie over de [klassieke en Resource Manager implementatiemodellen](../azure-classic-rm.md) voor Azure resources.

De logboeken van die WAF zijn nu volledig geïntegreerd. Security Center kunt starten automatisch verzamelen en analyseren van de logbestanden zodat deze beveiligingswaarschuwingen belangrijk u kunt oppervlak.

## <a name="see-also"></a>Zie ook

Dit document werd door het implementeren van de aanbeveling van de Security Center 'Een webtoepassing toevoegen'. Voor meer informatie over het configureren van een web application firewall, raadpleegt u het volgende:

- [Een Web Application Firewall (WAF) configureren voor App-omgeving](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Statuscontrole voor beveiliging in Azure Security Center](security-center-monitoring.md) --informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Aanbevelingen voor de beveiliging beheren in Azure Security Center](security-center-recommendations.md) --informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) --blog zoeken naar berichten over Azure beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
