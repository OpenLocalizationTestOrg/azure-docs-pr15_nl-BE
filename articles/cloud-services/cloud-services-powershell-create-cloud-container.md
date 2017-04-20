<properties
   pageTitle="Maak een cloud servicecontainer met PowerShell | Microsoft Azure"
   description="In dit artikel wordt uitgelegd hoe u een container cloud-service maken met PowerShell. De container fungeert als host voor websites en werknemer rollen."
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="cawa"/>

# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Gebruik een Azure PowerShell-opdracht voor het maken van een container leeg cloud-service
In dit artikel wordt uitgelegd hoe u snel een Cloud Services-container met Azure PowerShell-cmdlets maken. Volg de onderstaande stappen:

1. De Microsoft Azure PowerShell-cmdlet installeren vanaf de pagina [downloads Azure PowerShell](http://aka.ms/webpi-azps) .
2. De PowerShell-opdrachtprompt te openen.
3. Gebruik de [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) aan te melden.

    > [AZURE.NOTE] Raadpleeg voor nadere instructies over het installeren van de Azure PowerShell-cmdlet en verbinding maken met uw abonnement Azure [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

4. Gebruik de cmdlet **New-AzureService** voor het maken van een container leeg Azure cloud-service.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

5. Volg dit voorbeeld om aan te roepen de cmdlet:
```
New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
```

Voor meer informatie over het maken van de Azure cloud-service worden uitgevoerd:
```
Get-help New-AzureService
```

### <a name="next-steps"></a>Volgende stappen

 * Verwijzen naar de [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx)en [Software-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx) [Set AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) -opdrachten voor het beheren van de implementatie van de service cloud. U kunt ook verwijzen naar [cloud-services configureren](cloud-services-how-to-configure.md) voor meer informatie.

 * Raadpleeg voor het publiceren van uw project cloud service naar Azure, in het voorbeeld **PublishCloudService.ps1** bij [continue levering voor service in Azure cloud](cloud-services-dotnet-continuous-delivery.md).
