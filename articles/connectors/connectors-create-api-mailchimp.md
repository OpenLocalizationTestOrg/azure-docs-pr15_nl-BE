<properties
pageTitle="MailChimp | Microsoft Azure"
description="Logica apps maken met Azure App-service. MailChimp is een SaaS-service waarmee bedrijven te beheren en automatiseren van e-mail marketing activiteiten, waaronder marketingberichten, geautomatiseerde berichten en gerichte campagnes te verzenden."
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-mailchimp-connector"></a>Aan de slag met de MailChimp-connector

MailChimp is een SaaS-service waarmee bedrijven te beheren en automatiseren van e-mail marketing activiteiten, waaronder marketingberichten, geautomatiseerde berichten en gerichte campagnes te verzenden.


>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld.

U kunt aan de slag met het maken van een app logica nu, Zie [maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De MailChimp-connector kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens.

 De MailChimp-connector is de volgende actie (s) en/of trigger(s) beschikbaar:

### <a name="mailchimp-actions"></a>MailChimp-acties
U kunt deze acties ondernemen:

|Actie|Beschrijving|
|--- | ---|
|[newcampaign](connectors-create-api-mailchimp.md#newcampaign)|Een nieuwe campagne op basis van een Type campagne, campagne-instellingen (onderwerp, titel, from_name en reply_to) en lijst met geadresseerden maken|
|[newlist](connectors-create-api-mailchimp.md#newlist)|Een nieuwe lijst maken in uw MailChimp account|
|[AddMember](connectors-create-api-mailchimp.md#addmember)|Toevoegen of bijwerken van een lid van de|
|[RemoveMember](connectors-create-api-mailchimp.md#removemember)|Een lid verwijderen uit een lijst.|
|[updatemember](connectors-create-api-mailchimp.md#updatemember)|Bijwerkgegevens voor een specifiek lid|
### <a name="mailchimp-triggers"></a>MailChimp-triggers
U kunt voor deze gebeurtenis(sen) luisteren:

|Trigger | Beschrijving|
|--- | ---|
|Wanneer een lid is toegevoegd aan een lijst|Een werkstroom wordt geactiveerd wanneer een nieuw lid is toegevoegd aan een lijst|
|Wanneer u een nieuwe lijst wordt gemaakt|Een werkstroom wordt geactiveerd wanneer er een nieuwe lijst wordt gemaakt|


## <a name="create-a-connection-to-mailchimp"></a>Een verbinding maken met MailChimp
Logica apps maken met MailChimp, moet u eerst een **verbinding** maken en de informatie geven voor de volgende eigenschappen:

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|Referenties van MailChimp|

>[AZURE.INCLUDE [Steps to create a connection to MailChimp](../../includes/connectors-create-api-mailchimp.md)]

>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="reference-for-mailchimp"></a>Verwijzing voor MailChimp
Van toepassing op versie: 1.0

## <a name="newcampaign"></a>newcampaign
Nieuwe campagne: Maak een nieuwe campagne op basis van een Type campagne, lijst met geadresseerden en campagne-instellingen (onderwerp, titel, from_name en reply_to)

```POST: /campaigns```

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|newCampaignRequest| |Ja|hoofdtekst|geen|JSON-object in de hoofdtekst met parameters voor de nieuwe campagne aanvraag verzenden|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="newlist"></a>newlist
Nieuwe lijst: Een nieuwe lijst maken in uw MailChimp account

```POST: /lists```

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|newListRequest| |Ja|hoofdtekst|geen|JSON-object in de hoofdtekst met parameters voor de nieuwe campagne aanvraag verzenden|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="addmember"></a>AddMember
Lid toevoegen aan de lijst: toevoegen of bijwerken van een lid van de

```POST: /lists/{list_id}/members```

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|tekenreeks|Ja|pad|geen|De unieke id voor de lijst|
|newMemberInList| |Ja|hoofdtekst|geen|JSON-object te verzenden in de tekst met de nieuwe gegevens van lid|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="removemember"></a>RemoveMember
Lid verwijderen uit de lijst: een lid verwijderen uit een lijst.

```DELETE: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|tekenreeks|Ja|pad|geen|De unieke id voor de lijst|
|member_email|tekenreeks|Ja|pad|geen|Het e-mailadres van het lid verwijderen|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="updatemember"></a>updatemember
Bijwerken van gegevens: Update-informatie voor een lid van de specifieke

```PATCH: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|tekenreeks|Ja|pad|geen|De unieke id voor de lijst|
|member_email|tekenreeks|Ja|pad|geen|Het unieke e-mailadres van het lid bij te werken|
|updateMemberInListRequest| |Ja|hoofdtekst|geen|JSON-object in de hoofdtekst en de bijgewerkte gegevens verzenden|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="onmembersubscribed"></a>OnMemberSubscribed
Wanneer een lid is toegevoegd aan een lijst: een werkstroom wordt geactiveerd wanneer een nieuw lid is toegevoegd aan een lijst

```GET: /trigger/lists/{list_id}/members```

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|tekenreeks|Ja|pad|geen|De unieke id voor de lijst|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="oncreatelist"></a>OnCreateList
Wanneer u een nieuwe lijst wordt gemaakt: een werkstroom wordt geactiveerd wanneer er een nieuwe lijst wordt gemaakt

```GET: /trigger/lists```

Er zijn geen parameters voor deze oproep
#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldig verzoek|
|401|Niet-geautoriseerde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Er is een onbekende fout opgetreden|
|Standaard|De bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities

### <a name="newcampaignrequest"></a>NewCampaignRequest


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|type|tekenreeks|Ja |
|geadresseerden|niet gedefinieerd|Ja |
|Instellingen|niet gedefinieerd|Ja |
|variate_settings|niet gedefinieerd|Nee |
|bijhouden|niet gedefinieerd|Nee |
|rss_opts|niet gedefinieerd|Nee |
|social_card|niet gedefinieerd|Nee |



### <a name="recipient"></a>Ontvanger


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|list_id|tekenreeks|Ja |
|segment_opts|niet gedefinieerd|Nee |



### <a name="settings"></a>Instellingen


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|subject_line|tekenreeks|Ja |
|titel|tekenreeks|Nee |
|from_name|tekenreeks|Ja |
|reply_to|tekenreeks|Ja |
|use_conversation|Boole-waarde|Nee |
|to_name|tekenreeks|Nee |
|folder_id|geheel getal|Nee |
|verifiëren|Boole-waarde|Nee |
|auto_footer|Boole-waarde|Nee |
|inline_css|Boole-waarde|Nee |
|auto_tweet|Boole-waarde|Nee |
|auto_fb_post|matrix|Nee |
|fb_comments|Boole-waarde|Nee |



### <a name="variatesettings"></a>Variate_Settings


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|winner_criteria|tekenreeks|Nee |
|wachttijd|geheel getal|Nee |
|test_size|geheel getal|Nee |
|subject_lines|matrix|Nee |
|send_times|matrix|Nee |
|from_names|matrix|Nee |
|reply_to_addresses|matrix|Nee |



### <a name="tracking"></a>Bijhouden


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|wordt geopend|Boole-waarde|Nee |
|html_clicks|Boole-waarde|Nee |
|text_clicks|Boole-waarde|Nee |
|goal_tracking|Boole-waarde|Nee |
|ecomm360|Boole-waarde|Nee |
|google_analytics|tekenreeks|Nee |
|clicktale|tekenreeks|Nee |
|televergaderingen|niet gedefinieerd|Nee |
|highrise|niet gedefinieerd|Nee |
|Capsule|niet gedefinieerd|Nee |



### <a name="rssopts"></a>RSS_Opts


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|feed_url|tekenreeks|Nee |
|frequentie|tekenreeks|Nee |
|constrain_rss_img|tekenreeks|Nee |
|planning|niet gedefinieerd|Nee |



### <a name="socialcard"></a>Social_Card


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|image_url|tekenreeks|Nee |
|Beschrijving|tekenreeks|Nee |
|titel|tekenreeks|Nee |



### <a name="segmentopts"></a>Segment_Opts


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|saved_segment_id|geheel getal|Nee |
|overeenkomen met|tekenreeks|Nee |



### <a name="salesforce"></a>Televergaderingen


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|campagne|Boole-waarde|Nee |
|notities|Boole-waarde|Nee |



### <a name="highrise"></a>Highrise


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|campagne|Boole-waarde|Nee |
|notities|Boole-waarde|Nee |



### <a name="capsule"></a>Capsule


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|notities|Boole-waarde|Nee |



### <a name="schedule"></a>Planning


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|uur|geheel getal|Nee |
|daily_send|niet gedefinieerd|Nee |
|weekly_send_day|tekenreeks|Nee |
|monthly_send_date|nummer|Nee |



### <a name="dailysend"></a>Daily_Send


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|zondag|Boole-waarde|Nee |
|maandag|Boole-waarde|Nee |
|Dinsdag|Boole-waarde|Nee |
|woensdag|Boole-waarde|Nee |
|donderdag|Boole-waarde|Nee |
|vrijdag|Boole-waarde|Nee |
|zaterdag|Boole-waarde|Nee |



### <a name="campaignresponsemodel"></a>CampaignResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Nee |
|type|tekenreeks|Nee |
|create_time|tekenreeks|Nee |
|archive_url|tekenreeks|Nee |
|status|tekenreeks|Nee |
|emails_sent|geheel getal|Nee |
|send_time|tekenreeks|Nee |
|CONTENT_TYPE|tekenreeks|Nee |
|ontvanger|matrix|Nee |
|Instellingen|niet gedefinieerd|Nee |
|variate_settings|niet gedefinieerd|Nee |
|bijhouden|niet gedefinieerd|Nee |
|rss_opts|niet gedefinieerd|Nee |
|ab_split_opts|niet gedefinieerd|Nee |
|social_card|niet gedefinieerd|Nee |
|report_summary|niet gedefinieerd|Nee |
|delivery_status|niet gedefinieerd|Nee |
|_links|matrix|Nee |



### <a name="absplitopts"></a>AB_Split_Opts


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|split_test|tekenreeks|Nee |
|pick_winner|tekenreeks|Nee |
|wait_units|tekenreeks|Nee |
|wachttijd|geheel getal|Nee |
|split_size|geheel getal|Nee |
|from_name_a|tekenreeks|Nee |
|from_name_b|tekenreeks|Nee |
|reply_email_a|tekenreeks|Nee |
|reply_email_b|tekenreeks|Nee |
|subject_a|tekenreeks|Nee |
|subject_b|tekenreeks|Nee |
|send_time_a|tekenreeks|Nee |
|send_time_b|tekenreeks|Nee |
|send_time_winner|tekenreeks|Nee |



### <a name="reportsummary"></a>Report_Summary


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|wordt geopend|geheel getal|Nee |
|unique_opens|geheel getal|Nee |
|open_rate|nummer|Nee |
|muisklikken|geheel getal|Nee |
|subscriber_clicks|nummer|Nee |
|click_rate|nummer|Nee |



### <a name="deliverystatus"></a>Delivery_Status


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ingeschakeld|Boole-waarde|Nee |
|can_cancel|Boole-waarde|Nee |
|status|tekenreeks|Nee |
|emails_sent|geheel getal|Nee |
|emails_canceled|geheel getal|Nee |



### <a name="link"></a>Koppeling


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Rel|tekenreeks|Nee |
|href|tekenreeks|Nee |
|methode|tekenreeks|Nee |
|targetSchema|tekenreeks|Nee |
|schema|tekenreeks|Nee |



### <a name="newlistrequest"></a>NewListRequest


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|naam|tekenreeks|Ja |
|Neem contact op met|niet gedefinieerd|Ja |
|permission_reminder|tekenreeks|Ja |
|use_archive_bar|Boole-waarde|Nee |
|campaign_defaults|niet gedefinieerd|Ja |
|notify_on_subscribe|tekenreeks|Nee |
|notify_on_unsubscribe|tekenreeks|Nee |
|email_type_option|Boole-waarde|Ja |
|zichtbaarheid|tekenreeks|Nee |



### <a name="contact"></a>Contact


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|bedrijf|tekenreeks|Ja |
|adres1|tekenreeks|Ja |
|adres2|tekenreeks|Nee |
|stad|tekenreeks|Ja |
|staat|tekenreeks|Ja |
|ZIP|tekenreeks|Ja |
|land|tekenreeks|Ja |
|telefoon|tekenreeks|Ja |



### <a name="campaigndefaults"></a>Campaign_Defaults


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|from_name|tekenreeks|Ja |
|from_email|tekenreeks|Ja |
|Onderwerp|tekenreeks|Nee |
|taal|tekenreeks|Ja |



### <a name="createnewlistresponsemodel"></a>CreateNewListResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Ja |
|naam|tekenreeks|Ja |
|Neem contact op met|niet gedefinieerd|Ja |
|permission_reminder|tekenreeks|Ja |
|use_archive_bar|Boole-waarde|Nee |
|campaign_defaults|niet gedefinieerd|Ja |
|notify_on_subscribe|tekenreeks|Nee |
|notify_on_unsubscribe|tekenreeks|Nee |
|Date_Created|tekenreeks|Nee |
|list_rating|geheel getal|Nee |
|email_type_option|Boole-waarde|Ja |
|subscribe_url_short|tekenreeks|Nee |
|subscribe_url_long|tekenreeks|Nee |
|beamer_address|tekenreeks|Nee |
|zichtbaarheid|tekenreeks|Nee |
|modules|matrix|Nee |
|statistieken|niet gedefinieerd|Nee |
|_links|matrix|Nee |



### <a name="stats"></a>Statistieken


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|member_count|geheel getal|Nee |
|unsubscribe_count|geheel getal|Nee |
|cleaned_count|geheel getal|Nee |
|member_count_since_send|geheel getal|Nee |
|unsubscribe_count_since_send|geheel getal|Nee |
|cleaned_count_since_send|geheel getal|Nee |
|campaign_count|geheel getal|Nee |
|campaign_last_sent|geheel getal|Nee |
|merge_field_count|geheel getal|Nee |
|avg_sub_rate|nummer|Nee |
|avg_unsub_rate|nummer|Nee |
|target_sub_rate|nummer|Nee |
|open_rate|nummer|Nee |
|click_rate|nummer|Nee |
|last_sub_date|tekenreeks|Nee |
|last_unsub_date|tekenreeks|Nee |



### <a name="getlistsresponsemodel"></a>GetListsResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|lijsten|matrix|Nee |
|total_items|geheel getal|Nee |



### <a name="newmemberinlistrequest"></a>NewMemberInListRequest


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|email_type|tekenreeks|Nee |
|status|tekenreeks|Ja |
|merge_fields|niet gedefinieerd|Nee |
|belangen|tekenreeks|Nee |
|taal|tekenreeks|Nee |
|VIP|Boole-waarde|Nee |
|locatie|niet gedefinieerd|Nee |
|mailadres|tekenreeks|Ja |



### <a name="firstandlastname"></a>FirstAndLastName


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|FNAME|tekenreeks|Nee |
|LNAME|tekenreeks|Nee |



### <a name="location"></a>Locatie


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Latitude|nummer|Nee |
|lengtegraad|nummer|Nee |



### <a name="memberresponsemodel"></a>MemberResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Nee |
|mailadres|tekenreeks|Nee |
|unique_email_id|tekenreeks|Nee |
|email_type|tekenreeks|Nee |
|status|tekenreeks|Nee |
|merge_fields|niet gedefinieerd|Nee |
|belangen|tekenreeks|Nee |
|statistieken|niet gedefinieerd|Nee |
|ip_signup|tekenreeks|Nee |
|timestamp_signup|tekenreeks|Nee |
|ip_opt|tekenreeks|Nee |
|timestamp_opt|tekenreeks|Nee |
|member_rating|geheel getal|Nee |
|last_changed|tekenreeks|Nee |
|taal|tekenreeks|Nee |
|VIP|Boole-waarde|Nee |
|email_client|tekenreeks|Nee |
|locatie|niet gedefinieerd|Nee |
|last_note|niet gedefinieerd|Nee |
|list_id|tekenreeks|Nee |
|_links|matrix|Nee |



### <a name="lastnote"></a>Last_Note


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|note_id|geheel getal|Nee |
|created_at|tekenreeks|Nee |
|created_by|tekenreeks|Nee |
|Opmerking|tekenreeks|Nee |



### <a name="getallmembersresponsemodel"></a>GetAllMembersResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|leden|matrix|Nee |
|list_id|tekenreeks|Nee |
|total_items|geheel getal|Nee |



### <a name="object"></a>Object






### <a name="updatememberinlistrequest"></a>UpdateMemberInListRequest


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|mailadres|tekenreeks|Nee |
|email_type|tekenreeks|Nee |
|status|tekenreeks|Ja |
|merge_fields|niet gedefinieerd|Nee |
|belangen|tekenreeks|Nee |
|taal|tekenreeks|Nee |
|VIP|Boole-waarde|Nee |
|locatie|niet gedefinieerd|Nee |



### <a name="getmembersresponsemodel"></a>GetMembersResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|leden|matrix|Nee |
|list_id|tekenreeks|Nee |
|total_items|geheel getal|Nee |



### <a name="adduserresponsemodel"></a>AddUserResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Ja |
|mailadres|tekenreeks|Ja |
|unique_email_id|tekenreeks|Nee |
|email_type|tekenreeks|Nee |
|status|tekenreeks|Nee |
|merge_fields|niet gedefinieerd|Ja |
|belangen|tekenreeks|Nee |
|statistieken|niet gedefinieerd|Nee |
|ip_signup|tekenreeks|Nee |
|timestamp_signup|tekenreeks|Nee |
|ip_opt|tekenreeks|Nee |
|timestamp_opt|tekenreeks|Nee |
|member_rating|geheel getal|Nee |
|last_changed|tekenreeks|Nee |
|taal|tekenreeks|Nee |
|VIP|Boole-waarde|Nee |
|email_client|tekenreeks|Nee |
|locatie|niet gedefinieerd|Nee |
|last_note|niet gedefinieerd|Nee |
|list_id|tekenreeks|Nee |
|_links|matrix|Nee |


## <a name="next-steps"></a>Volgende stappen
[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md)
