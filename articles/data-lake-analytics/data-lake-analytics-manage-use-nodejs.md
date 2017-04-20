<properties
   pageTitle="Azure gegevens Lake Analytics met Azure SDK voor Node.js beheren | Azure"
   description="Informatie over het beheren van gegevens Lake Analytics-accounts, gegevensbronnen, taken en gebruikers met Azure SDK voor Node.js"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Azure gegevens Lake Analytics met Azure SDK voor Node.js beheren


[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

De SDK Azure voor Node.js kan worden gebruikt voor het beheren van Azure gegevens Lake Analytics-accounts, taken en catalogi. Om te zien met behulp van andere hulpprogramma's voor beheer-onderwerp, klikt u op de bovenstaande tabblad selecteren.

Op dit moment ondersteunt:

  *  **Node.js versie: 0.10.0 of hoger**
  *  **REST API versie voor Account: 2015-10-01-voorbeeld**
  *  **REST API versie voor catalogus: 2015-10-01-voorbeeld**
  *  **REST API versie voor project: 2016-03-20-voorbeeld**

## <a name="features"></a>Functies

- Account management: maken, ophalen, lijst, bijwerken en verwijderen.
- Project management: indienen, get, list, annuleren.
- Beheer van de catalogus: get, lijst, maken (geheimen), (geheimen) bijwerken, verwijderen (geheimen).

## <a name="how-to-install"></a>Het installeren van

```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Verifiëren met behulp van Azure Active Directory

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-analytics-client"></a>De gegevens Lake Analytics-client maken

```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Maak een account gegevens Lake Analytics

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-jobs"></a>Een lijst met taken

```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-the-data-lake-analytics-catalog"></a>Een lijst met databases in de catalogus voor Analytics Lake gegevens ophalen
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Zie ook

- [Microsoft Azure SDK voor Node.js](https://github.com/azure/azure-sdk-for-node)
- [Azure Microsoft SDK for Node.js - gegevensarchief Lake Management](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)
