<properties
    pageTitle="Probleemoplossing Microsoft Azure Stack | Microsoft Azure"
    description="Azure Stack het oplossen van problemen."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="microsoft-azure-stack-troubleshooting"></a>Probleemoplossing Microsoft Azure Stack

Dit document bevat algemene informatie over probleemoplossing voor Azure Stack.

Zorg ervoor dat uw werkomgeving voldoet aan alle [vereisten](azure-stack-deploy.md) en [preparaten](azure-stack-run-powershell-script.md) voor de installatie voor het beste resultaat. 

De aanbevelingen voor het oplossen van problemen die worden beschreven in deze sectie worden afgeleid uit verschillende bronnen en kunnen of uw probleem mogelijk niet opgelost. Als er een probleem is niet gedocumenteerd, Controleer de [Azure Stack MSDN Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) voor meer informatie en verdere ondersteuning.

Voorbeelden van programmacode worden geleverd in de huidige staat en verwachte resultaten niet worden gegarandeerd. Deze sectie kan veelvuldige wijzigingen en updates zoals verbeteringen aan het product worden geïmplementeerd.

  

## <a name="known-issues"></a>Bekende problemen

 - Ziet u de volgende-terminating fouten tijdens de implementatie, waardoor implementatie geslaagd:
     - 'De term 'C:\WinRM\Start-Logging.ps1' wordt niet herkend'
     - "Aanroepen van EceAction: kan niet worden geïndexeerd in een null-matrix" 
     - "InvokeEceAction: argument kan niet binden aan de parameter 'Bericht' omdat het een lege tekenreeks is."
 - Ziet u implementatie mislukt bij stap 60.61.93 met een fout 'Application met identifier URI niet gevonden'. Dit probleem is door de wijze waarop toepassingen worden geregistreerd in Azure Active Directory.  Als u dit foutbericht ontvangt, blijven [het installatiescript opnieuw uitvoeren](azure-stack-rerun-deploy.md) van stap 60.61.93 tot implementatie voltooid is.
 - Ziet u dat de resource **Beschikbaar** in de markt wordt weergegeven onder de categorie **virtualMachine ARM** – deze weergave alleen een cosmetische kwestie is.
 - Wanneer u een nieuwe virtuele machine maken in de portal in de stap van de **Grondbeginselen van** standaard de opslagoptie SSD.  Deze instelling moet worden gewijzigd naar de harde schijf of op de stap **grootte** van VM implementatie, VM formaten beschikbaar te selecteren en de implementatie verder wordt niet weergegeven. 
 - Ziet u AzureRM PowerShell-modules zijn niet meer standaard geïnstalleerd op de VM MAS CON01 (in TP1 dit is de naam ClientVM). Dit gedrag is inherent aan het ontwerp, omdat er een alternatieve methode om [deze modules te installeren en aansluiten](azure-stack-connect-powershell.md).  
 - Ziet u dat de resource **Microsoft.Insights** provider is niet automatisch geregistreerd voor huurder abonnementen. Als u zien van de controle van gegevens voor een VM geïmplementeerd als een huurder wilt, de volgende opdracht uitvoeren vanuit PowerShell (nadat u [hebt geïnstalleerd en verbinding maken](azure-stack-connect-powershell.md) als een huurder): 
       
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights 

 - Worden er functionaliteit in de portal voor resourcegroepen, exporteren, maar geen tekst weergegeven als beschikbaar voor exporteren wordt.      
 - U kunt een implementatie van de opslagmiddelen die groter zijn dan de beschikbare quota op te starten.  Deze installatie is mislukt en de middelen rekening zal worden geschorst.  Er zijn twee opties voor herstel:
     - Service-beheerder kan het quotum, verhogen al wijzigingen worden niet onmiddellijk van kracht en meestal duren een uur doorgeven.
     - Service-beheerder kan een invoegtoepassing plan maken met extra contingent van de huurder vervolgens aan het abonnement toevoegen kunt.
 - Wanneer u de portal VMs maken op stapel Azure omgevingen met identiteit in "Azure - China", u VM formaten beschikbaar om te selecteren in de stap van de **grootte** van de VM-implementatie wordt niet weergegeven en kan gaan implementeren.
 - Wordt er een fout bij implementatie in de portal, wanneer de VM daadwerkelijk heeft geïmplementeerd.
 - Wanneer u een plan, abonnement of aanbieding verwijderen, kunnen VMs niet worden verwijderd.
 - U ziet de VM-extensies op de markt.
 - U kunt een VM uit een opgeslagen VM-afbeelding niet implementeren.
 - Huurders kunnen Zie services die niet zijn opgenomen in hun abonnement.  Wanneer huurders voor de implementatie van deze bronnen, ontvangen zij een fout.  Voorbeeld: De huurder abonnement bevat alleen opslagbronnen.  Optie voor het maken van andere bronnen zoals VMs ziet huurder.  In dit scenario als een huurder geprobeerd te implementeren een VM, ontvangen ze een bericht met dat de VM kan niet worden gemaakt. 
 - Wanneer u TP2 installeert, moet u de host OS op de VHD waar u het script Azure Stack setup uitvoert of u mogelijk een foutbericht volstaan met de vermelding Windows messaging binnenkort verloopt, niet geactiveerd.


