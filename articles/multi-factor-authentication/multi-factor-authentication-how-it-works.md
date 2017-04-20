<properties 
    pageTitle="Azure meerledige verificatie - hoe het werkt"
    description="Azure meerledige verificatie helpt beschermen toegang tot gegevens en toepassingen voldoen aan eisen van de gebruiker voor een eenvoudig aanmelden proces. Het biedt extra beveiliging doordat een tweede vorm van verificatie en sterke verificatie via een reeks opties voor eenvoudige verificatie biedt."
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
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

#<a name="how-azure-multi-factor-authentication-works"></a>De werking van Azure meerledige verificatie

De beveiliging van een meerledige verificatie ligt in de aanpak van de gelaagde. Een belangrijke uitdaging in gevaar brengt meerdere verificatie factoren aanvallers worden weergegeven. Zelfs als een aanvaller erin slaagt voor meer informatie over het wachtwoord van de gebruiker, is het zinloos zonder dat ook het bezit van een vertrouwd apparaat. Moet de gebruiker het apparaat verliest, meer de persoon die u het zoekt niet kunt gebruiken, tenzij hij of zij weet ook het wachtwoord van de gebruiker.

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)



Azure meerledige verificatie helpt beschermen toegang tot gegevens en toepassingen voldoen aan eisen van de gebruiker voor een eenvoudig aanmelden proces.  Het biedt extra beveiliging doordat een tweede vorm van verificatie en sterke verificatie via een reeks opties voor eenvoudige verificatie biedt:

- telefoongesprek
- SMS-bericht
- mobiele app-meldingen, zodat gebruikers kunnen kiezen hoe ze de voorkeur geven
- mobiele app verificatiecode in
- 3e partij EDE tokens

Zie voor meer informatie oh hoe het werkt in de volgende video.

>[AZURE.VIDEO multi-factor-authentication-deep-dive-securing-access-on-premises]

##<a name="methods-available-for-multi-factor-authentication"></a>Beschikbare methoden voor meerledige verificatie
Wanneer een gebruiker zich aanmeldt, wordt een aanvullende verificatie naar de gebruiker verzonden.  Hieronder vindt u een lijst met methoden die kunnen worden gebruikt voor deze tweede verificatie.

Verificatiemethode  | Beschrijving
------------- | ------------- |
Telefoongesprek | Een aanroep van wordt de Smartphone van de gebruiker te vragen of ze zich wilt aanmelden door te drukken op het teken # geplaatst.  Dit zal het verificatieproces voltooien.  Deze optie kan worden geconfigureerd en kan worden gewijzigd in een code die u opgeeft.
SMS-bericht | Een SMS-bericht ontvangt van een gebruiker Smartphone met een 6-cijferige code.  Voer deze code in voor het voltooien van de verificatieprocedure.
Mobiele App-meldingen | Een verzoek om controle wordt op het verzoek om volledige controle door verifiëren te selecteren in de mobiele app voor Smartphone van de gebruiker verzonden. Dit gebeurt als u app melding hebt geselecteerd als uw primaire verificatiemethode.  Als ze dit wanneer ze niet aanmelden, kunnen ze kiezen om dit te melden als fraude.
Verificatiecode met mobiele App | Een verificatiecode wordt mobiele App die wordt uitgevoerd op de Smartphone van de gebruiker verzonden.  Dit gebeurt als u een verificatiecode geselecteerd als uw primaire verificatiemethode.


##<a name="available-versions-of-azure-multi-factor-authentication"></a>Beschikbare versies van Azure meerledige verificatie
Azure meerledige verificatie is beschikbaar in drie verschillende versies.  In de volgende tabel wordt elk van deze meer gedetailleerd beschreven.

