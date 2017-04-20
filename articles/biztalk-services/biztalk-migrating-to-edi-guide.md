<properties   
    pageTitle="BizTalk Server EDI oplossingen migreren naar BizTalk-Services technische handleiding | Microsoft Azure"
    description="EDI migreren naar MAB; Azure Microsoft BizTalk-Services"
    services="biztalk-services"
    documentationCenter="na"
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


# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>BizTalk Server EDI oplossingen migreren naar BizTalk-Services: technische handleiding

Auteur: Tim Wieman en Mirchandani Mehrotra

Revisoren: Karthik Bharthy

Geschreven met: Microsoft Azure BizTalk-Services – februari 2014 vrijgeven.

## <a name="introduction"></a>Inleiding

Elektronische Data Interchange (EDI) is een van de meest gangbare wijze waarop exchange bedrijven de gegevens in elektronische vorm, ook genoemd als B2B- of Business-to-Business. BizTalk Server heeft EDI-ondersteuning voor meer dan een decennium, sinds de eerste release van BizTalk Server. Met BizTalk-Services, heeft Microsoft de ondersteuning voor EDI oplossingen blijft op het platform Microsoft Azure. B2B-transacties zijn meestal buiten een organisatie en daarom is gemakkelijker te implementeren als het programma is uitgevoerd op een wolk platform. Microsoft Azure biedt deze mogelijkheden via de BizTalk-Services.

Sommige klanten kijkt naar BizTalk-Services als "greenfield" platform voor nieuwe EDI oplossingen, hebben veel klanten de huidige BizTalk Server EDI oplossingen die zij willen migreren naar Azure. Omdat EDI van BizTalk-Services is ontworpen kan op basis van dezelfde sleutel diensten als BizTalk Server EDI-architectuur (trading partners, diensten, overeenkomsten), BizTalk Server EDI artefacten migreren naar BizTalk-Services.

