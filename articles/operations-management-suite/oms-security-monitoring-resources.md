<properties
   pageTitle="Resources controleren in Operations Management Suite beveiliging en controle oplossing | Microsoft Azure"
   description="Dit document helpt u bij het gebruik van de OMS en de controle van de mogelijkheden om uw bronnen te controleren en beveiligingsproblemen identificeren."
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

# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Controle van de bronnen in het Operations Management Suite beveiliging en controle-oplossing

Dit document kunt u gebruiken OMS beveiliging en controle mogelijkheden om uw bronnen te controleren en beveiligingsproblemen identificeren.

## <a name="what-is-oms"></a>Wat is de OMS?

Microsoft Operations Management Suite (OMS) is van Microsoft cloud gebaseerde IT management-oplossing waarmee u beheren kunt en beveiligen van uw locatie op en cloud infrastructuur. Lees het artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx)voor meer informatie over het Kantoorbeheersysteem.

## <a name="monitoring-resources"></a>Bronnen controleren

Wanneer mogelijk is, is het verstandig om te voorkomen dat beveiligingsincidenten gebeurt in de eerste plaats. Het is echter onmogelijk alle beveiligingsincidenten te voorkomen. Als een beveiligingsincident voordoet, moet u ervoor zorgen dat het effect ervan is geminimaliseerd.  Er zijn drie belangrijke aanbevelingen die kunnen worden gebruikt tot een minimum beperken van het aantal en de invloed van beveiligingsincidenten:

- Zwakke plekken in uw omgeving regelmatig beoordelen.
- Controleer regelmatig alle computersystemen en netwerkapparaten zijn van de laatste patches geïnstalleerd.
- Controleer regelmatig alle logboeken en logboekregistratie mechanismen, met inbegrip van de gebeurtenislogboeken van besturingssystemen, toepassingen en inbraakdetectiesysteem.

Minimaliseer de gevolgen van beveiligingsincidenten OMS beveiligings- en controle-oplossing kunnen IT-actief controleren alle bronnen die u kunnen helpen. OMS beveiliging en controle heeft de van beveiligingsdomeinen die kunnen worden gebruikt voor het controleren van de resources. De beveiligingsdomeinen biedt snel toegang tot opties voor een voor de beveiliging controle de volgende domeinen worden behandeld in meer details:

- Beoordeling van malware
- Beoordeling van de update
- Identiteits- en toegangsbeheer

> [AZURE.NOTE] lezen [aan de slag met Operations Management Suite beveiliging en controle-oplossing](oms-security-getting-started.md)voor een overzicht van al deze opties.

### <a name="monitoring-system-protection"></a>Controleren, systeembeveiliging

Elke laag van bescherming is een defense in depth benadering belangrijk voor de algehele beveiliging van uw activa. Computers met gedetecteerde bedreigingen en computers met onvoldoende bescherming worden weergegeven in de tegel Malware beoordeling onder beveiligingsdomeinen. Met behulp van de gegevens op de Malware-beoordeling, kunt u een plan voor de bescherming van toepassing op de servers die deze identificeren. Voor toegang tot deze optie de volgende stappen:

