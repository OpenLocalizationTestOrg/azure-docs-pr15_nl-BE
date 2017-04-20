
<properties 
    pageTitle="Azure AD + Active Directory-vereisten voor RemoteApp-Azure | Microsoft Azure" 
    description="Informatie over het instellen van Active Directory voor het werken met Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + voor RemoteApp-Azure Active Directory-vereisten

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.


Uw hybride Azure RemoteApp-verzameling of voor een cloud-collectie die u wilt communiceren met AD verbinden, moet u het volgende doen.

### <a name="connect-azure-ad-and-active-directory"></a>Verbinding maken met Azure AD en Active Directory

Als u verbinding maken met uw huurder Azure AD en uw Active Directory-omgevingen op gebouwen wilt, gebruiken AD verbinden. U alleen [4 muisklikken](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) verbinding maken met de twee mappen die wordt uitgevoerd.

Opmerking: adreslijstsynchronisatie is vereist voor hybride collecties.

### <a name="make-sure-your-domaincom-match"></a>Zorg ervoor dat uw "@domain.com" vergelijken
Voordat u begint, controleert u of het achtervoegsel van het domein Azure AD komt overeen met de UPN voor uw forest op gebouwen. 

Nadat u het domein UPN-achtervoegsel in Azure AD ingesteld, alle gebruikers inloggen bij Azure RemoteApp wordt aanmelden als “user@ <the suffix you set up>". Zorg ervoor dat gebruikers zich ook met dezelfde aanmelden kunnen user@suffix in het domein op gebouwen. In bepaalde gevallen kunt u de naam van een domein instellen in Azure AD tijdens het opgeven van een ander domeinachtervoegsel voor de gebruiker op prem. In dit geval uw gebruikers niet mogelijk verbinding maken met een domein behoren computers of bronnen via RemoteApp Azure.

Als u uw domein UPN-achtervoegsel in AAD als contoso.com hebt ingesteld, maar sommige gebruikers van gebouwen/AD zijn zodanig geconfigureerd dat met bijvoorbeeld @contoso.uk, en vervolgens de gebruikers worden niet correct zich aanmelden bij de ARA-collectie. UPN in AAD en AD-gebruikers moeten hetzelfde zijn voor de aanmelding mogelijk"

### <a name="create-objects-for-azure-remoteapp"></a>Objecten maken voor Azure RemoteApp
Moet u ook de volgende op-ruimten Active Directory-objecten te maken:

- Een service-account voor toegang tot domeinbronnen voor RemoteApp-programma's door te RDSH eindpunten koppelen aan het domein op gebouwen.
- Een organisatie-eenheid (OU) RemoteApp-machine-objecten bevatten. Gebruik van de organisatie-eenheid is het raadzaam (maar niet vereist) voor het isoleren van de rekeningen en het beleid dat u met RemoteApp gebruiken wilt.

Moet u deze beide objecten bij het maken van RemoteApp-collectie worden eerst deze stappen doen.