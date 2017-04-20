<properties
    pageTitle="Logboek Analytics oplossingen toevoegen uit de galerie met oplossingen | Microsoft Azure"
    description="Logboek Analytics oplossingen zijn dat logica, visualisatie en data acquisition een verzameling regels die maatstaven gedraaid om een bepaald probleemgebied bieden."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="add-log-analytics-solutions-from-the-solutions-gallery"></a>Logboek Analytics oplossingen toevoegen uit de galerie met oplossingen

Logboek Analytics oplossingen zijn een verzameling van **logica**, **visualisatie** en **data acquisition regels** waarmee metrics gedraaid om een bepaald probleemgebied. In dit artikel lijsten oplossingen logboek Analytics ondersteund en wordt uitgelegd hoe toevoegen en verwijderen met behulp van de galerie met oplossingen.

Oplossingen toestaan meer inzicht aan:

- helpen bij het onderzoeken en oplossen van operationele problemen sneller
- verzamelen en het correleren van verschillende soorten machine-gegevens
- kunt u actie moet ondernemen met activiteiten zoals capaciteitsplanning, patch statusrapportage en een beveiligingscontrole.


>[AZURE.NOTE] Logboek Analytics bevat functionaliteit voor het logboek zoeken, zodat u niet hoeft te installeren zodat er een oplossing voor. U kunt echter data visualisaties, voorgestelde zoekopdrachten en inzichten opvragen door oplossingen toevoegen uit de galerie van oplossingen.

Nadat u een oplossing hebt toegevoegd, worden de gegevens verzameld van de servers in uw infrastructuur en verzonden naar de OMS-service. Verwerking door de OMS duurt service meestal een paar minuten tot een uur. Nadat u de service de gegevens verwerkt, kunt u deze weergeven in de OMS.

U kunt gemakkelijk een oplossing verwijderen wanneer het niet langer nodig is. Wanneer u een oplossing verwijdert, wordt de gegevens niet verzonden naar OMS, waardoor de hoeveelheid gegevens die wordt gebruikt door uw dagelijkse quotum verminderen kan als er een.


## <a name="solutions-supported-by-the-microsoft-monitoring-agent"></a>Ondersteund door de Microsoft Agent Monitoring oplossingen

Servers die zijn verbonden met Microsoft Monitoring Agent met OMS kunt op dit moment, de meeste van de oplossingen die beschikbaar zijn, met inbegrip van:

- Beoordeling van Active Directory
- Beheer van waarschuwingen (zonder waarschuwingen SCOM)
- Antimalware
- Wijzigingen bijhouden
- Beveiliging
- SQL-beoordeling
- Systeemupdates

De volgende oplossingen zijn echter *niet* ondersteund door Microsoft Monitoring Agent en System Center Operations Manager (SCOM)-agent vereist.

- Beheer van waarschuwingen (inclusief waarschuwingen SCOM)
- Beheer van de capaciteit
- Beoordeling van de configuratie

Raadpleeg [Operations Manager verbinding maken met Analytics logboek](log-analytics-om-agents.md) voor informatie over het aansluiten van de agent SCOM op logboek Analytics.

### <a name="to-add-a-solution-using-the-solutions-gallery"></a>Toevoegen van een oplossing met behulp van de galerie met oplossingen

1. Klik op de tegel **Galerie met oplossingen** op de pagina overzicht in OMS.    
    ![Galerie met oplossingen](./media/log-analytics-add-solutions/sol-gallery.png)
2. Informatie over de verschillende oplossingen beschikbaar op de pagina Galerie met OMS oplossingen. Klik op de naam van de oplossing die u wilt toevoegen aan OMS.
3. Gedetailleerde informatie over de oplossing is weergegeven op de pagina voor de oplossing die u hebt gekozen. Klik op **toevoegen**.
4. Een nieuwe tegel voor de oplossing die u hebt toegevoegd, wordt weergegeven in het overzicht pagina OMS en u kunt gaan gebruiken nadat de OMS-service uw gegevens verwerkt.

## <a name="to-configure-solutions"></a>Voor het configureren van oplossingen
1. U moet enkele oplossingen te configureren. U moet bijvoorbeeld automatisering, Azure Site herstellen en back-up configureren voordat u deze kunt gebruiken.
2. Een van deze oplossingen, klikt u op de tegel op de pagina overzicht.  
    ![oplossing configureren](./media/log-analytics-add-solutions/configure-additional.png)
3. Vervolgens configureert u de oplossing met de nodige informatie en klik vervolgens op **Opslaan**.  
    ![oplossing configureren](./media/log-analytics-add-solutions/configure.png)

### <a name="to-remove-a-solution-using-the-solutions-gallery"></a>Voor het verwijderen van een oplossing met behulp van de galerie met oplossingen

