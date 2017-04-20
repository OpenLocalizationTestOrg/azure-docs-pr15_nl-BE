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


# <a name="add-azure-automation-runbooks-to-recovery-plans---classic"></a>Azure automatisering runbooks toevoegen aan de herstelplannen - klassiek


In deze zelfstudie wordt beschreven hoe Azure-Site-Recovery is geïntegreerd met Azure automatisering voor herstelplannen uitbreidingsmogelijkheden. Herstelplannen kunnen evoluerende herstel van uw virtuele machines met Azure Site herstel voor secundaire cloud-replicatie en replicatie naar Azure scenario's beschermd. Ze helpen ook bij het maken van de recovery **accuraat**, **herhaalbare**en **geautomatiseerde**. Als u via uw virtuele machines naar Azure mislukken, integratie met Azure automatisering de herstelplannen breidt uit en geeft u de mogelijkheid om te runbooks, waardoor de krachtige automatiseringstaken uitvoeren.

Als u niet gehoord over Azure automatisering nog, [hier](https://azure.microsoft.com/services/automation/) aanmelden en de voorbeeldscripts downloaden [hier](https://azure.microsoft.com/documentation/scripts/). Lees meer over het [Herstel van Azure Site](https://azure.microsoft.com/services/site-recovery/) en het herstel naar Azure herstelplannen met goedkeuringen [hier](https://azure.microsoft.com/blog/?p=166264).

In deze korte handleiding gaan we hoe u automatisering Azure runbooks in de herstelplannen integreren kunt. We eenvoudige taken automatiseren die eerder handmatige tussenkomst vereist is en hoe een multi-stap herstel omzetten in een enkele klik herstelactie. We zullen ook bekijken hoe u een eenvoudig script oplossen kunt als het mis gaat.

## <a name="protect-the-application-to-azure"></a>De toepassing op Azure beschermen

Laten we beginnen met een eenvoudige toepassing die bestaat uit twee virtuele machines. Hier hebben we een HRweb toepassing van Fabrikam. Zijn de twee virtuele machines naar Azure beveiligd met Azure Site herstel Fabrikam-HRweb-frontend en Fabrikam-Hrweb-backend. Volg de onderstaande stappen ter bescherming van de virtuele machines met Azure Site herstellen.

1.  Beveiliging voor uw virtuele machines inschakelen.

2.  Controleer of de virtuele machines eerste replicatie is voltooid en repliceert.

3.  Wacht totdat de eerste replicatie is voltooid en de replicatiestatus beveiligd zegt.

![](media/site-recovery-runbook-automation/01.png)
---------------------

In deze zelfstudie maken we een herstelplan voor de toepassing van de Fabrikam HRweb voor failover-de toepassing op Azure. Vervolgens zullen we integreren met een runbook die een eindpunt op de mislukte maakt via Azure virtuele machine voor webpagina's op poort 80.

Eerst maken we een plan voor herstel van de toepassing.

## <a name="create-the-recovery-plan"></a>Het herstelplan maken

Om te herstellen van de toepassing op Azure, moet u voor het maken van een herstelplan.
Met behulp van een herstelplan dat kunt u de volgorde van de terugwinning van de virtuele machines. De virtuele machine in de groep 1 geplaatst worden hersteld wordt eerst en vervolgens de virtuele machine in groep 2 volgen.

Maak een herstelplan dat eruit als hieronder ziet.

![](media/site-recovery-runbook-automation/12.png)

Meer lezen over herstelplannen, documentatie [hier](https://msdn.microsoft.com/library/azure/dn788799.aspx "hier").

Vervolgens maken we de nodige artefacten in Azure automatisering.

## <a name="create-the-automation-account-and-its-assets"></a>De rekening van de automatisering en haar activa maken

U moet een account Azure automatisering voor het maken van runbooks. Als u niet een account hebt nog, gaat u naar Azure automatisering tabblad aangeduid met ![](media/site-recovery-runbook-automation/02.png)en maak een nieuwe account.

1.  De account een naam aan geven.

2.  Geef een geografische regio waar u de rekening plaats.

Het wordt aanbevolen om de account in hetzelfde gebied, als de ASR-kluis.

![](media/site-recovery-runbook-automation/03.png)

Vervolgens maakt u de volgende elementen in de rekening.

### <a name="add-a-subscription-name-as-asset"></a>De naam van een abonnement toevoegen als actief

1.  Voeg een nieuwe instelling ![](media/site-recovery-runbook-automation/04.png) in de Azure automatisering activa en selecteer![](media/site-recovery-runbook-automation/05.png)

2.  Selecteer het type variabele als **tekenreeks**

3.  Geef de naam van variabele als **AzureSubscriptionName**

    ![](media/site-recovery-runbook-automation/06.png)

4.  Werkelijke naam van uw abonnement Azure als de waarde van de variabele opgeven.

    ![](media/site-recovery-runbook-automation/07_1.png)

U kunt de naam van uw abonnement op de instellingenpagina van uw account op de Azure portal identificeren.

### <a name="add-an-azure-login-credential-as-asset"></a>Toevoegen van een referentie Azure aanmelding als actief

Azure automatisering Azure PowerShell verbinding maakt met het abonnement en wordt toegepast op de artefacten er. Voor dit moet u verifiëren met behulp van uw Microsoft-account of een account voor werk of school.
U kunt de accountreferenties opslaan in activa veilig worden gebruikt door de runbook.

1.  Voeg een nieuwe instelling ![](media/site-recovery-runbook-automation/04.png) in Azure automatisering activa en selecteer![](media/site-recovery-runbook-automation/09.png)

2.  Selecteer het referentietype dat als **Referentie voor Windows PowerShell**

3.  Geef de naam op als **AzureCredential**

    ![](media/site-recovery-runbook-automation/10.png)

4.  Geef de gebruikersnaam en het wachtwoord voor het aanmelden bij.

Deze beide instellingen zijn nu beschikbaar in uw activa.

![](media/site-recovery-runbook-automation/11.png)

Meer informatie over verbinding maken met uw abonnement via PowerShell krijgt [hier](../powershell-install-configure.md).

Vervolgens maakt u een runbook in Azure automatisering die een eindpunt voor de front-virtuele machine na een failover kunnen toevoegen.

## <a name="azure-automation-context"></a>Azure automatisering context

ASR geeft een variabele context aan de runbook kunt u deterministic scripts schrijven. Een kan voerden aan dat de namen van de Cloud-Service en de virtuele Machine zijn voorspelbaar, maar dat het is niet altijd het geval als gevolg van bepaalde scenario's zoals die waar mogelijk de naam van de virtuele machine hebt gewijzigd van niet-ondersteunde tekens in Azure gebeurt. Dus wordt deze informatie doorgegeven aan het herstelplan ASR als onderdeel van de *context*.

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


Omgaan met de VmMap Key in de context kan u ook gaat u naar de eigenschappenpagina van VM in ASR en bekijkt u de eigenschap GUID VM.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>De auteur van een runbook automatisering

Nu de runbook om de open poort 80 op de front-end-virtuele machine maken.

1.  Maak een nieuwe runbook in de Azure automatisering rekening met de naam **OpenPort80**

    ![](media/site-recovery-runbook-automation/14.png)

2.  Ga naar de weergave van de auteur van het runbook en voer de conceptmodus.

3.  Eerst geeft u de variabele te gebruiken als de context van herstel plan

    ```
        param (
            [Object]$RecoveryPlanContext
        )

    ```

4.  Volgende keer bij het abonnement met de naam van de referentie- en abonnement

    ```
        $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

        # Connect to Azure
        $AzureAccount = Add-AzureAccount -Credential $Cred
        $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
        Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    ```

    Houd er rekening mee dat u de Azure activa – **AzureCredential** en **AzureSubscriptionName** is hier gebruikt.

5.  Geef nu de eindpuntdetails en de GUID van de virtuele machine die u wilt weergeven van het eindpunt. In dit geval de front-virtuele machine.

    ```
        # Specify the parameters to be used by the script
        $AEProtocol = "TCP"
        $AELocalPort = 80
        $AEPublicPort = 80
        $AEName = "Port 80 for HTTP"
        $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
    ```

    Hiermee geeft u het protocol Azure eindpunt, lokale poort op de VM en openbare poort toegewezen. Deze variabelen zijn parameters vereist door de eindpunten aan VMs toevoegen Azure opdrachten. De VMGUID bevat de GUID van de virtuele machine die u nodig hebt om te rijden op.

6.  Het script wordt nu pakken de context voor de opgegeven VM GUID en een eindpunt op de virtuele machine waarnaar wordt verwezen door het maken.

    ```
        #Read the VM GUID from the context
        $VM = $RecoveryPlanContext.VmMap.$VMGUID

        if ($VM -ne $null)
        {
            # Invoke pipeline commands within an InlineScript

            $EndpointStatus = InlineScript {
                # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
                # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

                $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                    Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                    Update-AzureVM
                Write-Output $Status
            }
        }
    ```

7. Zodra dit voltooid is, klik op publiceren ![](media/site-recovery-runbook-automation/20.png) om het script beschikbaar voor uitvoering.

Het volledige script worden hieronder gegeven voor uw referentie

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a>Het script toevoegen aan het herstelplan

Zodra het script klaar is, kunt u deze toevoegen aan het herstelplan dat u eerder hebt gemaakt.

1.  In het herstelplan dat u hebt gemaakt, kiest u een script toevoegen nadat de groep 2. ![](media/site-recovery-runbook-automation/15.png)

2.  Geef de scriptnaam van een. Dit is gewoon een beschrijvende naam voor dit script wordt weergegeven in het herstelplan.

3.  Selecteer de accountnaam Azure automatisering in failover naar Azure script –.

4.  Selecteer de runbook die u hebt samengesteld in de Runbooks Azure.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Scripts op de primaire

Wanneer u een failover naar Azure worden uitgevoerd, kunt u ook voor het uitvoeren van scripts op de primaire. Deze scripts worden uitgevoerd op de server VMM tijdens failover.
Scripts op de primaire alleen zijn alleen beschikbaar voor voorlopige afsluiten en boek fasen afsluiten. Dit komt omdat we verwachten dat de primaire site meestal niet beschikbaar zijn wanneer een ramp biedt.
Tijdens een niet-geplande failover alleen als u zich voor primaire bewerkingen uitschrijft, wordt geprobeerd de primaire zijde scripts worden uitgevoerd. Als ze niet bereikbaar zijn of time-out, de failover-functie blijft de virtuele machines te herstellen.
Scripts op de primaire zijn niet beschikbaar voor Sites van VMware/fysieke/Hyper-v zonder VMM beveiligd naar Azure - tijdens een failover naar Azure.
Echter, wanneer u failback van Azure op lokalen, scripts op primaire (Runbooks) kan worden gebruikt voor alle doelen, met uitzondering van VMware.

## <a name="test-the-recovery-plan"></a>Testen van het herstelplan

Als u de runbook hebt toegevoegd aan het plan kunt u een failover voor test- en de praktijk. Het verdient aanbeveling altijd een failover testen om te testen het herstelplan om ervoor te zorgen dat er geen fouten zijn en de toepassing uitvoeren.

1.  Selecteer het herstelplan en een test failover.

2.  Tijdens de uitvoering van het plan, kunt u zien of de runbook is uitgevoerd of niet via de status ervan.

    ![](media/site-recovery-runbook-automation/17.png)

3.  Ook ziet u de status van de uitvoering van gedetailleerde runbook op de pagina taken van Azure automatisering voor de runbook.

    ![](media/site-recovery-runbook-automation/18.png)

4.  Nadat de failover is voltooid, met uitzondering van het resultaat van de uitvoering van runbook kunt u de uitvoering is geslaagd of niet door de Azure VM pagina bezoeken en kijken naar de eindpunten bekijken.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Voorbeeldscripts

Terwijl we doorlopen kan een eindpunt toe te voegen aan een Azure virtuele machine in deze zelfstudie automatiseren een vaak gebruikt, u een aantal andere krachtige automatiseringstaken Azure automatisering gebruiken. Microsoft en de Gemeenschap Azure automatisering bieden monster runbooks waarmee u aan de slag maken van uw eigen oplossingen en het hulpprogramma runbooks, die u kunt gebruiken als bouwstenen voor grotere automatiseringstaken. Ze gebruiken in de galerie en bouwen van krachtige één klik herstelplannen voor uw toepassingen met Azure Site herstellen.

## <a name="additional-resources"></a>Aanvullende bronnen

[Overzicht automatisering Azure] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Overzicht automatisering Azure")

[Voorbeeldscripts Azure automatisering] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Voorbeeldscripts Azure automatisering")
