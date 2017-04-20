<properties
    pageTitle="Cloud App Discovery beveiliging en Privacy-overwegingen | Microsoft Azure"
    description="Dit onderwerp beschrijft de beveiliging en privacy overwegingen ten aanzien van Cloud App Discovery."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloud App Discovery beveiliging en Privacy-overwegingen

Microsoft hecht zeer aan de bescherming van uw privacy en beveiliging van uw gegevens bij de levering van software en services die u helpen bij het beheren van de beveiliging van uw organisatie. <br>
We herkennen wanneer u uw gegevens aan anderen toevertrouwen dat vertrouwen vereist strenge beveiliging technische investeringen en expertise back.
Microsoft voldoet aan de strikte naleving van regelgeving en richtlijnen voor beveiliging van veilige software development lifecycle procedures voor een dienst. <br>
Bevestiging en bescherming van gegevens is een topprioriteit bij Microsoft.

In dit onderwerp wordt uitgelegd hoe gegevens is verzameld, behandeld en beveiligd in Azure Active Directory Cloud App Discovery




##<a name="overview"></a>Overzicht

Cloud App Discovery is een functie van AD Azure en wordt gehost in Microsoft Azure. <br>
De agent Cloud App Discovery eindpunt wordt toepassing discovery om gegevens te verzamelen van beheerde IT-computers gebruikt. <br>
De verzamelde gegevens wordt veilig via een gecodeerd kanaal verzonden naar de wolk van Azure AD App Discovery-service. <br>
De Cloud App Discovery-gegevens voor een organisatie is zichtbaar in de portal Azure. <br>


<center>![De werking van Cloud App Discovery](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png)</center> <br>


Hieronder volgen de stroom van informatie en beschrijven hoe het beweegt naar de Cloud App Discovery-service van uw organisatie wordt beveiligd en uiteindelijk naar de Cloud App Discovery portal.



## <a name="collecting-data-from-your-organization"></a>Verzamelen van gegevens van uw organisatie

Wilt Azure Active Directory App Cloud discovery-functie gebruiken om inzicht te krijgen in de toepassingen die worden gebruikt door werknemers in uw organisatie, moet u eerst de agent Azure AD Cloud App Discovery eindpunt implementeren op computers in uw organisatie.

Beheerders van de pachter Azure Active Directory (of hun gemachtigde) kunnen downloaden van het installatiepakket van de agent van de Azure portal. De agent kan worden handmatig geïnstalleerd of via meerdere computers in de organisatie met SCCM of Groepsbeleid wordt geïnstalleerd.

Zie [Cloud App Discovery Group Policy Deployment Guide](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)voor meer informatie over implementatieopties.
<br>

### <a name="data-collected-by-the-agent"></a>Gegevens die zijn verzameld door de agent

