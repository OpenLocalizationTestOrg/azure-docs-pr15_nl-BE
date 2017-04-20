<properties
   pageTitle="Azure Virtual Machine DotNet Core 1 zelfstudie | Microsoft Azure"
   description="Azure Virtual Machine DotNet Core zelfstudie"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="automating-application-deployments-to-azure-virtual-machines"></a>Automatiseren van toepassing implementaties Azure virtuele Machines

Deze vierdelige reeks gegevens implementeren en configureren van Azure resource en toepassingen met behulp van sjablonen Azure Resource beheren. In deze serie, een voorbeeldsjabloon wordt geïmplementeerd en de implementatiesjabloon onderzocht. Het doel van deze reeks is om te informeren over het verband tussen Azure bronnen en voor handen op ervaring volledig geïntegreerde Azure Resource Manager templates implementeren. Dit document wordt ervan uitgegaan dat een basisniveau van kennis met Azure Resource Manager, voordat u deze zelfstudie begint de planningfase basisconcepten Azure Resource Manager.

## <a name="music-store-application"></a>Muziek store-toepassing

Het staal dat wordt gebruikt in deze serie is een .net Core toepassing een muziek winkel winkelervaring te simuleren. Deze toepassing kan worden geïmplementeerd op een Linux of Windows virtuele systeem, monster implementaties voor beide zijn gemaakt. De aanvraag bevat een webtoepassing en een SQL-database. Implementeren voordat u de artikelen in deze serie lezen, de toepassing met de implementatie van knop gevonden op deze pagina. Wanneer het volledig is geïmplementeerd, wordt de toepassing / Azure architectuur ziet eruit als in het volgende diagram. 

De sjabloon muziek winkel Resource Manager vindt u hier, [Muziek winkel Windows-sjabloon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)

![Muziek Store-toepassing](./media/virtual-machines-windows-dotnet-core/music-store.png)

Elk van deze onderdelen, inclusief de bijbehorende sjabloon JSON is onderzocht in de volgende vier artikelen.

- [**Application Architecture**](./virtual-machines-windows-dotnet-core-2-architecture.md) – onderdelen zoals websites en databases moeten worden gehost op Azure computerbronnen, zoals virtuele machines en Azure SQL-databases. Dit document helpt bij toewijzing compute nodig, naar Azure bronnen en het implementeren van deze bronnen met een sjabloon Azure Resource Manager. 

- [**Toegang en beveiliging**](./virtual-machines-windows-dotnet-core-3-access-security.md) – hosting van toepassingen in Azure, is het noodzakelijk te overwegen hoe de toepassing wordt geopend en toegang tot onderdelen van verschillende toepassingen elkaar. Dit document details verstrekken en beveiligen van internettoegang tot een toepassing en tussen toepassingsonderdelen van de.

- [**Beschikbaarheid en schaal**](./virtual-machines-windows-dotnet-core-4-availability-scale.md) – beschikbaarheid en schaal verwijzen naar toepassingen kunnen blijven uitvoeren tijdens de uitvaltijd van de infrastructuur en de mogelijkheid voor het schalen van compute bronnen om te voldoen aan de vraag van toepassing. Dit Documentdetails van de onderdelen die nodig zijn voor de implementatie van een taakverdeling en hoge beschikbaarheid van toepassing.

- [**Implementatie van toepassingen**](./virtual-machines-windows-dotnet-core-5-app-deployment.md) - bij het implementeren van toepassingen naar Azure virtuele Machines, de methode waarmee de toepassing binaire bestanden zijn geïnstalleerd op de virtuele Machine moet rekening worden gehouden. Dit document details automatiseren installatie van toepassingen met behulp van Azure Virtual Machine aangepast scriptextensies.

Het doel bij het ontwikkelen van sjablonen voor Azure Resource Manager is voor het automatiseren van de implementatie van Azure-infrastructuur en de installatie en configuratie van toepassingen die worden gehost op deze infrastructuur Azure. Werken met deze artikelen hier een voorbeeld van deze ervaring.

## <a name="deploy-the-music-store-application"></a>De muziek winkel-toepassingen distribueren

De toepassing van de winkel kan worden geïmplementeerd met behulp van deze knop.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-windows%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

De sjabloon Azure Resource Manager moet de volgende parameterwaarden.

|Naam van de parameter |Beschrijving   |
|---|---|
|ADMINUSERNAME   | Gebruikersnaam Admin die op de virtuele machine en de Azure SQL-Database wordt gebruikt.  |
|ADMINPASSWORD | Het wachtwoord dat wordt gebruikt op de Azure Virtual Machine en een SQL-Database.  |
|NUMBEROFINSTANCES | Het aantal virtuele machines te maken. Elk van deze virtuele machines muziek winkel-webtoepassing als host en al het verkeer wordt gelijkmatig verdeeld over hen. |
|PUBLICIPADDRESSDNSNAME | Globaal unieke DNS-naam die is gekoppeld aan het openbare IP-adres. |

Wanneer de sjabloonimplementatie is voltooid, gaat u naar de openbare IP-adres met behulp van een internet-browser. De .net Core muziek site wordt gepresenteerd.

## <a name="next-steps"></a>Volgende stappen

<hr>

[Stap 1 - toepassingsarchitectuur met Azure Resource Manager-sjablonen](./virtual-machines-windows-dotnet-core-2-architecture.md)

[Stap 2 - toegang en beveiliging in Azure Resource Manager-sjablonen](./virtual-machines-windows-dotnet-core-3-access-security.md)

[Stap 3: de beschikbaarheid en de schaal in Azure Resource Manager-sjablonen](./virtual-machines-windows-dotnet-core-4-availability-scale.md)

[Stap 4 - installatie van toepassingen met Azure Resource Manager-sjablonen](./virtual-machines-windows-dotnet-core-5-app-deployment.md)


