<properties
    pageTitle="Groepen in Azure Active Directory beheren | Microsoft Azure"
    description="Informatie over het maken en beheren van groepen Azure Azure Active Directory-gebruikers beheren."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/29/2016"
    ms.author="curtand"/>


# <a name="managing-groups-in-azure-active-directory"></a>Groepen in Azure Active Directory beheren

> [AZURE.SELECTOR]
- [Azure portal](active-directory-groups-create-azure-portal.md)
- [Azure klassieke portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)


Een van de functies van Gebruikersbeheer Azure Active Directory (AD Azure) is de mogelijkheid om groepen gebruikers te maken. Met een groep kunt u beheertaken uitvoeren, zoals het toewijzen van licenties of machtigingen voor een aantal gebruikers tegelijk. Ook kunt u groepen-machtiging toewijzen

- Bronnen, zoals objecten in de directory
- Bronnen buiten de map zoals SaaS-toepassingen, Azure services, SharePoint-sites of bronnen van gebouwen

De eigenaar van een resource kan bovendien ook toegang toewijzen aan een resource aan een Azure AD-groep die eigendom zijn van iemand anders. Deze toewijzing geeft de leden van die groepstoegang tot de bron. Vervolgens, de eigenaar van de groep beheert het lidmaatschap van de groep. Effectief, delegeert de eigenaar van de resource aan de eigenaar van de groep de machtiging gebruikers toe te wijzen aan de resource.

## <a name="how-do-i-create-a-group"></a>Hoe maak ik een groep?

Afhankelijk van de services die uw organisatie zich heeft geabonneerd, kunt u een groep met behulp van een van de volgende opties:
- de klassieke Azure portal
- de Office 365-account portal
- de Windows Intune account portal

Beschreven taken zoals in de klassieke Azure portal wordt uitgevoerd. Voor meer informatie over het gebruik van niet-Azure portals voor het beheren van de directory AD Azure, Zie [beheren van uw directory Azure AD](active-directory-administer.md).

1. Selecteer **Active Directory**in de [Azure klassieke portal](https://manage.windowsazure.com)en selecteert u de naam van de map voor uw organisatie.

2. Selecteer het tabblad **groepen** .

3. Selecteer **groep toevoegen**.

4. Geef de naam en de beschrijving van een groep in het venster **Groep toevoegen** .


## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Hoe ik toevoegen of verwijderen van individuele gebruikers in een groep?

**Een individuele gebruiker aan een groep toevoegen**

1. Selecteer **Active Directory**in de [Azure klassieke portal](https://manage.windowsazure.com)en selecteert u de naam van de map voor uw organisatie.

2. Selecteer het tabblad **groepen** .

3. Open de groep waaraan u leden wilt toevoegen. Open het tabblad **leden** van de geselecteerde groep als deze niet al wordt weergegeven.

4. Selecteer **leden toevoegen**.

5. Selecteer de naam van de gebruiker of groep die u wilt toevoegen als lid van deze groep op de pagina **Leden toevoegen** . Zorg ervoor dat deze naam wordt toegevoegd aan het **geselecteerde** deelvenster.


**Een gebruiker uit een groep verwijderen**

1. Selecteer **Active Directory**in de [Azure klassieke portal](https://manage.windowsazure.com)en selecteert u de naam van de map voor uw organisatie.

2. Selecteer het tabblad **groepen** .

3. Open de groep waaruit u leden wilt verwijderen.

4. Selecteer het tabblad **leden** , selecteert u de naam van het lid dat u wilt verwijderen uit deze groep en klik vervolgens op **verwijderen**.

6. Achter de prompt bevestigen dat u wilt verwijderen van dit lid uit de groep.


## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>Hoe kan ik het lidmaatschap van een groep dynamisch beheren?

In Azure AD, kunt u heel eenvoudig instellen van een eenvoudige regel om te bepalen welke gebruikers lid zijn van de groep zijn. Een eenvoudige regel is dat slechts een enkele vergelijking maakt. Bijvoorbeeld, als een groep wordt toegewezen aan een SaaS-toepassing, kunt u instellen van een regel toe te voegen gebruikers die beschikken over een functie van "Vertegenwoordiger". Deze regel vervolgens verleent toegang tot deze SaaS-toepassing voor alle gebruikers met die functie in de map.

Wanneer het systeem evalueert alle kenmerken van een gebruiker, worden alle dynamische groep regels in een map te zien als de wijziging van de gebruiker een groep wordt toegevoegd of verwijderd. Als een gebruiker voldoet aan een regel op een groep, worden ze als lid toegevoegd aan die groep. Als ze niet langer voldoen aan de regel van een groep die een lid van zijn, worden deze verwijderd als een lid van die groep.

> [AZURE.NOTE] U kunt een regel voor dynamische lidmaatschap van beveiligingsgroepen of Office 365 groepen instellen. Lidmaatschap van geneste groepen worden niet die momenteel voor de toewijzing op basis van een groep met toepassingen ondersteund.
>
> Dynamische lidmaatschappen voor groepen vereisen een Azure AD Premium licentie moet worden toegewezen aan
>
> - De beheerder die de regel op een groep beheert
> - Alle leden van de groep

**Lidmaatschap van een groep van dynamische inschakelen**

1. Selecteer **Active Directory**in de [Azure klassieke portal](https://manage.windowsazure.com)en selecteert u de naam van de map voor uw organisatie.

2. Klik op het tabblad **groepen** en open de groep die u wilt bewerken.

3. Selecteer in het tabblad **configureren** en **Inschakelen van dynamische lidmaatschappen** wordt ingesteld op **Ja**.

4. Een eenvoudige één regel voor de groep om te bepalen hoe dynamische lidmaatschap voor deze groepsfuncties instellen. Zorg ervoor dat de **gebruikers toevoegen waar** optie is geselecteerd en selecteer vervolgens een eigenschap uit de lijst (bijvoorbeeld afdeling, functie, enz.)

5. Selecteer een voorwaarde (niet gelijk aan, is gelijk aan, niet begint met, begint met, niet bevat, bevat, niet overeenkomen, overeenkomen).

6. Geef een vergelijkingswaarde voor de geselecteerde eigenschap.

Zie meer informatie over het maken van *Geavanceerde* regels (regels met meerdere vergelijkingen) voor het lidmaatschap van dynamische, [kenmerken gebruiken om geavanceerde regels te maken](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>Als u meer informatie

Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Toegang tot bronnen te beheren met Azure Active Directory-groepen](active-directory-manage-groups.md)

* [Azure Active Directory cmdlets voor het configureren van de instellingen](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)

* [Wat is Azure Active Directory?](active-directory-whatis.md)

* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
