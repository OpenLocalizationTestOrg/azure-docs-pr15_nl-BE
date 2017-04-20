<properties 
    pageTitle="Maken en terugzetten van een back-up in BizTalk-Services | Microsoft Azure" 
    description="BizTalk-Services bevat een back-up en terugzetten. Meer informatie over het maken en terugzetten van een back-up en bepalen waarvan een back-wordt gemaakt. MAB, WABS" 
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


# <a name="biztalk-services-backup-and-restore"></a>BizTalk-Services: Back-up en terugzetten

Azure BizTalk-Services omvat mogelijkheden voor back-up en terugzetten. In dit onderwerp wordt beschreven hoe u een back-up en terugzetten van BizTalk-Services met behulp van de klassieke Azure portal.

U kunt ook back-up maken met de [BizTalk-Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584)BizTalk-Services. 

> [AZURE.NOTE] Hybride verbindingen worden niet gekopieerd, ongeacht de versie. U moet uw hybride verbindingen opnieuw maken.

## <a name="before-you-begin"></a>Voordat u begint

- Back-up en herstel mogelijk niet beschikbaar voor alle edities. Zie [BizTalk-Services: edities grafiek](biztalk-editions-feature-chart.md).

- De klassieke Azure portal gebruikt, kunt u een On Demand back-up maken of een geplande back-up maken. 

- Back-inhoud kan worden hersteld naar dezelfde BizTalk Service of een nieuwe BizTalk-Service. Als u de BizTalk-Service met dezelfde naam herstellen, de bestaande BizTalk-Service moet worden verwijderd en de naam moet beschikbaar zijn. Nadat u een BizTalk-Service hebt verwijderd, kost het meer tijd dan wilde voor dezelfde naam beschikbaar is. Als u geen tijd voor dezelfde naam beschikbaar is, zet u een nieuwe BizTalk-Service.

- BizTalk-Services kunnen worden teruggezet naar dezelfde editie of een hogere versie. BizTalk-Services teruggezet naar een lagere versie, wordt van wanneer de back-up is gemaakt, niet ondersteund.

    Bijvoorbeeld kan een back-up met behulp van de Basic Edition worden hersteld naar de Premium Edition. Een back-up met behulp van de Premium Edition worden niet hersteld naar de Standard Edition.

- EDI-controlenummers zijn een back-up de continuïteit van de nummers van het besturingselement. Berichten worden verwerkt na de laatste back-up, herstel van deze back-inhoud kan leiden tot dubbele controle getallen.

- Als een partij actieve berichten heeft, verwerking van de batch **voordat** u een back-up. Bij het maken van een back-up (als vereiste of geplande), worden berichten in batches worden nooit opgeslagen. 

    **Als u een back-up wordt genomen met actieve berichten in een batch, deze berichten worden geen back-up gemaakt en daarom verloren.**

- Optioneel: In de Portal BizTalk-Services stoppen alle beheertaken uit te voeren.


## <a name="create-a-backup"></a>Een back-up maken

Een back-up op elk gewenst moment kan worden gehouden en wordt volledig door u beheerd. In deze sectie worden de stappen voor het maken van back-ups met de klassieke Azure portal, met inbegrip van:

