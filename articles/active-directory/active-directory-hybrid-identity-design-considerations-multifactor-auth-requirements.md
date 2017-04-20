<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - bepalen meerledige verificatie eisen"
    description="Azure Active Directory gecontroleerd met voorwaardelijke toegang beheren, de specifieke voorwaarden die u kiest bij het verifiëren van de gebruiker en voor het toestaan van toegang tot de toepassing. Zodra deze voorwaarden wordt voldaan, wordt de gebruiker geverifieerd en toegang hebben tot de toepassing."
    documentationCenter=""
    services="active-directory"
    authors="femila"
    manager="billmath"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Meerledige verificatie eisen voor uw identiteit hybride oplossing bepalen

In deze wereld van mobiliteit, met gebruikers met toegang tot gegevens en toepassingen in de cloud en vanaf elk apparaat, deze informatie te beveiligen is uitgegroeid tot grootste.  Elke dag is er een nieuwe kop over de beveiliging wordt doorbroken.  Maar er is geen garantie tegen overtredingen, biedt meerledige verificatie, om te voorkomen dat deze inbreuken op een extra beveiligingslaag.
Begin met het evalueren van de vereisten van organisaties voor meerledige verificatie. Dat wil zeggen, wat de organisatie wil beveiligen.  Deze evaluatie is belangrijk voor het definiëren van de technische voorschriften voor het instellen en gebruikers van de organisaties voor meerledige verificatie inschakelen.

>[AZURE.NOTE]
Als u niet bekend met MVR gesloten en wat het doet bent, is het aangeraden om het artikel te lezen [Wat is Azure meerledige verificatie?](../multi-factor-authentication/multi-factor-authentication.md) vóór het lezen van deze sectie gaan.

Zorg ervoor dat het volgende antwoord:

- Is uw bedrijf bij het beveiligen van Microsoft apps? 
- Hoe deze apps worden gepubliceerd?
- Biedt uw bedrijf RAS medewerkers toegang tot apps in de ruimten toe te staan?

Indien Ja, welk type RAS? U moet ook evalueren waar de gebruikers die toegang hebben tot deze toepassingen worden geplaatst. Deze evaluatie is een andere belangrijke stap voor het definiëren van de strategie van de juiste meerledige verificatie. Zorg ervoor dat de volgende vragen beantwoorden:

- Waar de gebruikers gaat worden gevonden?
- Kunnen ze worden gevonden ergens?
- Uw bedrijf tot stand wilt brengen beperkingen afhankelijk van de locatie van de gebruiker?

Als u eenmaal deze eisen begrijpt, is het belangrijk dat het een evaluatie maken van de gebruikerseisen voor meerledige verificatie. Deze evaluatie is belangrijk omdat het de vereisten voor het implementeren van een meerledige verificatie wilt definiëren. Zorg ervoor dat de volgende vragen beantwoorden:

- De gebruikers bekend zijn met meerledige verificatie?
- Enkele toepassingen vereist zal zijn voor aanvullende verificatie?  
 - Zo ja, de tijd, wanneer die afkomstig zijn van externe netwerken of toegang tot specifieke toepassingen, of onder andere voorwaarden?
- De gebruikers training voor het installeren en implementeren van meerledige verificatie vereist?
- Wat zijn de belangrijkste scenario's die uw bedrijf wil meerledige verificatie inschakelen voor gebruikers?

Na de vorige vragen te beantwoorden, kun je te begrijpen als een meerledige verificatie al geïmplementeerd op gebouwen. Deze evaluatie is belangrijk voor het definiëren van de technische voorschriften voor het instellen en gebruikers van de organisaties voor meerledige verificatie inschakelen. Zorg ervoor dat de volgende vragen beantwoorden:

- Uw bedrijf nodig heeft ter bescherming van beschermde accounts met de MVR gesloten?
- Uw bedrijf nodig heeft om in te schakelen voor bepaalde toepassingen voldoen omwille van de MVR gesloten?
- Moet uw bedrijf MVR gesloten voor alle in aanmerking komende gebruikers alleen beheerders van deze toepassing inschakelen?
- Moet er altijd ingeschakeld MVR gesloten of alleen wanneer de gebruikers buiten uw netwerk zijn aangemeld?


## <a name="next-steps"></a>Volgende stappen
[Een hybride identiteit aangenomen strategie definiëren](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)


## <a name="see-also"></a>Zie ook
[Overzicht van overwegingen bij het ontwerp](active-directory-hybrid-identity-design-considerations-overview.md)
