<properties
    pageTitle="Automatisch schalen een cloud-service in de portal | Microsoft Azure"
    description="Informatie over het gebruik van de portal voor het configureren van regels voor automatisch schalen voor een cloud service Webrol of functie van de werknemer in Azure."
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

Voorwaarden kunnen worden ingesteld voor een cloud service werknemer rol die resulteren in een schaal in of uit. De voorwaarden voor de rol kunnen worden gebaseerd op de CPU, schijf of netwerkbelasting van de rol. U kunt ook een conditation op basis van een berichtenwachtrij of de metric van sommige andere Azure-bron die is gekoppeld aan uw abonnement instellen.

>[AZURE.NOTE] Dit artikel richt zich op het web en werknemer rollen Cloud-Service. Wanneer u een virtuele machine (klassiek) direct maakt, wordt deze in een cloud-service gehost. U kunt een standaard virtuele machine aanpassen door deze te koppelen aan een [beschikbaar](../virtual-machines/virtual-machines-windows-classic-configure-availability.md) en handmatig inschakelen of uitschakelen.

## <a name="considerations"></a>Overwegingen met betrekking tot

U kunt de volgende informatie voordat u de schaalbaarheid van uw toepassing configureren:

- Schalen wordt beïnvloed door het gebruik van core. Grotere rol exemplaren gebruiken meer cores. U kunt een toepassing binnen de grenzen van de cores schalen voor uw abonnement. Als u uw abonnement heeft een limiet van twintig cores en uitvoeren van een toepassing met twee gemiddeld formaat bijvoorbeeld cloud services (in totaal vier cores), kunt u alleen de schaal van andere implementaties van de service cloud in uw abonnement door zestien cores. [Cloud Service formaten](cloud-services-sizes-specs.md) Zie voor meer informatie over de formaten.

- U kunt schalen op basis van een bericht wachtrij drempel. Zie [het gebruik van de wachtrij Storage-Service](../storage/storage-dotnet-how-to-use-queues.md)voor meer informatie over het gebruik van wachtrijen.

- U kunt ook andere bronnen die zijn gekoppeld aan uw abonnement schalen.

- Als u maximale beschikbaarheid van uw toepassing, moet u ervoor zorgen dat deze wordt gedistribueerd met twee of meer exemplaren van de rol. Zie voor meer informatie, [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="where-scale-is-located"></a>Waar de schaal zich bevindt

Nadat u uw cloud-service, moet u de zichtbare cloud service blade hebben.

1. Selecteer de naam van de service cloud op cloud service blade, op de tegel **rollen en instanties** .   
**Belangrijk**: Zorg ervoor dat u de rol van cloud-service, niet het exemplaar rol die lager is dan de rol.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)

2. Selecteer de **schaal** naast elkaar.

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatische schaal

Met **Automatische**of **handmatige** modus kunt u schaalinstellingen voor voor een rol. De handleiding is zoals u zou verwachten, stelt u het absolute aantal exemplaren. Automatische echter kunt u aan een setregels die hoe en hoe veel u bepalen moet schalen.

Stel de optie **schaal door** regels voor **planning en prestaties**.

![Instellingen voor cloud services schaal met profiel en regel](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Een bestaand profiel.
2. Een regel voor het bovenliggende profiel toevoegen.
3. Een ander profiel toevoegen.

Selecteer **profiel toevoegen**. Het profiel bepaalt welke modus u wilt gebruiken voor de schaal: **altijd**, **herhaling**, **vaste datum**.

Nadat u het profiel en de regels hebt geconfigureerd, selecteert u het pictogram **Opslaan** boven.

#### <a name="profile"></a>Profiel

Het profiel wordt de minimum- en maximumaantal exemplaren voor de schaal, en ook wanneer het bereik van deze actief is.

* **Altijd**

    Bewaar dit bereik van beschikbare exemplaren.  

    ![Cloud-service altijd schalen](./media/cloud-services-how-to-scale-portal/select-always.png)
    
* **Terugkeerpatroon**

    Kies een aantal dagen van de week te schalen.

    ![Cloud service schalen met een terugkerend schema](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
    
* **Vaste datum**

    Een vaste datumbereik voor het schalen van de rol.

    ![CLoud service schalen met een vaste datum](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Nadat u het profiel hebt geconfigureerd, selecteert u de knop **OK** onderaan het blad profiel.

#### <a name="rule"></a>Regel

Regels worden toegevoegd aan een profiel en een voorwaarde die de schaal vertegenwoordigt. 

De trigger voor de regel is gebaseerd op een metric van de cloud-service u een waarde kunt toevoegen (CPU-gebruik, schijfactiviteit of activiteit op het netwerk). U kunt ook de trigger op basis van een berichtenwachtrij of de metric van sommige andere Azure-bron die is gekoppeld aan uw abonnement hebben.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Nadat u de regel hebt geconfigureerd, selecteert u de knop **OK** onderaan het blad van de regel.

## <a name="back-to-manual-scale"></a>Terug naar handmatig schalen

Ga naar de [Instellingen voor schalen](#where-scale-is-located) en stel de optie **schaal door** **een**aantal exemplaar die ik handmatig invoeren.

![Instellingen voor cloud services schaal met profiel en regel](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Hiermee verwijdert u automatisch schalen uit de rol en vervolgens kunt u het aantal instantie rechtstreeks instellen. 

1. De schaal (handmatig of automatisch) optie.
2. Een rol exemplaar schuifregelaar voor het instellen van de exemplaren voor.
3. Exemplaren van de rol voor.

Nadat u de schaalinstellingen hebt geconfigureerd, selecteert u het pictogram **Opslaan** boven.

