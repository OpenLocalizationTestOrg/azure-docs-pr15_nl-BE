<properties
    pageTitle="Azure Active Directory-in activiteitenoverzicht API reference | Microsoft Azure"
    description="Verwijzing naar de API Azure Active Directory-in activiteit rapport"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory-in activiteitenoverzicht API: naslag


In dit onderwerp maakt deel uit van een verzameling onderwerpen over Active Directory reporting API Azure.  
Azure AD reporting biedt een API waarmee u toegang tot gegevens aanmelden activiteit met code of verwante hulpprogramma's.
De scope van dit onderwerp is te voorzien van informatie over het **aanmelden activiteit rapport API**.

Zie:

- [Aanmelden activiteiten](active-directory-reporting-azure-portal.md#sign-in-activities) voor meer algemene informatie
- [Aan de slag met de Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md) voor meer informatie over de API van de rapportage.

Voor vragen, problemen of feedback, neem contact op met [AAD te melden](mailto:aadreportinghelp@microsoft.com).



## <a name="who-can-access-the-api-data"></a>Wie toegang heeft tot de API-gegevens?

- Gebruikers met de rol Admin voor beveiliging of beveiliging Reader

- Globale beheerders

- Elke toepassing waarvoor een machtiging voor toegang tot de API (app vergunning kan worden instellingen alleen op basis van toestemming van de algemene Admin)



## <a name="prerequisites"></a>Vereisten

Als u dit rapport via de API voor rapportage, moet u hebben:

- Een [Azure Active Directory Premium P1 of P2 edition](active-directory-editions.md)

- De [voorwaarden voor toegang tot de Azure advertentie melden API](active-directory-reporting-api-prerequisites.md)voltooid. 


##<a name="accessing-the-api"></a>Toegang tot de API

Kunt u deze API via de [Grafiek Explorer](https://graphexplorer2.cloudapp.net) of via een programma openen met bijvoorbeeld PowerShell. Om ervoor te zorgen dat PowerShell correct te kunnen interpreteren de OData filtersyntaxis AAD Graph REST gebruikt, moet u de backtick (aka: accent grave) het teken $-teken "escape". Het teken backtick fungeert als [escape-teken van PowerShell](https://technet.microsoft.com/library/hh847755.aspx), PowerShell een letterlijke interpretatie van de $-teken en te voorkomen dat het verwarrend als PowerShell variabelenaam toe (ie: $filter).

De focus van dit onderwerp is op de grafiek Explorer. Zie voor een voorbeeld van PowerShell deze [PowerShell-script](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).


## <a name="api-endpoint"></a>API-eindpunt

U kunt toegang krijgen tot deze API met behulp van de volgende basis URI:  
    
    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Deze API is vanwege de omvang van de gegevens, een maximum van 1 miljoen records geretourneerd. 

Dit gesprek geeft de gegevens op in batches. Elke partij heeft een maximum van 1000 records.  
Als u de volgende batch records, gebruikt u de volgende koppeling. De [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) informatie vinden in de eerste set van de geretourneerde records. Het overslaan-token wordt aan het einde van het resultaat ingesteld.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Ondersteunde filters

U kunt beperken het aantal records dat wordt geretourneerd door een API aanroept in de vorm van een filter.  
Voor de API-in de bijbehorende gegevens, de volgende filters worden ondersteund:

- **$top =\<aantal records als resultaat worden gegeven\> ** - het aantal geretourneerde records beperken. Dit is geen erg efficiënte bewerking. U moet niet dit filter gebruiken als u wilt terugkeren van duizenden objecten.  
- **$filter =\<uw filterinstructie\> ** - type op te geven, op basis van ondersteunde filtervelden, de records die u belangrijk vindt



## <a name="supported-filter-fields-and-operators"></a>Ondersteunde filtervelden en operatoren

Als u wilt opgeven welk type van de records die u belangrijk vindt, kunt u een filterinstructie met een of meer van de volgende filtervelden te maken:

- [signinDateTime](#signindatetime) - Hiermee definieert u een datum of datumbereik

- [gebruikers-id](#userid) - definieert een specifieke gebruiker op basis van de gebruikersnaam.

- [userPrincipalName](#userprincipalname) - definieert een specifieke gebruiker op basis van de gebruiker UPN (user Principal name)

- [appId](#appid) - definieert een specifieke app op basis van de app-ID

- [appDisplayName](#appdisplayname) - definieert een specifieke app op basis van de weergavenaam van de app.

- [loginStatus](#loginStatus) - definieert de status van de aanmeldingen (geslaagde / mislukte)


> [AZURE.NOTE] Wanneer u de grafiek Explorer, is u hoeft te gebruiken de juiste case voor elke letter de filter-velden.


Wilt u het bereik van de geretourneerde gegevens beperken, kunt u combinaties van de filtervelden en ondersteunde filters maken. De volgende instructie wordt bijvoorbeeld de hoogste 10 records tussen juli 1e 2016 en juli 6e 2016:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


----------

### <a name="signindatetime"></a>signinDateTime

**Operatoren ondersteund**: eq, ge, le, gt, lt

**Voorbeeld**:

Met behulp van een specifieke datum

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z  



Met behulp van een datumbereik    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Notities**:

De datetime-parameter moet in de UTC-notatie 


----------

### <a name="userid"></a>gebruikers-id

**Operatoren ondersteund**: eq

**Voorbeeld**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Notities**:

De waarde van de gebruikers-id is een string-waarde



----------

### <a name="userprincipalname"></a>userPrincipalName

**Operatoren ondersteund**: eq

**Voorbeeld**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Notities**:

De waarde van userPrincipalName is een string-waarde

----------

### <a name="appid"></a>toepassings-id

**Operatoren ondersteund**: eq

**Voorbeeld**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Notities**:

De waarde van de toepassings-id is een string-waarde

----------


### <a name="appdisplayname"></a>appDisplayName

**Operatoren ondersteund**: eq

**Voorbeeld**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Notities**:

De waarde van appDisplayName is een string-waarde

----------

### <a name="loginstatus"></a>loginStatus

**Operatoren ondersteund**: eq

**Voorbeeld**:

    $filter=loginStatus+eq+'1'  


**Notities**:

Er zijn twee opties voor het loginStatus: 0 - succes, 1 - storing

----------



## <a name="next-steps"></a>Volgende stappen

- Wilt u voorbeelden voor gefilterde activiteiten aanmelden? Bekijk de [Azure Active Directory-in activiteit rapport API monsters](active-directory-reporting-api-sign-in-activity-samples.md).

- Wilt u meer weten over de Azure advertentie API melden? Zie [aan de slag met de Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md).