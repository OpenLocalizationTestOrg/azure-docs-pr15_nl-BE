<properties 
   pageTitle="DNS-instellingen op te geven in een configuratiebestand service | Microsoft Azure"
   description="aangepaste DNS-instellingen voor virtuele netwerk met behulp van service-configuratiebestand opgeven"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/24/2016"
   ms.author="jdial" />

# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>DNS-instellingen op te geven in een configuratiebestand Service

## <a name="dns-elements"></a>DNS-elementen

Een configuratiebestand service kan een lijst met IPv4-adressen voor de Domain Name System (DNS) servers die door de service met een DnsServers-element bevatten. Instellingen in het configuratiebestand service voorrang op instellingen in het configuratiebestand van het netwerk. Zie [Azure Service configuratieschema (.cscfg-bestand)](https://msdn.microsoft.com/library/azure/ee758710.aspx)voor meer informatie.

**NetworkConfiguration-element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] Het kenmerk **name** in het element van de **DNS-server** wordt alleen gebruikt als een verwijzing naar de naam. Het staat niet voor de hostnaam van de DNS-server. De kenmerkwaarde voor elke **DNS-server** moet uniek zijn in het hele Microsoft Azure-abonnement.

## <a name="see-also"></a>Zie ook

[Configuratieschema Azure Service (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure Virtual Network configuratieschema](http://go.microsoft.com/fwlink/?LinkId=248093)

[Een virtueel netwerk met behulp van de configuratiebestanden van netwerk configureren](http://go.microsoft.com/fwlink/?LinkId=248094)

[Over virtueel netwerk-instellingen in de Portal beheren](http://go.microsoft.com/fwlink/?LinkId=248092)

