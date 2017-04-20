<properties
    pageTitle="Wat is er nieuw in Azure stapel | Microsoft Azure"
    description="Wat is er nieuw in Azure stapel"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="whats-new-in-azure-stack-technical-preview-2"></a>Wat is nieuw in Azure Stack technische Preview 2
Deze release bevat nieuwe voorzieningen voor zowel huurders en beheerders.

## <a name="network"></a>Netwerk   
   - [IDN's](azure-stack-understanding-dns-in-tp2.md) biedt interne Netwerkregistratie en omzetting van de Domain Name System (DNS) zonder extra DNS-infrastructuur.
   - [Virtueel netwerkgateways](azure-stack-create-vpn-connection-one-node-tp2.md) bieden VPN-connectiviteitsopties bronnen Azure of op locatie.
   - Gebruiker gedefinieerde Routes kan routeren van netwerkverkeer via firewalls, beveiliging, andere toestellen en andere diensten.
   - Uit de Marketplace kunt u netwerkbronnen.   

## <a name="storage"></a>Opslag
 - [Azure wachtrijen](https://msdn.microsoft.com/library/dd179353.aspx) kunnen betrouwbaar en permanente service messaging.
 - [Analytics opslag](https://msdn.microsoft.com/library/azure/hh343270.aspx) opslag prestaties opnamegegevens. U kunt deze gegevens aanvragen traceren, gebruik trends analyseren en vaststellen van problemen met uw account voor opslag.
 - BLOB-opslag ondersteunt [toevoegbewerking blokkeren](https://msdn.microsoft.com/library/azure/mt427365.aspx).
 - Ondersteuning voor Premium opslag API-account.
 - Pachters storage service-ondersteuning voor algemene hulpprogramma's en SDK's, zoals de CLI Azure, PowerShell .NET, Python en Java-SDK. 
 - Gedetailleerde delegeren van toegang tot uw opslag inschakelen [Opslag Account gedeelde toegang handtekening](https://msdn.microsoft.com/library/azure/mt584140.aspx) zonder dat u uw volledige account sleutel delen.  
 - [Managed Service groepsaccounts](https://technet.microsoft.com/library/hh831477.aspx) Storage-services nu gebruiken voor een sterke beveiliging met lage beheerkosten.
 - U kunt vrij huurder van niet-gebruikte middelen op afroep.
 - Opslag van verwijderde account inhouding lengte kan worden geconfigureerd.
 - U kunt verwijderde huurder opslag rekeningen herstellen.

## <a name="compute"></a>Berekenen
- U kunt virtuele machines opheffen.
- U kunt virtuele machine-extensies voor het oplossen van problemen en configuratie management implementeren.
- U kunt het formaat van de schijven van de virtuele machine.
- Virtuele machines kunnen meerdere netwerkinterfaces hebben.

### <a name="portal-experience"></a>Portal-ervaring
 - Azure Stack gebieden zijn een logische eenheid van schaal en beheer in Azure Stack. In dit voorbeeld, kunt u informatie weergeven over services zoals compute, netwerk en opslag per regio.
 - U kunt de interface (updates) [azure stack updates.md] bekijken.

## <a name="key-vault"></a>Sleutel kluis
- [Sleutel kluis in Azure Stack](azure-stack-kv-intro.md) biedt veilig beheer van uw sleutels en wachtwoorden voor cloud-toepassingen.
- U kunt controleren en bewaken van sleutelgebruik door apps en VMs.

## <a name="billing-and-usage"></a>Facturering en het gebruik van
 - [Facturering en het verbruik van API's](azure-stack-billing-and-chargeback.md) bieden gegevens over hoe u uw diensten worden verbruikt.  
 - Gedelegeerde Providers kan wederverkopers in uw stapel Azure services aanbieden aan hun klanten.

## <a name="monitoring-and-health"></a>En de gezondheid
 - Nieuwe functies beschikbaar in de portal en API's kunt u proactief weergeven en beheren van waarschuwingen op uw omgeving.  

## <a name="next-steps"></a>Volgende stappen
- [Azure-Stack Implementatiemodel architectuur te begrijpen](azure-stack-architecture.md)      
- [Vereisten voor implementatie begrijpen](azure-stack-deploy.md)
- [Azure Stack implementeren](azure-stack-run-powershell-script.md)

  
