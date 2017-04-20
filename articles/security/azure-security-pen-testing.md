<properties
   pageTitle="Testen van pen | Microsoft Azure"
   description="Het artikel biedt een overzicht van de penetratie testen (pentest) proces en hoe pentest tegen uw toepassingen uitgevoerd in Azure infrastructuur uit te voeren."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="pen-testing"></a>Pen testen

Een van de grote dingen over het gebruik van Microsoft Azure voor het testen van de toepassing en implementatie is hoeft u niet om samen te stellen een infrastructuur op ruimten voor het ontwikkelen, testen en implementeren van uw toepassingen. Alle infrastructuur wordt verwerkt door de services van Microsoft Azure platform. U hebt geen zorgen te maken over de aanschaf, verwerven, en "wordt afgetapt en stapelen" uw eigen hardware op gebouwen.

Dit is fantastisch uit, maar nog steeds moet u controleren of u de beveiliging van uw normale uitvoeren voor het innen. Een van de dingen die u moet doen is penetratie testen van de toepassingen die u hebt geïmplementeerd in Azure.

Misschien weet u al dat Microsoft [penetratie testen van onze Azure-omgeving](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)uitvoert. Dit helpt ons bij het verbeteren van ons platform en onze acties van besturingselementen voor beveiliging te verbeteren, introductie van nieuwe besturingselementen voor beveiliging en verbeteren van onze beveiligingsprocessen begeleidt.

We geen pen testen van de toepassing voor u, maar wij begrijpen dat u wilt en hoeft uit te voeren, pen testen op uw eigen toepassingen. Dat is goed, omdat wanneer u de beveiliging van uw toepassingen verbeteren, u helpen het hele Azure ecosysteem veiliger te maken.

Wanneer u pen test uw toepassingen, lijkt het op een aanval voor ons. Wij [bewaken continu](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) aanvallen patronen en initiëren een beveiligingsplan proces als we moeten. Het heeft niets te maken en het ons niet helpen als we een beveiligingsplan door uw eigen verschuldigde toewijding inzetten voor pen testen activeren.

Wat te doen?

Wanneer u nu pen de Azure gehoste toepassingen testen, moet u te laten weten. Zodra we weten dat u gaat specifieke tests gaan uitvoeren, won't we per ongeluk afsluiten (zoals het blokkeren van de IP-adres dat u testen), zolang de tests voldoen aan de Azure pen testing bepalingen en voorwaarden.
Standaard tests die u kunt uitvoeren zijn onder andere:

- Tests op de eindpunten om de [bovenste 10 beveiligingslekken Open Web Application Security Project (OWASP)](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
- [Fuzz testen](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) van de eindpunten
- [Scannen van poorten](https://en.wikipedia.org/wiki/Port_scanner) van de eindpunten

Een type test die u niet kunt uitvoeren is een soort [Denial of Service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) aanval. Dit omvat een DoS-aanval zelf initiëren of het uitvoeren van gerelateerde tests die kunnen bepalen, demonstreren of een soort DoS-aanvallen te simuleren.

Wilt u aan de slag met pen uw toepassingen die worden gehost in Microsoft Azure testen? Als er in dat geval, hoofd op over de [Marktpenetratie Test overzicht](https://security-forms.azure.com/penetration-testing/terms) pagina (en klik op de knop testen aanvragen onder aan de pagina maken. Ook vindt u meer informatie over de voorwaarden en nuttige links over hoe u beveiligingsfouten Azure of andere Microsoft-service rapporteren kunt testen pen.
