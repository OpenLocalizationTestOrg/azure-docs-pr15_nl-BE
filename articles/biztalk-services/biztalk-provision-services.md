<properties
    pageTitle="Azure BizTalk-Services maken in de portal Azure | Microsoft Azure"
    description="Informatie over het creëren of Azure BizTalk-Services maken in de portal Azure; MAB, WABS"
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
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>



# <a name="create-biztalk-services-using-the-azure-portal"></a>BizTalk-Services met behulp van de portal Azure maken

Azure BizTalk-Services maken in de portal Azure.

> [AZURE.TIP] Om aan te melden bij de Azure portal, moet u een Azure-account en een abonnement op Azure. Als u geen account hebt, kunt u een gratis proefperiode account binnen enkele minuten. Zie [Azure gratis proefperiode](http://go.microsoft.com/fwlink/p/?LinkID=239738).

## <a name="create-a-biztalk-service"></a>Een BizTalk-Service maken
Afhankelijk van de editie die u kiest, kunnen niet alle BizTalk-Service-instellingen beschikbaar zijn.

1. Log in om de [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Selecteer in het navigatiedeelvenster onder **Nieuw**:  
![Klik op de knop Nieuw][NEWButton]

3. Selecteer **SERVICES APP** > **SERVICE BizTalk-** > **aangepaste maken**:  
![BizTalk-Service te selecteren en selecteer aangepaste maken][NewBizTalkService]

4. Geef de instellingen van de BizTalk-Service:

    <table border="1">
    <tr>
    <td><strong>BizTalk-servicenaam</strong></td>
    <td>U kunt een willekeurige naam opgeven maar specifieke. Enkele voorbeelden:<br/><br/>
    <em>mijnbedrijf</em>. biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>. biztalk.windows.net<br/>
    <em>MyApplication staat</em>. biztalk.windows.net<br/><br/>". biztalk.windows.net" wordt automatisch toegevoegd aan de naam die u invoert. Hiermee maakt u een URL die wordt gebruikt om naar uw BizTalk-Service, zoals <strong>https://<em>MyApplication staat</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Edition</strong></td>
    <td>Als u de test/ontwikkelingsfase, kies <strong>ontwikkelaar</strong>. Als u zich in de productiefase, gebruikt u de <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk-Services: edities grafiek</a> om te bepalen of <strong>Premium</strong>, <strong>standaard</strong>of <strong>standaard</strong> de juiste keuze voor uw bedrijfsscenario.
    </td>
    </tr>
    <tr>
    <td><strong>Regio</strong></td>
    <td>Selecteer de regio als host voor uw BizTalk-Service.</td>
    </tr>
    <tr>
    <td><strong>Domein URL</strong></td>
    <td>Dit is <strong>optioneel</strong>. Standaard is de URL van het domein <em>YourBizTalkServiceName</em>. biztalk.windows.net. Een aangepaste domein kan ook worden ingevoerd. Bijvoorbeeld, als uw domein <em>contoso</em>is, kunt u invoeren: <br/><br/>
    <em>Mijnbedrijf</em>. contoso.com<br/>
    <em>MyCompanyMyApplication</em>. contoso.com<br/>
    <em>MyApplication staat</em>. contoso.com<br/>
    <em>YourBizTalkServiceName</em>. contoso.com<br/>
    </td>
    </tr>
    </table>
Selecteer de pijl volgende gemarkeerd.

5. Voer de opslag en Database-instellingen:

    <table border="1">
    <tr>
    <td><strong>Bewaking/archiveren opslag account</strong></td>
    <td>Selecteer een bestaande opslag account of maak een nieuwe account voor opslag. <br/><br/>Als u een nieuwe opslag-account maakt, voert u de <strong>Accountnaam van de opslag</strong>.</td>
    </tr>
    <tr>
    <td><strong>Tracking-database</strong></td>
    <td>Als u een bestaande Azure SQL-Database, kan deze kan niet worden gebruikt door een andere BizTalk-Service. U moet de aanmeldingsnaam en wachtwoord ingevoerd wanneer dat Azure SQL-databaseserver is gemaakt.<br/><br/><strong>TIP</strong> De Tracking-database en Monitoring/archiveren opslag in hetzelfde gebied, als de BizTalk-Service maken.</td>
    </tr>
    </table>
Selecteer de pijl volgende gemarkeerd.

6. De Database-instellingen opgeven:

    <table border="1">
    <tr>
    <td><strong>Naam</strong></td>
    <td>Beschikbaar wanneer u <strong>een nieuw exemplaar van de SQL-Database maken</strong> is geselecteerd in het vorige scherm.
    <br/><br/>
   Voer de naam van een SQL-Database moet worden gebruikt door de BizTalk-Service.</td>
    </tr>
    <tr>
    <td><strong>Server</strong></td>
    <td>Beschikbaar wanneer u <strong>een nieuw exemplaar van de SQL-Database maken</strong> is geselecteerd in het vorige scherm.
    <br/><br/>
   Selecteer een bestaande SQL-databaseserver of maak een nieuwe Database in SQL server.</td>
    </tr>
    <tr>
    <td><strong>Server-aanmeldingsnaam</strong></td>
    <td>Geef de aanmeldingsnaam van de gebruiker.</td>
    </tr>
    <tr>
    <td><strong>Het aanmeldingswachtwoord server</strong></td>
    <td>Voer het aanmeldingswachtwoord.</td>
    </tr>
    <tr>
    <td><strong>Regio</strong></td>
    <td>Beschikbaar wanneer u <strong>een nieuw exemplaar van de SQL-Database maken</strong> is geselecteerd. Selecteer de regio als host voor de SQL-Database.</td>
    </tr>
    </table>

Schakel het selectievakje in om de wizard te voltooien. De voortgang van het pictogram wordt weergegeven:  
![Voortgang van het pictogram wordt weergegeven wanneer voltooid][ProgressComplete]

Als alles klaar is, wordt de BizTalk-Azure Service gemaakt en is klaar voor uw toepassingen. De standaardinstellingen zijn geschikt. Als u wilt om de standaardinstellingen te wijzigen, selecteert u **BizTalk-SERVICES** in het linkernavigatievenster en selecteert u de BizTalk-Service. Extra instellingen worden weergegeven in het [Dashboard, Monitor, en schaal tabbladen](biztalk-dashboard-monitor-scale-tabs.md) boven.

Afhankelijk van de status van de BizTalk Service zijn er bepaalde bewerkingen die niet kunnen worden voltooid. Ga naar [BizTalk-Services staat grafiek](biztalk-service-state-chart.md)voor een lijst van deze bewerkingen.


## <a name="post-provisioning-steps"></a>Stappen na het inrichten

-  [Het certificaat installeren op een lokale computer](#InstallCert)
-  [Een productie-ready-certificaat toevoegen](#AddCert)
-  [De toegangsbeheer-naamruimte ophalen](#ACS)

#### <a name="InstallCert"></a>Het certificaat installeren op een lokale computer
Als onderdeel van BizTalk-Service wordt ingericht, een zelf-ondertekend certificaat gemaakt en die is gekoppeld aan uw abonnement BizTalk-Service. U moet dit certificaat downloaden en installeren op computers waar u BizTalk-Service toepassingen implementeren of berichten verzenden naar een BizTalk-Service-eindpunt.

1. Log in om de [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. **BizTalk-SERVICES** te selecteren in het linkernavigatievenster en selecteer uw abonnement BizTalk-Service.
3. Selecteer het tabblad **Dashboard** .
4. Selecteer **SSL-certificaat downloaden**:  
![SSL-certificaat wijzigen][QuickGlance]
5. Dubbelklik op het certificaat en het uitvoeren van de wizard om het certificaat te installeren. Zorg ervoor dat u installeert het certificaat in het archief met **Vertrouwde basiscertificeringsinstanties** .

#### <a name="AddCert"></a>Een productie-ready-certificaat toevoegen
Zelf-ondertekend certificaat dat automatisch wordt gemaakt wanneer een BizTalk-Services maken is bedoeld voor gebruik in omgevingen met alleen ontwikkeling. Voor productie-scenario's, met een productie-ready-certificaat vervangen.

1. Selecteer op het tabblad **Dashboard** **Update SSL-certificaat**.
2. Ga naar uw persoonlijke SSL-certificaat (*CertificateName*.pfx) met de naam van uw BizTalk-Service, voer het wachtwoord in en klik op het vinkje.

#### <a name="ACS"></a>De toegangsbeheer-naamruimte ophalen

1. Log in om de [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. **BizTalk-SERVICES** te selecteren in het linkernavigatievenster en vervolgens selecteert u de BizTalk-Service.
3. Selecteer **Verbindingsgegevens**in de taakbalk:  
![Selecteer verbindingsgegevens][ACSConnectInfo]

4. Kopieer de waarden voor toegangsbeheer.

Wanneer u een Visual Studio-project BizTalk-Service implementeert, moet u deze naamruimte toegangsbeheer invoeren. De toegangsbeheer-naamruimte wordt automatisch gemaakt voor uw BizTalk-Service.

De Access Control-waarden kunnen worden gebruikt met elke toepassing. Wanneer Azure BizTalk-Services is gemaakt, bepaalt deze naamruimte toegangsbeheer de verificatie met de implementatie van BizTalk-Service. Als u het abonnement te wijzigen of de naamruimte te beheren, **ACTIVE DIRECTORY** selecteert in het linkernavigatievenster en selecteert u de naamruimte wilt. De taakbalk geeft een overzicht van de opties.

Op **beheren** te klikken opent de Access Control Management Portal. De BizTalk-Service gebruikt in de Portal voor Access Control Management **Service-identiteiten**:  
![ACS-Service identiteiten in het Access beheren Management Portal][ACSServiceIdentities]

De toegangsbeheer-id is een set referenties waarmee toepassingen of clients verifiëren met toegangsbeheer en ontvangen een token.

> [AZURE.IMPORTANT]**De eigenaar van** de BizTalk-Service gebruikt voor de standaard service-identiteit en de waarde **wachtwoord** . Als u de waarde van de symmetrische sleutel in plaats van de waarde van het wachtwoord gebruikt, wordt de volgende fout kan optreden.<br/><br/>*Kan geen verbinding maken met de Access Control Management Service-account met de opgegeven referenties*

[Beheer uw ACS Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx) worden enkele richtlijnen en aanbevelingen.

## <a name="requirements-explained"></a>Vereisten uiteengezet

Deze voorschriften zijn niet van toepassing op de gratis editie.
<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Wat u nodig hebt</strong></td>
        <td><strong>Waarom u deze nodig hebt</strong></td>
</tr>
<tr>
<td>Azure-abonnement</td>
<td>Het abonnement wordt bepaald wie zich aanmelden bij de Azure portal. De accounthouder maakt het abonnement op <a HREF="https://account.windowsazure.com/Subscriptions">Azure abonnementen</a>.
<br/><br/>
De Azure rekening kan meerdere abonnementen en kan worden beheerd door iedereen die is toegestaan. Bijvoorbeeld uw Azure accounthouder kunt u een abonnement met de naam <em>BizTalkServiceSubscription</em> maken en de BizTalk-beheerders binnen uw bedrijf (bijvoorbeeld ContosoBTSAdmins@live.com) toegang tot dit abonnement. In dit scenario wordt de BizTalk-beheerders zich aanmelden bij de Azure portal en hebben volledige beheerdersrechten voor de gehoste services in het abonnement, met inbegrip van Azure BizTalk. De BizTalk-beheerders zijn niet de Azure rekeninghouders en daarom geen toegang hebben tot alle informatie over facturering.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Abonnementen beheren en opslag rekeningen in de Azure portal</a> vindt u meer informatie.
</td>
</tr>
<tr>
<td>Azure SQL-Database</td>
<td>De tabellen, weergaven en opgeslagen procedures die worden gebruikt door de BizTalk-Service, inclusief de trackingsgegevens worden opgeslagen.
<br/><br/>
Wanneer u een BizTalk-Service maakt, kan u automatisch een nieuwe Server of Database maken of een bestaande Azure SQL Server, Azure SQL-Database gebruikt.
<br/><br/>
De schaal van de SQL-Database wordt automatisch geconfigureerd. De standaardschaal is meestal voldoende voor een BizTalk-Service. De schaal wijzigen van invloed op prijzen. Zie <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">Accounts en facturering in Azure SQL-Database</a>
<br/><br/>
<strong>Notities</strong>
<br/>
<ul>
<li> Wanneer u een nieuwe Database en Azure SQL Server maakt, wordt automatisch de Azure Services ingeschakeld. De BizTalk-Service vereist Azure Services worden ingeschakeld.</li>
<li>Als u een nieuwe Azure SQL-Database op een bestaande Azure SQL Server maakt, wordt de firewall-regels van de Server worden niet gewijzigd. Hierdoor is het mogelijk dat andere Azure Services geen toegang hebben tot de databases van de Server.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure Access Control-naamruimte</td>
<td>Verifieert met Azure BizTalk-Services. Wanneer u een Visual Studio-project BizTalk-Service implementeert, moet u deze naamruimte toegangsbeheer invoeren. Wanneer u een BizTalk-Service maakt, wordt automatisch de toegangsbeheer-naamruimte gemaakt.</td>
</tr>

<tr>
<td>Azure opslag account</td>
<td>Toegang krijgen tot tabellen en blobs wachtrijen gebruikt uw BizTalk-Service voor het opslaan van de volgende:

<ul>
<li>Logboekbestanden van de BizTalk-Service controleren. De uitvoer van de monitoring wordt ook weergegeven op het tabblad **controleren** in de portal Azure.</li>
<li>Bij het maken van een X12 of een overeenkomst tussen partners AS2, kunt u de functie voor het opslaan van eigenschappen van berichten voor archiveren inschakelen. Deze gegevens worden opgeslagen in de opslag.</li>
</ul>
<br/>
Wanneer u een BizTalk-Service maakt, kunt u een bestaande account voor de opslag of automatisch een nieuwe opslag-account maken.
<br/><br/>
De standaardinstellingen voor opslag zijn voldoende voor een BizTalk-Service.
<br/><br/>
Wanneer u een opslag-account maakt, worden automatisch een primaire en secundaire sleutel gemaakt. Deze sleutels beheren toegang tot uw account opslag. De BizTalk-Service gebruikt automatisch de primaire sleutel.
<br/><br/>
Zie <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">opslag</a> voor meer informatie.
</td>
</tr>

<tr>
<td>Persoonlijke SSL-certificaat</td>
<td>
Wanneer een BizTalk-Azure Service is gemaakt, wordt ook een HTTPS-URL met de naam van uw BizTalk-Service gemaakt. Deze URL wordt automatisch geconfigureerd voor het gebruik van een niet-geverifieerd certificaat alleen-ontwikkeling. Voor de productie moet u een persoonlijke SSL-certificaat.
<br/><br/>
<strong>De gegevens van belangrijke SSL certificaat</strong>

<ul>
<li>De vervaldatum van het certificaat moet minder dan 5 jaar.</li>
<li>Alle persoonlijke certificaten nodig een wachtwoord. Dit wachtwoord weet en een aanbevolen om dit wachtwoord met uw beheerders delen.</li>
<li>Zelfondertekende certificaten worden gebruikt in een test/ontwikkelomgeving. Bij het gebruik van zelfondertekende certificaten, importeer het certificaat in het persoonlijke certificaatarchief en het certificaatarchief Vertrouwde basiscertificeringsinstanties.</li>
</ul>
<br/>Bij het verzenden van de productie-certificaataanvraag naar de certificeringsinstantie, geeft u de volgende eigenschappen van het certificaat:
<br/>

<ul>
<li><strong>Uitgebreid sleutelgebruik</strong>: ten minste Azure BizTalk-Services-serververificatie vereist is.</li>
<li><strong>Algemene naam</strong>: Voer de FQDN-naam (Fully Qualified Domain Name) van de URL van BizTalk-Azure. Zie <a HREF="#BizTalk">een BizTalk-Service maken</a> in dit artikel.</li>
</ul>
<br/>
Een nieuwe of andere certificaten kan worden toegevoegd nadat de BizTalk-Service is gemaakt.
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>Hybride verbindingen

Wanneer u een BizTalk-Azure Service maakt, is het tabblad **Hybride verbindingen** beschikbaar:

![Tabblad hybride-verbindingen][HybridConnectionTab]

Hybride verbindingen worden gebruikt om een Azure website of Azure mobiele service alle bronnen op de ruimten die gebruikmaakt van een statisch TCP-poort, zoals SQL Server, MySQL, HTTP-Web-API's, mobiele diensten en de meeste aangepaste Web Services.  Hybride verbindingen en de Service van BizTalk-Adapter zijn verschillend. De Service van BizTalk-Adapter wordt verbinding Azure BizTalk-Services met een lijn van Business (LOB) op ruimten systeem gebruikt.

 Zie [Hybride verbindingen](integration-hybrid-connection-overview.md) voor meer informatie, inclusief het maken en beheren van hybride verbindingen.


## <a name="next-steps"></a>Volgende stappen

Nu dat een BizTalk-Service is gemaakt, dient u vertrouwd met de verschillende [BizTalk-Services: tabbladen Dashboard, Monitor en schaal](biztalk-dashboard-monitor-scale-tabs.md). De BizTalk-Service is klaar voor uw toepassingen. Als u wilt beginnen met het maken van toepassingen, gaat u naar [Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Zie ook
- [BizTalk-Services: Edities grafiek](biztalk-editions-feature-chart.md)<br/>
- [BizTalk-Services: Regionale grafiek](biztalk-service-state-chart.md)<br/>
- [BizTalk-Services: Back-up en terugzetten](biztalk-backup-restore.md)<br/>
- [BizTalk-Services: beperken](biztalk-throttling-thresholds.md)<br/>
- [BizTalk-Services: Naam van de uitgevende instelling en de sleutel van de uitgevende instelling](biztalk-issuer-name-issuer-key.md)<br/>
- [Hoe kan ik starten via de Azure BizTalk-Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
- [Hybride verbindingen](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
