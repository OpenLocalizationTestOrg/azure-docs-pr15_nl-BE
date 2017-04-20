<properties
    pageTitle="Een access-rapport wijzigen geschiedenis maken | Microsoft Azure"
    description="Genereer een rapport met alle wijzigingen in de toegang tot uw Azure abonnementen met toegangsbeheer op basis van rollen de afgelopen 90 dagen."
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

# <a name="create-an-access-change-history-report"></a>Een access-rapport wijzigen geschiedenis maken

Iemand verleent of toegang in uw abonnementen, trekt u ophalen de wijzigingen in Azure-gebeurtenissen vastgelegd. U kunt access rapporten met overzichten van wijzigen voor een overzicht van alle wijzigingen voor de afgelopen 90 dagen.

## <a name="create-a-report-with-azure-powershell"></a>Een rapport maken met Azure PowerShell
U kunt een access-rapport wijzigen geschiedenis in PowerShell maken de `Get-AzureRMAuthorizationChangeLog` opdracht. Meer informatie over deze cmdlet zijn beschikbaar in de [Galerie met PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/1.0.6/Content/ResourceManagerStartup.ps1).

Wanneer u deze opdracht aanroept, kunt u opgeven welke eigenschap van de toewijzingen die u weergegeven, waaronder de volgende wilt:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| **Actie** | Toegang is verleend of ingetrokken |
| **Beller** | De eigenaar die verantwoordelijk is voor de toegang wijzigen |
| **Datum** | De datum en tijd dat access is gewijzigd. |
| **DirectoryName** | De map Azure Active Directory |
| **PrincipalName** | De naam van de gebruiker, groep of toepassing |
| **PrincipalType** | Of de toewijzing is voor een gebruiker, groep of toepassing |
| **RoleId** | De GUID van de rol die is verleend of ingetrokken |
| **Rolnaam** | De rol die is verleend of ingetrokken |
| **Scopenaam** | De naam van het abonnement, resource of resourcegroep |
| **ScopeType** | Of de toewijzing is bij het abonnement, resourcegroep of resource scope |
| **SubscriptionId** | De GUID van het abonnement Azure |
| **SubscriptionName** | De naam van het abonnement Azure |

Deze opdracht worden alle access-wijzigingen in het abonnement voor de afgelopen zeven dagen:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog - screenshot](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Een rapport maken met Azure CLI
U kunt een access-rapport wijzigen geschiedenis maken in Azure opdrachtregelinterface (CLI) de `azure role assignment changelog list` opdracht.

## <a name="export-to-a-spreadsheet"></a>Exporteren naar een werkblad
Sla het rapport, of de gegevens bewerken, access wijzigingen in een CSV-bestand exporteren. U kunt het rapport bekijken in een werkblad voor revisie.

![Changelog weergegeven als werkblad - screenshot](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="see-also"></a>Zie ook
- Aan de slag met [Toegangsbeheer op Azure Role-Based](role-based-access-control-configure.md)
- Werken met [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md)
