<properties
    pageTitle="Azure Storage Service-codering voor gegevens in rust | Microsoft Azure"
    description="De coderingsfunctie Azure Storage-Service gebruiken voor het coderen van uw Azure Blob-opslag aan de kant van de service bij het opslaan van de gegevens en bij het ophalen van de gegevens worden gedecodeerd."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="robinsh"/>

# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage Service-codering voor gegevens in rust

Azure Storage Service codering (SSE) voor gegevens in rust helpt u te beschermen en beveiligen van uw gegevens om te voldoen aan de beveiliging van de organisatie en de naleving van verbintenissen. Met deze functie Azure opslag automatisch codeert de gegevens voor het persistent maken voor opslag en decodeert vóór ophalen. Het coderen, decoderen en sleutelbeheer zijn volledig transparant voor de gebruikers.

De volgende secties vindt gedetailleerde instructies over het gebruik van de versleuteling van de opslag-functies en de ondersteunde scenario's en gebruiker ervaringen.

## <a name="overview"></a>Overzicht

Azure opslag biedt een uitgebreide reeks beveiligingsmogelijkheden die samen kunnen ontwikkelaars toepassingen bouwen. Gegevens kunnen worden beveiligd tijdens de overdracht tussen een toepassing en Azure met behulp van [Client-Side codering](storage-client-side-encryption.md), HTTPs of SMB 3.0. Opslag Service codering biedt codering in rust, behandelingen codering decoderen en Sleutelbeheer op een volledig transparante wijze. Alle gegevens worden gecodeerd met 256-bits [AES-codering](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de sterkste blokcodering beschikbaar.

SSE werkt door de gegevens te coderen wanneer wordt geschreven naar Azure opslag, en kan worden gebruikt voor blok BLOB's, pagina BLOB's en BLOB's toevoegen. Het werkt voor de volgende opties:

-   Algemene opslag Blob storage rekeningen en
-   Standaardopslag en Premium-opslag 
-   Redundantie op alle niveaus (LRS, ZRS, GRS, RA GRS)
-   Azure Resource Manager opslag rekeningen (maar niet klassiek) 
-   Alle regio 's

Log in op de [Azure portal](https://azure.portal.com) of opslag Service codering uitschakelen voor een account voor de opslag, en selecteer een opslag. De blade instellingen zoeken naar de sectie Blob-Service, zoals in dit screenshot en klik op codering.

![De optie codering Portal schermafbeelding weergeven](./media/storage-service-encryption/image1.png)

Nadat u de instelling van de codering op, kunt u of opslag Service codering uitschakelen.

![Coderingseigenschappen Portal schermafbeelding weergeven](./media/storage-service-encryption/image2.png)

##<a name="encryption-scenarios"></a>Scenario's voor codering

Opslag Service codering kan worden ingeschakeld op het niveau van een opslag. Het ondersteunt de volgende scenario's van klant:

-   Codering van het blok BLOB's, toevoegquery BLOB's en pagina-BLOB's.

-   Codering van gearchiveerde VHD's en sjablonen op ruimten op Azure gebracht.

-   Codering van de onderliggende schijven van OS en gegevens voor IaaS VMs gemaakt met behulp van de VHD.

SSE heeft de volgende beperkingen:

-   Codering van klassieke opslag rekeningen wordt niet ondersteund.

-   Codering van klassieke opslag accounts naar Resource Manager opslag accounts migreren wordt niet ondersteund.

-   Bestaande gegevens - SSE zijn nieuwe gegevens alleen gecodeerd nadat de codering is ingeschakeld. Als bijvoorbeeld een nieuwe Resource Manager opslag account maken maar geen versleuteling inschakelen en vervolgens het BLOB's of gearchiveerde VHD's uploaden naar opslag account en schakel SSE, wordt deze BLOB's niet versleuteld tenzij ze worden herschreven of gekopieerd.

-   Ondersteuning voor Marketplace - codering inschakelen van VMs gemaakt op basis van de markt met behulp van de [portal Azure](https://portal.azure.com), PowerShell en Azure CLI. De VHD afbeelding blijft ongecodeerd; echter worden alle gedaan nadat de VM is gesponnen van schrijfbewerkingen gecodeerd.

-   Tabel, wachtrijen, en bestanden worden de gegevens niet gecodeerd.

##<a name="getting-started"></a>Aan de slag

###<a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Stap 1: [een nieuwe opslag-account maken](storage-create-storage-account.md).

###<a name="step-2-enable-encryption"></a>Stap 2: Schakel codering.

U kunt met behulp van de [portal Azure](https://portal.azure.com)codering inschakelen.

> [AZURE.NOTE] Als u via programmacode of uit te schakelen de codering Storage Service op de account van een opslag wilt, kunt u de [REST API van Azure opslag Resource Provider](https://msdn.microsoft.com/library/azure/mt163683.aspx), de [Opslag voorziening Client Bronbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](../powershell-install-configure.md)of de [CLI Azure](storage-azure-cli.md).

###<a name="step-3-copy-data-to-storage-account"></a>Stap 3: Gegevens kopiëren naar de account van de opslag

Als u SSE op een opslag-account inschakelen en vervolgens BLOB's aan de account van de opslag schrijven, worden de BLOB's gecodeerd. Een BLOB's al in die account opslag bevinden niet gecodeerd totdat ze worden herschreven. U kunt de gegevens van één opslag account kopiëren naar met SSE gecodeerd, of zelfs SSE- en de BLOB's naar de andere kopiëren uit een container te zorgen dat bestaande gegevens worden gecodeerd. U kunt een van de volgende hulpprogramma's om dit te bereiken.

#### <a name="using-azcopy"></a>Met behulp van AzCopy

AzCopy is een opdrachtregelprogramma van Windows ontworpen voor het kopiëren van gegevens van en naar Microsoft Azure Blob-bestand en tabel opslag met behulp van eenvoudige opdrachten met optimale prestaties. U kunt hiermee uw BLOB's kopiëren van de ene account voor opslag naar een andere die SSE ingeschakeld is. 

Voor meer informatie, Ga naar [gegevensoverdracht met AzCopy Command-Line Utility](storage-use-azcopy.md).

#### <a name="using-the-storage-client-libraries"></a>Met behulp van de opslag Client Libraries

Blob-gegevens kunt u kopiëren naar en vanuit blob-opslag of tussen opslag rekeningen met behulp van onze uitgebreide reeks clientbibliotheken opslag met inbegrip van .NET, C++, Java, Android, Node.js, PHP, Python en Ruby.

Voor meer informatie, Ga naar onze [aan de slag met Azure Blob-opslag met behulp van .NET](storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Met behulp van een Opslagverkenner

U kunt een Opslagverkenner opslag accounts maken, uploaden en downloaden van gegevens, inhoud van BLOB's weergeven en navigeren door mappen. U kunt een van deze BLOB's uploaden naar je account opslag met codering is ingeschakeld. Met sommige explorers opslag kunt u ook gegevens uit bestaande blob-opslag kopiëren naar een andere container in de opslag-account of een nieuwe account voor opslag met SSE ingeschakeld.

Voor meer informatie, Ga naar [Azure opslag Explorers](storage-explorers.md).

###<a name="step-4-query-the-status-of-the-encrypted-data"></a>Stap 4: De status van de gecodeerde gegevens opvragen

Een bijgewerkte versie van de opslag clientbibliotheken er is geïmplementeerd waarin u de status van een object om te bepalen kunt als het is gecodeerd of niet. Voorbeelden zal in de nabije toekomst aan dit document worden toegevoegd.

In de tussentijd kunt u de [Eigenschappen opvragen](https://msdn.microsoft.com/library/azure/mt163553.aspx) om te controleren of de account opslag heeft codering ingeschakeld of weergeven van de eigenschappen van de opslag in de Azure portal aanroepen.

##<a name="encryption-and-decryption-workflow"></a>Codering en decodering Workflow

Hier volgt een korte beschrijving van de werkstroom codering/decodering:

-   De klant kan de codering op de rekening van de opslag.

-   Wanneer de klant worden geschreven nieuwe gegevens (Blob plaatsen, plaatsen blok, pagina plaatsen, enz.) naar een Blob-opslag; elke schrijven, is gecodeerd met 256-bits [AES-codering](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de sterkste blokcodering beschikbaar.

-   Wanneer de klant moet toegang tot gegevens (Blob ophalen, enz.), is voordat u terugkeert naar de gebruiker gegevens automatisch gedecodeerd.

-   Als codering is uitgeschakeld, worden nieuwe schrijfbewerkingen zijn niet langer gecodeerd en bestaande gecodeerde gegevens gecodeerd blijven totdat herschreven door de gebruiker. Terwijl de codering is ingeschakeld, worden weggeschreven naar een Blob-opslag worden gecodeerd. De status van de gegevens wijzigen niet met de gebruiker schakelt tussen de codering voor de opslag account inschakelen/uitschakelen.

-   Alle sleutels worden opgeslagen, versleuteld en beheerd door Microsoft.

##<a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Veelgestelde vragen over opslag Service codering voor gegevens in rust

**V: ik heb een bestaande account voor klassieke opslag. Kan ik erop SSE inschakelen?**

A: Nee, SSE wordt alleen ondersteund op Resource Manager opslag rekeningen.

**V: hoe kan ik gegevens in Mijn account klassieke opslag coderen?**

A: u kunt een nieuwe Resource Manager opslag account en uw [AzCopy](storage-use-azcopy.md) van uw bestaande account van klassieke opslag met de nieuwe Resource Manager opslag account gegevens kopiëren. 

Een andere mogelijkheid is je account klassieke storage migreren naar een storage Resource beheren. Zie de [Platform ondersteund migratie van IaaS bronnen van klassiek tot Resource Manager](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/)voor meer informatie.

**V: ik heb een bestaande account van de Resource Manager-opslag. Kan ik erop SSE inschakelen?**

A: Ja, maar alleen nieuwe BLOB's geschreven worden gecodeerd. Deze niet terug en coderen van gegevens die al aanwezig was. 

**V: ik wilt coderen van de huidige gegevens in een bestaande Resource Manager opslag account?**

A: u kunt SSE op elk gewenst moment op de rekening van een Resource Manager opslag inschakelen. BLOB's die al aanwezig waren zal echter niet worden gecodeerd. Voor het coderen van de BLOB's kunt u deze kopiëren naar een andere naam of een andere container en verwijder de niet-gecodeerde versies.

**V: ik gebruik Premium opslag; kan ik de SSE gebruiken?**

A: Ja, SSE op Premium opslag en standaardopslag wordt ondersteund.

**Q: als ik een nieuwe account voor opslag en SSE inschakelen en maak vervolgens een nieuwe VM opslag account gebruikt, betekent dit dat mijn VM is gecodeerd?**

A: Ja. Alle schijven die zijn gemaakt met de nieuwe account voor de opslag worden gecodeerd als ze worden gemaakt nadat de SSE is ingeschakeld. Als de VM is gemaakt met behulp van Azure marktplaats, blijft de VHD afbeelding ongecodeerd; echter worden alle gedaan nadat de VM is gesponnen van schrijfbewerkingen gecodeerd.

**V: kan ik nieuwe opslag-accounts maken met SSE ingeschakeld met behulp van Azure PowerShell en Azure CLI?**

A: Ja.

**V: hoe veel meer kost Azure opslag als SSE is ingeschakeld?**

A: Er is geen extra kosten.

**V: wie beheert de coderingssleutels?**

A: de sleutels worden beheerd door Microsoft.

**V: kan ik mijn eigen coderingssleutels gebruiken?**

A: Wij werken bieden mogelijkheden voor klanten om hun eigen coderingssleutels.

**V: kan ik toegang tot de coderingssleutels intrekken?**

A: niet op dit moment; de sleutels worden volledig beheerd door Microsoft.

**V: is SSE standaard ingeschakeld wanneer ik een nieuwe opslag-account maken?**

A: SSE is geen optie standaard ingeschakeld. de Azure portal kunt u deze inschakelen. Ook kunt u deze functie met de opslag Resource Provider REST API.

**V: hoe is dit anders dan Azure-stationsversleuteling?**

A: deze functie wordt gebruikt voor het coderen van gegevens in Azure Blob-opslag. De Azure schijfversleuteling wordt gebruikt voor het coderen van gegevens en OS schijven in IaaS VMs. Ga naar onze [Opslag Security Guide](storage-security-guide.md)voor meer informatie.

**V: Wat gebeurt er als ik SSE, inschakelen en vervolgens in te gaan en Azure schijf codering inschakelen op de schijven?**

A: dit werkt probleemloos. Uw gegevens worden gecodeerd door beide methoden.

**V: Mijn account opslag is ingesteld voor geo-redundante worden gerepliceerd. Als ik de SSE, worden mijn redundante exemplaar ook gecodeerd?**

A: Ja, alle kopieën van de rekening van de opslag zijn gecodeerd en alle redundantieopties voor-Zone redundante opslag (ZRS) lokaal redundante opslag (LRS) Geo-redundante opslag (GRS) en leestoegang Geo-redundante opslag (RA-GRS)-worden ondersteund.

**V: ik kan geen codering inschakelen voor mijn account opslag.**

A: is het een bronnenbeheerder opslag account? Klassieke opslag accounts worden niet ondersteund. 

**V: SSE mag slechts in bepaalde gebieden?**

A: de SSE is beschikbaar in alle regio's. 

**V: hoe ik contact op met iemand als ik problemen heb of feedback wilt geven?**

A: Neem contact op met [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com) voor eventuele problemen met betrekking tot opslag Service codering.

##<a name="next-steps"></a>Volgende stappen

Azure opslag biedt een uitgebreide reeks beveiligingsmogelijkheden die samen kunnen ontwikkelaars toepassingen bouwen. Ga naar de [Opslag Security Guide](storage-security-guide.md)voor meer informatie.
