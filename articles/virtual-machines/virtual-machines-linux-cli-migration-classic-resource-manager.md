<properties
    pageTitle="IaaS resources migreren van klassiek Azure Resource Manager met behulp van de CLI Azure | Microsoft Azure"
    description="Dit artikel helpt bij de migratie platform ondersteund van bronnen van klassiek Azure Resource Manager met behulp van de CLI Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>IaaS resources migreren van klassiek Azure Resource Manager met behulp van de CLI Azure

Deze stappen laten zien hoe u Azure opdrachtregelinterface (CLI)-opdrachten gebruiken voor het migreren van infrastructuur als een (IaaS) serviceresources uit het implementatiemodel klassiek aan de bronnenbeheerder Azure implementatiemodel. Het artikel moet de [CLI Azure](../xplat-cli-install.md).

>[AZURE.NOTE] Alle hier beschreven handelingen zijn idempotency is ingeschakeld. Als er een probleem met een niet-ondersteunde functie of een fout in de configuratie, is het raadzaam dat u opnieuw het voorbereiden proberen, of bewerking afgebroken. Het platform wordt vervolgens probeer het opnieuw.

## <a name="step-1-prepare-for-migration"></a>Stap 1: Voorbereiden voor migratie

Hier volgen een paar tips aangeraden als u migreren IaaS resources uit klassieke Resource Manager te evalueren:

- Lees de [lijst met niet-ondersteunde configuraties of onderdelen](virtual-machines-windows-migration-classic-resource-manager.md). Als u virtuele machines die niet-ondersteunde configuraties of functies gebruiken, wordt u aangeraden wachten voor ondersteuning van de functie/configuratie worden aangekondigd. U kunt ook die functie verwijderen of verplaatsen van die configuratie migratie inschakelen als deze aan uw behoeften voldoet.
-   Als u hebben geautomatiseerde scripts die vandaag nog uw infrastructuur en toepassingen implementeren, kunt u een vergelijkbare instellingen te maken met behulp van deze scripts voor migratie. U kunt ook monster omgevingen instellen met behulp van de portal Azure.

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Stap 2: Stel uw abonnement en registreren van de provider

Migratiescenario's, moet u voor het instellen van uw omgeving voor zowel klassiek en Resource Manager. [Azure CLI installeren](../xplat-cli-install.md) en [Selecteer uw abonnement](../xplat-cli-connect.md).

Inloggen op uw account.
    
    azure login

Selecteer de Azure abonnement met de volgende opdracht.

    azure account set "<azure-subscription-name>"

>[AZURE.NOTE] Registratie is een tijdstap, maar moet één keer voordat u de migratie worden gedaan. Zonder te registreren ziet u het volgende foutbericht weergegeven 

>   *BadRequest: Abonnement is niet geregistreerd voor migratie.* 

Met de migratie resource provider registreren door de volgende opdracht. Houd er rekening mee dat in sommige gevallen wordt met deze opdracht een time-out. De registratie is gelukt.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Wacht vijf minuten om de inschrijving te voltooien. U kunt de status van de goedkeuring door de volgende opdracht controleren. Zorg ervoor dat RegistrationState `Registered` voordat u verdergaat.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Schakel nu over CLI om het `asm` modus.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Stap 3: Controleer of er voldoende cores Azure Resource Manager virtuele Machine in de regio van uw huidige distributie of VNET met Azure

Voor deze stap moet u overschakelen naar de `arm` modus. Dit doet u met de volgende opdracht.

```
azure config mode arm
```

Controleer het huidige cores u in Azure Resource Manager hebt kunt u de volgende opdracht in de CLI. Zie voor meer informatie over quota's core, [grenzen en de bronnenbeheerder Azure](../articles/azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Zodra u bent met klaar deze stap wordt gecontroleerd, kunt u overschakelen naar `asm` modus.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Stap 4: Optie 1 - migratie van virtuele machines in een cloud-service 

