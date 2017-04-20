<properties
    pageTitle="Geautomatiseerde SaaS App van gebruikers in Azure AD | Microsoft Azure"
    description="Een inleiding tot hoe u kunt Azure AD automatisch inrichten, opgeheven inrichten en continu gebruikersaccounts werken in verschillende toepassingen van derden SaaS."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatiseren van de gebruiker wordt ingericht en Deprovisioning voor SaaS-toepassingen met Azure Active Directory

##<a name="what-is-automated-user-provisioning-for-saas-apps"></a>Wat is Automatische gebruikersaanvragen voor SaaS-toepassingen?

Azure Active Directory (AD Azure), kunt u automatiseren van het maken, onderhouden en verwijderen van gebruikersidentiteiten in toepassingen zoals Dropbox, televergaderingen en ServiceNow wolk ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)).

**Hieronder volgen enkele voorbeelden van wat u kunt doen:**

- Automatisch nieuwe accounts maken bij de rechter SaaS-toepassingen voor nieuwe personen die lid worden van uw team.
- Accounts van SaaS-toepassingen automatisch deactiveren wanneer mensen onvermijdelijk het team verlaten.
- Zorg ervoor dat de identiteiten in uw SaaS-apps zijn bijgewerkt op basis van wijzigingen in de directory.
- Voorziening niet-gebruiker-objecten, zoals groepen voor SaaS-toepassingen die hen ondersteunen.

**Geautomatiseerde gebruikersaanvragen bevat ook de volgende functionaliteit:**

- De mogelijkheid om te voldoen aan de bestaande identiteiten tussen Azure AD en SaaS-apps.
- Aanpassingsopties Azure AD-Help aanpassen aan de huidige configuratie van de SaaS-toepassingen die uw organisatie momenteel wordt gebruikt.
- Optioneel e-mailwaarschuwingen voor het inrichten van fouten.
- Rapportage en activiteit Logboeken om te helpen bij het toezicht op en het oplossen van problemen.

##<a name="why-use-automated-provisioning"></a>Waarom gebruiken geautomatiseerde Provisioning?

Sommige algemene beweegredenen voor het gebruik van deze functie zijn:

- Om te voorkomen dat de kosten, inefficiëntie en menselijke fouten die zijn gekoppeld aan handmatige processen ingericht.
- Uw organisatie beveiligen doordat direct de gebruikers-id's uit belangrijke SaaS apps wanneer zij de organisatie verlaten.
- Eenvoudig importeren een massaal aantal gebruikers in een bepaalde SaaS-toepassing.
- U kunt genieten van het gemak van de inrichting met oplossing uitgevoerd op dezelfde app-beleid dat u hebt gedefinieerd voor Azure AD Single Sign-On.

##<a name="frequently-asked-questions"></a>Veelgestelde vragen

**Hoe vaak Azure AD directorywijzigingen SaaS App schrijven?**

Azure AD wijzigingen gecontroleerd om de vijf tot tien minuten. Als de SaaS-toepassing als resultaat verschillende fouten (zoals in het geval van admin ongeldige referenties), zal vervolgens Azure AD geleidelijk vertragen de frequentie tot eenmaal per dag totdat de fouten zijn opgelost.

**Hoe lang duurt het voordat mijn gebruikers?**

Incrementele wijzigingen vrijwel direct gebeuren, maar als u inrichten de meeste van de map wilt, klik afhankelijk is van het aantal gebruikers en groepen die u hebt. Kleine mappen alleen een paar minuten duren, middelgrote mappen kunnen enkele minuten duren en zeer grote mappen kunnen enkele uren duren.

**Hoe kan ik de voortgang van de huidige taak provisioning bijhouden?**

U kunt het rapport inrichting onder de sectie rapporten van de map bekijken. Een andere mogelijkheid is gaat u naar het tabblad Dashboard voor de SaaS-toepassing die u aan zijn ingericht en zoek in het gedeelte 'integratiestatus' aan de onderkant van de pagina.

**Hoe weet ik als gebruikers niet goed krijgen ingericht?**

Aan het einde van de provisioning configuratie is wizard er een optie om zich abonneren op de e-mailberichten voor fouten bij het inrichten. U kunt ook controleren of het rapport fouten ingericht om te zien welke gebruikers kunnen niet worden ingericht en waarom.

**Kan AD Azure wijzigingen vanuit de SaaS-app terug naar de map schrijven?**

