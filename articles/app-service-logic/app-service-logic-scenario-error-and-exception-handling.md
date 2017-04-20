<properties
    pageTitle="Logboekregistratie en foutafhandeling in logica Apps | Microsoft Azure"
    description="Een real-life use-case van geavanceerde fout verwerking en logboekregistratie met logica Apps weergeven"
    keywords=""
    services="logic-apps"
    authors="hedidin"
    manager="anneta"
    editor=""
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="b-hoedid"/>

# <a name="logging-and-error-handling-in-logic-apps"></a>Logboekregistratie en foutafhandeling in logica Apps

In dit artikel wordt beschreven hoe een app logica voor de afhandeling van uitzonderingen betere ondersteuning kan worden uitgebreid. Het is een real-life use-case en ons antwoord op de vraag van "Logica Apps ondersteunt uitzondering en foutafhandeling?"

>[AZURE.NOTE]De huidige versie van de functie logica Apps van Microsoft Azure App Service biedt een standaard sjabloon voor antwoorden op actie.
>Dit omvat zowel interne validatie en fout antwoorden geretourneerd van een API-app.

## <a name="overview-of-the-use-case-and-scenario"></a>Overzicht van de use-case en -scenario

Het volgende artikel is de use-case voor dit artikel.
Een bekende organisatie in de gezondheidszorg die ons voor het ontwikkelen van een Azure-oplossing die een patiënt portal wilt maken met Microsoft Dynamics CRM Online. Ze moesten afspraakrecords tussen de Dynamics CRM Online portal voor patiënten en televergaderingen verzenden.  We werden gebruiken de [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standaard voor alle patiëntrecords gevraagd.

Het project had twee belangrijke eisen:  

 -  Een methode voor records die worden verzonden vanaf de portal voor Dynamics CRM Online aanmelden
 -  Een manier om eventuele fouten in de werkstroom weer te geven


## <a name="how-we-solved-the-problem"></a>Hoe we het probleem opgelost

>[AZURE.TIP] De [Gebruikersgroep integratie](http://www.integrationusergroup.com/do-logic-apps-support-error-handling/ "Gebruikersgroep integratie")kunt u een video op een hoog niveau van het project bekijken.

We kozen [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/ "DocumentDB Azure") als opslagplaats voor de log- en foutenbestanden records (DocumentDB verwijst naar records als documenten). Aangezien de logica Apps heeft een standaardsjabloon voor alle antwoorden, hebben we zou niet voor het maken van een aangepast schema. We maken een API-app in te **voegen** en de **Query** voor fout- en logboekbestanden. We kunnen ook definiëren een schema in de API-app.  

Er is een andere behoefte records verwijderen na een bepaalde datum. DocumentDB heeft een eigenschap met de naam [Time to Live](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Time to Live") (TTL), kunnen we een **Time to Live** -waarde voor elke record of de collectie in te stellen. Dit was het niet nodig handmatig verwijderen van records in de DocumentDB.

### <a name="creation-of-the-logic-app"></a>Het maken van de logica-app

De eerste stap is de logica app maken en deze laden in de ontwerpfunctie. In dit voorbeeld gebruiken we bovenliggende en onderliggende logica apps. Gaan we ervan uit dat we het bovenliggende item al hebt gemaakt en gaat één onderliggende logica app maken.

Omdat we gaan worden de record uit de Dynamics CRM Online registratie, laten we beginnen aan de bovenkant. We moeten een verzoek-trigger te gebruiken omdat het bovenliggende logica app dit kind activeert.

> [AZURE.IMPORTANT] Als u deze zelfstudie hebt voltooid, moet u voor het maken van een database met DocumentDB en twee collecties (registratie en fouten).

### <a name="logic-app-trigger"></a>Logica app trigger

We gebruiken een trigger aanvraag zoals in het volgende voorbeeld wordt getoond.

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


### <a name="steps"></a>Werk

We moeten de bron (aanvraag) van de medische dossiers van het portal voor Dynamics CRM Online aanmelden.

1. Moeten we een nieuwe afspraakrecord ophalen van Dynamics CRM Online.
    De trigger die afkomstig zijn van CRM biedt ons de **CRM-PatentId**, **recordtype**, **Nieuw of bijgewerkt Record** (nieuwe of bijwerken van Boole-waarde), en **SalesforceId**. De **SalesforceId** kan niet null zijn, omdat deze alleen wordt gebruikt voor een update.
    De CRM-record krijgen we met behulp van de CRM- **PatientID** en het **Recordtype**.
1. Vervolgens moet onze DocumentDB API app **InsertLogEntry** bewerking toevoegen, zoals in de volgende cijfers.


#### <a name="insert-log-entry-designer-view"></a>Logboek post designer-weergave invoegen

![Logboek invoegen](./media/app-service-logic-scenario-error-and-exception-handling/lognewpatient.png)

#### <a name="insert-error-entry-designer-view"></a>Fout post designer-weergave invoegen
![Logboek invoegen](./media/app-service-logic-scenario-error-and-exception-handling/insertlogentry.png)

#### <a name="check-for-create-record-failure"></a>Controleren op mislukte record maken

![Voorwaarde](./media/app-service-logic-scenario-error-and-exception-handling/condition.png)


## <a name="logic-app-source-code"></a>Logica app broncode

>[AZURE.NOTE]  De volgende zijn slechts voorbeelden. Aangezien deze zelfstudie is gebaseerd op een implementatie die momenteel in productie, wordt de waarde van een **Bronknooppunt** eigenschappen die zijn gerelateerd aan een afspraak plannen mogelijk niet weergegeven.

### <a name="logging"></a>Logboekregistratie
De volgende logica app voorbeeld ziet u hoe logboekregistratie.

#### <a name="log-entry"></a>Logboek
Dit is de code app logica voor het invoegen van een logboek.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Logboek voor aanvraag

Dit is het log-aanvraagbericht geboekt naar de API-app.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}"
        }
    }

