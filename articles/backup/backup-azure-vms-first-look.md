<properties
    pageTitle="Eerste uiterlijk: Azure VMs beschermen met een back-up kluis | Microsoft Azure"
    description="Azure VMs beveiligen met back-up kluis. Zelfstudie wordt uitgelegd kluis maken, VMs registreren en beleid maken VMs in Azure beschermen."
    services="backup"
    documentationCenter=""
    authors="markgalioto"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/15/2016"
    ms.author="markgal; jimpark"/>


# <a name="first-look-backing-up-azure-virtual-machines"></a>Eerst wordt gezocht naar: back-ups van Azure virtual machines

> [AZURE.SELECTOR]
- [VMs beschermen met een kluis recovery services](backup-azure-vms-first-look-arm.md)
- [Azure VMs beschermen met een back-kluis](backup-azure-vms-first-look.md)

Deze zelfstudie gaat u door de stappen voor het maken van back-up een Azure VM (virtual machine) naar een back-up kluis in Azure. Dit artikel beschrijft het model Klassiek of Service Manager distributie model voor back-ups van VMs. Als u geïnteresseerd in een VM maken voor een kluis Recovery Services die deel uitmaakt van een resourcegroep bent, Zie [wordt eerst gezocht: VMs beschermen met een kluis recovery services](backup-azure-vms-first-look-arm.md). Om te kunnen voltooien van deze zelfstudie, moeten deze vereisten worden voldaan:

