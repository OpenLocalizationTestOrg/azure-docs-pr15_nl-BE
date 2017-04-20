<properties
    pageTitle="Machtigingen beheren voor bronnen per gebruiker in Azure stapel (de beheerder en huurder) | Microsoft Azure"
    description="Informatie over het beheren van machtigingen voor bronnen per gebruiker als een beheerder of de huurder."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="manage-user-permissions"></a>Gebruikersmachtigingen beheren

Een gebruiker in de stapel Azure is een lezer, de eigenaar of de inzender voor elk exemplaar van een abonnement, resourcegroep of service. Gebruiker A kan bijvoorbeeld leesmachtigingen op 1 abonnement hebben maar eigenaar gemachtigd tot en met 7 van de virtuele Machine.

-   Lezer: Gebruiker alles kunt bekijken maar geen wijzigingen kunt aanbrengen.

-   Inzender: Gebruiker, kan alles behalve toegang tot bronnen te beheren.

-   Eigenaar: Gebruiker, kan alles, met inbegrip van toegang tot bronnen te beheren.


## <a name="set-access-permissions-for-a-user"></a>Machtigingen instellen voor een gebruiker

1.  Meld u aan met een account die eigenaarsmachtigingen heeft voor de resource die u wilt beheren.

2.  In het blad voor de resource, klikt u op het pictogram van **Access** ![](media/azure-stack-manage-permissions/image1.png).

3.  Klik in de blade **gebruikers** **rollen**.

4.  Klik op **toevoegen** om de machtigingen voor de gebruiker in het blad **rollen** .

## <a name="next-steps"></a>Volgende stappen

[Een huurder Azure stapel toevoegen](azure-stack-add-new-user-aad.md)
