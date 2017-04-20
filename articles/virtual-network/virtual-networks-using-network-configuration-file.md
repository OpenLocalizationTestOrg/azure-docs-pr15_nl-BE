<properties 
    pageTitle="Een virtueel netwerk met behulp van een configuratiebestand netwerk configureren" 
    description="Instructies voor het exporteren en importeren van een configuratiebestand netwerk de Azure Management Portal maken of wijzigen van virtuele netwerken. " 
    services="virtual-network" 
    documentationCenter="" 
    authors="jimdial" 
    manager="carmonm" 
    editor="tysonn"/>

<tags
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services" 
    ms.date="03/15/2016"
    ms.author="jdial"/>

# <a name="configure-a-virtual-network-using-a-network-configuration-file"></a>Een virtueel netwerk met behulp van een configuratiebestand netwerk configureren

U kunt een virtueel netwerk (VNet) met behulp van de portal Azure-beheer of via een netwerk configuratie.

## <a name="creating-and-modifying-a-network-configuration-file"></a>Maken en wijzigen van een configuratiebestand netwerk 
De eenvoudigste manier om een netwerk-configuratiebestand is de netwerkinstellingen exporteren uit een bestaande virtuele netwerkconfiguratie, wijzigt u het bestand waarin de instellingen die u wilt configureren voor de virtuele netwerken.

De netwerk configuratie als bestand wilt bewerken, kunt u gewoon het bestand openen, breng de gewenste wijzigingen aan en slaat u het bestand. Een *XML-* editor kunt u wijzigingen aanbrengen in het configuratiebestand van het netwerk. 

Volg nauwkeurig de aanwijzingen voor [netwerk configuratie bestand schema-instellingen](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

Azure beschouwt een subnet dat iets geïmplementeerd als **in gebruik**heeft. Wanneer een subnet gebruikt wordt, kan niet worden gewijzigd. Verplaatsen voordat u wijzigingen gaat iets die u hebt geïmplementeerd op het subnet naar een ander subnet dat niet is gewijzigd.   Zie [een VM- of rol-exemplaar naar een ander Subnet verplaatst](virtual-networks-move-vm-role-to-subnet.md).

## <a name="export-and-import-virtual-network-settings-using-the-management-portal"></a>Instellingen van virtueel netwerk met behulp van de Portal Management importeren en exporteren  
U kunt importeren en exporteren van netwerk configuratieinstellingen in het configuratiebestand van uw netwerk met PowerShell of de Portal beheren. De volgende instructies kunt u met behulp van de Portal Management importeren en exporteren. 

### <a name="to-export-your-network-settings"></a>De netwerkinstellingen exporteren
Wanneer u exporteert, worden alle instellingen voor de virtuele netwerken in uw abonnement aan een XML-bestand geschreven. 

1. Meld u aan bij de **Portal beheren**.
2. Klik op **exporteren**in de Portal Management aan de onderkant van de pagina **netwerken** . 
3. Controleer of u het abonnement waarvoor u wilt uw netwerkinstellingen exporteren hebt geselecteerd in het venster **netwerkconfiguratie exporteren** . Klik op het vinkje in de rechterbenedenhoek. 
4. Wanneer u wordt gevraagd, sla het bestand *NetworkConfig.xml* naar de locatie van uw keuze.


### <a name="to-import-your-network-settings"></a>De instellingen voor netwerk importeren

1. Klik op **Nieuw**in het **Portal Management**in het navigatievenster links, onderaan.
2. Klik op **Netwerkservices** -> **virtueel netwerk** -> **configuratie importeren**.
3. Op de pagina **importeren van het configuratiebestand netwerk** bladert u naar het configuratiebestand van uw netwerk en klik vervolgens op de pijl **volgende** gemarkeerd.
4. Klik op de pagina **bouwen van uw netwerk** vindt u informatie op het scherm weergegeven welke gedeelten van uw netwerkconfiguratie worden gewijzigd of gemaakt. Als de wijzigingen die u juist uitziet, klikt u op het vinkje om terug te gaan naar de virtueel netwerk maken of bijwerken. 