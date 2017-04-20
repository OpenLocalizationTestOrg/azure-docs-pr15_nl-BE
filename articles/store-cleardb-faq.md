<properties
    pageTitle="Veelgestelde vragen over het ClearDB MySql-databases met Azure App Service | Microsoft Azure"
    description="Antwoorden op veelgestelde vragen over het gebruik van ClearDB MySQL-databases met Azure App-Service."
    documentationCenter="php"
    services=""
    authors="sunbuild"
    manager="yochayk"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="sumuth"/>

# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Veelgestelde vragen over het ClearDB MySql-databases met Azure App-Service

Deze Veelgestelde vragen antwoorden op veelgestelde vragen over het gebruik en de aanschaf van ClearDB MySQL databases voor Azure Web Apps.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Welke opties heb ik voor MySQL op Azure

U hebt verschillende opties:

* [ClearDB gedeelde MySQL-database](/marketplace/partners/cleardb/databases/)

* [ClearDB MySQL Premium clusters](/marketplace/partners/cleardb-clusters/cluster/)

* [MySQL cluster op een Azure VM](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)

* [Eén exemplaar van MySQL op een Azure VM](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md)

ClearDB is een hostingservice MySQL en beheert de infrastructuur van MySQL voor u. Wanneer u uw eigen MySQL cluster of een database op een Azure Virtual Machine uitvoert, hebt u de MySQL-server instellen en werken met patches.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Moet ik een creditcard voor de sjabloon in de markt Azure MySQL + Web app?

Dit hangt af van het soort abonnement dat u gebruikt. Hieronder staan enkele typen veelgebruikte abonnement:

* [Omslagstelsel](/offers/ms-azr-0003p/): vereist een creditcard en bij aankoop van een betaalde MySQL-database wordt afgeschreven van je creditcard.

* [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/): uitgerust met credits voor gebruik met Microsoft Azure services maar aankoop van middelen van derden is niet toegestaan. Voor aankoop van services van derden of een betaalde MySQL-database, moet u een abonnement ingeschakeld creditcard gebruiken. U kunt een gratis ClearDB MySQL-database maken voor Web-Apps.

* [MSDN-abonnement](/pricing/member-offers/msdn-benefits/) en een **MSDN Dev Test betalen als u go**: net als de gratis proefperiode, een MSDN-abonnement, moet u een creditcard voor aankoop van een betaalde MySQL-oplossing van ClearDB.

* [Enterprise-overeenkomst (EA)](/pricing/enterprise-agreement/): EA klanten worden gefactureerd tegen hun EA elk kwartaal voor al hun aankopen Azure Marketplace (derden) op een aparte, geconsolideerde factuur. Je krijgt de rekening buiten de monetaire verbintenis voor alle aankopen marketplace. Houd er rekening mee dat op dit moment is Azure archief is niet beschikbaar voor klanten die zijn ingeschreven in Azerbeidzjan, Kroatië, Noorwegen en Puerto Rico. 

*  [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): kunt u alleen databases die zijn vrije ClearDB voor Web Apps. Er is geen limiet op het aantal vrije ClearDB MySQL-databases die kunt u maken. Opmerking: de gratis databases niet worden gebruikt voor de productie van webtoepassingen, mogen als deze service is alleen bedoeld voor evaluatie.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Waarom was ik $3,50 voor een Web app + MySQL van Azure Marketplace in rekening gebracht?

De database standaard wordt Titan is $3,50. We de kosten niet weergeven tijdens het maken van de database en u per ongeluk een database die u niet wilt aanschaffen. Wij probeert te zoeken naar een manier om de ervaring te verbeteren, maar tot die tijd moet u alle uw geselecteerde prijzen lagen van web app en database controleren voordat u op **maken** te klikken en de implementatie van de resources te starten.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>MySQL heb ik op mijn eigen Azure virtuele machine. Kan ik mijn Azure web app met mijn database aansluiten?

