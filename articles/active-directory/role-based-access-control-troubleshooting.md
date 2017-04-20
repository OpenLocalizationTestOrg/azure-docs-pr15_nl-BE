<properties
    pageTitle="Op basis van rollen access control troubleshooting | Microsoft Azure"
    description="Hulp bij problemen of vragen hebt over de rollen gebaseerd toegangsbeheer."
    services="azure-portal"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="kgremban"/>

# <a name="role-based-access-control-troubleshooting"></a>Role-Based Access Control problemen oplossen

## <a name="introduction"></a>Inleiding

[Role-Based Access Control](role-based-access-control-configure.md) is een krachtige functie waarmee u verfijnde toegang tot bronnen in Azure delegeren. Dit betekent dat u kunt erop vertrouwen toekenning van een bepaalde persoon het recht op precies wat ze nodig hebben en niet meer. Echter kan soms het model van de resource voor Azure resources ingewikkeld en kan het moeilijk te begrijpen precies wat u verleent machtigingen zijn.

Dit document kunt u weet wat u kunt verwachten als u enkele van de rollen in de Azure portal. Deze drie rollen hebben betrekking op alle typen:

- Eigenaar  
- Inzender  
- Reader  

Eigenaars en medewerkers hebben volledige toegang tot de management ervaring, maar een Inzender toegang kan verlenen aan andere gebruikers of groepen. Dingen krijgen iets interessanter met de rol van lezer, zodat waar we enige tijd zult besteden. Zie het [artikel van op rollen gebaseerde toegangscontrole get started](role-based-access-control-configure.md) voor meer informatie over het verlenen van toegang.

## <a name="app-service-workloads"></a>Werkbelasting van App-service

### <a name="write-access-capabilities"></a>Schrijftoegang-mogelijkheden

Als u een gebruiker alleen-lezen toegang tot een enkel web app geven, worden sommige functies uitgeschakeld dat u niet zou verwachten. De volgende mogelijkheden voor beheer vereist **schrijven** toegang tot een web app (Inzender of eigenaar) en niet beschikbaar zijn in elk scenario alleen-lezen.

- Opdrachten (zoals starten, stoppen, enz.)
- Wijzigen van de instellingen, zoals algemene configuratie, schaalinstellingen voor, back-upinstellingen en controle-instellingen
- Toegang tot referenties publishing en andere geheimen zoals app instellingen en verbindingsreeksen
- Logboeken voor gegevensstromen
- Diagnoselogboeken configuratie
- Console (opdrachtprompt)
- Actieve en recente implementaties (voor lokale git continuous deployment)
- Geschatte uitgaven
- Web-tests
- Virtueel netwerk (alleen zichtbaar voor een lezer als eerder een virtueel netwerk is geconfigureerd door een gebruiker met schrijfrechten).

Als u geen toegang deze tegels tot, moet u de systeembeheerder voor de Inzender toegang tot de web app.

### <a name="dealing-with-related-resources"></a>Omgaan met verwante bronnen

Web apps worden bemoeilijkt door de aanwezigheid van een aantal verschillende bronnen die wisselwerking. Hier is een typische bronnengroep met een paar websites:

![Web app resourcegroep](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Als resultaat, als u iemand toegang tot alleen de web app, veel van de functionaliteit van de website blade in Azure portal wordt uitgeschakeld.

Deze artikelen **schrijven** toegang nodig tot de **App serviceplan** dat overeenkomt met uw website:  

- De web app bekijken de prijzen laag (vrije of standaard)  
- Schaal configuratie (aantal exemplaren, de grootte van de virtuele machine, de instellingen automatisch schalen)  
- Quota (opslag, bandbreedte, CPU)  

Deze artikelen **schrijven** toegang nodig tot de hele **groep van bronnen** die uw website bevat:  

- SSL-certificaten en bindingen (dit is omdat SSL-certificaten kunnen worden gedeeld tussen sites in dezelfde bronnengroep en geo-locatie)  
- Waarschuwingsregels  
- Instellingen voor automatisch schalen  
- Inzichten toepassingsonderdelen  
- Web-tests  

## <a name="virtual-machine-workloads"></a>Werkbelasting van de virtuele machine

Veel zoals met web apps, sommige functies op de virtuele machine blade vereist schrijftoegang voor de virtuele machine of aan andere resources in de resourcegroep.

Virtuele machines zijn gerelateerd aan het domein namen, virtuele netwerken opslag rekeningen en regels.

Deze artikelen **schrijven** toegang nodig tot de **virtuele machine**:

- Eindpunten  
- IP-adressen  
- Schijven  
- Extensies  

Deze vereist **schrijven** toegang tot de **virtuele machine**en de **resourcegroep** (in combinatie met de naam van het domein) die is:  

- Beschikbaarheid instellen  
- Evenwichtige set laden  
- Waarschuwingsregels  

Als u geen toegang deze tegels tot, moet u de systeembeheerder voor de Inzender toegang tot de resourcegroep.

## <a name="see-more"></a>Zie voor meer informatie
- [Rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md): aan de slag met RBAC in Azure portal.
- [Ingebouwde functies](role-based-access-built-in-roles.md): informatie over de rollen die standaard in RBAC.
- [Aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md): informatie over het maken van aangepaste rollen aanpassen aan uw behoeften toegang.
- [Maken van een rapport wijzigen](role-based-access-control-access-change-history-report.md): bijhouden van veranderende roltoewijzingen in RBAC.
