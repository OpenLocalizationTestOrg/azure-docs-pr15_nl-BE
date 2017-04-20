<properties
    pageTitle="Problemen oplossen: Azure AD wachtwoordbeheer | Microsoft Azure"
    description="Het oplossen van veelvoorkomende stappen voor Azure AD wachtwoordbeheer, met inbegrip van opnieuw instellen, wijzigen, Write-back, registratie, en welke gegevens wilt opnemen bij zoeken naar help."
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
    ms.date="08/12/2016"
    ms.author="asteen"/>

# <a name="how-to-troubleshoot-password-management"></a>Problemen met wachtwoorden oplossen

> [AZURE.IMPORTANT] **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

Als u problemen met het wachtwoord hebt, je we helpen graag. De meeste problemen die u kunt uitvoeren in kunnen worden opgelost met een paar eenvoudige stappen die u voor meer informatie over onderstaande oplossen van problemen met de implementatie:

* [**Informatie op te nemen wanneer u hulp nodig**](#information-to-include-when-you-need-help)
* [**Problemen met configuratie wachtwoord Management in Azure Management Portal**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**Problemen met rapporten in Azure Management Portal wachtwoord Management**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**Problemen met het wachtwoord opnieuw instellen-Portal registratie van**](#troubleshoot-the-password-reset-registration-portal)
* [**Problemen met het wachtwoord opnieuw instellen Portal**](#troubleshoot-the-password-reset-portal)
* [**Problemen met het wachtwoord Write-back**](#troubleshoot-password-writeback)
  - [Wachtwoord terugschrijven naar gebeurtenislogboek foutcodes](#password-writeback-event-log-error-codes)
  - [Problemen met connectiviteit wachtwoord Write-back](#troubleshoot-password-writeback-connectivity)

Als u de onderstaande stappen voor probleemoplossing hebt geprobeerd en nog steeds actief in de problemen, kunt u een vraag posten in de [Forums van Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) of neem contact op met ondersteuning en we zullen eens uw probleem zodra we kunnen.

## <a name="information-to-include-when-you-need-help"></a>Informatie op te nemen wanneer u hulp nodig

Als u uw probleem met de onderstaande instructies niet kunt oplossen, kunt u contact opnemen met onze support engineers. Als u contact met hen, wordt u aangeraden de volgende gegevens te bevatten:

 - **Algemene beschrijving van de fout** : welke exacte foutbericht heeft de gebruiker Zie?  Als er geen foutbericht wordt weergegeven, wordt het onverwachte gedrag dat u in detail opgemerkt beschreven.
 - **Pagina** – welke pagina u op wanneer u de fout gezien waren (inclusief de URL)?
 - **Datum / tijd / Timezone** – wat is de precieze datum en tijd die u hebt gezien de fout (inclusief de tijdzone)?
 - **Code ondersteunen** – wat is de van ondersteuningscode gegenereerd wanneer de gebruiker zag de fout (dit, de fout te reproduceren en vervolgens de Code ondersteunen koppeling onder aan het scherm en de ondersteuningstechnicus sturen de GUID die het resultaat).
   - Als u op een pagina zonder een ondersteuningscode onder druk op F12 en SID en CID zoeken en deze twee resultaten naar de ondersteuningstechnicus sturen.

    ![][001]

 - **Gebruikersnaam** : Wat is de ID van de gebruiker die de fout (bijvoorbeeld gezienuser@contoso.com)?
 - **Informatie over de gebruiker** – was de federatieve gebruiker, een hash van het wachtwoord gesynchroniseerd, alleen cloud?  Heeft de gebruiker een AAD Premium of Basic AAD licentie toegewezen?
 - **Logboek voor toepassingsgebeurtenissen** – als u wachtwoord-Write-back en de fout is in uw infrastructuur op gebouwen, neem een exemplaar van het logboek voor toepassingsgebeurtenissen op de server verbinden met Azure AD zip en verzenden met uw aanvraag.

Deze gegevens helpen ons uw probleem zo snel mogelijk opgelost.


## <a name="troubleshoot-password-reset-configuration-in-the-azure-management-portal"></a>Configuratie van wachtwoord opnieuw instellen in de Portal Azure beheer oplossen
Als u een foutbericht krijgt bij het opnieuw instellen van wachtwoorden configureren, kunt u mogelijk oplossen door de onderstaande stappen voor probleemoplossing te volgen:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fout bij aanvraag</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welk foutbericht ziet een gebruiker?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Oplossing</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ik zie niet het <strong>Wachtwoord opnieuw instellen voor gebruikersbeleid </strong>gedeelte onder het tabblad <strong>configureren</strong> in de portal Azure management</p>
            </td>
            <td>
              <p>Het <strong>Wachtwoord opnieuw instellen voor gebruikersbeleid </strong>sectie is niet zichtbaar op het tabblad <strong>configureren</strong> in Azure Management Portal.</p>
            </td>
            <td>
              <p>Dit kan gebeuren als er geen een AAD Premium of Basic AAD licentie is toegewezen aan de admin deze bewerking. </p>
              <p>AAD Premium of AAD Basic-licentie toewijzen aan de beheerdersaccount in kwestie door te navigeren naar het tabblad <strong>licenties</strong> om te verhelpen dit, en probeer het opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ik zie geen van de configuratie-opties in de sectie van het <strong>Wachtwoord opnieuw instellen voor gebruikersbeleid</strong> die worden beschreven in de documentatie.</p>
            </td>
            <td>
              <p>Het <strong>Wachtwoord opnieuw instellen voor gebruikersbeleid </strong>gedeelte zichtbaar is, maar de enige vlag die wordt weergegeven onder de vlag van <strong>Gebruikers voor het opnieuw instellen van wachtwoorden is ingeschakeld</strong> is.</p>
            </td>
            <td>
              <p>De rest van de gebruikersinterface wordt weergegeven wanneer u de vlag <strong>Gebruikers ingeschakeld voor het opnieuw instellen van wachtwoorden</strong> om te schakelen <strong>Ja.</strong></p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ik zie niet de optie voor een bepaalde configuratie.</p>
            </td>
            <td>
              <p>Bijvoorbeeld, zie ik niet de optie <strong>aantal dagen voordat een gebruiker moet hun contactgegevens bevestigen</strong> als ik blader door de sectie <strong>Wachtwoord opnieuw instellen voor gebruikersbeleid</strong> (of andere voorbeelden van het probleem).</p>
            </td>
            <td>
              <p>Vele elementen van de gebruikersinterface zijn verborgen totdat ze nodig zijn. Kunt u de opties op de pagina inschakelen als u wilt zien.</p>
              <p><a href="active-directory-passwords-customize.md#password-management-behavior">Wachtwoordbeheer gedrag</a> Zie voor meer info over alle besturingselementen die beschikbaar voor u zijn.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ik zie niet de optie <strong>Schrijven terug van wachtwoorden met On-Premises</strong></p>
            </td>
            <td>
              <p>De optie <strong>Schrijven terug van wachtwoorden met On-Premises</strong> is niet zichtbaar onder het tabblad <strong>configureren</strong> in Azure Management Portal</p>
            </td>
            <td>
              <p>Deze optie is alleen zichtbaar als u hebt gedownload van Azure AD verbinden en wachtwoord Writeback geconfigureerd. Als u dit hebt gedaan, is die optie wordt weergegeven en kunt u in- of uitschakelen vanuit de cloud Write-back.</p>
              <p>Zie <a href="active-directory-passwords-getting-started.md#step-2-enable-password-writeback-in-azure-ad-connect">Wachtwoord Writeback inschakelen in Azure AD verbinding maken</a> voor meer informatie over hoe u dit doet.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-management-reports-in-the-azure-management-portal"></a>Problemen met rapporten in Azure Management Portal wachtwoord
Als er een fout optreden wanneer u de wachtwoord management rapporten, kunt u mogelijk oplossen door de onderstaande stappen voor probleemoplossing te volgen:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fout bij aanvraag</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welk foutbericht ziet een gebruiker?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Oplossing</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ik zie geen wachtwoord management rapporten</p>
            </td>
            <td>
              <p>De <strong>activiteit opnieuw instellen van wachtwoord</strong> en <strong>wachtwoord opnieuw instellen registratie activiteit</strong> rapporten zijn niet zichtbaar in het <strong>Logboek voor faxactiviteit</strong> rapporten in het tabblad <strong>rapporten</strong> .</p>
            </td>
            <td>
              <p>Dit kan gebeuren als er geen een AAD Premium of Basic AAD licentie is toegewezen aan de admin deze bewerking. </p>
              <p>AAD Premium of AAD Basic-licentie toewijzen aan de beheerdersaccount in kwestie door te navigeren naar het tabblad <strong>licenties</strong> om te verhelpen dit, en probeer het opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Registraties van de gebruiker weergeven meerdere keren</p>
            </td>
            <td>
              <p>Wanneer een gebruiker registreert het alternatieve e-mailadres, mobiele telefoon en vragen over beveiliging, ze elk worden weergegeven als afzonderlijke regels in plaats van één regel.</p>
            </td>
            <td>
              <p>Op dit moment, wanneer een gebruiker zich aanmeldt, we niet van uitgaan dat zij alles aanwezig op de registratiepagina wordt geregistreerd. Als gevolg hiervan melden wij momenteel elk stuk dat is geregistreerd als een afzonderlijke gebeurtenis.</p>
              <p>Als u wilt dat deze gegevens worden samengevoegd, kunt u downloaden van het rapport en de gegevens in een draaitabel in excel als u meer flexibiliteit wilt openen.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Problemen met de portal Registratie van wachtwoord opnieuw instellen
Als u een foutbericht krijgt bij het registreren van een gebruiker voor het wachtwoord opnieuw instellen, kunt u mogelijk oplossen door de onderstaande stappen voor probleemoplossing te volgen:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fout bij aanvraag</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welk foutbericht ziet een gebruiker?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Oplossing</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory is niet ingeschakeld voor het opnieuw instellen van wachtwoorden</p>
            </td>
            <td>
              <p>De beheerder heeft het gebruik van deze functie niet ingeschakeld.</p>
            </td>
            <td>
              <p>Overschakelen van de vlag voor <strong>Gebruikers die zijn ingeschakeld voor het wachtwoord opnieuw instellen</strong> op <strong>Ja</strong> en klik op <strong>Opslaan</strong> in het tabblad configuratie van Azure Management Portal map. U moet een Azure AD Premium of toegewezen aan het uitvoeren van deze bewerking admin Basic-licentie hebben.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Gebruiker beschikt niet over een AAD Premium of Basic AAD licentie toegewezen</p>
            </td>
            <td>
              <p>De beheerder heeft het gebruik van deze functie niet ingeschakeld.</p>
            </td>
            <td>
              <p>Azure AD Premium of Azure AD Basic-licentie toewijzen aan de gebruiker onder de tab <strong>licenties</strong> in Azure Management Portal. U moet een Azure AD Premium of toegewezen aan het uitvoeren van deze bewerking admin Basic-licentie hebben.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fout bij verwerking van aanvraag</p>
            </td>
            <td>
              <p>Gebruiker ziet een foutbericht waarin wordt aangegeven:</p>
              <p>

              </p>
              <p>Fout bij verwerking van aanvraag </p>
              <p>Wanneer u probeert een wachtwoord opnieuw instellen.</p>
            </td>
            <td>
              <p>Dit kan worden veroorzaakt door te veel problemen, maar meestal deze fout wordt veroorzaakt door ofwel een stroomstoring of configuratie probleem service die niet kan opgelost worden. </p>
              <p>Als u deze fout ziet en deze invloed is op uw bedrijf, neem contact op met de ondersteuning en wij helpen u ZSM. Zie <a href="#information-to-include-when-you-need-help">informatie op te nemen wanneer u hulp nodig</a> om te zien moet u zelf naar de ondersteuningstechnicus helpt bij een snelle oplossing.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-portal"></a>Problemen met de portal wachtwoord opnieuw instellen
Als u een foutbericht krijgt wanneer een gebruiker een wachtwoord opnieuw instelt, kunt u mogelijk oplossen door de onderstaande stappen voor probleemoplossing te volgen:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fout bij aanvraag</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welk foutbericht ziet een gebruiker?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Oplossing</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory is niet ingeschakeld voor het opnieuw instellen van wachtwoorden</p>
            </td>
            <td>
              <p>Uw account is niet ingeschakeld voor het opnieuw instellen van wachtwoorden</p>
              <p>Onze excuses, maar de beheerder om uw account voor gebruik met deze service niet ingesteld. </p>
              <p>

              </p>
              <p>Als u wilt kunnen we contact op met de beheerder in uw organisatie om uw wachtwoord opnieuw instellen voor u.</p>
            </td>
            <td>
              <p>Overschakelen van de vlag voor <strong>Gebruikers die zijn ingeschakeld voor het wachtwoord opnieuw instellen</strong> op <strong>Ja</strong> en klik op <strong>Opslaan</strong> in het tabblad configuratie van Azure Management Portal map. U moet een Azure AD Premium of toegewezen aan het uitvoeren van deze bewerking admin Basic-licentie hebben.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Gebruiker beschikt niet over een AAD Premium of Basic AAD licentie toegewezen</p>
            </td>
            <td>
              <p>Terwijl we niet admin wachtwoorden kunnen niet automatisch wordt ingesteld, kunnen we contact opnemen met de beheerder van uw organisatie voor u doen.</p>
            </td>
            <td>
              <p>Azure AD Premium of Azure AD Basic-licentie toewijzen aan de gebruiker onder de tab <strong>licenties</strong> in Azure Management Portal. U moet een Azure AD Premium of toegewezen aan het uitvoeren van deze bewerking admin Basic-licentie hebben.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Map voor opnieuw instellen van wachtwoorden is ingeschakeld, maar de gebruiker heeft een ontbrekende of ongeldige verificatie-informatie</p>
            </td>
            <td>
              <p>Uw account is niet ingeschakeld voor het opnieuw instellen van wachtwoorden</p>
              <p>Onze excuses, maar de beheerder om uw account voor gebruik met deze service niet ingesteld. </p>
              <p>

              </p>
              <p>Als u wilt kunnen we contact op met de beheerder in uw organisatie om uw wachtwoord opnieuw instellen voor u.</p>
            </td>
            <td>
              <p>Zorgen voor dat die gebruiker heeft gegevens van contactpersonen in het bestand in de map voordat u verdergaat goed gevormd. Zie <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welke gegevens wordt gebruikt door het wachtwoord opnieuw instellen</a> voor informatie over het configureren van verificatie-informatie in Active directory zodat gebruikers deze fout niet te zien.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Map voor opnieuw instellen van wachtwoorden is ingeschakeld, maar een gebruiker heeft alleen één stuk van de gegevens in het bestand als beleid is vereist twee stappen van de controle</p>
            </td>
            <td>
              <p>Uw account is niet ingeschakeld voor het opnieuw instellen van wachtwoorden</p>
              <p>Onze excuses, maar de beheerder om uw account voor gebruik met deze service niet ingesteld. </p>
              <p>

              </p>
              <p>Als u wilt kunnen we contact op met de beheerder in uw organisatie om uw wachtwoord opnieuw instellen voor u.</p>
            </td>
            <td>
              <p>Zodanig dat ten minste twee correct geconfigureerde contactmethoden (bijv. mobiele telefoon en telefoonnummer) voordat u verdergaat. Zie <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welke gegevens wordt gebruikt door het wachtwoord opnieuw instellen</a> voor informatie over het configureren van verificatie-informatie in Active directory zodat gebruikers deze fout niet te zien.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory is ingeschakeld voor het opnieuw instellen van wachtwoorden, en gebruiker correct is geconfigureerd, maar kan worden benaderd </p>
            </td>
            <td>
              <p>Oeps!  Een onverwachte fout opgetreden wij contact met u opnemen.</p>
            </td>
            <td>
              <p>Dit kan zijn veroorzaakt door een fout van de service tijdelijk of onjuist geconfigureerd contactgegevens die wij kan niet correct worden gedetecteerd. Als de gebruiker 10 seconden, opnieuw proberen wacht en de koppeling 'Neem contact op met de systeembeheerder' wordt weergegeven. Probeer opnieuw te klikken wordt terugsturen het gesprek dat een formulier per e-mail te klikken op "Neem contact op met de beheerder" naar een gebruiker, wachtwoord of globale beheerders (in die volgorde van prioriteit stuurt) vraagt een wachtwoord ingesteld voor die account worden uitgevoerd.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Gebruiker ontvangt nooit het wachtwoord opnieuw instellen van SMS of telefoongesprek</p>
            </td>
            <td>
              <p>Gebruiker klikt op ' tekst mij ' of 'Bel me' en nooit ontvangt vervolgens.</p>
            </td>
            <td>
              <p>Dit kan zijn het resultaat van een ongeldige telefoonnummer in de directory. Controleer of het telefoonnummer is in de notatie '+ xxxyyyzzzzXeeee ccc'. Voor meer informatie over het opmaken van telefoon Zie nummers voor gebruik met het opnieuw instellen van wachtwoorden <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welke gegevens door het wachtwoord opnieuw instellen wordt gebruikt</a>.</p>
              <p>Als u een extensie moet worden doorgestuurd naar de desbetreffende gebruiker, houd er rekening mee dat opnieuw instellen van wachtwoorden biedt geen ondersteuning voor uitbreidingen, zelfs als u een in de directory (ze worden verwijderd voordat de oproep wordt verzonden) opgeven. Probeer met behulp van een nummer zonder een extensie, of de extensie integreren in het telefoonnummer in uw PBX.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Gebruiker ontvangt nooit e-mail van wachtwoord opnieuw instellen</p>
            </td>
            <td>
              <p>Gebruiker klikt op "Stuur me een e-mail" en vervolgens nooit ontvangt.</p>
            </td>
            <td>
              <p>De meest voorkomende oorzaak voor dit probleem is dat het bericht is geweigerd door een filter tegen ongewenste e-mail. Controleer uw spam, ongewenste e-mail of verwijderde items, map voor het e-mailbericht.</p>
              <p>Zorg er ook voor dat u het juiste e-mailadres controleren voor het bericht... veel mensen hebben erg veel e-mailadressen en uiteindelijk het verkeerde Postvak in voor het bericht te controleren. Als geen van beide opties werkt, is het ook mogelijk dat het e-mailadres in Active directory onjuist is, controleert u of dat het e-mailadres is het juiste en probeer het opnieuw. Voor meer informatie over het opmaken van e-mailadres zien adressen voor gebruik met het opnieuw instellen van wachtwoorden <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welke gegevens door het wachtwoord opnieuw instellen wordt gebruikt</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ik heb een wachtwoordbeleid opnieuw ingesteld, maar wanneer een account admin wachtwoord opnieuw instellen gebruikt, wordt dat beleid wordt niet toegepast</p>
            </td>
            <td>
              <p>Beheerdersaccounts hun wachtwoorden opnieuw instellen Zie dezelfde opties voor opnieuw instellen van wachtwoorden, e-mail en mobiele telefoon, ongeacht welk beleid wordt ingesteld onder het <strong>Wachtwoord opnieuw instellen voor gebruikersbeleid</strong> gedeelte van het tabblad <strong>configureren</strong> is ingeschakeld.</p>
            </td>
            <td>
              <p>De opties die zijn geconfigureerd in de sectie van het <strong>Wachtwoord opnieuw instellen voor gebruikersbeleid</strong> van het tabblad <strong>configureren</strong> , gelden alleen voor eindgebruikers in uw organisatie.</p>
              <p>Microsoft beheert en besturingselementen opnieuw de Admin-wachtwoord beleid met het oog op het hoogste niveau van beveiliging</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Gebruiker geen wachtwoord te vaak in een dag opnieuw geprobeerd</p>
            </td>
            <td>
              <p>Gebruiker ziet een foutbericht waarin wordt gemeld:</p>
              <p>

              </p>
              <p>Gebruik een andere optie.</p>
              <p>U hebt geprobeerd om te controleren of uw account te vaak in de laatste 1 uur. Om veiligheidsredenen moet u wacht 24 uur voordat u het opnieuw kunt proberen. </p>
              <p>Als u wilt kunnen we contact op met de beheerder in uw organisatie om uw wachtwoord opnieuw instellen voor u.</p>
            </td>
            <td>
              <p>We implementeren een automatische bandbreedtebeperking voorkomen dat gebruikers proberen hun wachtwoorden opnieuw instellen te vaak in een korte periode. Dit probleem treedt op wanneer:</p>
              <ol class="ordered">
                <li>
Pogingen van gebruikers voor het valideren van een nummer 5 keer in een uur.<br\><br\></li>
                <li>
Pogingen van gebruikers de beveiliging vragen gate 5 keer in een uur gebruiken.<br\><br\></li>
                <li>
Pogingen van gebruikers een wachtwoord opnieuw instellen voor dezelfde gebruikersaccount 5 keer een uur.<br\><br\></li>
              </ol>
              <p>U kunt dit verhelpen, moet de gebruiker Wacht 24 uur na de laatste poging en de gebruiker zijn vervolgens zijn of haar wachtwoord opnieuw instellen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Gebruiker ziet een fout bij het valideren van zijn of haar telefoonnummer</p>
            </td>
            <td>
              <p>Wanneer u probeert om te controleren of een telefoon te gebruiken als verificatiemethode, wordt de gebruiker ziet een foutbericht waarin wordt gemeld:</p>
              <p>

              </p>
              <p>Onjuist telefoonnummer opgegeven.</p>
            </td>
            <td>
              <p>Deze fout treedt op wanneer u het telefoonnummer hebt opgegeven komt niet overeen met het telefoonnummer dat in het bestand.</p>
              <p>Controleer of dat de gebruiker het volledige telefoonnummer, met inbegrip van gebied en land code, wanneer u probeert een methode op basis van een telefoon gebruiken voor het opnieuw instellen van wachtwoorden is ingevoerd.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fout bij verwerking van aanvraag</p>
            </td>
            <td>
              <p>Gebruiker ziet een foutbericht waarin wordt aangegeven:</p>
              <p>

              </p>
              <p>Fout bij verwerking van aanvraag </p>
              <p>Wanneer u probeert een wachtwoord opnieuw instellen.</p>
            </td>
            <td>
              <p>Dit kan worden veroorzaakt door te veel problemen, maar meestal deze fout wordt veroorzaakt door ofwel een stroomstoring of configuratie probleem service die niet kan opgelost worden. </p>
              <p>Als u deze fout ziet en deze invloed is op uw bedrijf, neem contact op met de ondersteuning en wij helpen u ZSM. Zie <a href="#information-to-include-when-you-need-help">informatie op te nemen wanneer u hulp nodig</a> om te zien moet u zelf naar de ondersteuningstechnicus helpt bij een snelle oplossing.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback"></a>Problemen oplossen met wachtwoord Write-back
Als u een fout bij het inschakelen, uitschakelen of terugschrijven wachtwoord te gebruiken, kunt u mogelijk oplossen door de onderstaande stappen voor probleemoplossing te volgen:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fout bij aanvraag</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welk foutbericht ziet een gebruiker?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Oplossing</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Algemene onboarding en opstarten</p>
            </td>
            <td>
              <p>Wachtwoord opnieuw instellen-service wordt niet gestart in ruimten met 6800 fout in het gebeurtenislogboek van de toepassing van de machine Azure AD verbinden.</p>
              <p>

              </p>
              <p>Gesynchroniseerde gebruikers niet hun wachtwoorden opnieuw instellen na onboarding, federatieve of hash van het wachtwoord.</p>
            </td>
            <td>
              <p>Als wachtwoord Write-back is ingeschakeld, belt de synchronisatie-engine de Write-back-bibliotheek voor het uitvoeren van de configuratie (onboarding) door te praten met de service cloud onboarding. Eventuele fouten tijdens onboarding of tijdens het starten van het eindpunt WCF voor wachtwoord Writeback resulteren in fouten in het gebeurtenislogboek in het gebeurtenislogboek van de computer verbinden met Azure AD.</p>
              <p>Tijdens het opnieuw opstarten van de service ADSync als write-back is geconfigureerd, zal het eindpunt WCF worden opgestart. Echter, als het opstarten van het eindpunt, mislukt, we gewoon 6800 gebeurtenis registreren en laat de sync-service is gestart. De aanwezigheid van deze gebeurtenis betekent dat het wachtwoord Writeback eindpunt is niet gestart. Event log-gegevens van deze gebeurtenis (6800) en logboekvermeldingen genereren door PasswordResetService onderdeel wordt aangegeven waarom het eindpunt kan niet worden opgestart. Deze fouten logboek en probeer de Azure AD verbinding maken als u terugschrijven wachtwoord werkt nog steeds niet opnieuw te starten. Als het probleem zich blijft voordoen, probeer het uitschakelen en opnieuw inschakelen wachtwoord Write-back.</p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Wanneer een gebruiker probeert een wachtwoord opnieuw instellen of een account ontgrendelen met een wachtwoord writeback is ingeschakeld, mislukt de bewerking. Zie ook een gebeurtenis in Azure AD verbinden gebeurtenislogboek waarin: "synchronisatie-Engine heeft een fout hr = 800700CE, bericht = de bestandsnaam of extensie is te lang ' nadat de unlock-bewerking plaatsvindt.
                            </p>
            </td>
            <td>
              <p>Dit kan gebeuren als u een upgrade van oudere versies van Azure AD verbinden of DirSync had uitgevoerd. Upgraden van oudere versies van Azure AD verbinding maken met een teken 254 wachtwoord instellen voor de Azure AD Management Agent-account (nieuwere versies ingesteld wachtwoord lengte 127 tekens). Dergelijke lange wachtwoorden voor AD-Connector in- en uitvoer-bewerkingen werken, maar worden niet ondersteund door de bewerking ontgrendelen.
                            </p>
            </td>
            <td>
              <p>[De Active Directory-account vinden](active-directory-aadconnect-accounts-permissions.md#active-directory-account) voor Azure AD verbinding maken en opnieuw instellen van wachtwoorden niet meer dan 127 tekens bevatten. Open vervolgens de **Tijdsynchronisatie-Service** uit het menu Start. Ga naar de **verbindingslijnen** en de **Active Directory Connector**vinden. Selecteer deze en klik op **Eigenschappen**. Navigeer naar de pagina **referenties** en typ het nieuwe wachtwoord. Klik op **OK** om de pagina te sluiten.
                            </p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Fout bij Write-back configureren tijdens de installatie van Azure AD verbinden.</p>
            </td>
            <td>
              <p>De laatste stap van het installatieproces Azure AD verbinden ziet u een fout die aangeeft dat Writeback wachtwoord kan niet worden geconfigureerd.</p>
              <p>

              </p>
              <p>Het gebeurtenislogboek toepassing verbinding Azure AD bevat 32009 fout met de tekst ' Fout bij ophalen auth token'.</p>
            </td>
            <td>
              <p>Deze fout treedt op in de volgende twee gevallen:</p>
              <ul>
                <li class="unordered">
U hebt een onjuist wachtwoord voor de globale administrator-account opgegeven aan het begin van de installatieprocedure Azure AD verbinden opgegeven.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
U hebt geprobeerd een federatieve gebruiker gebruiken voor de globale administrator-account die aan het begin van de installatieprocedure Azure AD Connect wordt opgegeven.<br\><br\></li>
              </ul>
              <p>U kunt deze fout oplossen, zorg ervoor dat u niet bent u aan het begin van de installatieprocedure Azure AD verbinden via een federatieve account voor de globale beheerder opgegeven en of het opgegeven wachtwoord juist is.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fout bij Write-back configureren tijdens de installatie van Azure AD verbinden.</p>
            </td>
            <td>
              <p>Het gebeurtenislogboek Azure AD verbinden machine bevat een fout veroorzaakt door de PasswordResetService 32002.</p>
              <p>

              </p>
              <p>De fout wordt gelezen: "Fout bij verbinding maakt met een ServiceBus, de token provider niet kon bieden een beveiligingstoken..."</p>
              <p>

              </p>
            </td>
            <td>
              <p>De oorzaak van deze fout is het wachtwoord opnieuw instellen-service wordt uitgevoerd in uw omgeving op locatie is geen verbinding kunnen maken met de bus service-eindpunt in de cloud. Deze fout wordt doorgaans normaal gesproken veroorzaakt door een firewallregel een uitgaande verbinding met een bepaalde poort of web-adres te blokkeren.</p>
              <p>

              </p>
              <p>Controleer of dat de firewall uitgaande verbindingen voor het volgende mogelijk:</p>
              <ul>
                <li class="unordered">
Al het verkeer via TCP-443 (HTTPS)<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Uitgaande verbindingen <br\><br\></li>
              </ul>
              <p>

              </p>
              <p>Als u deze regels hebt bijgewerkt, de Azure AD verbinding maken met computer opnieuw opstarten en Writeback wachtwoord opnieuw aan het werk moet beginnen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Wachtwoord Writeback eindpunt op-prem niet bereikbaar</p>
            </td>
            <td>
              <p>Nadat u voor bepaalde tijd, federatieve of hash van het wachtwoord herstellen niet gesynchroniseerde gebruikers hun wachtwoord.</p>
            </td>
            <td>
              <p>In sommige gevallen de wachtwoord Write-back-service niet opnieuw gestart wanneer Azure AD verbinding opnieuw is gestart. In deze gevallen eerst controleren of wachtwoord Write-back is waarschijnlijk ingeschakeld op prem. U kunt dit doen met de wizard Azure AD verbinden of powershell (HowTos Zie hierboven). Als de functie lijkt te worden ingeschakeld, probeer in- of uitschakelen van de functie opnieuw via de gebruikersinterface of PowerShell. Zie ' stap 2: wachtwoord Write-back op uw computer met Directory-synchronisatie inschakelen &amp; firewallregels configureren ' in <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">het wachtwoord Write-back in-of uitschakelen</a> voor meer informatie over hoe u dit doet.</p>
              <p>

              </p>
              <p>Als dit niet werkt, probeert u volledig verwijderen en opnieuw installeren van Azure AD verbinden.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fouten over machtigingen</p>
            </td>
            <td>
              <p>Federatieve of wachtwoord hash sync'd gebruikers die proberen hun wachtwoorden Zie een fout te herstellen na het verzenden van het wachtwoord aangegeven is er een serviceprobleem.</p>
              <p>

              </p>
              <p>Daarnaast wordt tijdens bewerkingen wachtwoord opnieuw is ingesteld, er dat een fout met betrekking tot management agent in gebeurtenislogboeken op gebouwen toegang is geweigerd.</p>
            </td>
            <td>
              <p>Als u deze fouten in het logboek voor systeemgebeurtenissen, bevestigen dat de MA AD-account (die is opgegeven in de wizard op het moment van de configuratie) de benodigde machtigingen voor wachtwoord Write-back heeft.</p>
              <p>

              </p>
              <p>DUREN nadat deze toestemming wordt verleend kan tot een uur om de machtigingen voor druppelen binnen af via sdprop achtergrondtaak op de domeincontroller. </p>
              <p>Wachtwoord opnieuw instellen als u wilt werken, moet de machtiging op de security descriptor van het object user waarvan wachtwoord worden gestempeld. Totdat deze machtiging wordt weergegeven op het object user, blijven opnieuw instellen van wachtwoorden mislukt en de toegang is geweigerd.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fout bij het configureren van wachtwoord Write-back van de configuratiewizard Azure AD verbinden </p>
            </td>
            <td>
              <p>'Kan niet zoeken MA' fout in de Wizard bij het in-of uitschakelen wachtwoord Write-back</p>
            </td>
            <td>
              <p>Er is een bekende bug in de uitgebrachte versie van Azure AD verbinden die in de volgende situatie:</p>
              <ol class="ordered">
                <li>
U configureert de Azure AD verbinden voor huurder abc.com (domein gecontroleerd) met behulp van referenties. Dit resulteert in de connector met de naam 'abc.com – AAD"AAD wordt gemaakt.<br\><br\></li>
                <li>
U vervolgens wijzigen de AAD creds voor de connector (met oude sync UI) als u wilt (Let erop dat de huurder hetzelfde maar een andere domeinnaam). <br\><br\></li>
                <li>
Nu wilt u uitschakelen wachtwoord Write-back. De wizard maken de naam van de connector met de creds, als "abc.onmicrosoft.com – AAD" en doorgeven aan de cmdlet wachtwoord Write-back. Dit mislukt omdat er geen connector gemaakt met deze naam.<br\><br\></li>
              </ol>
              <p>Dit probleem is opgelost in onze meest recente versies. Als u een oudere versie hebt, is het een tijdelijke oplossing de powershell-cmdlet gebruikt om de functie in-of uitschakelen. Zie ' stap 2: wachtwoord Write-back op uw computer met Directory-synchronisatie inschakelen &amp; firewallregels configureren ' in <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">het wachtwoord Write-back in-of uitschakelen</a> voor meer informatie over hoe u dit doet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kan niet opnieuw instellen van wachtwoord voor gebruikers in speciale groepen, zoals Domeinadministrators / Enterprise Admins enz.</p>
            </td>
            <td>
              <p>Federatieve of wachtwoord hash sync'd gebruikers die deel uitmaken van een beveiligde groep en probeert in te stellen hun wachtwoorden Zie een fout na het verzenden van het wachtwoord aangegeven is er een serviceprobleem.</p>
            </td>
            <td>
              <p>Gebruikers met de juiste machtigingen in Active Directory worden beschermd met AdminSDHolder. Zie <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a> voor meer informatie. </p>
              <p>

              </p>
              <p>Dit betekent dat security descriptors voor deze objecten worden periodiek gecontroleerd zodat deze overeenkomen met de opgegeven in een AdminSDHolder en worden opnieuw ingesteld als deze verschillen. De extra machtigingen die nodig zijn voor wachtwoord Writeback daarom niet druppelen binnen voor deze gebruikers. Dit kan resulteren in het Write-back wachtwoord werkt niet voor dergelijke gebruikers. Als gevolg hiervan ondersteunen we geen wachtwoorden beheren voor gebruikers binnen deze groepen omdat het beveiligingsmodel AD worden verbroken.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Reset bewerkingen mislukt met Object kunnen niet worden gevonden.</p>
            </td>
            <td>
              <p>Federatieve of wachtwoord hash sync'd gebruikers die proberen hun wachtwoorden Zie een fout te herstellen na het verzenden van het wachtwoord aangegeven is er een serviceprobleem.</p>
              <p>

              </p>
              <p>Daarnaast wordt tijdens bewerkingen wachtwoord opnieuw instellen, er een fout in de gebeurtenislogboeken van de Azure verbinden met AD-service waarmee een fout 'Object niet gevonden' wordt aangegeven.</p>
            </td>
            <td>
              <p>Deze fout gewoonlijk geeft aan dat de synchronisatie-engine kan het gebruikersobject in de ruimte AAD connector of de gekoppelde MV of een AD-connector space-object niet vinden. </p>
              <p>

              </p>
              <p>Zorg ervoor dat de gebruiker wordt inderdaad gesynchroniseerd uit op prem met AAD via het huidige exemplaar van Azure AD verbinden om dit probleem oplossen door en controleren van de status van de objecten in de ruimten van de connector en MV. Controleer of het object AD CS connector op de MV-object via de regel 'Microsoft.InfromADUserAccountEnabled.xxx'.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Reset bewerkingen mislukt bij meerdere overeenkomsten gevonden eror</p>
            </td>
            <td>
              <p>Federatieve of wachtwoord hash sync'd gebruikers die proberen hun wachtwoorden Zie een fout te herstellen na het verzenden van het wachtwoord aangegeven is er een serviceprobleem.</p>
              <p>

              </p>
              <p>Daarnaast wordt tijdens bewerkingen wachtwoord opnieuw instellen, er een fout in de gebeurtenislogboeken van de Azure verbinden met AD-service die een "Meerdere maches gevonden" fout aangeeft.</p>
            </td>
            <td>
              <p>Dit geeft aan dat de synchronisatie-engine heeft ontdekt dat het object MV verbonden zijn aan meer dan één AD CS-objecten via de 'Microsoft.InfromADUserAccountEnabled.xxx'. Dit betekent dat de gebruiker een ingeschakelde account in meer dan één forest heeft. </p>
              <p>

              </p>
              <p>In dit scenario wordt momenteel niet ondersteund voor wachtwoord Write-back.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Wachtwoord-bewerkingen mislukken met een fout in de configuratie.</p>
            </td>
            <td>
              <p>Wachtwoord-bewerkingen mislukken met een fout in de configuratie. Het logboek voor toepassingsgebeurtenissen bevat Azure AD verbinden fout 6329 met tekst: 0x8023061f (de bewerking is mislukt omdat de Wachtwoordsynchronisatie is niet ingeschakeld op dit Management Agent).</p>
            </td>
            <td>
              <p>Dit gebeurt als de Azure verbinden met AD-configuratie wordt gewijzigd om toe te voegen&nbsp;een nieuw AD-forest (of te verwijderen en opnieuw toevoegen van een bestaand forest) <strong>nadat</strong> het wachtwoord Write-back-functie is ingeschakeld. Bewerkingen voor gebruikers in deze nieuwe forests wachtwoord mislukt. Het probleem oplossen, uitschakelen en weer inschakelen van de functie wachtwoord Write-back na voltooiing van de configuratiewijzigingen forest.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Schrijven van wachtwoorden die opnieuw zijn ingesteld door de gebruikers werkt goed, maar het schrijven van wachtwoorden door een gebruiker wordt gewijzigd of opnieuw is ingesteld voor een gebruiker door een beheerder is mislukt.</p>
            </td>
            <td>
              <p>Wanneer u probeert een wachtwoord opnieuw instellen voor een gebruiker vanaf de Portal Azure Management, ziet u een bericht met de mededeling: 'het wachtwoord opnieuw instellen-service wordt uitgevoerd in de on-premises omgeving ondersteunt geen beheerders gebruikerswachtwoorden opnieuw instellen. Voer een upgrade uit naar de meest recente versie van Azure AD verbinding maken met dit probleem oplossen."</p>
            </td>
            <td>
              <p>Dit gebeurt wanneer de versie van de synchronisatie-engine biedt geen ondersteuning voor de speciale wachtwoord Write-back-bewerking die is gebruikt. Write-back, wachtwoord wijzigen Write-back en gestart met een administrator-wachtwoord opnieuw instellen van Write-back vanuit Azure Management Portal, inclusief wachtwoord versies van Azure AD verbinding hoger dan 1.0.0419.0911 ondersteuning voor alle beheerbewerkingen voor wachtwoord opnieuw instellen.&nbsp; DirSync versies ondersteunen hoger dan 1.0.6862 alleen wachtwoord reset terugschrijven. Dit probleem op te lossen wordt ten zeerste aanbevolen dat u de meest recente versie van Azure AD verbinding maken of verbinding Azure Active Directory installeren (Zie voor meer informatie, <a href="active-directory-aadconnect">Hulpprogramma's voor integratie van Directory</a>) om dit probleem te verhelpen en wachtwoord Write-back in uw organisatie optimaal gebruiken.</p>
            </td>
          </tr>
        </tbody></table>


## <a name="password-writeback-event-log-error-codes"></a>Wachtwoord terugschrijven naar gebeurtenislogboek foutcodes
Aanbevolen bij het oplossen van problemen met het wachtwoord Write-back is dat logboek voor toepassingsgebeurtenissen op uw computer verbinden met Azure AD inspecteren. Dit gebeurtenislogboek bevat gebeurtenissen uit twee bronnen die van belang zijn voor wachtwoord Write-back. De bron PasswordResetService beschrijft bewerkingen en kwesties in verband met de werking van wachtwoord Write-back. De bron ADSync beschrijft bewerkingen en problemen met betrekking tot het instellen van wachtwoorden in uw omgeving AD.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Code</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Naam / bericht</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Bron</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Beschrijving</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>BORGTOCHT: MMS(4924) 0x80230619: "een beperking voorkomt dat het wachtwoord wordt gewijzigd naar de huidige opgegeven."</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Deze gebeurtenis treedt op wanneer u probeert de service Writeback wachtwoord een wachtwoord instellen voor uw lokale map die niet voldoet aan de wachtwoordduur, geschiedenis, complexiteit of filteren eisen van het domein.</p>
              <ul>
                <li class="unordered">
Als u een minimale wachtwoordduur, en het wachtwoord in dat venster tijd onlangs hebt gewijzigd, kunt u worden niet opnieuw het wachtwoord wijzigen totdat de opgegeven leeftijd in uw domein bereikt. Minimumleeftijd voor testdoeleinden moet worden ingesteld op 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Als u de vereisten voor wachtwoordgeschiedenis ingeschakeld hebt, dan moet u een wachtwoord dat niet is gebruikt in de laatste N keer waarbij N de laatste wachtwoord(en). Als u een wachtwoord dat is gebruikt in de laatste N keer inschakelt, vervolgens ziet u een fout in dit geval. Geschiedenis voor testdoeleinden moet worden ingesteld op 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Als u de vereisten voor wachtwoordcomplexiteit hebt, deze worden van kracht wanneer de gebruiker probeert te wijzigen of een wachtwoord opnieuw instellen.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Bewerking mislukt als er wachtwoordfilters ingeschakeld en een gebruiker selecteert een wachtwoord dat niet voldoen aan de filtercriteria, vervolgens het opnieuw instellen of wijzigen.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>Synchronisatie-Engine heeft een fout hr = 80230402, bericht = een poging een object is mislukt omdat er dubbele items met de dezelfde anker</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Deze gebeurtenis treedt op wanneer dezelfde gebruikers-id is ingeschakeld in meerdere domeinen.  Deze fout kan bijvoorbeeld optreden als u de Account of de Resource forests worden gesynchroniseerd en dezelfde gebruikers-id aanwezig en ingeschakeld in elk.  </p>
              <p>Deze fout kan ook optreden als u een niet-unieke ankerkenmerk (zoals de alias of de UPN) en twee gebruikers die hetzelfde ankerkenmerk delen.</p>
              <p>Dit probleem oplossen door ervoor te zorgen dat er geen dubbele gebruikers binnen uw domeinen en u voor elke gebruiker een unieke ankerkenmerk gebruikt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat de service op locatie gevonden voor een federatieve opnieuw instellen van een wachtwoord of wachtwoord hash sync'd gebruiker afkomstig zijn uit de cloud. Deze gebeurtenis is dat de eerste gebeurtenis in elk wachtwoord opnieuw instellen van Write-back-bewerking.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat een gebruiker een nieuw wachtwoord hebt geselecteerd tijdens het opnieuw instellen van een wachtwoord, wij vastgesteld dat dit wachtwoord wachtwoord corporate voldoet en dat wachtwoord is geschreven terug in de lokale AD-omgeving.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat een gebruiker een wachtwoord geselecteerd en dat wachtwoord is aangekomen in de omgeving van gebouwen, maar als we geprobeerd het wachtwoord instellen in de lokale AD-omgeving, is een fout opgetreden. Dit kan om verschillende redenen gebeuren:</p>
              <ul>
                <li class="unordered">
Het wachtwoord van de gebruiker niet voldoet aan de leeftijd, de geschiedenis, de complexiteit of filteren van vereisten voor het domein. Probeer het wachtwoord u lost dit op.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
De serviceaccount MA heeft niet de juiste machtigingen voor het nieuwe wachtwoord voor de desbetreffende gebruikersaccount ingesteld.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
De gebruikersaccount zich in een beveiligde groep, zoals de domein- of beheerders die set-bewerkingen voor wachtwoord is niet toegestaan.<br\><br\></li>
              </ul>
              <p>Zie <a href="#troubleshoot-password-writeback">Problemen met wachtwoord Write-back</a> te leren meer over welke andere situtions kunnen deze fout veroorzaken.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis doet zich voor als u terugschrijven wachtwoord met Azure AD verbinden inschakelen en geeft aan dat we onboarding uw organisatie naar de webservice wachtwoord Write-back gestart.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat het onboarding-proces is voltooid en dat wachtwoord Write-back mogelijkheden klaar is voor gebruik.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat de service op locatie een aanvraag voor een federatieve wachtwoord gevonden of wachtwoord hash sync'd gebruiker afkomstig zijn uit de cloud. Deze gebeurtenis is de eerste gebeurtenis in elke bewerking wachtwoord wijzigen Write-back.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat een gebruiker een nieuw wachtwoord hebt geselecteerd tijdens het wachtwoord wijzigen, wij vastgesteld dat dit wachtwoord wachtwoord corporate voldoet en dat wachtwoord is geschreven terug in de lokale AD-omgeving.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat een gebruiker een wachtwoord geselecteerd en dat wachtwoord is aangekomen in de omgeving van gebouwen, maar als we geprobeerd het wachtwoord instellen in de lokale AD-omgeving, is een fout opgetreden. Dit kan om verschillende redenen gebeuren:</p>
              <ul>
                <li class="unordered">
Het wachtwoord van de gebruiker niet voldoet aan de leeftijd, de geschiedenis, de complexiteit of filteren van vereisten voor het domein. Probeer het wachtwoord u lost dit op.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
De serviceaccount MA heeft niet de juiste machtigingen voor het nieuwe wachtwoord voor de desbetreffende gebruikersaccount ingesteld.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
De gebruikersaccount zich in een beveiligde groep, zoals de domein- of beheerders die set-bewerkingen voor wachtwoord is niet toegestaan.<br\><br\></li>
              </ul>
              <p>Zie <a href="#troubleshoot-password-writeback">Problemen met wachtwoord Write-back</a> te leren meer over wat in andere gevallen kunnen deze fout veroorzaken.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>De service op locatie aanvraag voor een federatieve opnieuw instellen van een wachtwoord of wachtwoord hash sync'd afkomstig zijn van de beheerder een gebruiker gevonden. Deze gebeurtenis is de eerste gebeurtenis in elke geïnitieerde admin wachtwoord reset Write-back-bewerking.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Admin tijdens een bewerking gestart met een admin-wachtwoord opnieuw instellen van een nieuw wachtwoord hebt geselecteerd, wij vastgesteld dat dit wachtwoord wachtwoord corporate voldoet en dat wachtwoord is geschreven terug in de lokale AD-omgeving.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>De beheerder een wachtwoord voor een gebruiker, geselecteerd en dat wachtwoord is aangekomen in de omgeving van gebouwen, maar tijdens het we het wachtwoord instellen in de lokale AD-omgeving, is een fout opgetreden. Dit kan om verschillende redenen gebeuren:</p>
              <ul>
                <li class="unordered">
Het wachtwoord van de gebruiker niet voldoet aan de leeftijd, de geschiedenis, de complexiteit of filteren van vereisten voor het domein. Probeer het wachtwoord u lost dit op.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
De serviceaccount MA heeft niet de juiste machtigingen voor het nieuwe wachtwoord voor de desbetreffende gebruikersaccount ingesteld.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
De gebruikersaccount zich in een beveiligde groep, zoals de domein- of beheerders die set-bewerkingen voor wachtwoord is niet toegestaan.<br\><br\></li>
              </ul>
              <p>Zie <a href="#troubleshoot-password-writeback">Problemen met wachtwoord Write-back</a> te leren meer over welke andere situtions kunnen deze fout veroorzaken.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis doet zich voor als u terugschrijven wachtwoord met Azure AD verbinding maken uitschakelen en geeft aan dat we offboarding uw organisatie naar de webservice wachtwoord Write-back gestart.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat het offboarding-proces is voltooid en dat wachtwoord Write-back mogelijkheden met succes is uitgeschakeld.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat het proces van offboarding is niet gelukt. Dit kan worden veroorzaakt door een machtigingsfout op de cloud of op locatie administrator-account tijdens de configuratie wordt opgegeven of omdat u probeert te gebruiken een beheerder van een globale wolk federatieve tijdens het terugschrijven wachtwoord uitschakelen. U kunt dit oplossen, Controleer uw administratieve machtigingen en dat u geen van een gebruikmaakt federatieve rekening bij het configureren van de functie wachtwoord Write-back.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat het wachtwoord Write-back-service is gestart en geschikt is voor het accepteren van aanvragen van wachtwoord management vanuit de cloud.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat het wachtwoord Write-back-service is gestopt en dat wachtwoord management verzoeken vanuit de cloud niet voltooid worden kunnen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat we met succes verificatietoken voor de globale beheerder opgegeven tijdens de installatie van Azure AD verbinden om te kunnen starten offboarding of onboarding opgehaald.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat we de wachtwoordversleutelingssleutel die wordt gebruikt voor het versleutelen van wachtwoorden uit de cloud worden verzonden naar uw omgeving op ruimten is gemaakt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft een onbekende fout opgetreden tijdens het beheer van een wachtwoord. Kijk naar de tekst van de uitzondering in de gebeurtenis voor meer informatie. Als u hebt, probeer het uitschakelen en opnieuw inschakelen van wachtwoord Write-back problemen. Als dit niet helpt, inclusief een kopie van het gebeurtenislogboek, samen met de opgegeven id voor het bijhouden van handel met voorwetenschap aan onze ondersteuningstechnicus.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>ServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat er is een fout bij verbinden met het wachtwoord van cloud-service opnieuw instellen en treedt meestal op wanneer de service op locatie is geen verbinding met de webservice wachtwoord opnieuw instellen. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat er is een fout opgetreden bij het verbinding maken met een exemplaar van de huurder bus. Dit kan gebeuren omdat u uitgaande verbindingen in uw omgeving op gebouwen blokkeren. Controleer de firewall zodat u verbindingen toestaan via TCP-443 en <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>en probeer het opnieuw. Als u nog steeds hebt, probeer het uitschakelen en opnieuw inschakelen van wachtwoord Write-back problemen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat de invoer die is doorgegeven aan onze web service-API ongeldig is. Probeer het opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat er een fout bij het decoderen van het wachtwoord dat is verzonden vanuit de cloud. Dit kan zijn vanwege de decodering sleutel niet overeen met de service cloud en uw omgeving op gebouwen. Uitschakelen om dit probleem oplossen en Writeback wachtwoord opnieuw in te schakelen in uw omgeving op gebouwen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tijdens onboarding opslaan huurder-specifieke informatie we in een configuratiebestand in uw omgeving op gebouwen. Deze gebeurtenis geeft aan dat er is een fout opgetreden bij het opslaan van dit bestand of dat wanneer de service is gestart, er een fout bij van het bestand lezen is. U kunt dit probleem oplossen, probeer wachtwoord terugschrijven van uitschakelen en opnieuw inschakelen als u wilt dat een opnieuw schrijven van dit configuratiebestand. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis AFGESCHAFT – is niet aanwezig in Azure AD verbinden, alleen al vroeg bouwt van DirSync die Write-back wordt ondersteund.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tijdens onboarding sturen we gegevens uit de cloud om het wachtwoord op de ruimten-service opnieuw instellen. Die gegevens worden vervolgens naar een bestand in het geheugen geschreven voordat ze worden verzonden naar de synchronisatieservice om deze gegevens veilig opslaan op schijf. Deze gebeurtenis geeft aan dat een probleem met het schrijven of het bijwerken van gegevens in het geheugen. U kunt dit probleem oplossen, probeer wachtwoord terugschrijven van uitschakelen en opnieuw inschakelen als u wilt dat een opnieuw schrijven van deze configuratie.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat wij een ongeldig antwoord ontvangen van de webservice wachtwoord opnieuw instellen. U kunt dit probleem oplossen, probeer het terugschrijven naar wachtwoord uitschakelen en opnieuw inschakelen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat we niet een vergunning token voor de globale administrator-account die is opgegeven tijdens de installatie van Azure AD verbinden krijgen kan. Deze fout kan worden veroorzaakt door een ongeldige gebruikersnaam of wachtwoord opgegeven voor de globale account of omdat de globale account opgegeven federated is. U kunt dit probleem oplossen door de configuratie met de juiste gebruikersnaam en wachtwoord opnieuw uitvoeren en er zorg voor de beheerder een beheerde (alleen cloud- of wachtwoord-sync zou) account.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat er is een fout opgetreden bij het genereren van het wachtwoord coderingssleutel of decoderen van een wachtwoord dat via de service cloud binnenkomt. Deze fout waarschijnlijk duidt op een probleem met uw omgeving. Bekijk de details van het gebeurtenislogboek voor meer informatie en het oplossen van dit probleem. U kunt ook uitschakelen en opnieuw inschakelen van de service wachtwoord terugschrijven naar dit probleem oplossen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat de service op locatie kan niet goed communiceren met de webservice voor wachtwoord opnieuw instellen het onboarding-proces te starten. Dit komt mogelijk door een firewallregel of het probleem een auth-token ophalen voor de huurder. U corrigeert dit door ervoor te zorgen dat u geen uitgaande verbindingen blokkeren via TCP-443 en TCP-9350 9354 of <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>en de AAD beheerdersaccount die u met behulp van ingebouwde federated is niet. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis AFGESCHAFT – is niet aanwezig in Azure AD verbinden, alleen al vroeg bouwt van DirSync die Write-back wordt ondersteund.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat de service op locatie kan niet goed communiceren met de webservice voor wachtwoord opnieuw instellen op het offboarding-proces start. Dit komt mogelijk door een firewallregel of probleem Autorisatietoken verkrijgen voor de huurder. Zorg ervoor dat u geen uitgaande verbindingen via 443 of <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/ blokkeren</a>en de AAD account u gebruikt om te offboard is niet federatieve wilt herstellen. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat we hadden om verbinding maken met een exemplaar van de huurder bus. Onder normale omstandigheden moet dit niet van belang zijn, maar als u deze gebeurtenis in veel gevallen kunt u controleren van de netwerkverbinding met de bus service, vooral als het een lange wachttijden of een verbinding met lage bandbreedte.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>De gezondheid van uw wachtwoord Write-back-service controleren, zenden we de hartslag gegevens naar onze wachtwoord webservice opnieuw instellen om de 5 minuten. Deze gebeurtenis geeft aan dat er een fout opgetreden is bij het verzenden van deze gezondheidsinformatie terug naar de cloud webservice. Deze gezondheidsinformatie omvat niet de gegevens in een OII of persoonlijke gegevens en is louter een hartslag en basisdienst statistieken zodat we informatie over de service in de cloud geven kunnen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat er een onbekende fout die door AD, Controleer het gebeurtenislogboek van Azure AD verbinding maken met server voor gebeurtenissen van de bron ADSync voor meer informatie over deze fout.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat de gebruiker die probeert te herstellen of wijzigen van een wachtwoord niet in de map op locatie gevonden is. Dit kan gebeuren wanneer de gebruiker verwijderd in lokalen is, maar niet in de cloud, of als er een probleem met synchronisatie. Controleer de logboeken van de synchronisatie als de laatste paar synchronisatie uitvoeren details voor meer informatie.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Wanneer u een wachtwoord opnieuw instellen of de aanvraag afkomstig is uit de cloud, het anker wolk is opgegeven tijdens de installatie van Azure AD verbinden wij gebruiken om te bepalen hoe die aanvraag koppelen terug naar een gebruiker in uw omgeving op gebouwen. Deze gebeurtenis geeft aan dat we twee gebruikers in de directory op ruimten met hetzelfde wolk ankerkenmerk gevonden. Controleer de logboeken van de synchronisatie als de laatste paar synchronisatie uitvoeren details voor meer informatie.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat de Management Agent-account niet de juiste machtigingen hebt op de rekening in kwestie een nieuw wachtwoord instellen. Zorg ervoor dat de account MA in het forest van de gebruiker opnieuw instellen en wijzigen wachtwoord machtigingen voor alle objecten in het forest.  Voor meer informatie over hoe u kunt doen om deze, Zie <a href="active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions">stap 4: Stel de juiste machtigingen voor Active Directory</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat we geprobeerd te herstellen of wijzigen van een wachtwoord voor een account die is uitgeschakeld in gebouwen. Schakel de account en probeer de bewerking opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Gebeurtenis geeft aan dat we geprobeerd te herstellen of wijzigen van een wachtwoord voor een account die is vergrendeld in gebouwen. Uitsluitingen kunnen optreden wanneer een gebruiker heeft geprobeerd een wijziging of bewerking te vaak in een korte periode wachtwoord opnieuw instellen. De account ontgrendelt en probeer de bewerking opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat de gebruiker een onjuist wachtwoord voor de huidige opgegeven wanneer de bewerking voor het uitvoeren van een wachtwoord wijzigen. Geef het juiste huidige wachtwoord en probeer het opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis treedt op wanneer u probeert de service Writeback wachtwoord een wachtwoord instellen voor uw lokale map die niet voldoet aan de wachtwoordduur, geschiedenis, complexiteit of filteren eisen van het domein.</p>
              <ul>
                <li class="unordered">
Als u een minimale wachtwoordduur, en het wachtwoord in dat venster tijd onlangs hebt gewijzigd, kunt u worden niet opnieuw het wachtwoord wijzigen totdat de opgegeven leeftijd in uw domein bereikt. Minimumleeftijd voor testdoeleinden moet worden ingesteld op 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Als u de vereisten voor wachtwoordgeschiedenis ingeschakeld hebt, dan moet u een wachtwoord dat niet is gebruikt in de laatste N keer waarbij N de laatste wachtwoord(en). Als u een wachtwoord dat is gebruikt in de laatste N keer inschakelt, vervolgens ziet u een fout in dit geval. Geschiedenis voor testdoeleinden moet worden ingesteld op 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Als u de vereisten voor wachtwoordcomplexiteit hebt, deze worden van kracht wanneer de gebruiker probeert te wijzigen of een wachtwoord opnieuw instellen.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Bewerking mislukt als er wachtwoordfilters ingeschakeld en een gebruiker selecteert een wachtwoord dat niet voldoen aan de filtercriteria, vervolgens het opnieuw instellen of wijzigen.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat er is een probleem schrijven een wachtwoord terug naar de map op ruimten vanwege een probleem met Active Directory. Controleren van de machine Azure AD verbinden logboek voor toepassingsgebeurtenissen voor berichten van de ADSync-service voor meer informatie over welke fout is opgetreden. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis AFGESCHAFT – is niet aanwezig in Azure AD verbinden, alleen al vroeg bouwt van DirSync die Write-back wordt ondersteund.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis AFGESCHAFT – is niet aanwezig in Azure AD verbinden, alleen al vroeg bouwt van DirSync die Write-back wordt ondersteund.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis AFGESCHAFT – is niet aanwezig in Azure AD verbinden, alleen al vroeg bouwt van DirSync die Write-back wordt ondersteund.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback-connectivity"></a>Wachtwoord Write-back-connectiviteit oplossen

Als u serviceonderbrekingen van Azure AD verbinding maken met de component wachtwoord Write-back ondervindt, zijn hier enkele snelle stappen die u ondernemen kunt om dit probleem oplossen:

 - [De Azure advertentie opnieuw verbinding maken met synchronisatieservice](#restart-the-azure-AD-Connect-sync-service)
 - [Uitschakelen en opnieuw inschakelen van de functie wachtwoord Write-back](#disable-and-re-enable-the-password-writeback-feature)
 - [Installeer de meest recente versie van Azure AD verbinden](#install-the-latest-azure-ad-connect-release)
 - [Problemen oplossen met wachtwoord Write-back](#troubleshoot-password-writeback)

In het algemeen wordt aangeraden dat u deze stappen uitvoeren in de bovenstaande volgorde om te herstellen van uw service op de snelste manier.

### <a name="restart-the-azure-ad-connect-sync-service"></a>De Azure advertentie opnieuw verbinding maken met synchronisatieservice
De Azure AD verbinding Sync-Service opnieuw te starten, kunt oplossen verbindingsproblemen of andere problemen met de service van voorbijgaande aard.

 1. Klik op **Start** op de server met **Azure AD verbinden**als beheerder.
 2. Typ **"services.msc"** in het zoekvak en druk op **Enter**.
 3. U zoekt **AD verbinding maken met Microsoft Azure** .
 4. Met de rechtermuisknop op de servicevermelding, klikt u op **opnieuw starten**en wacht tot de bewerking is voltooid.

    ![][002]

Deze stappen opnieuw tot stand brengen van de verbinding met de service cloud en oplossen van eventuele onderbrekingen die u ondervindt.  Als de Service voor de synchronisatie opnieuw het probleem niet is opgelost, raden we aan dat u wilt uitschakelen en opnieuw inschakelen van de functie wachtwoord terugschrijven naar een volgende stap.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Uitschakelen en opnieuw inschakelen van de functie wachtwoord Write-back
Uitschakelen en opnieuw inschakelen van de functie Writeback wachtwoord kunt u verbindingsproblemen oplossen.

 1. Open de **configuratiewizard Azure AD verbinden**als beheerder.
 2. Voer in het dialoogvenster **verbinding maken met Azure AD** uw **Azure AD admin algemene referenties**
 3. Geef uw **referenties voor AD-Domain Services-beheerder**in het dialoogvenster **verbinding maken met AD DS** .
 4. Klik in het dialoogvenster **een unieke identificatie van uw gebruikers** op de knop **volgende** .
 5. Schakel het selectievakje **wachtwoord terugneming** in het dialoogvenster **optionele onderdelen** .

    ![][003]

 6. Klik op **volgende** in de resterende pagina's zonder iets totdat u op de pagina **Gereed om te configureren** .
 7. Controleer of de pagina configureren de **terugneming optie wachtwoord als uitgeschakeld** en klik vervolgens op de groene knop **configureren** als uw wijzigingen wilt doorvoeren.
 8. In het dialoogvenster **voltooid** , schakelt u de optie **Nu synchroniseren** en klik vervolgens op **Voltooien** om de wizard te sluiten.
 9. Open de **configuratiewizard Azure AD verbinden**.
 10.    **Herhaal stap 2-8**, behalve zodat u **de optie wachtwoord terugschrijven controleren** op de **optionele functies** scherm naar de service opnieuw in te schakelen.

    ![][004]

Deze stappen opnieuw tot stand brengen van de verbinding met onze service cloud en oplossen van eventuele onderbrekingen die u ondervindt.

Als uitschakelen en opnieuw inschakelen van de functie wachtwoord Write-back wordt uw probleem niet is opgelost, raden we aan dat u wilt opnieuw installeren Azure AD verbinding maken met een volgende stap.

### <a name="install-the-latest-azure-ad-connect-release"></a>Installeer de meest recente versie van Azure AD verbinden
Opnieuw installeren van het pakket Azure AD verbinden wordt opgelost door eventuele problemen die van invloed is op de mogelijkheid om een verbinding naar onze cloud services of voor het beheren van wachtwoorden in de omgeving van uw lokale AD.
Het is raadzaam, u deze stap uitvoeren na de poging van de eerste twee stappen hierboven.

 1. Download de nieuwste versie van Azure AD verbinden [hier](active-directory-aadconnect.md#install-azure-ad-connect).
 2. Aangezien u Azure AD verbinden al hebt geïnstalleerd, hoeft u voor het uitvoeren van een in-place upgrade uw Azure AD verbinden als installatie wilt bijwerken naar de nieuwste versie.
 3. Het gedownloade pakket uitvoeren en volg de aanwijzingen op het scherm instructies voor het bijwerken van uw computer verbinden met Azure AD.  Geen extra handmatige stappen zijn vereist, tenzij u de out van vak sync regels hebt aangepast in dat geval moet u **back-up maken voordat u doorgaat met werken en handmatig opnieuw implementeren nadat u klaar bent**.

Deze stappen opnieuw tot stand brengen van de verbinding met onze service cloud en oplossen van eventuele onderbrekingen die u ondervindt.

Als installeren van de meest recente versie van de server Azure AD verbinding maken met uw probleem niet is opgelost, raden wij u na het installeren van de laatste synchronisatie QFE probeert terugschrijven naar wachtwoord uitschakelen en opnieuw inschakelen als laatste stap.

Als het probleem niet is opgelost, vervolgens wordt aangeraden dat u eens [Oplossen wachtwoord Write-back](#troubleshoot-password-writeback) en [Azure AD wachtwoord Veelgestelde vragen over het beheer](active-directory-passwords-faq.md) van het te zien als uw probleem kan er worden besproken.


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Koppelingen naar wachtwoord opnieuw documentatie
Hieronder vindt u koppelingen naar alle pagina's documentatie Azure AD-wachtwoord opnieuw instellen:

* **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
* [**Hoe het werkt**](active-directory-passwords-how-it-works.md) - informatie over de zes verschillende onderdelen van de service en wat elk wordt
* [**Aan de slag**](active-directory-passwords-getting-started.md) - leren hoe u toestaan dat gebruikers hun wachtwoord cloud of op locatie wijzigen en instellen
* [**Aanpassen**](active-directory-passwords-customize.md) - informatie over het aanpassen van de look en feel en gedrag van de service aan de behoeften van uw organisatie
* [**Best practices**](active-directory-passwords-best-practices.md) - informatie over het snel en effectief beheren van wachtwoorden in uw organisatie
* [**Krijg inzicht**](active-directory-passwords-get-insights.md) - meer informatie over onze geïntegreerde rapportagemogelijkheden.
* [**Veelgestelde vragen**](active-directory-passwords-faq.md) - antwoorden vinden op veelgestelde vragen
* [**Meer informatie**](active-directory-passwords-learn-more.md) - Ga diep in de technische details van de werking van de service



[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"
