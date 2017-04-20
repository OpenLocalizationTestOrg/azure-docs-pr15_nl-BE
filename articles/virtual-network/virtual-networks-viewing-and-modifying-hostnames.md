<properties 
   pageTitle="Weergeven en wijzigen van hostnamen | Microsoft Azure"
   description="Het weergeven en wijzigen van hostnamen voor Azure virtuele machines, web en rollen van de werknemer voor naamomzetting"
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
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="viewing-and-modifying-hostnames"></a>Weergeven en wijzigen van hostnamen

Als u wilt dat uw rol exemplaren naar worden verwezen met de naam van de host, moet u de waarde voor de host-naam instellen in het configuratiebestand van de service voor elke rol. U doet dit door de gewenste host-naam toevoegen aan het kenmerk **vmName** van het element **Role** . De waarde van het kenmerk **vmName** wordt gebruikt als basis voor de hostnaam van elk exemplaar van de rol. Bijvoorbeeld als **vmName** *webrole* en er drie exemplaren van deze functie zijn, de hostnamen van de exemplaren worden *webrole0*, *webrole1*en *webrole2*. U hoeft niet de naam van een host voor virtuele machines opgeven in het configuratiebestand omdat de hostnaam voor een virtuele machine wordt ingevuld op basis van de naam van de virtuele machine. Zie voor meer informatie over het configureren van een service van Microsoft Azure [Azure Service configuratieschema (.cscfg bestand)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Hostnamen weergeven

U kunt de hostnamen van de rol van instanties en virtuele machines in een cloud-service met behulp van een van de onderstaande hulpprogramma's weergeven.

### <a name="azure-portal"></a>Azure Portal

De [Azure portal](http://portal.azure.com) kunt u de namen van host voor virtuele machines weergeven op het blad Overzicht voor een virtuele machine. Houd er rekening mee dat het blad geeft een waarde voor de **naam** en de **Naam van de Host**. Hoewel ze in eerste instantie hetzelfde zijn, verandert wijzigt de host-naam niet de naam van de virtuele machine of het exemplaar van de rol.

Rol exemplaren kunnen ook worden weergegeven in de portal Azure, maar wanneer u de exemplaren in een cloud-service, de host-naam niet wordt weergegeven. Ziet u een naam voor elk exemplaar, maar die naam staat niet voor de host-naam.

### <a name="service-configuration-file"></a>Service-configuratiebestand

Het configuratiebestand voor de service voor gedistribueerde service kunt u downloaden van de bladeserver **configureren** van de service in de portal Azure. Vervolgens kunt u het kenmerk **vmName** voor het element **naam rol** voor een overzicht van de host-naam zoeken. Houd er rekening mee dat deze hostnaam wordt gebruikt als basis voor de hostnaam van elk exemplaar van de rol. Bijvoorbeeld als **vmName** *webrole* en er drie exemplaren van deze functie zijn, de hostnamen van de exemplaren worden *webrole0*, *webrole1*en *webrole2*.

### <a name="remote-desktop"></a>Extern bureaublad

Nadat u extern bureaublad (Windows), Windows PowerShell remoting (Windows) of SSH (Linux en Windows) verbindingen met uw virtuele machines of rol exemplaren hebt ingeschakeld, kunt u de hostnaam van een actieve verbinding met extern bureaublad op verschillende manieren weergeven:

- Typ hostname bij de opdrachtprompt of SSH-terminal.

- Typ ipconfig/alle bij de opdrachtprompt (alleen Windows).

- De naam van de computer weergeven in de instellingen (alleen Windows).

### <a name="azure-service-management-rest-api"></a>Azure Service Management REST API

Volg de instructies in een REST-client:

1. Zorg ervoor dat er een clientcertificaat verbinding maken met de Azure portal. Als u een clientcertificaat, de stappen die [hoe: downloaden en publicatie-instellingen importeren en abonnementsgegevens](https://msdn.microsoft.com/library/dn385850.aspx). 

1. Stel een header-fragment met de naam ms-x-versie met een waarde van 11-01-2013.

1. Een aanvraag verzenden in de volgende notatie: https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<-servicenaam\>?embed-details = true

1. Zoek naar het element **HostName** voor elk element van de **RoleInstance** .

>[AZURE.WARNING] U kunt ook de interne domeinachtervoegsel voor cloud-service van de REST oproep reactie aan de hand van het element **InternalDnsSuffix** of door ipconfig/vanaf een opdrachtprompt weergeven in een sessie van extern bureaublad (Windows) of door een actieve kat /etc/resolv.conf van een SSH-terminal (Linux).

## <a name="modifying-a-hostname"></a>Een host-naam wijzigen

U kunt de hostnaam voor een virtuele machine of het exemplaar van de rol wijzigen door een service met gewijzigde configuratiebestand uploaden of de naam van de computer op vanaf een extern bureaublad-sessie.

## <a name="next-steps"></a>Volgende stappen

[Naamomzetting (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Configuratieschema Azure Service (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure Virtual Network configuratieschema](http://go.microsoft.com/fwlink/?LinkId=248093)

[DNS-instellingen via netwerk configuratiebestanden opgeven](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
