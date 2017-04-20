<properties
    pageTitle="Inleiding tot Azure DPM back-up | Microsoft Azure"
    description="Een inleiding tot de back-ups van DPM servers met behulp van de back-up Azure service"
    services="backup"
    documentationCenter=""
    authors="Nkolli1"
    manager="shreeshd"
    editor=""
    keywords="System Center Data Protection Manager, data protection manager, dpm back-up"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="trinadhk;giridham;jimpark;markgal"/>

# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Back-up werkbelasting op Azure met DPM wordt voorbereid

> [AZURE.SELECTOR]
- [Azure back-upserver](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Azure reserveserver (klassiek)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (klassiek)](backup-azure-dpm-introduction-classic.md)


Dit artikel bevat een inleiding tot het gebruik van back-up van Microsoft Azure ter bescherming van uw servers van System Center Data Protection Manager (DPM) en werkbelasting. Deze leest, zult u begrijpen:

- De werking van Azure DPM server back-up
- De vereisten voor het bereiken van een goede back-ervaring
- De typische fouten heeft gevonden en het omgaan met deze
- Ondersteunde scenario 's

System Center DPM back-up van gegevens en de toepassing. Aan DPM back-ups van gegevens kunnen worden opgeslagen op tape, op schijf of back-up in Azure met back-up van Microsoft Azure. DPM samenwerkt met Azure back-up als volgt:

- **DPM is geïmplementeerd als een fysieke server of op gebouwen virtuele machine** , DPM als is geïmplementeerd als een fysieke server of als een op ruimten Hyper-V virtuele machine, u kunt back-ups om een back-Azure kluis naast de schijf en tape back-up.
- **DPM is geïmplementeerd als een Azure VM** : van System Center 2012 R2 Update 3 DPM kan worden geïmplementeerd als een Azure virtuele machine. DPM is geïmplementeerd als een Azure virtuele machine die u kunt back-ups naar Azure schijven gekoppeld aan de DPM Azure virtual machine of u kunt de opslag van gegevens een back-up naar een kluis Azure back-up-offload.

## <a name="why-backup-your-dpm-servers"></a>Waarom back-up van uw servers DPM?

De zakelijke voordelen van het gebruik van Azure back-up voor back-ups van servers DPM omvatten:

- DPM-implementatie voor gebouwen kunt u als alternatief voor de lange termijn implementatie op tape Azure back-up.
- DPM-implementaties in Azure kunt Azure back-up u voor opslag van de schijf Azure offload zodat u om uit te breiden door oudere gegevens zijn opgeslagen in back-up Azure en nieuwe gegevens op schijf.

## <a name="how-does-dpm-server-backup-work"></a>Hoe werkt de DPM server back-up?
Als u wilt back-up van een virtuele machine, eerst een point-in-time momentopname van de gegevens nodig. De Azure back-service initieert de back-uptaak op het geplande tijdstip en activeert de back-extensie om een momentopname. De back-extensie coördinaten met de VSS-service in Gast consistentie te bereiken en roept de API blob momentopname van de Azure Storage-service zodra overeenstemming is bereikt. Dit gebeurt als u een consistent overzicht wilt van de schijven van de virtuele machine zonder af te sluiten.

Nadat de momentopname is genomen, worden de gegevens overgebracht door de Azure back-service om de back-up. De service zorgt voor het identificeren en overdragen van alleen de blokken die zijn gewijzigd ten opzichte van de laatste back-up maken van de opslag van back-ups en het netwerk efficiënt. Wanneer de overdracht van gegevens is voltooid, de momentopname wordt verwijderd en een herstelpunt is gemaakt. Dit herstelpunt kunt u bekijken in de klassieke Azure portal.

>[AZURE.NOTE] Voor Linux virtuele machines kan alleen bestand consistent back-up.

## <a name="prerequisites"></a>Vereisten
Azure back-up back-up kunt DPM als volgt voorbereiden:

1. **Een kluis voor back-up maken** — een kluis in de console Azure back-up maken.
2. **Download kluis referenties** — In Azure back-up van het management-certificaat dat u hebt gemaakt naar de kluis te uploaden.
3. **De reservekopie-Agent Azure installeren en registreren van de server** , van Azure back-up en de agent op elke DPM-server installeren en registreren de DPM-server in de back-kluis.

[AZURE.INCLUDE [backup-create-vault](../../includes/backup-create-vault.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]


## <a name="requirements-and-limitations"></a>Eisen (en beperkingen)

- DPM kan worden uitgevoerd als een fysieke server of een Hyper-V virtuele machine op System Center 2012 SP1 of System Center 2012 R2 is geïnstalleerd. Kan ook worden uitgevoerd als een Azure virtuele machine met System Center 2012 R2 met ten minste DPM 2012 R2 updatepakket 3 of een virtuele Windows-computer in VMWare met System Center 2012 R2 met ten minste Update Rollup 5.
- Als u DPM met System Center 2012 SP1 uitvoert, moet u updatepakket 2 voor System Center Data Protection Manager SP1 installeren. Dit is vereist voordat u de reservekopie-Agent Azure kunt installeren.
- De DPM-server moet beschikken over Windows PowerShell en .net Framework 4.5 is geïnstalleerd.
- DPM back-up de meeste werklasten Azure back-up. De Azure back-up ondersteuning voor een volledig overzicht van wat is ondersteund Zie onderstaande items.
- Gegevens die zijn opgeslagen in Azure back-up kan niet worden hersteld met de optie 'kopiëren naar tape'.
- Een Azure-account moet u de functie back-up van Azure is ingeschakeld. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Meer informatie over [back-up Azure prijzen](https://azure.microsoft.com/pricing/details/backup/).
- Azure back-up moet worden geïnstalleerd op de servers die u back wilt-up van de back-up Agent Azure. Elke server moet beschikken over ten minste 10% van de grootte van de gegevens die worden back-up, als lokale vrije opslagruimte beschikbaar. Back-up van gegevens van 100 GB is bijvoorbeeld minimaal 10 GB vrije ruimte op de tijdelijke locatie. Het minimum is 10%, wordt 15% van de vrije lokale opslagruimte moet worden gebruikt voor de locatie van de cache aanbevolen.
- Gegevens worden opgeslagen in de opslag Azure kluis. Er is geen limiet voor de hoeveelheid gegevens die u kunt back-up een back-up van Azure vault, maar de grootte van een gegevensbron (bijvoorbeeld een virtuele machine of database) 54,400 GB niet mag overschrijden.

Back-up Azure worden voor deze bestandstypen ondersteund:

- Gecodeerde (volledige back-ups alleen)
- Gecomprimeerd (incrementele back-ups wordt ondersteund)
- Sparse (incrementele back-ups wordt ondersteund)
- Gecomprimeerd en verspreide (behandeld als Sparse)

En deze worden niet ondersteund:

- Servers in een hoofdlettergevoelig bestandssysteem wordt niet ondersteund.
- Vaste koppelingen (overgeslagen)
- Reparse-punten (overgeslagen)
- Gecodeerd en gecomprimeerd (overgeslagen)
- Gecodeerde en verspreid (overgeslagen)
- Gecomprimeerde gegevensstroom
- Sparse-stream

>[AZURE.NOTE] Uit in System Center 2012 DPM met SP1, u kunt back-up van werkbelasting beschermd door de DPM naar Azure met Microsoft Azure Backup.
