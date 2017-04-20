<properties
   pageTitle="Het gebruik van Power BI ingesloten met REST | Microsoft Azure"
   description="Informatie over het gebruik van Power BI ingesloten met REST "
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="how-to-use-power-bi-embedded-with-rest"></a>Het gebruik van Power BI ingesloten met REST


## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Ingesloten voor Power BI: Wat het is en wat het is
Een overzicht van de Power BI ingesloten wordt beschreven in de officiële [site voor Power BI ingesloten](https://azure.microsoft.com/services/power-bi-embedded/), maar laten we snel voordat we hier verder in de details over het gebruik van het met de REST.

Het is echt heel eenvoudig. Vaak wil een ISV de visualisaties van dynamische gegevens van [Power BI](https://powerbi.microsoft.com) in hun eigen toepassing gebruiken als bouwstenen UI.

Maar u weet, Power BI rapporten of tegels in uw webpagina insluiten is al mogelijk zonder de ingesloten Azure voor Power BI-service met behulp van de **Power BI-API**. Als u wilt dat uw rapporten in uw organisatie hetzelfde delen, kunt u de rapporten met Azure AD-verificatie. De gebruiker die de rapporten bekijkt moet aanmelden met hun eigen Azure AD-account. Als u wilt delen uw rapporten voor alle gebruikers (inclusief externe gebruikers), kunt u alleen insluiten met anonieme toegang.

U ziet, sluit deze eenvoudige oplossing, maar niet helemaal voldoet aan de behoeften van een ISV-toepassing.
De meeste ISV-toepassingen moeten ervoor zorgen dat de gegevens voor hun eigen klanten, niet noodzakelijkerwijs de gebruikers in hun eigen organisatie. Bijvoorbeeld als u een service voor zowel bedrijf A en bedrijf B bieden, moeten gebruikers in bedrijf A alleen gegevens zien voor hun eigen bedrijf A. Dat wil zeggen is de meerdere pacht nodig voor de levering.

ISV-toepassing kan ook biedt eigen verificatiemethoden zoals formulierverificatie, basic auth, enz... Vervolgens de insluiting oplossing moet samen te werken met deze bestaande verificatiemethoden veilig. Het is ook noodzakelijk wanneer gebruikers gebruikmaken van de ISV-toepassingen zonder de extra aankoop of licentie van een Power BI-abonnement.

 **Power BI ingesloten** is ontworpen voor precies dit soort ISV-scenario's. Dus nu dat we de weg die snelle invoering hebben, we krijgen in sommige details

U kunt .NET \(C#) of Node.js SDK, om gemakkelijk maken voor uw toepassing met Power BI ingesloten. Maar in dit artikel leggen we over HTTP-stroom \(incl. AuthN) van Power BI zonder SDK's. Informatie over deze overdracht, kunt u uw toepassing **met elke programmeertaal**maken en u inzicht in de diep de essentie van Power BI ingesloten.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Power BI maken werkruimte collectie en krijgen toegang tot sleutel \(creëren)
Power BI ingesloten is een van de Azure services. Alleen de ISV die Azure Portal wordt gebruikt voor de gebruikskosten in rekening wordt gebracht \(per sessie van de gebruiker per uur), en de gebruiker die het rapport bekijkt, wordt niet in rekening gebracht of zelfs een Azure-abonnement nodig.
Voordat u begint met het ontwikkelen van toepassingen, maken we de **collectie van Power BI werkruimte** met Azure Portal.

Elke werkruimte van Power BI ingesloten is in de werkruimte voor elke klant (huurder) en we veel werkruimten in elke collectie werkruimte kunt toevoegen. Dezelfde toegangstoets wordt gebruikt in elke werkruimte-collectie. In-effect, de werkruimte-collectie is de beveiligingsgrens voor Power BI ingesloten.

![](media\power-bi-embedded-iframe\create-workspace.png)

Als we klaar bent met het maken van de werkruimte collectie kopiëren de toegangstoets van Azure Portal.

![](media\power-bi-embedded-iframe\copy-access-key.png)

> [AZURE.NOTE] We kunnen ook inrichten van de collectie van de werkruimte en toegangstoets via REST API krijgen. Voor meer informatie, Zie [API's voor Power BI Resource Provider](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>.Pbix-bestand maken met Power BI-Desktop
Vervolgens maken we moet de gegevensverbinding en rapporten kunnen worden ingesloten.
Voor deze taak is er geen programmering of code. We gebruiken gewoon Power BI bureaublad.
In dit artikel, won't doorloopt u de details over het gebruik van Power BI-bureaublad. Als u sommige meer informatie hier, Zie [aan de slag met Power BI bureaublad](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). In ons voorbeeld gebruiken we gewoon de [Retail analyse voorbeeld](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media\power-bi-embedded-iframe\power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Een Power BI-werkruimte maken

Nu dat de inrichting is voltooid, de slag van de klant werkruimte maken in de collectie van de werkruimte via REST API's. De volgende HTTP-POST aanvragen (REST) is de nieuwe werkruimte maken in de collectie van onze bestaande werkruimte. In ons voorbeeld is de naam van de werkruimte verzameling **mypbiapp**.
We stelt de toegangstoets die we eerder hebt gekopieerd, als **AppKey**. Het is een zeer eenvoudige verificatie!

**HTTP-aanvraag**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-antwoord**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

De geretourneerde **workspaceId** wordt gebruikt voor de volgende volgende API-aanroepen. De toepassing moet deze waarde behouden.

## <a name="import-pbix-file-into-the-workspace"></a>.Pbix-bestand importeren in de werkruimte
Elke werkruimte kan één bureaublad voor Power BI-bestand met een dataset host \(zoals datasource-instellingen) en rapporten. We kunnen onze .pbix-bestand importeren naar de werkruimte, zoals in de onderstaande code. Zoals u zien kunt, kunnen we het binaire bestand van multipart MIME in HTTP-met .pbix-bestand uploaden.

De uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** is de workspaceId en query-parameter **datasetDisplayName** is de gegevenssetnaam maken. De gemaakte dataset bevat alle gegevens gerelateerd artefacten in .pbix zoals het bestand als geïmporteerde gegevens, de aanwijzer naar de gegevensbron, enz...

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Deze taak importeren kan uitvoeren voor een tijdje. Na afloop kan de toepassing de status van de taak-id importeren met vragen. In ons voorbeeld is de import-id **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

De volgende wordt status met behulp van deze import-id gevraagd:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Als de taak niet is voltooid, kan het HTTP-antwoord zijn als volgt:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Als de taak voltooid is, is het mogelijk dat het HTTP-antwoord meer als volgt:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Verbinding met de gegevensbron \(en meerdere pacht van gegevens)
Terwijl bijna alle artefacten in het bestand .pbix in de werkruimte worden geïmporteerd, worden de referenties voor gegevensbronnen niet. Worden als gevolg hiervan bij gebruik van **DirectQuery-modus**, het rapport ingesloten kan niet weergegeven correct. Maar wanneer u de **importmodus**, we kunnen het rapport weergeven met de bestaande geïmporteerde gegevens. In dat geval moeten we de referentie volgt via het aanroepen van de REST instellen.

Eerst krijgen we de datasource gateway. We weten dat de dataset- **id** is de eerder geretourneerde id.

**HTTP-aanvraag**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-antwoord**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

De geretourneerde gateway-id en id van de gegevensbron met behulp van \(Zie de vorige **gatewayId** en de **id** in de geretourneerde resultatenlijst), kunnen we de referentie van deze gegevensbron als volgt wijzigen:

**HTTP-aanvraag**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**HTTP-antwoord**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

In productie, kunnen we ook de andere verbindingsreeks voor elke werkruimte met REST API instellen. \(Internet Explorer, we scheiden de database voor alle klanten.)

Het volgende is de verbindingsreeks van de gegevensbron via REST wijzigen.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Of gebruiken we de beveiliging op rij in Power BI ingesloten en we kunnen de gegevens voor alle gebruikers in het ene rapport te scheiden. As a Result, kunnen we elke klant-rapport met dezelfde .pbix inrichten \(UI, enz.) en andere gegevensbronnen.

> [AZURE.NOTE] Als u de **importmodus** in plaats van **DirectQuery-modus gebruikt**, is op geen enkele manier vernieuwen modellen via API. En gegevensbronnen via gateway Power BI op gebouwen nog wordt niet ondersteund in Power BI ingesloten. Echter, zult u echt in het oog houden van de [Power BI-blog](https://powerbi.microsoft.com/blog/) voor nieuw en wat er nieuw in toekomstige releases.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Verificatie en hosting (insluiten) rapporten in onze webpagina

We kunnen de toegangstoets **AppKey** zelf in de vorige REST-API gebruiken als de kop van de vergunning. Het is veilig omdat deze oproepen kunnen worden verwerkt op de back-end server.

Maar wanneer we het rapport in onze webpagina insluit, dit soort beveiliging zou worden verwerkt met behulp van JavaScript \(frontend). De waarde van de header vergunning moet vervolgens worden beveiligd. Onze toegangstoets is ontdekt door een kwaadwillende gebruikers of schadelijke code, belt ze alle bewerkingen met behulp van deze sleutel.

Wanneer we het rapport in onze webpagina insluit, gebruik wij het berekende token toegangstoets **AppKey**. De toepassing moet maken OAuth Json Web Token \(JWT) die bestaat uit de vorderingen en de berekende digitale handtekening. Zie de volgende afbeelding is dit JWT OAuth tokens punt gescheiden gecodeerde tekenreeks.

![](media\power-bi-embedded-iframe\oauth-jwt.png)

Eerst moeten we de invoerwaarde later wordt ondertekend voorbereiden. Deze waarde is de base64-tekenreeks als url zijn gecodeerd (rfc4648) van de volgende json en deze worden gescheiden door de punt \(.) teken. We leggen later, hoe u de lijst-id.

> [AZURE.NOTE] Als we rij niveau beveiliging (RLS) gebruiken met Power BI ingesloten willen, moet we **gebruikersnaam** en **rollen** ook opgeven in de claims.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Vervolgens maken we de base64-gecodeerde reeks HMAC \(de handtekening) met SHA256 algoritme. Dit ondertekende de ingevoerde waarde is de vorige tekenreeks.

Laatste, combineren we de waarde en de handtekening invoerreeks periode met \(.) teken. De voltooide tekenreeks is het app-token voor het insluiten van het rapport. Zelfs als het token app is ontdekt door een kwaadwillende gebruiker, kunnen zij de oorspronkelijke access-sleutel niet ophalen. Deze token app verloopt snel.

Hier volgt een voorbeeld van de PHP voor de volgende stappen uit:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Ten slotte het rapport in de webpagina insluiten

Voor het insluiten van de lijst moeten we de embed url ophalen en verslag **id** met behulp van de volgende REST API.

**HTTP-aanvraag**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-antwoord**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

We kunnen het rapport insluiten in onze web app met behulp van de vorige app-token.
Als we de volgende voorbeeldcode bekijkt, is de voormalige deel hetzelfde als het vorige voorbeeld. In het laatste deel in dit voorbeeld ziet u de **embedUrl** \(Zie het vorige resultaat) in de iframe en boeken van het app-token wordt in de iframe.

> [AZURE.NOTE] U moet de waarde van de rapport-id wijzigen in een van uw eigen. Vanwege een fout in ons systeem voor content management, is de iframe-code in het voorbeeld Lees ook letterlijk. Verwijder de capped tekst uit de code als u kopieert en plakt deze voorbeeldcode.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

En hier is het resultaat:

![](media\power-bi-embedded-iframe\view-report.png)

Op dit moment Power BI ingesloten alleen het rapport wordt weergegeven in het iframe. Maar in het oog houden van de [Power BI-Blog](). Nieuwe client-side API's die wordt laat ons verzenden van informatie in de iframe, alsmede informatie opvragen uit toekomstige verbeteringen zou kunnen gebruiken. Leuke dingen!


## <a name="see-also"></a>Zie ook
- [Verificatie en autorisatie in Power BI ingesloten](power-bi-embedded-app-token-flow.md)
