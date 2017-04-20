<properties
   pageTitle="Vergelijking met Azure opslag Blob Azure Lake gegevensarchief | Microsoft Azure"
   description="Vergelijking met Azure opslag Blob Azure Lake gegevensarchief"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/15/2016"
   ms.author="nitinme"/>

# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Vergeleken met Azure Lake gegevensarchief Azure Blob-opslag

De tabel in dit artikel wordt een overzicht gegeven van de verschillen tussen Azure Lake gegevensopslag en Azure Blob-opslag langs enkele belangrijke aspecten van gegevensverwerking groot. Azure Blob-opslag is een algemeen doel, schaalbare opslaglocatie die is ontworpen voor een groot aantal scenario's voor opslag. Azure Lake gegevensarchief is een hyperscale '-opslagplaats die is geoptimaliseerd voor big data analytics werkbelasting.

|    | Azure Lake gegevensarchief  | Azure Blob-opslag  |
|----|-----------------------|--------------------|
| Doel  | Geoptimaliseerde opslag voor big data analytics werkbelasting                                                                          | Algemene object opslaan voor een groot aantal scenario's voor opslag                                                                                |
| Use-Cases                                   | Batch, interactieve, analytics en machine learning data zoals streaming logboekbestanden, IoT-gegevens, klikt u op stromen, grote gegevenssets | Elk type tekst of binaire gegevens, zoals de toepassing back-end, back-ups, Mediaopslag voor streaming en algemeen doel gegevens |
| Basisbegrippen                                | Account Lake gegevensarchief bevat mappen, die op zijn beurt bevat gegevens die zijn opgeslagen als bestanden | Opslag-account heeft in containers, die op zijn beurt de gegevens in de vorm van BLOB's heeft |
| Structuur | Hiërarchisch bestandssysteem                                                                                                    | Objectarchief met platte naamruimte  |
| API   | REST API via HTTPS | REST via HTTP/HTTPS-API                                                                                                                            |
| Server-side-API                             | [WebHDFS-compatibele REST API](https://msdn.microsoft.com/library/azure/mt693424.aspx)                                                                                                 | [Azure Blob-opslag REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx)                                                                                                                         |
| Hadoop bestand systeem Client                   | Ja                                                                                                                         | Ja                                                                                                                                                 |
| Gegevensbewerkingen - verificatie            | Op basis van [Azure Active Directory identiteiten](../active-directory/active-directory-authentication-scenarios.md) | Op basis van gedeelde geheimen - [Account toegang](../storage/storage-create-storage-account.md#manage-your-storage-account) en [Gedeelde toegang handtekeningsleutels](../storage/storage-dotnet-shared-access-signature-part-1.md).                                                                       |
| Gegevensbewerkingen - verificatieprotocol     | OAuth 2.0. Gesprekken moeten een geldige JWT (JSON Web Token) uitgegeven door Azure Active Directory bevatten                     | Hash-based Message Authentication Code (HMAC). Gesprekken bevat over een deel van de HTTP-aanvraag een Base64-gecodeerd SHA-256 hash. |
| Gegevensbewerkingen - vergunning               | POSIX toegangsbeheerlijsten (ACL's).  ACL's op basis van Azure Active Directory identiteiten kunnen niveau voor bestanden en mappen instellen. | Voor een vergunning niveau – gebruik [Account Access-toetsen](../storage/storage-create-storage-account.md#manage-your-storage-account)<br>Voor de account, container of blob autorisatie - gebruiken [Toegangstoetsen handtekening gedeeld](../storage/storage-dotnet-shared-access-signature-part-1.md) |
| Gegevensbewerkingen - controle                    | Beschikbaar. Zie [hier](data-lake-store-diagnostic-logs.md) voor meer informatie.                                                                                                                   | Beschikbaar                                                                                                                                           |
| Codering van gegevens in rust                     | Transparant, Server side (binnenkort beschikbaar)<ul><li>Met managed service sleutels</li><li>Met de klant beheerde sleutels in Azure, KeyVault</li></ul>| <ul><li>Transparant, Server side</li> <ul><li>Met managed service sleutels</li><li>Met de klant beheerde sleutels in Azure KeyVault (binnenkort)</li></ul><li>Client-side-codering</li></ul> |
| Beheertaken uit te voeren (bijvoorbeeld Account maken) | [Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md) (RBAC) wordt geleverd door Azure voor accountbeheer                                                                       | [Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md) (RBAC) wordt geleverd door Azure voor accountbeheer                                                                                               |
| Developer SDK 's                              | .NET, Java, Node.js                                                                                                         | .NET, Java, Python, Node.js, C++, Ruby                                                                                                              |
| Analytics werkbelasting prestaties              | Geoptimaliseerde prestaties voor parallelle analytics werkbelasting. Hoge doorvoer en IOP's.                                           | Niet is geoptimaliseerd voor analytics werkbelasting                                                                                                               |
| Maximale grootte                                 | Geen limieten op account formaat, grootte of het aantal bestanden                                                                   | Specifieke limieten gedocumenteerd [hier](../azure-subscription-service-limits.md#storage-limits)                                                                                                                     |
| Geo-redundantie                              | Lokaal-redundante (meerdere kopieën van de gegevens in een Azure regio)                                                             | Lokaal redundante (LRS), globaal redundante (GRS), leestoegang tot globaal redundante (RA-GRS). Zie [hier](../storage/storage-redundancy.md) voor meer informatie |
| De status van service                               | Public Preview                                                                                                              | In het algemeen beschikbaar                                                                                                                                 |
| Beschikbaarheid van regionale  | Zie [hier](https://azure.microsoft.com/regions/#services)| Zie [hier](https://azure.microsoft.com/regions/#services) |
| Prijs                                       |     Zie [prijzen](https://azure.microsoft.com/pricing/details/data-lake-store/)| Zie [prijzen](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure Lake gegevensarchief](data-lake-store-overview.md)
- [Aan de slag met Lake gegevensarchief](data-lake-store-get-started-portal.md)



