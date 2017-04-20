<properties
    pageTitle="Controle van Azure Active Directory API reference | Microsoft Azure"
    description="Om te beginnen met de API Azure Active Directory controleren"
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
    ms.date="10/24/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-audit-api-reference"></a>Controle van Azure Active Directory API: naslag

In dit onderwerp maakt deel uit van een verzameling onderwerpen over Active Directory reporting API Azure.  
Azure AD reporting biedt een API waarmee u kunt toegang krijgen tot de gecontroleerde gegevens via programmacode of verwante hulpprogramma's.
Het bereik van dit onderwerp is te voorzien van informatie over de **API te controleren**.

Zie:

- [Controlelogboeken](active-directory-reporting-azure-portal.md#audit-logs) voor meer algemene informatie
- [Aan de slag met de Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md) voor meer informatie over de API van de rapportage.

Voor vragen, problemen of feedback, neem contact op met [AAD te melden](mailto:aadreportinghelp@microsoft.com).


## <a name="who-can-access-the-data"></a>Wie toegang heeft tot de gegevens?

- Gebruikers met de rol Admin voor beveiliging of beveiliging Reader

- Globale beheerders

- Elke toepassing waarvoor een machtiging voor toegang tot de API (app vergunning kan worden instellingen alleen op basis van toestemming van de algemene Admin)



## <a name="prerequisites"></a>Vereisten

Toegang tot dit rapport via de API melden, moet u hebben:

- Een [Azure Active Directory gratis of beter edition](active-directory-editions.md)

- De [voorwaarden voor toegang tot de Azure advertentie melden API](active-directory-reporting-api-prerequisites.md)voltooid. 
 

##<a name="accessing-the-api"></a>Toegang tot de API

Kunt u deze API via de [Grafiek Explorer](https://graphexplorer2.cloudapp.net) of via een programma openen met bijvoorbeeld PowerShell. Om ervoor te zorgen dat PowerShell correct te kunnen interpreteren de OData filtersyntaxis AAD Graph REST gebruikt, moet u de backtick (aka: accent grave) het teken $-teken "escape". Het teken backtick fungeert als [escape-teken van PowerShell](https://technet.microsoft.com/library/hh847755.aspx), PowerShell een letterlijke interpretatie van de $-teken en te voorkomen dat het verwarrend als PowerShell variabelenaam toe (ie: $filter).

De focus van dit onderwerp is op de grafiek Explorer. Zie voor een voorbeeld van PowerShell deze [PowerShell-script](active-directory-reporting-api-audit-samples.md#powershell-script).

 
 

## <a name="api-endpoint"></a>API-eindpunt


U kunt toegang krijgen tot deze API met behulp van de volgende URI:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Er is geen limiet op het aantal records dat wordt geretourneerd door de Azure AD audit API (met behulp van OData paginering).
Uitchecken voor behoud limieten op de rapportage van gegevens, [Rapportage bewaarbeleid](active-directory-reporting-retention.md).

Dit gesprek geeft de gegevens op in batches. Elke partij heeft een maximum van 1000 records.  
Als u de volgende batch records, gebruikt u de volgende koppeling. De skiptoken informatie vinden in de eerste set van de geretourneerde records. Het overslaan-token wordt aan het einde van het resultaat ingesteld.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Ondersteunde filters

U kunt beperken het aantal records dat wordt geretourneerd door een API aanroept in de vorm van een filter.  
Voor de API-in de bijbehorende gegevens, de volgende filters worden ondersteund:

- **$top =\<aantal records als resultaat worden gegeven\> ** - het aantal geretourneerde records beperken. Dit is geen erg efficiënte bewerking. U moet niet dit filter gebruiken als u wilt terugkeren van duizenden objecten.     
- **$filter =\<uw filterinstructie\> ** - type op te geven, op basis van ondersteunde filtervelden, de records die u belangrijk vindt



## <a name="supported-filter-fields-and-operators"></a>Ondersteunde filtervelden en operatoren

Als u wilt opgeven welk type van de records die u belangrijk vindt, kunt u een filterinstructie met een of meer van de volgende filtervelden te maken:

- [ActiviteitDatum](#activitydate) - Hiermee definieert u een datum of datumbereik
- [activityType](#activitytype) - definieert het type van een activiteit
- [activiteit](#activity) - bepaalt de activiteit als tekenreeks  
- [naam van de acteur /](#actorname) - definieert de actor in de vorm van de naam van de acteur
- [acteur/objectid](#actorobjectid) - definieert de actor in de formulier-id van de actor   
- [acteur/upn](#actorupn) - definieert de actor in de vorm van de acteur van beginsel (user Principal name) 
- [naam/naam van doel](#targetname) - het doel worden gedefinieerd in de vorm van de naam van de acteur
- [doel/objectid](#targetobjectid) - definieert het doel in de vorm van een van de doel-ID  
- [doel/upn](#targetupn) - definieert de actor in de vorm van de acteur van beginsel (user Principal name)   




----------

### <a name="activitydate"></a>activityDate

**Operatoren ondersteund**: eq, ge, le, gt, lt

**Voorbeeld**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago    

**Notities**:

datum/tijd moet worden weergegeven in UTC-notatie

----------

### <a name="activitytype"></a>activityType

**Operatoren ondersteund**: eq

**Voorbeeld**:

    $filter=activityType eq 'User'  

**Notities**:

hoofdlettergevoelig

----------

### <a name="activity"></a>activiteit

**Operatoren ondersteund**: eq, bevat, startsWith

**Voorbeeld**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')   

**Notities**:

hoofdlettergevoelig

----------

### <a name="actorname"></a>naam van de acteur /

**Operatoren ondersteund**: eq, bevat, startsWith

**Voorbeeld**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')  

**Notities**:

niet hoofdlettergevoelig

    

----------
### <a name="actorobjectid"></a>acteur/object-id

**Operatoren ondersteund**: eq

**Voorbeeld**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    

----------
### <a name="targetname"></a>Naam/doel

**Operatoren ondersteund**: eq, bevat, startsWith

**Voorbeeld**:

    $filter=targets/any(t: t/name eq 'some name')   

**Notities**:

Niet hoofdlettergevoelig

----------

### <a name="targetupn"></a>doel/upn

**Operatoren ondersteund**: eq, startsWith

**Voorbeeld**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc')) 

**Notities**:

- Niet hoofdlettergevoelig
- U moet de volledige naamruimte toevoegen bij het opvragen van Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity

----------

### <a name="targetobjectid"></a>doel/object-id

**Operatoren ondersteund**: eq

**Voorbeeld**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

----------

### <a name="actorupn"></a>acteur/upn

**Operatoren ondersteund**: eq, startsWith

**Voorbeeld**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')  

**Notities**:

- Niet hoofdlettergevoelig 
- U moet de volledige naamruimte toevoegen bij het opvragen van Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity

----------




## <a name="next-steps"></a>Volgende stappen

- Wilt u voorbeelden voor gefilterde systeemactiviteiten? Bekijk de [Azure Active Directory API monsters controleren](active-directory-reporting-api-audit-samples.md).

- Wilt u meer weten over de Azure advertentie API melden? Zie [aan de slag met de Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md).