<properties
    pageTitle="Wat te doen bij een Azure onderbrekingen beïnvloeden Azure virtuele netwerken | Microsoft Azure"
    description="Informatie over wat te doen bij een Azure onderbrekingen Azure virtuele netwerken beïnvloeden."
    services="virtual-network"
    documentationCenter=""
    authors="NarayanAnnamalai"
    manager="jefco"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="virtual-network"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="narayan;aglick"/>

#<a name="virtual-network--business-continuity"></a>Virtueel netwerk – bedrijfscontinuïteit

##<a name="overview"></a>Overzicht

Een virtueel netwerk (VNet) is een logische weergave van uw netwerk in de cloud. Hiermee kunt u uw eigen persoonlijke IP-adresruimte definiëren en het netwerk te segmenteren in subnetten. VNets fungeert als een barrière vertrouwen voor het hosten van uw resources compute zoals Azure virtuele Machines en Cloud Services (web/werknemer rollen). Een VNet kan rechtstreekse particuliere IP-communicatie tussen de bronnen die worden gehost in het. Een virtueel netwerk, kunnen ook worden gekoppeld aan een netwerk op locatie via een van de hybride-opties, zoals een VPN-Gateway of ExpressRoute.
 
Een VNet wordt binnen het bereik van een regio gemaakt. U kunt VNets maken met dezelfde adresruimte in twee verschillende regio's (d.w.z. ons Oost en West ons maar niet met ze elkaar direct). 

##<a name="business-continuity"></a>Bedrijfscontinuïteit

Er zijn verschillende manieren dat de toepassing kan worden verstoord. Een bepaald gebied kan worden volledig afgesneden als gevolg van een natuurramp of een gedeeltelijke noodsituatie als gevolg van een storing van meerdere apparaten en/of services. De impact op de VNet-service verschilt voor elk van deze situaties.

**V: wat moet u doen in geval van een storing op een hele regio? dat wil zeggen als een gebied volledig cutoff als gevolg van een natuurramp? Wat gebeurt er met de virtuele netwerken die zijn ondergebracht in de regio?**

A: het virtuele netwerk en de bronnen in de betrokken regio blijft gedurende de tijd van de onderbrekingen toegankelijk zijn.

![Eenvoudige virtuele netwerkdiagram](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**V: wat kan ik hetzelfde virtuele netwerk in een andere regio opnieuw te maken?**

A: virtual Network (VNet) is een tamelijk licht bron. U kunt oproepen Azure-API's voor het maken van een VNet met dezelfde adresruimte in een andere regio. Om dezelfde omgeving die aanwezig in het gebied waarin dit probleem optreedt was opnieuw te maken, hebt u de API-aanroepen Cloud Services (web/werknemer rollen) en virtuele Machines die u had opnieuw te implementeren. U moet draaien om een VPN-Gateway en verbinding maken met uw netwerk op ruimten als u had op gebouwen-connectiviteit (bijvoorbeeld in een implementatie van hybride).

De instructies voor het maken van een VNet worden gevonden [hier](./virtual-networks-create-vnet-arm-pportal.md). 

**V: kan een replica van een VNet in een bepaald gebied worden opnieuw gemaakt in een ander gebied tevoren?**

A: Ja, kunt u twee VNets met dezelfde persoonlijke IP-adresruimte en bronnen in twee verschillende regio's tijdig. Als een klant is hosting services in het VNet toegankelijk via internet, kunnen ze hebt ingesteld van verkeer Manager geo-route-verkeer naar de regio die actief is. Echter kan geen een klant twee VNets verbinding met dezelfde adresruimte met hun netwerk op ruimten zoals dit leiden problemen met routing tot zou. Op het moment van een noodsituatie en het verlies van een VNet in een regio, een klant verbinding kunt maken de andere VNet in de regio beschikbaar met dezelfde adresruimte met hun netwerk op gebouwen.

De instructies voor het maken van een VNet worden gevonden [hier](./virtual-networks-create-vnet-arm-pportal.md).
