<properties
    pageTitle="Azure RemoteApp-licenties | Microsoft Azure"
    description="Informatie over licentieverlening werking in Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Hoe werkt licentieverlening samen in Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Zodat u uw Azure RemoteApp-service, de sjablonen, gemaakt hebt ingesteld en apps voor uw gebruikers publiceren. Maar er nog één laatste ding is om erachter te komen: licentieverlening. Maar hoe werkt licentieverlening voor RemoteApp- en voor de apps die u via RemoteApp delen?

RemoteApp hoeft niet alle Windows-licenties en licenties voor clienttoegang voor extern bureaublad. Uw abonnement zorgt voor de RemoteApp-kant zelf. (Bekijk de details van de [prijsstelling plannen](https://azure.microsoft.com/pricing/details/remoteapp).)

Als u een van de afbeeldingen die deel uitmaakt van uw abonnement, kunt u delen van de apps op die afbeelding geïnstalleerd zonder dat daarvoor een afzonderlijke licentie nodig. Als u de installatiekopie van Windows Server 2012 R2 sjabloon aan uw collectie samenstellen, kunt u System Center Endpoint Protection delen met uw gebruikers. De enige uitzonderingen op deze regel zijn Office 365 ProPlus, waarvoor een apart abonnement en Office 2013, dat in een productie-collectie kan niet worden gedeeld.

Als u wilt dat de afbeelding van Office 365 sjabloon die wordt geleverd met RemoteApp gebruiken, moet u een *bestaand* Office 365 ProPlus schema hebben. Hetzelfde geldt voor alle Office 365-app die u publiceert met behulp van een aangepaste sjabloon. U moet de apps met uw eigen abonnement activeren. Dit geldt voor zowel proefversie en betaalde abonnementen. Als u wilt dat de afbeelding van de sjabloon voor Office 365 gebruiken tijdens de evaluatieperiode, *en u niet al een abonnement hebt*, gaat u naar de pagina Office 365 aan te [melden](https://go.microsoft.com/fwlink/p/?LinkID=403802) voor een proefabonnement. Zie [hoe RemoteApp- en Office werken samen?](remoteapp-o365.md) voor meer informatie.

Als tijdens de proefperiode u niet wilt krijgen een proefabonnement op Office 365, gebruikt u de Office 2013 Professional Plus image-sjabloon die wordt geleverd met RemoteApp. Deze Sjabloonafbeelding kan alleen worden gebruikt voor 30 dagen en kan niet worden geconverteerd naar een betaalde verzameling.

Voor andere toepassingen moet u de licentie voor het delen van de app hebt.

Dat is zinvol, rechts? U kunt publiceren elke toepassing die officieel gemachtigd zijn om te delen. En u moet ervoor zorgen dat u echt recht hebben op uw programma's delen.

Houd er rekening mee dat u een CAL of Volume License-overeenkomst niet in een wolk-collectie gebruiken. U *kunt* via een Volume License-overeenkomst activeren toepassingen in uw verzameling hybride (met uitzondering van Office). U hoeft alleen maar op de Sjabloonafbeelding van het Volume License-media installeren. Volg de informatie van de leverancier van de toepassing op licenties installeren in een omgeving voor extern bureaublad.