1. Klik op **beveiliging en controle** naast elkaar in het belangrijkste dashboard **Microsoft Operations Management Suite** .

    ![Beveiliging en controle](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. Klik op **Beoordeling Antimalware** onder **Beveiligingsdomeinen**in het dashboard **beveiliging en controle** . Het dashboard **Antimalware beoordeling** wordt weergegeven zoals hieronder:

![Beoordeling van malware](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

U kunt het dashboard **Malware beoordeling** identificeren de volgende beveiligingsproblemen:

- **Actieve bedreigingen**: computers die zijn aangetast en actieve bedreigingen in het systeem hebben.
- **Remediated bedreigingen**: computers die zijn aangetast, maar de bedreigingen zijn verholpen.
- **De handtekening is verlopen**: computers met malwarebescherming is ingeschakeld, maar de handtekening is verlopen.
- **Geen real-time bescherming**: computers die geen antimalware geïnstalleerd.

### <a name="monitoring-updates"></a>Controle op updates 

Toepassen van de meest recente beveiligingsupdates is veiligheidsoverwegingen en deze moet worden opgenomen in de strategie voor de update. Microsoft Monitoring Agent-service (HealthService.exe) informatie over de gecontroleerde computers leest en verzendt vervolgens deze bijgewerkte gegevens naar de OMS-service in de cloud voor verwerking. De service Microsoft Monitoring Agent is geconfigureerd als een automatische service en moet deze altijd worden uitgevoerd op de doelcomputer.

![controle op updates](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Logica wordt toegepast op de gegevens bijwerken en de cloud-service legt de gegevens. Als u ontbrekende updates worden gevonden, wordt deze weergegeven op het dashboard **Updates** . Het dashboard **Updates** kunt u werken met ontbrekende updates en een plan opstellen om te deze toepassen op de servers die ze nodig hebben. Volg de stappen hieronder om naar het dashboard **Updates** :

1. Klik op **beveiliging en controle** naast elkaar in het belangrijkste dashboard **Microsoft Operations Management Suite** .
2. Klik op **Update beoordeling** onder **Beveiligingsdomeinen**in het dashboard **beveiliging en controle** . Het dashboard Update wordt weergegeven zoals hieronder:

![beoordeling van de update](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

In dit dashboard kunt u een beoordeling van de update om de huidige status van uw computers begrijpen en adres van de belangrijkste bedreigingen uitvoeren. Met behulp van de tegel **kritieke en beveiligingsupdates** worden IT-beheerders toegang kunnen krijgen tot gedetailleerde informatie over de updates die ontbreken zoals hieronder wordt weergegeven:

![zoekresultaat](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Dit rapport kritieke gegevens bevatten die kan worden gebruikt voor de identificatie van het type van de bedreiging die dit systeem kwetsbaar is voor, waaronder de Microsoft KB-artikelen die zijn gekoppeld aan de beveiligingsupdate en van het Bulletin MS met meer details over het beveiligingslek.

### <a name="monitoring-identity-and-access"></a>Identiteits- en toegangsbeheer controleren

Met gebruikers overal en werken met verschillende apparaten en toegang tot een enorme hoeveelheid apps wolk en in de lokalen, is het noodzakelijk dat hun referenties worden beschermd. Referentie diefstal aanvallen zijn waarbij een aanvaller in eerste instantie toegang tot een gewone gebruiker referenties krijgt voor toegang tot een systeem binnen het netwerk. In veel gevallen deze eerste aanval is alleen een manier om toegang tot het netwerk, het uiteindelijke doel is te ontdekken accounts bevoegdheden. 

Aanvallers blijft staan in het netwerk, met gratis gereedschap om referenties ophalen uit de sessies van andere accounts is aangemeld. Afhankelijk van de systeemconfiguratie kunnen deze referenties in de vorm van hashes, tickets of zelfs leesbare wachtwoorden worden geëxtraheerd.  

> [AZURE.NOTE] machines die direct zijn blootgesteld aan Internet zien veel pogingen die proberen aan te melden met behulp van alle soorten van bekende gebruikersnamen (bijvoorbeeld Administrator). In de meeste gevallen is OK als de bekende gebruikersnamen niet worden gebruikt en als het wachtwoord sterk genoeg is.

Het is mogelijk deze indringers te identificeren voordat ze een account voor de bevoegdheid in gevaar brengen. U kunt gebruikmaken van **OMS beveiligings- en controle-oplossing** voor het controleren van identiteits- en toegangsbeheer. Volg de stappen hieronder om naar het dashboard **identiteits- en toegangsbeheer** :

1. Klik op beveiliging en controle in het belangrijkste dashboard **Microsoft Operations Management Suite** naast elkaar.
2. Klik in het dashboard **beveiliging en controle** , **identiteits- en toegangsbeheer** onder **Beveiligingsdomeinen**. Het dashboard **identiteits- en toegangsbeheer** wordt weergegeven zoals hieronder:

![identiteits- en toegangsbeheer](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Als onderdeel van uw strategie voor regelmatige controle, moet u de identiteit controleren opnemen. IT-beheer ziet er als er een bepaalde geldige gebruikersnaam met veel pogingen. Dit kan geven een aanvaller die de werkelijke gebruikersnaam verworven en proberen te brute kracht of een automatische tool gebruikt hard gecodeerde wachtwoord die zijn verlopen.

Dit dashboard enable IT snel identificeren van potentiële bedreigingen met betrekking tot de identiteit en de toegang tot de bronnen van het bedrijf. Het is bijzonder belangrijk is ook mogelijk om trends te ontdekken, bijvoorbeeld in de tegel aanmeldingen verloop van tijd ziet u in periode het aantal keren dat een mislukte poging tot aanmelden is uitgevoerd. In dit geval ontvangen de computer **bestandsserver** 35 aanmeldingspogingen. U vindt meer informatie over deze computer door erop te klikken. 

![meer details](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Het rapport gegenereerd voor deze computer biedt waardevolle informatie over dit patroon. Opgemerkt dat **de kolom** u de gebruikersaccount die is gebruikt kunt voor toegang tot het systeem, de kolom **TIMEGENERATED** geeft u het tijdsinterval waarin de poging is gedaan en de kolom **LOGONTYPENAME** kunt u de locatie waar deze poging is gedaan. Als deze pogingen zijn lokaal in het systeem uitgevoerd door een programma, zou de **proces** kolom de naam van het proces weergegeven. U hebt al beschikbaar op de naam van het proces in scenario's waar de aanmeldingspoging is afkomstig van een programma, en nu kunt u uitvoeren verder onderzoek in het target-systeem.

## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe OMS beveiliging en controle-oplossing voor het controleren van uw resources. Voor meer informatie over de beveiliging van OMS, Zie de volgende artikelen:

- [Operations Management Suite (OMS)-overzicht](operations-management-suite-overview.md)
- [Aan de slag met Operations Management Suite beveiliging en controle-oplossing](oms-security-getting-started.md)
- [Toezicht en het reageren op de beveiligingswaarschuwingen in Operations Management Suite beveiliging en controle-oplossing](oms-security-responding-alerts.md)