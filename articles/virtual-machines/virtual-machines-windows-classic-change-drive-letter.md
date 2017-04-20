<properties
    pageTitle="Het station D: maken van een schijf met gegevens van een VM | Microsoft Azure"
    description="Wordt beschreven hoe u stationsletters voor een Windows VM zodanig wijzigen dat u het station D: als een schijf met gegevens kunt gebruiken."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Het station D: als een gegevensstation van een Windows VM gebruiken 

Als uw toepassing het station D gebruiken moet voor het opslaan van gegevens, volg deze instructies om het gebruik van een andere stationsletter voor de tijdelijke schijf. Gebruik nooit de tijdelijke schijf voor het opslaan van gegevens die u wilt bewaren.

Als u de grootte van of **Stop (Deallocate)** een virtuele machine, kan dit de positie van de virtuele machine naar een nieuwe hypervisor starten. Deze plaatsing kan ook starten en een gebeurtenis of niet gepland onderhoud. In dit scenario wordt de tijdelijke schijf naar de eerste beschikbare stationsletter toegewezen. Als u een toepassing die specifiek het station D: hebt vereist, moet u deze als volgt tijdelijk pagefile.sys verplaatsen, een nieuwe gegevensschijf toegevoegd en hieraan de letter D en verplaatsen pagefile.sys terug op de tijdelijke schijf. Zodra voltooid, Azure niet terug te nemen de D: als VM wordt verplaatst naar een ander hypervisor.

Zie voor meer informatie over de manier waarop de tijdelijke schijf wordt gebruikt in Azure, [Wat de tijdelijke schijf op Microsoft Azure virtuele Machines?](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="attach-the-data-disk"></a>De gegevensschijf koppelen

Eerst moet u de gegevensschijf toevoegen aan de virtuele machine. 

- Zie voor het gebruik van de portal voor [het koppelen van een schijf met gegevens in de portal voor Azure](virtual-machines-windows-attach-disk-portal.md)
- Zie voor het gebruik van de klassieke portal, [het koppelen van een schijf met gegevens naar een virtuele Windows-computer](virtual-machines-windows-classic-attach-disk.md). 


## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Tijdelijk verplaatsen pagefile.sys naar station C

1. Verbinding maken met de virtuele machine. 

2. Klik met de rechtermuisknop op het menu **Start** en selecteer **systeem**.

3. Selecteer in het menu links **Geavanceerde systeeminstellingen**.

4. Selecteer **Instellingen**in het gedeelte **prestaties** .

5. Selecteer het tabblad **Geavanceerd** .

5. Selecteer in het gedeelte **virtueel geheugen** **wijzigen**.

6. Selecteer het station **C** en klik vervolgens op **systeem beheerde grootte** en klik op **instellen**.

7. Selecteer het station **D** en klikt u op **geen wisselbestand** en klik vervolgens op **instellen**.

8. Klik op toepassen. U krijgt een waarschuwing dat de computer moet opnieuw worden opgestart om de wijzigingen door te voeren.

9. Start de virtuele machine.




## <a name="change-the-drive-letters"></a>De stationsletters wijzigen 

1. Zodra de VM opnieuw is opgestart, meldt u zich weer aan bij de VM.

2. Klik in het menu **Start** en typ **diskmgmt.msc** en druk op Enter. Schijfbeheer wordt gestart.

3. Klik met de rechtermuisknop op **D**, de tijdelijke opslag-station en selecteer **wijzigen stationsletter en paden**.

4. Onder de letter van het station, station **G** selecteren en klik op **OK**. 

5. Klik met de rechtermuisknop op de gegevensschijf en selecteer **wijzigen stationsletter en paden**.

6. Selecteer station **D** onder de letter van het station en klik op **OK**. 

7. Klik met de rechtermuisknop op **G**, de tijdelijke opslag-station en selecteer **wijzigen stationsletter en paden**.

8. Onder de letter van het station, station **E** selecteren en klik op **OK**. 

> [AZURE.NOTE] Als uw VM andere schijven of -stations heeft, dezelfde methode gebruiken om de stationsletters van de andere schijven en stations. Wilt u de schijfconfiguratie te zijn:  
>- C: OS-schijf  
>- D: gegevens van de schijf  
>- E: tijdelijke schijf



## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Terug naar het station voor de tijdelijke opslag pagefile.sys 

1. Klik met de rechtermuisknop op het menu **Start** en selecteer **systeem**

2. Selecteer in het menu links **Geavanceerde systeeminstellingen**.

3. Selecteer **Instellingen**in het gedeelte **prestaties** .

4. Selecteer het tabblad **Geavanceerd** .

5. Selecteer in het gedeelte **virtueel geheugen** **wijzigen**.

6. Selecteer het OS station **C** en klikt u op **geen wisselbestand** en klik op **instellen**.

7. Selecteer de tijdelijke station **E** , klik op **systeem beheerde grootte** en klik vervolgens op **instellen**.

8. Klik op **toepassen**. U krijgt een waarschuwing dat de computer moet opnieuw worden opgestart om de wijzigingen door te voeren.

9. Start de virtuele machine.




## <a name="next-steps"></a>Volgende stappen
- Door het [toevoegen van een schijf met aanvullende gegevens](virtual-machines-windows-attach-disk-portal.md)kunt u de beschikbare opslagruimte op uw virtuele machine verhogen.



