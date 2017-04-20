<properties
    pageTitle="Analytische functies voor serviceproviders melden | Microsoft Azure"
    description="Logboek Analytics kunnen Managed Service Providers (MSPs), grote ondernemingen, software op onafhankelijke softwareleveranciers (ISV's) en hosting serviceproviders beheren en controleren van servers in de cloud infrastructuur of in gebouwen van de klant."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="richrund"/>

# <a name="log-analytics-features-for-service-providers"></a>Logboek analytische functies voor serviceproviders

Logboek Analytics kan helpen beheerde serviceproviders (MSPs), grote ondernemingen, onafhankelijke softwareleveranciers (ISV's) en hostingproviders beheren en controleren van servers in de cloud infrastructuur of in gebouwen van de klant. 

Grote ondernemingen delen veel overeenkomsten met dienstverleners, met name wanneer er een gecentraliseerde IT-team is verantwoordelijk voor het beheer van IT voor veel verschillende business units. Dit document wordt gebruikt de term *serviceprovider* voor eenvoud, maar dezelfde functionaliteit is ook beschikbaar voor bedrijven en andere klanten.

## <a name="cloud-solution-provider"></a>Cloud Solution Provider

Partners en dienstverleners die deel van het programma [Cloud Solution Provider (CSP uitmaken)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) is logboek Analytics een van de Azure services die beschikbaar zijn op een abonnement CSP. 

Voor Log Analytics gelden de volgende mogelijkheden abonnementen *Cloud Solution Provider* .

U kunt een *Cloud Solution Provider* :

+ Logboek Analytics werkruimten maken in een huurder (klant) abonnement.
+ Access-werkruimten die zijn gemaakt door huurders. 
+ Toevoegen en verwijderen van gebruikers die toegang tot de werkruimte met behulp van Gebruikersbeheer Azure. Wanneer u in de werkruimte van de huurder in de portal OMS Gebruikersbeheer van de pagina onder instellingen is niet beschikbaar
  - Logboek Analytics biedt geen ondersteuning voor toegang op basis van rollen, maar -waardoor een gebruiker `reader` machtiging in Azure portal kan deze configuratie te wijzigen in de portal OMS

Voor het aanmelden bij een abonnement van een huurder, moet u de id van de huurder op te geven. De id van de huurder is vaak dat laatste deel van het e-mailadres dat is gebruikt voor aanmelding.

+ Voeg in de portal OMS `?tenant=contoso.com` in de URL voor de portal. Bijvoorbeeld:`mms.microsoft.com/?tenant=contoso.com`
+ Gebruik in PowerShell, de `-Tenant contoso.com` parameter bij het gebruik van `Add-AzureRmAccount` cmdlet
+ De id van de huurder wordt automatisch toegevoegd wanneer u de `OMS portal` koppeling uit de Azure portal te openen en zich aanmelden bij de OMS-portal voor de geselecteerde werkruimte

U kunt als *klant* van een Cloud Solution Provider:

+ Logboek van een abonnement op CSP analytics werkruimten maken
+ Access-werkruimten die zijn gemaakt door de CSP
  -  Gebruik de `OMS portal` koppeling uit de Azure portal te openen en zich aanmelden bij de OMS-portal voor de geselecteerde werkruimte
+ Weergeven en gebruiken van de beheerpagina onder instellingen in de portal OMS

>[AZURE.NOTE] De back-up en herstel van de Site-oplossingen voor logboek Analytics kunnen geen verbinding maken met een kluis Recovery Services en kunnen niet worden geconfigureerd in een CSP-abonnement.

## <a name="managing-multiple-customers-using-log-analytics"></a>Beheer van meerdere klanten door middel van analyses van logboek 

Het verdient aanbeveling dat u een logboek Analytics werkruimte maken voor elke klant die u wilt beheren. Een logboek Analytics werkruimte biedt:

+ Een geografische locatie kunnen worden opgeslagen. 
+ Granulatie voor facturering 
+ Gegevensisolatie 
+ Unieke configuratie

U maakt een werkruimte per klant, bent u gescheiden houden van de gegevens van elke klant en het gebruik van elke klant ook bijhouden.

Meer informatie over wanneer en waarom meerdere werkruimten maken, zoals beschreven in [beheren toegang tot analytics melden] (log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Maken en configureren van een klant werkruimten kunnen worden geautomatiseerd met behulp van [PowerShell](log-analytics-powershell-workspace-configuration.md), [Resource Manager-sjablonen](log-analytics-template-workspace-configuration.md), of met de [REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).

Het gebruik van Resource Manager-sjablonen voor de configuratie van de werkruimte kunt u een master-configuratie maken en configureren van werkruimten kunnen worden gebruikt. U kunt altijd vertrouwen als werkruimten zijn gemaakt voor klanten automatisch geconfigureerd zijn voor uw behoeften. Tijdens het bijwerken van uw vereisten voor wordt de sjabloon bijgewerkt en vervolgens de bestaande werkruimten toegepast. Dit proces zorgt ervoor dat uw nieuwe normen voldoen aan de nog bestaande werkruimten.    

Als u meerdere logboekbestanden Analytics werkruimten beheert, wordt aangeraden elke werkruimte te integreren met uw bestaande ticketing systeem / console bewerkingen met [de waarschuwingsfunctie](log-analytics-alerts.md) . Door de integratie met uw bestaande systemen, kan ondersteunend personeel Volg verder hun vertrouwde processen. Logboek Analytics regelmatig controleert elke werkruimte tegen alert door u opgegeven criteria en genereert een waarschuwing wanneer actie nodig is.

Voor uitvoerend niveau rapporten die gegevens samenvatten in werkruimten kunt u de integratie tussen Analytics logboek en [PowerBI](log-analytics-powerbi.md). Als u integreren met een ander systeem wilt, kunt u de zoek-API (via PowerShell of [REST](log-analytics-log-search-api.md)) query's uitvoeren en de zoekresultaten exporteren.

## <a name="next-steps"></a>Volgende stappen

+ Maken en configureren van werkruimten met [Resource Manager-sjablonen](log-analytics-template-workspace-configuration.md) automatiseren
+ Het maken van werkruimten met [PowerShell](log-analytics-powershell-workspace-configuration.md) te automatiseren 
+ [Waarschuwingen](log-analytics-alerts.md) gebruiken voor integratie met bestaande systemen
+ Samenvattende rapporten genereren met [PowerBI](log-analytics-powerbi.md)
