<properties
    pageTitle="Meer informatie: Azure AD wachtwoordbeheer | Microsoft Azure"
    description="Geavanceerde onderwerpen over Azure AD wachtwoordbeheer, inclusief wachtwoord writeback werking, wachtwoordbeveiliging Write-back, hoe het wachtwoord opnieuw instellen werkt portal en welke gegevens wordt gebruikt door het wachtwoord opnieuw instellen."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="learn-more-about-password-management"></a>Meer informatie over het wachtwoord

> [AZURE.IMPORTANT] **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

Als u wachtwoordbeheer al hebt geïmplementeerd, of gewoon meer informatie over de technische mailindelingen dat ingaan hoe het werkt voordat u distribueert, in deze sectie krijgt u een goed overzicht van de technische concepten achter de service die op zoek zijn. Komen de volgende:

* [**Wachtwoord Write-back-overzicht**](#password-writeback-overview)
  - [De werking van wachtwoord Write-back](#how-password-writeback-works)
  - [Scenario's voor wachtwoord Write-back wordt ondersteund](#scenarios-supported-for-password-writeback)
  - [Wachtwoord Write-back-beveiligingsmodel](#password-writeback-security-model)
* [**Hoe het wachtwoord opnieuw instellen portal werk?**](#how-does-the-password-reset-portal-work)
  - [Welke gegevens worden gebruikt door het wachtwoord opnieuw instellen?](#what-data-is-used-by-password-reset)
  - [Wachtwoord toegang tot gegevens van uw gebruikers opnieuw instellen](#how-to-access-password-reset-data-for-your-users)

## <a name="password-writeback-overview"></a>Wachtwoord Write-back-overzicht
Wachtwoord Write-back is een onderdeel voor [Verbinding Azure Active Directory](active-directory-aadconnect.md) dat kan worden ingeschakeld en gebruikt door de huidige abonnees van Premium van Azure Active Directory. Zie voor meer informatie [Azure Active Directory-edities](active-directory-editions.md).

Writeback wachtwoord kunt u uw huurder wolk schrijven wachtwoorden voor u op-premises Active Directory configureren.  U hoeft te installeren en beheren van een complex van gebouwen wachtwoord zelf oplossing overbodig maakt en biedt een handige manier cloud-gebaseerde voor gebruikers in te stellen hun wachtwoorden in ruimten waar ze ook zijn.  Lees verder voor een aantal van de belangrijkste functies van wachtwoord Write-back:

- **De feedback is nul vertraging.**  Wachtwoord Write-back is een synchrone bewerking.  Gebruikers wordt direct gewaarschuwd als hun wachtwoord niet voldoet aan beleid of kon niet opnieuw worden ingesteld of gewijzigd om welke reden.
- **Ondersteunt het opnieuw instellen van wachtwoorden voor gebruikers met AD FS of een andere Federatie.**  Met wachtwoord Write-back, zolang de federatieve gebruikersaccounts zijn gesynchroniseerd met uw huurder Azure AD mogelijk voor het beheren van hun lokalen op AD wachtwoorden vanuit de cloud.
- **Ondersteunt het opnieuw instellen van wachtwoorden voor gebruikers met wachtwoord hash-sync.** Als het wachtwoord opnieuw instellen-service detecteert dat een gesynchroniseerde gebruikersaccount bij wachtwoord hash-synchronisatie is ingeschakeld, opnieuw ingesteld zowel op gebouwen van deze account en het wachtwoord van de wolk tegelijk.
- **Ondersteunt het wijzigen van wachtwoorden in het Configuratiescherm toegang en Office 365.**  Als federatieve of wachtwoord synchronisatie zouden gebruikers komen hun vervallen of niet-verlopen wachtwoord wijzigen, we zult schrijft deze wachtwoorden naar uw lokale AD-omgeving.
- **Ondersteunt het vorige wachtwoorden schrijven wanneer een beheerder opnieuw van instellen de** [**Azure Management Portal**](https://manage.windowsazure.com).  Wanneer een beheerder opnieuw instellen van het wachtwoord van een gebruiker in [Azure Management Portal](https://manage.windowsazure.com), als die gebruiker federated is of wachtwoord synchronisatie zou zullen we het wachtwoord dat Admin wordt geselecteerd op de lokale AD, ook instellen.  Dit wordt momenteel niet ondersteund in Office Admin Portal.
- **Hiermee wordt uw locatie op AD-wachtwoordbeleid.**  Wanneer een gebruiker zijn of haar wachtwoord herstelt, we zorgen ervoor dat dit voldoet aan de lokalen op Active Directory-beleid voordat deze naar die map.  Dit omvat geschiedenis, complexiteit, leeftijd, wachtwoordfilters en andere wachtwoordbeperkingen die u hebt gedefinieerd in de lokale AD.
- **Niet nodig alle binnenkomende firewallregels.**  Een relay Azure Service Bus writeback wachtwoord gebruikt als een onderliggende communicatiekanaal, wat betekent dat u niet hoeft te openen alle inkomende poorten in de firewall voor deze functie te werken.
- **Wordt niet ondersteund voor gebruikersaccounts die zijn opgeslagen in de beveiligde groepen in Active Directory op gebouwen.** Zie voor meer informatie over beveiligde groepen [beveiligde Accounts en groepen in Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

### <a name="how-password-writeback-works"></a>De werking van wachtwoord Write-back
Wachtwoord Write-back heeft drie hoofdonderdelen:

- Wachtwoord opnieuw instellen van cloud service (dit is ook geïntegreerd in Azure AD wachtwoord wijzigen pagina's)
- Huurder-specifieke Azure Service Bus relay
- Wachtwoordhersteldiskette-eindpunt op prem

Ze passen samen zoals beschreven in het onderstaande diagram:

  ![][001]

Bij het synchroniseren van een federatieve of wachtwoord hash zou gebruiker komt opnieuw instellen of zijn of haar wachtwoord wijzigen in de cloud, gebeurt het volgende:

1.  We controleren om te zien wat voor type wachtwoord van de gebruiker heeft.  Als we dat het wachtwoord wordt beheerd in gebouwen, zorg wij dat de Write-back-service actief is.  Als het is, we de gebruiker doorgaat, als dat niet het geval is, laten we vertelt de gebruiker die het wachtwoord niet opnieuw instellen hier.
2.  Vervolgens wordt de gebruiker geeft de juiste verificatie-gates en bereikt het wachtwoord opnieuw instellen scherm.
3.  De gebruiker selecteert een nieuw wachtwoord en bevestigt deze.
4.  Wanneer u op verzenden klikt, coderen wij het wachtwoord als leesbare tekst met een symmetrische sleutel die is gemaakt tijdens de installatie van Write-back.
5.  Nadat u het wachtwoord te coderen, opnemen we in een pakket dat wordt verzonden via een HTTPS-kanaal op uw huurder specifieke service bus relay (die we ook voor u ingesteld tijdens de installatie van Write-back).  Deze relay is beveiligd door een willekeurig gegenereerd wachtwoord dat alleen de installatie in gebouwen weet.
6.  Zodra het bericht service bus bereikt, wordt het wachtwoord opnieuw instellen eindpunt automatisch wakker en blijkt dat er een aanvraag in behandeling.
7.  De service zoekt vervolgens naar de gebruiker in kwestie met het kenmerk wolk anker.  Voor deze zoekactie alleen mogelijk als het gebruikersobject in de connector AD ruimte moet bestaan, moet het worden gekoppeld aan de bijbehorende MV-object en het moet worden gekoppeld aan de bijbehorende AAD connector-object. Tot slot voor synchronisatie met het vinden van deze gebruikersaccount, de koppeling van een object van AD-connector naar MV moet hebben de regel sync `Microsoft.InfromADUserAccountEnabled.xxx` op de koppeling.  Dit is nodig omdat als de oproep uit de cloud binnenkomt, de synchronisatie-engine maakt gebruik van het kenmerk cloudAnchor AAD connector space-object opzoeken en vervolgens volgt de koppeling naar het object MV en volgt de koppeling naar het object AD. Omdat er mogelijk meerdere AD objecten (met meerdere forests) voor dezelfde gebruiker, de synchronisatie-engine is gebaseerd op de `Microsoft.InfromADUserAccountEnabled.xxx` koppelen aan het kiezen van de juiste is.
8.  Als de gebruikersaccount wordt gevonden, proberen we het wachtwoord rechtstreeks in de juiste AD forest opnieuw instellen.
9.  Als het wachtwoord set-bewerking geslaagd is, zien we de gebruiker het wachtwoord is gewijzigd en dat ze op hun prettige manier kunnen gaan.
10. Als het wachtwoord mislukt, we de fout retourneren naar de gebruiker en laat hem opnieuw.  De bewerking kan mislukken omdat de service is, omdat het ze het geselecteerde wachtwoord niet voldeed aan beleid van de organisatie, omdat we de gebruiker niet in de lokale AD vinden kan of een aantal redenen.  We hebben een bericht dat voor veel van deze gevallen en de gebruiker laten weten wat zij kunnen doen om het probleem te verhelpen.

### <a name="scenarios-supported-for-password-writeback"></a>Scenario's voor wachtwoord Write-back wordt ondersteund
In de onderstaande tabel wordt beschreven welke scenario's worden ondersteund voor welke versies van onze synchronisatiemogelijkheden biedt.  In het algemeen verdient de meest recente versie van [Azure AD Connect](active-directory-aadconnect.md#install-azure-ad-connect) te installeren, als het gewenste wachtwoord Write-back.

  ![][002]

### <a name="password-writeback-security-model"></a>Wachtwoord Write-back-beveiligingsmodel
Wachtwoord Write-back is een krachtige en uiterst veilige service.  Om ervoor te zorgen dat uw gegevens worden beschermd, inschakelen we een beveiligingsmodel 4 lagen die hieronder wordt beschreven.

- **Huurder specifieke service bus relay** – bij het instellen van de service, stelt een huurder-specifieke service bus relay die wordt beveiligd door een willekeurig gegenereerd sterk wachtwoord dat nooit toegang tot Microsoft heeft.
- **Locked down cryptografisch sterke wachtwoordversleutelingssleutel** – nadat de relay service bus is gemaakt, maken we een sterke symmetrische sleutel die wij gebruiken voor het coderen van het wachtwoord als het gaat over het netwerk.  Deze sleutel woont alleen in het geheim archief van uw bedrijf in de cloud die sterk is vergrendeld en gecontroleerd, net als bij andere wachtwoorden in Active directory.
- **Industrie-standaard TLS** – wanneer u een wachtwoord opnieuw instellen of wijzigen van de bewerking plaatsvindt in de cloud, we nemen het leesbare wachtwoord coderen met uw openbare sleutel.  We vervolgens plop die in een HTTPS-bericht dat wordt verzonden via een gecodeerd kanaal met behulp van SSL-certificaten voor uw service bus relay van Microsoft.  Nadat het bericht aankomt op de Bus-Service, de agent op prem ontwaakt, wordt geverifieerd bij een Service Bus met het wachtwoord dat eerder is gegenereerd, het gecodeerde bericht opvangt, decodeert het wachtwoord via de API voor AD DS SetPassword instellen met behulp van de persoonlijke sleutel die is gegenereerd en pogingen.  Deze stap is wat we gebruikt om uw advertentie op prem wachtwoordbeleid (complexiteit, leeftijd, geschiedenis, filters, enz) kunnen in de cloud.
- **Bericht verloopbeleid** -ten slotte, als om welke reden het bericht bevindt zich in de Bus Service omdat uw service op prem down is, deze time-out en verwijderd na een paar minuten om de beveiliging nog verder verbeteren.

## <a name="how-does-the-password-reset-portal-work"></a>Hoe het wachtwoord opnieuw instellen portal werk?
Wanneer een gebruiker naar de wachtwoordhersteldiskette-portal, een werkstroom wordt gestart als die account geldig is, bepalen welke organisatie waartoe gebruikers behoort, indien het wachtwoord van de gebruiker wordt beheerd, en of de gebruiker is een licentie om de functie te gebruiken.  Lees de onderstaande stappen om te leren over de logica achter de pagina wachtwoord opnieuw instellen.

1.  Gebruiker klikt op de heeft geen toegang tot uw account koppelen of gaan rechtstreeks naar [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2.  Gebruiker voert een gebruikers-id en een captcha wordt doorgegeven.
3.  Azure AD wordt gecontroleerd als de gebruiker kan deze functie gebruiken door het volgende te doen:
    - Controleert of de gebruiker deze functie is ingeschakeld en een Azure AD-licentie toegewezen is.
        - Als de gebruiker niet over deze functie is ingeschakeld, of een licentie is toegewezen, wordt de gebruiker gevraagd contact op met beheerder zijn of haar als zijn of haar wachtwoord opnieuw wilt instellen.
    - Controles die de gebruiker heeft het recht gegevens die zijn gedefinieerd op zijn of haar account beheerder beleid een uitdaging.
        - Als het beleid is slechts één vraag, wordt vervolgens gewaarborgd dat de gebruiker de juiste gegevens gedefinieerd voor ten minste een van de uitdagingen die zijn ingeschakeld door de beheerdersbeleid heeft.
          - Als de gebruiker niet is geconfigureerd, wordt de gebruiker aanbevolen contact op met beheerder zijn of haar als zijn of haar wachtwoord opnieuw wilt instellen.
        - Als het beleid twee uitdagingen vereist, wordt vervolgens gewaarborgd dat de gebruiker de juiste gegevens gedefinieerd voor ten minste twee van de uitdagingen die zijn ingeschakeld door de beheerdersbeleid heeft.
          - Als de gebruiker niet is geconfigureerd, is er de gebruiker contact op met de beheerder van zijn of haar als zijn of haar wachtwoord opnieuw wilt instellen.
    - Controleert of het wachtwoord van de gebruiker wordt beheerd in gebouwen (federatieve of wachtwoord hash sync was).
       - Als Write-back wordt geïmplementeerd en het wachtwoord van de gebruiker wordt beheerd in gebouwen, is de gebruiker toegestaan om verder te gaan om te verifiëren en zijn of haar wachtwoord opnieuw instellen.
       - Als Write-back is niet geïmplementeerd en het wachtwoord van de gebruiker wordt beheerd in gebouwen, wordt de gebruiker gevraagd contact op met beheerder zijn of haar als zijn of haar wachtwoord opnieuw wilt instellen.
4.  Als blijkt dat de gebruiker kan zijn of haar wachtwoord opnieuw ingesteld, wordt de gebruiker geholpen bij het opnieuw instellen.

Meer informatie over het implementeren van wachtwoord terugschrijven op [aan de slag: wachtwoordbeheer Azure AD](active-directory-passwords-getting-started.md).

### <a name="what-data-is-used-by-password-reset"></a>Welke gegevens worden gebruikt door het wachtwoord opnieuw instellen?
De volgende tabel contouren waar en hoe deze gegevens tijdens het opnieuw instellen van wachtwoorden wordt gebruikt en is ontworpen om te beslissen welke verificatie beschikbaar zijn voor uw organisatie. Deze tabel bevat ook eventuele opmaak vereisten voor gevallen waarin u gegevens bieden ten behoeve van gebruikers van invoer paden die deze gegevens niet worden gevalideerd.

> [AZURE.NOTE] Telefoon (werk) niet in de portal Registratie omdat gebruikers momenteel niet kunnen bewerken van deze eigenschap in de directory.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Naam van de methode van contact</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Element Azure Active Directory-gegevens</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Gebruikte / instelbare waar?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Vereisten</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Telefoon (werk)</p>
            </td>
            <td>
              <p>Telefoonnummer</p>
              <p>bijvoorbeeld Set-MsolUser - UserPrincipalName JWarner@contoso.com - telefoonnummer '1234567890 + 1 x 1234'</p>
            </td>
            <td>
              <p>Gebruikt in:</p>
              <p>Wachtwoord opnieuw instellen van Portal</p>
              <p>Instelbaar van:</p>
              <p>Telefoonnummer kan worden ingesteld in PowerShell, DirSync Azure Management Portal en het Office Admin Portal</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (bijvoorbeeld + 1 1234567890)</p>
              <ul>
                <li class="unordered">
Moet een landcode opgeven<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Een netnummer moet opgeven (indien van toepassing)<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Hebben verstrekt een + op de voorgrond van het land code<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Een ruimte tussen het land en de rest van het nummer moet zijn<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Extensies worden niet ondersteund, hebt u de extensies die zijn opgegeven, zullen we het strook vanaf het nummer vóór de installatie van het telefoongesprek.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Mobiele telefoon</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>OR</p>
              <p>MobilePhone</p>
              <p>(Verificatie die telefoon wordt gebruikt als er gegevens aanwezig zijn, anders is dit valt terug op het veld mobiele telefoon).</p>
              <p>bijvoorbeeld Set-MsolUser - UserPrincipalName JWarner@contoso.com - MobilePhone '1234567890 + 1 x 1234'</p>
            </td>
            <td>
              <p>Gebruikt in:</p>
              <p>Wachtwoord opnieuw instellen van Portal</p>
              <p>Portal voor registratie</p>
              <p>Instelbaar van: </p>
              <p>AuthenticationPhone kan worden ingesteld vanaf de portal Registratie van wachtwoord opnieuw instellen of portal Registratie van MVR gesloten.</p>
              <p>MobilePhone kan worden ingesteld in PowerShell, DirSync Azure Management Portal en het Office Admin Portal</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (bijvoorbeeld + 1 1234567890)</p>
              <ul>
                <li class="unordered">
Moet een landcode opgeven.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Geef een netnummer (indien van toepassing).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Moet hebben een + op de voorgrond van het land-code opgeven.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Moet een spatie tussen het land en de rest van het nummer hebben.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Extensies worden niet ondersteund, hebt u de extensies die zijn opgegeven, we genegeerd wanneer het telefoongesprek te verzenden.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Alternatieve e-mailadres</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>OR</p>
              <p>AlternateEmailAddresses [0] </p>
              <p>(Verificatie die e-mailadres wordt gebruikt als er gegevens aanwezig zijn, anders is dit valt terug op het alternatieve e-mailadres-veld).</p>
              <p>Opmerking: het veld alternatieve e-mailadres is opgegeven als een matrix van tekenreeksen in de directory.  We gebruiken de eerste post in deze matrix.</p>
              <p>bijvoorbeeld Set-MsolUser - UserPrincipalName JWarner@contoso.com - AlternateEmailAddresses"email@live.com"</p>
            </td>
            <td>
              <p>Gebruikt in:</p>
              <p>Wachtwoord opnieuw instellen van Portal</p>
              <p>Portal voor registratie</p>
              <p>Instelbaar van: </p>
              <p>AuthenticationEmail kan worden ingesteld vanaf de portal Registratie van wachtwoord opnieuw instellen of portal Registratie van MVR gesloten.</p>
              <p>AlternateEmail kan worden ingesteld via PowerShell, de Azure Management Portal en het Office Admin Portal</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a>of甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
E-mailberichten moeten volgen als per standaardopmaak.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
E-mails in Unicode worden ondersteund.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Beveiliging vragen en antwoorden</p>
            </td>
            <td>
              <p>Rechtstreeks in de map wijzigen niet beschikbaar.</p>
            </td>
            <td>
              <p>Gebruikt in:</p>
              <p>Wachtwoord opnieuw instellen van Portal</p>
              <p>Portal voor registratie </p>
              <p>Instelbaar van: </p>
              <p>De enige manier om zekerheid vragen stellen is via de Portal Azure Management.</p>
              <p>De enige manier om te antwoorden op vragen over de beveiliging voor een bepaalde gebruiker ingesteld is via de Portal registratie.</p>
            </td>
            <td>
              <p>Vragen over beveiliging hebben een maximum van 200 tekens en een minimum van 3 tekens</p>
              <p>Antwoorden hebben een maximum van 40 tekens en een minimum van 3 tekens</p>
            </td>
          </tr>
        </tbody></table>

###<a name="how-to-access-password-reset-data-for-your-users"></a>Wachtwoord toegang tot gegevens van uw gebruikers opnieuw instellen
####<a name="data-settable-via-synchronization"></a>Instelbare via synchronisatie van gegevens
De volgende velden kunnen worden gesynchroniseerd op ruimten:

* Mobiele telefoon
* Telefoon (werk)

####<a name="data-settable-with-azure-ad-powershell"></a>Gegevens met Azure AD PowerShell instelbaar
De volgende velden zijn toegankelijk met Azure AD PowerShell & de Graph API:

* Alternatieve e-mailadres
* Mobiele telefoon
* Telefoon (werk)
* Telefoonnummer verificatie
* Verificatie-e-mailadres

####<a name="data-settable-with-registration-ui-only"></a>Instelbare met UI registratie alleen gegevens
De volgende velden zijn alleen toegankelijk via de registratie van SSPR UI (https://aka.ms/ssprsetup):

* Beveiliging vragen en antwoorden

####<a name="what-happens-when-a-user-registers"></a>Wat gebeurt er wanneer een gebruiker registreert?
Wanneer een gebruiker zich aanmeldt, wordt de registratiepagina **altijd** set de volgende velden:

* Telefoonnummer verificatie
* Verificatie-e-mailadres
* Beveiliging vragen en antwoorden

Als u een waarde hebt opgegeven voor de mobiele **Telefoon** of een **Ander e-mailadres**, kunnen gebruikers onmiddellijk gebruiken die opnieuw instellen van wachtwoorden, zelfs als ze nog niet geregistreerd voor de service.  Bovendien gebruikers zien deze waarden bij het registreren voor de eerste keer, en ze desgewenst wijzigen.  Echter, nadat zij zijn geregistreerd, deze waarden bewaard in de velden **Telefoon verificatie** en **Verificatie-e-** respectievelijk.

Dit is een handige manier om grote aantallen gebruikers SSPR gebruiken en gebruikers voor het valideren van deze informatie het registratieproces toch de blokkering opheffen.

####<a name="setting-password-reset-data-with-powershell"></a>Instellen van het wachtwoord opnieuw instellen voor gegevens met PowerShell
U kunt de waarden voor de volgende velden met Azure AD PowerShell instellen.

* Alternatieve e-mailadres
* Mobiele telefoon
* Telefoon (werk)

Om te beginnen moet u eerst [downloaden](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)en installeren van de module Azure AD PowerShell.  Als u eenmaal geïnstalleerd hebt, kunt u de volgende stappen voor het configureren van elk veld.

#####<a name="alternate-email"></a>Alternatieve e-mailadres
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####<a name="mobile-phone"></a>Mobiele telefoon
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####<a name="office-phone"></a>Telefoon (werk)
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####<a name="reading-password-reset-data-with-powershell"></a>Lezen van het wachtwoord opnieuw instellen voor gegevens met PowerShell
U kunt de waarden voor de volgende velden met Azure AD PowerShell lezen.

* Alternatieve e-mailadres
* Mobiele telefoon
* Telefoon (werk)
* Telefoonnummer verificatie
* Verificatie-e-mailadres

Om te beginnen moet u eerst [downloaden](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)en installeren van de module Azure AD PowerShell.  Als u eenmaal geïnstalleerd hebt, kunt u de volgende stappen voor het configureren van elk veld.

#####<a name="alternate-email"></a>Alternatieve e-mailadres
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####<a name="mobile-phone"></a>Mobiele telefoon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####<a name="office-phone"></a>Telefoon (werk)
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####<a name="authentication-phone"></a>Telefoonnummer verificatie
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####<a name="authentication-email"></a>Verificatie-e-mailadres
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## <a name="links-to-password-reset-documentation"></a>Koppelingen naar wachtwoord opnieuw documentatie
Hieronder vindt u koppelingen naar alle pagina's documentatie Azure AD-wachtwoord opnieuw instellen:

* **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
* [**Hoe het werkt**](active-directory-passwords-how-it-works.md) - informatie over de zes verschillende onderdelen van de service en wat elk wordt
* [**Aan de slag**](active-directory-passwords-getting-started.md) - leren hoe u toestaan dat gebruikers hun wachtwoord cloud of op locatie wijzigen en instellen
* [**Aanpassen**](active-directory-passwords-customize.md) - informatie over het aanpassen van de look en feel en gedrag van de service aan de behoeften van uw organisatie
* [**Best practices**](active-directory-passwords-best-practices.md) - informatie over het snel en effectief beheren van wachtwoorden in uw organisatie
* [**Krijg inzicht**](active-directory-passwords-get-insights.md) - meer informatie over onze geïntegreerde rapportagemogelijkheden.
* [**Veelgestelde vragen**](active-directory-passwords-faq.md) - antwoorden vinden op veelgestelde vragen
* [**Problemen oplossen**](active-directory-passwords-troubleshoot.md) - informatie over het oplossen van problemen met de service snel



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"
