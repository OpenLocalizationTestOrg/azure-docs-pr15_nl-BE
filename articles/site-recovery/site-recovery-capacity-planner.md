<properties
    pageTitle="Capaciteit voor het beveiligen van virtuele machines en fysieke servers in Azure-Site-Recovery plan | Microsoft Azure"
    description="Azure-Site-Recovery coördineert de replicatie, failover en herstel van virtuele machines en fysieke servers in op-ruimten naar Azure of een secundaire op gebouwen-site bevindt." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="07/12/2016" 
    ms.author="raynew"/>

# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Capaciteit voor het beveiligen van virtuele machines en fysieke servers in Azure-Site-Recovery plan

Het hulpprogramma Azure Site herstel Capacity Planner helpt u om uw capaciteitsbehoeften voor het beveiligen van VMs Hyper-V, VMware VMs en fysieke servers Windows/Linux met Azure Site herstel te berekenen.


## <a name="overview"></a>Overzicht

Gebruik de Planner Site herstel capaciteit voor het analyseren van uw bronomgeving en de werkbelasting en moet uitzoeken bandbreedte, server resources u nodig hebt op de bronlocatie en de bronnen die u nodig hebt in uw doellocatie (virtuele machines en opslag enz). 

U kunt het hulpprogramma in een paar van de modi uitvoeren:

- **Snelle planning**: het programma in deze modus voor netwerk- en prognoses op basis van een gemiddeld aantal VMs, schijven, opslag en snelheid wijzigen uitvoeren.
- **Gedetailleerde planning**: het hulpprogramma uitvoeren in deze modus en de bijzonderheden van elke belasting op het niveau van de VM. VM-compatibiliteit analyseren en projecties van netwerk- en ophalen.

## <a name="before-you-start"></a>Voordat u begint

Voordat u het hulpprogramma wilt uitvoeren:

