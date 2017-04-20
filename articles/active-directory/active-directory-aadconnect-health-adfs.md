
<properties
    pageTitle="Met behulp van Azure AD gezondheid verbinding met AD FS | Microsoft Azure"
    description="Dit is de pagina Azure AD verbinding maken met de gezondheid van het controleren van uw locatie in AD FS-infrastructuur."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-with-ad-fs"></a>Met behulp van Azure AD gezondheid verbinding met AD FS
De volgende documentatie is specifiek voor het controleren van uw AD FS-infrastructuur met Azure AD verbinding maken met de gezondheid. Zie voor meer informatie over het controleren van Azure AD verbinding maken (synchroniseren) met Azure AD verbinding maken met de gezondheid [Met behulp van Azure AD gezondheid verbinden voor synchronisatie](active-directory-aadconnect-health-sync.md). Zie daarnaast voor informatie over het controleren van Active Directory Domain Services met Azure AD verbinding maken met de gezondheid van [Azure met behulp van AD gezondheid verbinding met AD DS](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Waarschuwingen voor AD FS
De sectie Azure AD verbinding maken met de gezondheid van waarschuwingen kunt u de lijst met actieve waarschuwingen. Elke melding bevat relevante informatie, stappen voor het oplossen en koppelingen naar verwante documentatie.

U kunt dubbelklikken op een waarschuwing actief of afgesloten openen van een nieuwe blade met extra informatie kunt doen voor het herleiden van de waarschuwing en koppelingen naar relevante documentatie. U kunt ook historische gegevens bekijken over de waarschuwingen die zijn opgelost in het verleden.

![Azure AD verbinding gezondheid Portal](./media/active-directory-aadconnect-health/alert2.png)



## <a name="usage-analytics-for-ad-fs"></a>Gebruiksanalyse voor AD FS
Azure AD verbinding maken met de gezondheid van gebruiksanalyse analyseert het verificatieverkeer van de federatieservers. U kunt dubbelklikken op het vak gebruik analytics om het gebruik van analytics blade, waarin de verschillende maatstaven en groepen te openen.

>[AZURE.NOTE] Analytics voor gebruik met AD FS gebruikt, moet u ervoor zorgen dat controle voor AD FS is ingeschakeld. Zie [Controle inschakelen voor AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs)voor meer informatie.

![Azure AD verbinding gezondheid Portal](./media/active-directory-aadconnect-health/report1.png)

Selecteer extra statistieken, een tijdstip opgeven of wijzigen van de groepering, met de rechtermuisknop op het gebruik van analytics diagram en selecteer diagram bewerken. Vervolgens kunt u de periode opgeven, selecteert u een andere maateenheid en de groepering wijzigen. U kunt de verdeling van het verificatieverkeer dat op basis van verschillende "gegevens" en elke relevante "groeperen op" parameters die worden beschreven in de volgende tabel met metric groeperen:

| Metric | Groeperen op | Wat de groepering betekent en waarom is het nuttig? |
| ------ | -------- | -------------------------------------------- |
| Totaal aanvragen: Het totale aantal aanvragen dat is verwerkt door de federation-service | Alle | Toont de telling van het totale aantal aanvragen zonder te groeperen. |
|  | Toepassing | Het totaal aantal aanvragen op basis van de gerichte gebruikmakende partij worden gegroepeerd. Deze groepering is nuttig te weten welke toepassing welk percentage van het totale verkeer ontvangt. |
|  | Server | Het totaal aantal aanvragen dat is gebaseerd op de server die de aanvraag verwerkt worden. Deze groepering is nuttig voor het begrijpen van de verdeling van de belasting van het totale verkeer. |
|  | Werkplek Join | Het totaal aantal aanvragen op basis van of ze afkomstig zijn van de arbeidsplaats verbonden apparaten (bekende) gegroepeerd. Deze groepering is nuttig te weten als uw resources zijn toegankelijk via apparaten die niet bekend aan de infrastructuur van de identiteit. |
|  | Verificatiemethode | Het totaal aantal aanvragen op basis van de verificatiemethode die wordt gebruikt voor verificatie worden gegroepeerd. Deze groepering is nuttig voor het begrijpen van de gemeenschappelijke verificatiemethode die wordt gebruikt voor verificatie. Hieronder staan de mogelijke verificatiemethoden <ol> <li>Geïntegreerde Windows-verificatie (Windows)</li> <li>Op formulieren gebaseerde verificatie (formulieren)</li> <li>SSO (eenmalige aanmelding)</li> <li>X509 certificate Authentication (certificaat)</li> <br>Als de federation-servers met behulp van een Cookie SSO-verzoek ontvangt, wordt dit verzoek als SSO (Single Sign On) geteld. In dergelijke gevallen als de cookie geldig is, wordt de gebruiker niet wordt gevraagd referenties op te geven en naadloze toegang tot de toepassing als resultaat gegeven. Dit probleem komt voor als er meerdere gebruikmakende partijen beschermd door de federation-servers. |
|  | Netwerklocatie | Het totaal aantal aanvragen op basis van de locatie van de gebruiker worden gegroepeerd. Kan het zijn dat een intranet of extranet. Deze groepering is handig om te weten welk percentage van het verkeer dat afkomstig is van het intranet en extranet. |
| Totaal aantal mislukte aanvragen: Het totale aantal mislukte aanvragen die zijn verwerkt door de federation-service. <br> (Deze waarde is alleen beschikbaar voor AD FS voor Windows Server 2012 R2)| Fouttype | Geeft het aantal fouten dat is gebaseerd op de fouttypen vooraf gedefinieerde. Deze groepering is nuttig voor het begrijpen van de voorkomende fouten. <ul><li>Onjuiste gebruikersnaam of wachtwoord: fouten als gevolg van onjuiste gebruikersnaam of wachtwoord.</li> <li>'Extranet-vergrendeling': fouten als gevolg van de aanvragen ontvangen van een gebruiker die is uitgesloten van het extranet </li><li> 'Wachtwoord verlopen': fouten door gebruikers met een verlopen wachtwoord aanmelden.</li><li>'Account uitgeschakeld': fouten als gevolg van gebruikers die zich met een uitgeschakelde account.</li><li>"Verificatie apparaat": fouten door gebruikers niet worden geverifieerd met verificatie apparaat.</li><li>"Verificatie van gebruiker": fouten door gebruikers niet worden geverifieerd door een ongeldig certificaat.</li><li>"MVR gesloten": fouten als gevolg van de gebruiker te verifiëren met behulp van meerledige verificatie mislukt.</li><li>"Andere referentie": "Vergunning uitgifte": fouten als gevolg van storingen van de vergunning.</li><li>"Overdracht van uitgifte": fouten als gevolg van uitgifte overdracht fouten.</li><li>"Token aanvaarding": storingen door ADFS weigert het token van een gegevensprovider identiteit.</li><li>"Protocol": storingen als gevolg van protocolfouten.</li><li>"Onbekend": alle variabel. Eventuele andere fouten die niet in de gedefinieerde categorieën passen.</li> |
|  | Server | De fouten die zijn gebaseerd op de server-groepen. Deze groepering is nuttig te weten de fout verdeling tussen servers. Ongelijke verdeling kan een indicatie van een server in een gebrekkige toestand zijn. |
|  | Netwerklocatie | Groepen de fouten op basis van de netwerklocatie van de aanvragen (intranet en extranet). Deze groepering is nuttig te weten van het type van de aanvragen die zijn mislukt. |
|  | Toepassing | De storingen op basis van de beoogde toepassing (gebruikmakende partij) gegroepeerd. Deze groepering is nuttig te weten welke gerichte toepassing ziet de meeste aantal fouten. |
| Aantal gebruikers: gemiddeld aantal unieke gebruikers actief in het systeem | Alle | Deze metriek biedt een telling van het gemiddelde aantal gebruikers met behulp van de federation-service in het segment van de geselecteerde tijd. De gebruikers zijn niet gegroepeerd. <br>Het gemiddelde is afhankelijk van de tijdsegment geselecteerd. |
|  | Toepassing | Het gemiddelde aantal gebruikers op basis van de beoogde toepassing (gebruikmakende partij) gegroepeerd. Deze groepering is nuttig te weten hoeveel gebruikers welke toepassing gebruikt. |


## <a name="performance-monitoring-for-ad-fs"></a>Prestatiecontrole voor AD FS
Azure AD verbinding gezondheid prestatiecontrole hier controlegegevens metrics. Als u de controle inschakelt, wordt een nieuwe blade geopend met gedetailleerde informatie over de parameters.


![Azure AD verbinding gezondheid Portal](./media/active-directory-aadconnect-health/perf1.png)


Als u de optie Filter aan de bovenkant van het blad, kunt u filteren door de server een afzonderlijke Serverstatistieken bekijken. Metrics, met de rechtermuisknop op de grafiek controle onder de bladeserver controleren en bewerken van grafiek selecteren. Vervolgens, van het nieuwe blad dat wordt geopend, kunt u extra statistieken selecteren in de vervolgkeuzelijst en geef een tijdsbereik voor de weergave van de gegevens.

## <a name="reports-for-ad-fs"></a>Rapporten voor AD FS
Gezondheid verbinding Azure AD biedt rapporten over activiteiten en prestaties van AD FS. Deze rapporten helpen beheerders inzicht in de activiteiten op de AD FS-servers.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>Top 50 gebruikers met mislukte aanmeldingen van gebruikersnaam en wachtwoord

Een belangrijke reden voor de verificatieaanvraag van een mislukte op een AD FS-server is een verzoek met ongeldige referenties, dat wil zeggen, een onjuiste gebruikersnaam of wachtwoord. Meestal gebeurt aan gebruikers door complexe wachtwoorden, wachtwoorden vergeten of typefouten.

Maar er zijn andere redenen die kunnen leiden tot een onverwacht aantal aanvragen worden verwerkt door AD FS-servers, zoals: een toepassing die caches gebruikersreferenties en de referenties is verlopen of een kwaadwillende gebruiker die zich aanmeldt bij een account met een reeks bekende wachtwoorden. De volgende twee voorbeelden zijn geldige redenen die tot een toename in de aanvragen leiden kunnen.

Azure AD verbinding maken met de gezondheid voor AD FS biedt een rapport over top 50 gebruikers met mislukte inlogpogingen vanwege een ongeldige gebruikersnaam of wachtwoord. Dit rapport wordt bereikt door de controlegebeurtenissen gegenereerd door de AD FS-servers in de bedrijven verwerken

![Azure AD verbinding gezondheid Portal](./media/active-directory-aadconnect-health-adfs/report1a.png)

In dit rapport hebt u eenvoudig toegang tot de volgende gegevens:

- Totaal aantal mislukte aanvragen met de verkeerde gebruikersnaam en wachtwoord in de afgelopen 30 dagen
- Gemiddeld aantal gebruikers die niet met een ongeldige gebruikersnaam/wachtwoord-aanmelding per dag.


Dit onderdeel te klikken gaat u verder naar het hoofdrapport blade die aanvullende informatie biedt. Deze blade bevat een grafiek met gegevens volgen van trends bepalen een basislijn over aanvragen met de verkeerde gebruikersnaam of wachtwoord. Daarnaast biedt het de lijst van de top 50 gebruikers met het aantal mislukte pogingen.

De grafiek bevat de volgende informatie:

- Het totaal aantal mislukte aanmeldingen als gevolg van een ongeldige gebruikersnaam en wachtwoord op basis van per dag.
- Totaal aantal unieke gebruikers mislukte aanmeldingen op basis van per dag.
- Client IP-adres voor de laatste aanvraag

![Azure AD verbinding gezondheid Portal](./media/active-directory-aadconnect-health-adfs/report3a.png)

Het rapport bevat de volgende informatie:

| Report Item | Beschrijving
| ------ | -------- |
|Gebruikers-ID| Bevat de gebruikers-ID die is gebruikt. Deze waarde is wat de gebruiker heeft getypt, die in sommige gevallen is de verkeerde gebruiker-ID wordt gebruikt.|
|Mislukte pogingen| Geeft het totale aantal mislukte pogingen voor die specifieke gebruiker-ID. De tabel wordt met het meest aantal mislukte pogingen in aflopende volgorde gesorteerd.|
|Laatste mislukte poging| Het tijdstempel geeft de laatste fout is opgetreden.
|Fout bij het laatste IP | Het Client-IP-adres uit de meest recente ongeldig verzoek bevat.|



>[AZURE.NOTE] Dit rapport wordt automatisch bijgewerkt nadat elke twee uur met de nieuwe gegevens binnen die tijd verzameld. Login pogingen in de laatste twee uur kunnen daardoor niet in het rapport worden opgenomen.



## <a name="related-links"></a>Verwante koppelingen

* [Azure AD verbinding gezondheid](active-directory-aadconnect-health.md)
* [Azure AD Health Agent-installatie aansluiten](active-directory-aadconnect-health-agent-install.md)
* [Azure AD verbinding gezondheid bewerkingen](active-directory-aadconnect-health-operations.md)
* [Met behulp van gezondheid Azure AD verbinden voor synchronisatie](active-directory-aadconnect-health-sync.md)
* [Met behulp van Azure AD gezondheid verbinding met AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD gezondheid Veelgestelde vragen over verbinding](active-directory-aadconnect-health-faq.md)
* [Azure AD verbinding gezondheid versiegeschiedenis](active-directory-aadconnect-health-version-history.md)
