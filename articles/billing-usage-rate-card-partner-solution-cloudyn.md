<properties
   pageTitle="Gebruik Microsoft Azure en RateCard API's inschakelen Cloudyn ITFM om klanten te bieden | Microsoft Azure"
   description="Biedt een uniek perspectief van Microsoft Azure facturering partner Cloudyn, op hun ervaringen de Azure facturering-API's integreren in hun product.  Dit is vooral handig voor klanten met Azure en Cloudyn die zijn geïnteresseerd in het gebruik van/het Cloudyn voor Azure Services."
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

# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Gebruik Microsoft Azure en RateCard API's kunnen Cloudyn ITFM om klanten te bieden

Cloudyn, een Microsoft-partner ontwikkeling en een toonaangevende leverancier van beheerfuncties wolk, werd gekozen voor een persoonlijke voorbeeld van de nieuwe Microsoft Azure Resourcegebruik en RateCard APIs.  De API gebruik biedt toegang tot gegevens over het verbruik van geschatte Azure voor een abonnement. De API RateCard biedt volledige prijsinformatie van alle Azure services voor niet - Enterprise overeenkomst EA klanten. Deze API's bieden een basis volledige informatie voor invoer in IT financieel Management (ITFM)-hulpprogramma's zoals die door Cloudyn samen geïntegreerd.

## <a name="introduction"></a>Inleiding

De zogenaamde 'vermenigvuldigen' van de gegevens van de API voor gebruik met gegevens van de API RateCard (gebruik [eenheden] prijs [$unit] = gedetailleerde gebruik en kosten) wordt gemaakt van de meest gedetailleerde, nauwkeurige en betrouwbare informatie over facturering beschikbaar voor Azure vandaag.

![Overzicht ITFM][1]

