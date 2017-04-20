<properties 
   pageTitle="Beheren van DNS-servers die worden gebruikt door een virtueel netwerk (VNet)"
   description="Meer informatie over het toevoegen en verwijderen van DNS-servers in een virtueel netwerk (vnet)"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="manage-dns-servers-used-by-a-virtual-network-vnet"></a>Beheren van DNS-servers die worden gebruikt door een virtueel netwerk (VNet)

U kunt de lijst met DNS-servers die worden gebruikt in een VNet in de Portal Management of in het configuratiebestand van het netwerk beheren. U kunt maximaal 12 DNS-servers toevoegen voor elke VNet. Wanneer u DNS-servers opgeeft, is het belangrijk om te controleren of het overzicht van uw DNS-servers in de juiste volgorde voor uw omgeving. Round-robin werken DNS-serverlijst niet. Ze worden gebruikt in de volgorde waarin deze zijn opgegeven. Als de eerste DNS-server in de lijst kan worden bereikt, wordt de client die DNS-server, ongeacht of de DNS-server goed of niet functioneert gebruiken. De DNS-servers verwijderen uit de lijst om de volgorde van de DNS-server voor het virtuele netwerk wijzigen, en deze weer in de volgorde die u wilt toevoegen.

>[AZURE.WARNING] Nadat de DNS-lijst is bijgewerkt, moet u opnieuw de virtuele machines die zich in het virtuele netwerk zodat zij de nieuwe DNS-serverinstellingen afhalen. Virtuele machines blijven de huidige configuratie gebruiken totdat u ze opnieuw zijn gestart.

## <a name="edit-a-dns-server-list-for-a-virtual-network-using-the-management-portal"></a>Een lijst van de DNS-server voor een virtueel netwerk met behulp van de Portal Management bewerken

1. Meld u aan bij de **Portal beheren**.

1. Op **netwerken**en klik vervolgens op de naam van het virtuele netwerk in de kolom **naam** in het navigatiedeelvenster.

1. Klik op **configureren**.

1. In de **DNS-Servers**, kunt u het volgende:

    - **Registreren (toevoegen) een nieuwe DNS-server:** Typ de naam en het IP-adres in de vakken. Hiermee wordt een DNS-server toegevoegd aan het virtuele netwerk lijst met DNS-Servers en registreert ook de DNS-server met Azure.

    - **Toevoegen van een DNS-server die eerder is geregistreerd:** Als u al een DNS-server geregistreerd met Azure, selecteert u deze uit de lijst met vooraf ingestelde.

    - **Voor het verwijderen van een DNS-server van het virtuele netwerk:** Klik op de X naast de server die u wilt verwijderen. Houd er rekening mee dat dit alleen wordt de server verwijderd uit de Netwerklijst met virtuele. De DNS-server blijft geregistreerd in Azure voor uw virtuele netwerken gebruik. Als een DNS-server verwijderen uit uw abonnement, gaat u naar de pagina **netwerken -> DNS-Servers** .

    - **Op DNS-servers: opnieuw rangschikken** Verwijder alle DNS-servers die worden vermeld en deze toevoegen in de volgorde die u wilt inchecken. Houd er rekening mee dat dit niet een lijst van round-robin DNS is.

    - **Naam van een DNS-server:** De DNS-server in de lijst markeren en typ vervolgens de nieuwe naam. Hiermee wordt een nieuwe DNS-server registreren in Azure, alsmede toe te voegen aan de lijst met DNS-Servers voor het virtuele netwerk. De oude DNS-server en het IP-adres blijven ingeschreven met Azure. U kunt verwijderen op de pagina **DNS-Servers** als u dit niet voor andere virtuele netwerken gebruikt.

1. Klik op **Opslaan** onder aan de pagina om te slaan uw nieuwe configuratie van DNS-servers.

1. De virtuele machines die zich in het virtuele netwerk om het verkrijgen van de nieuwe DNS-instellingen te kunnen starten.

## <a name="edit-a-dns-server-list-using-a-network-configuration-file"></a>Een lijst van DNS-servers via een netwerk configuratie-bestand bewerken

Om een lijst met DNS-servers via een netwerk configuratiebestand bewerken, zult u eerst de configuratie-instellingen exporteren vanuit de Portal beheren. U vervolgens het netwerk configuratiebestand bewerken en opnieuw door de Portal Management te importeren. Hieronder vindt u een hoog niveau overzicht van de stappen om dit proces te voltooien.

1. De instellingen voor virtuele netwerk exporteren naar een configuratiebestand netwerk. Zie [Virtuele netwerkinstellingen naar een configuratiebestand netwerk exporteren](virtual-networks-using-network-configuration-file.md)voor meer informatie en stappen om uw netwerk configuratie-instellingen exporteren.

1. Geef de DNS-servergegevens voor het virtuele netwerk. Zie [een DNS-Server in een virtueel netwerk configuratiebestand opgeven](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)voor meer informatie over het opgeven van een DNS-server. Zie voor meer informatie over configuratiebestanden van netwerk [Azure virtuele netwerk configuratieschema](https://msdn.microsoft.com/library/azure/jj157100.aspx) en [Configureer een virtueel netwerk met behulp van een configuratiebestand netwerk](virtual-networks-using-network-configuration-file.md).

1. Het netwerk configuratiebestand importeren. Zie [een netwerk-configuratiebestand importeren](virtual-networks-using-network-configuration-file.md)voor meer informatie en stappen voor het importeren van het configuratiebestand van uw netwerk.

1. De virtuele machines die zich in het virtuele netwerk om het verkrijgen van de nieuwe DNS-instellingen te kunnen starten.