[Op vraag back-up](#backupnow)

[Een back-up plannen](#backupschedule)

#### <a name="backupnow"></a>Op vraag back-up
1. In de klassieke Azure portal, **BizTalk-Services**selecteren en selecteert u de BizTalk-Service u wilt een back-up.
2. Selecteer in het tabblad **Dashboard** onderaan de pagina een **Back-up** .
3. Voer de naam van een back-up. Voer bijvoorbeeld *myBizTalkService*BU*datum*.
4. Een blob storage kiezen en selecteert u het selectievakje Start de back-up.

Nadat de back-up is voltooid, wordt een container met back-up die naam gemaakt in de opslag. Deze container bevat de back-up van uw BizTalk-Service.

#### <a name="backupschedule"></a>Een back-up plannen

1. In de klassieke Azure portal, **BizTalk-Services**selecteren, selecteert u de naam van het BizTalk-Service die u wilt dat de back-up plannen en selecteert u het tabblad **configureren** .
2. De **Status van de reservekopie** instellen op **automatisch**. 
3. Selecteer de **Account van de opslag** de back-up opslaat, geeft u de **frequentie** wilt maken van de back-ups, en hoe lang houden de back-ups (**Bewaren dagen**):

    ![][AutomaticBU]

    **Notities**   
    - In **Dagen bewaren**, moet de bewaartermijn groter zijn dan de frequentie van de back-up.
    - Selecteer **altijd ten minste één back-up te houden**, zelfs als het voorbij de bewaartermijn.
    

4. Selecteer **Opslaan**.


Wanneer een geplande reservekopietaak wordt uitgevoerd, wordt een container (voor het opslaan van back-upgegevens) gemaakt in de opslag-account die u hebt ingevoerd. De naam van de container met de naam *Service BizTalk-naam-/-tijd*. 

Als het dashboard BizTalk-Service de status **mislukt** :

![Laatste status van geplande back-up][BackupStatus] 

De koppeling opent de logboeken voor Management Services om u te helpen oplossen. Zie [BizTalk-Services: problemen met Logboeken oplossen](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Herstellen

U kunt back-ups terugzetten uit de klassieke Azure portal of de [BizTalk-Service REST API herstellen](http://go.microsoft.com/fwlink/p/?LinkID=325582). In deze sectie worden de stappen terugzetten met behulp van de klassieke portal.

#### <a name="before-restoring-a-backup"></a>Voordat u een back-up terugzetten

- Nieuwe bijhouden, archivering en winkels controle kunnen worden ingevoerd bij het terugzetten van een BizTalk-Service.

- Dezelfde gegevens EDI Runtime wordt hersteld. Back-up van de EDI-Runtime worden de nummers van het besturingselement opgeslagen. De herstelde controlenummers worden op volgorde van het tijdstip van de back-up. Berichten worden verwerkt na de laatste back-up, herstel van deze back-inhoud kan leiden tot dubbele controle getallen.

#### <a name="restore-a-backup"></a>Een back-up terugzetten

1. Selecteer **Nieuw**in het portal voor Azure klassieke > **App Services** > **Service BizTalk-** > **herstellen**:

    ![Een back-up terugzetten][Restore]

2. In **Back-URL**, selecteer het mappictogram en vouw de Azure opslag account die de back-up configuratie BizTalk-Service worden opgeslagen. Vouw de container en selecteer het desbetreffende back-up van txt-bestand in het rechterdeelvenster. 
<br/><br/>
Selecteer **openen**.

3. Voer een **BizTalk-servicenaam** en controleer of het **Domein URL**, de **editie**en de **regio** voor de teruggezette BizTalk-Service op de pagina **Restore BizTalk-Service** . **Een nieuw exemplaar van de SQL-database maken** voor het bijhouden van database:

    ![][RestoreBizTalkService]

    Selecteer de pijl volgende gemarkeerd.

4.  Controleer of de naam van de SQL-database, de fysieke server waar u de SQL-database wordt gemaakt en een gebruikersnaam en wachtwoord invoeren voor die server.


    Als u wilt dat de SQL-database edition configureren, selecteer grootte en andere eigenschappen, **Geavanceerde Database-instellingen configureren**. 

    Selecteer de pijl volgende gemarkeerd.

5. Maak een nieuwe account voor opslag of een bestaande opslag account invoeren voor de BizTalk-Service.

7. Schakel het selectievakje Start het terugzetten.

Nadat de herstelbewerking is uitgevoerd, wordt een nieuwe BizTalk-Service op de pagina BizTalk-Services in de klassieke Azure portal geschorst vermeld.



### <a name="postrestore"></a>Na het terugzetten van een back-up

De BizTalk-Service is altijd in een **onderbroken** toestand hersteld. In deze status kunt u eventuele wijzigingen in de configuratie maken voordat de nieuwe omgeving functioneel is, met inbegrip van:

- Als u BizTalk-Service-toepassingen met behulp van de SDK van Azure BizTalk-Services hebt gemaakt, mogelijk moet u de referenties van de Access Control (ACS) in deze toepassingen om te werken met de herstelde omgeving bijwerken.

- U kunt een BizTalk-Service om een bestaande omgeving van BizTalk-Service repliceert terugzetten. In dit geval als er overeenkomsten in de oorspronkelijke BizTalk-Services portal worden geconfigureerd met een FTP-map, wellicht voor het bijwerken van de overeenkomsten in de herstelde omgeving naar een andere FTP-map te gebruiken. Anders kunnen er twee verschillende overeenkomsten probeert hetzelfde bericht ophalen.

- Als u voor omgevingen met meerdere BizTalk-Service hebt teruggezet, moet dat u de juiste omgeving in de Visual Studio-toepassingen, PowerShell-cmdlets, REST API's of Trading Partner OM API's richten.

- Het is een goede gewoonte om geautomatiseerde back-ups configureren op de herstelde BizTalk Service-omgeving.

De BizTalk-Service starten in de klassieke Azure portal, selecteert u de herstelde BizTalk-Service en selecteer **hervatten** op de taakbalk. 



## <a name="what-gets-backed-up"></a>Wat met deze eigenschap wordt een back-up

Wanneer een back-up wordt gemaakt, de volgende items zijn een back-up:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Back-ups van items</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk-serviceportal</strong></td>
</tr> 
<tr>
<td>Configuratie en Runtime</td> 
<td>
<ul>
<li>Partner en profiel details</li>
<li>Partner-overeenkomsten</li>
<li>Aangepaste assembly's geïmplementeerd</li>
<li>Bridges geïmplementeerd</li>
<li>Certificaten</li>
<li>Transformaties geïmplementeerd</li>
<li>Pijpleidingen</li>
<li>Sjablonen gemaakt en opgeslagen in de BizTalk-Services Portal</li>
<li>X12 ST01 en GS01-toewijzingen</li>
<li>Controlenummers (EDI)</li>
<li>AS2 bericht MIC-waarden</li>
</ul>
</td>
</tr> 
 
<tr>
<td colspan="2">
 <strong>Azure BizTalk-Service</strong></td>
</tr> 
<tr>
<td>SSL-certificaat</td> 
<td>
<ul>
<li>SSL certificaatgegevens</li>
<li>Wachtwoord voor SSL-certificaat</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk-Service-instellingen</td> 
<td>
<ul>
<li>Aantal schaal-eenheden</li>
<li>Edition</li>
<li>Versie van het product</li>
<li>Regio/Datacenter</li>
<li>Access Control-Service (ACS)-naamruimte en de sleutel</li>
<li>De verbindingsreeks bijhouden</li>
<li>Opslag account verbindingsreeks archiveren</li>
<li>Verbindingsreeks opslag-account controleren</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Extra artikelen</strong></td>
</tr> 
<tr>
<td>Database voor bijhouden</td> 
<td>Wanneer u de BizTalk-Service maakt, worden de details van het bijhouden van Database ingevoerd, inclusief de Azure SQL-databaseserver en de naam van de Database bijhouden. De Database bijhouden is niet automatisch een reservekopie.
<br/><br/>
<strong>Belangrijk</strong><br/>
Als de Database bijhouden wordt verwijderd en de databasebehoeften wordt hersteld, moet een vorige back-up bestaat. Als u een back-up niet bestaat, zijn de Database bijhouden en de gegevens niet terug te vorderen. In dit geval een nieuwe Database voor bijhouden van te maken met dezelfde naam van de database. Geo-replicatie wordt aanbevolen.</td>
</tr> 
</table>

## <a name="next"></a>Volgende

Azure BizTalk-Services in de klassieke Azure portal maken, gaat u naar [BizTalk-Services: klassiek ingericht met behulp van Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=302280). Als u wilt beginnen met het maken van toepassingen, gaat u naar [Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

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

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png
 
