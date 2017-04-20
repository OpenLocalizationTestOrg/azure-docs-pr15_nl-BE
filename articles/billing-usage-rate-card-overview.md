<properties
   pageTitle="Inzicht in uw verbruik van Microsoft Azure krijgen | Microsoft Azure"
   description="Biedt een overzicht van het gebruik van Azure facturering en de RateCard-APIs, die worden gebruikt om inzicht te krijgen in het verbruik van Azure en trends."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="gain-insights-into-your-microsoft-azure-resource-consumption"></a>Inzicht in uw verbruik van Microsoft Azure krijgen

Moeten de mogelijkheid om te voorspellen en beheren van hun Azure kosten nauwkeurig klanten en partners.  Terwijl ze van een Capex naar een Opex model, moeten ze ook de mogelijkheid om te doen showback vs. terugboeking analyse als modus geluidsgetrouwheid in schatting en facturering, vooral voor grote cloud implementaties te bieden.

De koers kaart API's en Azure Resourcegebruik die worden besproken in dit artikel adres moet doordat nieuwe inzichten in het verbruik van resources Azure.  

## <a name="introducing-the-azure-resource-usage-and-ratecard-apis"></a>Introductie van de RateCard API's en Azure Resourcegebruik

Het Resourcegebruik Azure en RateCard APIs worden geïmplementeerd als een Resource Provider, als onderdeel van de familie van API's beschikbaar gesteld door de Azure Resource Manager.  

### <a name="azure-resource-usage-api-preview"></a>Azure Resourcegebruik API (voorbeeld)
Klanten en partners kunnen de Azure Resource gebruik API gebruiken om hun geschatte Azure verbruiksgegevens. De functies omvatten:

