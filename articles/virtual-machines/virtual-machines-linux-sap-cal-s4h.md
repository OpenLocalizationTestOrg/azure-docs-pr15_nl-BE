<properties 
pageTitle="HANA S/4 of ZW/4 HANA op een Azure VM implementeren | Microsoft Azure" 
description="Implementeren van S/4 HANA of HANA op een Azure VM BW/4" 
services="virtual-machines-linux" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
  keywords=""/> 
<tags 
  ms.service="virtual-machines-linux" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.tgt_pltfrm="vm-linux" 
  ms.workload="infrastructure-services" 
  ms.date="09/15/2016" 
  ms.author="hermannd"/> 


# <a name="deploying-s4-hana-or-bw4-hana-on-microsoft-azure"></a>Implementeren van S/4 HANA of HANA BW/4 op Microsoft Azure 

In dit artikel wordt beschreven hoe S/4 HANA op Microsoft Azure via SAP Cloud toestel bibliotheek 3.0 implementeren.
De screenshots tonen het proces stap voor stap. Implementatie van andere oplossingen op basis van SAP HANA zoals BW/4 HANA werkt op dezelfde manier vanuit het perspectief van een proces. Een heeft zojuist een andere oplossing selecteren.

Beginnen met SAP Cloud toestel bibliotheek (SAP CAL) Ga [hier](https://cal.sap.com/). Er is een blog uit SAP over de nieuwe [SAP Cloud toestel bibliotheek 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 


De volgende screenshots hoe stapsgewijze implementatie van HANA S/4 op Microsoft Azure. Het proces werkt op dezelfde manier voor andere oplossingen likeBW/4 HANA.


![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-1b.jpg)

De eerste afbeelding ziet u alle CAL HANA van SAP-oplossingen die beschikbaar op Microsoft Azure zijn.
Exemplarily de 'SAP S/4 HANA on-premises edition' (de oplossing aan de onderkant van het screenshot) is gekozen om te gaan door het proces.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-2.jpg)

Eerst heeft een nieuwe SAP-CAL-account moet worden gemaakt. Er zijn twee mogelijkheden voor Azure - standaard Azure en Azure in vasteland China die wordt beheerd door 21Vianet partner.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic3b.jpg)

Een heeft vervolgens de Azure abonnement-ID invoeren die zijn te vinden op de Azure portal - Zie ook verder naar beneden te werk. Daarna moet een certificaat Azure beheer worden gedownload.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic6b.jpg)

In de nieuwe Azure vindt één portal u het artikel 'Abonnementen' aan de linkerkant. Klik erop om alle actieve abonnementen voor de gebruiker weer te geven.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic7b.jpg)

Een van de abonnementen te selecteren en vervolgens te kiezen "Certificaten beheren" wordt uitgelegd dat er is een nieuw concept 'service beveiligings-principals' gebruiken voor het nieuwe model van Azure Resource Manager.
SAP-CAL voor dit nieuwe model nog niet is aangepast en nog steeds de 'klassieke' model en de voormalige Azure portal werken met beheer van certificaten vereist.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic4b.jpg)

Hier kunnen zien de voormalige Azure portal. Het uploaden van een certificaat beheer biedt SAP CAL de machtigingen voor het maken van virtuele machines met een abonnement van de klant. Onder de "ABONNEMENTEN" vindt tabblad een u de abonnement-ID die moet worden vermeld in de SAP-CAL-portal.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic5.jpg)

Op het tweede tabblad kan vervolgens het management certificaat dat is gedownload voordat van SAP CAL uploaden.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic8.jpg)

Selecteer het gedownloade bestand verschijnt een dialoogvenster met weinig.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic9.jpg)

Nadat het certificaat is geüpload de verbinding tussen SAP CAL en de klant Azure abonnement binnen SAP CAl kan worden getest. Een beetje bericht wordt weergegeven waarin wordt aangegeven dat de verbinding geldig is.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic10.jpg)

Een heeft een oplossing die moet worden geïmplementeerd en maakt een instantie selecteren na het instellen van een account.
Met "basic"-modus is het heel eenvoudig. Voer een instantienaam, kies een regio Azure en definiëren van de master-wachtwoord voor de oplossing.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic11.jpg)

Na enige tijd is afhankelijk van de grootte en complexiteit van de oplossing (een schatting wordt gegeven door SAP CAL) wordt weergegeven als 'actief' en is klaar voor gebruik. Het is zeer eenvoudig.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic12.jpg)

Sommige details van de oplossing een ziet welke VMs zijn geïmplementeerd. In dit geval zijn drie Azure VMs van verschillende grootte en doel gemaakt.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic13.jpg)

Op de Azure portal de virtuele machines kunt u vinden met dezelfde naam die is opgegeven in de SAP-CAL.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic14b.jpg)

Nu is het mogelijk om te verbinden met de oplossing via de knop verbinding maken in de portal SAP CAL. Het dialoogvenster weinig bevat een koppeling naar een handleiding die de standaardreferenties beschrijft voor het werken met de oplossing.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic15.jpg)

Een andere mogelijkheid is het aanmelden bij de client Windows VM en start u bijvoorbeeld de vooraf geconfigureerde SAP GUI.







