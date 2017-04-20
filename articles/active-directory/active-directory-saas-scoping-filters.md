<properties
    pageTitle="Op basis van het kenmerk app ingericht met scopes filters | Microsoft Azure"
    description="Informatie over het scope-filters worden gebruikt om te voorkomen dat objecten in toepassingen die ondersteuning bieden voor Automatische gebruikersaanvragen vanuit daadwerkelijk wordt ingericht als een object niet voldoet aan uw zakelijke behoeften."
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


# <a name="attribute-based-app-provisioning-with-scoping-filters"></a>Op basis van het kenmerk app ingericht met scopes filters

Het doel van deze sectie is wordt uitgelegd hoe u de scope filters definiëren op basis van het kenmerk regels die bepalen welke gebruikers de gegevens naar de toepassing.





## <a name="clauses-and-scope-groups"></a>Componenten en Scope-groepen


![Scoping Filter][1] 




Scope-filters worden gedefinieerd door een of meer **bereikgroepen**, die elk een of meer **componenten**bevatten. Overzicht van de componenten voor een bepaald bereikgroep door te klikken op de pijl links van de naam van de groep weer.

Een **clausule** bepaalt u welke gebruikers de mogelijkheid de scope filter passeren door kenmerken van de gebruiker te evalueren. Bijvoorbeeld, wellicht een van de componenten die vereist dat een gebruikerskenmerk "staat" gelijk is aan New York, wat betekent dat alleen de gebruikers van de New York in de toepassing zal worden ingericht.

![De naam van scopes][2] 



Elke **bereikgroep** begint met een verplichte **component**, zoals in de screenshot hierboven. Deze clausule staat gewoon dat de gebruiker moet eerst worden toegewezen aan de toepassing voordat het wordt geëvalueerd door de filters van de scope. Deze component kan niet worden verwijderd of gewijzigd.

U kunt nieuwe componenten of nieuwe scope-groepen toevoegen door op de juiste knop te drukken. U kunt elke bereikgroep een naam geven door de eigenschap **Naam bereik** te bewerken.





## <a name="how-scoping-filters-are-evaluated"></a>Hoe Filters scopes worden geëvalueerd

Tijdens het inrichten testen we elke toegewezen gebruiker tegen uw scope filters om te bepalen als die gebruiker toegang tot de toepassing verdient. U kunt zien van elk component als een test om de gebruiker te voorkomen getting uitgefilterd worden doorgegeven. 

Als er meerdere scope-groepen die zijn gedefinieerd, moet iedere gebruiker ten minste één doorgeven voor toegang tot de toepassing. Binnen elke bereikgroep, de gebruiker moet doorgeven om één component te geven die specifiek bereikgroep. 

Met andere woorden, u kunt zien van bereikgroepen als samen wilt of kunt u denkt dat deze clausules in deze en zou samen. Bekijk de scope filter hieronder:


![De naam van scopes][2]  


Volgens deze scope filter moeten de gebruikers de volgende criteria voldoen om te worden ingericht:

1. Ze moeten worden toegewezen aan de toepassing.

2. Ze moeten werken op de afdeling Engineering

3. Zij moeten werken in San Francisco of Canada.


##<a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Automatiseren van de gebruiker wordt ingericht en SaaS-toepassingen Deprovisioning](active-directory-saas-app-provisioning.md)
- [Kenmerktoewijzingen voor gebruikersaanvragen aanpassen](active-directory-saas-customizing-attribute-mappings.md)
- [Het schrijven van expressies voor het toewijzen van kenmerken](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Account meldingen inrichten](active-directory-saas-account-provisioning-notifications.md)
- [Met behulp van SCIM automatisch inrichten van gebruikers en groepen van Azure Active Directory voor toepassingen inschakelen](active-directory-scim-provisioning.md)
- [Lijst met zelfstudies over het integreren van SaaS-toepassingen](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png