## <a name="deployment"></a>Implementatie

### <a name="deployment-failure"></a>Fout bij implementatie
Als u problemen tijdens de installatie ondervindt, wordt het installatieprogramma Azure stapel kunt u doorgaan met een mislukte installatie door de [implementatiestappen opnieuw uit te voeren](azure-stack-rerun-deploy.md).

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Aan het einde van de implementatie van de PowerShell-sessie is nog geopend en alle uitvoer wordt niet weergegeven

Dit probleem is waarschijnlijk slechts het resultaat van het standaardgedrag van een opdrachtvenster PowerShell wanneer deze is geselecteerd. De Haalbaarheidstest implementatie daadwerkelijk is gelukt, maar het script is onderbroken bij het selecteren van het venster. U kunt controleren of dat dit het geval is door te zoeken naar het woord "selecteren" in de titelbalk van het venster.  Druk op ESC om deze te deselecteren en het voltooiingsbericht moet worden weergegeven na het.

## <a name="templates"></a>Sjablonen

### <a name="azure-template-wont-deploy-to-azure-stack"></a>Azure sjabloon won't naar Azure Stack implementeren

Zorg ervoor dat:

- De sjabloon moet gebruikmaken van een Microsoft Azure service die nog beschikbaar zijn of in voorbeeld in Azure Stack.
- De API's voor een bepaalde resource wordt gebruikt door het lokale exemplaar van de Stack Azure worden ondersteund en dat u een geldige locatie ("local" in Azure Stack technische Preview (TP) 2 vs de 'Oost-Nederland' of 'Zuid-India"in Azure) zijn gericht.
- U controleren [in dit artikel](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) over de Test-AzureRmResourceGroupDeployment-cmdlets die werkelijk kleine verschillen in Azure Resource Manager-syntaxis.

