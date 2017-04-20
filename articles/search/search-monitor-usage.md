<properties 
   pageTitle="Gebruik en de statistieken in een Azure Search-service controleren | Microsoft Azure | De zoekservice hosted cloud" 
   description="Verbruiks- en grootte van resource bijhouden voor Azure zoeken, een zoekservice cloud gehost op Microsoft Azure." 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="jhubbard" 
   editor=""
   tags="azure-portal"/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required" 
   ms.date="05/17/2016"
   ms.author="heidist"/>

# <a name="monitor-usage-and-statistics-in-an-azure-search-service"></a>Gebruik en de statistieken in een Azure Search-service controleren

Bijhouden van de groei van de indexen en de documentgrootte van het kunt u proactief capaciteit aanpassen voordat het raken van de bovengrens die u hebt ingesteld voor uw service. 

Als u wilt controleren op Resourcegebruik, aantallen en statistieken voor uw service zijn gemakkelijk worden bekeken in [Azure Portal](https://portal.azure.com), maar verder kunt u de gegevens via programmacode als u een aangepaste service-beheerprogramma. In dit artikel worden de stappen voor beide technieken.

U kunt ook de nieuwe functie zoeken verkeer analytics gebruiken om inzicht te krijgen in de activiteit op het indexniveau van de. Ga naar [Zoeken verkeer Analytics voor Azure Search](search-traffic-analytics.md) aan de slag.

##<a name="view-counts-and-metrics-in-the-portal"></a>Tellingen en statistieken in de portal weergeven 

1. Aanmelden bij de [Azure Portal](https://portal.azure.com). 

2. Open het servicedashboard van uw Azure Search-service. Tegels voor de service kunnen u vinden op de introductiepagina of u kunt bladeren naar de service van de bladeren op de JumpBar. Zie [een service maken](search-create-service-portal.md) voor stapsgewijze instructies.

De sectie gebruik omvat een meter die aangeeft welk gedeelte van de beschikbare middelen zijn momenteel in gebruik.

  ![][1]

Dat de gedeelde service een maximum van één replica heeft intrekken en elke partitie. Bovendien 10.000 documenten kan alleen worden ondersteund in totaal of 50 MB, afhankelijk van wat zich het eerst.

##<a name="get-index-statistics-using-the-rest-api"></a>Met de REST API indexstatistieken opvragen

Zowel de Azure Search REST API en de SDK voor .NET programmatisch toegang biedt tot service maatstaven.  Als u een index van Azure SQL-Database of DocumentDB, een extra API laden [indexeerfuncties](https://msdn.microsoft.com/library/azure/dn946891.aspx) is beschikbaar voor het ophalen van de nummers die u nodig hebt. 

  + [Indexstatistieken opvragen](https://msdn.microsoft.com/library/azure/dn798942.aspx)
  + [Aantal documenten](https://msdn.microsoft.com/library/azure/dn798924.aspx)
  + [Indexeerfunctie Status ophalen](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## <a name="next-steps"></a>Volgende stappen

Bekijk de [grenzen en de capaciteit](search-limits-quotas-capacity.md) om te bepalen welke combinatie van partities en replica's die u nodig hebt als de bestaande capaciteit ontoereikend is. 

Ga naar [de zoekservice op Microsoft Azure beheren](search-manage.md) voor meer informatie over het servicebeheer.

<!--Image references-->
[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG




 