Ja. Als uw Azure VM externe toegang tot uw web app heeft gegeven, kunt u uw web app aan uw database. Zie voor meer informatie de [MySQL installeren op een virtuele machine](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>In welke landen zijn ClearDB Premium MySQL clusters ondersteund?

[ClearDB Premium MySQL clusters](/marketplace/partners/cleardb-clusters/cluster/) zijn beschikbaar in alle Azure regio's wereldwijd, met uitzondering van India, Australië, Zuid-Brazilië en China.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>Kan ik een nieuw cluster maken van een database met ClearDB premium-clusteroplossing maken?

Nee, lege ClearDB clusters maken wordt niet ondersteund. De Azure portal kunt u databases kunt maken in een cluster, die een nieuw cluster op hetzelfde moment maken mogelijk.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Ik gewaarschuwd als ik probeer te verwijderen van een ClearDB MySQL-database die wordt gebruikt door een van mijn toepassingen?

Nee, Azure ontvangt u geen waarschuwing als u een aankoop marketplace die uw toepassing afhankelijk van is verwijderen.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Welke regio's kan ik databases in ClearDB maken?

Azure Marketplace is niet beschikbaar voor klanten die zijn ingeschreven in Azerbeidzjan, Kroatië, Noorwegen of Puerto Rico. ClearDB is niet beschikbaar in deze regio's.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Welke prijzen laag moet ik kiezen voor een productie web app en de database?

Basic of een hogere prijzen laag gebruiken voor Web Apps. Voor ClearDB, wordt aangeraden een Jupiter Saturnus of plan. Bekijk de kenmerken en beperkingen van elke laag prijzen voor [Web Apps](/pricing/details/app-service/) en [ClearDB MySQL-databases](/marketplace/partners/cleardb/databases/) te kiezen dat past bij uw behoeften.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Hoe voer ik een upgrade mijn ClearDB database van het ene plan naar de andere?

In de [Azure portal](https://portal.azure.com)kunt u de schaal van een database ClearDB gedeelde hosting. Lees dit [artikel](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) voor meer informatie. We ondersteunen op dit moment geen upgrade voor ClearDB Premium clusters in de portal Azure.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Ik kan mijn database ClearDB in Azure portal niet zien?

Als we ClearDB database met behulp van bronbeheer Azure of [Nieuwe Azure Portal](https://portal.azure.com)maakt, is het niet zijn zichtbaar in de [Oude Azure-Portal](https://manage.windowsazure.com). Om te werken-rond dit is de database handmatig te koppelen aan de web app. Op dezelfde manier als ClearDB database maken in de [oude portal](https://manage.windowsazure.com) u niet mogelijk voor een overzicht van de database in de [Nieuwe Portal van Azure](https://portal.azure.com). Er is geen tijdelijke oplossing voor het laatste scenario.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Die ik contact opnemen voor ondersteuning wanneer mijn database niet beschikbaar is?

Vraagstukken in verband met contact [ClearDB ondersteuning](https://www.cleardb.com/developers/help/support) voor een database. Uw abonnementsgegevens Azure voorzien worden voorbereid.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Kan ik extra gebruikers maken voor mijn ClearDB MySQL database clusteroplossing? 

Nr. U kunt geen extra gebruikers maken, maar u kunt meer databases maken op uw cluster ClearDB database.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Kan Basic/Pro-serie databases worden bijgewerkt ter plekke vergelijkbaar met planeten plannen vandaag in ClearDB portal?

Ja, basismodule databases zijn bijgewerkt ter plekke (standaard 60 tot en met 500 Basic). Pro-serie kunnen worden bijgewerkt ter plekke (Pro 125 en Pro 1000), met uitzondering van Pro 60. Pro 60-database op dat moment upgraden wordt niet ondersteund. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Wanneer ik mijn bronnen van een abonnement naar de andere migreren, is mijn ClearDB MySQL-database worden overgezet ook? 

Wanneer u een resource migratie uitvoert over abonnementen, gelden enkele [beperkingen](./app-service-web/app-service-move-resources.md) . Een ClearDB MySQL-database is een service van derden en dus niet worden overgezet tijdens de migratie van Azure abonnement. Als u niet de migratie van uw MySQL database voor migratie Azure resources beheert, kunnen uw ClearDB MySQL-databases worden uitgeschakeld. Eerst de databases handmatig migreren en vervolgens Azure abonnement migratie uitvoeren voor uw web app. 

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>Kan ik een ClearDB-database van een creditcard-abonnement overbrengen naar een EA-abonnement?

Bestaande ClearDB databases gebruiken de creditcard die is gekoppeld aan de bestaande abonnementen. Als een EA-abonnement wilt gebruiken, moet u uw gegevens migreren naar een nieuwe database:

* Een nieuwe database ClearDB bij uw EA-abonnement aanschaffen.
* Uw gegevens migreren naar de nieuwe database.
* Uw toepassing gebruik van de nieuwe database bijwerken.
* Uw oude ClearDB-database verwijderd.

Wanneer u een nieuw web app met MySQL (ClearDB) te maken of een MySQL-database (ClearDB) maakt, wordt het abonnement dat u kiest bepaalt hoe u betaalt voor de service. Met een abonnement EA blokkeert we niet de aanschaf van de services van andere leveranciers zoals ClearDB in het portal voor Azure. EA abonnementen buiten monetaire verbintenis worden gefactureerd en worden gefactureerd per kwartaal en achterstallig. De EA-klant zou hebben voor het instellen van een betalingsmethode, zoals een creditcard te betalen voor alle services van derden marketplace.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>Waar kan ik de kosten voor resources in een abonnement EA ClearDB zien?

Voor directe EA klanten zijn toeslagen Azure Marketplace zichtbaar op de Enterprise Portal. Houd er rekening mee dat alle marketplace inkopen en verbruik buiten de monetaire verbintenis worden gefactureerd en worden gefactureerd per kwartaal en achterstanden. EA klanten moeten betalen rechtstreeks aan de serviceproviders van derden en hiertoe een betalingsmethode, zoals een creditcard met hun EA account inschakelen.

Indirecte EA klanten hun abonnementen Azure Marketplace kunnen vinden op de pagina **Abonnementen beheren** van de Enterprise Portal, maar de prijs is verborgen. Klanten moeten contact opnemen met hun gelaagde Serviceprovider voor informatie over toeslagen marketplace.

Toegang tot de markt Azure voor services van andere leveranciers kan worden beheerd door de beheerders van de inschrijving EA Azure. Ze kunnen uitschakelen of opnieuw inschakelen toegang op 3de partij aankopen uit de winkel in Accounts beheren en abonnementen onder de sectie Accounts in Enterprise Portal.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Wie neem ik contact op voor vragen over mijn factuur voor ClearDB services in mijn abonnement EA?

Neem contact op met [Customer Support voor Enterprise](http://aka.ms/AzureEntSupport) voor facturering onder hun inschrijving EA. De EA Portal-ondersteuningsteam zal antwoord op uw vraag of uw probleem op te lossen.

 



## <a name="more-information"></a>Meer informatie

[Veelgestelde vragen over Azure Marketplace](/marketplace/faq/)
