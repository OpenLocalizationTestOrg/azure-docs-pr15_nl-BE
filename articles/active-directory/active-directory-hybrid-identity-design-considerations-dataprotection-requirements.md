<properties
    pageTitle="Azure Active Directory hybride identiteit overwegingen bij het ontwerp - eisen inzake de bescherming van gegevens bepalen | Microsoft Azure"
    description="Bij het plannen van uw identiteit hybride oplossing identificeren de vereiste gegevens bescherming voor uw bedrijf en welke opties zijn beschikbaar voor het beste aan deze eisen voldoen."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

#<a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Plan voor het verbeteren van de beveiliging van gegevens door middel van sterke identiteit oplossing

De eerste stap bij het beveiligen van de gegevens is bepalen wie toegang heeft tot die gegevens en als onderdeel van dit proces moet u een identiteit hebt oplossing die kan worden geïntegreerd met mogelijkheden voor verificatie en autorisatie van uw systeem. Verificatie en autorisatie worden vaak verward met elkaar en hun rollen verkeerd begrepen. In werkelijkheid gaat heel anders, zoals in de onderstaande afbeelding:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)
 
**Fasen van hun levenscyclus beheer mobiel apparaat**

Bij het plannen van uw identiteit hybride oplossing moet u de vereiste gegevens bescherming voor uw bedrijf en welke opties zijn beschikbaar voor het beste voldoen aan deze vereisten begrijpen.
 
>[AZURE.NOTE]
Zodra u klaar bent met het plannen van de beveiliging van gegevens, Bekijk [meerledige verificatie eisen vast](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) om ervoor te zorgen dat uw selecties met betrekking tot een meerledige verificatie eisen niet werden beïnvloed door de beslissingen in deze sectie.

## <a name="determine-data-protection-requirements"></a>Bepalen van vereisten voor de bescherming van gegevens
In het tijdperk van mobiliteit, de meeste bedrijven hebben een gemeenschappelijk doel: hun gebruikers productiever op hun mobiele apparaten terwijl in gebouwen of op afstand vanaf elke locatie ter verhoging van de productiviteit. Dit kan zijn van een gemeenschappelijk doel, zal bedrijven die dergelijke eis hebben zijn bezorgdheid met betrekking tot het bedrag van de bedreigingen die moet worden beperkt om bedrijfsgegevens te beveiligen en beheren van de privacy van de gebruiker. Elk bedrijf kan hebben verschillende voorschriften ter zake; naleving van andere regels die varieert volgens welke bedrijfstak het bedrijf handelt zal leiden tot verschillende ontwerpbesluiten. 

Er zijn echter enkele beveiligingsaspecten dat moeten worden onderzocht en gevalideerd, onafhankelijk van de industrie, die in de volgende sectie wordt uitgelegd.

## <a name="data-protection-paths"></a>Data protection paden

![](./media/hybrid-id-design-considerations/data-protection-paths.png)
 
**Data protection paden**

In het bovenstaande diagram is het onderdeel van de identiteit de eerste die worden geverifieerd voordat toegang verkregen gegevens tot wordt. Deze gegevens kunnen zich in verschillende lidstaten gedurende de tijd dat deze werd geopend. Elk nummer in dit diagram geeft een pad waarin gegevens gevonden op een bepaald moment in de tijd worden kunnen. Deze nummers worden hieronder nader verklaard:

1. Bescherming van de gegevens op het apparaatniveau van het.
2. Bescherming van gegevens tijdens de overdracht.
3. Bescherming van de gegevens op de rest van gebouwen.
4. Bescherming van de gegevens op de rest van de wolk.

Hoewel de technische dat bepaalt enable IT ter bescherming van de gegevens zelf op een van deze fasen zijn niet rechtstreeks aangeboden door de hybride identiteit oplossing is het nodig dat de hybride identiteit oplossing kan worden op de bedrijfsruimten en-wolk is identity management middelen ter identificatie van de gebruiker toegang verlenen tot de gegevens. Als uw identiteit hybride oplossing planning zorgt ervoor dat de volgende vragen worden beantwoord volgens de vereisten van uw organisatie:

## <a name="data-protection-at-rest"></a>Bescherming van gegevens in rust
Ongeacht waar de gegevens in rust (apparaat, cloud of op locatie), is het belangrijk om een beoordeling in dit opzicht inzicht in de behoeften van de organisatie. Voor dit gebied, moet u ervoor zorgen dat de volgende vragen gesteld:

- Uw bedrijf nodig heeft ter bescherming van gegevens in rust?
 - Indien Ja, kan de hybride identiteit oplossing integreren met uw huidige infrastructuur voor op locatie?
 - Zo ja, kan de hybride identiteit oplossing integreren met uw werkbelasting die zich in de cloud?
- Kan de cloud identity-management referenties van de gebruiker en andere gegevens die zijn opgeslagen in de cloud te beschermen?

## <a name="data-protection-in-transit"></a>Bescherming van gegevens in transit
Gegevens tijdens de overdracht tussen het apparaat en het datacenter of tussen het apparaat en de cloud moet worden beschermd. Echter betekent wordt in-transit niet noodzakelijkerwijs een proces van communicatie met een component buiten uw cloud-dienst. gaat de intern ook, bijvoorbeeld tussen twee virtuele netwerken. Voor dit gebied, moet u ervoor zorgen dat de volgende vragen gesteld:

- Uw bedrijf nodig heeft om de gegevens tijdens de overdracht te beschermen?
 - Zo ja, kan de hybride identiteit oplossing integreren met veilige besturingselementen, zoals SSL/TLS?
- Houdt de cloud identity management het verkeer van en binnen de Active directory-archief (binnen en tussen datacenters) ondertekend?


## <a name="compliance"></a>Conformiteit
Voorschriften, wetten en regelgeving zijn afhankelijk van de sector waartoe uw bedrijf behoort. Bedrijven in hoge gereglementeerde sectoren moeten identity management bezorgdheid over compatibiliteit met het adres. Verordeningen zoals Sarbanes-Oxley (SOX), de Health Insurance Portability and Accountability Act (HIPAA), de Gramm-Leach-Bliley Act (GLBA) en de betaling kaart Industry Data Security Standard (PCI DSS) zijn zeer streng over identiteits- en toegangsbeheer. De hybride identiteit oplossing die uw bedrijf zal vaststellen, moet de kernfuncties die zal voldoen aan de vereisten van een of meer van deze voorschriften hebben. Voor dit gebied, moet u ervoor zorgen dat de volgende vragen gesteld:

- Is de hybride identiteit oplossing voldoet aan de wettelijke voorschriften voor uw bedrijf?
- De hybride identiteit oplossing met ingebouwde mogelijkheden waardoor uw bedrijf zijn regelgeving voldoen? 
 
>[AZURE.NOTE]
Zorg ervoor dat de notities van elk antwoord en inzicht in de grondgedachte achter het antwoord. [Strategie voor gegevensbescherming definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) gaat over de beschikbare opties en voordelen/nadelen van elke optie.  Door hebben beantwoord deze vragen u selecteert welke optie u best past bij uw bedrijf nodig heeft.

## <a name="next-steps"></a>Volgende stappen
 [Vereisten voor beheer van webinhoud bepalen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)


## <a name="see-also"></a>Zie ook
[Overzicht van overwegingen bij het ontwerp](active-directory-hybrid-identity-design-considerations-overview.md)
