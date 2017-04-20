<properties
    pageTitle="Problemen met automatisch schalen met virtuele Machine schaal Sets | Microsoft Azure"
    description="Problemen met automatisch schalen met virtuele Machine schaal Sets. Begrijp de typische problemen en hoe deze op te lossen."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="guybo"/>

# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Problemen met automatisch schalen met virtuele Machine schaal Sets

**Probleem** : u hebt een infrastructuur autoscaling gemaakt in Azure Resource Manager met VM schaal – bijvoorbeeld door een sjabloon als volgt implementeren: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale – u hebt uw regels schaal is gedefinieerd en dat is uitstekend, met dien verstande dat, ongeacht hoeveel belasting u op het VMs plaatsen, het automatisch schalen Won't.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

Er zijn enkele overwegingen:

- Hoeveel cores elke VM heeft en u elke core laden?
 Het bovenstaande voorbeeld Azure Quickstart sjabloon heeft een do_work.php script, dat een single core wordt geladen. Als u een groter is dan een single core VM-grootte zoals Standard_A1 of D1 VM moet u meerdere keren uitvoeren van deze belasting. Controleer hoeveel het aantal processorcores, uw VMs aan de hand van [grootte voor Windows virtuele machines in Azure](../virtual-machines/virtual-machines-windows-sizes.md)

- Hoeveel VMs in de VM schaal ingesteld, gaat het werk aan elkaar?

    Een schaal van de gebeurtenis alleen mag plaatsvinden als de gemiddelde CPU over **alle** de VMs in een schaal groter is dan de drempelwaarde, na verloop van tijd intern gedefinieerd in de regels automatisch schalen.

- U geen gebeurtenissen schaal mist?

    Controleer of dat de controle in Azure portal voor schaal-gebeurtenissen vastgelegd. Of er is een schaal en een schaal af die zijn mislukt. U kunt filteren op "Scale"...

    ![Controlelogboeken][audit]

- Zijn de drempelwaarden in te schalen en schalen voldoende?

    Stel dat u een regel schaalt wanneer gemiddelde CPU groter is dan 50% meer dan 5 minuten, en op schaal in als de gemiddelde CPU minder dan 50% is ingesteld. Dit zou een "flapping" probleem veroorzaken als het CPU-gebruik deze drempel, lijkt met schaal acties voortdurend vergroten en verkleinen van de omvang van de set. Dat probeert de service automatisch schalen om te voorkomen dat 'klapperen' die als niet schalen kan manifest. Zorg er daarom voor het schalen en schalen in drempels zijn voldoende ruimte tussen het schalen zodat verschillende.

- U een eigen sjabloon JSON schrijven?

    Het is gemakkelijk fouten maken, dus beginnen met een sjabloon zoals waarboven een is bewezen te werken en kleine incrementele wijzigingen aanbrengen. 

- Kunt u handmatig schalen in of uit?

    Probeer het opnieuw distribueren van het middel VM schaal ingesteld met een andere 'capaciteit' instelling handmatig wijzigen van het aantal VMs. Is hier een van de voorbeeldsjabloon om dit te doen: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – moet u controleren of machine dezelfde grootte heeft als uw schaal ingesteld is met behulp van de sjabloon te bewerken. Als u het nummer van het VMs is handmatig wijzigen kunt, weet u dat het probleem is dat het om automatisch schalen.

- Controleer uw Microsoft.Compute/virtualMachineScaleSet, en Microsoft.Insights middelen in [Azure Resource Explorer](https://resources.azure.com/)

    Dit is een onmisbaar hulpmiddel waarin de status van uw resources Azure Resource Manager. Uw abonnement op en bekijkt u de resourcegroep die u wilt oplossen. Onder de resource Compute provider bekijkt de VM schaal stelt u hebt gemaakt en controleer de exemplaar-weergave waarin u de status van een implementatie. Controleer ook de weergave van het exemplaar van VMs in VM schaal ingesteld. Vervolgens gaat u naar de bron Microsoft.Insights provider en controleer dat de regels automatisch schalen zien er goed uit.

- De diagnostische extensie werkt en is dat de prestatiegegevens?

    __Update:__ Azure automatisch schalen is verbeterd voor het gebruik van een host op basis van maatstaven pijpleiding die niet langer nodig een diagnose extensie moet worden geïnstalleerd. Dit betekent dat de volgende die paragrafen niet langer van toepassing als u een autoscaling toepassing maken met de nieuwe pijpleiding. Voorbeelden van Azure sjablonen die zijn geconverteerd naar de pijpleiding host gebruikt zijn: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale, https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale. 

    Met behulp van host op basis van maatstaven voor automatisch schalen is het beter om de volgende redenen:

    - Minder bewegende onderdelen als er geen extensies diagnostische gegevens moeten worden geïnstalleerd.
    - Sjablonen voor eenvoudiger. NET inzicht automatisch schalen regels toevoegen aan een bestaande sjabloon voor schaal instellen.
    - Melden van betrouwbaarder en sneller starten van nieuwe VMs.

    De enige redenen wilt u mogelijk een diagnostische extensie blijven gebruiken zou zijn als u geheugen diagnostiek rapportage/schalen nodig. Host op basis van maatstaven melden geheugen niet.

    Met dat in gedachten, alleen gaat u als volgt de rest van dit artikel als u nog steeds diagnostische uitbreidingen voor uw autoscaling.

    Automatisch schalen in Azure Resource Manager kunnen werken (maar niet meer is) door middel van een VM extensie de extensie Diagnostics genoemd. Hiermee plaatst u prestatiegegevens aan een opslag account u in de sjabloon definieert. Deze gegevens worden vervolgens samengevoegd door de Azure Monitor-service.

    Als de service inzichten gegevens van de VMs lezen kan, moet deze stuurt je een e-mail – bijvoorbeeld als het VMs zijn, dus Controleer uw e-mailadres (die u hebt opgegeven bij het maken van de account Azure).

    U kunt ook Ga en bekijk de gegevens zelf. Bekijk de Azure opslag rekening met een cloud explorer. Voor het voorbeeld met [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), log in en kies de Azure abonnement u gebruikt en de naam van de diagnostische gegevens opslag waarnaar wordt verwezen in de definitie van de extensie diagnostische gegevens in de sjabloon voor de implementatie...

    ![Cloud Explorer][explorer]

    Hier ziet u een aantal tabellen waarin de gegevens van elke VM wordt opgeslagen. Linux en de metric van de CPU als voorbeeld nemen, kijk op de meest recente rijen. De Visual Studio cloud explorer ondersteunt een querytaal zodat u kunt een query als volgt uitvoeren ' tijdstempel gt datetime'2016-02-02T21:20:00Z "" om ervoor te zorgen dat u de meest recente gebeurtenissen (uitgaan van tijd is in UTC). Worden de gegevens die u in ziet er komen overeen met de regels van de schaal instellen? In het onderstaande voorbeeld wordt begon de CPU voor machine 20 op tot 100% gedurende de laatste 5 minuten...

    ![Opslag-tabellen][tables]

    Als de gegevens niet bevat, vervolgens houdt het dat het probleem is met de diagnostische extensie in het VMs uitgevoerd. Als de gegevens aanwezig is, betekent dat er is een probleem met uw regels schaal of met de service inzichten. [Azure Status](https://azure.microsoft.com/status/)controleren.

    Zodra u door middel van deze stappen overboden als u nog steeds problemen automatisch schalen kan u proberen de forums op [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp)of [Stack overflow](http://stackoverflow.com/questions/tagged/azure)of meld u belt. Zorg ervoor dat u kunt de sjabloon en een weergave van de prestatiegegevens delen.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
