<properties
    pageTitle="Maak een VM beschikbaarheid set | Microsoft Azure"
    description="Informatie over het maken van een beschikbaarheid instellen voor de virtuele machines met Azure portal of het implementatiemodel Resource Manager met PowerShell."
    keywords="beschikbaarheid instellen"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>


# <a name="create-an-availability-set"></a>Maak een set beschikbaarheid 

Wanneer via de portal, als u wilt dat uw VM moet deel uitmaken van de beschikbaarheid, moet u de eerste beschikbaarheid maken.

Zie voor meer informatie over het maken en gebruiken van beschikbaarheid sets, [beheren van de beschikbaarheid van virtuele machines](virtual-machines-windows-manage-availability.md).


## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a>De portal te gebruiken voor het maken van een instellen voordat u uw VMs maakt beschikbaarheid

1. In het menu van de hub, klikt u op **Bladeren** en selecteer **beschikbaarheid wordt ingesteld**.

2. Op de u **beschikbaarheid blade ingesteld**, klikt u op **toevoegen**.

    ![Schermafdruk van de knop toevoegen voor het maken van een nieuwe beschikbaarheid instellen.](./media/virtual-machines-windows-create-availability-set/add-availability-set.png)

3. Vul de gegevens voor uw set op het blad **beschikbaar maken** .

    ![Schermafdruk van de gegevens moet u invoeren om de set beschikbaar te maken.](./media/virtual-machines-windows-create-availability-set/create-availability-set.png)

    - **Name** - de naam moet 1-80 tekens bestaan uit cijfers, letters, punten, onderstrepingstekens en streepjes. Het eerste teken moet een letter of cijfer. Het laatste teken moet een letter, getal of een onderstrepingsteken zijn.
    - **Fout met betrekking tot domeinen** - fout met betrekking tot domeinen definiëren de groep van virtuele machines die een gemeenschappelijke bron- en stopcontact delen. Standaard wordt het VMs worden gescheiden in drie fout met betrekking tot domeinen en kunnen worden gewijzigd in tussen 1 en 3.
    - **Domeinen werken** - vijf domeinen standaard en dit zijn toegewezen-update kan worden ingesteld op tussen 1 en 20. Update domeinen geven aan groepen van virtuele machines en de onderliggende fysieke hardware die tegelijkertijd kan worden gestart. Bijvoorbeeld, als we vijf domeinen worden bijgewerkt wanneer meer dan vijf virtuele machines worden geconfigureerd in een enkele Set beschikbaarheid opgeeft, de zesde virtuele machine wordt geplaatst in het domein bijwerken als de eerste virtuele machine, de zevende in de UD dezelfde als de tweede virtuele machine, enzovoort. De volgorde van de computer opnieuw hebt opgestart kan niet opeenvolgend worden genummerd, maar domein slechts één update zal opnieuw worden gestart, op een moment.
    - **Abonnement** - Selecteer het abonnement te gebruiken als u meer dan één hebben.
    - **Resourcegroep** - een bestaande resourcegroep selecteren door op de pijl te klikken en een groep in de vervolgkeuzelijst selecteren naar beneden. U kunt ook een nieuwe resourcegroep maken door een naam te typen. De naam mag de volgende tekens: letters, cijfers, punten, streepjes, onderstrepingstekens en opening of sluithaakje. De naam kan niet eindigen met een punt. Alle van de VMs in de beschikbaarheidsgroep moet worden gemaakt in dezelfde bronnengroep als de beschikbaarheid.
    - **Locatie** - Selecteer een locatie in de vervolgkeuzelijst.

4. Als u klaar bent in te voeren, klikt u op **maken**. Wanneer u de van beschikbaarheidsgroep hebt gemaakt, kunt u het bekijken in de lijst nadat u de portal vernieuwen.

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a>De portal te gebruiken voor het maken van een virtuele machine en een ingesteld op hetzelfde moment beschikbaarheid

Als u een nieuwe VM met behulp van de portal maken wilt, kunt u ook een nieuwe beschikbaarheid voor VM instellen tijdens het maken van de eerste VM in de set.

![Schermafdruk van het proces voor het maken van een nieuwe beschikbaarheid instellen tijdens het maken van de VM.](./media/virtual-machines-windows-create-availability-set/new-vm-avail-set.png)


## <a name="add-a-new-vm-to-an-existing-availability-set"></a>Een nieuwe VM toevoegen aan een bestaande set van beschikbaarheid

Voor elke extra VM die u maakt die in de groep moet behoren, moet u deze maken in dezelfde **bronnengroep** en selecteert u de bestaande beschikbaarheid ingesteld in stap 3. 

![Schermafdruk waarin wordt beschreven hoe u een bestaande beschikbaarheid die u wilt gebruiken om uw VM selecteren.](./media/virtual-machines-windows-create-availability-set/add-vm-to-set.png)



## <a name="use-powershell-to-create-an-availability-set"></a>Gebruik PowerShell om een set beschikbaar te maken

In dit voorbeeld wordt de beschikbaarheid in de resourcegroep **RMResGroup** ingesteld op de locatie **West VS** . Dit moet worden uitgevoerd voordat u de eerste VM in de set maken.

    New-AzureRmAvailabilitySet -ResourceGroupName "RMResGroup" -Name "AvailabilitySet03" -Location "West US"
    
Zie [Nieuw AzureRmAvailabilitySet](https://msdn.microsoft.com/library/mt619453.aspx)voor meer informatie.


## <a name="troubleshooting"></a>Het oplossen van problemen

- Bij het maken van een VM, als de gewenste beschikbaarheid set niet in de vervolgkeuzelijst in de portal kan u deze gemaakt in een andere bronnengroep bevinden. Als u niet weet, de groep voor de beschikbaarheid van bronnen instellen, Ga naar het menu van de hub en klikt u op Bladeren > beschikbaarheid wordt ingesteld voor een overzicht van de beschikbaarheid van sets en die resourcegroepen waartoe ze behoren.


## <a name="next-steps"></a>Volgende stappen

Extra opslagruimte toevoegen aan uw VM door een extra [schijf met gegevens](virtual-machines-windows-attach-disk-portal.md)toe te voegen.
