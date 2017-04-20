<properties
    pageTitle="Maak een Azure Search-service met behulp van de Portal Azure | Microsoft Azure | De zoekservice hosted cloud"
    description="Informatie over het inrichten van een Azure Search-service met behulp van de Portal Azure."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-service-using-the-azure-portal"></a>Maak een Azure Search-service met behulp van de Portal Azure

Deze handleiding begeleidt u stapsgewijs door het proces van het maken van (of provisioning) een Azure Search-service met behulp van de [Portal Azure](https://portal.azure.com/).

Deze handleiding wordt ervan uitgegaan dat u al een abonnement Azure en zich bij de Portal Azure aanmelden kan.

## <a name="find-azure-search-in-the-azure-portal"></a>Azure Search zoeken in de Portal voor Azure
1. Ga naar de [Azure Portal](https://portal.azure.com/) en meld u aan.
1. Klik op het plusteken ("+") in de linkerbovenhoek.
2. Selecteer **gegevens + opslag**.
3. Selecteer **Azure zoeken**.

![](./media/search-create-service-portal/find-search.png)

## <a name="pick-a-service-name-and-url-endpoint-for-your-service"></a>Kies een servicenaam en het eindpunt van de URL voor uw service
1. De servicenaam van uw zal deel uitmaken van eindpunt-URL van uw Azure Search service waartegen u de API-aanroepen brengt te beheren en de zoekservice gebruiken.
2. Typ uw naam in het veld **URL** . De naam van de service:
  * mag alleen kleine letters, cijfers of streepjes ('-')
  * een streepje niet gebruiken ('-') als het laatste teken of de eerste 2 tekens
  * geen opeenvolgende streepjes ('--')
  * alleen tussen 2 en 60 tekens bevatten


## <a name="select-a-subscription-where-you-will-keep-your-service"></a>Selecteer een abonnement waarbij u uw service wordt behouden
Als u meer dan één abonnement hebt, kunt u selecteren welke deze Azure Search-service bevat.

## <a name="select-a-resource-group-for-your-service"></a>Selecteer een resourcegroep voor uw service
Een nieuwe resourcegroep maken of een bestaande selecteren. Een resourcegroep is een verzameling van Azure services en bronnen die samen worden gebruikt. Bijvoorbeeld, als u een SQL-database indexeren via Azure Search, moeten vervolgens beide services deel uitmaken van dezelfde bronnengroep.

## <a name="select-the-location-where-your-service-will-be-hosted"></a>Selecteer de locatie waar u uw service wordt gehost
Als een Azure service is Azure zoeken beschikbaar voor zijn ondergebracht in datacenters over de hele wereld. Houd er rekening mee dat [prijzen kunnen verschillen](https://azure.microsoft.com/pricing/details/search/) door Geografie.

## <a name="select-your-pricing-tier"></a>De prijzen laag selecteren
[Azure zoeken is momenteel verkrijgbaar in meerdere prijzen lagen](https://azure.microsoft.com/pricing/details/search/): vrij, Basic of Standard. Elke laag heeft een eigen [capaciteit en grenzen](search-limits-quotas-capacity.md). [Kies een prijzen laag of SKU](search-sku-tier.md) Zie voor instructies.

In dit geval hebben we de standaard laag gekozen voor onze service.

## <a name="select-the-create-button-to-provision-your-service"></a>Klik op de knop 'Maken' inrichten van uw service

![](./media/search-create-service-portal/create-service.png)

## <a name="scale-your-service"></a>Schalen van uw service

Nadat de service is ingericht, kunt u deze aan uw behoeften te schalen. Als u de standaard laag voor uw Azure Search-service hebt gekozen, kunt u uw service schalen in twee dimensies: replica's en partities. Als u de laag standaard hebt gekozen, kunt u replica's alleen toevoegen.

*__Partities__* kunnen uw service voor het opslaan en meer documenten doorzoeken.

*__Replica's__* kunnen uw service voor het verwerken van een hogere belasting van zoekquery's - [een service vereist 2 replica's te bereiken een SLA alleen-lezen en vereist 3 replica's te bereiken een SLA lezen/schrijven](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Ga naar uw Azure Search-service management blade in Azure Portal.
2. Selecteer in het blad **Instellingen** **schaal**.
3. U kunt uw service schalen door replica's of partities toe te voegen.
  * U kunt uw service na 36 Zoeken eenheden kan niet schalen. Het totale aantal eenheden zoeken is het product van de partities en de replica's (replica's * partities = totaal aantal Search-eenheden).
  * Als u de laag standaard hebt gekozen, kunt u alleen de schaal 3 replica's. Services voor de Basic zijn afhankelijk van een enkele partitie.

![](./media/search-create-service-portal/scale-service.png)

## <a name="next"></a>Volgende
Na het inrichten van een Azure Search-service u worden [een zoekindex Azure](search-what-is-an-index.md) definiëren zodat u kunt uploaden en uw gegevens te zoeken.

Zie [aan de slag met Azure zoeken in de portal](search-get-started-portal.md) voor een snelle zelfstudie.
