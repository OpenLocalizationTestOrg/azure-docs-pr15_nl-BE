<properties
   pageTitle="Azure algemene scenario's voor een gegevenscatalogus | Microsoft Azure"
   description="Een overzicht van veelvoorkomende scenario's voor Azure catalogus gegevens, met inbegrip van de registratie en detectie van hoogwaardige bronnen, waardoor selfservice business intelligence en bestaande Tribale kennis over gegevensbronnen en processen vastleggen."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/03/2016"
   ms.author="maroche"/>


# <a name="azure-data-catalog-common-scenarios"></a>Azure algemene scenario's voor een gegevenscatalogus

Dit artikel bevat algemene scenario's waar Azure gegevenscatalogus Haal meer uit hun bestaande gegevensbronnen organisaties kunnen helpen.

## <a name="scenario-1---registration-of-central-data-sources"></a>#1 - registratie van centrale gegevensbronnen scenario

Organisaties hebben vaak een aantal gegevensbronnen met hoge waarde. Deze gegevensbronnen zijn business OLTP-systemen, Datawarehouses en business intelligence / analytics-databases. Vaak het aantal systemen en de overlapping tussen systemen, na verloop van tijd als de behoeften van de business-evolve groeit en als het bedrijf zelf ontwikkeld via fusies en overnames.

Vaak is het moeilijk voor gebruikers om te weten waar u de gegevens in deze gegevensbronnen. Op deze vragen zijn allemaal te vaak:

- Van de drie HR-systemen binnen het bedrijf worden gebruikt, welke moet ik gebruiken voor het maken van dit type rapport?
- Waar moet ik gaan voor de gecertificeerde verkoopcijfers voor het fiscale jaar dat net is afgelopen?
- Wie moet ik vragen of wat is het proces dat voor toegang tot het datawarehouse moet ik gebruiken?
- Ik weet niet als deze getallen rechts – die kan ik vragen voor inzicht in hoe deze gegevens worden gebruikt moeten voordat ik dit dashboard met mijn team delen?

In dit scenario kunt u catalogus Azure-gegevens. De centrale, hoge waarde, beheerde IT gegevensbronnen die worden gebruikt in de hele organisatie zijn vaak het logische uitgangspunt voor het vullen van de catalogus. Hoewel elke gebruiker een gegevensbron te registreren kan, helpt met de catalogus met de gegevensbronnen die waarschijnlijk om waarde aan het grootste aantal gebruikers kick-started station vaststelling en het gebruik van het systeem. Voor klanten aan de slag met Azure gegevenscatalogus, is identificeren en registreren van de belangrijke gegevensbronnen gebruikt door veel verschillende teams van gebruikers van gegevens de eerste stap naar succes.

In dit scenario biedt ook gelegenheid aantekeningen maken de hoge waarde gegevensbronnen om deze gemakkelijker te begrijpen en te openen. Een belangrijk aspect van dit initiatief is om op te nemen informatie over hoe gebruikers toegang tot de gegevensbron kunnen aanvragen. Catalogus Azure gegevens kunnen gebruikers het e-mailadres van de gebruiker of het team dat verantwoordelijk is voor het beheren van toegang tot gegevensbron, koppelingen naar bestaande hulpprogramma's of documentatie of vrije tekst beschrijving van het aanvraagproces voor toegang. Met deze gegevens opgenomen in de catalogus, kunnen gebruikers die geregistreerde gegevensbronnen ontdekken maar die nog niet hebben machtigingen voor toegang tot de gegevens gemakkelijk toegang met behulp van de processen gedefinieerd en beheerd door de eigenaars van de gegevensbron aanvragen.

## <a name="scenario-2---self-service-business-intelligence"></a>Scenario #2 - selfservice business intelligence

Hoewel traditionele corporate business intelligence-oplossingen een waardevol onderdeel van veel organisaties gegevens landschappen blijft, heeft wijzigen tempo van de zakelijke gesteld Self-service BI meer en meer belangrijk. Self-service BI kan IT-medewerkers en analisten hun eigen rapporten, dashboards en werkmappen maken zonder een centrale IT-team – of worden beperkt door de beschikbaarheid en planning die IT-team.

