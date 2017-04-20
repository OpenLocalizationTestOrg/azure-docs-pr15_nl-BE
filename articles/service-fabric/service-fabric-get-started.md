<properties
   pageTitle="Instellen van uw ontwikkelomgeving | Microsoft Azure"
   description="De runtime, hulpprogramma's en SDK te installeren en het maken van een cluster van plaatselijke ontwikkeling. Nadat setup is voltooid, zult u klaar zijn om toepassingen te bouwen."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/26/2016"
   ms.author="ryanwi"/>

# <a name="prepare-your-development-environment"></a>Bereid uw ontwikkelomgeving

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Bouwen en uitvoeren van [toepassingen Azure Service Fabric] [ 1] installeren op de ontwikkelcomputer de runtime, hulpprogramma's en SDK. U moet ook tot het uitvoeren van scripts opgenomen in de SDK van Windows PowerShell.

## <a name="prerequisites"></a>Vereisten
### <a name="supported-operating-system-versions"></a>Ondersteunde besturingssystemen
Voor de ontwikkeling van de volgende besturingssystemen ondersteund:

- Windows 7
- Windows 8 Windows 8.1
- Windows Server 2012 R2
- Windows 10

>[AZURE.NOTE] Windows 7 bevat alleen Windows PowerShell 2.0 standaard. Service Fabric PowerShell-cmdlets vereist PowerShell 3.0 of hoger. U kunt [Windows PowerShell 5.0 downloaden] [ powershell5-download] vanaf het Microsoft Download Center.

## <a name="install-the-runtime-sdk-and-tools"></a>De runtime, hulpprogramma's en SDK te installeren

Het installatieprogramma van de Web-Platform biedt twee configuraties voor de ontwikkeling van Fabric-Service:

- [Installeer de Service Fabric runtime, SDK en hulpprogramma's voor Visual Studio 2015 (Visual Studio 2015 Update 2 of hoger vereist)][full-bundle-vs2015]
- [De Service Fabric runtime en alleen SDK (geen Visual Studio tools) installeren][core-sdk]

## <a name="enable-powershell-script-execution"></a>Uitvoering van het PowerShell script inschakelen

Fabric-service maakt gebruik van Windows PowerShell-scripts voor het maken van een cluster van plaatselijke ontwikkeling en voor het implementeren van toepassingen van Visual Studio. Windows blokkeert standaard deze scripts worden uitgevoerd. Als u wilt inschakelen, moet u het uitvoeringsbeleid PowerShell wijzigen. Open PowerShell als beheerder en voer de volgende opdracht:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u klaar bent met het instellen van uw ontwikkelomgeving, bouwen en uitvoeren van toepassingen starten.

- [De eerste Service Fabric-toepassing maken in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
- [Meer informatie over het implementeren en beheren van toepassingen op uw lokale cluster](service-fabric-get-started-with-a-local-cluster.md)
- [Meer informatie over het programmeren modellen: betrouwbare diensten en betrouwbare actoren](service-fabric-choose-framework.md)
- [Bekijk de Service Fabric codevoorbeelden op GitHub](https://aka.ms/servicefabricsamples)
- [Het cluster met behulp van Service Fabric Explorer visualiseren](service-fabric-visualizing-your-cluster.md)
- [Volg het pad Fabric Service learning als u een uitgebreide inleiding tot het platform](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric campagne pagina"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI koppeling"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI koppeling"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI koppeling"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
