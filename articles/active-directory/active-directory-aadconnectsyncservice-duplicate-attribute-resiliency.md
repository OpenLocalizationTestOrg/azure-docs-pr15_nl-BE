<properties
    pageTitle="Tolerantie voor synchronisatie en het duplicaat kenmerk | Microsoft Azure"
    description="Nieuwe gedrag van het werken met objecten met UPN- of ProxyAddress conflicten tijdens de directory synchronisatie met Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="markusvi"/>



# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Tolerantie voor synchronisatie en het duplicaat kenmerk
Dubbele kenmerk tolerantie is een functie in Azure Active Directory dat wrijving door **UserPrincipalName** en **ProxyAddress** conflicten vermindert wanneer een van de hulpprogramma's van Microsoft voor synchronisatie wordt uitgevoerd.

Deze twee kenmerken zijn over het algemeen moet uniek zijn voor de **gebruiker**, **groep**of **contactpersoon** alle objecten in een bepaalde huurder van Azure Active Directory.

> [AZURE.NOTE] Alleen gebruikers kunnen UPN-namen hebben.

Het nieuwe gedrag waarmee deze functie wordt in het gedeelte van de wolk van de pijpleiding sync, is deze client agnostische en relevant zijn voor Microsoft synchronisatie producten waaronder Azure AD verbinden, DirSync en MIM-Connector. De algemene term 'sync client' wordt gebruikt in dit document voor een van deze producten.

## <a name="current-behavior"></a>Huidige werking
Als er een poging tot het creëren van een nieuw object met een waarde voor UPN- of ProxyAddress die in strijd met deze beperking van uniekheid, blokkeert Azure Active Directory dat object worden gemaakt. Op dezelfde manier als een object wordt bijgewerkt met een niet-uniek UPN- of ProxyAddress, mislukt het bijwerken. Het inrichtingsproces poging of de update opnieuw wordt gestart door de client synchroniseren na elke cyclus van export en blijft mislukken totdat het conflict opgelost is. Een e-mailbericht rapport fout wordt gegenereerd bij elke poging en een fout wordt geregistreerd door de client synchroniseren.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Probleem met dubbele kenmerk tolerantie
In plaats van helemaal niet te creëren of een object wilt bijwerken met een dubbel kenmerk, quarantaine Azure Active Directory"" het dubbele kenmerk dat zou strijdig zijn met de beperking van uniekheid. Als dit kenmerk vereist is voor de inrichting zoals UserPrincipalName, wijst de service de aanduidingswaarde van een tijdelijke. De indeling van deze tijdelijke waarden is  
"***<OriginalPrefix>+<4DigitNumber>@<InitialTenantDomain>. onmicrosoft.com***".  
Als het kenmerk niet vereist, zoals een **ProxyAddress is**, Azure Active Directory simpelweg het kenmerk conflict quarantaine en wordt doorgegaan met het object maken of bijwerken.

Bij het kenmerk isoleert, wordt informatie over het conflict in de dezelfde fout rapport e-mailadres die worden gebruikt in het oude gedrag worden verzonden. Deze informatie wordt alleen weergegeven in het rapport één keer wanneer de quarantaine gebeurt, deze komt niet blijft echter in toekomstige e-mails worden vastgelegd. Ook omdat de uitvoer voor dit object is voltooid, de synchronisatie-client wordt niet door een fout geregistreerd en wordt niet opnieuw voor het maken / bijwerken bewerking bij de volgende synchronisatie cycli.

Ter ondersteuning van dit probleem is een nieuw kenmerk toegevoegd aan de gebruikers, groepen en contactpersonen objectklassen:  
**DirSyncProvisioningErrors**

Dit is een kenmerk met meerdere waarden dat wordt gebruikt voor het opslaan van de conflicterende attributen die zou strijdig zijn met de beperking van uniekheid, moeten zij worden toegevoegd normaal. Een achtergrondtaak timer is ingeschakeld in Azure Active Directory die elk uur om te zoeken naar dubbele kenmerk conflicten die zijn opgelost en de betrokken kenmerken worden automatisch verwijderd uit quarantaine wordt uitgevoerd.

