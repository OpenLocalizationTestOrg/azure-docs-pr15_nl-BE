<properties
pageTitle="Het bijwerken van een cloud service | Microsoft Azure"
description="Informatie over het bijwerken van cloud-services in Azure. Informatie over hoe een update op een cloud-service wordt uitgevoerd om beschikbaarheid te garanderen."
services="cloud-services"
documentationCenter=""
authors="Thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/10/2016"
ms.author="adegeo"/>

# <a name="how-to-update-a-cloud-service"></a>Het bijwerken van een cloud-service

## <a name="overview"></a>Overzicht
Op 10.000 voet is bijwerken van een cloud-service, inclusief de functies en de Gast OS een proces van drie. Ten eerste moeten de configuratiebestanden voor de nieuwe wolk service of de versie van het besturingssysteem en de binaire bestanden worden geüpload. Vervolgens reserveert Azure compute- en netwerkbronnen voor de cloud-service op basis van de behoeften van de nieuwe versie van de cloud-service. Ten slotte voert Azure een rolling upgrade stapsgewijs de huurder bijwerken naar de nieuwe versie of een gastbesturingssysteem, behoud van de beschikbaarheid. In dit artikel worden de details van deze laatste stap – de rolling upgrade beschreven.

## <a name="update-an-azure-service"></a>Een Azure Service bijwerken

Azure organiseert de exemplaren van uw rol in logische groepen upgraden domeinen (UD) genoemd. Upgraden van domeinen (UD) zijn logische rol exemplaren die als groep worden bijgewerkt.  Azure updates een cloud-service een UD tegelijk, waardoor de instanties in andere UDs te gaan voor verkeer.