1. Informatie verzamelen over uw omgeving, met inbegrip van VMs, schijven per VM, opslag per schijf.
2. De snelheid van uw dagelijkse wijzigen (lospeuteren) voor gerepliceerde gegevens identificeren. Om dit te doen:

    - Als u bent repliceren van Hyper-V VMs download [Hyper-V capaciteit planning tool](https://www.microsoft.com/download/details.aspx?id=39057) als u de snelheid wijzigen. [Meer informatie](site-recovery-capacity-planning-for-hyper-v-replication.md) over dit hulpprogramma. We raden dat u dit hulpprogramma uitvoert via een week voor het vastleggen van gemiddelden.
    - Als u VMware virtuele machines te repliceren bent, gebruiken het [toestel voor capaciteitsplanning vSphere](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) om het lospeuteren tarief te berekenen.
    - Als u bij het repliceren van fysieke servers moet u handmatig te schatten.

## <a name="run-the-quick-planner"></a>De Planner snel uitvoeren
1.  Download en open het hulpprogramma [Azure Site herstel Capacity Planner](http://aka.ms/asr-capacity-planner-excel) . U moet selecteren inschakelen en inhoud wanneer u wordt gevraagd inschakelen van macro's uitvoeren. 
2.  Selecteer in **Selecteer een type planner** **Planner snel** uit de lijst.

    ![Aan de slag](./media/site-recovery-capacity-planner/getting-started.png)

3.  Voer de vereiste gegevens in het werkblad **Capacity Planner** . U moet alle velden omcirkeld in het rood in het screenshot hieronder invullen.

    - Kies in **uw scenario selecteren** **Azure Hyper-V** of **VMware/fysieke naar Azure**.
    - U verzamelt met het [hulpmiddel capaciteit Hyper-V](site-recovery-capacity-planning-for-hyper-v-replication.md) of het [toestel voor capaciteitsplanning vSphere](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance)in **gemiddelde dagelijkse gegevens wijzigt rente (%)** in de gegevens geplaatst.  
    - **Compressie** is alleen van toepassing op compressie aangeboden wanneer een VMs VMware of fysieke servers worden gerepliceerd naar Azure. We verwachten dat 30% of meer, maar u kunt de instelling desgewenst wijzigen. Voor het repliceren van Hyper-V VMs met Azure-compressie kunt u een toestel van derden zoals Riverbed. 
    -  In **Behoud ingangen** opgeven hoe lang de replica's moeten worden bewaard. Als u bij het repliceren van VMware of fysieke servers de invoerwaarde in dagen. Als u bij het repliceren van Hyper-V de tijd opgeven in uren.
    -  In een **aantal uren in waarvoor de eerste replicatie voor de batch van virtuele machines moet worden voltooid** en het **aantal virtuele machines per batch de eerste replicatie** moet u instellingen die worden gebruikt voor het berekenen van de eerste replicatie eisen invoeren.  Bij het herstel van de Site is geïmplementeerd moet de eerste volledige gegevensset worden geüpload. 

    ![Ingangen](./media/site-recovery-capacity-planner/inputs.png)

2.  Nadat u de waarden voor de bronomgeving hebt geplaatst, wordt de weergegeven uitvoer omvat:

    - **Bandbreedte nodig is voor replicatie delta** (MB/sec). Bandbreedte voor replicatie delta wordt berekend op het gemiddelde tarief voor dagelijkse gegevens wijzigen.
    - **Bandbreedte vereist is voor de eerste replicatie** (MB/sec). Bandbreedte van het netwerk voor de eerste replicatie wordt berekend op de eerste replicatie waarden die in worden geplaatst. 
    - **Opslag vereist (in GB)** is de totale Azure opslag vereist.
    - **Totale IOP's voor standaardopslag accounts** wordt berekend op basis van 8 K grootte van IOP's voor de totale standaardopslag accounts.  Voor de snelle Planner die het aantal wordt berekend op basis van de bron VMs schijven en daggegevens snelheid te wijzigen. Voor de gedetailleerde planning die het aantal wordt berekend op basis van het totale aantal VMs die zijn toegewezen aan de standaard Azure VMs en gegevens wijzigen voor de VMs. 
    - **Aantal accounts standaardopslag** kunt u het totale aantal standaardopslag accounts die ter bescherming van het VMs. Overigens past een standaard opslag account tot 20.000 IOP's via de VMs in een standaard opslag en maximaal 500 IOP's per schijf wordt ondersteund. 
    - **Aantal blob schijven vereist** , geeft het aantal schijven die op de Azure opslag wordt gemaakt.
    - **Nummer van de premie opslag rekeningen nodig** hier het totale aantal accounts van premium opslag die nodig zijn ter bescherming van het VMs. Houd er rekening mee dat een bron VM met hoge IOP's (meer dan 20000) een opslag premium account moet. Een opslag premium account kan tot 80.000 IOP's bevatten.
    - **Totale IOP's voor opslag van de premie** wordt berekend op basis van 256 K IOP's eenheid voor de breedte van de totale premie opslag rekeningen.  Voor de snelle Planner die het aantal wordt berekend op basis van de bron VMs schijven en daggegevens snelheid te wijzigen. Voor de gedetailleerde planning die het aantal wordt berekend op basis van het totale aantal VMs die zijn toegewezen aan premium Azure VM (DS en GS serie) en de gegevens wijzigen voor de VMs. 
    - **Aantal configuratieservers vereist** geeft aan hoeveel configuratieservers u vereist voor de implementatie (1)
    - **Aantal extra proces-servers vereist** geeft aan of extra proces-servers vereist naast de processerver die geconfigureerd op de configuratieserver standaard.
    - **100% extra opslagruimte op de bron** bevat of extra opslagruimte is vereist op de bronlocatie.
            
    ![Uitvoer](./media/site-recovery-capacity-planner/output.png)
 
## <a name="run-the-detailed-planner"></a>De gedetailleerde planning uitvoeren


1.  Download en open het hulpprogramma [Azure Site herstel Capacity Planner](http://aka.ms/asr-capacity-planner-excel) . U moet selecteren inschakelen en inhoud wanneer u wordt gevraagd inschakelen van macro's uitvoeren. 
2.  Selecteer in **Selecteer een type planner** **Gedetailleerde planning** in de keuzelijst.

    ![Aan de slag](./media/site-recovery-capacity-planner/getting-started-2.png)

3.  Voer de vereiste gegevens in het werkblad **Werkbelasting, kwalificatie** . U moet alle gemarkeerde velden invullen.

    - Geef het aantal cores op een bronserver in **processorcores** .
    - Geef de grootte van het RAM-geheugen van een bronserver in **toewijzing van geheugen in MB** . 
    - Het **Nummer van de NIC's** opgeven het aantal netwerkadapters op een bronserver. 
    -  Geef de totale grootte van de VM-opslag in de **totale opslag (in GB)** . Bijvoorbeeld als de bronserver heeft 3 schijven met 500 GB, totale opslagruimte is 1500 GB.
    -  Geef het totale aantal schijven van een bronserver in **aantal schijven die zijn aangesloten** .
    -  Geef het gemiddeld gebruik in de **bezettingsgraad van de schijf** .
    -  Geef dat de dagelijkse gegevens wijzigen tarief van een bronserver in **dagelijks tarief (%) te wijzigen** .
    -  Ofwel Voer Azure VM-grootte die u wilt toewijzen in **Azure toewijzing van formaat** . Klik op de**IaaS VMs berekenen**als u niet wilt dat dit handmatig doen. Houd er rekening mee dat als u een handmatige instelling invoer en klik vervolgens op berekenen IaaS VMs de handmatige instelling, kan worden overschreven omdat het proces compute automatisch de beste overeenkomst op Azure VM-grootte identificeert.

    ![Werkbelasting, kwalificatie](./media/site-recovery-capacity-planner/workload-qualification.png)

4.  Als u op klikt wordt hier **IaaS VMs berekenen** wat het doet:

    - Valideert de invoer verplicht.
    - IOP's berekend en stelt de beste Azure VM aize overeenkomt met elke VMs in aanmerking komt voor replicatie naar Azure. Als de juiste grootte van Azure VM kan niet worden opgespoord met dat een fout wordt uitgegeven. Bijvoorbeeld als het aantal in bijlage 65 een fout schijven is problemen sinds de hoogste grootte is Azure VM 64.
    - Een opslag-account die kan worden gebruikt voor een Azure VM voorgesteld.
    - Berekent het totaal aantal rekeningen van premie opslag vereist voor de werkbelasting en standaardopslag. Blader omlaag in Azure opslagtype en de opslag-account die kan worden gebruikt voor een bronserver bekijken
    - Is voltooid en de rest van de tabel op basis van het vereiste opslagtype (standard of premium) toegewezen voor een VM en het aantal aangesloten schijven worden gesorteerd. Bevat Ja voor alle VMs die voldoen aan de vereisten voor een reservekopie op Azure, kolom A (Is VM qualified?). Als een VM kan geen back-maximaal wordt Azure een fout weergegeven.

Worden uitgevoerd en de kolommen AA AE bevatten informatie voor elke VM.

![Werkbelasting, kwalificatie](./media/site-recovery-capacity-planner/workload-qualification-2.png)


### <a name="example"></a>Voorbeeld
Als u bijvoorbeeld voor zes VMs met de waarden in de tabel, het hulpprogramma berekent en wijst het meest geschikt voor Azure VM en de Azure opslagvereisten.

![Werkbelasting, kwalificatie](./media/site-recovery-capacity-planner/workload-qualification-3.png)

- Opmerking in de uitvoer in het voorbeeld:
    
    - De eerste kolom is een kolom validatie voor VMs, schijven en lospeuteren.
    - Twee accounts standaardopslag en één premie opslag account nodig zijn voor vijf VMs. 
    -  VM3 niet in aanmerking voor bescherming, omdat een of meer schijven meer dan 1 TB.
    -  VM1 en VM2 kunt de eerste standaardopslag account gebruiken
    -  VM4 kunt tweede standaardopslag account gebruiken.
    -  VM5 en VM6 moet een opslag premium account en kunnen zowel een enkele account gebruiken.

    >[AZURE.NOTE]  IOP's op standard en premium opslag worden berekend op het niveau van de VM en niet op het niveau van de schijf. Maximaal 500 IOP's per schijf kunt verwerken met een standaard virtuele machine. Als IOP's voor een schijf groter dan 500 zijn moet u premie opslag. Echter als IOP's voor een schijf groter is dan 500 maar IOP's voor de totale VM-schijven binnen de ondersteuning voor standaard Azure VM (VM grootte, het aantal schijven, aantal adapters, CPU, geheugen) vervolgens de planner zijn hervat een standaard VM en niet in de DS of GS serie. U moet handmatig bijwerken de toewijzing Azure size-cel met de juiste DS of GS serie VM.

5. Nadat alle details uitgevoerd worden, klikt u op **gegevens indienen om de planner tool** om te openen de **Planner capaciteit** werklasten worden gemarkeerd om weer te geven of ze voor bescherming in aanmerking komt of niet zijn.


### <a name="submit-data-in-the-capacity-planner"></a>Indienen van gegevens in de planning van de capaciteit

1.  Bij het openen van het werkblad **Capacity Planner** wordt deze gevuld op basis van de instellingen die u hebt opgegeven. Het woord "Belasting" wordt weergegeven in de broncel **Infra "inputs"** die de input **Werkbelasting, kwalificatie** werkblad weergeven. 
2.  Als u wijzigen wilt, u moet het werkblad **Werkbelasting, kwalificatie** wijzigen en klikt u opnieuw op verzendgegevens naar de planner tool.  

    ![Capaciteit Planner](./media/site-recovery-capacity-planner/capacity-planner.png)


