<properties
    pageTitle="Wat is Microsoft Azure Active Directory-licentieverlening? | Microsoft Azure"
    description="Beschrijving van Microsoft Azure Active Directory-licentieverlening, hoe het werkt, hoe aan de slag en beste praktijken, zoals Office 365, Microsoft Intune, en Azure Active Directory Premium en Basic edities"
    services="active-directory"
      keywords="Azure AD-licentieverlening"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>

# <a name="what-is-microsoft-azure-active-directory-licensing"></a>Wat is Microsoft Azure Active Directory-licentieverlening?

##<a name="description"></a>Beschrijving
Azure Active Directory (AD Azure) van Microsoft Identity is als een oplossing voor Service (IDaaS) en het platform. Azure AD wordt aangeboden in een aantal functionele en technische versies variërend van Azure AD gratis wordt geleverd bij alle Microsoft-services zoals Office 365, Dynamics, Microsoft Intune en Azure (Azure AD genereert geen toeslagen verbruik in deze modus), naar Azure AD betaalde versies zoals Enterprise Mobility Suite (EMS), Azure AD Premium Basic en Azure meerledige verificatie (MVR gesloten). Meest Azure AD betaalde versies worden geleverd via per gebruiker rechten net als veel van Microsoft online services zoals in Office 365 en Intune Microsoft Azure AD. In deze gevallen de aankoop service wordt aangegeven met een of meer abonnementen en elk abonnement bevat een pre-aankoop aantal licenties in uw huurder. De toeslagrechten per gebruiker worden bereikt via de licentie wilt toewijzen, maakt een koppeling tussen de gebruiker en het product, de onderdelen van de service voor de gebruiker inschakelen en een van de vooruitbetaalde licenties verbruikt.

