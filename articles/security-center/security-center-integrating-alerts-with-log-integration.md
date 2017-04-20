<properties
   pageTitle="Meldingen van Beveiligingscentrum Azure integreren met integratie van de Azure logboek (voorbeeld) | Microsoft Azure"
   description="Dit artikel helpt u aan de slag met meldingen van Beveiligingscentrum integreren met integratie van de Azure logboek."
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
   ms.date="08/08/2016"
   ms.author="terrylan"/>

# <a name="integrating-security-center-alerts-with-azure-log-integration-preview"></a>Meldingen van Beveiligingscentrum integreren met integratie van de Azure logboek (voorbeeld)

Veel beveiligingsbewerkingen en incident response teams kunt u vertrouwen op een oplossing voor informatie over beveiliging en gebeurtenis Management (SIEM) als uitgangspunt voor gesorteerd en beveiligingswaarschuwingen te onderzoeken. Klanten kunnen met integratie van de Azure logboek, Azure Security Center-waarschuwingen, samen met de virtuele machine beveiligingsgebeurtenissen verzameld door Azure diagnostiek en Azure controlelogboeken, met hun logboek analytics of SIEM oplossing in bijna real-time worden gesynchroniseerd.

Integratie van de Azure logboek werkt met HP ArcSight, Splunk, IBM QRadar en anderen.

## <a name="what-logs-can-i-integrate"></a>Welke logboeken kan ik integreren?

Azure levert uitgebreide logboekregistratie voor elke service. Deze logboeken worden gecategoriseerd als:

- **Logboeken** beheren/die geven inzicht in de activiteiten van Azure Resource Manager maken, bijwerken en verwijderen.
- **Gegevens vlak logboeken** die geven inzicht in de gebeurtenissen bij het gebruik van een resource Azure. Een voorbeeld is het gebeurtenislogboek Windows - beveiliging en toepassing meldt zich in een virtuele machine.

Integratie van de Azure logboek ondersteunt momenteel de integratie van:

- Logboeken voor Azure VM
- Azure controlelogboeken
- Meldingen van Beveiligingscentrum Azure

## <a name="install-azure-log-integration"></a>Integratie van de Azure logboek installeren

Download [Azure integratie melden](https://www.microsoft.com/download/details.aspx?id=53324).

Integratie van de Azure logboekservice verzamelt telemetriegegevens van de computer waarop Netwerkcontrole is geïnstalleerd.  Telemetriegegevens verzameld zijn:

- Uitzonderingen die zich tijdens het uitvoeren van de integratie van de Azure logboek voordoen
- Statistieken over het aantal query's en verwerkte gebeurtenissen
- Statistieken over welke Azlog.exe opdrachtregelopties worden gebruikt

> [AZURE.NOTE] U kunt verzameling telemetriegegevens door het uitschakelen van deze optie uitschakelen.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Audit- en Beveiligingscentrum Azure waarschuwingen integreren

1. Open de opdrachtprompt en **cd** in **c:\Program Files\Microsoft Azure logboek integratie**.

2. Voer de opdracht **azlog createazureid** voor het maken van een [Azure Active Directory Service Principal](../active-directory/active-directory-application-objects.md) in de huurder Azure Active Directory (AD) waarop de Azure abonnementen.

    U wordt gevraagd uw aanmelding Azure.

    > [AZURE.NOTE] U moet het abonnement eigenaar of beheerder van een collega van het abonnement.

    Azure-verificatie vindt plaats via Azure AD.  Maken van een service principal voor integratie van de Azure logboek maakt de Azure AD identiteit die toegang tot het lezen van Azure abonnementen wordt gegeven.

3. Uitvoeren van de **azlog toestaan <SubscriptionID> ** opdracht lezerstoegang op het abonnement toewijzen aan de service principal gemaakt in stap 2. Als u een **SubscriptionID**opgeeft, wordt de service principal toegewezen de rol van lezer voor alle abonnementen waartoe u toegang hebt.

    > [AZURE.NOTE] U kunt waarschuwingen kan zien als u de opdracht **toestaan dat** onmiddellijk na de opdracht **createazureid** omdat er een vertraging tussen wanneer de Azure AD-account is gemaakt en wanneer de account is beschikbaar voor gebruik. Als u ongeveer 10 seconden wacht na het uitvoeren van de opdracht **createazureid** uitvoeren van de opdracht **toestaan** , moet u deze waarschuwingen niet ziet.

4. Controleer de volgende mappen om te bevestigen dat de controlelogboekbestanden voor JSON er zijn:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Controleer de volgende mappen om te bevestigen dat de meldingen van Beveiligingscentrum erin bestaat:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Wijs de standaard SIEM bestand doorstuurserver verbindingslijn naar de juiste map naar de gegevens naar het exemplaar SIEM pipe. Zie [SIEM configuraties](https://azsiempublicdrops.blob.core.windows.net/drops/ALL.htm) van uw configuratie SIEM.

Als u vragen over Azure logboek integratie hebt, stuur een e-mail naar [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Audit- en eigenschapdefinities Azure, Zie:

- [Bewerkingen met Resource Manager controleren](../resource-group-audit.md)
- Een [overzicht van de gebeurtenissen in een abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx) - controlegebeurtenissen logboek ophalen.

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- [Beheren en reageren op de beveiligingswaarschuwingen in Beveiligingscentrum Azure](security-center-managing-and-responding-alerts.md) , informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Veelgestelde vragen over het Azure Security Center](security-center-faq.md) , zoeken, veelgestelde vragen over het gebruik van de service.
- [Beveiliging Azure blog](http://blogs.msdn.com/b/azuresecurity/) , de meest recente Azure beveiliging nieuws en informatie.