1. Klik op de tegel **Instellingen** op de pagina overzicht in OMS.
2. Op de pagina instellingen onder het tabblad oplossingen, klikt u op **verwijderen** voor de oplossing die u wilt verwijderen.
3. Klik in het bevestigingsvenster op **Ja** om het verwijderen van de oplossing.

## <a name="data-collection-details-for-oms-features-and-solutions"></a>Collectie detailgegevens voor OMS-functies en -oplossingen

In de volgende tabel bevat de methoden van de collectie gegevens en andere informatie over de manier waarop gegevens worden verzameld voor OMS-functies en -oplossingen. Directe agenten en SCOM agenten zijn in feite hetzelfde, maar de directe agent bevat aanvullende functionaliteit als u wilt dat deze verbinding maken met de werkruimte OMS en routeren via een proxy. Als u een SCOM-agent, moet deze als agent om te communiceren met OMS OMS worden gericht. SCOM agenten in deze tabel zijn OMS-agents die zijn verbonden met SCOM. Zie [Operations Manager verbinding maken met Analytics logboek](log-analytics-om-agents.md) voor informatie over het aansluiten van uw bestaande omgeving van SCOM op OMS.

>[AZURE.NOTE] Het type agent waarmee u bepaalt hoe gegevens worden verzonden naar OMS, aan de volgende voorwaarden:

- U de directe agent of gebruikt een agent OMS SCOM gekoppeld.
- Als SCOM vereist is, wordt SCOM agent gegevens voor de oplossing altijd verzonden naar OMS met de groep SCOM management. Bovendien als SCOM vereist is, wordt alleen de SCOM-agent gebruikt door de oplossing.
- Als SCOM is niet vereist in de tabel ziet dat SCOM agent worden verzonden naar OMS via de management-groep, wordt vervolgens SCOM agent gegevens altijd verzonden naar OMS beheer groepen. Directe agenten de management-groep overslaan en de gegevens rechtstreeks verzenden naar OMS.
- Wanneer SCOM agent niet via een management-groep verzonden worden, vervolgens worden de gegevens verzonden rechtstreeks naar OMS — zonder dat hiervoor de management-groep.


|gegevenstype| platform | Directe Agent | SCOM agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | frequentie van verzameling |
|---|---|---|---|---|---|---|---|
|Beoordeling van AD|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|  7 dagen|
|De replicatiestatus AD|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 dagen|
|Waarschuwingen (Nagios)|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|bij aankomst|
|Waarschuwingen (Zabbix)|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|1 minuut|
|Waarschuwingen (Operations Manager)|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|3 minuten|
|Antimalware|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| per uur|
|Beheer van de capaciteit|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| per uur|
|Wijzigingen bijhouden|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| per uur|
|Wijzigingen bijhouden|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|per uur|
|Beoordeling van de configuratie (oude Advisor)|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| twee keer per dag|
|ETW|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minuten|
|IIS-logboeken|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minuten|
|Sleutel kluizen|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuten|
|Toepassingsgateways netwerk|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuten|
|Netwerk-beveiligingsgroepen|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuten|
|Office 365|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|op de kennisgeving|
|Prestatiemeteritems|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|zoals gepland, minimaal 10 seconden|
|Prestatiemeteritems|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|zoals gepland, minimaal 10 seconden|
|Service-Fabric|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minuten|
|SQL-beoordeling|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| 7 dagen|
|SurfaceHub|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|bij aankomst|
|Syslog|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|uit de opslag voor Azure: 10 minuten; van agent: bij aankomst|
|Systeemupdates|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| ten minste 2 maal per dag en 15 minuten na het installeren van een update|
|Windows-beveiligingslogboek|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)| voor de opslag van Azure: 10 minuten; de agent: bij aankomst|
|Windows firewall, Logboeken|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)| bij aankomst|
|Windows-gebeurtenislogboeken|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| voor de opslag van Azure: 1 min; de agent: bij aankomst|
|Wire-gegevens|Windows (2012 R2 / 8.1 of hoger)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)| elke minuut 1|

## <a name="log-analytics-preview-solutions-and-features"></a>Logboek Analytics voorbeeld oplossingen en functies

Door een service wordt uitgevoerd en devops procedures volgen kunnen we samenwerken met klanten om functies en oplossingen te ontwikkelen.

Tijdens de persoonlijke voorbeeld geven wij een kleine groep klanten toegang tot een snelle tenuitvoerlegging van de functie of oplossing feedback krijgen en verbeteringen. Deze snelle tenuitvoerlegging heeft minimale functies en operationele mogelijkheden.

Ons doel is om te proberen dingen snel zodat we vinden kunnen wat werkt en wat niet werkt. We doorlopen dit proces totdat de feedback van de klanten persoonlijke voorbeeld ons informeert dat we klaar zijn voor een openbare preview.

Tijdens de public preview maken we de functie of oplossing beschikbaar voor alle gebruikers meer feedback krijgen en valideren van onze schalen en efficiëntie. Tijdens deze fase:

