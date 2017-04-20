
<properties
    pageTitle="Lijst met poorten en URL's naar "witte" lijst voor Azure RemoteApp geïmplementeerd in het virtuele netwerk klant | Microsoft Azure"
    description="Informatie over welke poorten en URL's u moet configureren voor communicatie via RemoteApp Azure."
    services="remoteapp"
    documentationCenter=""
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="elizapo" />



# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Lijst van havens en URL's om toegang te verlenen voor Azure RemoteApp geïmplementeerd in een virtueel netwerk van klant 

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Het volgende is van toepassing op RemoteApp Azure cloud of hybride-collectie als u deze in een virtueel netwerk (VNET implementeert). Lees voor meer informatie over virtuele netwerken, [Virtuele netwerk-overzicht](../virtual-network/virtual-networks-overview.md). Als u een netwerk security group (NSG) beperken van verkeer naar uw virtuele netwerkbronnen die u hebt gekozen voor Azure RemoteApp hebt gemaakt, Controleer of dat de volgende toegankelijk zijn en door het beveiligingsbeleid op het virtuele netwerk toegestaan. Voor meer informatie over netwerk-beveiligingsgroepen, Lees [Wat is een netwerk-beveiligingsgroep? (NSG)](../virtual-network/virtual-networks-nsg.md).

##  <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure RemoteApp-subnet moet toegang tot deze eindpunten en URL's: 
*   *. servicebus.windows.net
*    *. servicebus.net
*    https://*.RemoteApp.windwsazure.com  
*    https://www.RemoteApp.windowsazure.com 
*    https://*RemoteApp.windowsazure.com  
*    https://*.Core.Windows.NET  
*    Uitgaande: TCP: 443, TCP: 10101 10175 
*    Optioneel: UDP: 10201 10275  
 
## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure RemoteApp-clients toegang willen krijgen tot deze eindpunten en URL's: 

Door clients bedoel ik de desktops, enz. die mensen gebruiken om te verbinden met de toepassingen die zijn geïmplementeerd in de collectie Azure RemoteApp-apparaten.

-  https://telemetry.RemoteApp.windowsazure.com  
-  https://*.RemoteApp.windowsazure.com (de optionele UDP-poorten zijn voor dit adres) 
-  https://login.Windows.NET  
-  https://login.microsoftonline.com  
-  https://www.RemoteApp.windowsazure.com 
-  https://*.Core.Windows.NET  
-  Uitgaande: TCP: 443  
-  Optioneel - UDP: 3391 