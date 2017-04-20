<properties
   pageTitle="Test uw voorstel VM voor Marketplace | Microsoft Azure"
   description="Inzicht in uw image VM testen voor Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="hascipio" />

# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Uw voorstel VM voor Azure Marketplace in gefaseerde installatie testen

Fasering betekent het implementeren van de SKU in een privé "sandbox" kunt u testen en de functionaliteit te valideren voordat u deze implementeert op de markt. De SKU wordt weergegeven in de staging-net als voor een klant die gebruik heeft genomen. Uw VM image moet moet worden gepusht naar de staging gecertificeerd zijn.

## <a name="step-1-push-your-offer-to-staging"></a>Stap 1: Uw aanbieding staging Push

1. Klik op het tabblad **publiceren** **Push naar Staging**.

    ![tekening](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)

2. Als de Portal voor publiceren van eventuele fouten aangeeft, corrigeren.
3.  In de **wie toegang heeft tot uw voorstel gefaseerde?** dialoogvenster Geef de lijst met Azure abonnementen die u gebruiken wilt voor het bekijken van uw aanbieding in de [voorvertoning Azure portal](https://portal.azure.com).

    >[AZURE.NOTE] In het geval van virtuele Machines en oplossing sjablonen, moet u **niet het** "witte" lijst abonnementen van het type Cryptografieprovider DreamSpark of Azure in Open.


    > In het geval van virtuele Machines, dat wanneer u op de knop **DUWEN STAGING**, de volgende stappen worden uitgevoerd achter de scène. Is het mogelijk de voortgang van elke stap in het tabblad publiceren in de publicatie bekijken portal. U moet controleren deze pagina met regelmatig interval (totdat de status KLAARGEZET wordt) fout informatie die moet de correctie vanaf het einde.

    > - Uw aanvraag voor gefaseerde installatie wordt in eerste instantie de certificering team dat de vhd valideren. Echter, als uw aanvraag is alleen marketing wijzigen, wordt de certificeringsinstantie stap overgeslagen.
    > - Zodra het certificaat is voltooid, start de replicatie van de aanbieding via de Azure datacenters. Over het algemeen duurt 24-48hours voor de replicatie is voltooid maar duurt een week, afhankelijk van de grootte van de vhd. Echter, als uw verzoek is alleen marketing wijzigen, vervolgens de replicatie is sneller.
    > - Als de replicatie voltooid is, wordt de aanbieding in de [Azure portal](http:/portal.azure.com)beschikbaar zijn. Op die tijd de status in de publicatie worden KLAARGEZET portal. Een gefaseerde aanbieding wordt weergegeven in de [Azure portal](http:/portal.azure.com) met alleen de e-mail (s) die is gekoppeld aan het abonnement dat het aanbod wordt klaargezet.

4. Aanmelden bij de [Azure portal bekijken](https://portal.azure.com) met behulp van een van de Azure abonnementen die worden vermeld in de vorige stap.
5. Zoek uw voorstel en valideren van uw afbeelding VM punten:
  - Zorg ervoor dat marketing inhoud wordt weergegeven correct in de markt.
  - End-to-end implementatie van de VM-afbeelding.

      ![IMG-kaart-portal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [AZURE.IMPORTANT] Uw voorstel blijft in de staging-totdat u melden bij Microsoft via de Portal voor publiceren [tabblad**publiceren** > Klik op de knop **"aanvraag goedkeuring aan Push naar productie"**] u bent klaar om te duwen naar productie. Dit is een ideaal moment om alle leden van de controle van uw team beschikken over alles in voorbereiding op uw voorstel vermelde gaan.

> De staging-platform is ontworpen voor het testen van de aanbieding in een voorbeeldweergave door de uitgever. We ontmoedigen ten zeerste deze platofrm te gebruiken voor commerciële doeleinden.

## <a name="next-steps"></a>Volgende stappen
Nu uw voorstel 'klaargezet' en dat u de functionaliteit en de afzet van inhoud hebt getest, kunt u doorgaan naar de laatste fase voor publishing, **stap 4**: [implementatie van uw aanbod op de markt](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Zie ook
- [Aan de slag: het publiceren van een aanbod op de markt Azure](marketplace-publishing-getting-started.md)
