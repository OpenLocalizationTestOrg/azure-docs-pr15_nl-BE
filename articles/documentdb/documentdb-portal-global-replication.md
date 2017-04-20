<properties
    pageTitle="DocumentDB algemene databasereplicatie | Microsoft Azure"
    description="Informatie over het beheren van de globale replicatie van uw DocumentDB-account via de portal Azure."
    services="documentdb"
    keywords="globale database, replicatie"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="mimig"/>

# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>Het uitvoeren van DocumentDB globale databasereplicatie via de portal Azure

Informatie over de Azure portal gebruiken voor het repliceren van gegevens in meerdere regio's voor de algemene beschikbaarheid van de gegevens in Azure DocumentDB.

Voor meer informatie over hoe de globale database replicatie in DocumentDB, gaat [verdelen gegevens wereldwijd met DocumentDB](documentdb-distribute-data-globally.md). Zie voor meer informatie over het uitvoeren van replicatie van de globale database programmatisch [ontwikkelen met meerdere regio DocumentDB rekeningen](documentdb-developing-with-multiple-regions.md).

> [AZURE.NOTE] Wereldwijde distributie van DocumentDB databases in het algemeen beschikbaar is en automatisch ingeschakeld voor alle nieuwe accounts DocumentDB. Wij werken om de wereldwijde distributie van alle bestaande accounts inschakelen, maar in de tussentijd, als u wilt dat de wereldwijde distributie is ingeschakeld op uw account, neem [contact op met support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) en wij gaat instellen voor u nu.

## <a id="addregion"></a>Regio's globale database toevoegen

DocumentDB is beschikbaar in de meeste [gebieden Azure] [azureregions]. Als u het standaardniveau voor consistentie voor de databaseaccount van uw, koppelt u een of meer regio's (afhankelijk van uw keuze standaard consistent niveau en wereldwijde distributie behoeften).

1. Klik in de [portal Azure](https://portal.azure.com/), in de Jumpbar, **DocumentDB-Accounts**.
2. Selecteer de databaseaccount te wijzigen in de blade **DocumentDB Account** .
3. In het blad account klikt u op **globaal gegevens repliceren** in het menu.
4. Selecteer de regio's toevoegen of verwijderen in de blade **globaal gegevens repliceren** en klik op **Opslaan**. Er is een kosten voor het toevoegen van regio's, Zie de [prijzen pagina](https://azure.microsoft.com/pricing/details/documentdb/) of het [verdelen van gegevens wereldwijd met DocumentDB](documentdb-distribute-data-globally.md) -artikel voor meer informatie.

    ![Klik op de regio's in de toewijzing die u wilt toevoegen of verwijderen][1]

### <a name="selecting-global-database-regions"></a>Regio's globale database selecteren

Bij het configureren van twee of meer gebieden, wordt aanbevolen dat regio's worden geselecteerd op basis van de regio-paren die worden beschreven in de [Business continuïteit en disaster recovery (BCDR): Azure gekoppeld regio's]  [ bcdr] artikel.

Bij het configureren van meerdere regio's, controleert u met name hetzelfde aantal regio's (+/-1 voor even en oneven) van elk van de kolommen gepaarde regio selecteren. Bijvoorbeeld als u implementeren op vier Amerikaanse regio's wilt, selecteert u twee Amerikaanse regio's in de linkerkolom en twee vanaf de rechterkant. De volgende zou dus een passende set: West VS, Oost-VS, North Central ons en South Central ons.

Deze richtsnoeren is het belangrijk dat u volgt als u slechts twee regio's zijn geconfigureerd voor herstel na storing scenario's. Voor meer dan twee regio's, na deze instructies is een goed idee, maar geen kritische als sommige van de geselecteerde regio's aan deze combinatie voldoen.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a id="next"></a>Volgende stappen

Informatie over het beheren van de consistentie van uw account globaal gerepliceerde [consistentie niveaus in DocumentDB](documentdb-consistency-levels.md)lezen.

Voor meer informatie over hoe de globale database replicatie in DocumentDB, gaat [verdelen gegevens wereldwijd met DocumentDB](documentdb-distribute-data-globally.md). Zie voor meer informatie over het repliceren van gegevens in meerdere regio's via programmacode [ontwikkelen met meerdere regio DocumentDB rekeningen](documentdb-developing-with-multiple-regions.md).

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/
