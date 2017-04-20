<properties
   pageTitle="Aan de slag met Operations Management Suite beveiliging en controle oplossing | Microsoft Azure"
   description="Dit document helpt u aan de slag met Operations Management Suite beveiliging en controle oplossing mogelijkheden voor het controleren van de hybride cloud."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="get-started-article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>
 
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Aan de slag met Operations Management Suite beveiliging en controle-oplossing
Dit document helpt u snel aan de slag met mogelijkheden voor Operations Management Suite (OMS) beveiliging en controle oplossing door leidt u door elke optie.

## <a name="what-is-oms"></a>Wat is de OMS?
Microsoft Operations Management Suite (OMS) is Microsoft van cloud-gebaseerde IT-oplossing waarmee u beheren kunt en beveiligen van uw locatie op en cloud infrastructuur. Lees het artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx)voor meer informatie over het Kantoorbeheersysteem.

## <a name="oms-security-and-audit-dashboard"></a>Dashboard OMS beveiliging en controle

De oplossing OMS beveiliging en controle geeft een uitgebreid overzicht in van uw organisatie houding van IT-beveiliging met ingebouwde zoekopdrachten voor belangrijke problemen die uw aandacht vereisen. Het dashboard **beveiliging en controle** is het beginscherm voor alles wat betrekking heeft op beveiliging in het Kantoorbeheersysteem. Het biedt een hoog niveau inzicht in de beveiligingsstatus van uw computers. Bevat ook de mogelijkheid om alle gebeurtenissen van de afgelopen 24 uur, 7 dagen of andere aangepaste tijd frame weer te geven. Als u het dashboard **beveiliging en controle** , de volgende stappen uit:

