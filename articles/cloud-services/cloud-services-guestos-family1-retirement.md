<properties
   pageTitle="Gast OS familie 1 pensioen merken | Microsoft Azure"
   description="Informatie over wanneer de Azure Gast OS familie 1 pensioen is gebeurd en hoe om te bepalen als u last"
   services="cloud-services"
   documentationCenter="na"
   authors="raiye"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/24/2016"
   ms.author="raiye"/>



# <a name="guest-os-family-1-retirement-notice"></a>Aankondiging van de Gast OS familie 1 pensioen

OS-familie 1 met pensioen werd eerst aangekondigd op 1 juni 2013.

**2 Sept 2014** De Azure Gast besturingssysteem (OS Gast) familie 1.x, die is gebaseerd op het besturingssysteem Windows Server 2008, officieel werd teruggetrokken. Alle nieuwe services implementeren of een upgrade van bestaande services met 1 familie, niet met een foutbericht waarin wordt gemeld dat de Gast OS familie 1 is teruggetrokken.

**3 november 2014** Uitgebreide ondersteuning voor gast OS familie 1 beëindigd en het volledig is ingetrokken. Alle services nog steeds op 1-familie worden beïnvloed. We kunnen die services op elk gewenst moment stoppen. Er is geen garantie dat uw diensten actief blijft, tenzij u handmatig ze zelf upgraden.

Als u nog meer vragen hebt, ga dan naar de [Cloud Services Forums](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) of [Neem contact op met ondersteuning van Azure](https://azure.microsoft.com/support/options/).




## <a name="are-you-affected"></a>Worden beïnvloed?

Uw wolk diensten worden beïnvloed als een van de volgende geldt:

1. U hebt de waarde "osFamily ="1"expliciet zijn opgegeven in het bestand ServiceConfiguration.cscfg op uw Cloud-Service.
2. U hoeft niet een waarde voor osFamily die expliciet zijn opgegeven in het bestand ServiceConfiguration.cscfg op uw Cloud-Service. Op dit moment gebruikt het systeem de waarde "1" in dit geval.
3. De klassieke Azure portal worden het gastbesturingssysteem familie waarde weergegeven als 'Windows Server 2008 '.

Om te bepalen welke van uw wolk diensten welke OS-familie wordt uitgevoerd, kunt u het volgende script uitvoeren in Azure PowerShell, hoewel u eerst de [Azure PowerShell instellen](../powershell-install-configure.md) . Zie voor meer informatie over het script, [Azure Gast OS familie 1 einde van levenscyclus: juni 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx). 

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Uw wolk diensten worden beïnvloed door de OS-familie 1 pensioen als de kolom osFamily in de uitvoer van het script leeg is of bevat een '1'.

## <a name="recommendations-if-you-are-affected"></a>Aanbevelingen als u last

Wij raden u aan dat u uw rollen Cloud Service migreert naar een van de ondersteunde Gast OS Families:

**Gast OS familie 4.x** -Windows Server 2012 R2 *(aanbevolen)*

1. Zorg ervoor dat uw toepassing met .NET framework 4.0, 4.5 of 4.5.1 SDK 2.1 of hoger gebruiken.
2. Stelt u het kenmerk osFamily tot en met '4' in het bestand ServiceConfiguration.cscfg en implementeren van uw cloud-service.


**Gast OS 3.x familie** -Windows Server 2012

1. Zorg ervoor dat uw toepassing met .NET framework 4.0 of 4.5 SDK 1,8 of hoger gebruiken.
2. Stelt u het kenmerk osFamily naar "3" in het bestand ServiceConfiguration.cscfg en implementeren van uw cloud-service.


**Gast OS 2.x familie** -Windows Server 2008 R2

1. Zorg ervoor dat uw toepassing is SDK 1.3 en hoger met .NET framework 3.5 of 4.0.
2. Stelt u het kenmerk osFamily aan "2" in het bestand ServiceConfiguration.cscfg en implementeren van uw cloud-service.


## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Uitgebreide ondersteuning voor gast OS familie 1 afgelopen 3 Nov 2014
Cloud-services op het Gastbesturingssysteem familie 1 worden niet langer ondersteund. Migreer uit 1 familie zo spoedig mogelijk aan onderbrekingen te voorkomen.  

## <a name="next-steps"></a>Volgende stappen
Bekijk de nieuwste [Gast OS releases](cloud-services-guestos-update-matrix.md).
