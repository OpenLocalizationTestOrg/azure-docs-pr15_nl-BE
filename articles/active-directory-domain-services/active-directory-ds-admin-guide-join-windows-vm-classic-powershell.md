<properties
    pageTitle="Azure Active Directory Domain Services: Administration Guide | Microsoft Azure"
    description="Lid worden van een virtuele Windows-computer naar een beheerde domein met Azure PowerShell en het klassieke implementatiemodel."
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>


# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Toevoegen van een Windows Server virtuele machine aan een beheerde domein met PowerShell

> [AZURE.SELECTOR]
- [Klassieke Azure portal - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure heeft twee verschillende modellen voor het maken en werken met resources: [Resource Manager en klassiek](../resource-manager-deployment-model.md). In dit artikel beschreven hoe u met de klassieke implementatiemodel. Azure AD Domain Services ondersteunt momenteel niet het model Resource Manager.

Deze stappen laten zien hoe u een set van Azure PowerShell-opdrachten maken en vooraf een Windows Azure virtuele machine te configureren met behulp van een modulaire benadering aanpassen. Deze stappen kunt u een Windows Azure virtuele machine maken en toevoegen aan een beheerde Azure AD Domain Services-domein.

Deze stappen volgen een aanvullen-in-the-lege cellen benadering voor het maken van sets van Azure PowerShell-opdracht. Deze benadering is handig als u bekend met PowerShell bent of wilt u weten welke waarden op te geven voor een succesvolle configuratie. Geavanceerde PowerShell-gebruikers kunnen de opdrachten en vervangen door hun eigen waarden voor de variabelen (de regels die beginnen met "$").

Gebruik de instructies in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Azure PowerShell op uw lokale computer installeren als u dat nog niet hebt gedaan. Vervolgens opent u een opdrachtprompt van Windows PowerShell.

## <a name="step-1-add-your-account"></a>Stap 1: Uw account toevoegen

1. Bij de PowerShell-prompt **Toevoegen AzureAccount** en klik op **Enter**.
2. Typ in het e-mailadres dat is gekoppeld aan uw abonnement Azure en klik op **Doorgaan**.
3. Typ het wachtwoord voor uw account.
4. Klik op **aanmelden**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Stap 2: Uw abonnement en opslag-account instellen

Uw abonnement op Azure en opslag account instellen door het uitvoeren van deze opdrachten achter de opdrachtprompt van Windows PowerShell. Vervang alles binnen de aanhalingstekens, met inbegrip van de < en > tekens met de juiste namen.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Naam van het juiste abonnement krijgt u van de eigenschap SubscriptionName van de uitvoer van de opdracht **Get-AzureSubscription** . Nadat u de opdracht **Select AzureSubscription** uitgevoerd, kunt u de naam van de juiste opslag van de eigenschap Label van de uitvoer van de opdracht **Get-AzureStorageAccount** krijgen.


## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Stap 3: Stapsgewijze - inrichten van de virtuele machine en lid worden van het beheerde domein
Hier ziet u de bijbehorende Azure PowerShell-opdracht ingesteld op deze virtuele machine maken met lege regels tussen elk blok voor de leesbaarheid.

Geef informatie over de virtuele Windows-computer worden ingericht.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Zie voor de InstanceSize voor D-, DS- of G-serie virtuele machines, [virtuele Machine en Cloud Service formaten voor Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Bevatten informatie over de beheerde domein.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Geef de naam van de service cloud.

    $svcname="Contoso100-test"

Geef de naam van het virtuele netwerk waarmee de VM moet worden gekoppeld. Ervoor te zorgen dat de beheerde AAD-DS-domein beschikbaar in deze virtuele netwerk is.

    $vnetname="MyPreviewVnet"

Selecteer de afbeelding VM worden gebruikt voor het inrichten van de VM.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Configureer de VM - VM de naam, grootte instantie & afbeelding wordt gebruikt.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Referenties van lokale beheerder voor de VM downloaden. Kies een sterke lokale administrator-wachtwoord.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Referenties voor een gebruikersaccount van ' AAD DC-beheerdersgroep VM toevoegen aan de beheerde domein verkrijgen. Geef geen naam van het domein - bijvoorbeeld, in ons voorbeeld, we 'bob' opgeven als naam van de gebruiker.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Configureer de VM - domein join eis & vereiste referenties opgeven.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Een subnet voor de VM instellen.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Optioneel: Verwijzen naar het IP-adres van het domein. Als u de IP-adressen van het beheerde Azure AD Domain Services-domein naar de DNS-servers voor het virtuele netwerk worden ingesteld, is deze stap niet vereist.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Nu dient de VM Windows domein behoren.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Script voor een Windows VM inrichten en automatisch worden gekoppeld aan een beheerde AAD Domain Services-domein
Deze set PowerShell-opdracht maakt een virtuele machine voor een bedrijfs server die:

- De installatiekopie van Windows Server 2012 R2 Datacenter gebruikt.
- Is een extra kleine virtuele machine.
- Heeft de naam contoso-test.
- Automatisch is domein lid van de contoso100 beheerde domein.
- Hetzelfde virtuele netwerk als het beheerde domein wordt toegevoegd.

Hier is het volledige script voor het maken van de virtuele Windows-computer en automatisch worden gekoppeld aan de beheerde Azure AD Domain Services-domein.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Verwante inhoud
- [Azure AD Domain Services - handleiding aan de slag](./active-directory-ds-getting-started.md)

- [Een beheerde Azure AD Domain Services-domein beheren](./active-directory-ds-admin-guide-administer-domain.md)
