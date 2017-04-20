<properties
   pageTitle="Toezicht en het reageren op de beveiligingswaarschuwingen in Operations Management Suite beveiliging en controle oplossing | Microsoft Azure"
   description="Dit document kunt u de optie te gebruiken threat intelligence OMS beveiliging en controle om te controleren en te reageren op de beveiligingswaarschuwingen."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>Toezicht en het reageren op de beveiligingswaarschuwingen in Operations Management Suite beveiliging en controle-oplossing

Dit document helpt u bij het gebruik threat intelligence OMS beveiliging en controle om te controleren en te reageren op de beveiligingswaarschuwingen.

## <a name="what-is-oms"></a>Wat is de OMS?

Microsoft Operations Management Suite (OMS) is van Microsoft cloud gebaseerde IT management-oplossing waarmee u beheren kunt en beveiligen van uw locatie op en cloud infrastructuur. Lees het artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx)voor meer informatie over het Kantoorbeheersysteem.

## <a name="threat-intelligence"></a>Threat intelligence

In een bedrijfsomgeving waar gebruikers brede toegang hebben tot het netwerk en een verscheidenheid van apparaten verbinding maken met zakelijke gegevens gebruiken, is het noodzakelijk uw resources kan controleren en snel reageren op beveiligingsincidenten. Dit is bijzonder belangrijk vanuit het perspectief van de levenscyclus van beveiliging, omdat sommige bedreigingen kunnen niet verhogen cybersecurity waarschuwingen of verdachte activiteiten die door de traditionele beveiliging technische controles kunnen worden geïdentificeerd. 

Met behulp van de optie **Threat Intelligence** OMS beveiliging en controle, IT-beheerders identificeren van bedreigingen van het milieu, bijvoorbeeld, als een bepaalde computer deel uitmaakt van een [botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection)identificeren. Computers kunnen knooppunten in een botnet worden wanneer aanvallers malware die heimelijk deze computer met de opdracht en de verbindt vastlegt installeert. Het kan ook worden aangegeven mogelijke dreigingen afkomstig van ondergrondse communicatiekanalen zoals [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents). 

Gebruik gegevens die afkomstig zijn uit meerdere bronnen binnen Microsoft om te bouwen van deze bedreiging intelligence, OMS beveiliging en controle. OMS beveiliging en controle maken gebruik van deze gegevens ter identificatie van mogelijke bedreigingen van uw omgeving.

Het deelvenster Threat Intelligence is samengesteld door drie belangrijke opties:
- Servers met schadelijke het uitgaande verkeer
- Gedetecteerde bedreigingen typen
- Threat intelligence-kaart

> [AZURE.NOTE] lezen [aan de slag met Operations Management Suite beveiliging en controle-oplossing](oms-security-getting-started.md)voor een overzicht van al deze opties.

### <a name="responding-to-security-alerts"></a>Reageren op de beveiligingswaarschuwingen

Een van de stappen van een proces [security incident response](https://technet.microsoft.com/library/cc512623.aspx) is de ernst van de inbreuk systemen te identificeren. In deze fase moet u de volgende taken uitvoeren:

- De aard van de aanval vaststellen
- Bepalen van het punt van de aanval van oorsprong
- Bepaal het doel van de aanval. Was de aanval speciaal op uw organisatie gericht om specifieke informatie te verwerven, of was het een willekeurige?
- Identificatie van de systemen in gevaar zijn gebracht
- De bestanden die zijn geopend en het bepalen van de gevoeligheid van deze bestanden identificeren

U kunt gebruikmaken van **Threat Intelligence** -informatie van OMS beveiliging en controle oplossing om te helpen bij deze taken. Volg de stappen hieronder om naar deze **Threat Intelligence** -opties:

1. Klik op **beveiliging en controle** naast elkaar in het belangrijkste dashboard **Microsoft Operations Management Suite** .

    ![Beveiliging en controle](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. In het dashboard **beveiliging en controle** ziet u de opties **Threat Intelligence** in het recht, zoals hieronder wordt weergegeven:

    ![Intel bedreiging](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Deze drie tegels geven een overzicht van de huidige bedreigingen. Op de **Server met schadelijke het uitgaande verkeer** die is het mogelijk om aan te duiden als elke computer die u controleren wilt (binnen of buiten uw netwerk) stuurt die schadelijk verkeer op Internet. 

De tegel **gedetecteerd bedreiging typen** bevat een overzicht van de bedreigingen voor huidige 'in het wild zijn', als u klikt op deze tegel ziet u meer details over deze bedreigingen zoals u hierna ziet:

![Gedetecteerde bedreiging typen](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

U kunt meer informatie over elke bedreiging uitpakken door erop te klikken. In het volgende voorbeeld ziet u meer details over Botnet:

![meer informatie over een bedreiging](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Zoals in het begin van deze sectie wordt beschreven, is deze informatie met name handig in het geval van een beveiligingsplan. Het kan ook van belang zijn tijdens een forensisch onderzoek waar u wilt zoeken naar de bron van de aanval, welk systeem is ingebroken en de tijdlijn. In dit rapport kunt u gemakkelijk identificeren enkele belangrijke details over de aanval, zoals: de bron van de aanval, het lokale IP-dat is beschadigd en de huidige status van de verbinding. 

De **threat intelligence-kaart** kunt u de huidige locaties over de hele wereld die schadelijk verkeer hebben. Er zijn oranje (binnenkomende) en rood (uitgaand) pijlen in dit overzicht waarmee de richting verkeer als u in een van de pijlen te klikken, het type bedreiging en het verkeer richting zoals hieronder wordt het weergegeven:

![bedreiging intel-kaart](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [AZURE.NOTE] U kunt een demonstratie zien over het gebruik van deze mogelijkheid tijdens een incident antwoord proces volgen de presentatie [Mitigate datacenter beveiligingsrisico's met begeleide onderzoek met behulp van Operations Management Suite](https://myignite.microsoft.com/videos/5000) wordt geleverd bij Microsoft Ignite.

## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe u met de optie **Threat Intelligence** in OMS beveiliging en controle oplossing om te reageren op de beveiligingswaarschuwingen. Voor meer informatie over de beveiliging van OMS, Zie de volgende artikelen:

- [Operations Management Suite (OMS)-overzicht](operations-management-suite-overview.md)
- [Aan de slag met Operations Management Suite beveiliging en controle-oplossing](oms-security-getting-started.md)
- [Resources controleren in Operations Management Suite beveiliging en controle-oplossing](oms-security-monitoring-resources.md)