```


#### <a name="log-response"></a>Reactie van logboek

Dit is het logboek responsbericht van de API-app.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "\"0400fc2f-0000-0000-0000-575b3ff00000\"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Nu bekijken we de stappen voor foutafhandeling.


### <a name="error-handling"></a>Foutafhandeling

De volgende logica Apps voorbeeld ziet u hoe u implementeert foutafhandeling.

#### <a name="create-error-record"></a>Foutrecord maken

Dit is de code voor het maken van een foutrecord logica Apps.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}          
```

#### <a name="insert-error-into-documentdb--request"></a>Fout bij invoegen in DocumentDB--aanvragen

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MasterID_mp__c\":\"\",\"C_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"ONY_ID__c\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-documentdb--response"></a>Fout in DocumentDB--antwoord invoegen


``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "\"0c00eaac-0000-0000-0000-575b3fdc0000\"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\":\"DO - Degree level is DO\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MterID_mp__c\":\"\",\"Medicis_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"XXXXXXX\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>Televergaderingen fout antwoord

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="returning-the-response-back-to-the-parent-logic-app"></a>Retourneert het antwoord terug naar de bovenliggende logica app

Nadat u het antwoord hebt, kunt u deze naar de bovenliggende logica app doorgeven.

#### <a name="return-success-response-to-the-parent-logic-app"></a>Succes antwoord naar de bovenliggende logica app retourneren

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "   Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-the-parent-logic-app"></a>Fout in reactie op de bovenliggende logica app

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="documentdb-repository-and-portal"></a>Opslagplaats voor DocumentDB en -portal

Onze oplossing toegevoegd extra mogelijkheden met [DocumentDB](https://azure.microsoft.com/services/documentdb).

### <a name="error-management-portal"></a>Fout beheerportal

Het om fouten te bekijken, kunt u een MVC web app om foutrecords uit de DocumentDB weer te geven. **Lijst**, **Details**, **bewerken**en **verwijderen** zijn opgenomen in de huidige versie.

> [AZURE.NOTE]Bewerking bewerken: DocumentDB is het vervangen van het gehele document.
> De records weergegeven in de **lijst met** en **gedetailleerde** weergaven zijn slechts voorbeelden. Ze zijn niet de werkelijke patiënt afspraakrecords.

Hieronder vindt u voorbeelden van onze MVC app details gemaakt met de hierboven beschreven aanpak.

#### <a name="error-management-list"></a>Fout bij het beheer van lijst

![Lijst met fouten](./media/app-service-logic-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Fout management-detailweergave

![Foutdetails](./media/app-service-logic-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Logboek management portal

Als u wilt weergeven in de logboeken, ook wij een MVC web app.  Hieronder vindt u voorbeelden van onze MVC app details gemaakt met de hierboven beschreven aanpak.

#### <a name="sample-log-detail-view"></a>Monster logboek detailweergave

![Logboek detailweergave](./media/app-service-logic-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>API app details

#### <a name="logic-apps-exception-management-api"></a>Logica Apps uitzondering API voor beheer

Onze app open source Apps logica uitzondering management API biedt de volgende functionaliteit.

Er zijn twee controllers:

- **ErrorController** voegt een foutrecord voor een (document) in een DocumentDB-collectie.
- **LogController** Hiermee voegt een logboekrecord (document) in een DocumentDB-collectie.

> [AZURE.TIP] Beide controllers gebruiken `async Task<dynamic>` bewerkingen. U kunt bewerkingen kunnen worden omgezet tijdens runtime, zodat we het schema van de DocumentDB in de hoofdtekst van de bewerking kunt maken.

Elk document in de DocumentDB moet een unieke hebben id. We gebruiken `PatientId` en het toevoegen van een tijdstempel die is geconverteerd naar een Unix timestamp (double). We afkappen om de decimale waarde te verwijderen.

U kunt de broncode van onze controller fout API [van GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs)weergeven.

We aanroepen de API vanuit een logica app met behulp van de volgende syntaxis.

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

De expressie in het vorige codevoorbeeld wordt gecontroleerd voor de status van *Create_NewPatientRecord* **mislukt**.

## <a name="summary"></a>Samenvatting

- U kunt op eenvoudige wijze vastleggen en foutafhandeling in een app logica.
- U kunt DocumentDB gebruiken als opslagplaats voor logboek- en records (documenten).
- MVC kunt u een portal maken om te log- en foutenbestanden records weergeven.

### <a name="source-code"></a>Broncode
De broncode voor het beheer van logische Apps uitzondering toepassing API is beschikbaar in deze [bibliotheek GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logica App uitzondering Management API").


## <a name="next-steps"></a>Volgende stappen
- [Meer voorbeelden van Apps logica en scenario's weergeven](app-service-logic-examples-and-scenarios.md)
- [Meer informatie over Logic Apps controleprogramma 's](app-service-logic-monitor-your-logic-apps.md)
- [Een geautomatiseerde implementatie logica App sjabloon maken](app-service-logic-create-deploy-template.md)
