<properties
   pageTitle="Azure automatisering runbooks toevoegen aan de herstelplannen | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe Azure Site herstellen kunt u nu uitbreiden herstelplannen Azure automatisering met complexe taken uitvoeren tijdens het herstel naar Azure"
   services="site-recovery"
   documentationCenter=""
   authors="ruturaj"
   manager="gauravd"
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="powershell"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required"
   ms.date="10/23/2016"
   ms.author="ruturajd@microsoft.com"/>


# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Azure automatisering runbooks toevoegen aan de herstelplannen


In deze zelfstudie wordt beschreven hoe Azure-Site-Recovery is geïntegreerd met Azure automatisering voor herstelplannen uitbreidingsmogelijkheden. Herstelplannen kunnen evoluerende herstel van uw virtuele machines met Azure Site herstel voor secundaire cloud-replicatie en replicatie naar Azure scenario's beschermd. Ze helpen ook bij het maken van de recovery **accuraat**, **herhaalbare**en **geautomatiseerde**. Als u via uw virtuele machines naar Azure mislukken, integratie met Azure automatisering de herstelplannen breidt uit en geeft u de mogelijkheid om te runbooks, waardoor de krachtige automatiseringstaken uitvoeren.

Als u niet gehoord over Azure automatisering nog, [hier](https://azure.microsoft.com/services/automation/) aanmelden en de voorbeeldscripts downloaden [hier](https://azure.microsoft.com/documentation/scripts/). Lees meer over het [Herstel van Azure Site](https://azure.microsoft.com/services/site-recovery/) en het herstel naar Azure herstelplannen met goedkeuringen [hier](https://azure.microsoft.com/blog/?p=166264).

In deze zelfstudie, zullen we bekijken hoe u automatisering Azure runbooks in de herstelplannen integreren kunt. We eenvoudige taken automatiseren die eerder handmatige tussenkomst vereist en een herstelbewerking met meerdere stappen converteren naar een enkele klik herstelactie te zien. We zullen ook bekijken hoe u een eenvoudig script oplossen kunt als het mis gaat.

## <a name="customize-the-recovery-plan"></a>Aanpassen van het herstelplan

1. Laten we beginnen met operning het blad van de bron van het herstelplan. U ziet dat het herstelplan heeft twee virtuele machines voor herstel toegevoegd. 

    ![](media/site-recovery-runbook-automation-new/essentials-rp.PNG)
---------------------

2. Klik op de knop aanpassen om te beginnen met het toevoegen van een runbook. Hiermee opent u het herstelplan blade aanpassen.


    ![](media/site-recovery-runbook-automation-new/customize-rp.PNG)


3. Klik met de rechtermuisknop op de groep start 1 en selecteer 'Add post actie toevoegen '.

4. Selecteer een script te kiezen in het nieuwe blad.

5. De naam van het script 'Hello World'.

6. Kies een accountnaam automatisering. Dit is de rekening Azure automatisering. Houd er rekening mee dat deze account kan in de Azure Geografie maar moet het abonnement hetzelfde als de kluis Site herstellen.

7. Selecteer een runbook in de rekening van de automatisering. Dit is het script dat wordt uitgevoerd tijdens de uitvoering van het plan voor herstel na het herstel van de eerste groep.

    ![](media/site-recovery-runbook-automation-new/update-rp.PNG)


8. Klik op OK om het script opslaan. Hiermee wordt het script toegevoegd aan de actiegroep boeken van groep 1: Start.


    ![](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="salient-points-of-adding-a-script"></a>Belangrijke punten van een script toevoegen

1. U kunt een klik met de rechtermuisknop op het script en 'delete stap' of 'update script'.

2. Een script kan worden uitgevoerd op de Azure tijdens failover van On-premises tot Azure en als een primaire script voordat wordt afgesloten, tijdens de failback van Azure op ruimten op Azure kunt uitvoeren.

3. Wanneer een script wordt uitgevoerd, wordt er een context recovery plan invoeren.

Hieronder wordt een voorbeeld van hoe de variabele context eruit ziet.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


In de volgende tabel bevat een naam en beschrijving voor elke variabele in de context.

**Naam van variabele** | **Beschrijving**
---|---
RecoveryPlanName | De naam van het plan wordt uitgevoerd. Helpt u op basis van hetzelfde script met de naam
FailoverType | Hiermee geeft u aan of de failover is testen, of niet gepland.
FailoverDirection | Bepalen of herstel een primaire of secundaire
Groeps-id | Het groepsnummer binnen het herstelplan te identificeren wanneer het plan wordt uitgevoerd.
VmMap | Matrix van de virtuele machines in de groep
VMMap sleutel | Unieke sleutel (GUID) voor elke VM. Het is hetzelfde als de VMM-ID van de virtuele machine waar van toepassing.
Rolnaam | Naam van de Azure VM die worden hersteld
CloudServiceName | Azure Cloud Service naam waaronder de virtuele machine wordt gemaakt.
CloudServiceName (in de Resource Manager distributiemodel) | De naam Azure resourcegroep waaronder de virtuele machine wordt gemaakt.


## <a name="using-complex-variables-per-recovery-plan"></a>Met behulp van complexe variabelen per herstelplan

Een runbook moet soms meer informatie dan alleen de RecoveryPlanContext. Er is geen eerste-klas mechanisme voor een parameter doorgeven aan een runbook. Echter als u wilt gebruiken hetzelfde script via meerdere herstelplannen kunnen de Recovery Plan Context variabele 'RecoveryPlanName' en gebruik de onderstaande technieken gebruiken een Azure automatisering complex variabele in een runbook. In het volgende voorbeeld ziet u hoe u drie verschillende complexe variabele activa maken en deze gebruiken in de runbook op basis van de naam van het herstelplan.

U kunt dat 3 extra parameters gebruiken in een runbook. Laat het ons coderen in een formulier JSON {"Var1": "testautomation", "Var2": "Niet gepland", "Var3": "PrimaryToSecondary"}

[AA complexe variabele](../automation/automation-variables.md#variable-types Complex variable) gebruiken voor het maken van nieuwe activa automatisering.
Naam van de variabele als <RecoveryPlanName>- parameters.
U kunt hier de verwijzing voor het maken van een [complexe variabele](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396).

De variabele als een naam voor de verschillende herstelplannen,

1. recoveryPlanName1 >-parameters
2. recoveryPlanName2 >-parameters
3. recoveryPlanName3 >-parameters

Nu, verwijzen naar de parameters als in het script

1. De naam RP ophalen uit de $rpname = variabele $Recoveryplancontext
2. Ophalen van de activa van $paramValue = "$($rpname)-parameters"
3. Gebruik deze optie als een complexe variabele voor het herstelplan door het aanroepen van Get-AzureAutomationVariable [-AutomationAccountName] <String> -naam $paramValue.

Als u bijvoorbeeld als u de variabele complex/parameter voor het herstelplan SharepointApp, maken een Azure automatisering complexe variabele met de naam 'SharepointApp '-parameters.

Gebruiken in het herstelplan door de variabele te extraheren uit het actief gebruik van de instructie Get-AzureAutomationVariable [-AutomationAccountName] <String> [-naam] $paramValue. [Dit verwijst naar voor meer informatie](https://msdn.microsoft.com/library/dn913772.aspx)

Op deze manier hetzelfde script kan worden gebruikt voor verschillende herstelplan door het plan bepaalde complexe variabele op te slaan in de activa.

## <a name="sample-scripts"></a>Voorbeeldscripts

Zie voor een opslagplaats van scripts die u rechtstreeks in uw account voor de automatisering importeren kunt [van Kristian Nese OMS opslagplaats voor scripts](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/Solutions/asrautomation)

Hier het script is een bronnenbeheerder Azure sjabloon die alle implementeert de volgende scripts

* NSG

De NSG runbook wordt openbare IP-adressen toewijzen aan elke VM binnen het herstelplan en hun virtuele netwerkadapters te koppelen aan een beveiligingsgroep netwerk waardoor communicatie standaard

* PublicIP

Het openbare IP-runbook worden openbare IP-adressen toewijzen aan elke VM binnen het herstelplan. Toegang tot de machines en toepassingen hangt af van de firewall-instellingen in elke gast


* CustomScript

De runbook CustomScript wordt openbare IP-adressen toewijzen aan elke VM binnen het herstelplan en installeren van een aangepast script extensie die klikt, wordt het script dat u tijdens de implementatie van de sjabloon naar

* NSGwithCustomScript

De NSGwithCustomScript runbook openbare IP-adressen worden toegewezen aan elke VM binnen het herstelplan, installeren van een aangepast script met uitbreiding en verbinding maken met de virtuele netwerkadapters een NSG zodat binnenkomende en uitgaande communicatie voor RAS-standaard

## <a name="additional-resources"></a>Aanvullende bronnen

[Azure Automatiseringsservice worden uitgevoerd als de Account](../automation/automation-sec-configure-azure-runas-account.md)

[Overzicht automatisering Azure] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Overzicht automatisering Azure")

[Voorbeeldscripts Azure automatisering] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Voorbeeldscripts Azure automatisering")
