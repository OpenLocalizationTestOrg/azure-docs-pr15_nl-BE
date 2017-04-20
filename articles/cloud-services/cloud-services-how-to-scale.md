<properties
    pageTitle="Automatisch schalen een cloud-service in de portal | Microsoft Azure"
    description="(klassiek) Informatie over het gebruik van de portal voor klassieke auto schaal om regels te configureren voor een cloud service Webrol of functie van de werknemer in Azure."
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
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Het automatisch schalen een cloud-service

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-scale-portal.md)
- [Azure klassieke portal](cloud-services-how-to-scale.md)

U kunt handmatig schalen voor uw Webrol of functie van de werknemer op de pagina van de schaal van de klassieke Azure portal, of kunt u automatisch schalen op basis van CPU-belasting of een berichtenwachtrij.

>[AZURE.NOTE] Dit artikel richt zich op het web en werknemer rollen Cloud-Service. Wanneer u een virtuele machine (klassiek) direct maakt, wordt deze in een cloud-service gehost. Sommige van deze informatie is van toepassing op deze virtuele machines. Schalen van een set van de beschikbaarheid van virtuele machines is gewoon sluiten ze in- en uitschakelen op basis van de regeling van de schaal die u configureert. Zie voor meer informatie over virtuele Machines en beschikbaarheid sets, [beheren van de beschikbaarheid van virtuele Machines](../virtual-machines/virtual-machines-windows-classic-configure-availability.md)

U kunt de volgende informatie voordat u de schaalbaarheid van uw toepassing configureren:

- Schalen wordt beïnvloed door het gebruik van core. Grotere rol exemplaren gebruiken meer cores. U kunt een toepassing binnen de grenzen van de cores schalen voor uw abonnement. Als u uw abonnement heeft een limiet van twintig cores en uitvoeren van een toepassing met twee gemiddeld formaat bijvoorbeeld cloud services (in totaal vier cores), kunt u alleen de schaal van andere implementaties van de service cloud in uw abonnement door zestien cores. [Cloud Service formaten](cloud-services-sizes-specs.md) Zie voor meer informatie over de formaten.

- U moet een wachtrij maken en koppelen aan een rol voordat kunt u een toepassing op basis van een drempelwaarde bericht schalen. Zie [het gebruik van de wachtrij Storage-Service](../storage/storage-dotnet-how-to-use-queues.md)voor meer informatie.

