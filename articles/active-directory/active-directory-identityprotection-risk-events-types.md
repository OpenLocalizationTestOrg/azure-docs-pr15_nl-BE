<properties
    pageTitle="Soorten risico's gevonden door Azure Active Directory identiteitsbeveiliging | Microsoft Azure"
    description="Dit onderwerp biedt u een gedetailleerd overzicht van de beschikbare soorten risico's in Azure Active Directory identiteit bescherming"
    services="active-directory"
    keywords="Azure active directory identiteitsbeveiliging, cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingslek, beveiligingsbeleid beheren"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="markvi"/>

#<a name="types-of-risk-events-detected-by-azure-active-directory-identity-protection"></a>Soorten risico's door Azure Active Directory identiteitsbeveiliging gedetecteerd 

In Azure Active Directory identiteit bescherming risico gebeurtenissen zijn gebeurtenissen die:

- zijn gemarkeerd als verdacht

- aangeven dat een identiteit is ingebroken. 

In dit onderwerp kunt u een gedetailleerd overzicht van de beschikbare soorten risico's.


## <a name="leaked-credentials"></a>Uitgelekte referenties

Uitgelekte referenties gevonden door het onderzoeksteam van Microsoft security openbaar geboekt in het donker web. Meestal vindt u deze referenties in tekst zonder opmaak. Ze worden vergeleken met Azure AD referenties, en als er een overeenkomst wordt gevonden, worden gerapporteerd als "Leaked referenties" bescherming van de identiteit.

Risico's worden geclassificeerd als "Hoog" ernst onvoorziene gebeurtenis referenties lekken omdat ze een duidelijke indicatie bieden dat de gebruikersnaam en het wachtwoord beschikbaar voor een aanvaller zijn.

## <a name="impossible-travel-to-atypical-locations"></a>Onmogelijk reizen naar atypische locaties

Dit type risico gebeurtenis geeft twee aanmeldingen uit geografisch afgelegen locaties, waarbij ten minste één van de locaties mogelijk ook atypische voor de gebruiker, gezien het verleden gedrag. Bovendien is is de tijd tussen twee aanmeldingen korter dan de tijd die het zou hebben genomen van de eerste locatie gaan naar de tweede, die aangeeft dat een andere gebruiker dezelfde referenties gebruikt van de gebruiker. 

Deze machine learning algoritme die voor de hand liggende "*valse meldingen*" bij te dragen tot de voorwaarde onmogelijk reizen, zoals VPN's en locaties die regelmatig worden gebruikt door andere gebruikers in de organisatie wordt genegeerd.  Er is een eerste leren periode van 14 dagen waarin deze nieuwe gebruiker aanmelden gedrag aanleren.

Reizen onmogelijk is gewoonlijk een goede indicator dat een hacker kon met succes aanmelden. Echter, false positieven treedt op wanneer een gebruiker met behulp van een nieuw apparaat of via een VPN dat meestal niet wordt gebruikt door andere gebruikers in de organisatie is onderweg. Een andere bron van false positieven is voor toepassingen die niet goed server IP-adressen als client-IP-adressen, waardoor het uiterlijk van aanmeldingen gehouden in het datacenter waar die toepassing van back-end host (vaak dit zijn Microsoft datacenters, waardoor het uiterlijk van aanmeldingen nemen plaats van Microsoft die eigendom zijn van IP-adressen). Het risiconiveau voor deze risicogebeurtenis is als gevolg van deze false positieven, "**Medium**".

##<a name="sign-ins-from-infected-devices"></a>Aanmeldingen van geïnfecteerde apparaten

Dit type risico gebeurtenis geeft aanmeldingen van geïnfecteerd met schadelijke software, apparaten die actief communiceren met een server bot. Dit wordt bepaald door het correleren van IP-adressen van het apparaat van de gebruiker tegen de IP-adressen die in contact met een bot-server zijn. 

Deze risicogebeurtenis geeft IP-adressen, niet Gebruikersapparaten. Als meerdere apparaten achter een enkel IP-adres zijn en alleen sommige worden beheerd door een aanmeldingen vanaf andere apparaten in het netwerk bot mijn trigger deze gebeurtenis onnodig, dat is de reden voor het classificeren van deze risicogebeurtenis die '**laag**'.  

Wij raden aan dat u contact op met de gebruiker, en scan apparaten van de gebruiker. Het is ook mogelijk dat een gebruiker persoonlijke apparaat is geïnfecteerd of zoals eerder gezegd, dat iemand anders een geïnfecteerd apparaat van hetzelfde IP-adres als de gebruiker gebruikt is. Geïnfecteerde apparaten zijn vaak besmet door schadelijke software die nog niet door anti-virus software is geïdentificeerd, en kan ook aangeven als gebruiker slechte gewoonten waardoor het apparaat besmet worden.

Zie de [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409)voor meer informatie over het adres malware-infecties.


## <a name="sign-ins-from-anonymous-ip-addresses"></a>Aanmeldingen van anonieme IP-adressen