[Probeer nu Azure AD premium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [AZURE.NOTE] Azure AD beheer portal is een onderdeel van de klassieke Azure portal. Tijdens het gebruik van Azure AD Azure inkopen niet nodig, toegang tot deze portal is vereist een actieve Azure-abonnement of een [proefabonnement op Azure](https://azure.microsoft.com/pricing/free-trial/).

Zie [Wat is Azure AD](active-directory-whatis.md)voor een uitgebreid overzicht van de mogelijkheden van Azure AD.
[Meer informatie over de serviceniveaus Azure AD](https://azure.microsoft.com/support/legal/sla/)

> [AZURE.NOTE]  Azure omslagstelsel abonnementen zijn anders: terwijl ook weergegeven in de directory, deze abonnementen kunnen Azure resources maken en deze toewijzen aan uw betalingsmethode. In dit geval zijn er geen licentie-aantal dat is gekoppeld aan het abonnement. Gebruikersorganisatie met het abonnement, de toegang van gebruikers tot het beheren van bronnen abonnement, wordt bereikt door het verlenen van machtigingen voor Azure resources toegewezen aan het abonnement.


##<a name="how-does-azure-ad-licensing-work"></a>Hoe werkt licentieverlening Azure AD samen?

Op basis van een licentie (gebaseerd op het recht) Azure AD services werk door het activeren van een abonnement in de directory-service Azure AD huurder. Zodra het abonnement actief is kunnen de servicemogelijkheden worden beheerd door beheerders van de directory-service en gelicentieerde gebruikers.

Wanneer u aanschaft of Enterprise Mobility Suite, Azure AD Premium of Azure AD Basic activeert, wordt de map bijgewerkt met het abonnement, inclusief geldigheidsperiode en vooruitbetaalde licenties. Uw abonnementsgegevens, inclusief status, volgende lifecycle gebeurtenis en het aantal licenties toegewezen of beschikbaar is beschikbaar via de Azure klassieke portal onder de tab licenties voor de specifieke map. Dit is ook om de beste plaats voor het beheren van uw licentie-toewijzingen.

Elk abonnement bestaat uit een of meer serviceplannen die elke toewijzing van het opgenomen functioneel niveau van het servicetype; bijvoorbeeld: Azure AD MVR gesloten Azure, Microsoft Intune, Online Exchange of SharePoint Online. Niveau beheer service plan vereist Azure AD Licentiebeheer niet. Dit is anders dan Office 365, die gebaseerd op deze geavanceerde configuratie-modus is voor het beheren van toegang tot services opgenomen. Azure AD is gebaseerd op in de configuratie van de service, functies inschakelen en beheren van afzonderlijke machtigingen.

In het algemeen wordt Azure AD abonnementsgegevens beheerd via de Azure klassieke portal onder de tab licenties voor de specifieke map. Azure AD abonnementen, met uitzondering van Azure AD Premium, worden niet in de Office-portal weergegeven.

> [AZURE.IMPORTANT] Azure AD Premium en Basic, en Enterprise Mobility Suite-abonnementen zijn beperkt tot hun ingerichte directory/huurder. Abonnementen kunnen worden verdeeld over verschillende mappen of gebruikt voor gebruikers in andere mappen die recht geven. Een abonnement naar andere mappen verplaatsen is mogelijk maar het verzenden van een support ticket of annulering en inkoop in het geval van rechtstreekse aankopen.

> Bij de aanschaf van Azure AD of Enterprise Mobility Suite via Volume Licensing-abonnement activeren gebeurt automatisch wanneer de overeenkomst bevat andere Microsoft Online services, zoals Office 365.

Azure betaald AD functies omvatten de breedte van de map. Voorbeelden zijn:
- Groep-toewijzing op basis van toepassingen, die is ingeschakeld onder de specifieke toepassing die u beheert.
- Geavanceerde en groep Self-service mogelijkheden zijn beschikbaar onder de mapconfiguratie of binnen een bepaalde groep.
- Premium security rapporten zijn op het tabblad Rapportage
- Wolk toepassing discovery weergegeven in de portal Azure onder identiteit.

###<a name="assigning-licenses"></a>Het toewijzen van licenties
Hoewel het verkrijgen van een abonnement alles wat die u nodig hebt is voor het configureren van betaalde mogelijkheden, moet uw Azure advertentie betaalde functies verspreiden van licenties aan de juiste personen. Elke gebruiker die toegang moeten hebben tot of die wordt beheerd via een Azure advertentie betaalde functie moet in het algemeen een licentie toegewezen. Een licentie-toewijzing is een koppeling tussen een gebruiker en een gekochte service, zoals Azure AD Premium, Basic of Enterprise Mobility Suite.

Het is eenvoudig beheren welke gebruikers in de map moeten een licentie hebben. Worden kan bereikt door toe te wijzen aan een groep toewijzingsregels via de portal Azure AD beheer maken of licenties rechtstreeks naar de juiste personen via een portal, PowerShell of API's toewijst. Wanneer licenties toe te wijzen aan een groep, alle leden van de groep krijgt een licentie. Als gebruikers worden toegevoegd of verwijderd uit de groep deze wordt toegewezen of verwijderd van de juiste licentie. Toewijzingen groeperen kan gebruikmaken van een management groep beschikbaar zijn en in overeenstemming is met de toewijzing op basis van een groep met toepassingen. Met deze aanpak kunt u regels instellen dat alle gebruikers in de map worden automatisch toegewezen of zelfs het besluit tot andere managers in de organisatie delegeren, ervoor te zorgen dat iedereen met de juiste functie een licentie heeft.

Met een licentie op basis van de groep wilt toewijzen, elke gebruiker een gebruikslocatie ontbreekt neemt over de locatie van de map tijdens de toewijzing. Deze locatie kan worden gewijzigd door de beheerder op elk gewenst moment. In gevallen waar de automatische toewijzing is mislukt vanwege weerspiegelt de gebruikersinformatie onder die licentietype die staat.

##<a name="getting-started-with-azure-ad-licensing"></a>Aan de slag met Azure AD-licentieverlening

Aan de slag met Azure AD is eenvoudig; u kunt altijd uw directory als onderdeel van het aanmelden voor een gratis proefperiode Azure maken. [Meer informatie over het aanmelden als organisatie](sign-up-organization.md). De volgende opties kunt u ervoor zorgen dat de map best is uitgelijnd met andere Microsoft-diensten u kan worden verbruikt of van plan bent te gebruiken en uw doelen in het verkrijgen van de service.

Hier zijn een aantal aanbevolen procedures:
- Als u al een van de organisatie-services van Microsoft gebruikt, al hebt u een map Azure AD. In dit geval moet u blijven dezelfde map gebruiken voor andere services, zodat de core identiteitsbeheer, inclusief inrichten en hybride SBF, via de services kan worden gebruikt. Gebruikers hebben een één-aanmeldingsprocedure en profiteren van de mogelijkheden tussen services. Als gevolg hiervan als u koopt een Azure advertentie betaalde service voor uw personeel, wordt aangeraden dat u dezelfde map gebruiken om dit te doen.
- Als u van plan bent Azure AD gebruiken voor een andere set gebruikers (partners, klanten, enzovoort), of als u wilt evalueren AD Azure services en wil dat doen in isolatie van uw productieservice, of als u op zoek bent om in te stellen voor uw services sandbox-omgeving raden wij maken u eerst een nieuwe map via de klassieke Azure Azure-portal. [Meer informatie over het maken van een nieuwe map in de klassieke Azure portal Azure AD](active-directory-licensing-directory-independence.md). De nieuwe map wordt gemaakt met uw account als een externe gebruiker met globale administrator-machtigingen. Wanneer u zich bij de Azure klassieke portal met deze account aanmeldt, worden u kunt deze map bekijken en toegang tot alle directory-beheertaken. Het is raadzaam dat u een lokale account met de juiste toegangsrechten maken voor het beheren van andere Microsoft-services (die niet toegankelijk zijn via de klassieke Azure portal). [Meer informatie over het maken van gebruikersaccounts in AD Azure](active-directory-create-users.md).

> [AZURE.NOTE] Azure AD ondersteunt 'externe gebruikers', waarin de gebruikersaccounts zijn in een exemplaar van Azure AD die zijn gemaakt met behulp van een Microsoft-Account (MSA) of een AD Azure identiteit van een andere map. Terwijl we bezig zijn worden uitbreiding van deze mogelijkheid in alle organisatie-services van Microsoft, nu deze rekeningen niet ondersteund in sommige van de diensten van ervaringen; bijvoorbeeld, ondersteunt het beheer van Office 365 portal momenteel geen deze gebruikers. Hierdoor kunnen externe gebruikers met accounts voor Microsoft niet mogelijk tot het beheer van Office 365 portal, terwijl externe gebruikers van andere AD Azure-directory's worden genegeerd. In dat geval wordt alleen de lokale gebruikersaccount, de Azure advertentie of Office 365 directory waar de gebruiker is gemaakt, toegankelijk via deze ervaringen.

Zoals aangegeven, heeft Azure AD verschillende betaalde versies. Deze versies zijn enkele kleine verschillen in de beschikbaarheid van hun aankoop:


| Product   | EA/VL     | Open  |   CSP |   MPN gebruiksrechten  |   Directe aankoop | Evaluatieversie |
|---|---|---|---|---|---|---|
| Enterprise Mobility Suite |   X | X | X | X |  |      X |
| Azure AD Premium  | X | X | X |   | X | X |
| Azure AD Basic    | X | X | X | X |  <br /> |  <br />  |

###<a name="select-one-or-more-license-trials"></a>Selecteer een of meer licentie proeven
 In alle gevallen kunt u een proefabonnement op Azure AD Premium of Enterprise Mobility Suite activeren door de specifieke proef die u in de map op de tab licenties wilt selecteren. De evaluatieversie bevat een abonnement van 30 dagen met 100 licenties.

![Azure Active Directory proeflicentie plannen](./media/active-directory-licensing-what-is/trial_plans.png)

![Enterprise Mobility Suite proeflicentie plannen](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Actieve proeflicentie plannen](./media/active-directory-licensing-what-is/active_license_trials.png)

###<a name="assign-licenses"></a>Toewijzen van licenties
Zodra het abonnement actief is, moet u een licentie aan uzelf toewijzen en vernieuwen van de browser zodat u ziet alle functies van uw. De volgende stap is betaald voor het toewijzen van licenties voor de gebruikers die moeten worden opgenomen in of Azure AD-functies. Zoals we eerder vermeld bij "Licenties toewijzen", is de beste manier om dit te doen de groep die de gewenste doelgroep identificeren en toe te wijzen aan de licentie; op deze manier wordt gebruikers die worden toegevoegd of verwijderd uit de groep gedurende de levenscyclus toegewezen aan of verwijderd uit de licentie.

Een licentie toewijzen aan een groep of afzonderlijke gebruikers, plan selecteren dat de licentie wilt toewijzen en klik op **toewijzen** op de opdrachtbalk.

![Actieve proeflicentie plannen](./media/active-directory-licensing-what-is/assign_licenses.png)

Eenmaal in het dialoogvenster toewijzing voor het geselecteerde plan, kunt u gebruikers en deze toe te voegen aan de kolom **toewijzen** aan de rechterkant. U kunt de pagina via de lijst met gebruikers of zoeken naar bepaalde personen met het zoeken op de bovenste glas rechts van het raster voor de gebruiker. Groepen toewijzen, selecteert u 'Groepen' in het menu **weergeven** en klik vervolgens op de knop controleren op het recht voor het vernieuwen van de toewijzingen die worden weergegeven.

![Licenties toe te wijzen aan groepen](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

U kunt nu zoeken of pagina door groepen en deze toevoegen aan de kolom **toewijzen** op dezelfde manier. U kunt deze gebruiken voor het toewijzen van een combinatie van gebruikers en groepen in één bewerking. Klik op de knop controleren in de rechterbenedenhoek van de pagina om het toewijzingsproces.

![Voortgangsbericht licentie-toewijzing](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

Wanneer een groep wordt toegewezen, nemen de leden de licenties binnen 30 minuten, maar meestal binnen 1-2 minuten.

Toewijzingsfouten tijdens Azure AD licentie toewijzing kunnen plaatsvinden, maar zijn relatief zeldzaam. Mogelijke toewijzingsfouten zijn beperkt tot:
- Toewijzing conflict - wanneer een gebruiker een licentie die niet compatibel is met de huidige licentie was toegewezen. In dit geval wordt moet de nieuwe licentie toewijzen verwijderen van de vorige.
- Beschikbare licenties overschreden - wanneer het aantal gebruikers in groepen toegewezen beschikbare licenties overschrijden de gebruikers de status van toewijzing weerspiegelt niet toewijzen als gevolg van ontbrekende licenties.

###<a name="view-assigned-licenses"></a>Weergave toegewezen licenties

Een samenvattend overzicht van de toegewezen licenties beschikbaar toegewezen en volgende abonnement lifecycle gebeurtenis inclusief worden weergegeven op de tab **licenties** .

![Het aantal toegewezen licenties weergeven](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

Een gedetailleerde lijst met toegewezen gebruikers en groepen, met inbegrip van de, toewijzingsstatus en het pad (directe of overgenomen uit een of meer groepen) is beschikbaar bij het navigeren in een licentie-plan.

![Gegevens weergeven van de licenties die zijn toegewezen voor een licentie-plan](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Verwijderen van licenties is al net zo eenvoudig als het toewijzen van hen. Rechtstreeks aan de gebruiker is toegewezen of voor een groep toegewezen, kunt u de licentie verwijderen door het selecteren van het licentietype, **verwijderen**, de gebruiker of groep toe te voegen aan de lijst verwijderen en de actie te bevestigen. U kunt ook een licentietype openen, de specifieke gebruiker of groep selecteren en tik op **verwijderen** op de opdrachtbalk. U beëindigt de overname van een licentie uit een groep van een gebruiker, verwijdert u de gebruiker uit de groep.

###<a name="extending-trials"></a>Uitbreiding van de proeven

Evaluatieversie extensies voor klanten beschikbaar zijn als selfservice via de Office 365 portal. De beheerder van een klant kunt navigeren naar de [Office-portal](https://portal.office.com/#Billing) (toegang is afhankelijk van de machtigingen voor de Office-portal) en selecteert u de evaluatieversie Azure AD Premium. Klik op de koppeling voor de **evaluatieversie uitbreiden** en volg de instructies. U moet een creditcard invoeren, maar worden niet afgeschreven.

![Uitbreiding van een proef licentie in de Office-portal](./media/active-directory-licensing-what-is/extend_license_trial.png)

Een evaluatieversie extensie kunnen ook worden aangevraagd door het indienen van een verzoek om ondersteuning. De beheerder van een klant kunt navigeren naar de Office 365 portal [pagina ondersteuning](http://aka.ms/extendAADtrial) (toegang is afhankelijk van de machtigingen voor de pagina van Office ondersteuning). Selecteer "Abonnementen en experimenten" onder functies op deze pagina en "Trial vragen" onder symptoom. Tot slot voert u informatie over de omstandigheden

![Uitbreiding van de evaluatieversie van een licentie met een verzoek om ondersteuning](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Volgende stappen

Nu kan het zijn klaar om te configureren en sommige Azure AD Premium-functies gebruiken.

- [Self-wachtwoord opnieuw instellen](active-directory-manage-passwords.md)
- [Self-service groepsbeheer](active-directory-accessmanagement-self-service-group-management.md)
- [Azure AD verbinden Health](active-directory-aadconnect-health.md)
- [Groep toewijzen aan toepassingen](active-directory-manage-groups.md)
- [Azure meerledige verificatie](../multi-factor-authentication/multi-factor-authentication.md)
- [Rechtstreekse aankoop van Azure AD Premium licenties](http://aka.ms/buyaadp)
