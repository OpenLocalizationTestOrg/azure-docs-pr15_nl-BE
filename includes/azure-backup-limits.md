 (back-kluizen<properties
   Paginatitel = "Azure Backup limits table"
   Beschrijving = "beperkingen van het systeem voor de beschrijving Azure Backup."
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />
<tags
   ms.service="backup"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/05/2015"
   ms.author="trinadhk";"jimpark"; "aashishr" />


De volgende beperkingen gelden voor Azure back-up.

| Limiet-ID | Standaardlimiet |
|---|---|
|Aantal servers/machines die kunnen worden geregistreerd tegen elke kluis|50 voor Windows Client-Server/SCDPM <br/> 200 voor VMs IaaS|
|Grootte van een gegevensbron voor de gegevens die zijn opgeslagen in een kluis Azure opslag|54400 GB max<sup>1</sup>|
|Aantal back-kluizen die kunnen worden gemaakt in elk abonnement Azure|25 (back-up kluizen) <br/> Kluis 25 recovery Services per regio|
|Aantal keren per dag back-up worden gepland|3 per dag voor Windows Server/Client <br/> 2 per dag voor SCDPM <br/> Eenmaal per dag voor IaaS VMs|
|Gegevensschijven die zijn gekoppeld aan een Azure virtual machine voor back-up|16|

- <sup>1</sup> De 54400 GB-limiet geldt niet voor IaaS VM back-up.

