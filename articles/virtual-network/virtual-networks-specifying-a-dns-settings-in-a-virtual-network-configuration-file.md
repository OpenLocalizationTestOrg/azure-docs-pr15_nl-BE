<properties 
   pageTitle="DNS-instellingen op te geven in een virtueel netwerk configuratiebestand | Microsoft Azure"
   description="Het wijzigen van instellingen van de DNS-server in een virtueel netwerk met behulp van een virtueel netwerk configuratiebestand in de klassieke implementatiemodel"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" 
   tags="azure-service-management" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" /> 


# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>DNS-instellingen op te geven in een virtueel netwerk configuratiebestand

Een configuratiebestand netwerk bestaat uit twee elementen die u kunt instellingen voor Domain Name System (DNS): **DnsServers** en **DnsServerRef**. U kunt een lijst met DNS-servers toevoegen door op te geven van hun IP-adressen en namen van de verwijzing naar het element **DnsServers** . Vervolgens kunt u een **DnsServerRef** -element op te geven welke DNS-serververmeldingen van het element DnsServers worden gebruikt voor verschillende sites binnen het virtuele netwerk.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op de klassieke implementatiemodel.

Het configuratiebestand van het netwerk kan de volgende elementen bevatten. De titel van elk element is gekoppeld aan een pagina met meer informatie over de instellingen van het element.

>[AZURE.IMPORTANT] Zie [een virtuele netwerk met behulp van een netwerk configuratiebestand configureren](virtual-networks-using-network-configuration-file.md)voor meer informatie over het configureren van het netwerk configuratiebestand. Zie voor informatie over elk element in het configuratiebestand van het netwerk, [Azure virtuele netwerk configuratieschema](https://msdn.microsoft.com/library/azure/jj157100.aspx).

[DNS-Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING] Het kenmerk **name** in het element van de **DNS-server** wordt alleen als een verwijzing gebruikt voor het element **DnsServerRef** . Het staat niet voor de hostnaam van de DNS-server. De kenmerkwaarde voor elke **DNS-server** moet uniek zijn in het hele Microsoft Azure-abonnement

[Virtueel netwerk Sites Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] Om deze instelling voor het virtuele netwerksites element opgeeft, het moet eerder zijn gedefinieerd in het DNS-element. De DnsServerRef *de naam* in het virtuele netwerksites element moet verwijzen naar de waarde voor een opgegeven in het element van DNS voor *de naam*van de DNS-server.

## <a name="next-steps"></a>Volgende stappen

- Het [configuratieschema Azure virtueel netwerk](http://go.microsoft.com/fwlink/?LinkId=248093)begrijpt.
- Begrijp het [configuratieschema Azure Service](https://msdn.microsoft.com/library/windowsazure/ee758710).
- [Een virtueel netwerk configureren met behulp van configuratiebestanden van netwerk](virtual-networks-using-network-configuration-file.md).
