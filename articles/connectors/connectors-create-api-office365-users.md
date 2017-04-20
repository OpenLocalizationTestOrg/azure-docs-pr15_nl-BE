<properties
    pageTitle="De gebruikers van Office 365-connector in Apps logica toevoegen | Microsoft Azure"
    description="Overzicht van Office 365-gebruikers connector met REST API parameters"
    services=""    
    documentationCenter=""     
    authors="msftman"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office-365-users-connector"></a>Aan de slag met de gebruikers van Office 365-connector

Verbinding maken met Office 365 gebruikers profielen en gebruikers zoeken. 

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2015-08-01-voorbeeld.

Met Office 365-gebruikers kunt u:

- Bouw uw bedrijf flow op basis van de gegevens die u via de Office 365-gebruikers krijgen. 
- Gebruik van acties die directe ondergeschikten, krijgt het gebruikersprofiel van een manager en nog veel meer. Deze acties een reactie krijgt en vervolgens de uitvoer beschikbaar maken voor andere acties. Bijvoorbeeld iemands directe ondergeschikten, krijgen deze informatie en een Azure SQL-database bijwerken. 

Zie als u wilt een bewerking in apps logica toevoegen, [maken een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De gebruikers van Office 365-connector heeft de volgende acties beschikbaar. Er zijn geen triggers.

| Triggers | Acties|
| --- | --- |
|Geen | <ul><li>Manager ophalen</li><li>Mijn profiel ophalen</li><li>Directe rapporten ophalen</li><li>Profiel ophalen</li><li>Zoeken naar gebruikers</li></ul>|

Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens. 


## <a name="create-a-connection-to-office-365-users"></a>Een verbinding maken met Office 365-gebruikers

Wanneer u deze connector aan uw logica apps toevoegen, moet u aanmelden bij uw account voor Office 365-gebruikers en logica apps verbinding maken met uw account.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]

Nadat u de verbinding hebt gemaakt, voert u de Office 365-gebruikers eigenschappen, zoals de id van de gebruiker. De **REST API reference** in dit onderwerp worden deze eigenschappen beschreven.

>[AZURE.TIP] Kunt u deze dezelfde verbinding met Office 365-gebruikers in andere apps logica.


## <a name="office-365-users-rest-api-reference"></a>Office 365 gebruikers REST API: naslag
Van toepassing op versie: 1.0.

### <a name="get-my-profile"></a>Mijn profiel ophalen 
Het profiel voor de huidige gebruiker opgehaald.  
```GET: /users/me``` 

Er zijn geen parameters voor deze oproep.

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|202|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


### <a name="get-user-profile"></a>Profiel ophalen 
Het profiel van een specifieke gebruiker opgehaald.  
```GET: /users/{userId}``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|gebruikers-id|tekenreeks|Ja|pad|geen|Principal of een e-gebruikersnaam|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|202|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|


### <a name="get-manager"></a>Manager ophalen 
Gebruikersprofiel van de manager van de opgegeven gebruiker opgehaald.  
```GET: /users/{userId}/manager``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|gebruikers-id|tekenreeks|Ja|pad|geen|Principal of een e-gebruikersnaam|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|202|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|



### <a name="get-direct-reports"></a>Directe rapporten ophalen 
Krijg directe ondergeschikten.  
```GET: /users/{userId}/directReports``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|gebruikers-id|tekenreeks|Ja|pad|geen|Principal of een e-gebruikersnaam|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|202|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|



### <a name="search-for-users"></a>Zoeken naar gebruikers 
Haalt de zoekresultaten van gebruikersprofielen.  
```GET: /users``` 

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|searchTerm|tekenreeks|geen|query|geen|Zoeken (van toepassing op: naam, voornaam, achternaam, e, e bijnaam en gebruiker principal-naam worden weergegeven)|

#### <a name="response"></a>Reactie

|Naam|Beschrijving|
|---|---|
|200|Bewerking is voltooid|
|202|Bewerking is voltooid|
|400|BadRequest|
|401|Niet-geautoriseerde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|De bewerking is mislukt.|



## <a name="object-definitions"></a>Objectdefinities

#### <a name="user-user-model-class"></a>Gebruiker: Model gebruikersklasse

|Naam van eigenschap | Gegevenstype |Vereist
|---|---|---|
|DisplayName|tekenreeks|geen|
|GivenName|tekenreeks|geen|
|Achternaam|tekenreeks|geen|
|E-mail|tekenreeks|geen|
|MailNickname|tekenreeks|geen|
|TelephoneNumber|tekenreeks|geen|
|AccountEnabled|Boole-waarde|geen|
|ID|tekenreeks|Ja
|UserPrincipalName|tekenreeks|geen|
|Afdeling|tekenreeks|geen|
|Functie|tekenreeks|geen|
|mobilePhone|tekenreeks|geen|


## <a name="next-steps"></a>Volgende stappen

[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

Ga terug naar de [lijst met API's](apis-list.md).

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/connectors-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/connectors-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/connectors-create-api-office365-users/contoso-aad-app-configure.PNG
