<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - directory synchronisatie vereisten bepalen | Microsoft Azure"
    description="Identificeren welke vereisten zijn nodig voor het synchroniseren van de gebruikers tussen on = gebouwen en wolken voor de onderneming."
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

# <a name="determine-directory-synchronization-requirements"></a>Vereisten voor directory-synchronisatie
Synchronisatie is alles over het geven van een identiteit in de cloud op basis van hun identiteit op gebouwen. Al dan niet ze gesynchroniseerde account voor verificatie of verificatie federatieve gebruiken, moet de gebruikers nog steeds een identiteit in de cloud.  Deze identiteit moet worden onderhouden en regelmatig bijgewerkt.  De updates kunnen vele vormen, titel wijzigingen aan wachtwoordwijzigingen aannemen.  

Gestart door de organisaties op ruimten identiteit oplossing gebruiker eisen en te evalueren. Deze evaluatie is belangrijk voor het definiëren van de technische voorschriften voor hoe gebruikersidentiteiten worden gemaakt en beheerd in de cloud.  De meeste organisaties, Active Directory is op de gebouwen en de directory op gebouwen die door gebruikers worden door gesynchroniseerd uit, maar in sommige gevallen dit niet het geval zal zijn.  

Zorg ervoor dat de volgende vragen beantwoorden:


- Hebt u één AD forest, meerdere of geen?
 - Hoeveel Azure AD mappen zullen u synchroniseren met?
 
    1. Gebruikt u filteren?
    2. Hebt u meerdere Azure AD verbinding maken met servers gepland?
  
- U hebt een synchronisatie tool op ruimten?
  - Als u Ja kiest, wordt uw gebruikers als gebruikers een virtuele directory/integratie van identiteiten?
- Hebt u een andere map op-ruimten die u wilt synchroniseren (bijvoorbeeld LDAP Directory, HR-database, enz)?
  - Gaat u een GALSync doen?
  - Wat is de huidige status van UPN-namen in uw organisatie? 
  - Hebt u een andere map die gebruikers verifiëren aan de hand?
  - Gebruikt uw bedrijf Microsoft Exchange?
    - Ze dat een hybride exchange deployment plan?

Nu u een idee hebt over uw vereisten voor synchronisatie, moet u bepalen welke tool is het juist aan deze vereisten voldoen.  Microsoft biedt verschillende hulpmiddelen voor het uitvoeren van de directory-integratie en synchronisatie.  Zie de [Vergelijkingstabel van hybride identiteit directory integration tools](active-directory-hybrid-identity-design-considerations-tools-comparison.md) voor meer informatie. 
   
Nu u uw synchronisatie-vereisten en het gereedschap die dit voor uw bedrijf doet hebt, moet u voor de evaluatie van de toepassingen die gebruikmaken van deze adreslijstservices. Deze evaluatie is belangrijk voor het definiëren van de technische voorschriften voor de integratie van deze toepassingen naar de cloud. Zorg ervoor dat de volgende vragen beantwoorden:

- Deze toepassingen worden verplaatst naar de cloud en de map te gebruiken?
- Zijn er speciale kenmerken die worden gesynchroniseerd met de cloud moeten zodat deze toepassingen kunnen worden gebruikt met succes?
- Moeten deze toepassingen opnieuw worden geschreven om te profiteren van de wolk auth?
- Blijft deze toepassingen live op gebouwen terwijl gebruikers toegang krijgen tot de identiteit van de wolk met?

U moet ook bepalen van de vereisten en beperkingen directory beveiligingssynchronisatie. Deze evaluatie is het belangrijk om een lijst van de vereisten die nodig zijn om te maken en onderhouden van de identiteiten van de gebruiker in de cloud. Zorg ervoor dat de volgende vragen beantwoorden:

- Waar de synchronisatieserver zich bevindt?
- Het domein lid worden?
- De server bevindt zich op een beperkt netwerk achter een firewall, zoals een DMZ?
  - U mag de vereiste firewall-poorten voor synchronisatie te openen?
- Hebt u een calamiteitenplan voor de server voor de synchronisatie?
- Hebt u een account met de juiste machtigingen voor alle forests die u wilt synchroniseren met?
 - Als uw bedrijf niet het antwoord op deze vraag weet, Raadpleeg de sectie 'Machtigingen voor Wachtwoordsynchronisatie' in het artikel [de Azure Active Directory Sync-Service installeren](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) en bepalen of u al een account met deze machtigingen of als u wilt maken.
- Als u synchronisatie mutli-forest is de server synchroniseren kunt u voor elk forest?
 
>[AZURE.NOTE]
Zorg ervoor dat de notities van elk antwoord en inzicht in de grondgedachte achter het antwoord. [Bepalen beveiligingsplan eisen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) gaat over de beschikbare opties. Door hebben beantwoord deze vragen u selecteert welke optie u best past bij uw bedrijf nodig heeft.

## <a name="next-steps"></a>Volgende stappen
[Meerledige verificatie eisen bepalen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht van overwegingen bij het ontwerp](active-directory-hybrid-identity-design-considerations-overview.md)