Kun je de al opgegeven in de [opslagplaats van GitHub](http://aka.ms/AzureStackGitHub/) Azure Stack-sjablonen kunt u aan de slag.

## <a name="virtual-machines"></a>Virtuele machines

### <a name="after-starting-my-microsoft-azure-stack-poc-host-all-my-tenants-vms-are-gone-from-hyper-v-manager-and-come-back-automatically-after-waiting-a-bit"></a>Na het starten van mijn Microsoft Azure Stack Implementatiemodel host zijn alle mijn huurders VMs verdwenen uit de Hyper-V-beheer en terugkomen automatisch na een beetje wachten?

Als het systeem van het subsysteem voor opslag en RPs moet worden vastgesteld van consistentie terugkomt. De tijd die nodig is afhankelijk van de hardware en specificaties wordt gebruikt kan, maar het enige tijd na het opnieuw opstarten van de host voor de huurder VMs terugkomen en worden herkend.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Ik sommige virtuele machines hebt verwijderd, maar nog steeds de VHD-bestanden op de schijf. Wordt verwacht dat dit probleem?

Ja, is dit gedrag verwacht. Het is zo ontworpen omdat:

- Wanneer u een VM verwijdert, worden de VHD's niet verwijderd. Schijven zijn afzonderlijke resources in de resourcegroep.
- Wanneer u een opslag-account wordt verwijderd, de verwijdering is zichtbaar onmiddellijk via Azure Resource Manager (portal PowerShell) maar de schijven die komen er nog steeds in opslag worden bewaard totdat de garbage collector wordt uitgevoerd.

Als u VHD's 'regels' ziet, is het belangrijk om te weten als ze deel uitmaken van de map voor de account van een opslag die is verwijderd. Als de opslag-account kan niet worden verwijderd, is het normaal bent.

Meer informatie over het configureren van de drempel voor behoud in [opslag beheren](azure-stack-manage-storage-accounts.md).

## <a name="installation-steps"></a>Installatiestappen
De volgende informatie over Azure Stack installatiestappen kan nuttig zijn voor het oplossen van problemen:  

| Index | Naam | Beschrijving|
| ----- | ----- | -----|
|0.11 | (DEP) Fysieke Machines valideren | Valideren van de hardware en configuratie van het besturingssysteem op de fysieke knooppunten. |
| 0,12 | (DEP) Netwerk voor Haalbaarheidstest fysieke Machines configureren | Configureren van virtuele netwerk-switches en interfaces. |
| 0.14 | (DEP) Domein implementeren | Implementatie van Active Directory op een virtuele Machine. |
| 0,15 | (DEP) De domein-server configureren | Domeinserver configureren met beveiligingsgroepen enz. |
| 0.16 | (DEP) Fysieke Machine configureren | Netwerken, lid worden van domein en lokale beheerders instellingen configureren. |
| 0.18 | (STO) Opslag, Cluster configureren | Opslag, cluster maakt, maakt u een opslagserver en de bestandsnaam. |
| 0.19 | (KPI) Setup fabric-infrastructuur | Instellen van de vereisten voor de implementatie van het weefsel. |
| 0.21 | (NETTO) BGP en NAT tijdens Setup | BGP en NAT - alleen nodig voor een knooppunt installeert. |
| 0.22 | (NETTO) NAT en tijdserver configureren | De tijdserver wordt gesynchroniseerd en configureert u NAT-posten. |
| 40.41 | (KPI) Gast VMs maken | Maak het VMs-beheer. |
| 40.42 | (FBI) PowerShell JEA instellen | PowerShell JEA ingesteld voor alle rollen. |
| 40.43 | (FBI) Azure Stack certificeringsinstantie instellen | Azure Stack certificeringsinstantie installeert. |
| 40.44 | (FBI) Azure Stack certificeringsinstantie configureren | Hiermee configureert u Azure Stack certificeringsinstantie. |
| 40.45 | (NETTO) NC op VMs instellen | NC installeert op de Gast VMs |
| 40.46 | (NETTO) NC op VMs configureren | NC configureren op de Gast VMs |
| 40.47 | (NETTO) Gast VMs configureren | Het beheer van VMs met NC ACL's configureren. |
| 60.61.81 | (FBI) Stack Azure Ring configuratieservices - vereiste FabricRing implementeren | VIP's voor FabricRing wordt gemaakt |
| 60.61.82 | (FBI) Azure Stack Fabric Ring-Services implementeren - Fabric Ring Cluster implementeren | Geïnstalleerd en geconfigureerd Cluster Azure Stack Fabric-Ring. |
| 60.61.83 | (FBI) Admin-serverextensies implementeren voor Resource-providers | Admin-serverextensies installeren voor resource-providers |
| 60.61.84 | (ACS) Azure consistent opslag in niveau instellen. | Installeert en configureert Azure consistent opslag in niveau. |
| 60.61.85 | (ACS) Azure consistent opslag in een clusterniveau instellen. | Installeert en configureert Azure consistent opslag in een clusterniveau. |
| 60.61.86 | (FBI) Stack Azure Ring-Controller configuratieservices - vereiste implementeren | Vereisten voor InfraServiceController |
| 60.61.87 | (FBI) Stack Azure Ring-Controller configuratieservices - vereiste implementeren | Vereisten voor de CPI |
| 60.61.88 | (FBI) Stack Azure Ring-Controller configuratieservices - vereiste implementeren | Vereisten voor ASAppGateway |
| 60.61.89 | (FBI) Stack Azure Ring-Controller configuratieservices - vereiste implementeren | Vereisten voor een opslagcontroller |
| 60.61.90 | (FBI) Stack Azure Ring-Controller configuratieservices - vereiste implementeren | Vereisten voor HealthMonitoring |
| 60.61.91 | (FBI) Stack Azure Ring-Controller configuratieservices - vereiste implementeren | Vereisten voor het ECE |
| 60.61.92 | (FBI) Stack Azure Ring-Controller configuratieservices - vereiste implementeren | Vereisten voor PMM |
| 60.61.93 | (Katal) AzureStack-Service beveiligings-Principals maken | Azure Graph toepassingen en Service-Principals in AAD maken. |
| 60.61.94 | (NETTO) Setup GW VMs | GW installeert op de Gast VMs. |
| 60.61.95 | (NETTO) GW VMs configureren | GW configureert voor het VMs Gast. |
| 60.61.96 | (NETTO) Implementeren van IDN's op hosts | IDN op hosts infrastructuur implementeren |
| 60.61.97 | (NETTO) IDN's configureren | Rol van IDN's configureren |
| 60.61.98 | (FBI) Setup van WSUS VMs | WSUS-server installeert op de Gast VMs. |
| 60.61.99 | (FBI) VMs WSUS configureren | WSUS-server configureert voor het VMs Gast. |
| 60.61.100 | (FBI) VMs Azure SQL Setup | Azure SQL server wordt geïnstalleerd op de Gast VMs |
| 60.61.101 | (Katal) Setup-vereisten voor het VMs is. | De vereisten voor Microsoft Azure-Stack op de Gast VMs ingesteld. |
| 60.61.102 | (Katal) Setup is VMs | Installeert Microsoft Azure-Stack op de Gast VMs. |
| 60.120.121 | (FBI) Resource-providers en domeincontrollers implementeren | Resource-providers en domeincontrollers worden geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en domeincontrollers implementeren | Resource-providers en domeincontrollers worden geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en domeincontrollers implementeren | Resource-providers en domeincontrollers worden geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en domeincontrollers implementeren | Resource-providers en domeincontrollers worden geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en domeincontrollers implementeren | Resource-providers en domeincontrollers worden geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en domeincontrollers implementeren | Resource-providers en domeincontrollers worden geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en domeincontrollers implementeren | Resource-providers en domeincontrollers worden geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en domeincontrollers implementeren | Resource-providers en domeincontrollers worden geïnstalleerd |
| 60.120.122 | (FBI) Configuratie van domeincontroller | Domeincontrollers configureren |
| 60.120.123 | (Katal) Configureer WAS VMs | Hiermee configureert u Microsoft Azure Stack op de Gast VMs. |
| 60.120.124 | (Katal) Configuratie van Stack Azure AAD. | Hiermee configureert u Azure Stack met Azure Active Directory. |
| 60.120.125 | (Katal) AD FS installeren | Hiermee installeert u Active Directory Federation Services (ADFS) |
| 60.120.126 | (Katal) Installeer AD FS/grafiek | Azure Stack Graph worden geïnstalleerd |
| 60.120.127 | (Katal) AD FS configureren | Hiermee configureert u Active Directory Federation Services (ADFS) |
| 60.140.141 | (FBI) SRP configureren | Storage Resource Provider configureren |
| 60.140.142 | (ACS) Azure consistent opslag configureren. | Hiermee configureert u consistente Azure opslag. |
| 60.140.143 | (FBI) Opslag-Accounts maken | Maak alle opslag rekeningen door andere providers worden gebruikt. |
| 60.140.144 | (FBI) Registratie voor SRP | Registratie voor Storage Provider. |
| 60.140.145 | (KPI) Gemaakte VMs, Hosts en Cluster migreren naar CPI | Objecten van de gemaakte VMs, Hosts en Cluster migreert naar CPI |
| 60.140.146 | (FBI) Configureren van Windows Defender | Hiermee configureert u Windows Defender |
| 60.160.161 | (MA) Monitoring Agent configureren | Monitoring Agent configureren |
| 60.160.162 | (FBI) Vereiste NRP | NRP vereisten worden geïnstalleerd |
| 60.160.163 | (FBI) NRP-implementatie | NRP worden geïnstalleerd  |
| 60.160.164 | (FBI) NRP configuratie | Hiermee configureert u de NRP |
| 60.160.165 | (FBI) Vereiste CAPPL | CAPPL voorwaarden worden geïnstalleerd |
| 60.160.166 | (FBI) Implementatie CAPPL | CAPPL geïnstalleerd |
| 60.160.167 | (FBI) CAPPL configuratie | CAPPL configureren |
| 60.160.168 | (FBI) Vereiste FRP | Hiermee installeert u FRP-vereisten |
| 60.160.169 | (FBI) FRP-implementatie | Hiermee installeert u FRP  |
| 60.160.170 | (FBI) FRP configuratie | Hiermee configureert u FRP |
| 60.160.174 | (FBI) Vereiste URP | URP vereisten worden geïnstalleerd |
| 60.160.175 | (FBI) URP-implementatie | Hiermee installeert u URP  |
| 60.160.176 | (FBI) URP configuratie | Hiermee configureert u URP |
| 60.160.171 | (FBI) Vereiste HRP | Hiermee installeert u HRP-vereisten |
| 60.160.172 | (FBI) HRP-implementatie | Hiermee installeert u HRP  |
| 60.160.173 | (FBI) HRP-configuratie | Hiermee configureert u HRP |
| 60.160.177 | (KV) Vereiste KeyVault | Vereisten voor installatie van KeyVault |
| 60.160.178 | (KV) Implementatie KeyVault | KeyVault geïnstalleerd  |
| 60.160.179 | (KV) KeyVault configuratie | KeyVault configureren | 
| 60.190.191 | (FBI) Galerie configureren | Galerie configureren |
| 60.190.192 | (FBI) Fabric Ring-Services configureren | Fabric Ring-Services configureren |
| 60.221 | (FBI) Setup-Console VMs | Console-server wordt geïnstalleerd op de Gast VMs. |
| 60.222 | (FBI) Setup-Console VMs | DVM inhoud verplaatsen naar de Console VM. |
| 251 | Voorbereiding voor toekomstige host opnieuw is opgestart | Opnieuw opstarten-beleid instellen |


## <a name="next-steps"></a>Volgende stappen

[Veelgestelde vragen](azure-stack-FAQ.md)
