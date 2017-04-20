<properties
    pageTitle="Gebruikers toewijzen aan een aangepast domein in Azure Active Directory | Microsoft Azure"
    description="Hoe u een aangepast domein in Azure Active Directory voor gebruikersaccounts opgeeft."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="assign-users-to-a-custom-domain"></a>Gebruikers toewijzen aan een aangepast domein

Nadat u uw aangepaste domein aan Azure Active Directory hebt toegevoegd, moet u de gebruikersaccounts voor dit domein toevoegen zodat u kunt ze daarbij wilt verifiëren.

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>Gebruikers uit een map op het netwerk van uw bedrijf gesynchroniseerd

Als u hebt al een verbinding tussen uw locatie in Active Directory en Azure Active Directory, kan de accounts de synchronisatie invullen. Voor meer informatie over Azure Active Directory synchroniseren met Active Directory op lokalen, Zie [identiteiten in ruimten met Azure Active Directory integreren](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Gebruikers toevoegen en beheren in de cloud

Het domein voor een bestaande gebruikersaccount wijzigen:

1.  Open de Azure klassieke portal met een account die is een globale beheerder of een gebruiker beheerder.

2.  Open de map.

3.  Selecteer het tabblad **gebruikers** .

4.  Selecteer de gebruiker uit de lijst.

5.  Het domein van de gebruiker wijzigen en selecteer vervolgens **Opslaan**.

U kunt dit doen met behulp van [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) of de [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Selecteer een aangepaste domein wanneer u een nieuwe gebruiker

1.  Open de Azure klassieke portal met een account die is een globale beheerder of een gebruiker beheerder.

2.  Open de map.

3.  Selecteer het tabblad **gebruikers** .

4.  Selecteer in de opdrachtbalk **toevoegen**.

5.  Als u de gebruikersnaam van de toevoegt, kiest u uit de lijst met aangepaste domein.

6.  Selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

-   [De ervaring-in voor uw gebruikers vereenvoudigen met behulp van aangepaste domeinnamen](active-directory-add-domain.md)

-   [Aangepaste domeinnamen beheren](active-directory-add-manage-domain-names.md)

-   [Meer informatie over domein management concepten in Azure AD](active-directory-add-domain-concepts.md)