Dit type risico gebeurtenis identificeert gebruikers die zich heeft aangemeld vanaf een IP-adres dat is geïdentificeerd als een anonieme proxy IP-adres. Deze proxy's worden gebruikt door mensen die u wilt verbergen IP-adres van het apparaat en kwade bedoelingen kan worden gebruikt.

Wij raden aan dat u onmiddellijk contact op met de gebruiker om te controleren of als ze zijn met behulp van anonieme IP-adressen. Het risiconiveau voor dit type risico gebeurtenis is "**Normaal**" op zich een anoniem IP niet een sterke aanwijzing van een inbreuk op het account.

## <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Aanmeldingen van IP-adressen met verdachte activiteiten

Dit type risico gebeurtenis geeft IP-adressen waarvan een groot aantal mislukte aanmeldingspogingen zijn waargenomen, over meerdere gebruikersaccounts, gedurende een korte periode. Dit komt overeen met de verkeerspatronen van IP-adressen die worden gebruikt door hackers en is een sterke indicator dat accounts al zijn of kunnen worden aangetast. Dit is een machine leren algoritme dat wordt genegeerd voor de hand liggende "*false positieven*", zoals IP-adressen die regelmatig worden gebruikt door andere gebruikers in de organisatie.  Er is een eerste leren periode van 14 dagen waar het leert het gedrag aanmelden van een nieuwe gebruiker en de nieuwe huurder.

Wij raden aan dat u contact op met de gebruiker om te controleren of als zij werkelijk zich aangemeld via een IP-adres dat is gemarkeerd als verdacht. Het risiconiveau voor dit gebeurtenistype is "**Normaal**" omdat verschillende apparaten mogelijk achter hetzelfde IP-adres, terwijl slechts een paar die verantwoordelijk is voor de verdachte activiteiten. 


## <a name="sign-in-from-unfamiliar-locations"></a>Aanmelden van onbekende locaties

Dit type risico gebeurtenis is een real-time-in evaluatiemechanisme acht locaties-in het verleden (IP-, Latitude / lengtegraad en ASN) om te bepalen locaties voor nieuwe / niet bekend. Het systeem slaat informatie op over de vorige locatie die wordt gebruikt door een gebruiker en houdt rekening met deze locaties 'bekend'. Het risico wordt zelfs geactiveerd als het aanmelden vanaf een locatie die nog niet in de lijst met vertrouwde locaties. Er is een eerste leren periode van 14 dagen, gedurende welke zij heeft vlag niet nieuwe locaties als onbekende locaties. Ook negeert het systeem aanmeldingen van bekende apparaten en locaties die geografisch dicht bij een vertrouwde locatie. <br>
Onbekende locaties bieden een sterke aanwijzing dat een aanvaller in staat probeert te gebruiken een identiteit gestolen is. False positieven treedt op wanneer een gebruiker op reis gaat, uitproberen van een nieuw apparaat of een nieuwe VPN wordt gebruikt. Als gevolg van deze vals-positief zijn is het risiconiveau voor dit gebeurtenistype "**Normaal**".


## <a name="azure-ad-anomalous-activity-reports"></a>Azure activiteitenrapporten AD afwijkende

Sommige van deze risico's zijn beschikbaar via de Azure AD afwijkende activiteitenrapporten in Azure portal. In de onderstaande tabel geeft een overzicht van de gebeurtenistypen die verschillende risico's en de bijbehorende **Azure AD afwijkende** activiteitenverslag. Microsoft blijft investeren in deze ruimte, en plannen om voortdurend verbeteren van de nauwkeurigheid van de detectie van bestaande risico's en nieuwe risico gebeurtenistypen voortdurend toevoegen. 



| Identiteit bescherming risico gebeurtenistype | Bijbehorende Azure AD afwijkende activiteitenverslag |
| :-- | :-- |
| Uitgelekte referenties    | Gebruikers met uitgelekte referenties |
| Onmogelijk reizen naar atypische locaties | Onregelmatige activiteit aanmelden |
| Aanmeldingen van geïnfecteerde apparaten    | Aanmeldingen van mogelijk besmette apparaten |
| Aanmeldingen van anonieme IP-adressen  | Aanmeldingen van onbekende bronnen |
| Aanmeldingen van IP-adressen met verdachte activiteiten | Aanmeldingen van IP-adressen met verdachte activiteiten |
| Tekens in van onbekende locaties    | - |
| Gebeurtenissen voor accountvergrendeling    | - |

De volgende Azure AD afwijkende activiteit rapporten worden niet opgenomen als de risico's in Azure AD identiteit bescherming en derhalve niet beschikbaar zal zijn via de bescherming van de identiteit. Deze rapporten zijn nog steeds beschikbaar in de portal Azure echter ze zal worden afgeschaft op een bepaald moment in de toekomst als ze zijn wordt vervangen door de risico's in de bescherming van de identiteit.

- Na meerdere mislukte aanmeldingen
- Aanmeldingen vanaf meerdere locaties


## <a name="see-also"></a>Zie ook

- [Identiteitsbeveiliging Azure Active Directory](active-directory-identityprotection.md)