### <a name="enabling-duplicate-attribute-resiliency"></a>Dubbel kenmerk Resiliency inschakelen
Dubbele kenmerk tolerantie is het nieuwe standaardgedrag voor alle huurders van Azure Active Directory. Deze zijn standaard ingeschakeld voor alle huurders die synchronisatie voor de eerste keer op de 22e augustus 2016 of later wordt ingeschakeld. Huurders die ingeschakeld sync vóór die datum heeft de functie is ingeschakeld in batches. Deze zogenaamde rollout begint in September 2016 en een e-mailbericht ontvangt van elke huurder technische melding contact met de specifieke datum waarop de functie wordt ingeschakeld.

Nadat dubbele kenmerk tolerantie is ingeschakeld kan niet worden uitgeschakeld.

Als u wilt controleren of de functie is ingeschakeld voor uw huurder, kunt u doen door de meest recente versie van de module Azure Active Directory PowerShell downloaden en uitvoeren:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

Als u graag proactief de inschakelen voordat het voor de huurder is ingeschakeld, kunt u dit doen door de meest recente versie van de module Azure Active Directory PowerShell downloaden en uitvoeren:

`Set-MsolDirSyncFeature -Feature DuplicateUPNResiliency -Enable $true`

`Set-MsolDirSyncFeature -Feature DuplicateProxyAddressResiliency -Enable $true`

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Identificeren van objecten met DirSyncProvisioningErrors
Er zijn momenteel twee methoden voor het identificeren van objecten die deze fouten als gevolg van conflicten met dubbele eigenschap, Azure Active Directory PowerShell en Office 365 Admin Portal hebben. Er zijn plannen om uit te breiden naar extra portal gebaseerde rapportage in de toekomst.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
Voor de PowerShell cmdlets in dit onderwerp, de volgende voorwaarden wordt voldaan:

- De volgende cmdlets zijn hoofdlettergevoelig.
- De **– ErrorCategory PropertyConflict** moet altijd worden opgenomen. Er zijn momenteel geen andere soorten **ErrorCategory**, maar dit in de toekomst kan worden verlengd.

Eerst aan de slag door **Verbinding maken met MsolService** en invoeren van referenties voor de beheerder van een huurder.

Vervolgens de volgende cmdlets en operatoren gebruiken om fouten op verschillende manieren weer te geven:

