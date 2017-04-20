<properties
   pageTitle="Internet van dingen Security Best Practices | Microsoft Azure"
   description="Het artikel bevat een curated lijst van Microsoft Internet van dingen Security Best Practices en algemene aanbevelingen."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="internet-of-things-security-best-practices"></a>Internet van dingen Security Best Practices

Beveiligen van de infrastructuur van Internet dingen (IoT) is een belangrijke onderneming voor iedereen die betrokken zijn bij de IoT oplossingen. Vanwege het aantal betrokken apparaten en de gedistribueerde aard van deze apparaten, de impact beveiligingsgebeurtenis betrekking hebben op het binnendringen van miljoenen IoT apparaten is niet eenvoudig en wijdverbreide invloed kan hebben.

Om deze reden moet IoT beveiliging een benadering van de veiligheid in diepte. Gegevens moet veilig in de cloud en particuliere en openbare netwerken wordt. Methoden moeten worden om veilig de IoT apparaten zelf inrichten. Elke laag van het apparaat, met netwerk cloud back-end moet sterke beveiliging waarborgen.

IoT beste praktijken kunnen worden ingedeeld in de volgende manier:

- De hardwarefabrikant IoT of integrator
- IoT solution developer
- Implementatie van IoT-oplossing
- IoT solution operator

Dit artikel bevat een overzicht van [Internet van dingen Security Best Practices](../iot-suite/iot-security-best-practices.md). Raadpleeg dit artikel voor meer informatie.

## <a name="iot-hardware-manufacturer-or-integrator"></a>De hardwarefabrikant IoT of integrator

Volg de onderstaande richtlijnen als u een hardwarefabrikant IoT of een hardware-integrator:

- **Scope-hardware aan de minimumvereisten**: minimale die vereist zijn voor de werking van de hardware, en niets meer moet worden opgenomen in het ontwerp van hardwareconfiguratie. 
- **Maken hardware knoeien bewijs**: bouwen in de mechanismen voor het detecteren van fysieke sabotagepogingen van hardware, zoals het openen van de omslag apparaat verwijderen van een deel van het apparaat, enz. 
- **Bouwen om veilig hardware**: als [KPV](https://en.wikipedia.org/wiki/Cost_of_goods_sold) toestaan, beveiligingsfuncties zoals beveiligde en gecodeerde opslag en Trusted Platform Module TPM-opstartomgeving functionaliteit bouwen.
- **Beveiligde opwaarderen**: firmware bijwerken tijdens de levensduur van het apparaat is onvermijdelijk.

## <a name="iot-solution-developer"></a>IoT solution developer

Volg de onderstaande richtlijnen als u een ontwikkelaar IoT-oplossing:

- **Volg secure software development methodologie**: veilige software ontwikkelen is het grond-up over de beveiliging van de aanvang van het project aan de implementatie, testen en implementeren.
- **Open-sourcesoftware met zorg kiezen**: open-source-software biedt de mogelijkheid om snel oplossingen te ontwikkelen.
- **Integreren met zorg**: veel van de beveiligingsfouten software aanwezig zijn op de grens van API's en bibliotheken. 

## <a name="iot-solution-deployer"></a>Implementatie van IoT-oplossing

Voer de onderstaande procedures als u een IoT oplossing implementeren:

- **Hardware veilig implementeren**: IoT-implementaties vereisen hardware te implementeren in een niet-beveiligde locaties, zoals in openbare ruimten of werkende landinstellingen.
- **Verificatiesleutels veilig te houden**: tijdens de implementatie elk apparaat vereist apparaat-id's en verificatiesleutels die zijn gegenereerd door de cloud-service is gekoppeld. Deze sleutels fysiek veilig blijven ook na de implementatie. Een eventueel ontcijferde sleutel kan worden gebruikt door een kwaadwillende apparaat om zich voordoen als een bestaand apparaat.

## <a name="iot-solution-operator"></a>IoT solution operator

Voer de onderstaande procedures als u een operator IoT-oplossing:

- **Systemen up-to-date te houden**: apparaat, besturingssystemen en alle stuurprogramma's worden bijgewerkt naar de nieuwste versies. 
- **Beveiligen tegen schadelijke activiteit**: als het besturingssysteem dat toestaat, de nieuwste anti-virus en anti-malware mogelijkheden op elk besturingssysteem apparaat plaatsen. 
- **Vaak audit**: controle van IoT infrastructuur voor beveiliging problemen is essentieel bij het reageren op beveiligingsincidenten.
- **De IoT infrastructuur fysiek beveiligen**: de slechtste aanvallen tegen IoT infrastructuur met fysieke toegang tot apparaten worden gestart.
- **Protect wolk referenties**: wolk verificatiereferenties gebruikt voor het configureren en gebruiken van een IoT implementatie mogelijk zijn de eenvoudigste manier om toegang en krijgen een IoT-systeem. 
