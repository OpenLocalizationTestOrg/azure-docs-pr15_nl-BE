<properties
   pageTitle="Tolerantie richtlijnen service | Microsoft Azure"
   description="Koppelingen naar herstel na storingen en proactieve richtlijnen voor dynamiek en beschikbaarheid voor Microsoft Azure services."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

# <a name="microsoft-azure-service-resiliency-guidance"></a>Richtlijnen voor Microsoft Azure Services tolerantie
Microsoft Azure is ontworpen om te voorzien van middelen die u nodig hebt wanneer u ze nodig hebt. Als onderdeel van een goed ontwerp en operationele procedures, moet u zowel het bouwen van uw gebruik van Azure services voor een maximale beschikbaarheid als wat te doen als uw toepassing wordt getroffen door een onderbreking van de service te weten. Om u in dit proces, bevat dit document koppelingen naar disaster recovery richtlijnen alsmede richtlijnen voor verschillende Azure services.

##<a name="disaster-recovery-guidance"></a>Disaster recovery richtlijnen
De disaster recovery richtlijnen onderstaande koppelingen zijn kunt u voorzien van de informatie die u nodig hebt om te helpen u snel weer online uw toepassing als u een Azure onderbrekingen van invloed. Deze koppelingen zijn gemaakt, zodat u de vraag, "Ik ben wordt getroffen door een onderbreking Azure service, wat kan ik doen?"

##<a name="design-guidance"></a>Ontwerpondersteuning
De onderstaande ontwerp richtlijnen voor koppelingen zijn ontwerp en architecturale richtlijnen die u helpen bij het beste elke Azure-service wilt gebruiken op een manier die de uptime van uw toepassing maximaliseert is gemaakt. Deze koppelingen zijn gemaakt, zodat u de vraag "Hoe zorg ik ervoor dat een bug, hardwarestoringen, onderbrekingen of andere storing niet van invloed op de algehele beschikbaarheid van mijn toepassing?" Als er geen specifieke richtsnoeren voor de service die u momenteel zoekt, kan de [hoge beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure](./resiliency-high-availability-azure-applications.md) artikel aanvullende informatie die u bij het ontwerpen van helpen kan hebben. 

##<a name="service-guidance"></a>Richtlijnen voor services
| Service  | Disaster recovery richtlijnen | Ontwerpondersteuning |
|:---------|:--------------------------:|:------------------:|
| [Cloud Services] (https://azure.microsoft.com/services/cloud-services/ "Azure Cloud Services")       | [koppeling] (../cloud-services/cloud-services-disaster-recovery-guidance.md "Azure Cloud Services disaster recovery richtlijnen")   | Niet beschikbaar |
| [Sleutel kluis] (https://azure.microsoft.com/services/key-vault/ "Azure sleutel kluis")                      | [koppeling] (../key-vault/key-vault-disaster-recovery-guidance.md "Azure sleutel kluis disaster recovery richtlijnen")        | Niet beschikbaar |
| [Opslag] (https://azure.microsoft.com/services/storage/ "Azure opslag")                            | [koppeling] (../storage/storage-disaster-recovery-guidance.md "Azure opslag disaster recovery richtlijnen")          | Niet beschikbaar |
| [SQL-Databases] (https://azure.microsoft.com/services/sql-database/ "Azure SQL-Databases")           | [koppeling] (../sql-database/sql-database-disaster-recovery.md  "Richtlijnen voor disaster recovery Azure SQL-Database")    | [koppeling] (../sql-database/sql-database-business-continuity.md "Overzicht van de bedrijfscontinuïteit met Azure SQL-Database") |
| [Virtuele Machines] (https://azure.microsoft.com/services/virtual-machines/ "Azure virtuele Machines") | [koppeling] (../virtual-machines/virtual-machines-disaster-recovery-guidance.md "Richtlijnen voor disaster recovery Azure virtuele Machines") | Niet beschikbaar |
| [Virtueel netwerk] (https://azure.microsoft.com/services/virtual-network/ "Azure Virtual Network")    | [koppeling] (../virtual-network/virtual-network-disaster-recovery-guidance.md "Azure Virtual Network disaster recovery richtlijnen")  | Niet beschikbaar |

##<a name="next-steps"></a>Volgende stappen
Als u op zoek bent voor de richtlijnen die meer algemeen ligt de nadruk op systemen en oplossingen, Lees [noodherstel en hoge beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure](https://aka.ms/drtechguide).
