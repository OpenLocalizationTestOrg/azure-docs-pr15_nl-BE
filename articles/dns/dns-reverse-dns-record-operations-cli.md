<properties
   pageTitle="Reverse DNS-records voor uw Azure services met Azure CLI beheren | Microsoft Azure"
   description="Het beheren van reverse DNS-records of PTR-records voor Azure services met behulp van de CLI Azure in Resource Manager"
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-using-the-azure-cli"></a>Reverse DNS-records voor uw Azure services met behulp van de CLI Azure beheren

[AZURE.INCLUDE [DNS-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](dns-reverse-dns-record-operations-classic-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Validatie van reverse DNS-records
Azure kunt dat reverse DNS-records toe te wijzen aan uw DNS-domeinen kan niet worden gemaakt door een derde partij, zodat alleen het maken van een omgekeerde DNS-record waarvoor het volgende geldt:

- De 'ReverseFqdn' is hetzelfde als de "Fully Qualified Domain Name" voor het openbare IP-adres resource die is ingesteld of 'Fully' voor alle openbare IP-adres binnen het abonnement hetzelfde b.v., "ReverseFqdn" "contosoapp1.northus.cloudapp.azure.com.".

- De 'ReverseFqdn' vooruit wordt omgezet in de naam of het IP van het openbare IP-adres voor die dit is opgegeven, of aan een openbaar IP-adres 'Fully' of IP-binnen het abonnement hetzelfde bijvoorbeeld 'ReverseFqdn' is 'app1.contoso.com'. Wat is een CNAME-alias voor 'contosoapp1.northus.cloudapp.azure.com.'

Validatiecontroles worden alleen uitgevoerd als de omgekeerde DNS-eigenschap voor een openbaar IP-adres wordt ingesteld of gewijzigd. Periodieke wordt opnieuw niet gevalideerd.

## <a name="add-reverse-dns-to-existing-public-ip-addresses"></a>Reverse DNS toevoegen aan bestaande openbare IP-adressen
U kunt reverse DNS toevoegen aan een bestaande openbare IP-adres met behulp van de openbare ip-netwerk azure set:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -f contosoapp1.westus.cloudapp.azure.com.

Als u wilt een reverse DNS toevoegen aan een bestaande openbare IP-adres dat nog niet beschikt over een DNS-naam, moet u ook een DNS-naam opgeven. Kunt u dit met behulp van de openbare ip-netwerk azure set toevoegen:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.

## <a name="create-a-public-ip-address-with-reverse-dns"></a>Het maken van een openbaar IP-adres met reverse DNS
Maken van een nieuwe openbare IP-adres met de omgekeerde DNS-eigenschap die is opgegeven met het openbare netwerk azure-ip, kunt u toevoegen:

    azure network public-ip create -n PublicIp3 -g NRP-DemoRG-PS -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.

## <a name="view-reverse-dns-for-existing-public-ip-addresses"></a>Reverse DNS voor de bestaande openbare IP-adressen weergeven
U kunt de geconfigureerde waarde weergeven voor een bestaande openbare IP-adres het openbare ip-netwerk azure weergeven met:

    azure network public-ip show -n PublicIp3 -g NRP-DemoRG-PS

## <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Reverse DNS verwijderen uit bestaande openbare IP-adressen
U kunt een reverse DNS-eigenschap verwijderen uit een bestaande openbare IP-adres met behulp van azure netwerk openbare ip-set. Dit wordt gedaan door de waarde van de eigenschap ReverseFqdn leeg:

    azure network public-ip set -n PublicIp3 -g NRP-DemoRG-PS –f “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-arm-include.md)]
