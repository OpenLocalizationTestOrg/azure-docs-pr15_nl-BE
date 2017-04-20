<properties
    pageTitle="Patroon van meerdere huurder Web Application | Microsoft Azure"
    description="Overzichten van architectuur en ontwerppatronen waarin wordt beschreven hoe u een webtoepassing met meerdere huurder implementeren op Azure vinden."
    services=""
    documentationCenter=".net"
    authors="wadepickett" 
    manager="wpickett"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/05/2015"
    ms.author="wpickett"/>

# <a name="multitenant-applications-in-azure"></a>Multitenant toepassingen in Azure

Een multitenant-toepassing is een gedeelde bron waarmee afzonderlijke gebruikers of "huurders", de toepassing weergeven alsof het is hun eigen. Een typisch scenario die gepaard met een multitenant-toepassing is een waarin alle gebruikers van de toepassing willen misschien anders hebben dezelfde fundamentele eisen, maar de gebruikerservaring aanpassen. Voorbeelden van grote multitenant toepassingen zijn Office 365, Outlook.com en visualstudio.com.

Vanuit het oogpunt van de toepassingsprovider van een betrekking de voordelen van multitenancy voornamelijk op efficiencyverbeteringen werkwijzen en kosten. Een versie van uw toepassing kan de behoeften van veel huurders/klanten, consolidatie van een systeem waarin beheertaken uitvoeren, zoals controle, afstelling van prestaties, software onderhoud en back-ups van gegevens.

Hieronder vindt u een overzicht van de belangrijkste doelstellingen en behoeften vanuit het oogpunt van de provider.

- **Provisioning**: U moet zich kunnen richten nieuwe huurders van de toepassing.  Voor multitenant toepassingen met een groot aantal huurders is het meestal noodzakelijk dit proces te automatiseren door zelf inrichten.
- **Onderhoud**: U moet het mogelijk om een upgrade van de toepassing en andere onderhoudstaken uitvoeren terwijl meerdere huurders met deze.
- **Monitoring**: U moet zich kunnen houden op de toepassing te allen tijde om eventuele problemen te identificeren en oplossen van problemen met deze. Dit omvat het controleren hoe elke huurder de toepassing wordt gebruikt.

Een goed geïmplementeerde multitenant-toepassing biedt de volgende voordelen voor gebruikers.

- **Isolatie**: het gebruik van de toepassing door de andere huurders niet van invloed op de activiteiten van de individuele huurder. Huurder heeft geen toegang tot elkaars gegevens. Het lijkt alsof de huurder alsof ze exclusief gebruik van de toepassing zijn.
- **Beschikbaarheid**: individuele huurders wilt dat de toepassing bijvoorbeeld voortdurend beschikbaar zijn gedefinieerd in een SLA garanties bieden. Nogmaals, de activiteiten van andere huurders moeten niet van invloed op de beschikbaarheid van de toepassing.
- **Schaalbaarheid**: de toepassing wordt aangepast om te voldoen aan de vraag van individuele huurders. De aanwezigheid en de acties van andere huurders moeten geen invloed op de prestaties van de toepassing.
- **Kosten**: kosten lager zijn dan met een speciale toepassing van één huurder omdat meerdere pacht maakt het delen van bronnen.
- **Aanpassingsmogelijkheden**. De mogelijkheid voor het aanpassen van de aanvraag van een individuele huurder op verschillende manieren, zoals het toevoegen of verwijderen van onderdelen, wijzigen van kleuren en logo's of zelfs de toevoeging van hun eigen code of script.

Kortom, zijn er veel overwegingen waarmee u rekening houden moet een zeer schaalbare services te verlenen, maar er zijn ook een aantal van de doelstellingen en de vereisten die gemeenschappelijk voor veel multitenant-toepassingen zijn. Sommige mogelijk niet relevant voor specifieke scenario's en het belang van individuele doelstellingen en eisen in elk scenario verschillen. Als een provider van de multitenant toepassing ook hebt u doelstellingen en vereisten, zoals aan de huurder doelstellingen en eisen, winstgevendheid, facturering, meerdere serviceniveaus, inrichten, onderhoudbaarheid controle en automatisering.

