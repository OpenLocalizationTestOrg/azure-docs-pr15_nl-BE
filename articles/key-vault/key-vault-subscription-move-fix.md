<properties
    pageTitle="De sleutel kluis huurder-ID wijzigen nadat een abonnement | Microsoft Azure"
    description="Meer informatie over het overschakelen van de huurder-ID voor een sleutel kluis nadat een abonnement is verplaatst naar een andere huurder"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Een sleutel kluis huurder-ID wijzigen nadat een abonnement verplaatsen
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>V: Mijn abonnement is van huurder A verplaatst naar de huurder B. Hoe ik de huurder-ID wijzigen voor mijn bestaande sleutel kluis en stel de juiste ACL's voor beveiligings-principals in huurder B?

Wanneer u een nieuwe sleutel kluis in een abonnement maakt, is het automatisch gebonden aan de standaard Azure Active Directory huurder-ID voor dit abonnement. Posten van alle access-beleid zijn ook verbonden met deze huurder. Wanneer u naar uw Azure abonnement van de huurder een huurder B, zijn de bestaande sleutel kluizen niet toegankelijk door de beveiligings-principals (gebruikers en toepassingen) in een huurder B. U kunt dit probleem oplossen, moet u:

- De huurder-ID wijzigen die zijn gekoppeld aan alle bestaande sleutels kluizen in dit abonnement aan huurder B.
- Verwijder alle bestaande access policy posten.
- Voeg nieuwe items van access-beleid die gekoppeld aan de huurder B. zijn

Bijvoorbeeld als u een abonnement dat is verplaatst van sleutel kluis 'myvault' pachters A naar B, hier van huurder de huurder-ID voor deze sleutel kluis wijzigen en verwijderen van oude-beleid.

<pre>
$vaultResourceId = (get-AzureRmKeyVault - VaultName-myvault). ResourceId $vault = Get-AzureRmResource – ResourceId $vaultResourceId - ExpandProperties $vault. Properties.TenantId = (Get-AzureRmContext). Tenant.TenantId $vault. Properties.AccessPolicies = @() $vaultResourceId Set AzureRmResource - ResourceId-$vault eigenschappen. Eigenschappen
</pre>

Omdat deze kluis in een huurder vóór de verplaatsing, de oorspronkelijke waarde van **$vault is. Properties.TenantId** is een huurder, terwijl **(Get-AzureRmContext). Tenant.TenantId** is pachters B.

Uw kluis is gekoppeld aan de juiste huurder-ID en de oude access policy vermeldingen worden verwijderd, wordt nieuwe toegang beleid posten met [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx)instellen.

## <a name="next-steps"></a>Volgende stappen

Als u vragen over Azure sleutel kluis hebt, gaat u naar de [Azure sleutel kluis Forums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
