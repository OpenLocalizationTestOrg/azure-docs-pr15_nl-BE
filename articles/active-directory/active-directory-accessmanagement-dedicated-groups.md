<properties
    pageTitle="Speciale groepen in Azure Active Directory | Microsoft Azure"
    description="Overzicht van hoe specifieke groepen werken in Azure Active Directory en hoe ze worden gemaakt."
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

# <a name="dedicated-groups-in-azure-active-directory"></a>Speciale groepen in Azure Active Directory

In Azure Active Directory (AD Azure), de functie van de specifieke groepen automatisch gemaakt en gevuld lidmaatschap voor Azure AD vooraf gedefinieerde groepen. Leden van specifieke groepen kunnen niet worden toegevoegd of verwijderd met behulp van de Windows PowerShell-cmdlets, Azure klassieke portal of via programmacode.

>[AZURE.NOTE] Speciale groepen vereisen dat een Azure AD Premium licentie is toegewezen aan
>- de beheerder die de regel op een groep beheert
>- alle gebruikers die lid zijn van de groep worden geselecteerd door de regel

**Speciale groepen inschakelen**

1. Selecteer **Active Directory**in de [Azure klassieke portal](https://manage.windowsazure.com)en open directory van uw organisatie.

2. Klik op het tabblad **groepen** , en open vervolgens de groep die u wilt bewerken.

3. Selecteer in het tabblad **configureren** en **Toegewezen groepen inschakelen** wordt ingesteld op **Ja**.

Na het inschakelen van specifieke groepen switch is ingesteld op **Ja**, verder kunt u de map automatisch de speciale groep alle gebruikers maken door de **inschakelen 'Alle gebruikers' groep** overschakelen naar **Ja**. U kunt vervolgens ook bewerken de naam van deze specifieke groep door te typen in het **weergegeven naam voor 'Alle gebruikers' groep** veld.

De groep alle gebruikers kan worden gebruikt om dezelfde machtigingen toewijzen aan alle gebruikers in de directory. Zo kunt u alle gebruikers in uw Active directory-toegang tot een toepassing SaaS verlenen toegang tot de gespecialiseerde groep alle gebruikers toewijzen aan deze toepassing.

De speciale groep alle gebruikers bevat alle gebruikers in de directory, inclusief gasten en externe gebruikers. Als u een groep die externe gebruikers worden uitgesloten en u kunt dit doen door een groep maken met een kenmerk gebaseerde dynamische regel de volgende:

                (user.userPrincipalName -notContains "#EXT#@")

Voor een groep die alle gasten worden uitgesloten, met een regel als volgt:

                (user.userType -ne "Guest")

Zie meer informatie over het maken van *Geavanceerde* regels (regels met meerdere vergelijkingen) voor het lidmaatschap van dynamische, [kenmerken gebruiken om geavanceerde regels te maken](active-directory-accessmanagement-groups-with-advanced-rules.md).


Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Toegang tot bronnen te beheren met Azure Active Directory-groepen](active-directory-manage-groups.md)
* [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
* [Wat is Azure Active Directory?](active-directory-whatis.md)
* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