De gegevens die worden beschreven in de onderstaande lijst worden verzameld door de agent wanneer een verbinding wordt gemaakt met een webtoepassing. De gegevens worden alleen verzameld voor de toepassingen die de beheerder heeft geconfigureerd voor detectie. <br>
U kunt de lijst van cloud-apps die de agent via de Cloud App Discovery blade in het Microsoft [Azure portal](https://portal.azure.com/)onder **instellingen controleert**bewerken->**Gegevensverzameling**->**App lijst**. Zie voor meer informatie [Ophalen van de slag met Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
<br>
**Categorie informatie**: gebruikersgegevens <br>
**Beschrijving**: <br>
De Windows-gebruikersnaam van het proces dat een verzoek ingediend bij de doel-webtoepassing (bv.: DOMEIN\gebruikersnaam) en de Windows Security Identifier (SID) van de gebruiker.


**Categorie van gegevens**: de verwerking van gegevens <br>
**Beschrijving**: <br>
De naam van het proces dat het verzoek heeft ingediend met het doel webtoepassing (bv.: "iexplore.exe")

**Categorie informatie**: informatie van de computer <br>
**Beschrijving**: <br>
De NetBIOS-naam van de computer waarop de agent is geïnstalleerd.

**Categorie informatie**: verkeersinformatie App <br>
**Beschrijving**: <br>

De volgende informatie:

- De bron (lokale computer) en doel-IP-adressen en poortnummers

- Het openbare IP-adres van de organisatie die de aanvraag gaat uit.

- Het tijdstip van de aanvraag

- Het volume van het verkeer verzonden en ontvangen

- De IP-versie (4 of 6)

- Voor TLS-verbindingen: de hostnaam van de Server naam aanduiding extensie of het servercertificaat.

De volgende HTTP-gegevens:

- Methode (GET, POST, enz.)

- Protocol (HTTP/1.1, enz.)

- Tekenreeks van de gebruikersagent

- Hostnaam

- TARGET URI (met uitzondering van query-tekenreeks)

- Informatie over inhoudstype

- Verwijzende URL-gegevens (met uitzondering van de query-tekenreeks)



> [AZURE.NOTE] De bovenstaande informatie voor HTTP wordt verzameld voor alle niet-gecodeerde verbindingen.
Deze informatie is voor TLS-verbindingen alleen vastgelegd wanneer de instelling 'Grondige inspectie' is ingeschakeld in de portal. De instelling is 'ON' standaard.
Voor meer details, Zie hieronder en [Ophalen van de slag met Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)


Naast de gegevens die door de agent worden verzameld over de activiteit op het netwerk, ook anoniem worden gegevens verzameld over de software en hardware-configuratie, rapporten en informatie over de manier waarop de agent wordt gebruikt.

<br><br>
### <a name="how-the-agent-works"></a>De werking van de agent

De agent-installatie bevat twee onderdelen:

- Een onderdeel van de gebruikersmodus

- Een component in kernel-modus-stuurprogramma (Windows Filtering Platform stuurprogramma)



Als de agent voor het eerst wordt geïnstalleerd wordt een vertrouwd certificaat van de computer opgeslagen op de computer die vervolgens wordt een beveiligde verbinding met de Cloud App Discovery-service. <br>
Haalt de agent regelmatig configuratie van het beleid van de Cloud App Discovery-service via deze beveiligde verbinding. <br>
Het beleid bevat informatie over welke cloud-toepassingen te controleren of Automatische updates moet worden ingeschakeld, onder andere.

Als Internet-verkeer wordt verzonden en op de computer van Internet Explorer en Chrome ontvangen, de agent Cloud App Discovery analyseert het verkeer en de betreffende metagegevens worden uitgepakt (Zie de bovenstaande sectie **gegevens verzameld door de agent** ). <br>
Elke minuut, uploadt de agent u de verzamelde metagegevens aan de Cloud App Discovery-service via een gecodeerd kanaal.

Het stuurprogrammaonderdeel gecodeerd verkeer onderschept en zelf wordt ingevoegd in de versleutelde stroom. Meer informatie in de sectie **gegevens van versleutelde verbindingen (grondige inspectie) Intercepting** hieronder.


### <a name="respecting-user-privacy"></a>Privacy van gebruiker respecteren

Ons doel is om te bieden beheerders de hulpmiddelen voor het instellen van de balans tussen gedetailleerde beeldverwerkingstoepassingen in toepassing gebruiks- en privacy voor hun organisatie. Wij bieden de volgende knoppen op de pagina in de Portal te dien einde:

- **Gegevens verzamelen**: beheerders kunnen kiezen welke toepassingen of ze gegevens ophalen discovery wilt op Toepassingscategorieën opgeven.

- **Grondige inspectie**: beheerders kunnen wilt opgeven als de agent verzamelt HTTP-verkeer voor SSL/TLS-verbindingen (aka **' grondige inspectie '**). Meer hierover in de volgende sectie.

- **Instemming opties**: beheerders kunnen de Cloud App Discovery portal gebruiken om te kiezen of u op de hoogte stellen van de gebruikers van het verzamelen van gegevens door de agent en of de gebruiker toestemming voordat de agent wordt gestart voor het verzamelen van gegevens van de gebruiker.

De Cloud App Discovery eindpunt agent verzamelt alleen de informatie die wordt beschreven in de bovenstaande sectie **gegevens verzameld door de agent** .


### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Gegevens onderschept uit versleutelde verbindingen (grondige inspectie)
Zoals eerder vermeld, kunnen beheerders de agent voor het controleren van gegevens van versleutelde verbindingen ('grondige inspectie') te configureren. TLS ([Transport Layer Security](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) is een van de meest gangbare protocollen in gebruik op het Internet vandaag. Door het versleutelen van communicatie met TLS, kunt een client een beveiligde, persoonlijke communicatiekanaal met een webserver maken TLS biedt essentiële bescherming voor het doorgeven van verificatiereferenties en te voorkomen dat de openbaarmaking van gevoelige informatie.

Terwijl de end-to-end beveiliging gecodeerd kanaal geleverd door TLS belangrijke beveiligings- en privacy kunt, wordt het protocol vaak doelwit voor kwaadwillende of slechte doeleinden. Veel dus in feite wordt dat TLS vaak genoemd "universele firewall-bypass protocol". De oorzaak van het probleem is dat de meeste firewalls kan TLS-communicatie controleren omdat de toepassingslaag gegevens worden versleuteld met SSL. Dit weet, aanvallers regelmatig gebruikmaken van TLS om schadelijke payloads ervoor zorgen dat een gebruiker er zeker van dat zelfs de meest intelligente toepassingslaag firewalls volledig blind aan TLS zijn en gewoon TLS-communicatie tussen hosts moeten sturen. Eindgebruikers kunnen vaak maken gebruik van TLS om toegangscontroles afgedwongen door hun corporate firewalls en proxyservers omzeilen met verbinding maken met openbare proxy's en voor tunneling-protocollen via de firewall die anders mogelijk geblokkeerd door het beleid voor niet-TLS.

Grondige inspectie kan de Cloud App Discovery-agent op te treden als een betrouwbare man-in-the-middle. Wanneer een client wordt aangevraagd voor toegang tot een beveiligde HTTPS-bron wordt het eindpunt Agent-stuurprogramma de verbinding worden onderschept en maakt dat een nieuwe verbinding met de doelserver voor de SSL-certificaat voor de client worden opgehaald. De agent controleert of dat het certificaat kan worden vertrouwd (door controle niet is ingetrokken en het uitvoeren van andere controles certificaat) en, als deze stap, het eindpunt van de Agent vervolgens kopieert de gegevens van het certificaat van de server zijn eigen servercertificaat--als een certificaat wordt onderschept--die gegevens bekend wordt gemaakt. Het certificaat wordt onderschept is ondertekend op het moment door de agent eindpunt met een basiscertificaat dat wordt geïnstalleerd in de vertrouwde Windows-certificaatarchief. Dit certificaat zelfondertekende basiscertificeringsinstantie is gemarkeerd als niet-exporteerbare en is ACL moesten beheerders. Het is bedoeld om nooit laat de computer waarop deze is gemaakt. Wanneer de eindgebruiker clienttoepassing ontvangt het certificaat worden onderschept, zal deze het vertrouwen omdat het met succes de certificaatketen helemaal tot aan het basiscertificaat kunt valideren. Dit proces is meestal doorzichtig uit oogpunt van een eindgebruiker met een paar valkuilen, zoals hieronder beschreven.

Doordat de grondige inspectie de Cloud App Discovery eindpunt Agent decoderen en inspecteren TLS versleutelde communicatie, waardoor de service Ruis reduceren en bieden informatie over het gebruik van de gecodeerde cloud-apps.

#### <a name="a-word-of-caution"></a>Een woord van waarschuwing
Voordat u grondige inspectie inschakelt, wordt sterk aangeraden dat u uw bedoelingen aan de juridische en HR afdelingen communiceren en hun instemming te verkrijgen. Controle van de eindgebruiker particuliere gecodeerde communicatie is een gevoelig onderwerp voor de hand liggende redenen. Controleer of de beveiliging van uw bedrijf voor een productie roll-out van de grondige inspectie, en aanvaardbaar gebruik beleid zijn bijgewerkt om aan te geven dat gecodeerde communicatie worden gecontroleerd. Gebruikers waarschuwen en vrijstelling van gevoelige geacht sites (bijvoorbeeld Bank- en medische sites) kunnen ook nodig zijn als Cloud App detectie als deze is geconfigureerd. Zoals hierboven vermeld, kunnen beheerders de portal Cloud App Discovery kunnen gebruiken te kiezen of u op de hoogte stellen van de gebruikers van het verzamelen van gegevens door de agent en of toestemming van de gebruiker vereist voordat de agent wordt gestart met het verzamelen van gebruikersgegevens.

### <a name="known-issues-and-drawbacks"></a>Bekende problemen voor- en nadelen
Er zijn enkele gevallen waarbij TLS onderschepping ervaring van de eindgebruiker kan beïnvloeden:

- Extended Validation (EV) certificaten weer de adresbalk van de webbrowser groen als een visuele aanwijzing dat u een vertrouwde website bezoeken. TLS inspectie EV niet dupliceren in het certificaat van dat deze service aan de client, zodat de websites die gebruikmaken van EV-certificaten werkt normaal, maar de adresbalk groen niet weergegeven.  

- Openbare sleutel vastzetten (ook wel certificaat vastzetten) zijn ontworpen om gebruikers beschermen tegen man-in-the-middle-aanvallen en rogue certificeringsinstanties. Wanneer het basiscertificaat voor een vastgezette site niet overeenkomt met een van de bekende goede CA, weigert de browser de verbinding met een fout. Deze verbindingen mislukken omdat TLS worden onderschept, in feite een man-in-the-middle.

- Als gebruikers op het hangslotpictogram in de browser adres balk browser om te controleren van de site-informatie, zien ze een reeks die eindigt op de certificeringsinstantie gebruikt voor het ondertekenen van het certificaat van de website niet, maar in plaats daarvan een certificaatketen eindigt met de Windows certificaatarchief Vertrouwde.

Om de oorzaken van deze problemen, wij bijhouden van cloud services en toepassingen bekend met uitgebreide validatie of openbare sleutel vastzetten en de eindpunt-Agent de opdracht geven om te voorkomen dat risico verbindingen te onderscheppen. Zelfs in deze gevallen echter nog steeds ontvangt u rapporten over het gebruik van deze cloud-apps en het volume van de gegevens die worden overgebracht, maar omdat ze niet grondig geïnspecteerd, geen details over het gebruik de apps zijn beschikbaar zal zijn.


## <a name="sending-data-to-cloud-app-discovery"></a>Verzenden van gegevens naar de Cloud App Discovery

Nadat de metagegevens zijn verzameld door de agent, het is in de cache op de computer voor een minuut of tot de gegevens in de cache heeft een grootte van 5MB bereikt. Het is vervolgens gecomprimeerd en via een beveiligde verbinding met de Cloud App Discovery-service verzonden.

Als de agent kan niet communiceren met de Cloud App Discovery-service om enigerlei reden is, wordt de verzamelde metagegevens worden opgeslagen in een lokale cache die alleen toegankelijk voor gebruikers met de juiste machtigingen op de computer (zoals de groep Administrators). <br>
De agent probeert automatisch te verzenden de metagegevens in de cache opgeslagen totdat het is ontvangen door de Cloud App Discovery-service.



## <a name="receiving-the-data-at-the-service-end"></a>Ontvangen van de gegevens aan het einde van de service

De agenten worden geverifieerd bij de ontdekking van de App Cloud service met behulp van de machine specifiek certificaat voor clientverificatie waarnaar hierboven wordt verwezen en de gegevens worden doorgestuurd via een gecodeerd kanaal. <br>
De Cloud App Discovery-service analytics pijpleiding verwerkt metagegevens voor elke klant afzonderlijk het logisch partitioneren in alle stadia van de pijpleiding analytics.
De metagegevens van de geanalyseerde stations verschillende rapporten in de portal.

De onbewerkte metagegevens en de geanalyseerde metagegevens worden opgeslagen voor maximaal 180 dagen. Klanten kunnen ook kiezen voor het vastleggen van de geanalyseerde metagegevens in een account Azure blob opslag van hun keuze.
Dit is handig voor off line analyse van metagegevens ook langer bewaren van de gegevens.

## <a name="accessing-the-data-using-the-azure-portal"></a>Toegang tot de gegevens met behulp van de portal Azure

In een poging om de metagegevens verzameld te beveiligen, hebben alleen globale beheerders van de huurder standaard toegang tot de Cloud App detectiefunctie in Azure portal. <br>
Echter, kunnen beheerders deze toegang tot andere gebruikers of groepen overdragen.



> [AZURE.NOTE] Zie voor meer informatie [Ophalen van de slag met Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

<br>
Elke gebruiker die toegang tot de gegevens in de portal moet beschikken over een licentie Azure AD Premium.



##<a name="additional-resources"></a>Aanvullende bronnen


* [Hoe kan ik verzoek tot niet toegestande cloud-toepassingen die worden gebruikt binnen mijn organisatie ontdekken](active-directory-cloudappdiscovery-whatis.md)
* [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