- **Azure op rollen gebaseerde toegangscontrole** - klanten en partners kunnen hun-beleid configureren op de [Azure portal](https://portal.azure.com) of via [Azure PowerShell-cmdlets](powershell-install-configure.md) kunt u opgeven welke gebruikers of toepassingen toegang tot gegevens over het gebruik van het abonnement krijgen kunnen. Bellers moeten standaard Azure Active Directory tokens gebruiken voor verificatie. De aanvrager moet ook worden toegevoegd aan de lezer, eigenaar of Inzender rol voor toegang tot de gebruiksgegevens voor een bepaalde Azure abonnement.

- **Elk uur of dagelijks aggregaties** - Bellers kunnen aangeven of ze hun Azure gebruiksgegevens in buckets per uur of dagelijks buckets. De standaardwaarde is dagelijks.

- **Exemplaar metagegevens (inclusief bron codes)** – instantieniveau details, zoals de volledige bron-uri (/subscriptions/ {abonnement-id} /..), samen met de resource groep informatie- en codes worden geleverd in het antwoord. Dit zal helpen klanten jokertekenelement en gebruik programmatisch toewijzen met de tags, use-cases zoals cross opladen.

- In het antwoord, zodat de beller een beter begrip van wat werd verbruikt worden ook **metagegevens resource** - Resourcedetails, zoals de naam meter, categorie meter, meter subcategorie, eenheid en regio doorgegeven. Wij werken ook als resource metadata terminologie in het portal voor Azure uitlijnen Azure gebruik CSV, EA facturering CSV- en andere publieke ervaringen, kunnen klanten het relateren van gegevens over de ervaringen.

- **Gebruik voor alle typen van het aanbod** -gegevens zijn toegankelijk voor alle typen Pay-as-you-go, MSDN, Monetary commitment, krediet voor monetaire en EA waaronder bieden.

### <a name="azure-resource-ratecard-api-preview"></a>Azure Resource RateCard API (voorbeeld)
Klanten en partners kunnen de Azure Resource RateCard API gebruiken om de lijst met beschikbare bronnen Azure samen met geschatte prijsinformatie voor elk. De functies omvatten:

- **Azure op rollen gebaseerde toegangscontrole** - klanten en partners kunnen hun-beleid configureren op de [Azure portal](https://portal.azure.com) of via [Azure PowerShell-cmdlets](powershell-install-configure.md) kunt u opgeven welke gebruikers of toepassingen toegang tot de gegevens RateCard krijgen kunnen. Bellers moeten standaard Azure Active Directory tokens gebruiken voor verificatie. De aanvrager moet ook worden toegevoegd aan de lezer, eigenaar of Inzender rol voor toegang tot de gebruiksgegevens voor een bepaalde Azure abonnement.

- **Ondersteuning voor Pay-as-you-go, MSDN, Monetary commitment en monetaire krediet biedt (EA niet ondersteund)** - deze API biedt informatie Azure aanbieding niveau snelheid versus abonnement.  De aanroeper van deze API moet doorgeven in de aanbieding voor Resourcedetails en tarieven.  Zoals EA biedt aangepaste tarieven per inschrijving hebt, zijn we op dit moment niet de EA-tarieven.

## <a name="scenarios"></a>Scenario 's

Hier volgen enkele scenario's die mogelijk met de combinatie van het gebruik en de APIs RateCard worden gemaakt:

- **Azure besteden tijdens de maand** - klanten kunnen gebruiken om het gebruik en RateCard APIs in combinatie om beter inzicht te krijgen in hun cloud besteden tijdens de maand, door een analyse van de buckets uur- en schattingen gebruik en kosten.

- **Waarschuwingen instellen** – klanten en partners kunnen bron- of monetaire waarschuwingen instellen op de consumptie van cloud met het ophalen van het geschatte verbruik en kosten raming met behulp van het gebruik en de API RateCard.

- **Predict bill** – klanten en partners hun geschatte verbruik kunt opvragen en cloud besteden en machine learning algoritmen om te voorspellen wat hun factuur aan het einde van de factureringscyclus zou zijn van toepassing.

- **Kostenanalyse vóór consumptie** – klanten kan ook de API RateCard te voorspellen hoeveel hun factuur zijn zou als ze naar hun werkbelasting Azure, op voorwaarde dat zijn gebruik gebruiksstatistieken gewenst. Als klanten hebben een bestaande werklast in andere wolken of particuliere wolken, ze ook hun gebruik met de Azure kunnen toewijzen tarieven voor een betere schatting van de geraamde Azure besteden. Dit biedt een betere weergave van wat kan worden verkregen via de [Azure prijzen Rekenmachine](https://azure.microsoft.com/pricing/calculator/)(bijvoorbeeld) de mogelijkheid om te draaien op het voorstel en vergelijken/contrast tussen verschillende aanbiedingen buiten Pay-As-You-Go, met inbegrip van monetaire verbintenis en monetaire krediet, onze partners facturering zijn beschikbaar. De API's ook de mogelijkheid bieden naar schatting wijzigingen per regio, kost het inschakelen van het type wat-als-analyse vereist om beslissingen te implementeren, zoals implementeren bronnen in andere DC's over de hele wereld een directe invloed op de totale kosten hebben kunnen.

- **Wat-als-analyse** -

    - Klanten en partners kunnen bepalen of het hun werkbelasting uitvoeren in een ander gebied of op een andere configuratie van de resource Azure rendabeler zou zijn. Azure resource kosten kunnen verschillen op basis van de Azure regio waarin ze worden uitgevoerd en hierdoor kunnen klanten en partners voor optimalisatie van de kosten.

    - Klanten en partners kunnen ook als een ander type van Azure bieden een betere snelheid op een Azure bron geeft bepalen.

## <a name="partner-solutions"></a>Oplossingen van partners

Beschrijving van de integratie met aangeboden door partner Azure facturering API [Cloudyn](https://www.cloudyn.com/microsoft-azure/) [Microsoft Azure gebruiks- en RateCard API's Cloudyn inschakelen om ITFM voor klanten bieden](billing-usage-rate-card-partner-solution-cloudyn.md) .  Dit artikel bevat gedetailleerde dekking van hun ervaringen, met inbegrip van een korte video die laat zien hoe een Azure klant kan met Cloudyn en de Azure facturering-API's inzichten winsten uit hun verbruiksgegevens Azure.

[Wolk Cruiser en facturering API integratie met Microsoft Azure](billing-usage-rate-card-partner-solution-cloudcruiser.md) wordt beschreven hoe [de wolk Cruiser Express voor Azure Pack](http://www.cloudcruiser.com/partners/microsoft/) werkt direct vanaf de portal WAP klanten naadloos operationele en financiële aspecten van hun Microsoft Azure private of gehoste openbare cloud beheren vanuit één enkele interface inschakelen.   

## <a name="next-steps"></a>Volgende stappen
+ Bekijk de [Azure facturering REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) voor meer informatie over beide API's die deel van de verzameling API's die door de Azure Resource Manager uitmaken.
+ Bekijk onze Microsoft Azure facturering API-codevoorbeelden op [Azure codevoorbeelden](https://azure.microsoft.com/documentation/samples/?term=billing)in als u wilt direct Duik in de voorbeeldcode.

## <a name="learn-more"></a>Meer informatie
+ Zie het artikel [Azure Resource Manager-overzicht](azure-resource-manager/resource-group-overview.md) voor meer informatie over de Azure Resource Manager.
+ Besteden, Gartner artikel [Markt handleiding voor extra IT financieel Management (ITFM)](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb)voor meer informatie over de suite met hulpprogramma's die nodig zijn om te helpen bij het verkrijgen van inzicht in de cloud.
