<properties
   pageTitle="Operations Management Suite beveiliging en controle oplossing basislijn | Microsoft Azure"
   description="Dit document wordt uitgelegd hoe u OMS beveiliging en controle-oplossing voor het uitvoeren van een evaluatie van de basislijn van alle gecontroleerde computers voor naleving van regelgeving en veiligheid doel."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="yurid"/>

# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Beoordeling van de basislijn in het Operations Management Suite beveiliging en controle-oplossing

Dit document helpt u bij het [Operations Management Suite (OMS) beveiliging en controle oplossing](operations-management-suite-overview.md) basislijn beoordeling van mogelijkheden voor toegang tot de beveiligingsstatus van uw gecontroleerde bronnen gebruiken.

## <a name="what-is-baseline-assessment"></a>Wat is de beoordeling van de basislijn?

Microsoft, definieert samen met de industrie en de overheid organisaties over de hele wereld, u een zeer veilige serverimplementaties geeft Windows-configuratie. Deze configuratie is een set registersleutels, instellingen voor controlebeleid en instellingen voor het beveiligingsbeleid en de aanbevolen waarden voor deze instellingen van Microsoft. Deze reeks regels staat bekend als de beveiliging. OMS beveiliging en controle basislijn beoordeling mogelijkheid kunt naadloos scannen al uw computers om te voldoen. 

Er zijn drie soorten regels:

- **Register regels**: Controleer of de registersleutels juist zijn ingesteld.
- **Controlebeleidsregels**: regels met betrekking tot uw controlebeleid.
- **Beleidsregels voor beveiliging**: regels met betrekking tot de machtigingen van de gebruiker op de computer.

> [AZURE.NOTE] [Gebruik OMS beveiliging voor de beoordeling van de basisbeveiliging configuratie](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) lezen voor een kort overzicht van deze functie.

## <a name="security-baseline-assessment"></a>Beoordeling van de veiligheid volgens basislijn

U kunt uw huidige beoordeling van de veiligheid volgens de basislijn voor alle computers die worden gecontroleerd door OMS beveiliging en controle via het dashboard kunt bekijken.  Voer de volgende stappen uit om toegang tot de dashboard security baseline beoordeling:

1. Klik op **beveiliging en controle** naast elkaar in het belangrijkste dashboard **Microsoft Operations Management Suite** .
2. Klik op **Basislijn beoordeling** onder **Beveiligingsdomeinen**in het dashboard **beveiliging en controle** . Het dashboard van de **Veiligheidsbeoordeling van de basislijn** wordt weergegeven in de volgende afbeelding:
    
    ![OMS beveiliging en controle basislijn beoordeling](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Dit dashboard is onderverdeeld in drie belangrijke gebieden:

- **Computers in vergelijking met basislijn**: deze sectie bevat een samenvatting van het aantal computers die zijn geopend en het percentage van de computers die de beoordeling wordt doorgegeven. Het biedt ook de top 10 computers en het resultaat van het percentage voor de beoordeling.
- **Vereiste regels Status**: in dit gedeelte heeft de intentie om bewuster te maken van de regels is mislukt door ernst en regels per type is mislukt. Door te kijken naar de eerste grafiek kunt u snel kunt nagaan of de meeste mislukte regels zijn van cruciaal belang is, of niet. Het biedt ook een lijst van de top 10 mislukte regels en de ernst. De tweede grafiek geeft het type regel dat is mislukt tijdens de beoordeling. 
- **Computers basislijn beoordeling ontbreekt**: deze sectie lijst met computers die niet zijn geopend als gevolg van incompatibiliteit van het besturingssysteem of storingen. 

### <a name="accessing-computers-compared-to-baseline"></a>Toegang tot computers in vergelijking met basislijn

In het ideale geval worden al uw computers worden in overeenstemming met de veiligheidsbeoordeling van de basislijn. Echter verwacht wordt dat in sommige dit omstandigheden gebeurt niet. Als onderdeel van het proces voor beveiliging is het belangrijk om op te nemen met het controleren van de computers die niet door te geven van alle security assessment tests. Er is een snelle manier om te visualiseren die door de optie **Computers toegankelijk** vindt u in de sectie **Computers vergeleken met de basislijn** . Hier ziet u het zoekresultaat voor logboek met de lijst met computers, zoals wordt getoond in het volgende scherm:

![Toegang tot computer-resultaten](./media/oms-security-baseline/oms-security-baseline-fig2.png)

De zoekresultaten wordt weergegeven in een tabel, waarbij de eerste kolom bevat de naam van de computer en de tweede kleur is het aantal regels dat is mislukt. Klik in het aantal mislukte regels naast de naam van de computer voor het ophalen van de gegevens met betrekking tot het type regel dat is mislukt. Hier ziet u een resultaat vergelijkbaar met die in de volgende afbeelding:

![Toegang tot computer resultaten details](./media/oms-security-baseline/oms-security-baseline-fig3.png)

In deze zoekresultaten hebt u het totaal van de gebruikte regels, het aantal essentiële regels die niet, de waarschuwingsregels en de regels met gegevens is mislukt.

### <a name="accessing-required-rules-status"></a>Toegang tot de status van de vereiste regels

Na het verkrijgen van de gegevens met betrekking tot het percentage aantal computers die de beoordeling wordt doorgegeven, kunt u meer informatie over welke regels volgens de belangrijkheid mislukken. Deze visualisatie helpt u te bepalen welke computers moeten eerst worden behandeld om ervoor te zorgen dat zij zijn compatibel met de volgende beoordeling. Plaats de muisaanwijzer op een essentieel onderdeel van de grafiek zich bevindt in de tegel **kan geen regels op volgorde van prioriteit** onder de **vereiste status van regels** en klik erop. Hier ziet u een resultaat vergelijkbaar met het volgende scherm:

![Kan geen regels door ernst details](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

In dit logboek-resultaat ziet u het type regel basislijn die niet de beschrijving van deze regel en de ID van de algemene configuratie opsomming (CCE) van deze regel. Deze kenmerken moeten voldoende zijn voor een corrigerende actie u kunt dit probleem op de doelcomputer.

> [AZURE.NOTE] Toegang tot de [Nationale beveiligingslek Database](https://nvd.nist.gov/cce/index.cfm)voor meer informatie over het CCE.

### <a name="accessing-computers-missing-baseline-assessment"></a>Toegang tot computers basislijn beoordeling ontbreekt

Het profiel domein basislijn ondersteunt OMS op Windows Server 2008 R2 naar Windows Server 2012 R2. Windows Server 2016 basislijn nog niet klaar en zal worden toegevoegd zodra deze is gepubliceerd. Alle andere besturingssystemen gescand via OMS beveiliging en controle basislijn beoordeling weergegeven onder de sectie **Computers basislijn beoordeling ontbreekt** .

## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd over de beoordeling van de basislijn OMS beveiliging en controle. Voor meer informatie over de beveiliging van OMS, Zie de volgende artikelen:

- [Operations Management Suite (OMS)-overzicht](operations-management-suite-overview.md)
- [Toezicht en het reageren op de beveiligingswaarschuwingen in Operations Management Suite beveiliging en controle-oplossing](oms-security-responding-alerts.md)
- [Resources controleren in Operations Management Suite beveiliging en controle-oplossing](oms-security-monitoring-resources.md)