Het aantal domeinen upgraden is 5. U kunt een verschillend aantal domeinen upgraden door het kenmerk upgradeDomainCount in het bestand van de service (.csdef). Zie voor meer informatie over het kenmerk upgradeDomainCount [WebRole Schema](https://msdn.microsoft.com/library/azure/gg557553.aspx) of [Schema voor WorkerRole](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Wanneer u een update in plaats van een of meer rollen in uw service uitvoert, werkt de Azure sets exemplaren rol volgens de upgrade domein waartoe ze behoren. Azure updates alle exemplaren in een upgrade gegeven domein – stoppen, bijgewerkt, waardoor ze on line-back wordt verplaatst naar het volgende domein. Door alleen de exemplaren die worden uitgevoerd in het huidige domein bijwerken stoppen, zorgt Azure een update plaatsvindt met de minste mogelijke impact op de actieve service. Zie [hoe de update gaat](#howanupgradeproceeds) verderop in dit artikel voor meer informatie.

> [AZURE.NOTE] Voorwaarden **update** en **upgrade** moet iets andere betekenis in de context van Azure, kunnen zij door elkaar worden gebruikt voor processen en beschrijvingen van de functies in dit document.

De service moet ten minste twee exemplaren van een rol voor die rol worden bijgewerkt definiëren ter plekke zonder uitvaltijd. Als de service uit slechts één exemplaar van een rol bestaat, de service is niet beschikbaar totdat de interne-update is voltooid.

Dit onderwerp omvat de volgende informatie over Azure updates:

-   [Wijzigingen in service toegestaan tijdens een update](#AllowedChanges)
-   [Hoe een upgrade wordt uitgevoerd](#howanupgradeproceeds)
-   [Terugdraaien van een update](#RollbackofanUpdate)
-   [Meerdere mutating bewerkingen op de implementatie van een voortdurend starten](#multiplemutatingoperations)
-   [Verdeling van de rollen voor upgraden domeinen](#distributiondfroles)

<a name="AllowedChanges"></a>
## <a name="allowed-service-changes-during-an-update"></a>Wijzigingen in service toegestaan tijdens een update
De volgende tabel ziet u de toegestane wijzigingen in een service tijdens een update:

|Wijzigingen toegestaan hosting services en functies|In-place update|Gefaseerde (VIP swap)|Verwijderen en opnieuw installeren|
|---|---|---|---|
|Besturingssysteemversie|Ja|Ja|Ja
|.NET vertrouwensniveau|Ja|Ja|Ja|
|Virtuele machine grootte<sup>1</sup>|Ja,<sup>2</sup>|Ja|Ja|
|Instellingen voor lokale opslag|Alleen<sup>2</sup> verhogen|Ja|Ja|
|Toevoegen of verwijderen van rollen in een service|Ja|Ja|Ja|
|Aantal exemplaren van een bepaalde functie|Ja|Ja|Ja|
|Aantal of type eindpunten voor een service|Ja,<sup>2</sup>|Nee|Ja|
|Namen en waarden van de configuratie-instellingen|Ja|Ja|Ja|
|De waarden (maar niet de naam) van de configuratie-instellingen|Ja|Ja|Ja|
|Nieuwe certificaten toevoegen|Ja|Ja|Ja|
|Bestaande certificaten wijzigen|Ja|Ja|Ja|
|Nieuwe code-implementatie|Ja|Ja|Ja|
<sup>1</sup> Grootte wijzigen is beperkt tot de subset van de papierformaten die beschikbaar zijn voor de service cloud.

<sup>2</sup> Azure SDK 1.5 of hoger vereist.

> [AZURE.WARNING] Wijzigen van de grootte van de virtuele machine worden lokale gegevens vernietigd.


De volgende items worden niet ondersteund tijdens een update:

-   De naam van een rol wijzigen. Verwijderen en vervolgens de functie met de nieuwe naam toevoegen.
-   Wijzigen van de telling van het domein upgraden.
-   Verkleinen van de lokale resources.

Als u andere updates aan de definitie van uw service, zoals het verkleinen van de lokale resource moet u een VIP swap-update uitvoeren. Zie [Verwisselen implementatie](https://msdn.microsoft.com/library/azure/ee460814.aspx)voor meer informatie.

<a name="howanupgradeproceeds"></a>
## <a name="how-an-upgrade-proceeds"></a>Hoe een upgrade wordt uitgevoerd
U kunt beslissen of u wilt bijwerken op alle rollen van de in de service of een enkele rol in de service. In beide gevallen worden alle exemplaren van elke rol die wordt bijgewerkt en die deel uitmaken van het eerste domein bijwerken gestopt, bijgewerkt en opnieuw on line gebracht. Wanneer ze weer on line bent, worden de exemplaren die in het tweede domein upgraden gestopt, bijgewerkt en opnieuw on line gebracht. Een cloud-service kan hooguit één upgrade tegelijk actief zijn. De upgrade wordt altijd tegen de meest recente versie van de cloud-service uitgevoerd.

In het volgende diagram ziet u hoe de upgrade verloopt als u een van alle van de rollen in de service upgrade:

![Upgrade service] (media/cloud-services-update-azure-service/IC345879.png "Upgrade service")

In dit volgende diagram ziet u hoe de update wordt uitgevoerd als u een upgrade van slechts één functie uitvoert:

![Rol bijwerken] (media/cloud-services-update-azure-service/IC345880.png "Rol bijwerken")  

> [AZURE.NOTE] Bij het upgraden van een service uit één exemplaar naar meerdere exemplaren worden uw service omlaag gebracht tijdens de upgrade wordt uitgevoerd door de manier waarop upgrades Azure services. De serviceovereenkomst aansprakelijke servicebeschikbaarheid van de service is alleen van toepassing op de services die worden geïmplementeerd met meer dan één exemplaar. De volgende lijst wordt beschreven hoe de gegevens op elke schijf wordt beïnvloed door elke Azure service-upgrade scenario:
>
>VM opstarten:
>
-   C: behouden
-   D: behouden
-   E: behouden
>
>Portal opnieuw opstarten:
>
-   C: behouden
-   D: behouden
-   E: vernietigd
>
>Reimage Portal:
>
-   C: behouden
-   D: vernietigd
-   E: vernietigd

>In-Place Upgrade:
>
-   C: behouden
-   D: behouden
-   E: vernietigd
>
>Migratie van knooppunt:
>
-   C: vernietigd
-   D: vernietigd
-   E: vernietigd

>Let op dat in het bovenstaande overzicht het station E: het hoofdstation van de rol vertegenwoordigt en mag geen hardgecodeerde. Gebruik in plaats daarvan de omgevingsvariabele % RoleRoot % vertegenwoordigen van het station.

>Om de uitvaltijd te minimaliseren bij het upgraden van een single-instance service, een nieuwe service voor meerdere exemplaren op de staging-server implementeren en uitvoeren van een VIP-swap.

Tijdens een automatische update evalueert de Azure Fabric Controller periodiek de gezondheid van de cloud-service om te bepalen wanneer het naar de volgende UD lopen veilig is. Deze evaluatie gezondheid wordt uitgevoerd op een per rol basis en houdt rekening met alleen exemplaren in de meest recente versie (dat wil zeggen exemplaren van UDs die hebben al zijn gelopen). Wordt gecontroleerd of een minimum aantal exemplaren van de rol, voor elke rol een bevredigende toestand terminal hebben bereikt.

### <a name="role-instance-start-timeout"></a>Rol exemplaar Start time
De Controller Fabric wacht 30 minuten voor elk exemplaar van de rol te bereiken van een staat gestart. Als de duur van de time-out verstrijkt, blijft de Controller Fabric lopen op de volgende rol.

<a name="RollbackofanUpdate"></a>
## <a name="rollback-of-an-update"></a>Terugdraaien van een update
Azure biedt flexibiliteit bij het beheren van services, doordat u extra bewerkingen op een Windows-service starten na de eerste update-aanvraag wordt geaccepteerd door de Controller Azure Fabric tijdens een update. Terugdraaien kan alleen worden uitgevoerd wanneer u een update (configuratiewijziging) of upgrade wordt de status **in behandeling** voor de implementatie. Een update of upgrade wordt beschouwd als in uitvoering, zolang er ten minste één exemplaar van de service die nog niet zijn bijgewerkt naar de nieuwe versie. Als u wilt testen of terugdraaien is toegestaan, Controleer de waarde van de vlag RollbackAllowed, die wordt geretourneerd door de bewerkingen [Ophalen implementatie](https://msdn.microsoft.com/library/azure/ee460804.aspx) en [Cloud Service eigenschappen](https://msdn.microsoft.com/library/azure/ee460806.aspx) , wordt ingesteld op true.

> [AZURE.NOTE] Alleen is het handig ongedaan maken aanroept voor een **in-place** update of upgrade omdat VIP swap upgrades waarbij een hele actieve sessie van uw service te vervangen door een andere.

Terugdraaien van een lopende update heeft de volgende effecten op de implementatie:

-   Rol exemplaren die was nog niet zijn bijgewerkt of upgrade naar de nieuwe versie worden niet bijgewerkt of bijgewerkt omdat de instanties van de doelversie van de service al wordt uitgevoerd.
-   Rol exemplaren die had al zijn bijgewerkt of een upgrade naar de nieuwe versie van het servicepakket (\*.cspkg)-bestand of de configuratie van de service (\*.cscfg)-bestand (of beide) worden teruggedraaid door de upgrade-versie van deze bestanden.

Dit is functioneel bepaald door de volgende functies:

-   De bewerking [Terugdraaien bijwerken of bijwerken](https://msdn.microsoft.com/library/azure/hh403977.aspx) , die kan worden aangeroepen voor een configuratie-update (geactiveerd door het aanroepende [Implementatieconfiguratie wijzigen](https://msdn.microsoft.com/library/azure/ee460809.aspx)) of een upgrade (geactiveerd door het aanroepende [Implementatie upgraden](https://msdn.microsoft.com/library/azure/ee460793.aspx)), zolang er is minstens één exemplaar in de service die nog niet zijn bijgewerkt naar de nieuwe versie.
-   Het element vergrendeld en het element RollbackAllowed die worden geretourneerd als deel van het lichaam van de reactie van de bewerkingen [Ophalen implementatie](https://msdn.microsoft.com/library/azure/ee460804.aspx) en [Cloud Service eigenschappen](https://msdn.microsoft.com/library/azure/ee460806.aspx) :
    1.  Het element vergrendeld, kunt u detecteren wanneer een bewerking van de mutating kan worden aangeroepen voor een bepaalde implementatie.
    2.  Het element RollbackAllowed kunt u detecteren wanneer de bewerking [Terugdraaien Update of een Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx) kan worden aangeroepen voor een bepaalde implementatie.

    Om een rollback, er geen zowel het vergrendeld als de RollbackAllowed-elementen. Het kwaliteiteisen om te bevestigen dat de RollbackAllowed is ingesteld op true. Deze elementen worden alleen geretourneerd als deze methoden worden aangeroepen met behulp van de verzoek-header ingesteld op "x-ms-versie: 10-01-2011 ' of een latere versie. Zie voor meer informatie over versiebeheer headers, [Versies van Service Management](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Er zijn situaties waarin het terugdraaien van een update of upgrade wordt niet ondersteund, deze zijn als volgt:

-   Vermindering van de lokale resources - als de update de lokale resources voor een rol de Azure platform verhoogt kunnen niet worden teruggedraaid. 
-   Quotabeperkingen - hebben als de update is een schaal door de bewerking die u mogelijk niet meer voldoende compute quota voor het voltooien van de bewerking ongedaan maken. Elk abonnement Azure heeft een quotum gekoppeld waarmee het maximum aantal cores die kan worden gebruikt door alle gehoste services die deel uitmaken van dit abonnement. Als terugdraaien van een bepaalde update uitvoeren zou uw abonnement over quota en die wordt een rollback wordt niet ingeschakeld.
-   Conflictsituatie - als de eerste update is voltooid, een ongedaan maken is niet mogelijk.

Een voorbeeld van wanneer het terugdraaien van een update is mogelijk handig is als u de bewerking [Upgrade implementatie](https://msdn.microsoft.com/library/azure/ee460793.aspx) gebruikt in de handmatige modus bepalen de koers waartegen een belangrijke in-place upgrade uw Azure service gehost is uitgerold.

Tijdens het traject van de upgrade [Upgrade implementatie](https://msdn.microsoft.com/library/azure/ee460793.aspx) aanroepen in de handmatige modus en beginnen met het upgraden van domeinen lopen. Als op een bepaald moment controleren als u de upgrade, let u op sommige gevallen rol in de eerste upgrade domeinen die u hebt niet meer reageert, kunt u de bewerking [Terugdraaien Update of een upgrade uitvoert](https://msdn.microsoft.com/library/azure/hh403977.aspx) op de implementatie die ongewijzigd laat de exemplaren die had nog niet bijgewerkt en rollback exemplaren die had zijn bijgewerkt naar de vorige servicepakket en configuratie aanroepen.

<a name="multiplemutatingoperations"></a>
## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Meerdere mutating bewerkingen op de implementatie van een voortdurend starten
In sommige gevallen kunt u meerdere gelijktijdige mutating bewerkingen op de implementatie van een voortdurend starten. Bijvoorbeeld een service kan bijwerkt en, terwijl die update is via de service worden geïnstalleerd, wilt u maken een andere, bijvoorbeeld een andere update toepassen om de update opnieuw gooit, of zelfs verwijderen van de installatie. Een geval waarin dit mogelijk is als een service-upgrade bevat buggy code die ervoor zorgt dat een exemplaar van de bijgewerkte functie herhaaldelijk vastloopt. In dit geval de Azure Fabric-Controller worden niet bij de toepassing van voortgang die niet bijwerken omdat het een onvoldoende aantal exemplaren in het bijgewerkte domein in orde zijn. Deze status wordt genoemd een *implementatie blijven hangen*. U kunt de implementatie door het terugdraaien van de update of een nieuwe update toepast op de bovenkant van het mislukken van de Registreer een.

Nadat de eerste aanvraag bij te werken of upgrade van de service is ontvangen door de Controller Azure Fabric, kunt u verdere mutating bewerkingen kunt starten. Dat wil zeggen, hebben niet te wachten tot de eerste bewerking is voltooid voordat u een andere mutating bewerking kan worden gestart.

Een tweede updatebewerking starten terwijl de eerste update voortdurend is wordt vergelijkbaar met de rollback-bewerking uitgevoerd. Als de tweede update in de automatische modus is, het eerste domein upgraden upgrade onmiddellijk mogelijk leiden tot exemplaren van meerdere upgraden domeinen worden off line op een bepaald punt in de tijd.

De mutating bewerkingen zijn als volgt: [Implementatieconfiguratie wijzigen](https://msdn.microsoft.com/library/azure/ee460809.aspx), [Implementatie upgraden](https://msdn.microsoft.com/library/azure/ee460793.aspx) [Bijwerkstatus implementatie](https://msdn.microsoft.com/library/azure/ee460808.aspx), [Implementatie verwijderen](https://msdn.microsoft.com/library/azure/ee460815.aspx)en [Rollback Update of een upgrade uitvoert](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Twee bewerkingen, [Distributie krijgen](https://msdn.microsoft.com/library/azure/ee460804.aspx) en [Cloud Service eigenschappen](https://msdn.microsoft.com/library/azure/ee460806.aspx), retourneren de vlag vergrendeld kunnen worden onderzocht om te bepalen of een mutating bewerking kan worden aangeroepen voor een bepaalde implementatie.

Om de versie die de vlag vergrendeld als resultaat van deze methoden aanroept, moet u verzoek-header ingesteld op "x-ms-versie: 10-01-2011 ' of een hoger. Zie voor meer informatie over versiebeheer headers, [Versies van Service Management](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>
## <a name="distribution-of-roles-across-upgrade-domains"></a>Verdeling van de rollen voor upgraden domeinen
Azure verdeelt gelijkmatig exemplaren van een rol in een aantal domeinen upgraden, die kan worden geconfigureerd als onderdeel van de service-definitiebestand (.csdef). Het maximum aantal domeinen upgraden is 20 en de standaardwaarde is 5. Zie voor meer informatie over het wijzigen van het definitiebestand voor [Azure Service Definition Schema (.csdef-bestand)](cloud-services-model-and-package.md#csdef).

Bijvoorbeeld, als uw rol tien exemplaren heeft, bevat elke upgrade domein standaard twee exemplaren. Als uw rol 14 exemplaren heeft, bevat vier domeinen upgrade drie exemplaren en een vijfde domein bevat twee.

Upgraden van domeinen worden aangeduid met een op nul gebaseerde index: de eerste upgrade domein heeft een ID van 0 en het tweede domein upgrade ID 1, enzovoort.

In het volgende diagram ziet u hoe een service dan bevat twee rollen worden verdeeld wanneer de service definieert twee domeinen voor upgrade. De service wordt uitgevoerd, acht exemplaren van de Webrol en negen exemplaren van de rol van de werknemer.

![Distributie van domeinen voor Upgrade] (media/cloud-services-update-azure-service/IC345533.png "Distributie van domeinen voor Upgrade")

> [AZURE.NOTE] Houd er rekening mee dat Azure bepaalt hoe exemplaren voor upgraden domeinen worden toegewezen. Het is niet mogelijk om op te geven welke exemplaren worden toegewezen aan het domein.

## <a name="next-steps"></a>Volgende stappen
[Cloud-Services beheren](cloud-services-how-to-manage.md)  
[Cloud-Services controleren](cloud-services-how-to-monitor.md)  
[Cloud-Services configureren](cloud-services-how-to-configure.md)  
