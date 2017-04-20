<properties
   pageTitle="Inleiding tot Microsoft Azure logboek integratie | Microsoft Azure"
   description="Meer informatie over de integratie van de Azure logboek, de belangrijkste mogelijkheden en hoe het werkt."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="introduction-to-microsoft-azure-log-integration-preview"></a>Inleiding tot Microsoft Azure logboek integratie (voorbeeld)

Meer informatie over de integratie van de Azure logboek, de belangrijkste mogelijkheden en hoe het werkt.

## <a name="overview"></a>Overzicht

Platform als een Service (PaaS) en Infrastructure als Service (IaaS) dat wordt gehost in Azure genereren een grote hoeveelheid gegevens in beveiligingslogboeken. Deze logboeken bevatten belangrijke informatie die intelligentie en krachtige inzichten overtredingen van het beleid, interne en externe bedreigingen, problemen van de naleving van regelgeving en anomalieën in het netwerk, host en gebruikersactiviteiten.

Integratie van de Azure logboek kunt u ruwe logboeken van uw resources Azure integreren in uw systemen van beveiligingsinformatie en gebeurtenis Management (SIEM) in ruimten. Azure logboek integratie Azure diagnostische gegevens verzamelt van uw vensters *(af)* virtuele machines, alsmede de diagnostische gegevens van de oplossingen van partners zoals een Web Application Firewall (WAF). Deze integratie garandeert een consistente dashboard voor alle activa, in ruimten of in de cloud, zodat verzamelen, correleren, analyseren en voor beveiligingsgebeurtenissen waarschuwen.

![Integratie van de Azure logboek][1]

## <a name="what-logs-can-i-integrate"></a>Welke logboeken kan ik integreren?

Azure levert uitgebreide logboekregistratie voor elke Azure service. Deze logboeken worden gecategoriseerd door twee hoofdtypen:

- **Beheren/logs**, die geven inzicht in de activiteiten van Azure Resource Manager maken, bijwerken en verwijderen. Controlelogboeken Azure is een voorbeeld van dit type logboek.
- **Gegevenslogboeken vlak**, die geven inzicht in de gebeurtenissen die optreedt als onderdeel van het gebruik van een resource Azure. Voorbeelden van dit type van het logboek zijn het Windows-gebeurtenislogboek systeem, beveiliging en toepassing worden vastgelegd in een virtuele machine.

Integratie van de Azure logboek ondersteunt momenteel de integratie van de Azure controlelogboeken, virtuele machine logboeken en waarschuwingen van Beveiligingscentrum Azure.

Als u vragen over Azure logboek integratie hebt, stuur een e-mail naar [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Volgende stappen

In dit document, kunt u kennisgemaakt met Azure logboek integratie. Voor meer informatie over de integratie van de Azure logboek en de typen logboeken wordt ondersteund, raadpleegt u het volgende:

- [Integratie van Microsoft Azure logboek voor Azure Logboeken (voorbeeld)](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center voor details, systeemvereisten en installatie-instructies voor de Azure logboek integratie.
- [Aan de slag met Azure logboek integratie](security-azure-log-integration-get-started.md) - deze zelfstudie doorloopt u installatie van Azure logboek integratie en integratie van Logboeken van Azure opslag, Azure controlelogboeken en meldingen van Beveiligingscentrum.
- [Partner configuratiestappen](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – dit blogbericht wordt beschreven hoe u integratie werken met oplossingen van partners Splunk ArcSight HP en IBM QRadar Azure logboekbestand configureren.
- Antwoorden op vragen over de integratie van de Azure logboek [azure logboek integratie vaak gestelde vragen (FAQ)](security-azure-log-integration-faq.md) - deze Veelgestelde vragen.
- [Waarschuwingen van Beveiligingscentrum integratie met Azure melden integratie](../security-center/security-center-integrating-alerts-with-log-integration.md) – dit document wordt beschreven hoe u meldingen van Beveiligingscentrum, samen met de virtuele machine beveiligingsgebeurtenissen verzameld door Azure diagnostiek en Azure controlelogboeken, met uw analytics logboek of SIEM oplossing synchroniseren.
- [Nieuwe functies voor Azure diagnostiek en Azure controlelogboeken](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – dit blogbericht maakt u kennis met Azure controlelogboeken en andere functies die u helpen om inzicht te krijgen in de activiteiten van uw Azure bronnen krijgen.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
