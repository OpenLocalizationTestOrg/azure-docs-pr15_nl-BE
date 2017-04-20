<properties
    pageTitle="Informatie over functies in BizTalk-Services edities | Microsoft Azure"
    description="Vergelijk de mogelijkheden van de versies van BizTalk-Services: vrij, Developer, Basic, Standard en Premium. MAB, WABS."
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
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="biztalk-services-editions-chart"></a>BizTalk-Services: Edities grafiek

Azure BizTalk-Services biedt verschillende versies. In dit artikel gebruiken om te bepalen welke versie geschikt is voor uw scenario en zakelijke behoeften.


## <a name="compare-the-editions"></a>De edities vergelijken

**Vrij (voorbeeld)**

Kunt maken en beheren van hybride verbindingen. Een hybride verbinding is een eenvoudige manier om een website Azure verbinden met een systeem van gebouwen, zoals SQL-Server.

**Ontwikkelaar**

Verwerking van hybride verbindingen, EAI en EDI berichten met een eenvoudig te gebruiken trading partner beheerportal en ondersteuning voor gemeenschappelijke EDI-schema's en rijke EDI verwerking bevat via X12 en AS2. Algemene scenario's voor EAI services in de cloud verbinden met een HTTP/S, REST, FTP, WCF en SFTP-protocollen voor lezen en schrijven van berichten kan worden gemaakt.  Connectiviteit met LOB-systemen voor ruimten met kant-en-klare SAP, Oracle eBusiness DB Oracle, Siebel en SQL Server-adapters gebruiken. Een ontwikkelaar-georiënteerde omgeving gebruiken met Visual Studio tools voor eenvoudige ontwikkeling en implementatie. Beperkt tot ontwikkeling en test doeleinden alleen met geen Service niveau overeenkomst (SLA).

**Basic**

Bevat het merendeel van de mogelijkheden voor ontwikkelaars met verhogingen in hybride verbindingen, EAI bruggen, EDI-overeenkomsten, BizTalk-Adapter Pack verbindingen en. Biedt ook een hoge mate van beschikbaarheid en de optie voor het schalen met een Service niveau overeenkomst (SLA).

**Standaard**

Bevat alle essentiële mogelijkheden met verhogingen in hybride verbindingen, EAI bruggen EDI overeenkomsten en BizTalk-Adapter Pack verbindingen. Biedt ook een hoge mate van beschikbaarheid en de optie voor het schalen met een Service niveau overeenkomst (SLA).

**Premium**

Bevat de standaard mogelijkheden met verhogingen in hybride verbindingen, EAI bruggen EDI overeenkomsten en BizTalk-Adapter Pack verbindingen. Bevat ook archiveren, hoge beschikbaarheid en de optie voor het schalen met een Service niveau overeenkomst (SLA).


## <a name="editions-chart"></a>Grafiek van de edities
De volgende tabel worden de verschillen.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Vrij (voorbeeld)</th>
        <th>Ontwikkelaar</th>
        <th>Basic</th>
        <th>Standaard</th>
        <th>Premium</th>
</tr>

