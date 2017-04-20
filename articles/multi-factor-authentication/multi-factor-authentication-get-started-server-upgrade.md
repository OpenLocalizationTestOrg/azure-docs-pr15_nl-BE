<properties 
    pageTitle="De Agent voor PhoneFactor een upgrade uitvoeren naar Azure meerledige verificatieserver"
    description="Dit document wordt beschreven hoe aan de slag met Azure MVR gesloten Server en het bijwerken van de oudere phonefactor agent."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="upgrading-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>De Agent voor PhoneFactor een upgrade uitvoeren naar Azure meerledige verificatieserver

Een upgrade van de PhoneFactor Agent v5.x of ouder naar de Server Azure meerledige verificatie moet PhoneFactor Agent en de verbonden onderdelen worden verwijderd voordat de Server meerledige verificatie en haar verbonden onderdelen kunnen worden geïnstalleerd.

## <a name="to-upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>De PhoneFactor Agent upgraden naar Azure meerledige verificatieserver
<ol>
<li>Ten eerste back-up van het bestand PhoneFactor. De standaardlocatie is C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.


<li>Als de User Portal is geïnstalleerd:</li>
<ol>
<li>Ga naar de installatiemap en back-up van het bestand web.config. De standaardlocatie is C:\inetpub\wwwroot\PhoneFactor.</li>


<li>Als u aangepaste thema's hebt toegevoegd aan de portal, back-up van de aangepaste map onder de map C:\inetpub\wwwroot\PhoneFactor\App_Themes.</li>


<li>De User Portal door de PhoneFactor Agent (alleen beschikbaar als is geïnstalleerd op dezelfde server als de PhoneFactor-Agent) of Windows-programma's en onderdelen te verwijderen.</li></ol>




<li>Als de webservice Mobile App is geïnstalleerd:
<ol>
<li>Ga naar de installatiemap en back-up van het bestand web.config. De standaardlocatie is C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.</li>
<li>De mobiele App webservice via Windows verwijderen programma's en onderdelen.</li></ol>

<li>Als het Web Service SDK is geïnstalleerd, moet u het verwijderen door de PhoneFactor Agent of Windows-programma's en onderdelen.

<li>De Agent PhoneFactor via Windows verwijderen programma's en onderdelen.

<li>Installeer de Server meerledige verificatie. Houd er rekening mee dat het installatiepad wordt opgehaald uit het register van de vorige installatie van de Agent voor PhoneFactor zodat deze op dezelfde locatie (bijv. C:\Program Files\PhoneFactor) te installeren. Nieuwe installaties hebben een verschillende standaard pad (bijvoorbeeld C:\Program Files\Multi-Factor Authentication Server) installeren. Het gegevensbestand dat door de vorige PhoneFactor Agent links moet worden bijgewerkt tijdens de installatie, zodat uw gebruikers en instellingen nog steeds er moet na de installatie van de nieuwe Server voor meerledige verificatie.

<li>Desgevraagd de meerledige verificatieserver activeren en ervoor te zorgen dat is toegewezen aan de juiste replicatiegroep.

<li>Als de SDK Web Service al is geïnstalleerd, installeert u de nieuwe SDK Web Service via de gebruikersinterface meerledige verificatie Server. Opmerking: de standaardnaam voor de virtuele map is nu "MultiFactorAuthWebServiceSdk" in plaats van 'PhoneFactorWebServiceSdk'. Als u wilt dat de vorige naam te gebruiken, moet u de naam van de virtuele map tijdens de installatie. Anders, als u de installatie aan de nieuwe standaardnaam gebruiken, u moet de URL in alle toepassingen die verwijzen naar de Web Service SDK zoals de User Portal en de webservice voor mobiele App aan te wijzen op de juiste locatie wijzigen.

