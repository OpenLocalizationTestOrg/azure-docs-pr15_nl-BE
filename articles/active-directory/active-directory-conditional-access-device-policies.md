<properties
    pageTitle="Voorwaardelijke-apparaatbeleid voor Office 365 services | Microsoft Azure"
    description="Meer informatie over het apparaat op basis van voorwaarden toegang tot Office 365-services. Terwijl de IT-medewerkers (of ervaren kenniswerkers) toegang tot Office 365 services, zoals Exchange en SharePoint Online op het werk of school van hun persoonlijke apparaten wilt, wil hun IT-beheerder de toegang tot zijn beheerders secure.IT voorwaardelijke toegang apparaatbeleid beveiligde bedrijfsnetwerken, terwijl op hetzelfde moment waarmee kenniswerkers op compatibele apparaten toegang krijgen tot de services kunt inrichten."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>
# <a name="conditional-access-device-policies-for-office-365-services"></a>Voorwaardelijke-apparaatbeleid voor services van Office 365

De term "voorwaardelijke toegang tot" heeft veel voorwaarden gekoppeld zoals een geverifieerde gebruiker meerdere factoren, geverifieerd apparaat, compatibele apparaat enz. In dit onderwerp wordt voornamelijk op voorwaarden toegang tot Office 365 diensten op basis van het apparaat focusses. Terwijl de IT-medewerkers (of ervaren kenniswerkers) toegang tot Office 365 services, zoals Exchange en SharePoint Online op het werk of school van hun persoonlijke apparaten wilt, wil de IT-beheerder de toegang tot veilig. IT-beheerders kunnen inrichten apparaatbeleid voorwaardelijke toegang wilt beveiligen, bedrijfsnetwerken, terwijl op hetzelfde moment waarmee kenniswerkers op compatibele apparaten toegang krijgen tot de services. Beleid voor voorwaardelijke toegang tot Office 365 kunnen worden geconfigureerd vanuit de portal voor Microsoft Intune voorwaardelijke toegang.

Azure Active Directory wordt afgedwongen beleid beveiligde toegang tot Office 365 diensten voor voorwaardelijke toegang. Een huurder admin kunt maken van een beleid van voorwaardelijke toegang die een gebruiker op een niet-compatibele apparaat toegang krijgen tot een service O365 blokkeert. De gebruiker moet voldoen aan apparaat van bedrijfsbeleid voordat toegang kan worden verleend aan de service. U kunt ook kunt admin ook een beleid dat afdwingt dat gebruikers alleen hun apparaten toegang krijgen tot een service O365 inschrijven maken. Beleid kunnen toegepast op alle gebruikers van een organisatie, of beperkt tot een paar doelgroepen en steeds meer doelgroepen zijn verbeterd.

Een vereiste voor het afdwingen van apparaatbeleid is bedoeld voor gebruikers die hun apparaten met Azure Active Directory de registratie service registreren. U kunt ervoor kiezen om het meerledige verificatie inschakelen (MVR gesloten) voor apparaten met Azure Active Directory de registratie service registreren. MVR gesloten wordt aanbevolen voor Azure Active Directory de registratie service. Wanneer de MVR gesloten is ingeschakeld, worden gebruikers hun apparaten met Azure Active Directory de registratie service registreren voor de tweede factor verificatie aangevochten.

##<a name="how-does-conditional-access-policy-work"></a>Hoe werkt voorwaardelijke-beleid?

Wanneer een gebruiker aanvragen toegang tot O365 service van een platform ondersteund apparaat, verifieert Azure Active Directory de gebruiker en het apparaat waaruit de gebruiker het verzoek start; en verleent toegang tot de service alleen wanneer de gebruiker aan het beleid instellen voor de service voldoet. Gebruikers die niet hun apparaat ingeschreven krijgt corrigerende instructies over het inschrijven en toegang krijgen tot zakelijke services van O365 compatibel is. Gebruikers op iOS- en Android-apparaten moet inschrijven hun apparaten met toepassing van de bedrijfsportal. Wanneer een gebruiker zich zijn apparaat inschrijft, is het apparaat met Azure Active Directory is geregistreerd en ingeschreven voor Apparaatbeheer en de controle. Klanten moeten de registratieservice Azure Active Directory-apparaat gebruiken in combinatie met Microsoft Intune mobiel Apparaatbeheer voor Office 365-service inschakelen. Apparaat voor inschrijving is spelen voor gebruikers voor toegang tot Office 365 services als apparaatbeleid van kracht zijn.

Wanneer een gebruiker zich met succes zijn apparaat inschrijft, wordt het apparaat vertrouwd. Azure Active Directory biedt Single-Sign-On om toegang tot bedrijfstoepassingen en zorgt voor een beleid van voorwaardelijke toegang om toegang tot een service niet alleen de eerste keer de gebruiker vraagt om toegang, maar elke keer dat de gebruiker verzoekt om toegang te verlengen. De gebruiker wordt toegang tot services geweigerd wanneer referenties worden gewijzigd, apparaat verloren/gestolen is of het beleid niet op het moment van de aanvraag voor vernieuwing is voldaan.

## <a name="deployment-considerations"></a>Aandachtspunten bij de implementatie:
Azure Active Directory de registratie service voor de registratie moet worden gebruikt.

Als gebruikers worden geverifieerd op de gebouwen, wordt Active Directory Federation Services (AD FS) (1.0 en hoger) is vereist. Meerledige verificatie voor werkplek deelnemen mislukt wanneer de identiteitsprovider kan niet meerledige verificatie. AD FS 2.0 is bijvoorbeeld niet meerledige verificatie uitvoeren. Admin huurder moet ervoor zorgen dat de ruimten voor AD FS is geschikt MVR gesloten en geldige methode MVR gesloten voordat de MVR gesloten Azure Active Directory de registratie-service inschakelen is ingeschakeld. AD FS in Windows Server 2012 R2 heeft bijvoorbeeld de mogelijkheden van de MVR gesloten. Voordat u de MVR gesloten op Azure Active Directory de registratie service inschakelt, moet u ook een extra geldig (MVR gesloten)-verificatiemethode op de AD FS-server inschakelen. Zie extra verificatiemethoden configureren voor AD FS voor meer informatie over ondersteunde methoden voor MVR gesloten in AD FS.

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen (FAQ)

V: Wat is het uitsluitingsbeleid standaard voor niet-ondersteund apparaat platforms?

A: op dit moment worden selectief voorwaardelijke-beleid toegepast op gebruikers op iOS- en Android-apparaten. Toepassingen op andere platforms apparaat worden standaard niet beïnvloed door het beleid van voorwaardelijke toegang voor iOS en Android apparaten. Huurder admin, echter kunt overschrijven van het globale beleid voor het weigeren van toegang aan gebruikers op niet-ondersteunde platforms.
Het is op de routekaart voor het beleid van voorwaardelijke toegang aan gebruikers op andere platforms, waaronder Windows uitbreiden.

V: wanneer wordt beleid voorwaardelijke toegang tot Office 365-services worden uitgebreid tot de Browser gebaseerde toepassingen (bijvoorbeeld, OWA, SharePoint gebaseerde browser).

A: op dit moment is de voorwaardelijke toegang tot de Office365 beperkt tot uitgebreide toepassingen op apparaat. Het is op de routekaart voor het beleid van voorwaardelijke toegang aan gebruikers met toegang tot de diensten van browsers uitbreiden.
