<properties 
    pageTitle="Wat voor soort collectie hebt u nodig voor Azure RemoteApp? | Microsoft Azure" 
    description="Meer informatie over de typen collecties met RemoteApp-Azure beschikbaar." 
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



# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Wat voor soort collectie hebt u nodig voor Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp kunt u toepassingen en bronnen delen met gebruikers op elk apparaat. We dit doen door het maken van collecties bevatten van de toepassingen en bronnen en u deze collecties met gebruikers deelt. Er zijn 2 verschillende collectie-opties, met verschillende netwerk- en verificatieopties - is geschikt voor u?

We doorlopen de verschillende overwegingen en de keuzes die u nodig hebt om optimaal van uw Azure RemoteApp-collectie. 


## <a name="quick-differences-between-the-collection-types"></a>Snelle verschillen tussen de collectietypen

|           | Wolk | Hybride |
|-----------|-------|--------|
|Gebruik een bestaande VNET| Ja| Ja|
|AD verbonden accounts (DirSync) is vereist| Nee| Ja|
|Vereist aan domein toevoegen| Nee| Ja|
|Domeincontroller die toegankelijk zijn voor VNET vereist| Nee| Ja|

## <a name="cloud-collections"></a>Cloud-collecties
- Snel te maken - de collectie snel is ingericht, wat betekent dat uw apps krijgen gebruikers sneller.
- Breng uw eigen apps of onze delen. U kunt een aangepaste afbeelding (ontwikkeld op basis van een Azure VM) of een van de afbeeldingen die deel uitmaken van uw abonnement.
- U hoeft niet voor het configureren van een verbinding tussen uw collectie en het domein op gebouwen.
- Maar u kunt eventueel uw eigen Azure-VNET gebruiken om toegang te bieden in uw omgeving in ruimten voor het delen van gegevens of niet-Windows-verificatie in bronnen zoals SQL Server (met behulp van de database-verificatie) gebruiken.


OK, hoe maak ik een?

- Cloud? Maken met de optie **Snel maken** in de portal.
- Wolk + VNET? Maken met behulp van de optie **maken met VNET** , maar niet wilt toevoegen aan een domein.

## <a name="hybrid-collections"></a>Hybride collecties
- Volledige toegang tot het netwerk op gebouwen + Azure VNET bieden.
- Join domeintoegang voor apps en gegevens bevat. Externe toepassingen kunnen verificatie ten opzichte van uw Active Directory op gebouwen - zij vervolgens toegang krijgen tot bronnen in het domein.
- Schakel geavanceerde controle en beheer met bestaande oplossingen van System Center en Windows-groepsbeleid (door middel van een aangepaste installatiekopie die is gebaseerd op Windows Server 2012 R2)
- [ExpressRoute](https://azure.microsoft.com/services/expressroute/) voor uw Azure-VNET verbinding met uw lokale VNET ondersteunen.

Maak met behulp van de optie **maken met VNET** en kies toevoegen aan een domein.

## <a name="authentication-options"></a>Verificatie-opties
Azure RemoteApp ondersteunt Microsoft-accounts en Azure Active Directory-accounts, maar niet alle collecties ondersteunen alle methoden. 

| Accounttype                      |                                                             | Wolk | Wolk + VNET | Hybride |
|-----------------------------------|-------------------------------------------------------------|-------|--------------|--------|
| Microsoft-Account                 |                                                             | Ja   | Ja          | Nee     |
| Azure Active Directory (AD Azure) |                                                             |       |              |        |
|                                   | Alleen Azure AD                                               | Ja   | Ja          | Nee     |
|                                   | AD verbinden met wachtwoord synchronisatie                               | Ja   | Ja          | Ja    |
|                                   | AD verbinden zonder wachtwoord synchronisatie                            | Ja   | Ja          | Nee     |
|                                   | AD verbinding met AD FS                                       | Ja   | Ja          | Ja    |
|                                   | 3de partij Azure ondersteund id-providers (zoals Ping) | Ja   | Ja          | Ja    |
| Meerledige verificatie       |                                                             | Ja   | Ja          | Ja    |



### <a name="cloud-and-cloud--vnet"></a>Cloud en Cloud + VNET 
Met cloud collecties, kunt u Microsoft-accounts, Azure AD accounts of een combinatie van beide. Gebruik de rekeningen die het meest voor uw gebruikers geschikt.

Er zijn geen specifieke vereisten voor het gebruik van Microsoft-accounts. 

Als u Azure AD-account gebruiken wilt, moet u om ervoor te zorgen dat de huurder Azure AD overeenkomt met de naam die is gekoppeld aan uw abonnement. Wanneer u uw Azure RemoteApp-abonnement hebt gemaakt, is de Azure AD huurder u automatisch gekoppeld aan uw abonnement. Gebruikers machtigen voor Azure AD moet die dezelfde huurder. Indien nodig kunt u [de huurder Azure AD wijzigen](remoteapp-changetenant.md) die is gekoppeld aan uw abonnement.
 
### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hybride (of cloud + Azure AD + AD)

Met behulp van Azure AD + Active Directory is een vereiste voor een hybride collectie op gebouwen. U moet verbinding maken met AD voor de integratie van de twee mappen gebruiken. Maar u hebt een keuze als het gaat om het configureren van AD verbinden. 

Er zijn 2 AD scenario's verbinden - met Wachtwoordsynchronisatie of AD federation. Bekijk de [informatie AD verbinden](../active-directory/active-directory-aadconnect.md) om erachter te komen welke van deze voor u het beste werkt.

U kunt ook Azure AD + AD met een cloud-collectie gebruiken. Zorg ervoor dat u volgt dezelfde stappen instellen.

Uitchecken [Azure AD + Active Directory-vereisten voor Azure RemoteApp-](remoteapp-ad.md) voor de vereiste stappen voor het configureren van AD Azure en Active Directory.

## <a name="go-create-your-collection"></a>Ga naar de collectie maken!
OK, ik denk dat hebben we nu uit meegerekend dus er is één ding om te doen - het maken van uw eerste Azure RemoteApp-collectie.

[Maken een collectie cloud](remoteapp-create-cloud-deployment.md) of [een hybride collectie maken](remoteapp-create-hybrid-deployment.md) - opvragen maken.