- U hebt een VM in Azure abonnement gemaakt.
- De VM heeft verbinding met Azure openbare IP-adressen. Zie [Netwerkverbindingen](./backup-azure-vms-prepare.md#network-connectivity)voor meer informatie.

Als u wilt een back-up een VM, zijn er vijf hoofdstappen:  

![stap 1](./media/backup-azure-vms-first-look/step-one.png) een kluis back-up maken of een bestaande back-kluis te identificeren. <br/>
![stap twee](./media/backup-azure-vms-first-look/step-two.png) de klassieke Azure portal gebruiken om te detecteren en registreren van de virtuele machines. <br/>
![stap drie](./media/backup-azure-vms-first-look/step-three.png) de VM-Agent te installeren. <br/>
![stap vier](./media/backup-azure-vms-first-look/step-four.png) het beleid voor de bescherming van de virtuele machines maken. <br/>
![Stap vijf](./media/backup-azure-vms-first-look/step-five.png) de back-up uitvoert.

![Globaal overzicht van de back-up VM](./media/backup-azure-vms-first-look/backupazurevm-classic.png)

>[AZURE.NOTE] Azure heeft twee implementatiemodellen voor het maken en werken met resources: [Resource Manager en klassiek](../resource-manager-deployment-model.md). Deze handleiding is bedoeld voor gebruik met het VMs die kunnen worden gemaakt in de klassieke Azure portal. De back-up Azure service ondersteunt VMs op basis van een Resource Manager. Zie voor meer informatie over het VMs maken voor een kluis recovery services [eerste blik: VMs beschermen met een kluis recovery services](backup-azure-vms-first-look-arm.md).



## <a name="step-1---create-a-backup-vault-for-a-vm"></a>Stap 1: Maak een back-up kluis voor een VM

Een back-vault is een entiteit die de back-ups en herstel punten die zijn gemaakt na verloop van tijd worden opgeslagen. De back-kluis bevat ook de back-beleid dat wordt toegepast op de virtuele machines die back-up wordt gemaakt.

1. Log in om de [klassieke Azure portal](http://manage.windowsazure.com/).

2. Klik op **Nieuw** in de linkerbenedenhoek van de portal voor Azure

    ![Klik op Nieuw menu](./media/backup-azure-vms-first-look/new-button.png)

3. Klik in de wizard Snelle invoer **Van gegevens** > **Recovery Services** > **Back-up Vault** > **Snelle invoer**.

    ![Maken van back-kluis](./media/backup-azure-vms-first-look/new-vault-wizard-one-subscription.png)

    De wizard wordt u gevraagd de **naam** en **de regio**. Als u meer dan één abonnement beheren, verschijnt er een dialoogvenster voor het kiezen van het abonnement.

4. Voer voor de **naam**een beschrijvende naam ter identificatie van de kluis. De naam moet uniek zijn voor het abonnement Azure.

5. Selecteer de regio voor de kluis in de **regio**. In hetzelfde gebied, als de virtuele machines die het voorkomt dat de kluis **moet** worden.

    Als u de regio waarin uw VM bestaat niet, sluit deze wizard en klik op **virtuele Machines** in de lijst met services op Azure. De kolom locatie bevat de naam van de regio. Hebt u virtuele machines in meerdere regio's, maakt u een back-up kluis in elke regio.

6. Als er geen dialoogvenster **abonnement** in de wizard, gaat u verder met de volgende stap. Als u met meerdere abonnementen werkt, selecteert u een abonnement wilt koppelen aan de nieuwe back-kluis.

    ![Kluis toast melding maken](./media/backup-azure-vms-first-look/backup-vaultcreate.png)

7. Klik op de **kluis te maken**. Het kan even duren voor de kluis back-up moet worden gemaakt. Controleren van de statusmeldingen onderaan in de portal.

    ![Kluis toast melding maken](./media/backup-azure-vms-first-look/create-vault-demo.png)

    Een bericht bevestigt dat de kluis heeft gemaakt. Dit wordt vermeld op de pagina **services herstellen** als **actief**.

    ![Kluis toast melding maken](./media/backup-azure-vms-first-look/create-vault-demo-success.png)

8. Selecteer in de lijst van kluizen op **Recovery Services** pagina de kluis die u hebt gemaakt voor het starten van de pagina **Quick Start** .

    ![Lijst met back-kluizen](./media/backup-azure-vms-first-look/active-vault-demo.png)

9. Klik op de pagina **Quick Start** **configureren** om te openen de optie opslag replicatie.
    ![Lijst met back-kluizen](./media/backup-azure-vms-first-look/configure-storage.png)

10. De optie **opslag, replicatie** , kiest u de replicatieoptie voor uw kluis.

    ![Lijst met back-kluizen](./media/backup-azure-vms-first-look/backup-vault-storage-options-border.png)

    Standaard heeft de kluis geo-redundante opslag. Geo-redundante opslag kiezen als dit uw primaire back-up. Kies lokaal redundante opslag als u wilt een goedkopere optie die niet helemaal zo duurzaam. Meer informatie over opties voor geo-redundante en lokaal redundante opslag in de [opslag van Azure replicatie-overzicht](../storage/storage-redundancy.md).

Nadat u de opslagoptie voor uw kluis, bent u gereed om de VM koppelen aan de kluis. Om te beginnen met de koppeling, ontdekken en registreer de Azure virtuele machines.

## <a name="step-2---discover-and-register-azure-virtual-machines"></a>Stap 2 - ontdekken en Azure registreren virtuele machines
Voordat u zich registreert de VM met een kluis, de discovery-proces voor elke nieuwe VMs worden uitgevoerd. Dit geeft een lijst van virtuele machines in het abonnement, samen met extra informatie, zoals de naam van de wolk en de regio.

1. Aanmelden bij de [klassieke Azure portal](http://manage.windowsazure.com/)

2. Klik in de klassieke Azure portal **Recovery Services** om de lijst van kluizen Recovery Services.
    ![Selecteer de werkbelasting](./media/backup-azure-vms-first-look/recovery-services-icon.png)

3. Selecteer in de lijst van kluizen, de kluis back-up een VM.

    Wanneer u uw kluis selecteert, wordt het geopend in de pagina **Quick Start**

4. Klik op **Items geregistreerd**in het menu kluis.

    ![Selecteer de werkbelasting](./media/backup-azure-vms-first-look/configure-registered-items.png)

5. Selecteer in het menu **Type** **Azure Virtual Machine**.

    ![Selecteer de werkbelasting](./media/backup-azure-vms/discovery-select-workload.png)

6. Klik op **DISCOVER** onderaan de pagina.
    ![Knop ontdekt](./media/backup-azure-vms/discover-button-only.png)

    De discovery-proces duurt een paar minuten terwijl de tabelindeling van de virtuele machines. Er is een bericht onder aan het scherm waarin u zien kunt dat het proces wordt uitgevoerd.

    ![Ontdek VMs](./media/backup-azure-vms/discovering-vms.png)

    Wijzigingen in de berichtgevingsvoorkeuren als het proces is voltooid.

    ![Discovery gedaan](./media/backup-azure-vms-first-look/discovery-complete.png)

7. Klik op **REGISTREREN** onderaan de pagina.
    ![Knop registreren](./media/backup-azure-vms-first-look/register-icon.png)

8. Selecteer de virtuele machines die u wilt registreren in het snelmenu **Artikelen registreren** .

    >[AZURE.TIP] Meerdere virtuele machines kunnen in één keer worden geregistreerd.

    Er wordt een taak gemaakt voor elke virtuele machine die u hebt geselecteerd.

9. Klik op **Taak weergeven** in de kennisgeving naar de pagina **taken** .

    ![Project registreren](./media/backup-azure-vms/register-create-job.png)

    De virtuele machine wordt ook weergegeven in de lijst met geregistreerde artikelen, en de status van de bewerking van de registratie.

    ![Status 1 registreren](./media/backup-azure-vms/register-status01.png)

    Wanneer de bewerking is voltooid, de status verandert zodat de status *geregistreerd* .

    ![Registratiestatus 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---install-the-vm-agent-on-the-virtual-machine"></a>Stap 3: de VM-Agent installeren op de virtuele machine

De Azure VM-Agent moet worden geïnstalleerd op de Azure virtual machine voor de uitbreiding van de back-up om te werken. Als uw VM uit de galerie met Azure is gemaakt, is de VM-Agent al aanwezig op de VM. Aan de [bescherming van de VMs](backup-azure-vms-first-look.md#step-4-protect-azure-virtual-machines)kunt u overslaan.

Als uw VM migratie van een datacenter in gebouwen, de VM waarschijnlijk beschikt niet over de VM Agent is geïnstalleerd. U moet de VM-Agent installeren op de virtuele machine voordat u overgaat tot het beschermen van de VM. Zie voor gedetailleerde stappen voor het installeren van de Agent VM de [VM Agent sectie van het artikel VMs back-up](backup-azure-vms-prepare.md#vm-agent).


## <a name="step-4---create-the-backup-policy"></a>Stap 4: het back-beleid maken
Voordat u de eerste back-uptaak activeren, het schema instellen als back-momentopnamen. De planning wanneer back-momentopnamen worden en hoe lang deze momentopnamen worden bewaard, is de back-beleid. De gegevens bewaren is gebaseerd op opa-vader-zoon back-schema.

1. Ga naar de back-kluis onder **Recovery Services** in de klassieke Azure portal en klikt u op **Items geregistreerd**.
2. **Azure Virtual Machine** selecteert in de vervolgkeuzelijst.

    ![Selecteer de werkbelasting in de portal](./media/backup-azure-vms/select-workload.png)

3. Klik op **beveiligen** onder aan de pagina.
    ![Klik op beschermen](./media/backup-azure-vms-first-look/protect-icon.png)

    De **wizard Beveiliging van Items** wordt weergegeven en worden *alleen* virtuele machines die worden geregistreerd en niet beveiligd.

    ![Beveiliging configureren op schaal](./media/backup-azure-vms/protect-at-scale.png)

4. Selecteer de virtuele machines die u wilt beveiligen.

    Als er twee of meer virtuele machines met dezelfde naam, kunt u de Cloud-Service gebruiken onderscheid maken tussen de virtuele machines.

5. Selecteer een bestaand beleid of maak een nieuw beleid ter bescherming van de virtuele machines die u in het menu **Beveiliging configureren** .

    Nieuwe back-up kluizen hebben het standaardbeleid dat is gekoppeld aan de kluis. Dit beleid gaat dagelijks elke avond momentopname en dagelijkse momentopname is gedurende 30 dagen bewaard. Elke back-beleid kan meerdere virtuele machines gekoppeld hebben. De virtuele machine kan echter alleen zijn gekoppeld aan één beleid tegelijkertijd.

    ![Bescherm met nieuw beleid](./media/backup-azure-vms/policy-schedule.png)

    >[AZURE.NOTE] Een back-up beleid omvat een stelsel bewaren voor de geplande back-ups. Als u een bestaande back-up beleid selecteert, kunt u zich niet wijzigen van de opties voor het behoud in de volgende stap.

6. Op **Bereik bewaren** de draagwijdte dagelijkse, wekelijkse, maandelijkse en jaarlijkse voor de specifieke punten van de back-up.

    ![Virtuele machine is een back-up met herstelpunt](./media/backup-azure-vms/long-term-retention.png)

    Bewaarbeleid geeft de lengte van de tijd om een back-up op te slaan. U kunt verschillende bewaarbeleid op basis van wanneer de back-up wordt genomen.

7. Klik op **taken** om de lijst met taken **Configureren beveiliging** weer te geven.

    ![Bescherming-taak configureren](./media/backup-azure-vms/protect-configureprotection.png)

    Nu u het beleid hebt gemaakt, gaat u naar de volgende stap en de eerste back-up uitvoeren.

## <a name="step-5---initial-backup"></a>Stap 5 - eerste back-up

Als een virtuele machine met een beleid is beveiligd, kunt u die relatie bekijken op het tabblad **Items beveiligd** . Totdat de eerste back-up, geeft de **Status van bescherming** als **beveiligd - (in afwachting van de eerste back-up)**. De eerste geplande back-up is de *eerste back-up*.

![Back-up in behandeling](./media/backup-azure-vms-first-look/protection-pending-border.png)

De eerste back-up nu wilt starten:

1. Klik op **Nu back-up** onder aan de pagina op de pagina **Items beveiligd** .
    ![Het pictogram nu een back-up](./media/backup-azure-vms-first-look/backup-now-icon.png)

    De Azure back-service maakt voor de eerste back-up een back-up.

2. Klik op het tabblad **taken** om de lijst met taken.

    ![Back-up gemaakt](./media/backup-azure-vms-first-look/protect-inprogress.png)

    Wanneer de eerste back-up is voltooid, wordt de status van de virtuele machine op het tabblad **Beveiligde Items** *beveiligd*.

    ![Virtuele machine is een back-up met herstelpunt](./media/backup-azure-vms/protect-backedupvm.png)

    >[AZURE.NOTE] Back-ups maken van virtuele machines is een lokaal proces. U kan niet back-up van virtuele machines van de ene regio naar een kluis back-up in een ander gebied. Voor elke Azure regio met VMs die u back wilt-up worden gemaakt, moet ten minste één back-kluis dus worden gemaakt in die regio.

## <a name="next-steps"></a>Volgende stappen
Nu dat u een reservekopie hebt gemaakt van een VM, zijn er verschillende Vervolgstappen die van belang kunnen zijn. De meest logische stap is vertrouwd raken met het terugzetten van gegevens naar een VM. Er zijn echter de beheertaken die u inzicht in uw gegevens veilig te houden en de kosten te minimaliseren.

- [Beheren en controleren van uw virtuele machines](backup-azure-manage-vms.md)
- [Herstellen van virtuele machines](backup-azure-restore-vms.md)
- [Richtlijnen voor het oplossen van problemen](backup-azure-vms-troubleshoot.md)


## <a name="questions"></a>Heb je vragen?
Als u vragen hebt of als er een functie die u zou willen zien opgenomen, [feedback verzenden](http://aka.ms/azurebackup_feedback).
