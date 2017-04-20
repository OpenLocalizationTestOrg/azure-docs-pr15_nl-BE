<properties
   pageTitle="Reverse DNS-records voor uw Azure (klassiek)-services met PowerShell beheren | Microsoft Azure"
   description="Het beheren van reverse DNS-records of PTR-records voor Azure services met PowerShell in het klassieke implementatiemodel. "
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>Reverse DNS-records voor uw gebruik van Azure PowerShell Azure services (klassiek) beheren

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over hoe u [deze stappen uitvoert met behulp van het model Resource Manager](dns-reverse-dns-record-operations-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Validatie van reverse DNS-records
Azure kunt dat reverse DNS-records toe te wijzen aan uw DNS-domeinen kan niet worden gemaakt door een derde partij, zodat alleen het maken van een omgekeerde DNS-record waarvoor het volgende geldt:

- Het omgekeerde DNS FQDN is de naam van de Cloud-Service die is ingesteld of een willekeurige naam Cloud Service binnen het abonnement hetzelfde BV, reverse DNS "contosoapp1.cloudapp.net.".
- Reverse DNS FQDN vooruit wordt omgezet in de naam of het IP van de Cloud-Service waarbij dit is opgegeven of in een Cloud Service-naam of IP-binnen het abonnement hetzelfde b.v. reverse DNS is 'app1.contoso.com'. Wat is een CNAME-alias voor contosoapp1.cloudapp.net.

Validatiecontroles worden alleen uitgevoerd als de reverse DNS-eigenschap voor een Cloud-Service wordt ingesteld of gewijzigd. Periodieke wordt opnieuw niet gevalideerd.

## <a name="add-reverse-dns-to-existing-cloud-services"></a>Reverse DNS toevoegen aan bestaande Cloud Services
U kunt een omgekeerde DNS-record toevoegen aan een bestaande Cloud-Service met de cmdlet "Set-AzureService":

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>Een Cloud-Service maken met reverse DNS
U kunt een nieuwe wolk Service toevoegen met de omgekeerde DNS-eigenschap die is opgegeven met de cmdlet "Set-AzureService":

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>Reverse DNS voor bestaande Cloud Services weergeven
U kunt de geconfigureerde waarde weergeven voor een bestaande Cloud-Service met de cmdlet 'Get-AzureService':

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>Reverse DNS verwijderen uit bestaande Cloud-Services
U kunt een reverse DNS-eigenschap verwijderen uit een bestaande Cloud-Service met de cmdlet "Set-AzureService". Dit wordt gedaan door de reverse DNS-eigenschapswaarde leeg:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]
