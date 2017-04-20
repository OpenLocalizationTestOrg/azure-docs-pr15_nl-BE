<properties
    pageTitle="Azure AD en toepassingen: groepen toewijzen aan een toepassing | Microsoft Azure"
    description="Het implementeren van toewijzingen voor Azure toepassingen groeperen."
    services="active-directory"
    documentationCenter=""
    authors="IHenkel"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/03/2015"
    ms.author="inhenk"/>

# <a name="azure-ad-and-applications-assigning-groups-to-an-application"></a>Azure AD en toepassingen: groepen toewijzen aan een toepassing
Voordat u gebruikers en groepen aan een toepassing toewijzen kunt, moet u vereisen dat de gebruiker is toegewezen. Als u wilt weten hoe u Gebruikerstoewijzing vereist, Zie het artikel [Waarvoor de gebruiker is toegewezen](active-directory-applications-guiding-developers-requiring-user-assignment.md) .

In dit artikel wordt ervan uitgegaan dat u al groepen hebt gemaakt in active directory die u voor deze toepassing gebruikt.

## <a name="assigning-groups-to-an-application"></a>Groepen toewijzen aan een toepassing
1. Log in op de Azure portal met een administrator-account.
2. Klik op het artikel voor **Alle Items** in het hoofdmenu.
3. Kies de map die u voor de toepassing gebruikt.
4. Klik op het tabblad **toepassingen** .
5. Selecteer de toepassing uit de lijst met toepassingen die zijn gekoppeld aan deze map.
6. Klik op het tabblad **Gebruikers en groepen** .
7. De lijst met groepen in active directory met behulp van de vervolgkeuzelijst **groepen** filteren.
8. Selecteer de groep.
9. Klik op **toewijzen**.
10. Klik op **Ja** wanneer u wordt gevraagd.

## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