Zie [een toepassing met meerdere huurder op Azure Hosting][]voor meer informatie over aanvullende overwegingen van een multitenant-toepassing. Zie voor informatie over algemene gegevens architectuur patronen van databasetoepassingen voor meerdere huurder software as a service (SaaS), [Ontwerppatronen voor SaaS-toepassingen met meerdere huurder met Azure SQL-Database](./sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure biedt tal van functies waarmee u kunt het adres van de belangrijkste problemen die zich bij het ontwerpen van een multitenant-systeem.

**Isolatie**

- Segment Website huurders van Host-Headers, met of zonder SSL-communicatie
- Segment Website huurders door Query-Parameters
- Webservices in rollen van de werknemer
    - Rollen van de werknemer. die normaal gesproken gegevens op de back-end van een toepassing worden verwerkt.
    - Web-rollen die meestal als de front-end toepassingen fungeren.

**Opslag**

Gegevensbeheer, zoals Azure SQL-Database of opslag Azure services, zoals de tabel service die services voor de opslag van grote hoeveelheden ongestructureerde gegevens biedt en de Blob-service services levert voor het opslaan van grote hoeveelheden ongestructureerde tekst of binaire gegevens, zoals video, audio en afbeeldingen.

- Het beveiligen van gegevens Multitenant in de juiste SQL-Database per-huurder SQL Server-aanmeldingen.
- Met Azure tabellen voor toepassing bronnen door te geven een container niveau-beleid kunt u de mogelijkheid om machtigingen aanpassen zonder de nieuwe URL's voor de bronnen die zijn beveiligd met handtekeningen voor gedeelde toegang te verlenen.
- Azure wachtrijen voor toepassing bronnen Azure wachtrijen worden vaak gebruikt voor de verwerking ten behoeve van huurders station, maar kunnen ook worden gebruikt voor het verdelen van werk die vereist is voor het inrichten of management.
- Servicewachtrijen Bus voor toepassingsbronnen waardoor werken aan een gedeelde service, kunt u één wachtrij waar elke afzender huurder heeft alleen machtigingen (die is afgeleid van de vorderingen die zijn uitgegeven door de ACS) om push naar die wachtrij, terwijl alleen de ontvangers van de service hebben tot gegevens die afkomstig zijn uit meerdere huurders worden getrokken uit de wachtrij.


**Verbindings- en beveiligingsservices**

- Azure Service Bus, een infrastructuur voor berichtenafhandeling die bevindt zich tussen toepassingen zodat ze voor het uitwisselen van berichten op een losse manier voor verbeterde schaal en tolerantie.

**Netwerkservices**

Azure biedt verschillende netwerkservices die verificatie ondersteunen en de beheerbaarheid van uw gehoste toepassingen. Deze services omvatten het volgende:

- Azure Virtual Network kunt u inrichten en beheren van virtuele particuliere netwerken (VPN's) in Azure, alsmede veilig koppelen aan op-premises IT-infrastructuur.
- Verkeer met Virtual Network Manager kunt u binnenkomend verkeer evenwicht tussen meerdere gehoste Azure services laden of ze in het datacenter van dezelfde of verschillende datacenters over de hele wereld werken.
- Azure Active Directory (AD Azure) is een moderne, REST gebaseerde service mogelijkheden voor beheer- en controle voor uw cloud-toepassingen biedt. Met behulp van Azure AD voor toepassingsbronnen Azure AD naar biedt een eenvoudige manier voor verificatie en machtiging van gebruikers om toegang te krijgen tot uw webtoepassingen en -services terwijl u de functies voor verificatie en machtiging om uit de code moet rekening worden gehouden.
- Azure Service Bus biedt een beveiligde berichten en gegevensstroom mogelijkheden voor gedistribueerde en hybride toepassingen, zoals communicatie tussen Azure gehoste toepassingen en toepassingen in gebouwen en diensten, zonder complexe firewall- en infrastructuren. Met behulp van Service Bus Relay tot de services die beschikbaar zijn als de eindpunten voor toepassingsbronnen kan behoren tot de huurder (bijvoorbeeld buiten het systeem, zoals de lokale host) of services ingericht speciaal voor de huurder (omdat de gevoelige, huurder-specifieke gegevens passeert ze) te zijn.



**Inrichten van Resources**

Azure biedt een aantal manieren nieuwe huurder dient voor de toepassing. Voor multitenant toepassingen met een groot aantal huurders is het meestal noodzakelijk dit proces te automatiseren door zelf inrichten.

- Werknemer rollen kunt u leveren en intrekken per huurder bronnen (bijvoorbeeld wanneer een nieuwe huurder borden omhoog of wordt geannuleerd), verzamelen metrics voor meting gebruikt en beheerd volgens een bepaalde planning schaal of in reactie op het overschrijden van de drempelwaarden van prestatie-indicatoren. Deze dezelfde rol kan ook worden gebruikt voor push-updates en upgrades van de oplossing.
- Azure BLOB's worden gebruikt voor het inrichten van de compute of vooraf geïnitialiseerd opslagbronnen voor nieuwe huurders terwijl de container niveau-beleid ter bescherming van de compute service pakketten, VHD afbeeldingen en andere bronnen.
- Opties voor het inrichten van bronnen voor SQL-Database voor een huurder omvatten:

    -   DDL in scripts of ingesloten als bronnen binnen de assembly's
    -   SQL Server 2008 R2 DAC-pakketten via een programma worden geïmplementeerd.
    -   Kopiëren van een database master verwijzing
    -   Inrichten van nieuwe databases uit een bestand met behulp van database importeren en exporteren.



<!--links-->

[Een toepassing met meerdere huurder op Azure hosting]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
