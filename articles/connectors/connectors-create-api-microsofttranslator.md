<properties
    pageTitle="De Microsoft Translator in apps logica toevoegen | Microsoft Azure"
    description="Overzicht van de connector van Microsoft Translator met REST API parameters"
    services=""
    suite=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-microsoft-translator-connector"></a>Aan de slag met Microsoft Translator-connector
Verbinding maken met Microsoft Translator te vertalen tekst, ontdekt een taal, en nog veel meer. Met Microsoft Translator kunt u: 

- Uw business-flow op basis van de gegevens die u van ontvangt Microsoft Translator bouwen. 
- Acties gebruiken om te vertalen tekst, ontdekt een taal, en nog veel meer. Deze acties een reactie krijgt en vervolgens de uitvoer beschikbaar maken voor andere acties. Wanneer een nieuw bestand wordt gemaakt in Dropbox, kunt u bijvoorbeeld de tekst in het bestand naar een andere taal met behulp van Microsoft Translator vertalen.

Zie als u wilt een bewerking in apps logica toevoegen, [maken een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties
Microsoft Translator omvat de volgende acties. Er zijn geen triggers.

Triggers | Acties
--- | ---
Geen | <ul><li>Taal gedetecteerd</li><li>Tekst naar spraak</li><li>Tekst vertalen</li><li>Talen ophalen</li><li>Spraakherkenning, talen ophalen</li></ul>

Alle verbindingslijnen ondersteuning in JSON en XML-indelingen voor gegevens.


## <a name="create-a-connection-to-microsoft-translator"></a>Een verbinding maken met Microsoft Translator

>[AZURE.INCLUDE [Steps to create a connection to Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]


## <a name="swagger-rest-api-reference"></a>Swagger REST API: naslag
Van toepassing op versie: 1.0.

### <a name="detect-language"></a>Taal gedetecteerd    
Detecteert de brontaal van de opgegeven tekst.  
```GET: /Detect```

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|query|tekenreeks|Ja|query|geen |Tekst waarvan de taal worden geïdentificeerd.|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="text-to-speech"></a>Tekst naar spraak    
Converteert een bepaalde tekst naar spraak als een audio-stream in wave-indeling.  
```GET: /Speak```

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|query|tekenreeks|Ja|query|geen |Tekst converteren|
|taal|tekenreeks|Ja|query|geen |De taalcode voor het genereren van spraak (voorbeeld: ' en-us')|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="translate-text"></a>Tekst vertalen    
Tekst aan een bepaalde taal met behulp van Microsoft Translator vertaalt.  
```GET: /Translate```

| Naam| Gegevenstype|Vereist|Zich In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|query|tekenreeks|Ja|query|geen |Tekst vertalen|
|languageTo|tekenreeks|Ja|query| geen|Code doeltaal (voorbeeld: 'fr')|
|languageFrom|tekenreeks|geen|query|geen |Brontaal; Als u niet opgeeft, probeert Microsoft Translator automatisch te detecteren. (voorbeeld: nl)|
|categorie|tekenreeks|geen|query|Algemeen |Vertaling van categorie (standaard: 'Algemeen')|

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="get-languages"></a>Talen ophalen    
Haalt alle talen die Microsoft Translator ondersteunt.  
```GET: /TranslatableLanguages```

Er zijn geen parameters voor deze oproep. 

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|


### <a name="get-speech-languages"></a>Spraakherkenning, talen ophalen    
De talen die beschikbaar zijn voor spraak synthese opgehaald.  
```GET: /SpeakLanguages``` 

Er zijn geen parameters voor deze oproep.

#### <a name="response"></a>Reactie
|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|De bewerking is mislukt.|

## <a name="object-definitions"></a>Objectdefinities

#### <a name="language-language-model-for-microsoft-translator-translatable-languages"></a>Taal: taalmodel voor Microsoft Translator vertaalbare talen

|Naam van eigenschap | Gegevenstype | Vereist|
|---|---|---|
|Code|tekenreeks|geen|
|Naam|tekenreeks|geen|


## <a name="next-steps"></a>Volgende stappen

[Maken van een app logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

Ga terug naar de [lijst met API's](apis-list.md).


<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png