- U kunt resources die zijn gekoppeld aan de service cloud schalen. Zie voor meer informatie over het koppelen van resources, [procedure: een resource koppelen aan een cloud service](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

- Als u maximale beschikbaarheid van uw toepassing, moet u ervoor zorgen dat deze wordt gedistribueerd met twee of meer exemplaren van de rol. Zie voor meer informatie, [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).



## <a name="schedule-scaling"></a>Schema aanpassen

Standaard alle rollen niet op een bepaalde planning. Daarom geen instellingen gewijzigd van toepassing op alle tijden en alle dagen gedurende het hele jaar. Als u wilt, kunt u setup handmatig of automatisch schalen voor:

- Weekdagen
- Tijdens het weekend
- Week nachten
- Week voor u uitvoeren
- Specifieke datums
- Specifiek datumbereik

Dit is conigured in de [Azure klassieke portal](https://manage.windowsazure.com/) op de  
**Cloud Services** > **\[uw service cloud\]** > **schaal** > **\[productie- of Staging\] ** pagina.

Klik op de knop **schema tijden instellen** voor elke rol die u wilt wijzigen.

![Cloud service automatisch schalen op basis van een schema][scale_schedules]



## <a name="manual-scale"></a>Handmatig schalen

Op de pagina **schaal** kunt u handmatig verhogen of verlagen van het aantal exemplaren in een cloud-service wordt uitgevoerd. Dit is geconfigureerd voor het schema dat u hebt gemaakt of aan alle tijd als u een planning hebt gemaakt.

1. In [Azure klassieke portal](https://manage.windowsazure.com/)op **Cloud Services**en klik vervolgens op de naam van de cloud-service voor het openen van het dashboard.

    > [AZURE.TIP] Als u uw service cloud niet ziet, moet u mogelijk wijzigen van **productie** naar de **Staging** - of vice versa.

2. Klik op **schaal**.

3. Selecteer de gewenste schaalopties voor planning. Standaard *geen geplande tijden* als er geen schema's gedefinieerd.

4. Zoek de sectie **schaal door metrische** en selecteert u ****. Dit is de standaardinstelling voor alle rollen.

5. Elke rol in de cloud-service heeft een schuifregelaar voor het wijzigen van het aantal exemplaren te gebruiken.

    ![De rol van een cloud-service handmatig schalen][manual_scale]

    Als u meer exemplaren, wellicht moet u de [grootte van de virtuele machine cloud service](cloud-services-sizes-specs.md)wijzigen.

6. Klik op **Opslaan**.  
Exemplaren van de rol worden toegevoegd of verwijderd op basis van uw selecties.

>[AZURE.TIP] Wanneer u ziet ![][tip_icon] de muis bewegen en u hulp wilt hebben over welke specifieke instelling heeft.


## <a name="automatic-scale---cpu"></a>Automatische schaal - CPU

Dit wordt aangepast als het gemiddelde percentage CPU-gebruik boven of onder de opgegeven drempelwaarden gaat; rol exemplaren worden gemaakt of verwijderd.

1. In [Azure klassieke portal](https://manage.windowsazure.com/)op **Cloud Services**en klik vervolgens op de naam van de cloud-service voor het openen van het dashboard.

    > [AZURE.TIP] Als u uw service cloud niet ziet, moet u mogelijk wijzigen van **productie** naar de **Staging** - of vice versa.

2. Klik op **schaal**.

3. Selecteer de gewenste schaalopties voor planning. Standaard *geen geplande tijden* als er geen schema's gedefinieerd.

4. Zoek de sectie **schaal door metric** en selecteer **CPU**.

5. Nu kunt u een minimum en maximum aantal rollen instanties, het doel van het CPU-gebruik (op een schaal van trigger) en het aantal exemplaren aan schaal omhoog en omlaag door.

![De rol van een cloud service schalen door cpu-belasting][cpu_scale]

>[AZURE.TIP] Wanneer u ziet ![][tip_icon] de muis bewegen en u hulp wilt hebben over welke specifieke instelling heeft.





## <a name="automatic-scale---queue"></a>Automatische schaal - wachtrij

Dit wordt automatisch aangepast als het aantal berichten in een wachtrij boven of onder een bepaalde drempel gaat; rol exemplaren worden gemaakt of verwijderd.

1. In [Azure klassieke portal](https://manage.windowsazure.com/)op **Cloud Services**en klik vervolgens op de naam van de cloud-service voor het openen van het dashboard.

    > [AZURE.TIP] Als u uw service cloud niet ziet, moet u mogelijk wijzigen van **productie** naar de **Staging** - of vice versa.

2. Klik op **schaal**.

3. Zoek de sectie **schaal door metric** en selecteer **CPU**.

4. Nu kunt u een minimum en maximum aantal exemplaren van de rollen, de wachtrij en het bedrag van de berichten in wachtrij plaatsen voor elk exemplaar en hoeveel exemplaren aan schaal omhoog en omlaag door.

![De rol van een cloud-service door een berichtenwachtrij schalen][queue_scale]

>[AZURE.TIP] Wanneer u ziet ![][tip_icon] de muis bewegen en u hulp wilt hebben over welke specifieke instelling heeft.


## <a name="scale-linked-resources"></a>Gekoppelde bronnen schalen

Vaak wanneer u de schaal van een rol, is het nuttig om schalen van de database die door de toepassing wordt ook gebruikt. Als u de database een koppeling naar de cloud-service, kunt u de instellingen aanpassen voor die resource door op de desbetreffende koppeling te klikken kunt openen.

1. In [Azure klassieke portal](https://manage.windowsazure.com/)op **Cloud Services**en klik vervolgens op de naam van de cloud-service voor het openen van het dashboard.

    > [AZURE.TIP] Als u uw service cloud niet ziet, moet u mogelijk wijzigen van **productie** naar de **Staging** - of vice versa.

2. Klik op **schaal**.

3. Het gedeelte **gekoppelde bronnen** zoeken en klikt u op **schaal voor deze database beheren**.

    > [AZURE.NOTE] Als u een gedeelte **gekoppelde bronnen** niet ziet, u waarschijnlijk geen gekoppelde bronnen.

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
