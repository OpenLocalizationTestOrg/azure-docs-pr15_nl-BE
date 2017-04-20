<properties
    pageTitle="Symantec Endpoint Protection op een VM installeren | Microsoft Azure"
    description="Informatie over het installeren en configureren van de uitbreiding van Symantec Endpoint Protection security op een nieuwe of bestaande Azure VM gemaakt met het klassieke implementatiemodel."
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

# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Het installeren en configureren van Symantec Endpoint Protection op een Windows VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

In dit artikel wordt beschreven hoe u installeren en configureren van de client voor Symantec Endpoint Protection op een bestaande virtuele machine (VM) met Windows Server. Dit is de volledige client, inclusief services zoals virussen en spyware bescherming, firewall en inbraak preventie. De client is geïnstalleerd als een uitbreiding van de beveiliging met behulp van de VM-Agent.

Hebt u een abonnement van Symantec voor een oplossing op locatie, kunt u het beschermen van uw Azure virtuele machines. Als u niet een klant nog bent, kunt u zich aanmelden voor een proefabonnement. Zie voor meer informatie over deze oplossing, [Symantec Endpoint Protection op Azure platform van Microsoft][Symantec]. Deze pagina bevat ook koppelingen naar licentiegegevens en instructies voor het installeren van de client als u al een klant van Symantec.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Symantec Endpoint Protection installeren op een bestaande VM

Voordat u begint, moet u het volgende:

- De module Azure PowerShell, versie 0.8.2 of hoger op uw werkcomputer. U kunt controleren welke versie van Azure PowerShell die u hebt geïnstalleerd met de **Get Module azure | tabel opmaken versie** opdracht. Zie voor instructies en een koppeling naar de nieuwste versie [installeren en configureren van Azure PowerShell][PS]. Log in op uw Azure abonnement met behulp van `Add-AzureAccount`.

- De VM-Agent op de Azure virtuele Machine wordt uitgevoerd.

Controleer eerst dat de Agent VM al is geïnstalleerd op de virtuele machine. Vul de naam van de service cloud en de naam van de virtuele machine en voert u de volgende opdrachten bij een opdrachtprompt beheerdersniveau Azure PowerShell. Vervang alles binnen de aanhalingstekens, met inbegrip van de < en > tekens.

> [AZURE.TIP] Als u niet de cloud-service en de namen van de virtuele machine, **Get-AzureVM** om de namen van alle virtuele machines in uw huidige abonnement worden uitgevoerd.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Als de opdracht **write-host** **True wordt**, wordt de VM-Agent wordt geïnstalleerd. Als **False**weergegeven, raadpleegt u de instructies en een link naar de download in de Azure blog boeken [VM Agent en extensies - deel 2][Agent].

Als de VM-Agent is geïnstalleerd, moet u deze opdrachten de Symantec Endpoint Protection-agent installeren uitvoeren.

    $Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

    Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

Controleren of de extensie van Symantec security is geïnstalleerd en up-to-date is:

1.  Meld u op de virtuele machine. Zie voor instructies [hoe u zich aanmeldt bij een virtuele Machine waarop Windows Server][Logon].
2.  Voor Windows Server 2008 R2, klikt u op **Start > Symantec Endpoint Protection**. Voor Windows Server 2012- of Windows Server 2012 R2 van het startscherm, typ **Symantec**en klik vervolgens op **Symantec Endpoint Protection**.
3.  Op het tabblad **Status** in het venster **Status Symantec Endpoint Protection** updates toe te passen of opnieuw opstarten indien nodig.

## <a name="additional-resources"></a>Aanvullende bronnen

[Hoe u zich aanmeldt bij een virtuele Machine met Windows Server][Logon]

[Functies en uitbreidingen voor Azure VM][Ext]


<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Portal]: http://manage.windowsazure.com

[Create]: virtual-machines-windows-classic-tutorial.md

[PS]: ../powershell-install-configure.md

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]: virtual-machines-windows-classic-connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493