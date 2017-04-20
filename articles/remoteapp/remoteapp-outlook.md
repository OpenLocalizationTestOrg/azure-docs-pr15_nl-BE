<properties
    pageTitle="Outlook gebruikt in Azure RemoteApp | Microsoft Azure" 
    description="Meer informatie over het configureren en gebruiken van Outlook in Azure RemoteApp | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Met behulp van Microsoft Outlook in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp ondersteunt Microsoft Outlook O365. Lees meer over hoe [Office werkt in Azure RemoteApp](remoteapp-officesubscription.md). Er zijn een paar aanbevolen instellingen voor Outlook gebruikt in Azure RemoteApp.

## <a name="cached-mode"></a>Modus met cache
Cache-modus wordt configuratie aanbevolen wanneer u Outlook gebruikt in Azure RemoteApp. Bij het configureren van een Outlook 2013 account op de Exchange-modus met cache gebruikt, werkt Outlook 2013 van een lokale kopie van de Microsoft Exchange-postvak van de gebruiker die is opgeslagen in een bestand met off line gegevens (OST-bestand) op de computer van de gebruiker, samen met de off line adres boek (OAB). Het postvak in de cache en de OAB worden regelmatig bijgewerkt bij de service O365. Lees meer over [de verschillen tussen de in de cache en on line-modus](https://technet.microsoft.com/library/jj683103.aspx).

De gebruiker kan **De Exchange-modus met cache** of **On line-modus** selecteren tijdens de installatie van de account of door de accountinstellingen. U kunt ook een modus of de andere implementeren met behulp van de Office Customization Tool (OCT) of Groepsbeleid.  

[Stapsgewijze instructies voor het inschakelen van de modus met cache](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc)wordt gelezen.

## <a name="search"></a>Zoeken
In Azure RemoteApp heeft met behulp van zoeken in Outlook beperkingen. Gegroepeerde VMs wordt Azure RemoteApp ten behoeve van gebruikerssessies. Zoekindexering afhankelijk is van de computer-ID, die verschillend voor verschillende VMs is. Het is mogelijk dat telkens wanneer een gebruiker zich in Azure RemoteApp aanmeldt, ze worden doorgestuurd naar een nieuwe VM. Dit betekent dat, als we lokale zoekopdrachten inschakelt, de indexering wordt uitgevoerd telkens wanneer de computer-ID (als de gebruiker op een andere VM) wordt gewijzigd. Afhankelijk van de grootte van het. OST-bestand, de indexering kan lang duren om te voltooien en het gebruik van bronnen die nodig zijn voor andere toepassingen. Niet alleen zou langzaam maar kunnen geen resultaten opleveren. Met een profiel van de account on line-modus zou dit probleem oplossen door, maar de algehele prestaties zou lijden door het ontbreken van een lokale cache (Zie de bovenstaande link voor meer informatie over het verschil tussen de in de cache en on line-modus). Helaas kan niet worden geïndexeerd/local search uitgeschakeld en online zoeken kan niet worden standaard ingeschakeld in Outlook 2013.
