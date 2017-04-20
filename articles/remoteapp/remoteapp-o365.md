
<properties
    pageTitle="Gebruik van Office met Azure RemoteApp | Microsoft Azure" 
    description="Meer informatie over hoe Office en Azure RemoteApp samen"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-office-with-azure-remoteapp"></a>Gebruik van Office met Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

U hebt twee mogelijkheden voor het hosten van Office-toepassingen in Azure RemoteApp: Office 365 ProPlus of proefversie van Office 2013 Professional Plus.

**Hoi, wist u dat we hebben een nieuw artikel dat wordt binnenkort vervangen door dit beter? Ontdek [hoe u uw abonnement op Office 365 met RemoteApp-Azure](remoteapp-officesubscription.md). Dit omvat alle informatie die u nodig hebt voor het gebruik van Office 365 + Azure RemoteApp.**

## <a name="office-365-proplus"></a>Office 365 ProPlus
U kunt een RemoteApp-collectie met behulp van de Office 365 ProPlus afbeelding van de sjabloon maken. Met deze optie kunt u uw Office 365-service worden uitgebreid tot RemoteApp. Hebt u een bestaande abonnementen en uw gebruikers moeten beschikken voor de Office 365 ProPlus-service, een zelfstandige of plannen via de Office 365-service.

RemoteApp ondersteunt gedeelde computeractivering van Office 365. Wanneer u de gedeelde Computer activeren inschakelen en de [Office Deployment tool](http://www.microsoft.com/download/details.aspx?id=36778) voor installatie gebruiken, Office 365 ProPlus wordt geïnstalleerd zonder het te activeren. Wanneer de symptomen van een gebruiker in een collectie met Office 365, Office wordt gecontroleerd als de gebruiker voor Office 365 ProPlus is ingericht. Als dus Office tijdelijk geactiveerd Office 365 ProPlus - deze activering blijft zich voordoen totdat die gebruikers afmelden bij de service.

Activering van Office 365 gedeelde Computer gebruikt, moet u een [aangepaste sjabloon](remoteapp-create-custom-image.md) maken en installeren van Office 365 ProPlus, volg [deze aanwijzingen](https://technet.microsoft.com/library/dn782858.aspx).

U kunt uw gebruikers Office 365 licenties op de [Office 365-beheerportal](https://portal.office365.com/)beheren. Lees meer informatie over [Office 365 serviceplannen](http://technet.microsoft.com/library/office-365-plan-options.aspx).  


## <a name="office-2013-professional-plus-trial"></a>Proefversie van Office 2013 Professional Plus
Tijdens een proefperiode van 30 dagen van RemoteApp, kunt u de afbeelding van Office 2013 Professional Plus (proef) sjabloon voor het maken van een RemoteApp-collectie. U kunt gebruikers toewijzen aan deze evaluatie-collectie met hun werk Azure Active Directory of Microsoft grootboekrekeningen. Geen extra abonnement is vereist.

Dit is een geweldige optie te ere van de banden en een goed gevoel voor Office in RemoteApp. Deze optie is echter die bestemd zijn voor de evaluatie en testen alleen. RemoteApp-collecties gemaakt met de afbeelding van Office 2013 Professional Plus (proef) sjabloon kunnen niet worden overgestapt naar de productiemodus en wordt uitgeschakeld aan het einde van de proefperiode.

## <a name="switching-from-trial-to-production"></a>Overschakelen van een evaluatieversie naar productie
Bij het starten van de gratis proefperiode van 30 dagen ziet een notitie in de RemoteApp-sectie van de portal u hoe lang u in de evaluatieversie hebt verlaten voordat u nodig hebt om een overgang naar een betaalde account. U kunt uw account activeren en overschakelen naar de productiemodus op de koppeling in deze notitie.

Wanneer u uw account activeert, heeft dit invloed op alle RemoteApp-collecties in uw account.

- Collecties met met Windows Server 2012 R2 of de Office 365 ProPlus sjabloon-afbeeldingen wordt naadloos overgang naar productie. Alle gebruikersgegevens en -instellingen, inclusief lopende sessies blijven intact.
- Als u de aangepaste sjabloon afbeeldingen hebt geüpload, overgang collecties met behulp van deze afbeeldingen ook naadloos.
- De afbeelding van Office 2013 Professional Plus (proef) sjabloon is bedoeld voor evaluatie alleen. Collecties met de Sjabloonafbeelding van deze kunnen niet worden overgestapt naar productie. Ze worden opgeslagen in 'uitgeschakeld' staat.


Als u doet geen overgang naar een productiemodus door het verlopen van de evaluatieversie, worden RemoteApp-collecties uitgeschakeld. Maak je geen zorgen - uw instellingen en gegevens van gebruikers worden opgeslagen voor een ander 90 dagen, zodat u kunt nog steeds uw service activeren en productie-modus zonder dat er gegevens verloren gaan.