<tr>
<td><strong>Startprijs</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Prijzen Azure BizTalk-Services</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full">Azure prijzen Calculator</a></td>
</tr>
<tr>
<td><strong>Minimale standaard-configuratie</strong></td>
<td>1 gratis eenheid</td>
<td>Unit 1-ontwikkelaar</td>
<td>Basiseenheid 1</td>
<td>1 standaard eenheid</td>
<td>1 eenheid</td>
</tr>
<tr>
<td><strong>Schaal</strong></td>
<td>Geen schaal</td>
<td>Geen schaal</td>
<td>Ja, in stappen van 1 basiseenheid</td>
<td>Ja, in stappen van 1 standaard eenheid</td>
<td>Ja, in stappen van 1 eenheid</td>
</tr>
<tr>
<td><strong>Maximaal toegestane schaal van</strong></td>
<td>Geen schaal</td>
<td>Geen schaal</td>
<td>Maximaal 8 eenheden</td>
<td>Maximaal 8 eenheden</td>
<td>Maximaal 8 eenheden</td>
</tr>
<tr>
<td><strong>EAI bruggen per eenheid</strong></td>
<td>Niet opgenomen.</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>EDI, AS2</strong>
<br/><br/>
Omvat de overeenkomsten TPM</td>
<td>Niet opgenomen.</td>
<td>Opgenomen. 10 overeenkomsten per eenheid.</td>
<td>Opgenomen. 50 overeenkomsten per eenheid.</td>
<td>Opgenomen. 250 overeenkomsten per eenheid.</td>
<td>Opgenomen. 1000 overeenkomsten per eenheid.</td>
</tr>
<tr>
<td><strong>Hybride verbindingen per eenheid</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Hybride verbinding gegevens overbrengen (GB) per eenheid</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>BizTalk-Adapter Service verbindingen met LOB-systemen voor bedrijven</strong></td>
<td>Niet opgenomen.</td>
<td>1 verbinding</td>
<td>2 verbindingen</td>
<td>5 verbindingen</td>
<td>25 verbindingen</td>
</tr>
<tr>
<td align="left"><strong>Ondersteunde protocollen/systemen:</strong>
<ul>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Service Bus (SB)</li>
<li>Azure Blob</li>
<li>REST API 's</li>
</ul>
</td>
<td>Niet opgenomen.</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
</tr>
<tr>
<td><strong>Hoge beschikbaarheid</strong>
<br/><br/>
Voor Service niveau overeenkomst (SLA) Zie <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Prijzen van Azure BizTalk-Services</a>.
</td>
<td>Niet opgenomen.</td>
<td>Niet opgenomen.</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
</tr>
<tr>
<td><strong>Back-up en terugzetten</strong></td>
<td>Niet opgenomen.</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
</tr>
<tr>
<td><strong>Bijhouden</strong></td>
<td>Niet opgenomen.</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
</tr>
<tr>
<td><strong>Archivering</strong><br/><br/>
Bevat niet-afwijzing van ontvangst (NRR) en bijgehouden berichten downloaden</td>
<td>Niet opgenomen.</td>
<td>Opgenomen</td>
<td>Niet opgenomen.</td>
<td>Niet opgenomen.</td>
<td>Opgenomen</td>
</tr>
<tr>
<td><strong>Gebruik van aangepaste code</strong></td>
<td>Niet opgenomen.</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
</tr>
<tr>
<td><strong>Gebruik van transformaties, met inbegrip van aangepaste XSLT</strong></td>
<td>Niet opgenomen.</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
</tr>
</table>

> [AZURE.NOTE] Voor meer herstellingsvermogen tegen hardwarestoringen betekent hoge beschikbaarheid met meerdere VMs binnen één BizTalk-eenheid.


## <a name="faqs"></a>Veelgestelde vragen

#### <a name="what-is-a-biztalk-unit"></a>Wat is een BizTalk-eenheid?
Een "eenheid" is het atomaire niveau van een implementatie Azure BizTalk-Services. Elke editie is voorzien van een eenheid met verschillende computercapaciteit en geheugen. Bijvoorbeeld een basiseenheid heeft meer berekenen dan Developer, standaard meer berekenen dan de eenvoudige, enzovoort. Wanneer u een BizTalk-Service schaalt, u de schaal in termen van eenheden.

#### <a name="what-is-the-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Wat is het verschil tussen BizTalk-Services en Azure BizTalk-VM?
BizTalk-Services biedt een waar Platform-as-a-Service (PaaS)-architectuur voor het bouwen van oplossingen voor integratie in de cloud. Met het PaaS-model u volledig richten op de toepassingslogica en laat u alle van het beheer van de infrastructuur bij Microsoft, waaronder:

- Hoeft niet te beheren of de patch van virtuele machines.
- Microsoft garandeert de beschikbaarheid.
- U de schaal op verzoek met een eenvoudig verzoek om meer of minder capaciteit via de portal Azure bepalen.

Azure virtuele Machines van BizTalk Server biedt een infrastructuur-as-a-Service (IaaS)-architectuur. U virtuele machines maken en configureren net als uw omgeving op lokalen om bestaande toepassingen uitvoeren in de cloud zonder wijzigingen code te vereenvoudigen. Met IaaS bent u nog steeds verantwoordelijk voor het configureren van de virtuele machines, de virtuele machines (bijvoorbeeld installeren van software en OS patches) beheren en aanpassen van de toepassing voor maximale beschikbaarheid.

Als u naar nieuwe integratie-oplossingen die uw infrastructuur management inspanning te minimaliseren kijkt, kunt u BizTalk-Services gebruiken. Als u op zoek bent naar snel migreren van de bestaande BizTalk-oplossingen of op zoek naar een omgeving op aanvraag ontwikkelen en testen van BizTalk Server toepassingen, BizTalk-Server gebruiken op een Azure Virtual Machine.

