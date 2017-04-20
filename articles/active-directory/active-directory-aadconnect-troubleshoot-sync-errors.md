<properties
    pageTitle="Azure AD verbinden: Het oplossen van fouten tijdens synchronisatie | Microsoft Azure"
    description="Hoe oplossen van fouten die zijn opgetreden tijdens de synchronisatie met Azure AD verbinden."
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
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="troubleshooting-errors-during-synchronization"></a>Het oplossen van fouten tijdens synchronisatie
Fouten kunnen voorkomen wanneer de id-gegevens gesynchroniseerd naar Azure Active Directory (AD Azure) van Windows Server Active Directory (AD DS). Dit artikel biedt een overzicht van verschillende soorten synchronisatiefouten enkele van de mogelijke scenario's die ervoor zorgen dat deze fouten en mogelijke manieren om de fouten te corrigeren. Dit artikel bevat de voorkomende fout en kan niet betrekking hebben op alle mogelijke fouten.

 In dit artikel wordt ervan uitgegaan dat de lezer vertrouwd is met de onderliggende [begrippen Azure AD en Azure AD ontwerpen verbinden](active-directory-aadconnect-design-concepts.md).

Met de nieuwste versie van Azure AD verbinden \(augustus 2016 of hogere\), een rapport van synchronisatiefouten is beschikbaar in de [Portal Azure](https://aka.ms/aadconnecthealth) als onderdeel van Azure AD verbinding maken met de gezondheid voor synchronisatie.


Vanaf 1 September 2016 [Azure Active Directory dubbele kenmerk Resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) voorziening standaard voor alle de *nieuwe* huurders van Azure Active Directory. Deze functie wordt automatisch ingeschakeld voor bestaande huurders in de komende maanden.

Azure AD verbinden 3 soorten bewerkingen uitvoert vanuit de mappen synchroon blijven: importeren, synchronisatie en exporteren. Fouten kunnen in alle bewerkingen plaatsvinden. Dit artikel richt zich voornamelijk op fouten tijdens het exporteren naar Azure AD.

## <a name="errors-during-export-to-azure-ad"></a>Fouten tijdens het exporteren naar Azure AD
Volgende sectie beschrijft verschillende soorten synchronisatiefouten die kunnen optreden tijdens het exporteren naar Azure AD met de Azure AD-connector. Deze connector kan worden geïdentificeerd door de naamnotatie wordt 'contoso. *onmicrosoft.com*'.
Fouten tijdens het exporteren naar Azure AD geven aan dat de bewerking \(toevoegen, bijwerken en verwijderen enz.\) geprobeerd door te verbinden met Azure AD \(Sync Engine\) op Azure Active Directory is mislukt.

![Overzicht van fouten exporteren](.\media\active-directory-aadconnect-troubleshoot-sync-errors\Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Gegevens komen niet overeen fouten
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Beschrijving
- Als AD Azure verbinding \(sync engine\) geïnstrueerd Azure Active Directory toe te voegen of objecten bijwerken, Azure AD komt overeen met de inkomende object met het kenmerk **sourceAnchor** aan het kenmerk **immutableId** van objecten in AD Azure. Deze overeenkomst heet een **Harde overeenkomen**.
- Wanneer de Azure AD **vindt geen** elk object dat overeenkomt met de **immutableId** van het kenmerk met het kenmerk **sourceAnchor** van de inkomende object voor het inrichten van een nieuw object valt terug om de kenmerken ProxyAddresses en UserPrincipalName gebruiken om een overeenkomst te vinden. Deze overeenkomst heet een **Zachte overeenkomen**. Zachte overeenkomen is ontworpen om overeen met objecten die al in Azure AD (waarin zijn in Azure AD) met de nieuwe objecten worden toegevoegd/bijgewerkt tijdens de synchronisatie die dezelfde entiteit (gebruikers, groepen) in ruimten vertegenwoordigen.
- **InvalidSoftMatch** -fout treedt op wanneer de vaste overeenkomst vindt geen overeenkomende objecten **en** zachte overeenkomen met een overeenkomend object gevonden, maar dit object heeft een andere waarde van *immutableId* dan de inkomende object *SourceAnchor*, wat erop wees dat het overeenkomende object is gesynchroniseerd met een ander object uit Active Directory bij.

Met andere woorden, om het zachte overeenkomt met werken, mogen worden zacht vergeleken met het object geen waarde voor de *immutableId*. Als een object met *immutableId* wordt een waarde bij gebreke van de vaste overeenkomst maar die voldoet aan de criteria van de soft-match, dat de bewerking leidt tot een fout bij de gegevenssynchronisatie InvalidSoftMatch.

Azure Active Directory-schema is niet toegestaan voor twee of meer objecten met dezelfde waarde van de volgende kenmerken hebben. \(Dit is geen uitputtende lijst.\)

- ProxyAddresses
- UserPrincipalName
- onPremisesSecurityIdentifier
- Object-id

>[AZURE.NOTE] [Tolerantie voor Azure AD kenmerk dubbel kenmerk](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) functie is ook uitgerold als het standaardgedrag van Azure Active Directory.  Dit beperkt het aantal synchronisatiefouten gezien door Azure AD verbinden (evenals andere clients synchroniseren) door Azure AD veerkrachtiger maken in de manier waarop die wordt omgegaan met dubbele ProxyAddresses UserPrincipalName kenmerken en op omgevingen AD ruimten aanwezig in. Deze functie herstelt de fouten duplicatie niet. Zodat moet de gegevens nog worden vastgesteld. Maar kunt inrichten van nieuwe objecten die anders worden geblokkeerd als gevolg van dubbele waarden in Azure AD wordt ingericht. Dit wordt ook minder synchronisatiefouten aan de synchronisatie-client geretourneerd.
Als deze functie is ingeschakeld voor de huurder, worden er geen InvalidSoftMatch synchronisatiefouten weergegeven tijdens het inrichten van nieuwe objecten.


#### <a name="example-scenarios-for-invalidsoftmatch"></a>Voorbeeldscenario's voor InvalidSoftMatch
1. Er bestaat in twee of meer objecten met dezelfde waarde van het kenmerk ProxyAddresses bij Active Directory. Slechts één is in Azure AD ophalen ingericht.
2. Twee of meer objecten met dezelfde waarde van userPrincipalName bestaat in Active Directory bij. Slechts één is in Azure AD ophalen ingericht.
3. Een object is toegevoegd in de ruimten in Active Directory met dezelfde waarde van het kenmerk ProxyAddresses als die van een bestaand object in Azure Active Directory. Het object toegevoegd op gebouwen is niet ophalen ingericht in Azure Active Directory.
4. Een object is toegevoegd in in gebouwen van Active Directory met dezelfde waarde van kenmerk userPrincipalName als die van een account in Azure Active Directory. Het object is niet ophalen ingericht in Azure Active Directory.
5. Een gesynchroniseerde account is verplaatst van Forest A naar Forest b Azure AD verbinden (sync engine) ObjectGUID kenmerk werd gebruikt voor het berekenen van de SourceAnchor. Na de verplaatsing forest is de waarde van de SourceAnchor anders. Het nieuwe object (van Forest B) niet is gesynchroniseerd met het bestaande object in Azure AD.
6. Een gesynchroniseerde object hebt u per ongeluk verwijderd uit in Active Directory en een nieuw object is gemaakt in Active Directory voor dezelfde entiteit (bijvoorbeeld gebruiker) zonder dat u de account in Azure Active Directory verwijdert ruimten. De nieuwe account niet synchroniseren met de bestaande AD Azure-object.
7. Azure AD Connect is verwijderd en opnieuw geïnstalleerd. Een ander kenmerk is tijdens de installatie opnieuw gekozen als de SourceAnchor. Alle objecten die eerder waren gesynchroniseerd synchronisatie met de fout InvalidSoftMatch is gestopt.

#### <a name="example-case"></a>Voorbeeld van de aanvraag:
1. **Bob Smit** is een gebruiker gesynchroniseerd in Azure Active Directory uit in Active Directory van *contoso.com* gebouwen
2. Bob Smith **UserPrincipalName** is ingesteld als **bobs@contoso.com**.
3. **"abcdefghijklmnopqrstuv =="** is de **SourceAnchor** berekend door Azure AD verbinding maken met behulp van Bob Smith **objectGUID** uit op bedrijfsruimten Active Directory is de **immutableId** voor Bob Smith in Azure Active Directory.
4. Bob heeft ook de volgende waarden voor het kenmerk **proxyAddresses** :
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
5.  **Robert Taylor**, een nieuwe gebruiker wordt toegevoegd aan het op Active Directory.
6. Bob Taylor van **UserPrincipalName** is ingesteld als **bobt@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** is de **sourceAnchor** berekend door Azure AD verbinding maken met behulp van Bob Taylor **objectGUID** van op Active Directory bedrijfsruimten. Bob Taylor van object is niet gesynchroniseerd met de Azure Active Directory nog.
8. Bob Taylor heeft de volgende waarden voor het kenmerk proxyAddresses
    - smtp:bobt@contoso.com
    - smtp:bob.taylor@contoso.com
    - **smtp:bob@contoso.com**
9. Tijdens de synchronisatie wordt Azure AD verbinden de toevoeging van Bob Taylor in bij Active Directory wordt herkend en vraag Azure AD de dezelfde wijzigingen aanbrengen.
10. Azure AD harde match eerst uitgevoerd. Dat wil zeggen wordt gezocht als er een object met de immutableId gelijk is aan ' abcdefghijkl0123456789 == ". Vaste overeenkomst mislukt omdat er geen andere objecten in Azure AD die immutableId.
11. Azure AD wordt vervolgens geprobeerd te soft-match Bob Taylor. Dat wil zeggen wordt gezocht als er een object met proxyAddresses gelijk aan de drie waarden is, met inbegrip vansmtp:bob@contoso.com
12. Azure AD vindt van Bob Smith-object om te voldoen aan de criteria van de soft-match. Maar dit object heeft de waarde van immutableId = "abcdefghijklmnopqrstuv ==". vanuit een ander object uit Active Directory bij is die aangeeft dat dit object gesynchroniseerd. Azure AD kan niet soft-match deze objecten en dus leidt tot een synchronisatiefout **InvalidSoftMatch** .

#### <a name="how-to-fix-invalidsoftmatch-error"></a>Bij het herstellen van de fout InvalidSoftMatch
De meest voorkomende reden voor de fout InvalidSoftMatch is twee objecten met verschillende SourceAnchor \(immutableId\) dezelfde waarde hebben voor de ProxyAddresses en/of UserPrincipalName kenmerken die worden gebruikt tijdens de soft-match op Azure AD. Voor het vaststellen van de ongeldige zachte Match

1.  Identificeer de gedupliceerde proxyAddresses, userPrincipalName of andere kenmerkwaarde die de fout veroorzaakt. Ook weten welke twee \(of meer\) objecten in het conflict zijn betrokken. Het rapport gegenereerd door [Azure AD verbinding maken met de gezondheid voor synchronisatie](https://aka.ms/aadchsyncerrors) kunt u de twee objecten identificeren.
2. Bepalen welk object moet blijven de gedupliceerde waarde hebben en welk object moet niet.
3. Verwijder de dubbele waarde van het object dat moet geen waarde. Houd er rekening mee dat u ervoor moet de wijziging in de map waarin het object is opgehaald. In sommige gevallen moet u een van de objecten in een conflict verwijderen.
4. Als u de wijziging hebt aangebracht in de op AD, laat Azure AD verbinden de wijziging gesynchroniseerd.

Houd er rekening mee dat rapport in Azure AD verbinding maken met de gezondheid voor synchronisatie Sync elke 30 minuten bijgewerkt wordt en de fouten van de laatste synchronisatiepoging bevat.

>[AZURE.NOTE] ImmutableId, per definitie niet te wijzigen in de levensduur van het object. Als Azure AD verbinden niet met sommige van de scenario's in het achterhoofd in de bovenstaande lijst is geconfigureerd, kan het uiteindelijke in een situatie waar Azure AD verbinden wordt berekend met een andere waarde van de SourceAnchor voor de AD-object met dezelfde entiteit (dezelfde gebruiker/groep/contact enz) met een bestaande Azure AD Object dat u wilt blijven gebruiken.

#### <a name="related-articles"></a>Verwante artikelen
- [Dubbele of ongeldige kenmerken voorkomen adreslijstsynchronisatie in Office 365](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Beschrijving
Bij AD Azure soft twee objecten vergelijkt, is het mogelijk dat twee objecten van verschillende "objecttype" (bijvoorbeeld gebruiker, groep, Contact enz.) hebben dezelfde waarden voor de kenmerken voor het uitvoeren van de overeenkomst in zachte. Duplicatie van deze kenmerken is niet toegestaan in Azure AD, de bewerking kan leiden tot 'ObjectTypeMismatch' Fout bij de gegevenssynchronisatie.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Voorbeeldscenario's voor ObjectTypeMismatch-fout
- Een e-mailtoegang beveiligingsgroep wordt gemaakt in Office 365. Admin voegt een nieuwe gebruiker of contactpersoon in in pand AD (dat Azure AD nog niet gesynchroniseerd) met dezelfde waarde voor het kenmerk ProxyAddresses als die van de groep van Office 365.

#### <a name="example-case"></a>Voorbeeld van de aanvraag

1. Admin maakt een nieuwe beveiligingsgroep van e-mail is ingeschakeld in Office 365 voor de afdeling belastingen en vindt u een e-mailadres als tax@contoso.com. Hiermee wordt het kenmerk ProxyAddresses voor deze groep met de waarde van de toegewezen**smtp:tax@contoso.com**
2. Een nieuwe gebruiker wordt toegevoegd aan Contoso.com en een account gemaakt voor de gebruiker in ruimten met de proxyAddress als**smtp:tax@contoso.com**
3. Wanneer u de nieuwe gebruikersaccount, Azure AD Connect worden gesynchroniseerd, krijgt deze de fout 'ObjectTypeMismatch'.

#### <a name="how-to-fix-objecttypemismatch-error"></a>Bij het herstellen van de fout ObjectTypeMismatch
De meest voorkomende reden voor de fout ObjectTypeMismatch is twee objecten van verschillende type (gebruiker, groep, Contact enz.) hebben dezelfde waarde voor het kenmerk ProxyAddresses. Voor het vaststellen van de ObjectTypeMismatch:

1.  Identificatie van de gedupliceerde proxyAddresses (of een ander kenmerk)-waarde die de fout veroorzaakt. Ook weten welke twee \(of meer\) objecten in het conflict zijn betrokken. Het rapport gegenereerd door [Azure AD verbinding maken met de gezondheid voor synchronisatie](https://aka.ms/aadchsyncerrors) kunt u de twee objecten identificeren.
2. Bepalen welk object moet blijven de gedupliceerde waarde hebben en welk object moet niet.
3. Verwijder de dubbele waarde van het object dat moet geen waarde. Houd er rekening mee dat u ervoor moet de wijziging in de map waarin het object is opgehaald. In sommige gevallen moet u een van de objecten in een conflict verwijderen.
4. Als u de wijziging hebt aangebracht in de op AD, laat Azure AD verbinden de wijziging gesynchroniseerd. Rapport in Azure AD verbinding maken met de gezondheid voor synchronisatie sync wordt elke 30 minuten bijgewerkt en bevat de fouten van de laatste synchronisatiepoging.


## <a name="duplicate-attributes"></a>Dubbele kenmerken
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Beschrijving
Azure Active Directory-schema is niet toegestaan voor twee of meer objecten met dezelfde waarde van de volgende kenmerken hebben. Dat is dat elk object in Azure AD gedwongen een unieke waarde van deze kenmerken hebben op een bepaalde instantie.

- ProxyAddresses
- UserPrincipalName

Als Azure AD verbinding probeert om een nieuw object toevoegen of bijwerken van een bestaand object met een waarde voor de bovenstaande kenmerken die al is toegewezen aan een ander object in Azure Active Directory, wordt de bewerking resulteert in synchronisatiefout 'AttributeValueMustBeUnique'.
#### <a name="possible-scenarios"></a>Mogelijke scenario's:
1. Dubbele waarde is toegewezen aan een al gesynchroniseerd object, veroorzaakt een met een andere gesynchroniseerde object conflict.

#### <a name="example-case"></a>Voorbeeld van de aanvraag:
1. **Bob Smit** is een gebruiker gesynchroniseerd in Azure Active Directory uit in Active Directory van contoso.com gebouwen
2. Bob Smith **UserPrincipalName** in gebouwen is ingesteld als **bobs@contoso.com**.
3. Bob heeft ook de volgende waarden voor het kenmerk **proxyAddresses** :
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
4. **Robert Taylor**, een nieuwe gebruiker wordt toegevoegd aan het op Active Directory.
5. Bob Taylor van **UserPrincipalName** is ingesteld als **bobt@contoso.com**.
6. **Bob Taylor** heeft de volgende waarden voor het kenmerk **ProxyAddresses** i. smtp:bobt@contoso.comII. smtp:bob.taylor@contoso.com
7. Bob Taylor van object gesynchroniseerd met Azure AD met succes.
8. Admin besloten van Bob Taylor **ProxyAddresses** kenmerk bijwerken met de volgende waarde: ik. **smtp:bob@contoso.com**
9. Azure AD probeert object bij te werken van Bob Taylor in Azure AD met de bovenstaande waarde, maar dat mislukt als die waarde ProxyAddresses al aan Bob Smith toegewezen is, waardoor de fout 'AttributeValueMustBeUnique'.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>Bij het herstellen van de fout AttributeValueMustBeUnique
De meest voorkomende reden voor de fout AttributeValueMustBeUnique is twee objecten met verschillende SourceAnchor \(immutableId\) dezelfde waarde hebben voor de kenmerken ProxyAddresses en/of UserPrincipalName. Voor het vaststellen van AttributeValueMustBeUnique fout

1.  Identificeer de gedupliceerde proxyAddresses, userPrincipalName of andere kenmerkwaarde die de fout veroorzaakt. Ook weten welke twee \(of meer\) objecten in het conflict zijn betrokken. Het rapport gegenereerd door [Azure AD verbinding maken met de gezondheid voor synchronisatie](https://aka.ms/aadchsyncerrors) kunt u de twee objecten identificeren.
2. Bepalen welk object moet blijven de gedupliceerde waarde hebben en welk object moet niet.
3. Verwijder de dubbele waarde van het object dat moet geen waarde. Houd er rekening mee dat u ervoor moet de wijziging in de map waarin het object is opgehaald. In sommige gevallen moet u een van de objecten in een conflict verwijderen.
4. Als u de wijziging hebt aangebracht in de op AD, laat Azure AD verbinden synchroniseren de wijziging voor de fout hersteld.

#### <a name="related-articles"></a>Verwante artikelen
-[Dubbele of ongeldige kenmerken voorkomen adreslijstsynchronisatie in Office 365](https://support.microsoft.com/en-us/kb/2647098)


## <a name="data-validation-failures"></a>Mislukte gegevensvalidatie
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Beschrijving
Azure Active Directory zorgt ervoor dat verschillende beperkingen instellen voor de gegevens zelf voordat u toestaat dat gegevens worden geschreven naar de map. Dit is om ervoor te zorgen dat eindgebruikers de best mogelijke ervaring krijgen tijdens het gebruik van de toepassingen die afhankelijk van deze gegevens zijn.
#### <a name="scenarios"></a>Scenario 's
een. De waarde van het kenmerk UserPrincipalName is tekens ongeldig.
b. Het kenmerk UserPrincipalName voldoet niet aan de vereiste indeling.
#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Bij het herstellen van de fout IdentityDataValidationFailed

een. Zorg ervoor dat het kenmerk userPrincipalName tekens en de vereiste indeling is ondersteund.

#### <a name="related-articles"></a>Verwante artikelen
- [Voorbereiding van gebruikers via de directory-synchronisatie voor Office 365]( https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)


### <a name="datavalidationfailed"></a>DataValidationFailed
#### <a name="description"></a>Beschrijving
Dit is een zeer specifieke geval dat in een synchronisatiefout **'DataValidationFailed' resulteert** wanneer u het achtervoegsel van UserPrincipalName van een gebruiker wordt gewijzigd van een federatieve domein naar een ander domein van de federatieve.

#### <a name="scenarios"></a>Scenario 's
Het achtervoegsel UserPrincipalName is een gesynchroniseerde gebruiker gewijzigd van een federatieve domein naar een ander domein van federatieve in gebouwen. Bijvoorbeeld *UserPrincipalName = bob@contoso.com * is gewijzigd in *UserPrincipalName = bob@fabrikam.com *.

#### <a name="example"></a>Voorbeeld
1. Bob Smith, een account voor Contoso.com wordt toegevoegd als een nieuwe gebruiker in Active Directory met de UserPrincipalName.bob@contoso.com
2. Bob verplaatst naar een andere verdeling van Contoso.com Fabrikam.com genoemd en zijn UserPrincipalName wordt gewijzigd inbob@fabrikam.com
3. Domeinen contoso.com en fabrikam.com zijn federatieve domeinen met Azure Active Directory.
4. Bob's userPrincipalName bijgewerkt en resulteert in een synchronisatiefout 'DataValidationFailed'.

#### <a name="how-to-fix"></a>Bij het herstellen van
Als UserPrincipalName-achtervoegsel van de gebruiker is bijgewerkt van bob@ **contoso.com** naar bob@ **fabrikam.com**, waar **contoso.com** en **fabrikam.com** **federatieve domeinen**zijn, volg de onderstaande stappen te volgen om de synchronisatiefout te herstellen

1. Bijwerken van de gebruiker UserPrincipalName in Azure AD van bob@contoso.com op bob@contoso.onmicrosoft.com. Met de Module Azure AD PowerShell kunt u de volgende PowerShell-opdracht:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`

2. Kunnen de volgende cyclus van de synchronisatie wilt synchroniseren. Deze synchronisatie is geslaagd en de UserPrincipalName Bob te worden bijgewerkt bob@fabrikam.com zoals verwacht.


## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Beschrijving
Wanneer een kenmerk overschrijdt de toegestane maximumgrootte, maximale lengte of maximum aantal Azure Active Directory-schema instellen, leidt de synchronisatiebewerking de synchronisatiefout **LargeObject** of **ExceededAllowedLength** . Deze fout treedt meestal op voor de volgende kenmerken

- userCertificate
- thumbnailPhoto
- proxyAddresses

### <a name="possible-scenarios"></a>Mogelijke scenario 's
1. Bob's userCertificate kenmerk is te veel certificaten toegewezen aan Bob op te slaan. Deze mag oudere, verlopen certificaten bevatten.
2. Bob's thmubnailPhoto instellen in Active Directory is te groot om te worden gesynchroniseerd in Azure AD.
3. Tijdens de automatische populatie van het kenmerk ProxyAddresses in Active Directory een object kreeg toegewezen > 500 ProxyAddresses.

### <a name="how-to-fix"></a>Bij het herstellen van

1. Zorg ervoor dat het kenmerk dat de fout veroorzaakt binnen de toegestane beperking.

## <a name="related-links"></a>Verwante koppelingen
- [Ga naar Active Directory-objecten in Active Directory Administrative Center] (https://technet.microsoft.com/library/dd560661.aspx)
- [Azure Active Directory voor een object met Azure Active Directory PowerShell doorzoeken](https://msdn.microsoft.com/library/azure/jj151815.aspx)