<li>Als de User Portal al op de Server PhoneFactor Agent is geïnstalleerd, installeert u nieuwe meerledige verificatie User Portal via de gebruikersinterface meerledige verificatie Server. Opmerking: de standaardnaam voor de virtuele map is nu "MultiFactorAuth" in plaats van 'PhoneFactor'. Als u wilt dat de vorige naam te gebruiken, moet u de naam van de virtuele map tijdens de installatie. Anders als u de installatie aan de nieuwe standaardnaam gebruiken, moet u klikt u op het pictogram User Portal in de Server meerledige verificatie en de URL van de Portal gebruiker op het tabblad instellingen.

<li>Als de User Portal en/of mobiele App Web Service al is geïnstalleerd op een andere server van de PhoneFactor Agent:
<ol>
<li>Ga naar de locatie voor de installatie (bijvoorbeeld C:\Program Files\PhoneFactor) en de juiste installer(s) te kopiëren naar de andere server. Er zijn 32-bits en 64-bits installatieprogramma's voor de User Portal en de webservice Mobile-App. Ze zijn MultiFactorAuthenticationUserPortalSetupXX.msi en MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi respectievelijk genoemd.</li>
<li>Open een opdrachtprompt als administrator om de User Portal op de webserver hebt geïnstalleerd, en de MultiFactorAuthenticationUserPortalSetupXX.msi worden uitgevoerd. Opmerking: de standaardnaam voor de virtuele map is nu "MultiFactorAuth" in plaats van 'PhoneFactor'. Als u wilt dat de vorige naam te gebruiken, moet u de naam van de virtuele map tijdens de installatie. Anders als u de installatie aan de nieuwe standaardnaam gebruiken, moet u klikt u op het pictogram User Portal in de Server meerledige verificatie en de URL van de Portal gebruiker op het tabblad instellingen. Bestaande gebruikers moeten worden geïnformeerd over de nieuwe URL.</li>
<li>Ga naar de User Portal installatielocatie (bv. C:\inetpub\wwwroot\MultiFactorAuth) en het web.config-bestand bewerken. Kopieer de waarden in de sectie appSettings en applicationSettings uit de oorspronkelijke versie van het bestand web.config dat back-up is vóór de upgrade naar het nieuwe bestand web.config. Als de nieuwe naam van de virtuele map standaard werd gehouden bij de installatie van de SDK Web Service, moet u de URL in de sectie applicationSettings om te verwijzen naar de juiste locatie wijzigen. Eventuele andere standaardinstellingen zijn gewijzigd in het vorige bestand web.config, die dezelfde wijzigingen toepassen op het nieuwe bestand web.config.</li>
<li>De webservice Mobile App installeren op de webserver, open een opdrachtprompt als administrator en de MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi worden uitgevoerd. Opmerking: de standaardnaam voor de virtuele map is nu "MultiFactorAuthMobileAppWebService" in plaats van 'PhoneFactorPhoneAppWebService'. Als u wilt dat de vorige naam te gebruiken, moet u de naam van de virtuele map tijdens de installatie. U wilt een kortere naam voor eindgebruikers in te typen op hun mobiele apparaten te vereenvoudigen. Anders als u de installatie aan de nieuwe standaardnaam gebruiken, moet u klikt u op het pictogram Mobile App in de Server meerledige verificatie en URL van de mobiele App webservice bijwerken.</li>
<li>Ga naar de webservice Mobile App installeren locatie (bv. C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) en bewerk het bestand web.config. Kopieer de waarden in de sectie appSettings en applicationSettings uit de oorspronkelijke versie van het bestand web.config dat back-up is vóór de upgrade naar het nieuwe bestand web.config. Als de nieuwe naam van de virtuele map standaard werd gehouden bij de installatie van de SDK Web Service, moet u de URL in de sectie applicationSettings om te verwijzen naar de juiste locatie wijzigen. Eventuele andere standaardinstellingen zijn gewijzigd in het vorige bestand web.config, die dezelfde wijzigingen toepassen op het nieuwe bestand web.config.</li></ol>
