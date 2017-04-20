<properties
   pageTitle="Verhelpen van beveiligingsproblemen in Beveiligingscentrum Azure OS | Microsoft Azure"
   description="Dit document wordt beschreven hoe u de aanbeveling Azure Security Center **problemen verhelpen OS**implementeren."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Een OS beveiligingsproblemen in Azure Security Center te verhelpen

Beveiligingscentrum Azure analyseert dagelijks uw besturingssysteem (OS) van de virtuele machine (VM) voor configuraties die kunnen de VM kwetsbaar maken voor aanvallen en raadt aan om de configuratiewijzigingen van deze beveiligingslekken. Zie de [lijst met regels van de aanbevolen configuratie](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335)voor meer informatie over de specifieke configuraties worden gecontroleerd. Security Center raadt u beveiligingsproblemen oplossen wanneer de configuratie van het besturingssysteem van uw VM komt niet overeen met de aanbevolen configuratieregels.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.  Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in de blade **aanbevelingen** **beveiligingslekken OS verhelpen**.
![Een OS beveiligingslekken te verhelpen][1]

    Het blad **problemen verhelpen OS** wordt geopend en geeft een overzicht van de VMs met OS-configuraties die niet overeenkomen met de aanbevolen configuratieregels.  Voor elke VM geeft het blad:

   - **Regels is mislukt** - het aantal regels die niet van de VM configuratie van het besturingssysteem.
   - **Tijd laatste SCAN** --de datum en tijd dat Security Center laatste configuratie van het besturingssysteem van de VM gescand.
   - **Status** --de huidige status van het beveiligingslek:

        - Open: Het beveiligingslek niet is beantwoord nog
        - In uitvoering: Het beveiligingslek dat momenteel wordt toegepast, door u is geen actie vereist.
        - Opgelost: Het lek is al besproken (wanneer het probleem opgelost is, de post wordt grijs weergegeven)
  - **ERNST** --alle beveiligingslekken zijn ingesteld op een prioriteit van de laag, wat betekent dat een beveiligingslek moet worden behandeld, maar geen onmiddellijke aandacht vereist.

Selecteer een VM. Een blade voor die VM wordt geopend en geeft de regels die zijn mislukt.
   ![Van configuratieregels die niet zijn][2]

Selecteer een regel. In dit voorbeeld kunt **wachtwoorden moeten voldoen aan complexiteitsvereisten**selecteren. Een blade wordt geopend met een beschrijving van de regel is mislukt en de impact. Controleer de gegevens en kunt u het besturingssysteemconfiguraties worden toegepast.
  ![Beschrijving van de regel is mislukt][3]

  Security Center maakt gebruik van algemene configuratie opsomming (CCE) voor het toewijzen van unieke id's voor van configuratieregels. De volgende informatie is beschikbaar op deze blade:

  - --Naam van regel
  - ERNST--CCE ernst waarde van essentieel belang of waarschuwing
  - CCIED--CCE de unieke id voor de regel
  - Beschrijving: Beschrijving van regel
  - Beveiligingslek met betrekking tot--Uitleg van beveiligingslek of het risico als de regel wordt niet toegepast
  - IMPACT--Business impact als regel wordt toegepast.
  - VERWACHTE waarde: Waarde verwacht wanneer Beveiligingscentrum uw configuratie VM OS tegen de regel analyseert
  - BEWERKING van de regel--Regel bewerking door Beveiligingscentrum gebruikt tijdens de analyse van de configuratie van de VM OS tegen de regel
  - WERKELIJKE waarde--Waarde geretourneerd na analyse van de configuratie van de VM OS tegen de regel
  - RESULTAAT van de evaluatie –-resultaat van de analyse: doorgeeft, is mislukt


## <a name="see-also"></a>Zie ook

In dit artikel hebt u geleerd hoe implementeert u de aanbeveling Security Center "Remediate OS beveiligingsproblemen". U kunt de set configuratieregels controleren [hier](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). CCE (algemene configuratie opsomming) Security Center gebruikt voor het toewijzen van unieke id's voor van configuratieregels. Ga naar de [CCE](http://cce.mitre.org) voor meer informatie.

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Security Center](security-center-recommendations.md) --informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Statuscontrole voor beveiliging in Azure Security Center](security-center-monitoring.md) --informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring van partneroplossingen met Azure Security Center](security-center-partner-solutions.md) --informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) --blog zoeken naar berichten over Azure beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
