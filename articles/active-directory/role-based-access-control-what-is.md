<properties
    pageTitle="Role-Based Access Control | Microsoft Azure"
    description="Beheer van toegang aan de slag met Azure op rollen gebaseerde toegangscontrole, in de Portal Azure. Roltoewijzingen gebruiken voor het toewijzen van machtigingen voor de map."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="get-started-with-access-management-in-the-azure-portal"></a>Aan de slag met access management in Azure portal

Beveiliging-georiënteerde bedrijven moeten zich concentreren op uw werknemers de exacte machtigingen die ze nodig hebben. Te veel machtigingen beschrijft een account voor inbreuken door onbevoegden. Te weinig machtigingen houdt in dat werknemers hun werk efficiënt kunnen niet ophalen. Azure Role-Based Access Control RBAC () kunt u dit probleem oplossen door aan te bieden, toegesneden toegangsbeheer voor Azure.

RBAC gebruikt, kunt u taken binnen uw team te scheiden en de hoeveelheid toegang verlenen aan gebruikers die ze nodig hebben om hun taken uitvoeren. Onbeperkte machtigingen in plaats van iedereen in uw abonnement Azure of resources, kunt u alleen bepaalde acties. Bijvoorbeeld: gebruik RBAC als één werknemer beheren van virtuele machines in een abonnement, terwijl een andere SQL-databases in het hetzelfde abonnement kunt beheren.

## <a name="basics-of-access-management-in-azure"></a>Basisbeginselen van het toegangsbeheer in Azure
Elk Azure abonnement is gekoppeld aan een bepaalde map Azure Active Directory (AD). Gebruikers, groepen en toepassingen van die directory kunnen bronnen in de Azure abonnement beheren. Deze met Azure portal, Azure opdrachtregelprogramma's en API's voor Azure toegangsrechten toewijzen.

Toegang verlenen door de juiste RBAC-rol toewijzen aan gebruikers, groepen en toepassingen op een bepaalde scope. Het bereik van een roltoewijzing is een abonnement, een resourcegroep of een enkele bron. Een rol die wordt toegewezen aan een bovenliggende bereik ook verleent toegang aan de aanwezige kinderen. Een gebruiker met toegang tot een groep kan bijvoorbeeld alle bronnen, zoals websites, virtuele machines en subnetten bevat beheren.

![Relatie tussen elementen Azure Active Directory - diagram](./media/role-based-access-control-what-is/rbac_aad.png)

De RBAC-rol die u toewijst, bepaalt welke bronnen de gebruiker, groep of een toepassing binnen dat bereik kunt beheren.

## <a name="built-in-roles"></a>Ingebouwde functies
Azure RBAC heeft drie elementaire functies die betrekking hebben op alle typen:

- **De eigenaar** heeft volledige toegang tot alle bronnen, met inbegrip van het recht op toegang aan anderen overdragen.
- **Inzender** kunt maken en beheren van alle soorten Azure bronnen maar kan geen toegang tot de andere.
- **Lezer** kan bestaande Azure resources kunt bekijken.

De rest van de RBAC-rollen in Azure beheer van specifieke bronnen van Azure toestaan. De rol van inzender voor virtuele Machine kunt bijvoorbeeld de gebruiker maken en beheren van virtuele machines. Het geeft ze toegang het virtuele netwerk of het subnet dat verbinding met de virtuele machine maakt.

[Ingebouwde RBAC-rollen](role-based-access-built-in-roles.md) worden de rollen in Azure beschikbaar. Deze geeft de bewerkingen en de omvang waarin elke ingebouwde rol wordt toegekend aan gebruikers. Zie hoe u [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md)als u op zoek bent om uw eigen rollen voor nog meer controle te definiëren.

## <a name="resource-hierarchy-and-access-inheritance"></a>Resource-hiërarchie en toegang tot overname
- Elk **abonnement** in Azure behoort tot slechts één map.
- Elke **groep** hoort slechts één abonnement.
- Elke **resource** deel uitmaakt van de groep slechts één resource.

Toegang die u bij het bereiken van de bovenliggende verleent overgenomen op child-scopes. Bijvoorbeeld:

- U kunt de rol van lezer toewijst aan een Azure AD binnen het bereik van het abonnement. De leden van de groep kunnen elke groep en bron weergeven in het abonnement.
- U toewijzen de rol van inzender voor een toepassing op het groepsbereik resource. Deze kunt bronnen van alle soorten van die groep, maar geen andere resourcegroepen in het abonnement beheren.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC versus klassiek abonnement beheerders
Klassieke abonnement beheerders en co-beheerders hebben volledige toegang tot de Azure abonnement. Ze kunnen via de [portal Azure](https://portal.azure.com) met Azure Resource Manager API's of het [Azure klassieke portal](https://manage.windowsazure.com) en Azure klassieke implementatiemodel bronnen beheren. In het model RBAC klassieke beheerders de rol van eigenaar op het abonnement bereik toegewezen.

Alleen de Azure portal en de nieuwe API's van Azure Resource Manager ondersteunt RBAC Azure. Gebruikers en toepassingen die RBAC-rollen zijn toegewezen niet de klassieke management portal en het model Klassiek Azure-implementatie te gebruiken.

## <a name="authorization-for-management-vs-data-operations"></a>Vergunning voor het beheer van versus gegevensbewerkingen
Azure RBAC ondersteunt alleen beheertaken uit te voeren van de Azure resources in de portal Azure en Azure Resource Manager API's. Het kan niet alle gegevens op bewerkingen voor Azure middelen toestaan. Zo kun je iemand opslag beheren, maar niet aan de BLOB's of tabellen in een opslag-Account niet. Ook kan een SQL-database worden beheerd, maar niet aan de tabellen in het.

## <a name="next-steps"></a>Volgende stappen
- Aan de slag met [toegangsbeheer op basis van rollen in de portal Azure](role-based-access-control-configure.md).
- Zie de [ingebouwde RBAC-rollen](role-based-access-built-in-roles.md)
- Uw eigen [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md) definiëren
