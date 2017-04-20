<properties
    pageTitle="Toegang tot sleutel kluis achter firewall | Microsoft Azure"
    description="Informatie over toegang tot sleutel kluis uit een toepassing die u achter een firewall"
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

# <a name="accessing-key-vault-behind-firewall"></a>Toegang tot sleutel kluis achter firewall
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-portshostsip-addresses-should-i-open-to-enable-access-to-key-vault"></a>V: Mijn sleutel kluis clienttoepassing moet zich achter een firewall, welke hosts-poorten/IP-adressen is het openen voor toegang tot de sleutel kluis?

Voor toegang tot een sleutel kluis moet de sleutel kluis clienttoepassing toegang kunnen krijgen tot meerdere parameters voor verschillende functies.

- Verificatie (via Azure Active Directory)
- Beheer van de sleutel-kluis (waaronder maken lezen/bijwerken of verwijderen en ook instelling-beleid) via Azure Resource Manager
- Openen en beheren van objecten (toetsen en geheimen) opgeslagen in de sleutel kluis zelf, doorloopt de sleutel kluis specifiek eindpunt (bv. [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

Er zijn bepaalde variaties afhankelijk van uw configuratie en omgeving.   

## <a name="ports"></a>Poorten

Al het verkeer naar de sleutel kluis voor alle 3 functies (verificatie, management en vlak toegang) gaat via HTTPS: poort 443. Echter voor de CRL, zal er soms HTTP (poort 80)-verkeer. Clients die ondersteuning bieden voor OCSP CRL niet moeten bereiken, maar af en toe [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl)kunnen bereiken.  

## <a name="authentication"></a>Verificatie

Sleutel kluis clienttoepassing moet toegang hebben tot Active Directory Azure eindpunten voor verificatie. Het eindpunt gebruikt is afhankelijk van de configuratie van de huurder AAD en het type van de hoofdsom--UPN, service principal en het type account, bijvoorbeeld Microsoft-Account of organigram-ID.  

| Type principal | Poort van eindpunt: |
|----------------|---------------|
| Gebruiker met behulp van Microsoft-Account<br> (b.v.user@hotmail.com) | **Globale:**<br> Login.microsoftonline.com:443<br><br> **Azure China:**<br> Login.chinacloudapi.CN:443<br><br>**Azure Amerikaanse overheid:**<br> inloggen us.microsoftonline.com:443<br><br>**Azure Duitsland:**<br> Login.microsoftonline.de:443<br><br> en <br>Login.live.com:443   |
| User-Service principal met organisatie-ID AAD (b.v.user@contoso.com) | **Globale:**<br> Login.microsoftonline.com:443<br><br> **Azure China:**<br> Login.chinacloudapi.CN:443<br><br>**Azure Amerikaanse overheid:**<br> inloggen us.microsoftonline.com:443<br><br>**Azure Duitsland:**<br> Login.microsoftonline.de:443 |
| User-Service principal met organisatie -ID + ADFS of andere federatieve eindpunt (b.v.user@contoso.com) | De bovenstaande eindpunten voor organisatie-ID plus ADFS of andere federatieve eindpunten |

Er zijn andere mogelijke complexe scenario's. Raadpleeg [Azure Active Directory verificatie vloeien](/documentation/articles/active-directory-authentication-scenarios/), [Integratie van toepassingen met Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) en [Active Directory-verificatieprotocollen](https://msdn.microsoft.com/library/azure/dn151124.aspx) voor meer informatie.  

## <a name="key-vault-management"></a>Sleutel kluis Management

Kluis Sleutelbeheer (CRUD en -beleid in te stellen), moet de clienttoepassing sleutel kluis eindpunt Azure Resource Manager openen.  

| Soort bewerking | Poort van eindpunt: |
|----------------|---------------|
| Sleutel kluis besturingsvlak bewerkingen<br> via Azure Resource Manager | **Globale:**<br> Management.Azure.com:443<br><br> **Azure China:**<br> Management.chinacloudapi.CN:443<br><br> **Azure Amerikaanse overheid:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Duitsland:**<br> Management.microsoftazure.de:443 |
| Azure Active Directory Graph API | **Globale:**<br> Graph.Windows.NET:443<br><br> **Azure China:**<br> Graph.chinacloudapi.CN:443<br><br> **Azure Amerikaanse overheid:**<br> Graph.Windows.NET:443<br><br> **Azure Duitsland:**<br> Graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Sleutel kluis bewerkingen

Voor alle belangrijke vault-object (toetsen en geheimen) management en cryptografische bewerkingen sleutel kluis client toegang nodig heeft tot het eindpunt van de sleutel kluis. Het DNS-achtervoegsel van eindpunt verschilt afhankelijk van de locatie van de sleutel kluis. Het eindpunt van de sleutel kluis is van de opmaak: < naam kluis >. < regio-specifieke-dns-achtervoegsel > zoals beschreven in de onderstaande tabel.  

| Soort bewerking | Poort van eindpunt: |
|----------------|---------------|
| Sleutel kluis bewerkingen, zoals cryptografische bewerkingen op toetsen, sleutels gemaakt lezen/bijwerken of verwijderen en geheimen, instellen of opvragen codes en andere kenmerken voor sleutel kluis objecten (toetsen/geheimen)     | **Globale:**<br> &lt;Vault-naam&gt;. vault.azure.net:443<br><br> **Azure China:**<br> &lt;Vault-naam&gt;. vault.azure.cn:443<br><br> **Azure Amerikaanse overheid:**<br> &lt;Vault-naam&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> &lt;Vault-naam&gt;. vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP-adresbereiken

Sleutel kluis service weer gebruikmaakt van een andere Azure bronnen zoals PaaS-infrastructuur, dus het is niet mogelijk om een specifiek IP-adressen die sleutel kluis eindpunten op een bepaald moment. Maar als uw firewall biedt alleen ondersteuning voor IP-adres bereiken vervolgens Zie het document [Microsoft Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653) .   Voor de verificatie en de identiteit (Azure Active Directory) moet uw toepassing verbinding maken met de eindpunten die worden beschreven in de [verificatie- en adressen van de identiteit](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="next-steps"></a>Volgende stappen

- Als u vragen over sleutel kluis hebt, gaat u naar de [Azure sleutel kluis Forums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
