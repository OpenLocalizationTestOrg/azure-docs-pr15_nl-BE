<properties
    pageTitle="-In-ervaringen met Azure AD identiteitsbeveiliging | Microsoft Azure"
    description="Een overzicht van de gebruikerservaring als identiteitsbeveiliging heeft beperkt of een gebruiker verholpen of als meerledige verificatie door een beleid is vereist."
    services="active-directory"
    keywords="Azure active directory identiteitsbeveiliging, cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingslek, beveiligingsbeleid beheren"
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
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>-In ervaringen met Azure AD identiteit bescherming

U kunt met Azure Active Directory identiteit bescherming:

- gebruikers moeten zich registreren voor een meerledige verificatie

- riskante aanmeldingen en gemanipuleerde gebruikers verwerken

De reactie van het systeem op deze problemen invloed is op een teken in gebruikerservaring omdat alleen direct aanmelden door een gebruikersnaam en wachtwoord zijn niet mogelijk niet meer. Er zijn extra stappen vereist om een gebruiker veilig weer in bedrijf.

Dit onderwerp bevat een overzicht van de gebruiker aanmelden ervaring voor alle gevallen die zich kunnen voordoen.

**Meerledige verificatie**

- Meerledige verificatie registratie



**Aanmelden voor risico**

- Riskante recovery-in

- Riskante aanmelden geblokkeerd

- Registratie meerledige verificatie tijdens een riskante aanmelden
 

**Gebruiker risico**

- Accountherstel beschadigde

- Gemanipuleerde account geblokkeerd




## <a name="multi-factor-authentication-registration"></a>Meerledige verificatie registratie

De beste gebruikerservaring voor zowel de account die oneigenlijk herstel en de riskante stroom-in is als de gebruiker zelf kan herstellen. Als gebruikers worden geregistreerd voor een meerledige verificatie, al ze een nummer dat is gekoppeld aan de account die kan worden gebruikt voor het doorgeven van veiligheidsproblemen. Geen help desk of beheerder betrokkenheid is nodig om te herstellen van de inbreuk rekening. Dus is het raadzaam om geregistreerd voor meerledige verificatie van gebruikers. 

Beheerders kunnen:

- een beleid instellen dat gebruikers stellen hun accounts voor extra beveiliging verificatie is vereist. 
- kunnen meerledige verificatie registratie overslaan voor maximaal 30 dagen in het geval ze gebruikers geven een respijtperiode wilt voordat u zich registreert.

**De registratie meerledige verificatie omvat drie stappen:**

1. In de eerste stap krijgt de gebruiker een melding over de vereisten voor het instellen van de account voor een meerledige verificatie. 

    ![Herstel] (./media/active-directory-identityprotection-flows/140.png "Herstel")


2. Meerledige verificatie als u wilt instellen, moet u laten weten hoe u gecontacteerd wenst te worden.

    ![Herstel] (./media/active-directory-identityprotection-flows/141.png "Herstel")
 
3. Het systeem dient een uitdaging voor u en u moet reageren.

    ![Herstel] (./media/active-directory-identityprotection-flows/142.png "Herstel")

 



## <a name="risky-sign-in-recovery"></a>Riskante recovery-in

Wanneer een beheerder heeft een beleid voor aanmelden risico's worden geconfigureerd, worden de betreffende gebruikers gemeld wanneer ze proberen om aan te melden. 

**De stroom voor risicovolle-in bestaat uit twee stappen:** 

1. De gebruiker krijgt de melding dat iets vreemds over hun aanmelden ontdekt, zoals het aanmelden van een nieuwe locatie, het apparaat of de app. 

    ![Herstel] (./media/active-directory-identityprotection-flows/120.png "Herstel")

2. De gebruiker moet hun identiteit aantonen door het oplossen van een uitdaging voor de beveiliging. Als de gebruiker is geregistreerd voor een meerledige verificatie moeten ze ommetje een beveiligingscode naar hun telefoonnummer. Aangezien dit een gewoon een riskante inloggen en niet een account die oneigenlijk, de gebruiker hoeft te wijzigen van het wachtwoord in deze stroom. 

    ![Herstel] (./media/active-directory-identityprotection-flows/121.png "Herstel")



 
## <a name="risky-sign-in-blocked"></a>Riskante aanmelden geblokkeerd
Beheerders kunt ook een risico-In-beleid instellen voor voorkomen dat gebruikers bij het aanmelden afhankelijk van het risiconiveau. Als u de blokkering is opgeheven, eindgebruikers moet contact opnemen met een netwerkbeheerder of de helpdesk, of ze kunnen probeer aan te melden vanaf een vertrouwde locatie of apparaat. Zelf herstellen door met het oplossen van een meerledige verificatie is geen optie in dit geval.

![Herstel] (./media/active-directory-identityprotection-flows/200.png "Herstel")




## <a name="compromised-account-recovery"></a>Accountherstel beschadigde

Wanneer een beveiligingsbeleid voor risico van gebruiker is geconfigureerd, risico-niveau dat is opgegeven in het beleid gebruikers die voldoen aan de gebruiker (en wordt daarom verondersteld dat gevaar) door de gebruiker inbreuk herstel stroom moet gaan voordat ze kunnen aanmelden. 

**De gebruiker inbreuk herstel stroom omvat drie stappen:**

1. De gebruiker krijgt de melding dat de accountbeveiliging van hun risico vanwege verdachte activiteit loopt of referenties uitgelekt.

    ![Herstel] (./media/active-directory-identityprotection-flows/101.png "Herstel")

2.  De gebruiker moet hun identiteit aantonen door het oplossen van een uitdaging voor de beveiliging. De gebruiker is geregistreerd voor een meerledige verificatie kan zichzelf herstellen kunnen worden achterhaald. Zij moeten ommetje een beveiligingscode naar hun telefoonnummer. 

    ![Herstel] (./media/active-directory-identityprotection-flows/110.png "Herstel")


3.  Ten slotte wordt de gebruiker gedwongen hun wachtwoord wijzigen omdat iemand anders toegang tot hun account hebben gehad. Screenshots van deze ervaring staan hieronder.
 
    ![Herstel] (./media/active-directory-identityprotection-flows/111.png "Herstel")



## <a name="compromised-account-blocked"></a>Gemanipuleerde account geblokkeerd 

Als u een gebruiker die is geblokkeerd door een risico gebruikersbeveiligingsbeleid gedeblokkeerd, moet de gebruiker contact opnemen met een beheerder of helpdesk. Zelf herstellen door met het oplossen van een meerledige verificatie is geen optie in dit geval.


![Herstel] (./media/active-directory-identityprotection-flows/104.png "Herstel")



 
## <a name="reset-password"></a>Wachtwoord opnieuw instellen

Als gemanipuleerde gebruikers aanmelden zijn geblokkeerd, kan een beheerder een tijdelijk wachtwoord voor hen genereren. De gebruikers moeten hun wachtwoord wijzigen tijdens een volgende aanmelden.

![Herstel] (./media/active-directory-identityprotection-flows/160.png "Herstel")


 




 

## <a name="see-also"></a>Zie ook

- [Identiteitsbeveiliging Azure Active Directory](active-directory-identityprotection.md) 