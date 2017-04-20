
<properties
    pageTitle="Valideer de Azure VNET met Azure RemoteApp | Microsoft Azure"
    description="Informatie over hoe om te controleren of dat uw VNET Azure is klaar voor gebruik met Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>De VNET Azure met Azure RemoteApp valideren

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Voordat u een Azure-VNET met Azure RemoteApp gebruiken, kunt u voor het valideren van de VNET. Dit voorkomt problemen met connectiviteit.

Voor het valideren van uw VNET Azure, het volgende doen:

1. Maak een virtuele machine met Azure binnen het subnet van de Azure-VNET die u wilt gebruiken in combinatie met Azure RemoteApp.

2. Verbinding maken met deze VM met behulp van de optie **verbinding maken** in de portal beheren.
3. De virtuele machine toevoegen aan het domein dat u wilt gebruiken in combinatie met Azure RemoteApp. Als u een hybride-collectie die is verbonden met het netwerk op gebouwen maakt, deelnemen aan de virtuele machine op uw lokale domein.

Als dit lukt, is de VNET Azure klaar voor gebruik met RemoteApp.

Raadpleeg de volgende artikelen voor meer informatie over de collectie end-to-end hybride werkstroom:

- [Het plannen van het virtuele netwerk voor Azure RemoteApp](remoteapp-planvnet.md)
- [Een collectie hybride maken](remoteapp-create-hybrid-deployment.md)
- [Collectie Azure RemoteApp implementeren op het virtuele netwerk Azure (met ondersteuning voor ExpressRoute)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)
