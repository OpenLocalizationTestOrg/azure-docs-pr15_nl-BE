<properties
   pageTitle="Azure opslag-overzicht | Microsoft Azure"
   description=" Azure opslag is de cloud storage oplossing voor moderne toepassingen die afhankelijk zijn van duurzaamheid, beschikbaarheid en schaalbaarheid om te voldoen aan de behoeften van hun klanten. Dit artikel bevat een overzicht van de belangrijkste Azure beveiligingsfuncties die met Azure opslag kunnen worden gebruikt. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/16/2016"
   ms.author="terrylan"/>

# <a name="azure-storage-security-overview"></a>Azure opslag-overzicht

Azure opslag is de cloud storage oplossing voor moderne toepassingen die afhankelijk zijn van duurzaamheid, beschikbaarheid en schaalbaarheid om te voldoen aan de behoeften van hun klanten. Azure opslag beschikt over een uitgebreide set beveiligingsfuncties:

- De account van de opslag kan worden beveiligd met behulp van toegangsbeheer op basis van rollen en Azure Active Directory.
- Gegevens kunnen worden beveiligd tijdens de overdracht tussen een toepassing en Azure met behulp van Client-Side codering, HTTPS of SMB 3.0.
- Gegevens kan worden ingesteld op automatisch worden gecodeerd als geschreven naar Azure opslag opslag Service codering gebruiken.
- OS en Data schijven die worden gebruikt door virtuele machines kunnen worden versleuteld met Azure schijf codering worden ingesteld.
- Gedelegeerde toegang tot de objecten data in Azure opslag kan worden verleend met gedeelde toegang handtekeningen.
- De verificatiemethode die door iemand wordt gebruikt wanneer zij toegang hebben tot opslag kan worden bijgehouden met behulp van analytics voor opslag.

Zie voor een meer gedetailleerde beschrijving beveiliging in Azure, opslag, [opslag Azure security guide](../storage/storage-security-guide.md). Deze handleiding biedt een diepgaande Kennismaking in de beveiligingsfuncties van Azure opslag zoals opslag account sleutels, codering in transit en op de rest en analytics voor opslag van gegevens.

Dit artikel biedt een overzicht van Azure beveiligingsfuncties die met Azure opslag kunnen worden gebruikt. Koppelingen zijn naar artikelen waarin de details van elke functie zodat geven u meer.

Hier vindt u de kernfuncties zullen worden behandeld in dit artikel:

- Role-Based Access Control
- Gedelegeerde toegang tot objecten voor gegevensopslag
- Codering in transit
- Codering op rest/Storage Service-codering
- Azure schijfversleuteling
- Azure sleutel kluis

## <a name="role-based-access-control-rbac"></a>Role-Based Access Control RBAC)

