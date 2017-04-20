<properties
    pageTitle="Trend Micro diep Security op een VM installeren | Microsoft Azure"
    description="Dit artikel wordt beschreven hoe u kunt installeren en configureren van Trend Micro security op een VM gemaakt met het klassieke implementatiemodel in Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="iainfou"/>


# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Het installeren en configureren van diep Security van Trend Micro als een Service op een Windows VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

In dit artikel wordt beschreven hoe u installeren en configureren van Trend Micro diep beveiliging als een Service op een nieuwe of bestaande virtuele machine (VM) met Windows Server. Uitgebreide beveiliging als een Service bevat anti-malware bescherming, een firewall, een intrusion prevention system en integriteit controleren.

De client wordt geïnstalleerd als een extensie beveiliging via de VM-Agent. U installeert de VM-Agent met de uitgebreide beveiligings-Agent op een nieuwe virtuele machine. Op een bestaande virtuele machine die niet de VM-Agent, moet u downloaden en installeren eerst. Dit artikel heeft betrekking op beide situaties.

Als er bestaande abonnement van Trend Micro voor een oplossing op locatie, kunt u deze beschermen uw Azure virtuele machines. Als u niet een klant nog bent, kunt u zich aanmelden voor een proefabonnement. Zie de [Microsoft Azure VM Agent extensie voor uitgebreide beveiliging](http://go.microsoft.com/fwlink/p/?LinkId=403945)boeken Trend Micro-blog voor meer informatie over deze oplossing.

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>De uitgebreide beveiligings-Agent installeren op een nieuwe VM

De [Azure klassieke portal](http://manage.windowsazure.com) kunt u de VM-Agent en de uitbreiding van Trend Micro security installeren wanneer u de optie **Galerij uit** te maken van de virtuele machine. Als u een enkele virtuele machine maakt is met behulp van de portal een eenvoudige manier toe te voegen bescherming van Trend Micro.

Deze optie **Uit galerie** opent u een wizard voor het instellen van de virtuele machine. De laatste pagina van de wizard kunt u de VM-Agent en de Trend Micro security-extensie te installeren. Zie [virtuele machines maken waarop u Windows in Azure klassieke portal](virtual-machines-windows-classic-tutorial.md)voor algemene instructies. Wanneer u naar de laatste pagina van de wizard, het volgende doen:

1.  Controleer onder de **VM-Agent**, **VM-Agent installeren**.

2.  Controleer onder **Security Extensions** **Diep beveiligingsagent van Trend Micro**.

    ![De VM-Agent en de uitgebreide beveiligings-Agent installeren](./media/virtual-machines-windows-classic-install-trend/InstallVMAgentandTrend.png)

3.  Klik op het selectievakje om de virtuele machine maken.

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>De uitgebreide beveiligings-Agent installeren op een bestaande VM

De agent installeren op een bestaande VM, moet u het volgende:

- De module Azure PowerShell, versie 0.8.2 of recenter, op de lokale computer is geïnstalleerd. U kunt controleren welke versie van Azure PowerShell die u hebt geïnstalleerd met behulp van de **Get Module azure | tabel opmaken versie** opdracht. Zie voor instructies en een koppeling naar de nieuwste versie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Log in op uw Azure abonnement met behulp van `Add-AzureAccount`.

- De VM-Agent is geïnstalleerd op de virtuele machine van doel.

Controleer eerst of de Agent VM al is geïnstalleerd. Vul de naam van de service cloud en de naam van de virtuele machine en voert u de volgende opdrachten bij een opdrachtprompt beheerdersniveau Azure PowerShell. Vervang alles binnen de aanhalingstekens, met inbegrip van de < en > tekens.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Als u niet de cloud-service en de naam van de virtuele machine, **Get-AzureVM** als u wilt dat gegevens weergegeven voor de virtuele machines in uw huidige abonnement worden uitgevoerd.

Als de opdracht **write-host** **True**retourneert, wordt de VM-Agent wordt geïnstalleerd. Als **False**retourneert, raadpleegt u de instructies en een link naar de download in de Azure blog boeken [VM Agent en extensies - deel 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Als de VM-Agent is geïnstalleerd, moet u deze opdrachten uitvoeren.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen

Duurt het enkele minuten duren voordat de agent te starten tijdens de installatie wordt uitgevoerd. Daarna moet u uitgebreide beveiliging op de virtuele machine activeren zodat deze kan worden beheerd door een diepe Security Manager. Zie de volgende onderwerpen voor aanvullende instructies:

- Trend van artikel over deze oplossing, [Instant-On wolk Security voor Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
- Een [voorbeeld van Windows PowerShell-script](http://go.microsoft.com/fwlink/?LinkId=404100) voor het configureren van de virtuele machine
- [Instructies](http://go.microsoft.com/fwlink/?LinkId=404099) voor het monster

## <a name="additional-resources"></a>Aanvullende bronnen

[Hoe aan te melden op een virtuele machine met Windows Server]

[Functies en uitbreidingen voor Azure VM]


<!--Link references-->
[Hoe aan te melden op een virtuele machine met Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Functies en uitbreidingen voor Azure VM]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