Versie  | Beschrijving
------------- | ------------- |
Meerledige verificatie voor Office 365 | Deze versie werkt uitsluitend in combinatie met Office 365-toepassingen en vanuit de Office 365 portal wordt beheerd. Zodat beheerders kunnen nu helpen beveiligen hun bronnen voor Office 365 door meerledige verificatie.  Deze versie wordt geleverd met een Office 365-abonnement.
Meerledige verificatie voor Azure beheerders | De dezelfde subset van mogelijkheden voor Office 365 meerledige verificatie kan worden zonder bijkomende kosten voor alle Azure beheerders. Elke Administrator-account van een Azure abonnement krijgt nu extra beveiliging doordat deze kernfunctionaliteit meerledige verificatie. Zodat een beheerder die Azure portal maken een VM, een website toegang wil tot opslag beheren, mobiele services of andere Azure-Service kan toevoegen meerledige verificatie aan de administrator-account.
Azure meerledige verificatie | Azure meerledige verificatie biedt de rijkste set van mogelijkheden. <br><br>Extra configuratie-opties via de portal Management Azure, geavanceerde rapportage en ondersteuning biedt voor een reeks van ruimten en cloud toepassingen. Azure meerledige verificatie kan worden aangeschaft als een zelfstandige licentie en wordt meegeleverd in Azure Active Directory Premium en Enterprise Mobility Suite. <br><br>Het kan ook worden gekocht op basis van het verbruik door het maken van een Provider Azure meerledige verificatie in een Azure-abonnement.
##<a name="feature-comparison-of-versions"></a>Functie voor vergelijken van versies
De volgende in de volgende tabel bevat een lijst van de functies die beschikbaar in de verschillende versies van Azure meerledige verificatie zijn.


Functie  | Meerledige verificatie voor Office 365 (opgenomen in Office 365 SKU's)|Meerledige verificatie voor Azure beheerders (meegeleverd met Azure-abonnement) | Azure meerledige verificatie (opgenomen in Azure AD Premium en Enterprise Mobility Suite)
------------- | :-------------: |:-------------: |:-------------: |
Beheerders kunnen accounts met de MVR gesloten beschermen| * | * (Alleen beschikbaar voor Azure Administrator-accounts)|*
Mobiele app als tweede factor|* | * | *
Telefoongesprek als tweede factor|* | * | *
SMS als tweede factor|* | * | *
Wachtwoorden voor clients die geen ondersteuning voor MVR gesloten bieden App|* | * | *
Admin controle over verificatiemethoden| *| *| *
PIN-modus| | | *
Fraudewaarschuwing| | | *
Rapporten van de MVR gesloten| | | *
Eenmalige overslaan| | | *
Aangepaste begroetingen voor telefoongesprekken| | | *
Aanpassing van de beller-ID voor telefoongesprekken| | | *
Bevestiging van de gebeurtenis| | | *
Vertrouwde IP-adressen| | | *
Stand-by MVR gesloten voor onthouden apparaten (Public Preview)| | | *
MVR GESLOTEN SDK| | | *
MVR gesloten voor MVR gesloten server toepassingen voor gebouwen| | | *


##<a name="how-to-get-azure-multi-factor-authentication"></a>Het verkrijgen van Azure meerledige verificatie

Als u de volledige functionaliteit van Azure meerledige verificatie in plaats van alleen bedoeld voor gebruikers van Office 365 en Azure beheerders dat wilt, zijn er verschillende manieren te werk:

1.  Azure meerledige verificatie licenties aan te schaffen en deze toewijzen aan de gebruikers.
2.  Azure meerledige verificatie gebundeld in deze zoals Azure Active Directory Premium of Enterprise Mobility Suite-licenties aanschaffen en toewijzen aan gebruikers.
3.  Een Provider Azure meerledige verificatie binnen een Azure-abonnement maken. Als u niet al een Azure-abonnement hebt, kunt u zich aanmelden voor een proefabonnement op Azure. Evaluatieversie abonnementen moeten worden geconverteerd naar normale abonnementen vóór afloop van de evaluatieperiode.

Bij gebruik van een Azure meerledige verificatieprovider zijn er twee gebruikersmodellen beschikbaar via Azure abonnementskosten worden gefactureerd:


- **Per gebruiker**. Voor ondernemingen die willen doorgaans meerledige verificatie inschakelen voor een vast aantal werknemers die regelmatig dienen te worden geverifieerd.
- **Per verificatie**. Voor ondernemingen die willen doorgaans meerledige verificatie inschakelen voor een grote groep externe gebruikers die minder vaak verificatie nodig.

Zie voor details prijzen [prijzen van Azure MVR gesloten.](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)

Kies de per seat- of consumptie-model die geschikt is voor uw organisatie.   Klik om begonnen Zie [Aan de slag](multi-factor-authentication-get-started.md)
