<properties 
    pageTitle="Meer informatie over deze beperking in BizTalk-Services | Microsoft Azure" 
    description="Informatie over drempels te beperken en ten gevolge van runtime-gedrag voor BizTalk-Services. Bandbreedtebeperking is gebaseerd op het geheugengebruik en een aantal berichten. MAB, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>





# <a name="biztalk-services-throttling"></a>BizTalk-Services: beperken

Azure BizTalk-Services implementeren service beperken op basis van twee voorwaarden: geheugengebruik en het aantal gelijktijdige berichten verwerken. In dit onderwerp worden de bandbreedteregeling drempels en het Runtime-gedrag wordt beschreven als een voorwaarde voor bandbreedteregeling plaatsvindt.

## <a name="throttling-thresholds"></a>Beperking van de drempels

De volgende tabel worden de drempelwaarden en bandbreedteregeling bron:

||Beschrijving|Lage drempel|Hoge drempel|
|---|---|---|---|
|Geheugen|% van totale systeem geheugen beschikbaar/PageFileBytes. <p><p>Totale beschikbare PageFileBytes is ongeveer 2 maal het RAM-geheugen van het systeem.|60%|70%|
|Verwerking van berichten|Aantal berichten tegelijkertijd verwerken|40 * aantal cores|100 * aantal cores|

Als een hoge drempel is bereikt, begint de Azure BizTalk-Services beperken. Bandbreedtebeperking stopt wanneer de lage drempel is bereikt. Uw service gebruikt bijvoorbeeld systeemgeheugen 65%. In dit geval wordt de service niet beperken. De service wordt gestart met behulp van het systeemgeheugen van 70%. In dit geval wordt de service bandbreedte en wordt voortgezet totdat de service 60% (lage drempel) geheugen gebruikt beperken.

Azure BizTalk-Services worden bijgehouden voor de bandbreedteregeling status (status Normaal versus beperkt staat) en de bandbreedteregeling duur.


## <a name="runtime-behavior"></a>Runtime-gedrag

Wanneer u Azure BizTalk-Services in een bandbreedteregeling staat, gebeurt het volgende:

- Bandbreedtebeperking is per rol exemplaar. Bijvoorbeeld:<br/>
Het beperken van RoleInstanceA. RoleInstanceB is niet beperken. In deze situatie verwerkt berichten in RoleInstanceB zoals verwacht. Berichten in de RoleInstanceA worden verwijderd en mislukken met het volgende foutbericht:<br/><br/>
**De server is bezet. Probeer het opnieuw.**<br/><br/>
- Een pull-bronnen niet controleren of een bericht te downloaden. Bijvoorbeeld:<br/>
Een pijpleiding haalt berichten uit een externe FTP-bron. De rol van instantie doet de pull haalt in een bandbreedteregeling staat. In dit geval stopt de pijpleiding extra berichten gedownload totdat u stopt met het exemplaar van de rol beperken.
- Een antwoord wordt naar de client verzonden zodat de client kan het bericht opnieuw.
- U moet wachten tot de beperking opgelost is. Met name moet u wachten totdat de lage drempel is bereikt.

## <a name="important-notes"></a>Belangrijke opmerkingen
- Deze methode kan niet worden uitgeschakeld.
- Drempels beperking kan niet worden gewijzigd.
- Bandbreedtebeperking is hele systeem geïmplementeerd.
- De SQL-databaseserver Azure heeft ook ingebouwde beperken.

## <a name="additional-azure-biztalk-services-topics"></a>Extra onderwerpen over het Azure BizTalk-Services

-  [De Azure BizTalk-Services SDK installeren](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Zelfstudie: Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Hoe kan ik starten via de Azure BizTalk-Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Zie ook
- [BizTalk-Services: Ontwikkelaar, Basic, Standard en Premium-edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk-Services: Met behulp van Azure klassieke portal inrichten](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk-Services: Status grafiek inrichten](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk-Services: Tabbladen voor het Dashboard, de Monitor en de schaal](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk-Services: Back-up en terugzetten](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk-Services: Naam van de uitgevende instelling en de sleutel van de uitgevende instelling](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
 