#### <a name="what-is-the-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Wat is het verschil tussen de BizTalk-Adapter Service en hybride verbindingen?
De BizTalk-Adapter-Service wordt gebruikt door een Azure BizTalk-Service. De BizTalk-Adapter Service maakt het Pack voor BizTalk-Adapter verbinding met een systeem van de regel van Business (LOB) op gebouwen. Een hybride verbinding biedt een eenvoudige en handige manier Azure-toepassingen, zoals de functie Web Apps in Azure App Service en Azure Mobile Services, verbinding met een bron op gebouwen.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-the-limit-is-reached"></a>Wat betekent "Hybride verbinding gegevens overbrengen (GB) per eenheid" Is dit per minuut/uur/dag/week/maand? Wat gebeurt er wanneer de limiet wordt bereikt?

De hybride verbinding kosten per eenheid is afhankelijk van de editie van BizTalk-Services. Eenvoudig gezegd, kosten zijn afhankelijk van hoeveel gegevens u overbrengen. Bijvoorbeeld kosten minder dan 100 GB per dag overbrengen gegevensoverdracht 10 GB per dag. De [Rekenmachine prijzen](https://azure.microsoft.com/pricing/calculator/?scenario=full) voor BizTalk-Services gebruiken om te bepalen van de specifieke kosten. Meestal worden de limieten dagelijks afgedwongen. Als u de limiet overschrijdt, wordt een overdosering belast tegen het tarief van $1 per GB.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-the-number-of-bridges-go-up-by-two-instead-of-just-one"></a>Wanneer ik een overeenkomst in BizTalk-Services maakt, waarom het aantal bruggen gaat omhoog met twee in plaats van één?

Elke overeenkomst omvat twee verschillende bruggen: een brug verzenden naast communicatie en een ontvangen naast communicatie bridge.

####  <a name="what-happens-when-i-hit-the-quota-limit-on-the-number-of-bridges-or-agreements"></a>Wat gebeurt er als ik de quotumlimiet voor het aantal bruggen of overeenkomsten geraakt?

U bent niet kunnen alle nieuwe bruggen implementeren of maken van nieuwe overeenkomsten. Als u wilt meer implementeert, moet u uitgebreid naar meer eenheden van de BizTalk-service of een upgrade naar een hogere editie.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-to-another"></a>Hoe ik migreren van een laag van BizTalk-Services naar de andere?

De gratis versie kan niet worden gemigreerd of 'aangepast' aan een andere laag niet een back-up en terugzetten naar een andere laag. Als u een andere laag, moet u een nieuwe BizTalk-Service met behulp van de nieuwe laag maken. Alle artefacten gemaakt met behulp van de gratis editie, met inbegrip van hybride verbindingen, moeten opnieuw worden gemaakt in de nieuwe BizTalk-Service. 

Voor de overige edities met de back-up en herstel voor de migratie van de artefacten van één laag naar de andere. Bijvoorbeeld, back-up van uw artefacten in de standaard laag en zet naar de premie laag. [BizTalk-Services: een back-up en terugzetten](biztalk-backup-restore.md) beschrijving van de ondersteunde migratiescenario paden en geeft een overzicht van welke artefacten zijn een back-up. Houd er rekening mee dat hybride verbindingen zijn niet een back-up. Nadat de back-up en terugzetten naar een nieuwe laag, u vervolgens opnieuw maken de hybride verbindingen.  


#### <a name="is-the-biztalk-adapter-service-included-in-the-service-how-do-i-receive-the-software"></a>Is de BizTalk-Adapter-Service die deel uitmaken van de service? Hoe ontvang ik de software?

Ja, de Service BizTalk-Adapter met de BizTalk-Adapter Pack zijn opgenomen in de Azure BizTalk-Services SDK [downloaden](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Volgende stappen

Azure BizTalk-Services maken in de portal Azure, gaat u naar [BizTalk-Services: ingericht met de Azure portal](biztalk-provision-services.md). Als u wilt beginnen met het maken van toepassingen, gaat u naar [Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>Aanvullende bronnen
- [BizTalk-Services: Inrichten met de Azure portal](biztalk-provision-services.md)<br/>
- [BizTalk-Services: Status grafiek inrichten](biztalk-service-state-chart.md)<br/>
- [BizTalk-Services: Tabbladen voor het Dashboard, de Monitor en de schaal](biztalk-dashboard-monitor-scale-tabs.md)<br/>
- [BizTalk-Services: Back-up en terugzetten](biztalk-backup-restore.md)<br/>
- [BizTalk-Services: beperken](biztalk-throttling-thresholds.md)<br/>
- [BizTalk-Services: Naam van de uitgevende instelling en de sleutel van de uitgevende instelling](biztalk-issuer-name-issuer-key.md)<br/>
- [Hoe kan ik starten via de Azure BizTalk-Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