- Voorbeeld-functies weergegeven op het tabblad instellingen en kunnen worden ingeschakeld door de gebruiker
- Voorbeeld-oplossingen kunnen worden toegevoegd door de galerie of het gebruik van een gepubliceerde script

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Wat moet ik weten over de functies van de voorbeeldweergave en oplossingen

Wij zijn verheugd over de nieuwe functies en oplossingen en we werken met u ontwikkelen ze graag.

Voorbeeld functies en oplossingen zijn niet geschikt voor iedereen, dus voordat gevraagd deel te nemen aan een voorbeeld van persoonlijke of een openbare preview in te schakelen moet u OK werkt met iets dat in ontwikkeling.

Wanneer u een functie via de portal die u inschakelt waarschuwing herinneren dat de functie Afdrukvoorbeeld wordt weergegeven.

#### <a name="for-both-private-and-public-preview"></a>Voor zowel *particuliere* als *openbare* preview

Het volgende is van toepassing op openbare en particuliere voorbeelden:

- Dingen kunnen niet altijd goed.
  - Bereik worden lichte ergernis veroorzaken via in iets dat niet werkt op alle problemen
- Er is kans op het voorbeeld om een negatieve invloed hebben op uw systemen / omgeving
  - We proberen te voorkomen dat negatieve dingen gebeurt op de systemen, maar u gebruikt in combinatie met OMS gebeuren soms onverwachte dingen
- Verlies van gegevens / beschadigd kan raken.
- We kunnen verzoeken u diagnostische logboeken of andere gegevens voor het oplossen van problemen verzamelen
- De functie of oplossing kan worden verwijderd (tijdelijk of permanent)
  - Op basis van onze geleerde lessen tijdens de voorvertoning die we besluiten niet vrij functie of oplossing
- Voorbeelden werken mogelijk niet of niet getest met alle configuraties en we kunnen beperken:
  - De besturingssystemen die kunnen worden gebruikt (bijvoorbeeld een functie kan alleen van toepassing op Linux in de voorvertoning)
  - Het type van de agent (MMA, SCOM) die kan worden gebruikt (bijvoorbeeld een functie werkt mogelijk niet met SCOM in de voorvertoning)  
- Voorbeeld-oplossingen en functies vallen niet de Service Level Agreement
- Gebruik van voorvertoning voorzieningen gelden de tarieven en gebruik
- Functies of mogelijkheden die u nodig hebt voor de functie / oplossing zijn om de mogelijk ontbreken of onvolledig
- Functies / oplossingen mogelijk niet beschikbaar in alle regio's
- Functies / oplossingen kunnen niet worden gelokaliseerd.
- Functies / oplossingen mogelijk een limiet op het aantal klanten of apparaten die u kunnen gebruiken
- Moet u scripts gebruiken voor het uitvoeren van de configuratie en de oplossing/functie in te schakelen
- De gebruikersinterface (UI) wordt niet volledig en kan van dag tot dag wijzigen
- Openbare voorbeelden zijn mogelijk niet geschikt is voor uw productie / kritieke systemen

#### <a name="for-private-preview"></a>Voor *particuliere* preview

Naast de genoemde items volgt specifieke persoonlijke voorbeelden:

- We verwachten u bieden ons feedback over uw ervaringen, zodat we van de functie of oplossing verbeteren kunnen
- We kunnen contact met u opnemen voor enquêtes, telefoongesprekken of e-mail met feedback
- Dingen werken niet altijd correct
- We verlangen voor deelname aan een Non-Disclosure overeenkomst (NDA) of vertrouwelijke inhoud kan bevatten
  - Controleer voordat u bloggen, tweeting of anderszins aan derden communiceert met de Program Manager die verantwoordelijk is voor de voorvertoning te begrijpen eventuele beperkingen inzake het vrijgeven
- Niet worden uitgevoerd op productie / kritieke systemen


### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Hoe krijg ik toegang tot persoonlijke voorbeeld functies en oplossingen

We nodigen klanten persoonlijke voorbeelden via verschillende manieren afhankelijk van het voorbeeld.

- De maandelijkse enquête te beantwoorden en ons toestemming om contact met u geven verbetert uw kans om te worden uitgenodigd voor een persoonlijk voorbeeld.
- Uw accountteam van Microsoft kunt u benoemen.
- U kunt zich aanmelden op basis van details gepost op twitter [msopsmgmt](https://twitter.com/msopsmgmt)
- U kunt zich aanmelden op basis van details gedeelde community gebeurtenissen – Bekijk voor ons voldoen aan ups, conferenties en online Gemeenschappen.


## <a name="next-steps"></a>Volgende stappen

- [Zoeken in Logboeken](log-analytics-log-searches.md) voor gedetailleerde informatie verzameld door oplossingen te bekijken.