Deze API's die bevat belangrijke informatie over gebruik en kosten, waardoor de Cloudyn van klanten op een eenvoudige, programmatische manier analyseren en diverse ITFM taken uitvoeren voor de klanten van uw klanten.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>Integratie van Cloudyn met de RateCard en het gebruik van API 's
De API RateCard verschillende invoerparameters--net als regio-info, valuta en locale--vereist maar het belangrijkste is OfferDurableID, waarmee het soort Azure biedt de klant wordt gebruikt (Pay-as-you-Go, oude verbintenis voor 6 en 12 maanden plannen, MSDN aanbiedingen, MPN aanbiedingen, aanbiedingen en andere). De OfferDurableID vindt u in de [Azure gebruik en facturering portal](https://account.windowsazure.com/Subscriptions)onder het "voorstel-ID" voor het opgegeven abonnement.

Na inschrijving voor [Cloudyn voor Azure](https://www.cloudyn.com/microsoft-azure/) services, kunnen klanten hun code OfferDurableID die Cloudyn om op te halen via de API RateCard relevante prijsgegevens kunt toevoegen.  Informatie over de verschillende soorten aanbiedingen vindt u een de pagina [Microsoft Azure bieden Details](https://azure.microsoft.com/support/legal/offer-details/) .

![Cloudyn ITFM-Engine-overzicht][2]

Cloudyn gebruikt de gebruik en RateCard APIs, naast de Performance Azure-API voor het maken van extra lagen van visualisatie, analytics, waarschuwing, rapportage, kostenbeheer en uitvoerbare aanbevelingen die Azure klanten bieden een betrouwbare enterprise cloud ITFM gereedschap.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Cloudyn ITFM use-cases ingeschakeld door het gebruik en de RateCard API-integratie
Algemene Cloudyn ITFM gebruiksvoorbeelden ingeschakeld door het gebruik en RateCard APIs omvatten:

+ **Kostenanalyse** - kunnen kosten worden uitgesplitst op een native identificerende dimensie (provider, service, account, regio enz.) van de wolk. Het gebruik van Azure en RateCard APIs hier door middel van de meest gedetailleerde uitsplitsing van het gebruik van een eenvoudige taak maken en kostengegevens per rekening, die vervolgens gegroepeerd en gefilterd op Cloudyn en voor de gebruiker, in de vorm van een grafisch of in tabelvorm weergegeven.

![Kosten analyse cirkeldiagram][3]

+ **Kosten verdeling 360** - Hiermee Financiën en IT-managers om de verdeling van de werkelijke kosten, stuurprogramma's en de ontwikkeling van hun cloud-implementatie. Verder kunnen beheerders eenvoudig implementatie kosten koppelen aan business units, departementen, regio's en meer, biedt ongekende inzichten in de kosten van de wolk en de onderneming terugboekingen en showbacks. Het gebruik van Azure en RateCard APIs dienen als input voor de Cloudyn kosten toewijzing engine, die de API's aanvult door methoden en zakelijke logica voor het toewijzen van bronnen voor niet-gecodeerde of untaggable te definiëren.

![Kostenverdeling 360 grafiek][4]

+ **Cost-Effective Sizing** - aanbevelingen rechts aanpassen voor onderbenutte virtuele machines, dus lagere kosten van de klant op te groot of te weinig ingerichte machines. Dit gebeurt door het onderzoeken van virtuele machine CPU en RAM metrics (via prestaties API) uur runtime (via de API voor gebruik) en kosten (via de API RateCard). Cloudyn rechts sizing aanbevelingen op basis van onderbenutte CPU of RAM-bronnen (prestaties), en berekent de geschatte besparingen door te vermenigvuldigen met de prijs delta (RateCard) tussen de VMs door de werkelijke tijd-gebruik (gebruik) van het onderbenutte bewerkingsplaats.

![Voordelige formaat][5]

+ **Wolk overbrengen van aanbevelingen** - biedt financieel advies op cloud overdragen. Wordt de huidige kosten van een gebruiker van cloud-middelen die worden ingezet op belangrijke wolk leveranciers, gecontroleerd en vergeleken met de kosten van een gelijkwaardige implementatie op Azure. Vervolgens wordt gedetailleerd, per resource, financieel gebaseerde aanbevelingen Azure overdragen. Na beoordeling van het gelijkwaardige implementatie vereist op Azure (op basis van de voorkeuren voor prestaties maatstaven en gebruiker), Cloudyn de RateCard-API gebruikt voor de evaluatie van de kosten van de gelijkwaardige implementatie op Azure.

+ **Prestatierapporten** - ingeschakeld door de prestaties van Azure API, deze rapporten bevatten een reeks functies van CPU en RAM-gebruik aanbevelingen voor optimalisatie. Hieronder is een instantie gebruik rapport voorbeeld, presenteert defect exemplaar door de gemiddelde CPU-gebruik.

![Van prestatierapporten][6]

+ **Categoriemanager** - een krachtige functie in Cloudyn die order naar ongeordende cloud resources brengt. Het biedt gebruikers de vrijheid voor het maken van hun eigen unieke categorieën (tags) voor het effectief meten en rapporteren die in overeenstemming met de bedrijfsvoering. Verder kunnen kunnen gebruikers eenvoudig regelen en categoriseren inconsistente tagging (typefouten en andere afwijkingen) en niet-gelabelde resources voor een nauwkeurige kostenberekening attribution automatisch detecteren.

![Categorie-Manager][7]

## <a name="video"></a>Video

Hier is een korte video die laat hoe een klant Azure gebruiken Cloudyn voor Azure en Azure facturering API's zien krijgen hun verbruiksgegevens Azure inzichten.

> [AZURE.VIDEO cloudyn-provides-cloud-itfm-tools-via-microsoft-azure-apis]


## <a name="next-steps"></a>Volgende stappen

+ Start een gratis proefversie [Cloudyn voor Azure](https://www.cloudyn.com/microsoft-azure/) om te zien hoe u kosten transparantie met aangepaste rapportage en analyse voor de implementatie van Microsoft Azure cloud kunt verkrijgen.
+ Zie [inzicht in uw verbruik van Microsoft Azure krijgen](billing-usage-rate-card-overview.md) voor een overzicht van het Resourcegebruik Azure en RateCard APIs.
+ Bekijk de [Azure facturering REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) voor meer informatie over beide API's die deel van de verzameling API's die door de Azure Resource Manager uitmaken.
+ Bekijk onze Microsoft Azure facturering API-codevoorbeelden op [Azure codevoorbeelden](https://azure.microsoft.com/documentation/samples/?term=billing)in als u wilt direct Duik in de voorbeeldcode.

## <a name="learn-more"></a>Meer informatie
+ Bezoek voor meer informatie over aanbiedingen van Microsoft Azure Enterprise overeenkomst (EA), [Azure licentieverlening voor de onderneming] (https://azure.microsoft.com/pricing/enterprise-agreement/)
+ Zie het artikel [Azure Resource Manager-overzicht](azure-resource-manager/resource-group-overview.md) voor meer informatie over de Azure Resource Manager.
+ Besteden, Gartner artikel [Markt handleiding voor extra IT financieel Management (ITFM)](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb)voor meer informatie over de suite met hulpprogramma's die nodig zijn om te helpen bij het verkrijgen van inzicht in de cloud.

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
