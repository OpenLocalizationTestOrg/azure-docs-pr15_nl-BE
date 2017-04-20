
<properties
    pageTitle="Informatie voor een VNET in Azure RemoteApp-formaat | Microsoft Azure"
    description="Klik hier voor informatie over de IP-adres voor Azure RemoteApp is uitgevoerd met een VNET"
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



# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Informatie voor een VNET in Azure RemoteApp-formaat

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Als u RemoteApp Azure met een virtueel netwerk (VNET), RemoteApp maakt gebruik van IP-adressen binnen het subnet. Op basis van de omvang van de RemoteApp-service, moet u ervoor zorgen dat uw subnet voldoende IP-adressen beschikbaar voor RemoteApp-virtuele machines. Terwijl deze instructies aanpassen niet perfect hoe RemoteApp dynamisch draait en draait u virtuele machines in een collectie, kunt u uw subnetbereik schatten. Dit is vooral van belang als u eenmaal een RemoteApp-service in een VNET wordt geplaatst, u kan niet groter subnet zonder RemoteApp.

U moet voor elk RemoteApp-collectie die u wilt uitvoeren op de maximale capaciteit 100 IP-adressen beschikbaar hebben. Bijvoorbeeld, als u een RemoteApp-collectie in het standaard plan hebt en u wilt het maximum 500 gebruikers hebben, moeten er 100 IP-adressen voor die collectie. Daarnaast moet u 100 IP-adressen voor een RemoteApp-collectie in het basisniveau met 800 gebruikers. Als u van plan bent minder gebruikers (kleiner dan het maximum), kunt u de IP-adressen nodig per collectie verkleinen. De vereiste minimale subnet grootte is 30 IP-adressen (/ 27).

De volgende informatie om te controleren of uw VNET uitchecken is geconfigureerd en werkt propertly:

- [Een persoonlijke VNET migreren naar een Azure-VNET](remoteapp-migratevnet.md)
- [De VNET Azure met Azure RemoteApp valideren](remoteapp-vnet.md)
