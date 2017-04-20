<properties
   pageTitle="Contactgegevens in Beveiligingscentrum Azure beveiliging bieden | Microsoft Azure"
   description="Dit document wordt uitgelegd hoe u contactgegevens in Beveiligingscentrum Azure beveiliging bieden."
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

# <a name="provide-security-contact-details-in-azure-security-center"></a>Contactgegevens in Beveiligingscentrum Azure beveiliging bieden

Beveiligingscentrum Azure aanbevelen contactgegevens beveiliging te bieden voor uw Azure abonnement als u dat nog niet gedaan hebt. Deze informatie wordt gebruikt door Microsoft contact met u opnemen als u het Microsoft Security Response Center (MSRC) ontdekt dat uw klantgegevens is geopend door een onwettige of niet-geautoriseerde persoon. MSRC voert selecteert u beveiliging controle op de Azure-netwerk en de infrastructuur en threat intelligence en misbruik klachten van derden ontvangt.

Een e-mailbericht wordt verzonden op het eerste exemplaar dagelijks een alarm en waarschuwingen met hoge prioriteit. E-mailvoorkeuren kunnen alleen worden geconfigureerd voor een abonnement beleid. Resourcegroepen binnen een abonnement neemt deze instellingen over.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.  Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in het blad **aanbevelingen** **bieden beveiliging contactgegevens**.
![Bieden beveiliging-contact][1]

2. Hiermee opent u het blad **bieden beveiliging contactgegevens**. Selecteer het abonnement Azure voor contactgegevens op.
![Contactgegevens van beveiliging bieden][2]

3. Hiermee opent u een tweede **bieden beveiliging contactgegevens** blade.

  - Voer de beveiliging contact e-mailadres of de adressen van elkaar gescheiden door komma's. Er is een limiet aan het aantal e-mailadressen die u kunt invoeren.
  - Een security internationaal telefoonnummer invoeren.
  - Schakel de optie **Stuur me e-mails over waarschuwingen**ontvangen van e-mails over signalen met hoge prioriteit.
  - In de toekomst, hebt u de optie voor e-mailmeldingen verzenden aan eigenaren van abonnement. Deze optie is momenteel grijs weergegeven.
  - Klik op **OK** om de contactgegevens van de beveiliging van toepassing op uw abonnement.

## <a name="see-also"></a>Zie ook

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Security Center](security-center-recommendations.md) --informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Statuscontrole voor beveiliging in Azure Security Center](security-center-monitoring.md) --informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring van partneroplossingen met Azure Security Center](security-center-partner-solutions.md) --informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --laatste Azure beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
