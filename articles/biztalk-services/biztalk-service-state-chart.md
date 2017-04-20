<properties 
    pageTitle="Taken die zijn toegestaan in andere lidstaten of statussen in BizTalk-Services | Microsoft Azure" 
    description="De acties/bewerkingen toegestaan in andere MAB-status: stoppen, starten, opnieuw starten, onderbreken, hervatten, verwijderen, schalen, bijwerken configuratie en back" 
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



# <a name="biztalk-services-service-state-chart"></a>BizTalk-Services: Service staat grafiek
Er zijn bewerkingen die u kunt uitvoeren op de BizTalk-service of afhankelijk van de huidige status van de BizTalk-service.

Zo kunt u een nieuwe BizTalk-service in de klassieke Azure portal inrichten. Wanneer deze is voltooid, wordt de BizTalk-service is in actieve staat. In de actieve weergave kunt u de BizTalk-service stoppen. Als geslaagd is, wordt de BizTalk-service een staat gestopt. Als stoppen is mislukt, wordt de BizTalk-service een StopFailed staat. In de StopFailed staat, kunt u de BizTalk-service opnieuw starten. Als u een bewerking die niet is toegestaan probeert, zoals cv de BizTalk-service het volgende foutbericht weergegeven:

**De bewerking is niet toegestaan.**

Voor het inrichten van een BizTalk-Service, gaat u naar [BizTalk-Services: klassiek ingericht met behulp van Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=302280).

De volgende tabellen bevatten de bewerkingen of bewerkingen die kunnen worden voltooid als de BizTalk-Service in een specifieke staat. Een ✔ betekent dat de bewerking is toegestaan in de lidstaat. Een lege waarde betekent dat de bewerking kan niet worden uitgevoerd in de lidstaat.

## <a name="start-stop-restart-suspend-resume-and-delete-operations"></a>Starten, stoppen, starten, onderbreken, hervatten en verwijderen
<table border="1">
<tr>
        <th colspan="15">Actie of bewerking</th>
</tr>

<tr>
        <th rowspan="18">De status van BizTalk-Service</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Start</th>
        <th>Stop</th>
        <th>Opnieuw opstarten</th>
        <th>Stand-by</th>
        <th>Cv</th>
        <th>Verwijderen</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Actieve</b></td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Uitgeschakeld</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Geschorst</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Gestopt</b></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Service-Update is mislukt</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
</table>
<br/>

## <a name="scale-update-configuration-and-backup-operations"></a>Schaal, configuratie voor updates en back-upbewerkingen
<table border="1">
<tr>
        <th colspan="15">Actie of bewerking</th>
</tr>

<tr>
        <th rowspan="18">De status van BizTalk-Service</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Schaal</th>
        <th>Configuratie bijwerken</th>
        <th>Back-up</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Actieve</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Uitgeschakeld</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Geschorst</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Gestopt</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Service-Update is mislukt</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
</tr>
</table>

## <a name="see-also"></a>Zie ook
- [BizTalk-Services: Met behulp van Azure klassieke portal inrichten](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk-Services: Tabbladen voor het Dashboard, de Monitor en de schaal](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk-Services: Ontwikkelaar, Basic, Standard en Premium-edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk-Services: Back-up en terugzetten](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk-Services: beperken](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalk-Services: Naam van de uitgevende instelling en de sleutel van de uitgevende instelling](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [Hoe kan ik starten via de Azure BizTalk-Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)


 
