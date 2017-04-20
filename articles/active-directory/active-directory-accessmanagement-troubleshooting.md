
<properties
    pageTitle="Problemen met dynamische lidmaatschap voor groepen | Microsoft Azure"
    description="Tips voor probleemoplossing voor dynamische lidmaatschap voor groepen in AD Azure."
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
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Problemen met dynamische lidmaatschappen voor groepen

**Ik een regel op een groep hebt geconfigureerd, maar geen lidmaatschappen worden bijgewerkt in de groep**<br/>Controleer of de instelling **inschakelen gedelegeerd groepsbeheer** is ingesteld op **Ja** in het tabblad **configureren** . Ziet u deze instelling alleen als u bent aangemeld als een gebruiker aan wie een Azure Active Directory Premium licentie is toegewezen. Controleer of de waarden voor gebruikerskenmerken op de regel: zijn er gebruikers die voldoen aan de regel?

**Ik heb een regel hebt geconfigureerd, maar nu de bestaande leden van de regel verwijderd**<br/>Dit is normaal. Bestaande leden van de groep worden verwijderd wanneer een regel wordt ingeschakeld of gewijzigd. De gebruikers die worden geretourneerd uit de evaluatie van de regel worden toegevoegd als leden aan de groep.     

**Ik zie geen wijziging van het lidmaatschap direct als ik toevoegen of wijzigen van een regel, waarom niet?**<br/>Van gespecialiseerde lidmaatschapsevaluatie doet periodiek een asynchrone achtergrondproces. Hoe lang dit duurt, wordt bepaald door het aantal gebruikers in de directory en de grootte van de groep gemaakt als gevolg van de regel. Mappen met een klein aantal gebruikers verschijnt normaal gesproken de wijzigingen in groepslidmaatschappen in minder dan een paar minuten. Mappen met een groot aantal gebruikers kunnen duren 30 minuten of langer om in te vullen.

Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Toegang tot bronnen te beheren met Azure Active Directory-groepen](active-directory-manage-groups.md)
* [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
* [Wat is Azure Active Directory?](active-directory-whatis.md)
* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
