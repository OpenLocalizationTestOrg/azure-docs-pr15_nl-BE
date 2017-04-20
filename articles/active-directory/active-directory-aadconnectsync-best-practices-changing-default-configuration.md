<properties
    pageTitle="Azure AD verbinden sync: aanbevolen procedures voor het wijzigen van de standaardconfiguratie | Microsoft Azure"
    description="Bevat de aanbevolen procedures voor het wijzigen van de standaardconfiguratie van Azure AD verbinden sync."
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
    ms.date="08/22/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD verbinden sync: aanbevolen procedures voor het wijzigen van de standaardconfiguratie
Het doel van dit onderwerp wordt beschreven, ondersteunde en niet-ondersteunde wijzigingen synchroniseren Azure AD verbinden.

De configuratie die is gemaakt door Azure AD Connect werkt "as is" voor de meeste omgevingen waarin Active Directory op ruimten met Azure Active Directory synchroniseren. In sommige gevallen is het echter noodzakelijk bepaalde wijzigingen toepassen op een configuratie om te voldoen aan een bepaalde behoefte of eis.

## <a name="changes-to-the-service-account"></a>Wijzigingen in de serviceaccount
Azure AD verbinden synchronisatie wordt uitgevoerd met een service-account gemaakt met de installatiewizard. Deze serviceaccount bevat de coderingssleutels voor de database die wordt gebruikt door de synchronisatie. Het is gemaakt met een lang wachtwoord van 127 tekens en het wachtwoord is ingesteld op niet is verlopen.

- **Niet-ondersteunde** wijzigen of opnieuw instellen van het wachtwoord van de serviceaccount is. Doet de coderingssleutels vernietigt en de service is geen toegang tot de database en kan niet worden gestart.

## <a name="changes-to-the-scheduler"></a>Wijzigingen in de scheduler
Beginnen met de release van versie 1.1 (februari 2016) kunt u de [Taakplanner](active-directory-aadconnectsync-feature-scheduler.md) hebben een cyclus van synchronisatie met andere dan de standaard 30 minuten.

## <a name="changes-to-synchronization-rules"></a>Wijzigingen in de synchronisatieregels voor
De installatiewizard biedt een configuratie die moet werken voor de meest voorkomende scenario's. Als u wijzigingen aanbrengen in de configuratie wilt, moet u een ondersteunde configuratie nog steeds deze regels volgen.

- U kunt [wijzigen kenmerk stromen](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) als de standaard kenmerk directe stromen niet geschikt voor uw organisatie zijn.
- Als u [geen stroom een kenmerk](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) wilt verwijderen van alle bestaande waarden van kenmerken in Azure AD, moet u een regel maken voor dit scenario.
- [Een regel voor ongewenste synchronisatie uitschakelen](#disable-an-unwanted-sync-rule) in plaats van te verwijderen. Een verwijderde regel opnieuw wordt gemaakt tijdens een upgrade.
- [Wijzig de regel voor een kant-en-klare](#change-an-out-of-box-rule), maak een kopie van de originele regel en de out-of-box-regel uitschakelen. De regeleditor Sync wordt gevraagd en u helpt.
- Aangepaste met de regeleditor synchronisatie synchronisatieregels exporteren. De editor biedt een PowerShell-script die kunt u ze gemakkelijk opnieuw te maken in het herstellen van fouten.

>[AZURE.WARNING] De out-of-box sync regels hebben een vingerafdruk. Als u een wijziging in deze regels aanbrengt, wordt de vingerafdruk niet langer overeenkomen. U hebt problemen in de toekomst wanneer u een nieuwe versie van Azure AD verbinden van toepassing. Alleen wijzigingen aanbrengen de manier die wordt beschreven in dit artikel.

### <a name="disable-an-unwanted-sync-rule"></a>Een regel voor ongewenste synchronisatie uitschakelen
Een out-of-box sync regel niet verwijderen. De opnieuw wordt gemaakt tijdens de volgende upgrade.

In sommige gevallen is de installatiewizard een configuratie die niet voor uw topologie werkt geproduceerd. Bijvoorbeeld, als u het schema in het forest rekening met het Exchange-schema hebt uitgebreid, maar u een topologie rekening bron-forest hebt, worden vervolgens regels voor Exchange gemaakt voor de account en het bronforest. In dit geval moet u de regel synchronisatie uitschakelen voor Exchange.

![Regel uitgeschakeld sync](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

In de afbeelding, de aangetroffen een oude Exchange 2003-schema in het accountforest. Dit schema-uitbreiding is toegevoegd voordat het bronforest werd geïntroduceerd in de omgeving van Fabrikam. Geen kenmerken van de oude Exchange-implementatie worden gesynchroniseerd, zodat moet de sync-regel uitschakelen, zoals wordt weergegeven.

### <a name="change-an-out-of-box-rule"></a>Een out-of-box-regel wijzigen
Als u wijzigingen aanbrengen aan een out-of-box-regel wilt, moet u een kopie van de out-of-box-regel maken en de oorspronkelijke regel uitschakelen. Breng de wijzigingen aan de gekloonde regel. De regeleditor Sync helpt u met deze stappen. Wanneer u een out-of-box-regel opent, weergegeven in dit dialoogvenster:  
![Waarschuwing bij het vak regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Selecteer **Ja** voor het maken van een kopie van de regel. De gekloonde regel wordt vervolgens geopend.  
![Gekloonde regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Op deze regel gekloonde letterstijlen scope, join en transformaties.

## <a name="next-steps"></a>Volgende stappen

**Van overzichtsonderwerpen**

- [Azure AD verbinden sync: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