1. Klik op de tegel **Instellingen** aan de linkerkant in het belangrijkste dashboard **Microsoft Operations Management Suite** .
2. In het blad **Instellingen** klikt u op de optie **beveiliging en controle** **oplossingen** .
3. De **beveiliging en controle** dashboard wordt weergegeven:

    ![Dashboard OMS beveiliging en controle](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Als er geen apparaten gecontroleerd door OMS krijgt u toegang tot dit dashboard voor de eerste keer wordt niet de tegels gevuld met gegevens die zijn verkregen van de agent. Als u de agent hebt geïnstalleerd, duurt enige tijd om in te vullen, dus Zie in eerste instantie mogelijk ontbreken er bepaalde gegevens als ze nog steeds worden geüpload naar de cloud.  In dit geval is het normaal dat sommige tegels zonder concrete informatie. [Verbinding maken met Windows-computers rechtstreeks OMS](https://technet.microsoft.com/library/mt484108.aspx) Lees voor meer informatie over het Kantoorbeheersysteem agent installeren in een Windows-systeem en [OMS-computers verbinding maken met Linux](https://technet.microsoft.com/library/mt622052.aspx) voor meer informatie over het uitvoeren van deze taak in een Linux-systeem.

> [AZURE.NOTE] De agent verzamelt de gegevens op basis van de huidige gebeurtenissen die zijn ingeschakeld, bijvoorbeeld computernaam, IP-adres en de gebruiker. Echter geen document/bestanden, naam of persoonlijke gegevens worden verzameld.   

Oplossingen zijn van logica, visualisatie en data acquisition regels die belangrijke klant uitdagingen. Beveiliging en controle van een oplossing, anderen kunnen afzonderlijk worden toegevoegd. Lees het artikel [toevoegen oplossingen](https://technet.microsoft.com/library/mt674635.aspx) voor meer informatie over het toevoegen van een nieuwe oplossing.

Het dashboard OMS beveiliging en controle is onderverdeeld in vier hoofdcategorieën:

- **Beveiligingsdomeinen**: verkennen op dit gebied is het mogelijk om meer records beveiliging na verloop van tijd, toegang tot de beoordeling van malware, beoordeling, netwerkbeveiliging, identiteit en toegang tot informatie, computers met beveiligingsgebeurtenissen bijwerken en snel toegang hebben tot Azure Security Center-dashboard.
- **Opvallende problemen**: met deze optie kunt u het aantal actieve kwesties en de ernst van deze problemen snel te identificeren.
- **Detectie (voorbeeld)**: aanval patronen identificeren door beveiligingswaarschuwingen visualiseren terwijl ze tegen uw resources plaatsvinden.
- **Threat Intelligence**: aanval patronen identificeren door het totale aantal servers met schadelijke uitgaande IP-verkeer, het type schadelijk bedreiging en een overzicht waarin de plaats van herkomst van deze IP-adressen van visualiseren. 
- **Algemene query's voor beveiliging**: deze optie geeft u een lijst van de meest voorkomende beveiliging query's die u gebruiken kunt voor het controleren van uw omgeving. Wanneer u op een van deze query's klikt, wordt het blad **Zoeken** geopend met de resultaten van die query.

> [AZURE.NOTE] voor meer informatie over hoe OMS uw gegevens beveiligt, lees dat hoe OMS beveiligt uw gegevens.

## <a name="security-domains"></a>Beveiligingsdomeinen

Als u resources controleren, is het belangrijk dat u snel toegang tot de huidige status van uw omgeving. Het is echter ook belangrijk dat u vorige gebeurtenissen die hebben plaatsgevonden in het verleden dat leiden kan tot een beter begrip van wat in uw omgeving op een bepaald punt in de tijd gebeurt er bijhouden. 

> [AZURE.NOTE] bewaren van gegevens vindt plaats volgens de OMS plan prijzen. Ga naar de [Microsoft Operations Management Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) prijzen pagina voor meer informatie.

Incident response forensics onderzoek scenario's en profiteren direct van de resultaten beschikbaar in de **Beveiliging van Records in de tijd** naast elkaar.

![Beveiliging van records in de tijd](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Wanneer u op deze tegel klikt, de blade **Zoeken** wordt geopend met daarin een queryresultaat voor **Beveiligingsgebeurtenissen** (Type = SecurityEvents) met gegevens op basis van de laatste zeven dagen, zoals hieronder wordt weergegeven:

![Beveiliging van records in de tijd](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

Het zoekresultaat is verdeeld in twee deelvensters: het linker deelvenster kunt u een uitsplitsing van het aantal beveiligingsgebeurtenissen die zijn gevonden, de computers waarop deze gebeurtenissen zijn gevonden, het aantal accounts die zijn gevonden in deze computers en de soorten activiteiten. Het rechterdeelvenster kunt u de totale resultaten en een chronologisch overzicht van welke beveiligingsgebeurtenissen bij de naam en de gebeurtenis activiteit van de computer. Klik op **Meer weergeven** om meer details over deze gebeurtenis, zoals gegevens van de gebeurtenis en de gebeurtenis-ID van de gebeurtenisbron weer te geven.

> [AZURE.NOTE] voor meer informatie over de zoekopdracht OMS lezen [OMS verwijzing zoeken](https://technet.microsoft.com/library/mt450427.aspx).

### <a name="antimalware-assessment"></a>Antimalware-beoordeling

Met deze optie kunt u snel om computers te identificeren met onvoldoende bescherming bieden en computers die zijn aangetast door een stuk van malware. Status van malware beoordeling en gedetecteerde bedreigingen op gecontroleerde servers worden gelezen en vervolgens de gegevens worden verzonden naar de OMS-service in de cloud voor verwerking. Servers met gevonden bedreigingen en servers met onvoldoende bescherming worden weergegeven in het dashboard malware beoordeling, dat kan geopend worden nadat u op in de tegel **Antimalware-beoordeling** . 

![beoordeling van malware](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Net als elke andere levende tegel in Dashboard OMS beschikbaar wanneer u op klikt, de blade **Zoeken** geopend met het resultaat van de query. Voor deze optie als u in de optie **Niet worden gerapporteerd** onder **Bescherming**hebt u het resultaat van de query waarin deze één vermelding met de naam van de computer en de rang, zoals hieronder wordt weergegeven:

![zoekresultaat](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [AZURE.NOTE] *rang* is een cijfer geven aan de status van de bescherming (ingeschakeld, uitgeschakeld, bijgewerkt, enz) en bedreigingen die zijn gevonden. Dat die als een nummer helpt om aggregaties.

Als u de naam van de computer, hebt u het chronologisch overzicht van de beveiligingsstatus van deze computer. Dit is handig voor scenario's die u begrijpen moet als de antimalware eenmaal is geïnstalleerd en op een bepaald moment die is verwijderd.   

### <a name="update-assessment"></a>Beoordeling van de update 

Met deze optie kunt u snel bepalen de algehele blootstelling aan mogelijke beveiligingsproblemen, en of of hoe essentieel zijn deze updates zijn voor uw omgeving. OMS beveiliging en controle oplossing alleen de visualisatie van deze updates bieden, de reële gegevens afkomstig van de [System-oplossingen voor Updates](https://technet.microsoft.com/library/mt484096.aspx), dat een andere module binnen OMS is. Hier een voorbeeld van de updates:

![systeemupdates](./media/oms-security-getting-started/oms-getting-started-fig6.png)

> [AZURE.NOTE] voor meer informatie over Updates oplossing lezen [servers met de oplossing van het systeemupdates bijwerken](https://technet.microsoft.com/library/mt484096.aspx).

### <a name="identity-and-access"></a>Identiteits- en toegangsbeheer

Identiteit het besturingsvlak moet zijn voor uw onderneming, uw identiteit beschermen moet de hoogste prioriteit. Terwijl in het verleden planimetrische rond organisaties werden en deze verbindingen een van de grenzen van de primaire defensief zijn, wordt tegenwoordig met meer gegevens en meer apps verplaatsen naar de cloud de identiteit de nieuwe rand. 

> [AZURE.NOTE] op dit moment de gegevens zijn gebaseerd op beveiligingsgebeurtenissen login gegevens (gebeurtenis-ID 4624) in de toekomstige Office365 aanmeldingen en Azure AD gegevens ook worden opgenomen.

Door de activiteiten van uw identiteit controleren zijn u kunt proactieve acties uitvoeren voordat een incident plaats of reactieve acties een poging van de aanval te stoppen. Het **identiteits- en toegangsbeheer** dashboard biedt u een overzicht van uw identiteit staat, inclusief het aantal mislukte aanmeldingspogingen, de account van de gebruiker die zijn gebruikt tijdens deze pogingen, accounts die zijn vergrendeld, accounts met gewijzigd of opnieuw instellen van wachtwoord en momenteel het aantal accounts die zijn vastgelegd in. 

Wanneer u in de **identiteits- en toegangsbeheer** tegel klikt ziet u de volgende dashboard:

![identiteits- en toegangsbeheer](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

De informatie in dit dashboard kunt onmiddellijk u helpen bij het identificeren van een mogelijke verdachte activiteiten. Er zijn bijvoorbeeld 338 probeert aan te melden als **beheerder** en 100% van deze pogingen mislukt. Dit kan worden veroorzaakt door een felle aanval op deze rekening. Als u op deze account klikt krijgt u meer informatie u helpen kan bij het bepalen van de doelbron voor deze aanval:

![zoekresultaten](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

Het gedetailleerde rapport bevat belangrijke informatie over deze gebeurtenis, met inbegrip van: de doelcomputer, de soort aanmelding (in dit geval netwerkaanmelding), de activiteit (in dit geval gebeurtenis 4625) en een uitgebreide tijdlijn van elke poging. 

### <a name="computers"></a>Computers

Deze naast elkaar kan worden gebruikt voor toegang tot alle computers met actief beveiligingsgebeurtenissen. Wanneer u in dit venster klikt ziet u de lijst met computers met beveiligingsgebeurtenissen en het aantal gebeurtenissen op elke computer:

![Computers](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

U kunt uw onderzoek gaan door te klikken op elke computer en bekijken welke beveiligingsgebeurtenissen die zijn gemarkeerd.

### <a name="azure-security-center"></a>Azure Security Center

Deze tegel is in feite een snelkoppeling naar het dashboard Azure Beveiligingscentrum te openen. Lezen [aan de slag met Azure Security Center](../security-center/security-center-get-started.md) voor meer informatie over deze oplossing.

## <a name="notable-issues"></a>Belangrijke kwesties

Het belangrijkste doel van deze groep opties wordt een beknopt overzicht van de problemen die er in uw omgeving, door ze te categoriseren in kritieke, waarschuwings- en ter informatie verstrekt. De actieve kwestie type tegel is een visualisatie van deze problemen, maar kunt u niet meer details hierover, voor verkennen die u gebruiken het onderste gedeelte van dit element met de naam van het probleem (naam wilt), hoeveel objecten had dit gebeurt (aantal) en hoe essentieel is (PRIORITEIT).

![Belangrijke kwesties](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

U kunt zien dat deze problemen zijn reeds in verschillende gebieden van de **Domeinen Security** groep de bedoeling van deze weergave versterkt: visualiseren van de belangrijkste punten in uw omgeving vanaf één enkele plek.

## <a name="detections-preview"></a>Detectie (voorbeeld)

Het belangrijkste doel van deze optie is om de IT snel identificeren van potentiële bedreigingen voor hun omgeving via en de ernst van deze bedreiging.

![Intel bedreiging](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Deze optie kan ook worden gebruikt tijdens een incident response-verificatie uit te voeren beoordeling meer informatie verkrijgen over de aanval.

> [AZURE.NOTE] Bekijk [hoe u kunt profiteren van de Microsoft Operations Management Suite voor een Incident Response & Azure Security Center](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703)voor meer informatie over het gebruik van OMS voor Incident Response.

## <a name="threat-intelligence"></a>Threat Intelligence

De nieuwe sectie van de intelligence bedreiging van de beveiliging en controle oplossing visualiseert de patronen om aanvallen op verschillende manieren: het totale aantal servers met schadelijke uitgaande IP-verkeer, het type schadelijk bedreiging en een overzicht waarin wordt aangegeven waar deze IP-adressen afkomstig zijn uit. U kunt communiceren met de kaart en klik op de IP-adressen voor meer informatie.

Gele markeringspunten op de kaart geven binnenkomende verkeer van schadelijke IP-adressen. Het is niet ongebruikelijk voor servers die zijn blootgesteld aan internet naar binnenkomend verkeer van schadelijke, maar het beste beoordelen deze pogingen om ervoor te zorgen dat geen van hen is geslaagd. Deze indicatoren zijn gebaseerd op de IIS-logboeken, WireData en het logboek van Windows Firewall.  

![Intel bedreiging](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Algemene query's voor beveiliging

De lijst met algemene query's beveiliging beschikbaar is handig om u snel toegang tot de bron van informatie en het aanpassen op basis van de behoeften van uw omgeving. Deze algemene query's zijn:

- Alle activiteiten
- Activiteiten op de computer 'computer01.contoso.com' (vervangen door de naam van uw eigen computer)
- Activiteiten op de computer 'computer01.contoso.com' voor account 'Administrator' (vervangen door uw eigen computer en account namen)
- Activiteit op de Computer aanmelden
- Accounts die Microsoft antimalware op elke computer wordt afgesloten
- Computers waarop de Microsoft antimalware-proces is beëindigd
- Computers waarop 'hash.exe' is uitgevoerd (Vervang door de naam van een ander proces)
- Alle procesnamen die zijn uitgevoerd
- Activiteit op Account aanmelden
- Accounts die op afstand zijn aangemeld op de computer 'computer01.contoso.com' (vervangen door de naam van uw eigen computer)

## <a name="see-also"></a>Zie ook

In dit document, kunt u kennisgemaakt met OMS beveiligings- en controle-oplossing. Voor meer informatie over de beveiliging van OMS, Zie de volgende artikelen:

- [Operations Management Suite (OMS)-overzicht](operations-management-suite-overview.md)
- [Toezicht en het reageren op de beveiligingswaarschuwingen in Operations Management Suite beveiliging en controle-oplossing](oms-security-responding-alerts.md)
- [Resources controleren in Operations Management Suite beveiliging en controle-oplossing](oms-security-monitoring-resources.md)