1. [Alle bekijken](#see-all)

2. [Door de eigenschapstype](#by-property-type)

3. [Door conflicterende waarde](#by-conflicting-value)

4. [Met behulp van een tekenreeks zoeken](#using-a-string-search)

5. [Gesorteerd](#sorted)

6. [In een beperkt aantal of alle](#in-a-limited-quantity-or-all)


#### <a name="see-all"></a>Alle bekijken
Zodra verbonden, een algemeen overzicht van het kenmerk inrichten uitvoeren fouten in de huurder:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Dit levert een resultaat als volgt uit:  
 ![Get-MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  


#### <a name="by-property-type"></a>Door de eigenschapstype
Overzicht van fouten die door de eigenschapstype toevoegen de **PropertyName -** vlag met het argument **UserPrincipalName** **ProxyAddresses** :

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Of

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Door conflicterende waarde
Fouten met betrekking tot een specifieke eigenschap toe te voegen de **Eigenschapwaarde -** vlag (**- Eigenschapnaam** moet ook worden gebruikt bij het toevoegen van deze vlag):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`


#### <a name="using-a-string-search"></a>Met behulp van een tekenreeks zoeken
Een brede reeks zoeken gebruik de vlag **- Zoekreeks** . Dit kan afzonderlijk worden gebruikt in alle bovenstaande vlaggen, met uitzondering van **-ErrorCategory PropertyConflict**, die is vereist:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>In een beperkt aantal of alle
1. **MaxResults <Int> ** kan worden gebruikt om de query beperken tot een bepaald aantal waarden.

2. **Alle** kunnen worden gebruikt om ervoor te zorgen dat alle resultaten worden opgehaald in het geval dat een groot aantal fouten bestaat.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Office 365-beheerportal

U kunt synchronisatiefouten directory weergeven in het Office 365 admin center. **User** -objecten dat deze fouten worden alleen weergegeven in het rapport in de Office 365 portal. Info over conflicten tussen **groepen** en **contactpersonen**wordt niet weergegeven.


![Actieve gebruikers] (./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "Actieve gebruikers")

Zie voor instructies voor het directory synchronisatiefouten weergeven in het Office 365 admin center [identificeren directory synchronisatiefouten in Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).


### <a name="identity-synchronization-error-report"></a>Identiteit rapport
Wanneer een object met een dubbel kenmerk conflict wordt verwerkt met dit nieuwe gedrag een kennisgeving is opgenomen in de standaard identiteit rapport e-mail die wordt verzonden naar de kennisgeving van technische contactpersoon voor de huurder. Er is echter een belangrijke wijziging in dit gedrag. In het verleden, zou informatie over een conflict dubbel kenmerk worden opgenomen in elke latere foutenrapport totdat het conflict is opgelost. Met dit nieuwe gedrag wordt de foutmelding voor een bepaald conflict alleen weergegeven eens - op het moment dat de conflicterende kenmerk in quarantaine is geplaatst.

Hier volgt een voorbeeld van het e-mailbericht ziet er als er conflicten zijn ProxyAddress:  
    ![Actieve gebruikers](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

## <a name="resolving-conflicts"></a>Conflicten oplossen
Strategie en resolutie tactieken voor deze fouten oplossen moet niet verschillen van de manier waarop dubbele kenmerk fouten in het verleden zijn verwerkt. Het enige verschil is dat de timer taak steeds de huurder aan de service-het desbetreffende kenmerk automatisch naar het juiste object toevoegen als het conflict opgelost is.

Het volgende artikel geeft een overzicht van verschillende strategieën voor het oplossen van problemen en resolutie: [dubbele of ongeldige kenmerken voorkomen adreslijstsynchronisatie in Office 365](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Bekende problemen
Geen van deze problemen zorgt ervoor dat gegevens verloren gaan of service afbraak. Sommige van hen zijn esthetische en andere zorgt ervoor dat bepaalde fouten vereist extra handmatige correctie van anderen standaard "*vooraf tolerantie*" dubbel kenmerk dit fouten veroorzaken in plaats van het kenmerk conflict in quarantaine plaatsen worden gegenereerd.

**Core gedrag:**

1. Objecten met een bepaald kenmerk configuraties blijven ontvangen uitvoer fouten in plaats van de dubbele kenmerken die in quarantaine worden geplaatst.  
Bijvoorbeeld:

    een. Nieuwe gebruiker wordt gemaakt in AD met de UPN- **Joe@contoso.com** en ProxyAddress**smtp:Joe@contoso.com**

    b. De eigenschappen van dit object in strijd zijn met een bestaande groep, waarbij ProxyAddress is **SMTP:Joe@contoso.com**.

    c. Bij uitvoer een **conflict ProxyAddress** fout gegenereerd in plaats van de kenmerken van het conflict in quarantaine geplaatst. De bewerking wordt opnieuw geprobeerd bij iedere latere sync-cyclus als zou zijn geweest voordat de tolerantiefunctie is ingeschakeld.

2. Als twee groepen op ruimten met hetzelfde SMTP-adres gemaakt zijn, mislukt een bepaling bij de eerste poging met een standaard dubbele **ProxyAddress** -fout. Dubbele waarden in goed quarantaine bij de volgende synchronisatie-cyclus.

**Office Portal rapport**:

1. Het gedetailleerde foutbericht voor twee objecten in een UPN-conflict is hetzelfde. Dit betekent dat ze hebben beide hun UPN gewijzigd / quarantaine, wanneer in feite slechts een van hen geen gegevens gewijzigd hadden.

2. Het gedetailleerde foutbericht van een UPN-conflict geeft de verkeerde displayName voor een gebruiker die de UPN gewijzigd/quarantaine heeft gehad. Bijvoorbeeld:

    een. **Een gebruiker** wordt gesynchroniseerd eerst met **UPN = User@contoso.com **.

    b. **Gebruiker B** wordt geprobeerd moet worden gesynchroniseerd met jarig **UPN = User@contoso.com **.

    c. **Gebruiker B** UPN wordt gewijzigd in **User1234@contoso.onmicrosoft.com** en **User@contoso.com** wordt toegevoegd aan de **DirSyncProvisioningErrors**.

    d. Het foutbericht voor de **Gebruiker B** moet aangeven dat **een gebruiker** al heeft **User@contoso.com** als een UPN, maar de eigen naam **Van gebruiker B** bevat.



**Identiteit rapport**:

De koppeling voor *Stapsgewijze instructies voor het oplossen van dit probleem* is onjuist:  
    ![Actieve gebruikers](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

Het moet verwijzen naar [https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency).


## <a name="see-also"></a>Zie ook

- [Azure AD verbinden sync](active-directory-aadconnectsync-whatis.md)

- [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)

- [Identificeren van directory synchronisatiefouten in Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)