U kunt uw account opslag met op rollen gebaseerde toegangscontrole RBAC () beveiligen. Beperken van toegang op basis van de beginselen van de beveiliging [moet weten](https://en.wikipedia.org/wiki/Need_to_know) en [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) is van cruciaal belang voor organisaties die willen af te dwingen voor toegang tot gegevens. Deze toegangsrechten worden verleend door de juiste RBAC-rol toekennen aan groepen en toepassingen op een bepaalde scope. [Ingebouwde RBAC-rollen](../active-directory/role-based-access-built-in-roles.md), zoals opslag Account Inzender, kunt u machtigingen toewijzen aan gebruikers.

Meer informatie:

- [Azure Active Directory toegang op basis van rollen beheren](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-to-storage-objects"></a>Gedelegeerde toegang tot objecten voor gegevensopslag

Een gedeelde toegang handtekening (SAS) biedt een gedelegeerde toegang tot bronnen in uw account voor opslag. De SA's betekent dat u een client beperkte machtigingen op objecten in uw account opslag gedurende een bepaalde tijd en met een opgegeven reeks machtigingen kunt verlenen. U kunt deze beperkte machtigingen verlenen zonder de toegangstoetsen account delen. De SAS is een URI die in de query-parameters alle gegevens omvat die nodig zijn voor geverifieerde toegang aan een resource opslag. Als u opslagbronnen met de SAS, hoeft de client alleen in de SA's doorgeven aan de methode of de juiste constructor.

Meer informatie:

- [Wat is het model SAS?](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Maken en gebruiken van een SAS met Blob-opslag](../storage/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Codering in transit
Codering in transit is een mechanisme om gegevens te beschermen wanneer deze worden verzonden via netwerken. U kunt beveiligen met behulp van gegevens met Azure opslag:

- [Codering transportniveau](../storage/storage-security-guide.md#encryption-in-transit), zoals HTTPS, wanneer u gegevens van of naar Azure opslag overbrengt.
- [Draad codering](../storage/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), zoals codering voor bestandsshares Azure SMB 3.0.
- [Codering van client-side](../storage/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), gegevens te coderen voordat deze wordt overgebracht op te slaan en te decoderen van de gegevens na het overbrengen van opslag.

Meer informatie over de client-side-codering:

- [Client-Side-codering voor de opslag van Microsoft Azure](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
- [Cloud beveiliging reeks besturingselementen: gegevens coderen in Transit](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Codering in rust

[Codering van gegevens in rust](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) is voor veel organisaties een verplichte stap in de richting van, gegevensprivacy, compliance en gegevens-soevereiniteit. Er zijn drie Azure functies voor het coderen van gegevens 'in rust':

- [Opslag Service codering](../storage/storage-security-guide.md#encryption-at-rest) kunt u vragen dat de storage-service automatisch coderen van gegevens bij het schrijven naar Azure opslag.
- [Client-side codering](../storage/storage-security-guide.md#client-side-encryption) biedt ook de functie van de codering in rust.
- [Azure schijf codering](../storage/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) kunt u voor het coderen van de OS-schijven en gegevensschijven door een IaaS virtuele machine gebruikt.

Meer informatie over codering van Storage-Service:

- [Codering van Azure Storage Service](https://azure.microsoft.com/services/storage/) is beschikbaar voor [Azure Blob-opslag](https://azure.microsoft.com/services/storage/blobs/). Zie voor meer informatie over de andere opslagtypen Azure [bestand](https://azure.microsoft.com/services/storage/files/), [schijf (opslag Premium)](https://azure.microsoft.com/services/storage/premium-storage/), [tabel](https://azure.microsoft.com/services/storage/tables/)en [wachtrij](https://azure.microsoft.com/services/storage/queues/).
- [Azure Storage Service-codering voor gegevens in rust](../storage/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure schijfversleuteling

Azure schijf codering voor virtuele machines (VMs) helpt u organisatorische beveiliging en naleving van voorschriften door de VM-schijven (met inbegrip van de opstart- en schijven) met sleutels en het beleid voor die u in [Azure sleutel kluis regelen](https://azure.microsoft.com/services/key-vault/)te coderen.

Schijfversleuteling voor VMs werkt voor Linux en Windows-besturingssystemen. Sleutel kluis wordt gebruikt om te helpen beveiligen, beheren en het gebruik van uw coderingssleutels schijf controleren. De gegevens in de VM-schijven in rust gecodeerd met behulp van industriestandaard coderingstechnologie in uw rekeningen Azure opslag. De codering van de schijf-oplossing voor Windows [BitLocker Drive Encryption](https://technet.microsoft.com/library/cc732774.aspx)is gebaseerd en Linux-oplossing is gebaseerd op [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Meer informatie:

- [Azure schijf codering voor Windows en Linux IaaS virtuele Machines](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure sleutel kluis

[Azure sleutel kluis](https://azure.microsoft.com/services/key-vault/) Azure schijf codering gebruikt voor het controleren en beheren van coderingssleutels schijf en geheimen in je abonnement sleutel kluis, terwijl ervoor te zorgen dat alle gegevens in de virtuele machine-schijven in rust in Azure opslag worden gecodeerd. Sleutel kluis moet u controleren en het beleid gebruik van sleutels.

Meer informatie:

- [Wat is Azure sleutel kluis?](../key-vault/key-vault-whatis.md)
- [Aan de slag met Azure sleutel kluis](../key-vault/key-vault-get-started.md)
