<properties
   pageTitle="Een Azure cluster en de bijbehorende bronnen verwijderen | Microsoft Azure"
   description="Informatie over hoe u een Service Fabric cluster wilt verwijderen of verwijderen van de bronnengroep met de cluster of door bronnen selectief verwijderen."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>

# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a>Verwijderen van een cluster Service Fabric op Azure en de bronnen die worden gebruikt

Een Service Fabric '-cluster bestaat uit vele andere Azure bronnen dan de clusterbron zelf. Dus voor het volledig verwijderen van een Service Fabric '-cluster u ook verwijderen van alle bronnen moet die van wordt gemaakt.
Hebt u twee mogelijkheden: ofwel de resourcegroep die het cluster verwijderen (die verwijdert de clusterbron en andere bronnen in de bronnengroep) of specifiek de clusterbron verwijderen en de bijbehorende bronnen (maar niet door andere resources in de resourcegroep).

>[AZURE.NOTE] De cluster resource **niet** verwijdert, alle andere bronnen die de cluster-Service Fabric is samengesteld uit.

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a>De hele resourcegroep (RG) de cluster-Service Fabric in verwijderen

Dit is de eenvoudigste manier om ervoor te zorgen dat alle resources die zijn gekoppeld aan het cluster, met inbegrip van de resourcegroep worden verwijderd. U kunt de resourcegroep met PowerShell verwijderen of via de portal Azure. Als de resourcegroep bronnen die niet zijn gerelateerd aan de fabric '-cluster Service heeft, kunt u bepaalde resources verwijderen.

### <a name="delete-the-resource-group-using-azure-powershell"></a>De resourcegroep met Azure PowerShell verwijderen

U kunt de resourcegroep ook verwijderen door de volgende Azure PowerShell cmdlets uit te voeren. Zorg ervoor dat Azure PowerShell 1.0 of hoger op uw computer is geïnstalleerd. Als u dit nog niet hebt gedaan, voert u de stappen in [installeren en configureren van Azure PowerShell.](../powershell-install-configure.md)

Open een venster PowerShell en voer de volgende cmdlets voor PS:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Wordt u gevraagd het verwijderen te bevestigen als u niet hebt gebruikt de *-Force* optie. Op de bevestiging worden de RG en alle bronnen die erin verwijderd.

### <a name="delete-a-resource-group-in-the-azure-portal"></a>Een resourcegroep in Azure portal verwijderen  

1. Aanmelden bij de [Azure portal](https://portal.azure.com).
2. Ga naar de Service Fabric-cluster dat u wilt verwijderen.
3. Klik op de naam van de resourcegroep op de pagina cluster essentials.
4. Verschijnt de pagina **Resource groep Essentials** .
5. Klik op **verwijderen**.
6. Volg de instructies op die pagina voor het voltooien van de verwijdering van de resourcegroep.

![Resourcegroep verwijderen][ResourceGroupDelete]


## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a>De clusterbron en de bronnen die wordt gebruikt, maar niet door andere resources in de resourcegroep verwijderen

Als de resourcegroep alleen de resources die zijn gerelateerd aan de Service Fabric '-cluster te verwijderen heeft, is het gemakkelijker om de hele groep verwijderen. Als u de resources één voor één in de resourcegroep selectief verwijderen wilt, vervolgens als volgt te werk.

Als u het cluster met behulp van de portal of met een van de sjablonen Service Fabric Resource Manager uit de galerie met lijstsjablonen geïmplementeerd, worden alle bronnen die het cluster gebruikt gelabeld met de volgende twee codes. U kunt ze gebruiken om te bepalen welke resources u wilt verwijderen.

***Label #1:*** Toets = clusternaam, waarde = 'naam van het cluster'

***Code 2:*** Sleutel resourceName, waarde = ServiceFabric =

### <a name="delete-specific-resources-in-the-azure-portal"></a>Bepaalde resources in Azure portal verwijderen

1. Aanmelden bij de [Azure portal](https://portal.azure.com).
2. Ga naar de Service Fabric-cluster dat u wilt verwijderen.
3. Ga naar **alle instellingen** op het blad essentials.
4. Klik op de **labels** onder **Beheer van de Resource** in de blade instellingen.
5. Klik op een van de **codes** in de blade codes om een lijst van alle bronnen met dat label.

    ![Bron codes][ResourceTags]

6. Als u de lijst van gelabelde resources hebt, klikt u op elk van de bronnen en verwijder deze.

    ![Gelabelde Resources][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a>De bronnen met Azure PowerShell verwijderen

U kunt de bronnen een voor een verwijderen door het uitvoeren van de volgende Azure PowerShell-cmdlets. Zorg ervoor dat Azure PowerShell 1.0 of hoger op uw computer is geïnstalleerd. Als u dit nog niet hebt gedaan, voert u de stappen in [installeren en configureren van Azure PowerShell.](../powershell-install-configure.md)

Open een venster PowerShell en voer de volgende cmdlets voor PS:

```powershell
Login-AzureRmAccount
```
Voor elk van de resources die wilt u verwijderen, de volgende:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

U verwijdert het cluster resource, het volgende uitvoeren:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a>Volgende stappen
Lees ook meer informatie over het upgraden van een cluster en partitionering van de diensten van de volgende:

- [Meer informatie over de cluster-upgrades](service-fabric-cluster-upgrade.md)
- [Meer informatie over het partitioneren van stateful services voor maximale schaal](service-fabric-concepts-partitioning.md)


<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