Dit document worden enkele van de verschillen die betrokken zijn bij migratie BizTalk Server EDI artefacten BizTalk-Services beschreven. Dit document veronderstelt praktische ervaring met de verwerking van BizTalk Server EDI en Trading Partner overeenkomsten. Zie voor meer informatie op de BizTalk Server EDI [Trading Partner beheer BizTalk-Server gebruiken](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Welke versie van BizTalk Server EDI artefacten kan worden gemigreerd naar BizTalk-Services?

De BizTalk Server EDI module is aanzienlijk verbeterd voor BizTalk Server 2010, wanneer het opnieuw opnemen van partners, profielen en overeenkomsten is gemodelleerd. Hetzelfde model BizTalk-Services gebruikt voor het ordenen van de handelspartners en de business afdelingen binnen de handelspartners. Als gevolg hiervan is migratie EDI artefacten van BizTalk Server 2010 en hoger naar BizTalk-Services, een veel meer eenvoudig proces. Migratie van EDI-artefacten die met versies vóór 2010 van BizTalk Server, moet u eerst een upgrade naar BizTalk Server 2010 en vervolgens uw EDI-artefacten migreren naar BizTalk-Services.

## <a name="scenariosmessage-flow"></a>Scenario's / stroom

Als met BizTalk Server is EDI verwerking van BizTalk-Services gebaseerd op een oplossing voor Trading Partner (TPM). De TPM-oplossing heeft de volgende kerncomponenten:

- Handelspartners, die de organisatie in een B2B-transacties vertegenwoordigen.
- Profielen die de afdelingen binnen een handelspartner.
- Trading partner overeenkomsten (of overeenkomsten), die de zakelijke overeenkomst tussen de twee partners/profielen vertegenwoordigen.

In de volgende afbeelding ziet u de overeenkomsten, alsmede de verschillen tussen een BizTalk-Services EDI oplossing en BizTalk Server EDI-oplossing:

![][EDImessageflow]

De belangrijkste verschillen en overeenkomsten tussen een stroom EDI oplossing op de BizTalk Server en BizTalk-Services zijn:

- Net zoals een pijpleiding EDIReceive BizTalk Server gebruikt voor het ontvangen van een EDI-bericht en een pijpleiding EDISend EDI-berichten wilt verzenden, gebruikt de BizTalk-Services een brug EDI wordt ontvangen en verzenden van EDI bridge EDI-berichten verzenden. Op de BizTalk Server, de pijpleidingen zijn gekoppeld aan een overeenkomst met behulp van verzenden of ontvangen van poorten. De overeenkomst zelf geeft het verzenden of ontvangen bridge in BizTalk-Services.
- Op de BizTalk Server, nadat de pijpleiding EDIReceive het EDI-bericht verwerkt, het bericht gedumpt met een SQL Server-database. De pijpleiding EdiSend opvangt vervolgens het bericht van de SQL Server-database verwerkt het en verstuurt het vervolgens naar de handelspartner.

    In de BizTalk-Services, nadat de EDI ontvangen bridge verwerkt het EDI-bericht, het routeert het bericht naar een extern proces. Extern proces kan worden uitgevoerd op Microsoft Azure of op locatie. Extern proces moet het bericht doorsturen aan de brug van EDI verzenden; de brug verzenden het bericht niet per definitie worden getrokken. Nadat het bericht verwerkt, stuurt de brug EDI verzenden het bericht naar de handelspartner.

BizTalk-Services biedt een eenvoudig te gebruiken configuratie ervaring om snel te maken en implementeren van een B2B overeenkomst tussen handelspartners zonder het configureren van een Microsoft Azure berekenen-exemplaren (Web of werknemer rollen), geen Microsoft Azure SQL-Databases of Microsoft Azure opslag rekeningen. Meer complexe scenario's moet u koppelverkoop in workflows of van andere service "rond de randen' van een Trading Partner-overeenkomst, dat wil zeggen, vóór of na verwerking voor Trading Partner overeenkomst EDI-brug. In detail plaats de volgende reeks gebeurtenissen tijdens een EDI-berichten in BizTalk-Services.

1. Een EDI-bericht wordt ontvangen van een handelspartner, Fabrikam.  EDI-berichten ontvangen van handelspartners, ondersteunt BizTalk-Services overdrachtsprotocollen zoals FTP, SFTP, AS2 en HTTP/S.

2. De trading partner overeenkomst ontvangen verwerking worden ontleed het EDI-bericht naar XML-indeling.  De gedemonteerde EDI-bericht (in XML-indeling) kunt u doorsturen naar Service Bus eindpunten, zoals een Bus-Relay-Service-eindpunt, Service Bus onderwerp, Service Bus wachtrij of een brug BizTalk-Services.

3. Vanuit het eindpunt voor verdere verwerking van aangepaste kunnen vervolgens de gedemonteerde XML-berichten worden ontvangen.  Eindpunten kunnen bijvoorbeeld worden verwerkt door een onderdeel in de lokalen of verdere verwerking van het bericht in een service Windows Workflow (WF) of Windows Communication Foundation (WCF) een exemplaar van Microsoft Azure berekenen.

4. "Verwerking van verzenden-zijde" van de handelspartner overeenkomst vervolgens het XML-bericht naar EDI-indeling assembleert en stuurt dit naar handelspartner Contoso.  EDI-berichten verzonden naar handelspartners, ondersteunt BizTalk-Services dezelfde protocollen als die worden gebruikt voor het ontvangen van EDI-berichten.

Verder in dit document vindt u algemene richtlijnen op sommige andere artefacten BizTalk Server EDI naar BizTalk-Services migreren.

## <a name="sendreceive-ports-to-trading-partners"></a>Verzenden/ontvangen poorten voor handelspartners

BizTalk Server u locaties ontvangen en poorten ontvangen EDI/XML berichten ontvangen van handelspartners, en verzenden poorten instellen EDI/XML berichten verzenden naar handelspartner. U vervolgens bezet deze poorten een trading partner overeenkomst met de console Beheer van BizTalk Server. In de BizTalk-Services, de locaties waar u berichten ontvangt van handelspartners en waar u berichten met handelspartners zijn geconfigureerd als onderdeel van de trading partnerovereenkomst zelf (als onderdeel van de instellingen voor vervoer) in de BizTalk-Services Portal sturen.  Dus u echt geen het concept van "poorten verzenden" en "locaties ontvangen," per definitie in BizTalk-Services. Zie [Overeenkomsten maken](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx)voor meer informatie.

## <a name="pipelines-bridges"></a>Pijpleidingen (bruggen)

Pijpleidingen zijn in BizTalk Server EDI, bericht verwerking entiteiten die kunnen ook aangepaste logica voor de van specifieke verwerkingsmogelijkheden, zoals vereist door de toepassing opnemen. Het equivalent zou voor BizTalk-Services, een EDI-brug. Echter in de BizTalk-Services, nu de EDI-bruggen zijn 'gesloten'.  U kunt uw eigen aangepaste activiteiten, toevoegen aan een EDI-brug. Aangepaste verwerking moet gebeuren buiten de EDI-brug in uw toepassing, voordat of nadat het bericht de brug die is geconfigureerd als onderdeel van de overeenkomst Trading Partner wordt. EAI bruggen hebben de optie om aangepaste verwerking. Desgewenst kunt u aangepaste verwerking kunt EAI bruggen voordat of nadat het bericht wordt verwerkt door de EDI-brug. Zie voor meer informatie [hoe u aangepaste Code opnemen in bruggen](https://msdn.microsoft.com/library/azure/dn232389.aspx).

U kunt een stroom publiceren/abonneren met aangepaste code en/of Service Bus berichten wachtrijen en onderwerpen voordat de trading partnerovereenkomst ontvangt het bericht, of nadat de overeenkomst het bericht verwerkt en doorgestuurd naar een Bus-Service-eindpunt gebruikt.

Zie **Scenario's / berichtenstroom** in dit onderwerp voor het bericht stroom-patroon.

## <a name="agreements"></a>Overeenkomsten

Als u bekend met de BizTalk Server 2010 handel partnerovereenkomst gebruikt voor het verwerken van EDI bent, zoekt BizTalk-Services trading partner overeenkomsten bekend. De meeste van de instellingen van de overeenkomst zijn hetzelfde en de dezelfde terminologie te gebruiken. In sommige gevallen, de instellingen van de overeenkomst zijn veel eenvoudiger in vergelijking met de instellingen op de BizTalk Server. Microsoft Azure BizTalk-Services ondersteunt X12, EDIFACT en AS2 transport.

Microsoft Azure BizTalk-Services biedt ook een **TPM Data Migration** tool trading partners en overeenkomsten van module BizTalk Server Trading Partner Portal van BizTalk-Services migreren. De TPM Data Migration tool is beschikbaar als onderdeel van een pakket hulpprogramma's die kunnen worden gedownload van de [MAB-SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). Het pakket bevat ook een Leesmij-bestand dat vindt u instructies voor het gebruik van het hulpprogramma en informatie over probleemoplossing voor het hulpprogramma.

## <a name="schemas"></a>Schema 's

BizTalk-Services biedt EDI-schema's die kunnen worden gebruikt in BizTalk-Services-oplossingen.  BizTalk Server EDI-schema's kunnen daarnaast ook worden gebruikt met BizTalk-Services omdat het hoofdknooppunt van de EDI-schema is hetzelfde voor alle BizTalk Server als BizTalk-Services. Zo kun je direct uw BizTalk Server EDI-schema's en deze gebruiken in de EDI-oplossingen die u ontwikkelt met behulp van BizTalk-Services. U kunt ook de schema's uit de [MAB-SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057)downloaden.

## <a name="maps-transforms"></a>Kaarten (transformaties)

Kaarten op de BizTalk Server, transformaties worden genoemd in BizTalk-Services. Toewijzingen van BizTalk Server migreren naar BizTalk-Services is een van de meer complexe taken te bereiken (afhankelijk van de complexiteit van de kaart). De toewijzingsfunctie gebruikt om BizTalk-Services verschilt van BizTalk mapper. Hoewel de mapper meestal ziet er hetzelfde uit, is de onderliggende map-indeling anders. Functoids (genaamd **Kaart bewerkingen** in BizTalk-Services) beschikbaar voor de gebruikers zijn ook verschillend.  U kunt een BizTalk-kaart van kracht rechtstreeks in BizTalk-Services gebruiken. Niet alle beschikbare BizTalk Server functoids zijn ook beschikbaar als kaart bewerkingen in BizTalk-Services.

### <a name="new-transform-operations"></a>Nieuwe transformatiebewerkingen

Terwijl de lijst met hyperlinks transformatiebewerkingen beschikbaar heel anders dan de mapper van BizTalk Server lijkt, BizTalk-Services transformeert beschikken over nieuwe manieren van dezelfde taken uitvoeren. BizTalk-Services zet bijvoorbeeld **Lijst bewerkingen** beschikbaar. Dit was niet beschikbaar in BizTalk mapper.  De **Lijst met bewerkingen** kunt u maken en werken met een "List", waarbij een lijst is een verzameling items (ook wel bekend als ' rijen') en waarbij elk item kan meerdere leden (ook bekend als "kolommen") hebben.  U kunt de lijst sorteren, selecteert u items op basis van een voorwaarde, enz.

Een ander voorbeeld van de nieuwe functionaliteit in BizTalk-Services transformaties zijn de **Bewerkingen van de lus**.  Het is moeilijk te maken van geneste lussen in de mapper van BizTalk Server.  Zo worden de lus kaart bewerkingen toegevoegd voor de BizTalk-Services worden omgezet.

Nog is een ander voorbeeld de **If-Then-Else** expressie kaart bewerking.  Doet een if-then-else bewerking mogelijk was in BizTalk mapper, maar het vereist meerdere functoids voor een schijnbaar eenvoudige taak.

### <a name="migrating-biztalk-server-maps"></a>Toewijzingen migreren BizTalk Server

Microsoft Azure BizTalk-Services bevat een hulpprogramma voor het migreren van BizTalk Server wordt toegewezen aan de BizTalk-Services transformaties. De **BTMMigrationTool** is beschikbaar als onderdeel van het pakket's **** voorzien zijn van de [BizTalk-Services SDK downloaden](http://go.microsoft.com/fwlink/p/?LinkId=235057). Zie [converteren van een BizTalk-kaart voor de transformatie van een BizTalk-Services](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx)voor meer informatie over het hulpprogramma.

U kunt ook zoeken op een steekproef door Sandro Pereira, BizTalk-MVP, voor het [migreren van BizTalk Server wordt toegewezen aan de BizTalk-Services transformaties](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Integraties

Als u migreren, BizTalk Server orchestration verwerken van Microsoft Azure wilt, moet de integraties worden herschreven met BizTalk Server integraties niet wordt ondersteund door Microsoft Azure.  U kunt de functionaliteit van orchestration in een service van Windows Workflow Foundation 4.0 (WF4) kan herschrijven.  Dit zou volledig herschreven worden omdat er momenteel geen migratie van BizTalk Server integraties naar WF4. Er zijn bronnen voor Windows Workflow:

- [*Het integreren van een Service WCF Workflow met Bus servicewachtrijen en onderwerpen*](https://msdn.microsoft.com/library/azure/hh709041.aspx) door Paolo Salvatori. 

- [ *Apps bouwen met Windows Workflow Foundation en Azure* -sessie](http://go.microsoft.com/fwlink/p/?LinkId=237314) van de Build-2011-conferentie.

- [*Windows Workflow Foundation Developer Center*](http://go.microsoft.com/fwlink/p/?LinkId=237315) op MSDN.

- [*Windows Workflow Foundation 4 (WF4) documentatie*](https://msdn.microsoft.com/library/dd489441.aspx) op MSDN.

## <a name="other-considerations"></a>Andere overwegingen

Hier volgen enkele overwegingen die u moet tijdens het gebruik van BizTalk-Services.

### <a name="fallback-agreements"></a>Fallback overeenkomsten

Verwerking van BizTalk Server EDI heeft het begrip "Overeenkomsten terugval".  BizTalk-Services is **niet** een concept overeenkomst terugval tot nu toe hebben.  Zie BizTalk-documentatie onderwerpen [De rol van de overeenkomsten bij het verwerken van EDI](http://go.microsoft.com/fwlink/p/?LinkId=237317) en [terugval overeenkomst eigenschappen of globale configureren](https://msdn.microsoft.com/library/bb245981.aspx) voor informatie over hoe terugval overeenkomsten worden gebruikt in BizTalk Server.

### <a name="routing-to-multiple-destinations"></a>Routering naar meerdere bestemmingen

BizTalk-Services bruggen, in zijn huidige staat biedt geen ondersteuning voor routeren van berichten naar meerdere bestemmingen met behulp van een publicatie-model te abonneren. U kunt berichten van een brug BizTalk-Services naar een onderwerp Bus Service, die meerdere abonnementen op het bericht op meer dan één eindpunt hebben in plaats daarvan kan routeren.

## <a name="conclusion"></a>Conclusie

Microsoft Azure BizTalk-Services wordt bijgewerkt in de reguliere mijlpalen toevoegen meer functies en mogelijkheden. Bij elke update hopen we te ondersteunen, verhoogde functionaliteit voor het maken van end-to-end oplossingen met BizTalk-Services en andere Azure technologieën vergemakkelijken.

## <a name="see-also"></a>Zie ook

[Ontwikkeling van bedrijfstoepassingen met Azure](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