In de self-service BI scenario's is het gebruikelijk voor gebruikers om gegevens uit meerdere bronnen, die mogelijk niet eerder is gebruikt voor BI en analyse te combineren. Hoewel sommige van deze gegevensbronnen mogelijk al bekend zijn, is er vaak een proces om te ontdekken wat plaats te vinden en evalueren van potentiële gegevensbronnen voor een bepaalde taak moet uitvoeren.

Traditioneel deze discovery-proces is een handmatige: analisten hun peer-Netwerkverbindingen wordt gebruikt voor het identificeren van andere mensen die werken met de gegevens wordt gezocht. Nadat u een gegevensbron is gevonden kan worden gebruikt, maar het proces wordt herhaald opnieuw voor elke volgende Self-service BI-inspanning, met meerdere gebruikers dezelfde redundante handmatig proces van discovery uitvoeren.

Met Azure Data-catalogus, kunnen gebruikers deze cyclus van redundante inspanning verbroken. Nadat u een gegevensbron met behulp van traditionele middelen is gevonden, kan een analist de gegevensbron, zodat het gemakkelijker kan worden gevonden in de toekomst kunt registreren. Hoewel de gebruiker meer waarde toevoegen kan door het toewijzen van aantekeningen aan de activa van de geregistreerde gegevens, hoeft dit niet plaatsvinden op hetzelfde moment als de registratie. Gebruikers kunnen bijdragen in de tijd als hun vergunning schema's geleidelijk waarde toe te voegen aan de gegevensbronnen die zijn geregistreerd in de catalogus.

Deze organische groei van de catalogusinhoud is een natuurlijke aanvulling op de aanvang registratie van centrale gegevensbronnen. Vooraf invullen van de catalogus met gegevens die veel gebruikers nodig hebben is een motivator voor het eerste gebruik en discovery. Waardoor gebruikers te registreren en aanvullende bronnen van aantekeningen voorzien, is een manier om te houden en hun collega's die zich bezighoudt.

Het is ook vermeldenswaardig dat hoewel dit scenario richt zich specifiek op de self-service BI, de patronen en de uitdagingen van toepassing op grootschalige bedrijven BI-projecten ook. Elke inspanning die betrekking heeft op een handmatig proces van data source discovery is een inspanning die waarde aan de organisatie door het gebruik van de catalogus gegevens Azure toevoegen kan.

## <a name="scenario-3---capturing-tribal-knowledge"></a>#3 - vastgelegde Tribale knowledge scenario

Hoe weet u welke gegevens u nodig hebt voor uw werk en waar vind ik die gegevens?

Als u al uw werk voor een tijdje, weet u waarschijnlijk alleen maar. U bent via een geleidelijke upgrade leerproces geweest en na verloop van tijd hebt geleerd over de gegevensbronnen die essentieel voor uw dagelijkse werk zijn.

Wanneer u een nieuwe medewerker in dienst neemt uw team, hoe hij weet wat hij nodig heeft zijn taak en waar vind ik deze gegevens?

Kans zijn, vraagt hij u.

Deze aanhoudende Tribale kennisoverdracht is onderdeel van de data source discovery-proces in organisaties grote en kleine. Senior en ervaren teamleden hebben kennis opgebouwd over de jaren en nieuwere teamleden hebben geleerd om te vragen wanneer ze vragen hebben. De meest essentiële informatie vaak alleen aanwezig is in de hoofden van een paar belangrijke personen en wanneer de mensen op vakantie zijn, of het team laat, de organisatie suffers.

Soms maakt deze deskundigen gegevens de inspanningen om hun kennis, documenten delen via e-mail of in Word-documenten op een SharePoint-teamsite. Hoewel dit waardevolle dat het introduceert een nieuwe discovery probleem – hoe mensen weet welke documentatie bestaat en kan vinden...

Azure gegevenscatalogus biedt een locatie voor het delen van deze kennis Tribale te maken en het gemakkelijk kan worden gevonden. Deskundigen van de gegevens kunnen rechtstreeks gegevens activa van aantekeningen voorzien en kunnen ook links toevoegen aan bestaande documentatie. Niet alleen heeft de kennis zelf vastleggen, maar ook wordt de kennis geplaatst in de dezelfde ervaring die wordt gebruikt voor gegevensbron te zoeken. Als iemand vinden met de catalogus van een gegevensbron niet alleen hij de bron zelf zoeken, vindt hij ook kennis van de deskundige die eerder alleen in de gedachten van de deskundige zelf.
