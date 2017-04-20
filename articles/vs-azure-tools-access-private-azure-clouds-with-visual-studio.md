<properties 
   pageTitle="Toegang tot persoonlijke Azure wolken met Visual Studio | Microsoft Azure"
   description="Informatie over toegang tot persoonlijke cloud resources met behulp van Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Toegang tot persoonlijke Azure wolken met Visual Studio

##<a name="overview"></a>Overzicht

Visual Studio ondersteunt standaard openbare Azure cloud REST eindpunten. Dit is een probleem, maar als u met behulp van Visual Studio met een persoonlijke Azure cloud. U kunt certificaten gebruiken voor het configureren van Visual Studio voor toegang tot persoonlijke Azure cloud REST eindpunten. U krijgt deze certificaten via uw Azure instellingenbestand publiceren.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Toegang krijgen tot een particulier Azure cloud in Visual Studio

1. Downloaden van het bestand publiceren met instellingen in [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885) voor de private cloud, of neem contact op met uw beheerder voor een bestand publiceren. Op de openbare versie van Azure is de koppeling voor het downloaden van dit [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Het bestand dat u downloadt moet de extensie .publishsettings hebben).

1. In Visual Studio **Server Explorer** Kies het knooppunt **Azure** en kies in het snelmenu de opdracht **Abonnementen beheren** .

    ![Opdracht abonnementen beheren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. Klik op het tabblad **certificaten** en klik op de knop **importeren** in het dialoogvenster **Microsoft Azure abonnementen beheren** .

    ![Azure certificaten importeren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. Blader naar de map waar u het bestand publiceren instellingen opgeslagen en kies het bestand en kies vervolgens de knop **importeren** in het dialoogvenster **Importeren abonnementen op Microsoft Azure** . Dit importeert u de certificaten in het instellingenbestand publiceren in Visual Studio. Nu moet u kunnen werken met uw persoonlijke cloud resources.

    ![Publicatie-instellingen importeren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

## <a name="next-steps"></a>Volgende stappen

[Publiceren naar een Azure Cloud-Service vanuit Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

[Procedure: downloaden en importeren van instellingen en abonnementsinformatie publiceren](https://msdn.microsoft.com/library/dn385850(v=nav.70).aspx)

