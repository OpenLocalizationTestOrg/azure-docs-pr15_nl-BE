<properties
    pageTitle="Azure Active Directory instellen voor beheer van toepassing-selfservice | Microsoft Azure"
    description="Self-service groepsbeheer kan gebruikers maken en beheren van beveiliging of Office 365 groepen in Azure Active Directory en biedt gebruikers de mogelijkheid om op verzoek-beveiligingsgroep of groepslidmaatschappen voor Office 365"
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Azure Active Directory instellen voor het groepsbeheer zelf

Self-service groepsbeheer kan gebruikers beveiligingsgroepen of Office 365 groepen in Azure Active Directory (AD Azure) maken en beheren. Gebruikers kunnen ook aanvragen beveiligingsgroep of Office 365 groepslidmaatschappen en vervolgens de eigenaar van de groep kunt toestaan of weigeren lidmaatschap. Op deze manier kan dagelijkse beheer van groepslidmaatschappen worden overgedragen aan mensen die de zakelijke context voor lidmaatschap begrijpen. Groep zelf functies zijn beschikbaar voor beveiligingsgroepen en Office 365-groepen, maar niet voor het e-mailadres beveiligingsgroepen of distributielijsten.

Management van de groep zelf op dit moment bestaat uit twee belangrijke scenario's: gedelegeerd groepsbeheer en het groepsbeheer zelf.

- **Gedelegeerd groepsbeheer** 
   een voorbeeld is een beheerder die de toegang tot een SaaS-toepassing die van het bedrijf gebruikmaakt beheert. Deze toegangsrechten beheren wordt steeds omslachtig, zodat deze systeembeheerder dit vraagt de eigenaar van het bedrijf een nieuwe groep maken. De beheerder toegang voor de toepassing van de nieuwe groep wordt toegewezen en wordt toegevoegd aan de groep Iedereen al toegang tot de toepassing. De eigenaar van een bedrijf meer gebruikers kunt toevoegen en deze gebruikers automatisch aan de toepassing worden ingericht. Eigenaar van het bedrijf hoeft niet te wachten tot de beheerder voor het beheren van toegang voor gebruikers. Als de beheerder de toestemming aan een manager in een andere groep, verleent en vervolgens die persoon kan ook voor hun eigen gebruikers beheren. Noch de eigenaar van het bedrijf de manager weergeven of beheren van andere gebruikers. De beheerder ziet nog steeds alle gebruikers die toegang tot de toepassing en de toegangsrechten blok hebben als dat nodig is.

- **Self-service groep management** 
   een voorbeeld van dit scenario is twee gebruikers die beide beschikken over SharePoint Online-sites dat ze onafhankelijk van elkaar instellen. Ze willen van andere teams toegang geven tot hun sites. Om dit te bereiken, kunnen ze een groep maken in Azure AD en in SharePoint Online elk van deze groep voor toegang tot hun sites wordt geselecteerd. Wanneer iemand wil toegang, vanuit het Configuratiescherm toegang tot hun verzoek en na goedkeuring zij toegang krijgen tot zowel SharePoint Online-sites automatisch. Een van hen later besluit dat alle personen die toegang hebben tot de site ook toegang tot een bepaalde SaaS-toepassing krijgen moet. De beheerder van de SaaS-toepassing kunt toegangsrechten voor de toepassing op de SharePoint Online-site toevoegen. Alle aanvragen die goedgekeurd krijgt daarna geeft toegang tot twee SharePoint Online-sites en naar deze toepassing SaaS.

## <a name="making-a-group-available-for-end-user-self-service"></a>Een groep maken beschikbaar voor de eindgebruiker Self-service

1. Open de map Azure AD in [Azure klassieke portal](https://manage.windowsazure.com).

2. Stel op het tabblad **configureren** **groepsbeheer overgedragen** op ingeschakeld.

3. **Gebruikers kunnen maken van beveiligingsgroepen** of **gebruikers kunnen Office-groepen maken** ingesteld op ingeschakeld.

Wanneer **dat gebruikers beveiligingsgroepen kunnen maken** is ingeschakeld, kunnen alle gebruikers in de directory nieuwe beveiligingsgroepen maken en leden toevoegen aan deze groepen. Deze nieuwe groepen zou ook weergegeven in het deelvenster voor toegang voor alle gebruikers. Als de instelling van de groep kan kunnen andere gebruikers aanvragen voor deelname aan deze groepen maken. Als **gebruikers beveiligingsgroepen kunnen maken** is uitgeschakeld, kunnen gebruikers geen groepen maken en bestaande groepen waarvan ze eigenaar zijn niet wijzigen. Zij kunnen echter nog steeds het lidmaatschap van deze groepen beheren en verzoeken van andere gebruikers deelnemen aan hun groepen goedkeuren.

Kun je **gebruikers die selfservice voor beveiligingsgroepen kunnen gebruiken** om een meer verfijnde toegangsbeheer via groepsbeheer zelf voor uw gebruikers. Wanneer **dat gebruikers kunnen groepen maken** is ingeschakeld, worden alle gebruikers in de directory voor het maken van nieuwe groepen en leden toevoegen aan deze groepen toegestaan. Ook **gebruikers die selfservice voor beveiligingsgroepen kunnen gebruiken** door op te stellen sommige, u bent beperken groep management aan een beperkte groep gebruikers. Wanneer deze schakeloptie is ingesteld op sommige, moet u gebruikers toevoegen aan de groep SSGMSecurityGroupsUsers kunnen nieuwe groepen maken en leden aan toevoegen. **Gebruikers die de self-service voor beveiligingsgroepen kunnen gebruiken** op alle instelt, kunt u alle gebruikers in de map voor het maken van nieuwe groepen inschakelen.

Kun je het vak **groep die selfservice voor beveiligingsgroepen kunt gebruiken** om een aangepaste naam opgeven voor een groep waarvan de leden self-service kunnen gebruiken.

## <a name="additional-information"></a>Als u meer informatie

Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Toegang tot bronnen te beheren met Azure Active Directory-groepen](active-directory-manage-groups.md)

* [Azure Active Directory cmdlets voor het configureren van de instellingen](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)

* [Wat is Azure Active Directory?](active-directory-whatis.md)

* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
