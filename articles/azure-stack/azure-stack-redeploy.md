<properties
    pageTitle="Azure stapel voor opnieuw lanceren | Microsoft Azure"
    description="Azure stapel voor opnieuw lanceren."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="erikje"/>

# <a name="redeploy-azure-stack"></a>Azure stapel voor opnieuw lanceren

Om Azure stapel voor opnieuw lanceren, moet u helemaal zoals hieronder beschreven.

## <a name="steps-to-redeploy-azure-stack"></a>Stappen voor het opnieuw implementeren Azure Stack

1. Start opnieuw op de host in het oorspronkelijke besturingssysteem (bare metal geïnstalleerd). Dit is niet de standaardinstelling in het opstartmenu zodat KVM- of lokale console moet worden gebruikt om deze te selecteren tijdens het opstarten (tijdens setup wordt u de "Boot uit VHD" OS te "AzureStack TP2" genoemd, op deze manier bepalen welke OS is).

    U hoeft niet te verwijderen van de bestaande opstartvermelding (het nieuwe script voor ondersteuning van "PrepareBootFromVHD.ps1" voor die voor u zorgt.)

2. Als u geen KVM of kiest het besturingssysteem opstarten wilt voordat de computer opnieuw opstart:
    
    1. Zoek het script.\BootMenuNoKVM.ps1. Dit bestand is beschikbaar met de andere scripts ondersteuning met deze build verstrekt.
    
    2. Het script wordt uitgevoerd met verhoogde bevoegdheden. Selecteer de naam van de oorspronkelijke Host OS. Dit wordt de host opstarten in de oorspronkelijke host OS zonder KVM-toegang.
    
    3. Wanneer het script voltooid is wordt u gevraagd te bevestigen van de computer wordt opgestart.

    - Als er andere gebruikers die zijn aangemeld, wordt deze opdracht mislukt.

    - Voer de volgende opdracht NET: Computer opnieuw opstarten-forceren 
 
3. Verwijder het bestand CloudBuilder.vhdx dat is gebruikt als onderdeel van de vorige installatie.

    U hoeft niet de bestaande toepassingen van de opslag van de vorige TP2-implementatie verwijderen. Het implementatiescript detecteert en opruimen van de bestaande en maakt vervolgens Nieuw.

5. Implementeren van het kopiëren van een nieuwe kopie van de CloudBuilder.vhdx, opstarten, enz.

## <a name="next-steps"></a>Volgende stappen

[Verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md)
