
<properties
    pageTitle="Azure consistent opslag: verschillen en overwegingen | Microsoft Azure"
    description="Het verschil van Azure opslag en andere overwegingen voor implementatie van Azure consistent opslag."
    services="azure-stack"
    documentationCenter=""
    authors="MChadalapaka"
    manager="siroy"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="mchad"/>

# <a name="azure-consistent-storage-differences-and-considerations"></a>Azure consistent opslag: verschillen en overwegingen

Azure consistent opslag is de set van de storage cloud-services in Microsoft Azure stapel. Azure consistent opslag biedt blob, tabel, wachtrij en account management functionaliteit met Azure consistent semantiek. In dit artikel worden de bekende Azure consistent opslag verschillen met Azure opslag. Ook ziet u andere overwegingen rekening moet houden wanneer u de momenteel openbare preview-versie van Microsoft Azure Stack implementeren.

<span id="Concepts" class="anchor"><span id="_Toc386544169" class="anchor"><span id="_Toc389466742" class="anchor"><span id="_Ref428966996" class="anchor"><span id="_Toc433223853" class="anchor"></span></span></span></span></span>
## <a name="known-differences"></a>Bekende verschillen

Deze technische voorbeeldversie van Azure consistent opslag heeft geen 100% functie pariteit met Azure opslag voor de API-versies die worden ondersteund. Verschillen tussen functies omvatten het volgende bekend:

-   Bepaalde opslagtypen nog niet beschikbaar zijn, bijvoorbeeld standaard\_RAGRS en standaard\_GRS.

-   Premium\_LRS opslag rekeningen kunnen worden ingericht. Echter, er zijn momenteel geen prestatielimieten of garanties.

-   Azure bestanden functionaliteit is nog niet beschikbaar.

-   De API krijgen pagina bereiken biedt geen ondersteuning voor het ophalen van pagina's die tussen momentopnamen van BLOB's pagina verschillen.

-   De API krijgen pagina bereiken wordt-pagina's met 4 KB specifieker.

-   Partitie en rij-sleutel in de Azure consistente implementatie van opslag tabel zijn elk maximaal 400 tekens (dat wil zeggen 800 bytes).

-   BLOB-namen in de Azure consistent opslag Blob-implementatie is beperkt tot 880 tekens (dat wil zeggen, 1760 bytes).

-   De Azure consistent opslag implementatie van huurder opslag gebruik gegevensrapportage biedt opslag gebruik meters die identiek aan die welke in Azure, met dezelfde semantiek en eenheden zijn. Op dit moment echter de meter opslag transacties gebruik bevat geen IaaS transacties en de overdracht van gegevens gebruik meter geen onderscheid maken het bandbreedtegebruik door interne of externe netwerkverkeer naar een gebied Azure Stack.

-   Bepaalde verschillen bestaan in het bereik van de functionaliteit voor beheer van opslag. U kan bijvoorbeeld het accounttype wijzigen of aangepaste domeinen. Bovendien alleen consistentie van API-niveau wordt aangeboden voor de premie\_LRS opslag rekeningsoort.

## <a name="deployment-considerations"></a>Aandachtspunten bij de implementatie

-   **Test.** Implementeer niet consistent met Azure opslag in productieomgevingen die gebruikmaken van de huidige Microsoft Azure Stack technische Preview-versie. Deze versie is alleen voor evaluatiedoeleinden in een testomgeving bedoeld.

-   **Prestaties**. De Microsoft Azure Stack technische voorbeeldversie van Azure consistent opslag is niet volledig door de prestaties geoptimaliseerd.

-   **Schaalbaarheid.** De Microsoft Azure Stack technische voorbeeldversie van Azure consistent opslag is niet geoptimaliseerd voor schaalbaarheid.

## <a name="version-support-considerations"></a>Overwegingen voor ondersteuning van versie

De volgende versies worden ondersteund met deze preview-versie van Azure consistent opslag:

> Client-bibliotheek van Azure opslag: [Microsoft Azure opslag 6.x .NET SDK](http://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)
>
> Azure opslag van gegevens: [2015-04-05 REST API versie](https://msdn.microsoft.com/library/azure/mt705637.aspx)
>
> Azure Storage management-services: [2015-05-01-preview](https://msdn.microsoft.com/library/azure/mt163683.aspx)
> [2015-06-15](https://msdn.microsoft.com/library/azure/mt163683.aspx)
## <a name="next-steps"></a>Volgende stappen

-   [Inleiding tot Azure consistent opslag](azure-stack-storage-overview.md)
