<properties
   pageTitle="Resource Manager REST API's | Microsoft Azure"
   description="Een overzicht van de voorbeelden van verificatie- en gebruiksgegevens Resource Manager REST API 's"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="navale;tomfitz;"/>
   
# <a name="resource-manager-rest-apis"></a>Resource Manager REST API 's

> [AZURE.SELECTOR]
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Portal](./azure-portal/resource-group-portal.md) 
- [REST-API](resource-manager-rest-api.md)

Achter elke aanroep naar Azure Resource Manager achter elke geïmplementeerde sjabloon achter elke opslag van geconfigureerde account is een of meerdere aanroepen van de Azure bronnenbeheerder RESTful API. Dit onderwerp is gewijd aan deze API's en hoe u ze kunt bellen zonder helemaal met behulp van een SDK. Dit is met name handig als u wilt dat de volledige controle over alle aanvragen voor Azure of als de SDK voor uw voorkeurstaal niet beschikbaar is of ondersteunt geen bewerkingen dat u wilt uitvoeren.

In dit artikel niet doorloopt elke API die beschikbaar zijn in Azure, maar wordt in plaats daarvan gebruiken als een voorbeeld hoe u doorgaan en verbinding maken met deze. Als u de basisbeginselen vervolgens doorgaan en de [Azure Resource Manager REST API Reference](https://msdn.microsoft.com/library/azure/dn790568.aspx) voor gedetailleerde informatie over het gebruik van de rest van de API's te lezen.

## <a name="authentication"></a>Verificatie
Verificatie voor de ARM wordt verzorgd door Azure Active Directory (AD). De verbinding met API's moet u eerst worden geverifieerd met Azure AD een verificatietoken ontvangen die u kunt doorgeven aan elke aanvraag. Als we zijn met een beschrijving van een zuivere gesprek direct naar de REST API's, wordt ook ervan uitgegaan dat u niet wilt verifiëren met een normale gebruikersnaam wachtwoord waar een pop-van-scherm kan u om een gebruikersnaam en wachtwoord gevraagd en misschien zelfs andere verificatiemechanismen gebruikt in twee factor authentication-scenario's. We gaan daarom maken wat heet een Azure AD-toepassing en een Service Principal die wordt gebruikt voor aanmelding bij. Maar vergeet niet dat Azure AD ondersteunen verschillende procedures voor verificatie en ze allemaal op te halen die verificatietoken die we nodig hebben voor de volgende API aanvragen kunnen worden gebruikt.
Volg [Azure maken AD toepassing en het beginsel van de Service](./resource-group-create-service-principal-portal.md) voor stapsgewijze instructies.

### <a name="generating-an-access-token"></a>Een toegangstoken genereren 
Azure AD verificatie gedaan door te bellen naar Azure AD, te vinden op login.microsoftonline.com. Om te verifiëren dat u moeten de volgende informatie:

* Azure AD huurder-ID (de naam van die Azure advertentie die u gebruikt voor het aanmelden, vaak hetzelfde als uw bedrijf, maar niet noodzakelijk)
* Toepassings-ID (overgenomen tijdens de stap maken van Azure AD toepassing)
* Wachtwoord (die u hebt geselecteerd tijdens het maken van de Azure AD-toepassing)

In de volgende HTTP-aanvraag zorg "Azure AD huurder ID", 'Toepassings-ID' en 'Wachtwoord' vervangen door de juiste waarden.

**Algemene HTTP-aanvraag:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... wordt (als de verificatie slaagt) resulteren in een vergelijkbaar antwoord hierop:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(De access_token in de bovenstaande reactie zijn ingekort om de leesbaarheid te vergroten)

**Met behulp van Bash toegangstoken genereren:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Met PowerShell toegangstoken genereren:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Het antwoord bevat een Access Token, informatie over hoe lang deze token geldig is en over welke resource kunt u het token voor.
Het toegangstoken dat u in de vorige HTTP-oproep ontvangen moet worden doorgegeven voor alle aanvragen voor de ARM API als een header met de naam "Vergunning" met de waarde "YOUR_ACCESS_TOKEN aan toonder". Let op de spatie tussen 'Aan toonder' en uw Access Token.

Zoals u van de bovenstaande HTTP-resultaat zien kunt, is het token is geldig voor een bepaalde periode gedurende welke u cache kunnen opslaan en opnieuw gebruiken die dezelfde token. Zelfs als het verifiëren van Azure AD voor elke API-aanroep mogelijk is, zou het zeer inefficiënt zijn.

## <a name="calling-arm-rest-apis"></a>API's aanroepen ARM REST

[Azure Resource Manager REST API's zijn hier gedocumenteerd](https://msdn.microsoft.com/library/azure/dn790568.aspx) en's buiten het bereik van deze zelfstudie voor het gebruik van elk en elk document. Deze documentatie wordt alleen gebruikt voor enkele API's om uit te leggen het basisgebruik van de API's voor en na die wij u naar de officiële documentatie verwijzen.

### <a name="list-all-subscriptions"></a>Overzicht van alle abonnementen

Een van de meest eenvoudige bewerkingen die u kunt doen is om de beschikbare abonnementen die voor u toegankelijk. In de onderstaande aanvraag kunt u zien hoe de Access Token wordt doorgegeven als een kop.

(Vervang YOUR_ACCESS_TOKEN door uw werkelijke Access Token.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... en daardoor krijgt u een overzicht van de abonnementen die deze Service Principal is toegang tot

(Abonnement-id's hieronder zijn ingekort voor leesbaarheid)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Lijst van alle resourcegroepen in een bepaald abonnement

Alle bronnen die beschikbaar zijn met de ARM API's worden genest in een resourcegroep. We gaan query ARM voor bestaande resourcegroepen in onze abonnement met behulp van de onder het HTTP GET verzoek. U ziet hoe de abonnement-ID wordt doorgegeven als onderdeel van de URL van deze tijd.

(YOUR_ACCESS_TOKEN en SUBSCRIPTION_ID vervangen door de werkelijke Access Token en de abonnement-ID)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Het antwoord dat u krijgt is afhankelijk van of u gedefinieerd bronnengroepen hebt en zo ja, hoeveel.

(Abonnement-id's hieronder zijn ingekort voor leesbaarheid)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Tot nu toe we hebt alleen zijn bij het controleren van de ARM-API's voor meer informatie, is het tijd dat we in plaats daarvan sommige bronnen maken en laten we beginnen met de eenvoudigste van allemaal, een resourcegroep. De volgende HTTP-aanvraag wordt een nieuwe groep gemaakt in een regio/locatie van uw keuze en worden een of meer codes aan toegevoegd (het voorbeeld onder slechts wordt een code toegevoegd).

(YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME vervangen door de werkelijke Access Token, abonnement-ID en de naam van de resourcegroep die u wilt maken)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Als dit lukt, krijgt u een soortgelijke reactie op dit

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

U hebt een groep gemaakt in Azure. Gefeliciteerd!

### <a name="deploy-resources-to-a-resource-group-using-an-arm-template"></a>Bronnen voor een bronnengroep met een sjabloon van ARM implementeren

Met ARM, kunt u resources met behulp van sjablonen ARM implementeren. Een ARM sjabloon definieert verschillende bronnen en bijbehorende afhankelijkheden. Voor deze sectie nemen we wordt alleen aan u bekend bent met de ARM sjablonen en we net leert u hoe u de API-aanroep start van de implementatie van een. Hier kunt u een gedetailleerde documentatie van ARM sjablonen vinden.

Implementatie van een sjabloon ARM verschillen niet veel hoe u andere API's aanroepen. Een belangrijk aspect is dat implementatie van een sjabloon kan behoorlijk lang duren, afhankelijk van wat in de sjabloon is, en de API-aanroep alleen retourneert en is het aan u als ontwikkelaar opvragen van de status van de distributie om te weten wanneer de installatie is voltooid.

In dit voorbeeld gebruiken we een vrij toegankelijke ARM sjabloon beschikbaar op [GitHub](https://github.com/Azure/azure-quickstart-templates). De sjabloon die we gaan implementeert een Linux VM aan de regio West VS. Hoewel deze sjabloon de sjabloon beschikbaar in een openbare bibliotheek als GitHub hebt, kunt u ook de volledige sjabloon doorgeven als onderdeel van de aanvraag selecteren. Houd er rekening mee dat wij parameterwaarden als onderdeel van de aanvraag die wordt gebruikt in de gebruikte sjabloon.

(SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD en DNS_NAME_FOR_PUBLIC_IP om waarden op die geschikt zijn voor uw aanvraag te vervangen)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Het behoorlijk lang JSON antwoord voor deze aanvraag ter verbetering van de leesbaarheid van deze documentatie zijn weggelaten. Het antwoord bevat informatie over de implementatie van de sjablonen die u zojuist hebt gemaakt.