De lijst met cloud services ophalen met behulp van de volgende opdracht uit en kies vervolgens de cloud-service die u wilt migreren. Houd er rekening mee dat als de VMs in de cloud-service zich in een virtueel netwerk of als ze rollen web/werknemer hebben, u een foutbericht krijgt.

    azure service list

Voer de volgende opdracht om de naam van de implementatie van de cloud-service van de uitgebreide uitvoer. In de meeste gevallen is de naam van de implementatie hetzelfde als de naam van de wolk.

    azure service show <serviceName> -vv

De virtuele machines in de cloud service voor migratie voorbereiden. U hebben kunt kiezen uit twee opties.

Als u het VMs migreren naar virtueel netwerk platform gemaakt wilt, gebruikt u de volgende opdracht.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Als u migreren naar een bestaande virtuele netwerk in het implementatiemodel Resource Manager wilt, gebruikt u de volgende opdracht.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> subnetName <vnetName>

Nadat de voorbereidingsbewerking voltooid is, kunt u zoeken via de uitgebreide uitvoer de status van de migratie van de VMs en ervoor zorgen dat ze de `Prepared` staat.

    azure vm show <vmName> -vv

Controleer de configuratie voor de voorbereide resources met behulp van de CLI of de Azure portal. Als u niet gereed voor migratie zijn en u wilt teruggaan naar de oude staat, gebruikt u de volgende opdracht.

    azure service deployment abort-migration <serviceName> <deploymentName>

U kunt de vooraf gedefinieerde configuratie ziet er goed uit, vooruit en verbindt zich ertoe de resources door de volgende opdracht.

    azure service deployment commit-migration <serviceName> <deploymentName>


    
## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Stap 4: De optie 2 - migratie van virtuele machines in een virtueel netwerk

Kies het virtuele netwerk waarmee u wilt migreren. Houd er rekening mee dat als het virtuele netwerk web/werknemer rollen of VMs met niet-ondersteunde configuraties, ontvangt u een foutbericht voor gegevensvalidatie.

De virtuele netwerken krijgen in het abonnement met de volgende opdracht.

    azure network vnet list
    
De uitvoer ziet er ongeveer zo uitziet:

![Schermafbeelding van de opdrachtregel met de hele virtuele-netwerknaam gemarkeerd.](./media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

In het bovenstaande voorbeeld is de **virtualNetworkName** van de volledige naam **"Classicubuntu16 groep classicubuntu16"**.

Het virtuele netwerk van uw keuze voorbereiden op migratie met behulp van de volgende opdracht.

    azure network vnet prepare-migration <virtualNetworkName>

Controleer de configuratie voor de voorbereide virtuele machines met behulp van de CLI of de Azure portal. Als u niet gereed voor migratie zijn en u wilt teruggaan naar de oude staat, gebruikt u de volgende opdracht.

    azure network vnet abort-migration <virtualNetworkName>

U kunt de vooraf gedefinieerde configuratie ziet er goed uit, vooruit en verbindt zich ertoe de resources door de volgende opdracht.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Stap 5: Een opslag-account migreren

Zodra u bent met klaar de virtuele machines migreren, wordt aangeraden u de opslag account migreren.

De account opslag voorbereiden voor migratie door de volgende opdracht

    azure storage account prepare-migration <storageAccountName>

Controleer de configuratie voor de opslag van bereide rekening met CLI of de Azure portal. Als u niet gereed voor migratie zijn en u wilt teruggaan naar de oude staat, gebruikt u de volgende opdracht.

    azure storage account abort-migration <storageAccountName>

U kunt de vooraf gedefinieerde configuratie ziet er goed uit, vooruit en verbindt zich ertoe de resources door de volgende opdracht.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Volgende stappen

- [Ondersteund platform migratie van IaaS bronnen van klassiek tot Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md)
- [Technische diep kennismaking op platform ondersteund migratie van klassiek tot Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