Voor de meeste toepassingen SaaS wordt inrichting alleen uitgaande, wat betekent dat gebruikers de toepassing uit de directory worden geschreven en wijzigingen van de toepassing kunnen niet terug worden geschreven naar de map. Voor een [werkdag](https://msdn.microsoft.com/library/azure/dn762434.aspx)is inrichten echter inkomende alleen-lezen, wat betekent dat gebruikers worden geïmporteerd in de map van de werkdag en ook wijzigingen in de directory doen niet weggeschreven terug naar werkdag.

**Hoe kan ik feedback geven aan het technische team?**

Neem contact met ons opnemen via het [Feedbackforum Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="how-does-automated-provisioning-work"></a>Hoe werkt automatische Provisioning?

Gebruikers kunnen apps SaaS bepalingen Azure AD door verbinding te maken met eindpunten die door elke leverancier wordt ingericht. Eindpunten kunnen Azure AD om via programmering maken, bijwerken en verwijderen van gebruikers. Hieronder vindt u een beknopt overzicht van de verschillende stappen die Azure AD provisioning automatiseren.

1. Als u provisioning voor een toepassing voor de eerste keer inschakelt, worden de volgende acties uitgevoerd:
 - Azure AD probeert te voldoen aan alle bestaande gebruikers in de toepassing SaaS met hun bijbehorende identiteit in de directory. Wanneer een gebruiker is gekoppeld, worden deze *niet* automatisch ingeschakeld voor eenmalige aanmelding. Om een gebruiker toegang tot de toepassing, moeten zij worden expliciet toegewezen App in Azure AD, hetzij rechtstreeks, hetzij via groepslidmaatschap.
 - Als u al hebt opgegeven welke gebruikers de toepassing moeten worden toegewezen, en Azure AD niet kan vinden bestaande accounts voor gebruikers, zal Azure AD inrichten van nieuwe accounts voor hen in de toepassing.
2. Zodra de initiële synchronisatie is voltooid zoals hierboven beschreven, controleert Azure AD elke 10 minuten voor de volgende wijzigingen:
 - Als u nieuwe gebruikers zijn toegewezen aan de toepassing (rechtstreeks of via een groepslidmaatschap), vervolgens worden ze voorzien van een nieuwe account in de SaaS-app.
 - Als toegang van een gebruiker is verwijderd, wordt de account in de SaaS-app wordt gemarkeerd als uitgeschakeld (gebruikers zijn nooit volledig verwijderd, die u beschermt tegen gegevensverlies in geval van een onjuiste configuratie).
 - Als een gebruiker onlangs is toegewezen aan de toepassing en ze had al een account in de SaaS-app, die rekening wordt gemarkeerd als ingeschakeld en worden bepaalde eigenschappen van de gebruiker mogelijk bijgewerkt als ze verouderd ten opzichte van de map zijn.
 - Als u gegevens van een gebruiker (zoals het telefoonnummer, de kantoorlocatie, enzovoort) is gewijzigd in Active directory, wordt die informatie ook in de SaaS-toepassing bijgewerkt automatisch.

Voor meer informatie over hoe de kenmerken worden toegewezen tussen AD Azure en uw SaaS-app, Zie het artikel op [Kenmerktoewijzingen aanpassen](active-directory-saas-customizing-attribute-mappings.md).

##<a name="list-of-apps-that-support-automated-user-provisioning"></a>Lijst van Apps die ondersteuning bieden voor Automatische gebruikersaanvragen

Klik op een app voor een zelfstudie bekijken over het configureren van geautomatiseerde provisioning voor:

- [Vak](http://go.microsoft.com/fwlink/?LinkId=286016)
- [Citrix GoToMeeting](http://go.microsoft.com/fwlink/?LinkId=309580)
- [Toegeven](http://go.microsoft.com/fwlink/?LinkId=309575)
- [Docusign](http://go.microsoft.com/fwlink/?LinkId=403254)
- [Dropbox voor bedrijven](http://go.microsoft.com/fwlink/?LinkId=309581)
- [Google Apps](http://go.microsoft.com/fwlink/?LinkId=309577)
- [Jive](http://go.microsoft.com/fwlink/?LinkId=309591)
- [Televergaderingen](http://go.microsoft.com/fwlink/?LinkId=286017)
- [Sandbox-televergaderingen](http://go.microsoft.com/fwlink/?LinkId=327869)
- [ServiceNow](http://go.microsoft.com/fwlink/?LinkId=309587)
- [Werkdag](http://go.microsoft.com/fwlink/?LinkId=690250) (inkomende inrichting)

Om een toepassing voor de ondersteuning van Automatische gebruikersaanvragen, verstrekt zij de nodige eindpunten waarmee externe programma's voor het automatiseren van het maken, onderhouden en verwijderen van gebruikers. Daarom zijn niet alle SaaS apps compatibel met deze functie. Het technische team Azure AD zijn vervolgens een provisioning verbindingslijn om deze apps te bouwen voor apps die dit ondersteunen, en dit werk is gerangschikt naar prioriteitsvolgorde van de behoeften van de huidige en potentiële klanten.

Team voor het aanvragen van ondersteuning voor extra toepassingen inrichten contact opnemen met de technische Azure AD verzend een bericht via het [Feedbackforum Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Kenmerktoewijzingen voor gebruikersaanvragen aanpassen](active-directory-saas-customizing-attribute-mappings.md)
- [Het schrijven van expressies voor het toewijzen van kenmerken](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Filters voor gebruikersaanvragen scoping](active-directory-saas-scoping-filters.md)
- [Met behulp van SCIM automatisch inrichten van gebruikers en groepen van Azure Active Directory voor toepassingen inschakelen](active-directory-scim-provisioning.md)
- [Account meldingen inrichten](active-directory-saas-account-provisioning-notifications.md)
- [Lijst met zelfstudies over het integreren van SaaS-toepassingen](active-directory-saas-tutorial-list.md)
