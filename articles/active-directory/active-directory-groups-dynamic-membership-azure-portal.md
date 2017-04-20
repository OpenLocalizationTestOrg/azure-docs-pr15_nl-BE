
<properties
    pageTitle="Geavanceerde regels voor lidmaatschap van de groep in de voorvertoning Azure Active Directory maken met behulp van kenmerken | Microsoft Azure"
    description="Het maken van geavanceerde regels voor het opnemen van dynamische groepslidmaatschap ondersteund regel operatoren en parameters."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules-for-group-membership-in-azure-active-directory-preview"></a>Geavanceerde regels voor lidmaatschap van de groep in de voorvertoning Azure Active Directory maken met behulp van kenmerken

De Azure portal biedt u geavanceerde regels zodat complexere kenmerken gebaseerde dynamische lidmaatschappen voor Azure Active Directory (AD Azure) voorbeeld-groepen maken. [Wat is in het voorbeeld?](active-directory-preview-explainer.md) In dit artikel worden de kenmerken van de regel en de syntaxis voor het maken van deze geavanceerde regels.

## <a name="to-create-the-advanced-rule"></a>De geavanceerde regel maken

1.  Log in om de [Azure portal](https://portal.azure.com) met een account die is een algemene admin voor de map.

2.  Selecteer **meer services**, **gebruikers en groepen** in het tekstvak invoeren en selecteer vervolgens **Enter**.

  ![Gebruikersbeheer openen](./media/active-directory-groups-dynamic-membership-azure-portal/search-user-management.png)

3.  Selecteer **alle groepen**op de bladeserver **gebruikers en groepen** .

  ![De blade groepen openen](./media/active-directory-groups-dynamic-membership-azure-portal/view-groups-blade.png)

4. Selecteer de opdracht **toevoegen** op het blad **gebruikers en groepen - alle groepen** .

  ![Nieuwe groep toevoegen](./media/active-directory-groups-dynamic-membership-azure-portal/add-group-type.png)

5. Voer een naam en beschrijving voor de nieuwe groep op het blad **groep** . Selecteer een **typt u het lidmaatschap** van **Dynamische gebruiker** of **Dynamische apparaat**, of u wilt een regel maken voor gebruikers of apparaten, en selecteer vervolgens de **dynamische query toevoegen**. Zie voor de kenmerken voor apparaat regels gebruikt, [werken met kenmerken voor het maken van regels voor apparaatobjecten](#using-attributes-to-create-rules-for-device-objects).

  ![Lidmaatschap van de dynamische regel toevoegen](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)

6. Op de bladeserver **van dynamische lidmaatschapsregels** de regel invoeren in het vak **Geavanceerde dynamische lidmaatschap-regel toevoegen** , drukt u op Enter en selecteer **maken** onder aan het blad.

7. Selecteer **maken** op het blad van de **groep** om de groep te maken.

## <a name="constructing-the-body-of-an-advanced-rule"></a>De hoofdtekst van een geavanceerde regel maken

De geavanceerde regel die u voor de dynamische lidmaatschappen voor groepen maken kunt is in feite een binaire-expressie die bestaat uit drie delen en resulteert in een uitkomst waar of ONWAAR. Er zijn drie delen:

- Links-parameter
- Binaire operator
- Juiste constante

Een volledige geavanceerde regel er ongeveer zo uitziet: (leftParameter binaryOperator "RightConstant"), waarbij het openen en een sluithaakje vereist zijn voor de volledige expressie binaire, dubbele aanhalingstekens zijn vereist voor de juiste constante en de syntaxis voor de parameter links user.property. Een geavanceerde regel kan bestaan uit meer dan één binair expressies worden gescheiden door de - en, - of en - geen logische operators.

Hier volgen enkele voorbeelden van een correct geconstrueerde geavanceerde regel:

- (user.department - eq "Verkopen")- of (user.department - eq ' Marketing')
- (user.department - eq "Verkopen")- en - niet (user.jobTitle-'SDE' bevat)

Zie voor de volledige lijst van ondersteunde parameters en regel operatoren in de volgende secties. Zie voor de kenmerken voor apparaat regels gebruikt, [werken met kenmerken voor het maken van regels voor apparaatobjecten](#using-attributes-to-create-rules-for-device-objects).

De totale lengte van de tekst van de geavanceerde regel mag maximaal 2048 tekens lang zijn.

> [AZURE.NOTE]
>Tekenreeks en regex-bewerkingen zijn niet hoofdlettergevoelig. U kunt ook een Null-controles, met $null als een constante, bijvoorbeeld, user.department - eq $null uitvoeren.
Tekenreeksen met offertes ' moet worden voorafgegaan met ' teken, bijvoorbeeld user.department - eq \`'Verkoop'.

## <a name="supported-expression-rule-operators"></a>Ondersteunde expressie regel operators
De volgende tabel worden de operators voor de regel ondersteunde expressie en de syntaxis moet worden gebruikt in de hoofdtekst van de geavanceerde regel:

| Operator        | Syntaxis         |
|-----------------|----------------|
| Is niet gelijk aan      | -ne            |
| Is gelijk aan          | -eq            |
| Niet begint met | -notStartsWith |
| Begint met     | startsWith-    |
| Bevat niet    | -notContains   |
| Bevat        | -bevat      |
| Komt niet overeen met       | -notMatch      |
| Match           | -voldoen aan         |


## <a name="query-error-remediation"></a>Query fout herstellen
De volgende tabel geeft een overzicht van mogelijke fouten en hoe deze te corrigeren als ze zich voordoen

| Fout bij het parseren van query     | Fout bij gebruik       | Gecorrigeerde gebruik             |
|-----------------------|-------------------|-----------------------------|
| Fout: Kenmerk wordt niet ondersteund.                                      | (user.invalidProperty - eq "Waarde")       | (user.department - eq "waarde")<br/>Eigenschap moet overeenkomen met een van de [lijst met eigenschappen wordt ondersteund](#supported-properties).                          |
| Fout: De Operator wordt niet ondersteund op kenmerk.                       | (user.accountEnabled-waarde waar bevat)                                                                               | (user.accountEnabled - eq waar)<br/>De eigenschap is van het type boolean. De ondersteunde operators (-eq of - ne) van het type boolean in de bovenstaande lijst.                                                                                                                                   |
| : Fout Query-compilatie.                                      | (user.department - eq "Verkopen")- en (user.department - eq 'Marketing')(user.userPrincipalName-match"*@domain.ext") | (user.department - eq "Verkopen")- en (user.department - eq ' Marketing')<br/>Logische operator moet overeenkomen met een van de bovenstaande lijst van ondersteunde eigenschappen. (user.userPrincipalName-overeenkomen met ".*@domain.ext")or(user.userPrincipalName -overeenkomen met "@domain.ext$")Error in de reguliere expressie. |
| Fout: Binaire expressie is niet in de juiste indeling.                     | (user.department – eq 'Sales') (user.department - eq 'Sales') (user.department-eq 'Sales')                             | (user.accountEnabled - eq waar)- en (user.userPrincipalName-bevat"alias@domain")<br/>Query heeft meerdere fouten. Haakje () niet op de juiste plaats.                                                                                                                            |
| Fout: Er is een onbekende fout opgetreden tijdens de dynamische lidmaatschappen. | (user.accountEnabled - eq "True" en user.userPrincipalName-bevat"alias@domain")                               | (user.accountEnabled - eq waar)- en (user.userPrincipalName-bevat"alias@domain")<br/>Query heeft meerdere fouten. Haakje () niet op de juiste plaats.                                                                                                                            |

## <a name="supported-properties"></a>Ondersteunde eigenschappen
Dit zijn alle eigenschappen voor de gebruikersaccount die u in uw geavanceerde regel gebruiken kunt:

### <a name="properties-of-type-boolean"></a>Eigenschappen van het type boolean

Toegestane operators

* -eq


* -ne


| Eigenschappen     | Toegestane waarden  | Gebruik                          |
|----------------|-----------------|--------------------------------|
| accountEnabled | True, false      | user.accountEnabled - eq waar)  |
| dirSyncEnabled | True, false null | (user.dirSyncEnabled - eq waar) |

### <a name="properties-of-type-string"></a>Eigenschappen van het type string

Toegestane operators

* -eq


* -ne


* -notStartsWith


* StartsWith-


* -bevat


* -notContains


* -voldoen aan


* -notMatch

| Eigenschappen                 | Toegestane waarden                                                                                        | Gebruik                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| stad                       | Elke tekenreekswaarde of $null                                                                           | (user.city - eq "waarde")                                   |
| land                    | Elke tekenreekswaarde of $null                                                                            | (user.country - eq "waarde")                                |
| afdeling                 | Elke tekenreekswaarde of $null                                                                          | (user.department - eq "waarde")                             |
| displayName                | Een string-waarde                                                                                 | (user.displayName - eq "waarde")                            |
| facsimileTelephoneNumber   | Elke tekenreekswaarde of $null                                                                           | (user.facsimileTelephoneNumber - eq "waarde")               |
| givenName                  | Elke tekenreekswaarde of $null                                                                           | (user.givenName - eq "waarde")                              |
| Functie                   | Elke tekenreekswaarde of $null                                                                           | (user.jobTitle - eq "waarde")                               |
| e-mail                       | Elke tekenreekswaarde of $null (SMTP-adres van de gebruiker)                                                  | (user.mail - eq "waarde")                                   |
| mailNickName               | Een string-waarde (e-mailalias van de gebruiker)                                                            | (user.mailNickName - eq "waarde")                           |
| mobiel                     | Elke tekenreekswaarde of $null                                                                           | (user.mobile - eq "waarde")                                 |
| object-id                   | De GUID van het object user                                                                            | (user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies           | Geen DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |   (user.passwordPolicies - eq "DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | Elke tekenreekswaarde of $null                                                                            | (user.physicalDeliveryOfficeName - eq "waarde")             |
| Postcode                 | Elke tekenreekswaarde of $null                                                                            | (user.postalCode - eq "waarde")                             |
| preferredLanguage          | ISO 639-1-code                                                                                        | (user.preferredLanguage - eq "en-US")                      |
| sipProxyAddress            | Elke tekenreekswaarde of $null                                                                            | (user.sipProxyAddress - eq "waarde")                        |
| staat                      | Elke tekenreekswaarde of $null                                                                            | (user.state - eq "waarde")                                  |
| streetAddress              | Elke tekenreekswaarde of $null                                                                            | (user.streetAddress - eq "waarde")                          |
| Achternaam                    | Elke tekenreekswaarde of $null                                                                            | (user.surname - eq "waarde")                                |
| telephoneNumber            | Elke tekenreekswaarde of $null                                                                            | (user.telephoneNumber - eq "waarde")                        |
| usageLocation              | Landcode van twee letters                                                                           | (user.usageLocation - eq "US")                             |
| userPrincipalName          | Een string-waarde                                                                                     | (user.userPrincipalName - eq"alias@domain")               |
| userType                   | Gast lid $null                                                                                    | (user.userType - eq "Lid")                              |

### <a name="properties-of-type-string-collection"></a>Eigenschappen van het type string-collectie

Toegestane operators

* -bevat


* -notContains

| Poperties      | Toegestane waarden                        | Gebruik                                                |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails     | Een string-waarde                      | (user.otherMails-bevat"alias@domain")           |
| proxyAddresses | SMTP: alias@domain smtp:alias@domain | (user.proxyAddresses-bevat "SMTP:alias@domain") |

## <a name="extension-attributes-and-custom-attributes"></a>Kenmerken van de extensie en aangepaste kenmerken
Kenmerken van extensie en aangepaste kenmerken worden in het van dynamische lidmaatschapsregels ondersteund.

Uitbreiding kenmerken van op de lokale Server venster AD zijn gesynchroniseerd en nemen de opmaak van de 'ExtensionAttributeX', waarbij X gelijk is aan 1-15.
Een voorbeeld van een regel die gebruikmaakt van een uitbreidingskenmerk

(user.extensionAttribute15 - eq "Marketing")

Aangepaste kenmerken van zijn gesynchroniseerd op de lokale Server Windows AD of van een verbonden SaaS-applicatie en de indeling van ' user.extension_[GUID]\__ [kenmerk] ", waar is de unieke id in AAD voor de toepassing die het kenmerk in AAD gemaakt [GUID] en [kenmerk] de naam van het kenmerk, is zoals deze is gemaakt.
Een voorbeeld van een regel die gebruikmaakt van een aangepast attribuut is

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

Naam van het aangepaste kenmerk in Active directory kunt u vinden door het opvragen van een gebruiker het kenmerk Graph Verkenner en zoekt naar de naam van het kenmerk.

## <a name="direct-reports-rule"></a>Directe ondergeschikten regel
U kunt nu vullen de leden van een groep op basis van het kenmerk van de manager van een gebruiker.

**Een groep configureren als een groep "Manager"**

1. Volg stappen 1-5 in [de geavanceerde regel maken](#to-create-the-advanced-rule)en selecteer een **type lidmaatschap** van **Dynamische gebruiker**.

2. Voer op de bladeserver **van dynamische lidmaatschapsregels** de regel met de volgende syntaxis:

    Directe rapporten voor *directe rapporten voor {obectID_of_manager}*. Is een voorbeeld van een geldige regel voor directe rapporten

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    '62e19b97-8b3d-4d4a-a106-4ce66896a863' is waar de object-id van de manager. De object-ID kan worden gevonden in de Azure AD op het **tabblad profiel** van de pagina voor de gebruiker die de manager.

3. Als u deze regel opslaat, worden alle gebruikers die voldoen aan de regel deel uitmaakt als leden van de groep. Het kan enkele minuten duren voordat de groep in eerste instantie vullen.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Regels voor apparaatobjecten maken met behulp van kenmerken

U kunt ook een regel die wordt geselecteerd apparaatobjecten voor het lidmaatschap in een groep maken. De volgende kenmerken van apparaat kunnen worden gebruikt:

| Eigenschappen           | Toegestane waarden                  | Gebruik                                                |
|----------------------|---------------------------------|------------------------------------------------------|
| displayName          | een string-waarde                | (device.displayName - eq 'Rob Iphone')                 |
| deviceOSType         | een string-waarde                | (device.deviceOSType - eq "IOS")                      |
| deviceOSVersion      | een string-waarde                | (apparaat. Versie_besturing - eq "9.1")                         |
| isDirSynced          | True, false null                 | (device.isDirSynced - eq "true")                      |
| isManaged            | True, false null                 | (device.isManaged - eq "false")                       |
| isCompliant          | True, false null                 | (device.isCompliant - eq "true")                      |


## <a name="additional-information"></a>Als u meer informatie
Deze artikelen bevatten meer informatie over groepen in Azure Active Directory.

* [Zie bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Lidmaatschap van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels beheren voor gebruikers in een groep](active-directory-groups-dynamic-membership-azure-portal.md)
