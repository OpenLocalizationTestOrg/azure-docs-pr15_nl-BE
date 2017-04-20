<properties
    pageTitle="Wat te doen bij een Azure service onderbrekingen die invloed heeft op de Azure sleutel kluis | Microsoft Azure"
    description="Informatie over wat te doen in geval van een onderbreking Azure service die van invloed is op de Azure sleutel kluis."
    services="key-vault"
    documentationCenter=""
    authors="adamglick"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="key-vault"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="sumedhb;aglick"/>


# <a name="azure-key-vault-availability-and-redundancy"></a>Azure sleutel kluis beschikbaarheid en redundantie

Azure sleutel kluis is uitgerust met meerdere lagen van redundantie om ervoor te zorgen dat uw sleutels en geheimen beschikbaar voor de toepassing blijven ook als afzonderlijke onderdelen van de service mislukt.

De inhoud van de sleutel kluis worden gerepliceerd in het gebied en naar een secundaire regio ten minste 150 mijl weg, maar binnen de dezelfde Geografie. Dit houdt de hoge duurzaamheid van uw sleutels en geheimen.

Als afzonderlijke componenten in de sleutel kluis-service niet, stap alternatieve onderdelen in het gebied te dienen uw aanvraag om ervoor te zorgen dat er geen afname van de functionaliteit is. Niet hoeft u niets te doen om dit te activeren. Dit is gebeurt automatisch en transparant voor de gebruiker.

In het zeldzame geval dat een gehele Azure gebied niet beschikbaar is, worden de aanvragen die u van Azure sleutel kluis in dat gebied automatisch gerouteerd (*failover*) naar een secundaire regio. Als de primaire regio opnieuw beschikbaar is, de aanvragen worden gerouteerd back (*kan niet terug*) naar de primaire regio. Nogmaals, niet hoeft geen actie te ondernemen omdat dit automatisch gebeurt.

Er zijn een paar valkuilen rekening houden met:

* Bij een failover regio duurt het enkele minuten duren voordat de failover-service. Aanvragen die worden uitgevoerd tijdens deze periode kunnen mislukken nadat de failover is voltooid.
* Nadat een failover uitgevoerd is, is uw sleutel kluis in de modus alleen-lezen. Aanvragen die in deze modus worden ondersteund zijn:
 * Lijst met sleutels kluizen
 * Opvragen van eigenschappen van sleutels kluizen
 * Lijst met geheimen
 * Geheimen ophalen
 * Lijst met sleutels
 * (Eigenschappen van) sleutels ophalen
 * Coderen
 * Decoderen
 * Automatische terugloop
 * Pak
 * Controleer of
 * Aanmelden
 * Back-up
* Nadat een failover is opnieuw mislukt, worden alle typen (met inbegrip van aanvragen voor lezen *en* schrijven) beschikbaar zijn.
