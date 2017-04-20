<properties
    pageTitle="Azure Active Directory cmdlets voor het configureren van instellingen voor groepen | Microsoft Azure"
    description="Hoe de instellingen voor de cmdlets Azure Active Directory-groepen beheren."
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
    ms.date="09/22/2016"
    ms.author="curtand"/>


# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory cmdlets voor het configureren van de instellingen

De volgende instellingen voor uniforme groepen kunnen worden geconfigureerd in de map:

1.  Categorieën: de door komma's gescheiden lijst van categorieën die u voor een groep instellen kunt. Voorbeelden zijn "Krant", "Geheim" en "Top Secret".

2.  Gebruik richtlijnen URL: een URL die gebruikers naar de gebruiksvoorwaarden voor het gebruik van Unified groepen, zoals gedefinieerd door uw organisatie. Deze URL wordt weergegeven in de gebruikersinterface waar gebruikers groepen gebruiken.

3.  Groep maken ingeschakeld: of geen, sommige of alle gebruikers mogen Unified groepen maken. Als de waarde op, kunnen alle gebruikers maken. Als de waarde op uit, kunnen geen gebruikers maken. Wanneer uitgeschakeld, kunt u ook opgeven een beveiligingsgroep waarvan de gebruikers die nog steeds zijn toegestaan voor het maken van groepen.

Deze instellingen zijn geconfigureerd met een instellingen en SettingsTemplate objecten. In eerste instantie, worden er geen voor instellingenobjecten in de directory. Dit betekent dat de map met de standaardinstellingen is geconfigureerd. Als u de standaardinstellingen wilt wijzigen, maakt u een nieuwe instellingenobject met behulp van een sjabloon instellingen. Instellingen voor sjablonen worden gedefinieerd door Microsoft.

U kunt de module met de cmdlets gebruikt voor deze bewerkingen op de [website van Microsoft Connect](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)downloaden.

## <a name="create-settings-at-the-directory-level"></a>Instellingen op het mapniveau van de maken

Deze stappen maken instellingen op het mapniveau, die voor alle Office-groepen in Active directory gelden.

1. Als u niet welke SettingTemplate moet worden gebruikt weet, retourneert deze cmdlet de lijst met sjablonen instellingen:

    `Get-MsolAllSettingTemplate`

    ![Lijst met instellingen voor sjablonen](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)

2. Een gebruik richtsnoer als URL wilt toevoegen, moet u eerst het SettingsTemplate-object waarin het richtsnoer URL gebruikswaarde; ophalen dat wil zeggen, de sjabloon Group.Unified:

    `$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`

3. Maak vervolgens een nieuwe instellingenobject op basis van die sjabloon:

    `$setting = $template.CreateSettingsObject()`

4. De waarde van de richtlijn gebruik vervolgens bijwerken:

    `$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`

5. Ten slotte, de instellingen van toepassing:

    `New-MsolSettings –SettingsObject $setting`

    ![Een richtsnoer gebruik URL toevoegen](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

Hier vindt u de instellingen die zijn gedefinieerd in de Group.Unified SettingsTemplate.

 **Instelling**                          | **Beschrijving**                                                                                             
--------------------------------------|-----------------------------------------------
 <ul><li>ClassificationList<li>Type: String<li>Standaard: ""                  | Een door komma's gescheiden lijst met waarden van geldige indeling die kan worden toegepast op groepen Unified.                
 <ul><li>EnableGroupCreation<li>Type: Boolean<li>Default: True              | De vlag die aangeeft of de centrale groep maken in Active directory is toegestaan.                               
 <ul><li>GroupCreationAllowedGroupId<li>Type: String<li>Standaard: ""         | De GUID van de beveiligingsgroep die is toegestaan voor het maken van groepen Unified zelfs wanneer EnableGroupCreation == false.
 <ul><li>UsageGuidelinesUrl<li>Type: String<li>Standaard: ""                  | Een koppeling naar de richtlijnen voor de groep.                                                                       

## <a name="read-settings-at-the-directory-level"></a>Instellingen op het mapniveau van de lezen

Deze stappen lezen op mapniveau, instellingen die voor alle Office-groepen in de map gelden.

1. Lees alle bestaande instellingen in de map:

    `Get-MsolAllSettings`

2. Lees alle instellingen voor een specifieke groep:

    `Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`

3. Specifieke instellingen, aangeduid met de GUID SettingId lezen:

    `Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

    ![Instellingen-ID GUID](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## <a name="update-settings-at-the-directory-level"></a>Update-instellingen op het mapniveau van de

Deze stappen werken instellingen op het mapniveau, die voor alle Office-groepen in Active directory gelden.

1. Het bestaande object instellingen opvragen:

    `$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

2. Haal de waarde die u wilt bijwerken:

    `$value = $Setting.GetSettingsValue()`

3. De waarde bijwerken:

    `$value["AllowToAddGuests"] = "false"`

4. De instelling van de update:

    `Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## <a name="remove-settings-at-the-directory-level"></a>Instellingen op het mapniveau van de verwijderen

Deze stap verwijdert u de instellingen op het mapniveau, die voor alle Office-groepen in Active directory gelden.

    `Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## <a name="cmdlet-syntax-reference"></a>Verwijzing naar cmdlet-syntaxis

U kunt meer Azure Active Directory PowerShell documentatie vinden op [Azure Active Directory Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260).

## <a name="settingstemplate-object-reference-groupunified-settingstemplate-object"></a>SettingsTemplate-objectverwijzing (Group.Unified SettingsTemplate-object)

- "naam": "EnableGroupCreation", "type": "System.Boolean", 'defaultValue': 'true', 'beschrijving': "Een Boole-vlag die aangeeft of de functie Unified groep maken op is."

- "naam": "GroupCreationAllowedGroupId", "type": "System.Guid", "standaardwaarde": "", "description": 'GUID van de beveiligingsgroep die is whitelisted Unified groepen maken'.

- "naam": "ClassificationList", "type": "System.String", "standaardwaarde": "", "omschrijving": "Een door komma's gescheiden lijst met waarden van geldige indeling die kan worden toegepast op groepen Unified."

- "naam": "UsageGuidelinesUrl", "type": "System.String", "standaardwaarde": "", "description": "Een koppeling naar de richtlijnen voor de groep."

naam | type | Standaardwaarde | Beschrijving
----------  | ----------  | ---------  | ----------
"EnableGroupCreation"  | "System.Boolean"  | "true"  | "Een Boole-vlag die aangeeft of de functie Unified groep maken op is."
"GroupCreationAllowedGroupId"  | "System.Guid"  | ""  | "De GUID van de beveiligingsgroep die is whitelisted Unified groepen maken."
"ClassificationList"  | "System.String"  | ""  | "Een door komma's gescheiden lijst met waarden van geldige indeling die kan worden toegepast op groepen Unified."
"UsageGuidelinesUrl"  | "System.String"  | ""  | "Een koppeling naar de richtlijnen voor de groep."

## <a name="next-steps"></a>Volgende stappen

U kunt meer Azure Active Directory PowerShell documentatie vinden op [Azure Active Directory Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260).

Aanvullende instructies van Microsoft-programmamanager Rob de Jong is beschikbaar op de [Groepen-Blog van Rob](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad).

* [Toegang tot bronnen te beheren met Azure Active Directory-groepen](active-directory-manage-groups.md)

* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
