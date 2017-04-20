<properties
    pageTitle="Hoe een VNET RemoteApp migreren naar een Azure-VNET | Microsoft Azure"
    description="Meer informatie over het migreren van een RemoteApp VNET naar een Azure-VNET"
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



# <a name="how-to-migrate-a-hybrid-collection-from-a-remoteapp-vnet-to-an-azure-vnet"></a>Een hybride-collectie van een VNET RemoteApp migreren naar een Azure-VNET

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Goed nieuws! We hebt implementeren hybride RemoteApp-collecties rechtstreeks in uw bestaande Azure virtuele netwerken (VNETs) in plaats van RemoteApp-specifieke VNETs u ingeschakeld. Hiermee kunt u profiteren van de nieuwste functies van VNET (zoals ExpressRoute) en hybride collecties direct netwerktoegang geven tot andere Azure services en virtuele machines die VNET geïmplementeerd.  (Dit haalt u sneller en eenvoudiger instellen dan de configuraties van VNET naar VNET).


Stel, u hebt al een hybride RemoteApp-collectie genaamd *OriginalCollection* met een RemoteApp-VNET genaamd *RemoteAppVNET*gemaakt. Hier zijn de stappen om te migreren naar een nieuwe Azure VNET *AzureVNET*genoemd.

1.  Maak een VNET genaamd *AzureVNET*, met dezelfde locatie, DNS-configuratie en -adresruimte (voor ten minste één van de subnetten *AzureVNET* ) als u hebt gebruikt voor *RemoteAppVNET*op het tabblad **netwerken** in de [portal beheren](http://manage.windowsazure.com/).
2.  *AzureVNET* op de host configureren of netwerkverbinding met de Active Directory-implementatie die *OriginalCollection* lid is van domein hebt.
3.  Maak een nieuwe RemoteApp-collectie *Nieuwe collectie*genoemd op het tabblad **RemoteApps** . (Gebruik de optie **maken met VNET** , niet **Snel te maken**.)
3.  Configureer de *NewCollection* op een subnet in *AzureVNET*kunnen worden geïmplementeerd.
4.  *NewCollection* om dezelfde afbeelding en domeingegevens join gebruiken als u hebt gebruikt voor *OriginalCollection*configureren.
5.  Na een paar uur wordt *NewCollection* weergegeven in de lijst met collecties met een actieve status.

Nu, als u geen gebruikersgegevens uit de oorspronkelijke collectie migreren naar de nieuwe collectie, doen deze stappen:

6.  *OriginalCollection*verwijderen.
7.  *RemoteAppVNET*verwijderen.

En u bent klaar!

Ook als u gebruikersgegevens uit de oorspronkelijke collectie migreren naar de nieuwe collectie, hoeft deze stappen nu doen:

6.  Een e-mail sturen naar [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) met uw Azure abonnements-ID, de naam van de oorspronkelijke verzameling en de naam van de nieuwe collectie en vragen om uw gebruikersgegevens te migreren.
7.  Binnen 2 werkdagen gaat de RemoteApp-team de lijst voor gebruikerstoegang alle gebruikersdocumenten en gebruikersinstellingen uit de oorspronkelijke collectie naar de nieuwe collectie.
8.  *OriginalCollection*verwijderen.
9.  *RemoteAppVNET*verwijderen.

En u bent nu klaar!

Mocht u vragen hebben of speciale hulp nodig hebt, stuur een e-mail [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).
