<properties 
    pageTitle="Problemen oplossen met behulp van Logboeken voor BizTalk-Services | Microsoft Azure" 
    description="Problemen oplossen met BizTalk-Services met behulp van Logboeken. MAB, WABS" 
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


# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk-Services: Problemen oplossen met behulp van Logboeken

## <a name="what-are-the-operation-logs"></a>Wat zijn de logboeken voor
Logboeken is een functie Management Services is beschikbaar in de Azure klassieke portal waarmee u historische logboeken van bewerkingen die worden uitgevoerd op de Azure services, met inbegrip van de BizTalk. Hiermee kunt u historische gegevens met betrekking tot het van beheerbewerkingen op de BizTalk-Service abonnement tot 180 dagen weergeven.

> [AZURE.NOTE]Deze functie alleen wordt vastgelegd de logboeken van het beheer van BizTalk-Services, zoals wanneer de service is gestart, back-up, enzovoort. Dergelijke transacties worden bijgehouden ongeacht of ze uit de klassieke Azure portal of via de [BizTalk-REST API's](http://msdn.microsoft.com/library/azure/dn232347.aspx)worden uitgevoerd. Zie voor een volledige lijst van bewerkingen die worden bijgehouden met behulp van Management Services [Operations bijgehouden met behulp van Azure Management Services](#bizops).<br/><br/>
De logboeken voor activiteiten in verband met de uitvoering van de BizTalk-Service (zoals het bericht verwerkt door de bruggen, enzovoort.), wordt dit niet vastgelegd. Om deze logboeken wilt weergeven, gebruikt u de weergave bijhouden via de portal BizTalk-Services. Zie [Berichten bijhouden](http://msdn.microsoft.com/library/azure/hh949805.aspx)voor meer informatie.

## <a name="view-biztalk-services-operation-logs"></a>BizTalk-Services Logboeken weergeven
1. In de klassieke Azure portal **Management Services**selecteren en selecteer vervolgens het tabblad **Logboeken** .
2. U kunt de logboeken op basis van verschillende parameters, zoals een datumbereik, abonnement, servicetype (bijvoorbeeld BizTalk-Services), servicenaam of status van de bewerking (geslaagd, mislukt) filteren.
3. Schakel het selectievakje in om de gefilterde lijst weer te geven. De volgende afbeelding toont de activiteiten in verband met testbiztalkservice:  ![logboeken weergeven][ViewLogs] 
4. Meer over een bepaalde bewerking weergeven, selecteert u de rij en klikt u op **Details** in de taakbalk onder aan het.


## <a name="bizops"></a>Bewerkingen die worden bijgehouden met Azure Management Services
De volgende tabel worden de bewerkingen die worden bijgehouden met behulp van de Azure Services:

Naam van bewerking | Taak
--- | ---
CreateBizTalkService | Tijdens het maken van een nieuwe BizTalk-Service
DeleteBizTalkService | Bewerking is een BizTalk-Service verwijderen
RestartBizTalkService | Om een BizTalk-Service opnieuw te starten
StartBizTalkService | Bewerking een BizTalk-Service starten
StopBizTalkService | Bewerking een BizTalk-Service stoppen
DisableBizTalkService | Bewerking een BizTalk-Service uitschakelen
EnableBizTalkService | Bewerking een BizTalk-Service inschakelen
BackupBizTalkService | De bewerking back-up een BizTalk-Service
RestoreBizTalkService | Bewerking een BizTalk-Service uit de opgegeven back-up terugzetten
SuspendBizTalkService | Bewerking een BizTalk-Service onderbreken
ResumeBizTalkService | Bewerking te hervatten, een BizTalk-Service
ScaleBizTalkService | De bewerking voor het schalen van een BizTalk-Service omhoog of omlaag
ConfigUpdateBizTalkService | De bewerking voor het bijwerken van de configuratie van een BizTalk-Service
ServiceUpdateBizTalkService | Bewerking wilt upgraden of downgraden van een BizTalk-Service naar een andere versie
PurgeBackupBizTalkService | De bewerking back-ups van de BizTalk-Service buiten de bewaarperiode wissen


## <a name="see-also"></a>Zie ook
- [Back-up van BizTalk-Service](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [BizTalk-Service herstellen vanaf back-up](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [BizTalk-Services: Ontwikkelaar, Basic, Standard en Premium-edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalk-Services: Met behulp van Azure klassieke portal inrichten](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [BizTalk-Services: Status grafiek inrichten](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk-Services: Tabbladen voor het Dashboard, de Monitor en de schaal](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [BizTalk-Services: beperken](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalk-Services: Naam van de uitgevende instelling en de sleutel van de uitgevende instelling](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Hoe kan ik starten via de Azure BizTalk-Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png
 
