<properties
    pageTitle="Azure synchroniseren met AD verbinding maken: met de installatiewizard op een tweede keer | Microsoft Azure"
    description="Wordt uitgelegd hoe de installatiewizard werkt voor de tweede keer dat u deze uitvoert."
    keywords="De installatiewizard Azure AD Connect kunt u de tweede keer dat u het uitvoeren van onderhoudsinstellingen configureren"
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure synchroniseren met AD verbinding maken: met de installatiewizard op een tweede keer
De eerste keer dat u de installatiewizard Azure AD verbinding uitvoert helpt het u bij het configureren van uw installatie. Als u de installatiewizard opnieuw uitvoert, biedt opties voor het onderhoud.

De installatiewizard kunt u vinden in het startmenu met de naam **Azure AD verbinden**.

![Het menu Start](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Wanneer u de installatiewizard start, ziet u een pagina met de volgende opties:

![De pagina met een lijst met extra taken.](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Als u AD FS met Azure AD Connect hebt geïnstalleerd, hebt u nog meer opties. De aanvullende opties voor AD FS worden beschreven in de [AD FS-beheer](active-directory-aadconnect-federation-management.md#ad-fs-management).

Selecteer een van de taken en klik op **volgende** om door te gaan.

> [AZURE.IMPORTANT] Terwijl u de installatiewizard geopend hebt, worden alle bewerkingen in de synchronisatie-engine geschorst. Zorg ervoor dat u de installatiewizard sluiten als u de wijzigingen in de configuratie hebt voltooid.

## <a name="view-current-configuration"></a>De huidige configuratie weergeven
Met deze optie kunt u een beknopt overzicht van de momenteel geconfigureerde opties.

![De pagina met een lijst van alle opties en hun status](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Klik op **vorige** om terug te gaan. Als u **Afsluiten**selecteert, sluit u de installatiewizard.

## <a name="customize-synchronization-options"></a>Synchronisatie-opties aanpassen
Deze optie wordt gebruikt om de sync-configuratie te wijzigen. Er is een subset van de opties uit het installatiepad van aangepaste configuratie. U ziet deze optie ook als u in eerste instantie snelle installatie gebruikt.

- [Meer mappen toevoegen](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Zie [een Connector verwijderen](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete)voor het verwijderen van een map.
- [Domein wijzigen en filteren van de organisatie-eenheid](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
- Het filter groep verwijderen.
- [Optionele functies wijzigen](active-directory-aadconnect-get-started-custom.md#optional-features).

De andere opties uit de installatie kunnen niet worden gewijzigd en zijn niet beschikbaar. Deze opties zijn:

- Wijzig het kenmerk userPrincipalName en sourceAnchor.
- Wijzig de gekoppelde methode voor objecten uit een ander forest.
- Filteren op basis van een groep inschakelen.

## <a name="refresh-directory-schema"></a>Directory-schema vernieuwen
Deze optie wordt gebruikt als u het schema hebt gewijzigd in een van uw locatie op AD DS-forests. U kunt bijvoorbeeld hebben Exchange geïnstalleerd of bijgewerkt naar een Windows Server 2012 schema met apparaatobjecten. In dit geval moet u instrueren Azure AD verbinding maken met het schema van AD DS opnieuw te lezen en bijwerken van de cache. Deze actie wordt ook de regels voor de synchronisatie opnieuw gegenereerd. Als u het Exchange-schema als voorbeeld toevoegt, worden de synchronisatie regels voor Exchange toegevoegd aan de configuratie.

Als u deze optie selecteert, worden alle mappen in uw configuratie weergegeven. U laat de standaardinstelling en vernieuwen van alle forests of hef de selectie van enkele van deze.

![De pagina met een lijst van alle mappen in de omgeving](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Staging-modus configureren
Met deze optie kunt u in- en uitschakelen van de staging-modus op de server. In [bewerkingen](active-directory-aadconnectsync-operations.md#staging-mode)vindt u meer informatie over de staging-modus en hoe deze wordt gebruikt.

De optie wordt weergegeven als staging is in- of uitgeschakeld:  
![Optie dat ook de huidige status van een staging-modus wordt weergegeven](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

Om de status te wijzigen, selecteert u deze optie en selecteren of deselecteren van het selectievakje.  
![Optie dat ook de huidige status van een staging-modus wordt weergegeven](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Aanmelden van gebruiker wijzigen
Met deze optie kunt u wijzigen van het wachtwoord gesynchroniseerd naar federation of andersom. U kan wijzigen **niet is geconfigureerd**.

Zie voor meer informatie over deze optie, [gebruiker aanmelden](active-directory-aadconnect-user-signin.md#changing-user-sign-in-method).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de configuratiemodel gebruikt door sync in [Declaratieve ingericht wat](active-directory-aadconnectsync-understanding-declarative-provisioning.md)Azure AD verbinden.

**Van overzichtsonderwerpen**

- [Azure AD verbinden sync: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
