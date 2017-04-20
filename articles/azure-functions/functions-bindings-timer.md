<properties
    pageTitle="Azure functies timer trigger | Microsoft Azure"
    description="Begrijpen hoe timer triggers in Azure functies."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure functies, functies, verwerking van gebeurtenissen, dynamische compute, zonder server architecture"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-timer-trigger"></a>Azure functies timer trigger

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe u timer triggers in Azure functies configureren. Timer gebeurtenis call functies op basis van een schema, eenmalige of terugkerende.  

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-timer-trigger"></a>Function.JSON voor timer-trigger

Het bestand *function.json* bevat een expressie voor een schema. Bijvoorbeeld wordt het volgende schema elke minuut:

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

De trigger timer schalen meerdere exemplaren automatisch worden verwerkt: slechts één exemplaar van een bepaalde timerfunctie wordt in alle exemplaren worden uitgevoerd.

## <a name="format-of-schedule-expression"></a>Indeling van schema-expressie

De schema-expressie is een [CRON expressie](http://en.wikipedia.org/wiki/Cron#CRON_expression) met 6 velden: `{second} {minute} {hour} {day} {month} {day of the week}`. 

Opmerking: veel van de cron expressies u on line vindt weglaten het veld {tweede}, zodat als u vanuit een van deze kopieert aan te passen voor het extra veld. 

Hier volgen voorbeelden van expressies een ander schema:

Om te activeren om de 5 minuten:

```json
"schedule": "0 */5 * * * *"
```

Om te activeren eenmaal aan het begin van elk uur:

```json
"schedule": "0 0 * * * *",
```

Om te activeren om de twee uur:

```json
"schedule": "0 0 */2 * * *",
```

Elk uur van 9 AM tot 5 PM activeren:

```json
"schedule": "0 0 9-17 * * *",
```

Om te activeren bij elke dag 9:30 uur:

```json
"schedule": "0 30 9 * * *",
```

Om te activeren op elke weekdag 9:30 uur:

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="timer-trigger-c-code-example"></a>Voorbeeld van de timer trigger C#-code

In dit voorbeeld C#-code schrijft een afzonderlijk logboek telkens wanneer die de functie wordt geactiveerd.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
