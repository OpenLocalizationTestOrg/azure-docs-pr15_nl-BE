<properties
   pageTitle="Tekenreeksen maken voor de table designer | Microsoft Azure"
   description="Samenstellen van tekenreeksen voor de ontwerpfunctie voor tabellen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="constructing-filter-strings-for-the-table-designer"></a>Samenstellen van tekenreeksen voor de ontwerpfunctie voor tabellen

## <a name="overview"></a>Overzicht

Om gegevens te filteren in een Azure-tabel die wordt weergegeven in de **Ontwerpfunctie voor tabellen**voor Visual Studio, een filtertekenreeks samenstellen en deze invoeren in het filterveld. De syntaxis van de filter is gedefinieerd door de Data WCF Services en lijkt op een SQL WHERE-component, maar naar de tabel service via een HTTP-aanvraag is verzonden. De **Ontwerpfunctie voor tabellen** verwerkt de codering voor u, zo kunt u filteren op een waarde van de gewenste eigenschap, moet u alleen de naam van de eigenschap, vergelijkingsoperator criteriawaarden en desgewenst Booleaanse invoeren operator in het filterveld. U hoeft niet de optie $filter query opnemen als wanneer u een URL om de tabel via de [REST API Reference voor opslag diensten](http://go.microsoft.com/fwlink/p/?LinkId=400447)zijn samen.

De gegevens WCF-Services zijn gebaseerd op de [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Zie voor meer informatie over de optie filter systeem query (**$filter**), de [URI-conventies OData-specificatie](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Vergelijkingsoperatoren

De volgende logische operatoren worden ondersteund voor alle soorten:

|Logische operator|Beschrijving|Voorbeeld van de filtertekenreeks|
|---|---|---|
|EQ|Gelijk aan|Stad eq "Redmond"|
|gt|Groter dan|Prijs gt 20|
|ge|Groter dan of gelijk aan|Prijs ge 10|
|lt|Minder dan|Prijs lt 20|
|le|Kleiner dan of gelijk|Prijs le 100|
|ne|Niet gelijk aan|Ne plaats 'London'|
|en|En|Le prijs 200 en prijs gt 3.5|
|of|Of|Prijs le 3.5 of gt prijs 200|
|niet|Niet|niet isAvailable|

De volgende regels zijn van belang bij het construeren van een filtertekenreeks:

- Logische operators gebruiken een eigenschap een waarde vergelijken. Het is niet mogelijk te vergelijken een eigenschap aan een dynamische waarde; een kant van de expressie moet een constante.

- Alle delen van de filtertekenreeks zijn hoofdlettergevoelig.

- De constante waarde moet van hetzelfde gegevenstype zijn als de eigenschap voor het filter om geldige resultaten te retourneren. Zie [Wat zijn de gegevens van tabel servicemodel](http://go.microsoft.com/fwlink/p/?LinkId=400448)voor meer informatie over ondersteunde eigenschaptypen.

## <a name="filtering-on-string-properties"></a>Filteren op een reeks eigenschappen

Wanneer u tekenreekseigenschappen filteren, plaatst u de tekenreeksconstante tussen enkele aanhalingstekens.

De volgende filters voorbeeld van de eigenschappen **PartitionKey** en **RowKey** ; aanvullende eigenschappen met key niet kunnen ook worden toegevoegd aan de filtertekenreeks:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

U kunt haakjes elke filterexpressie, maar niet vereist is:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Houd er rekening mee dat de tabel service biedt geen ondersteuning voor query's met jokertekens en worden niet ondersteund in de ontwerpfunctie voor tabellen of. U kunt echter prefix dat overeenkomt met behulp van vergelijkingsoperators op het gewenste voorvoegsel uitvoeren. In het volgende voorbeeld geeft als resultaat de entiteiten met een eigenschap die achternaam begint met de letter 'A':

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Filteren op numerieke eigenschappen

Als u wilt filteren op een geheel getal of getal met drijvende komma, geef het aantal zonder aanhalingstekens te gebruiken.

In het volgende voorbeeld wordt alle entiteiten met een leeftijd eigenschap waarvan de waarde groter dan 30 is:

    Age gt 30

In het volgende voorbeeld wordt alle entiteiten met een AmountDue eigenschap waarvan de waarde kleiner dan of gelijk aan 100.25 is:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Filteren op Booleaanse eigenschappen

Opgeven als u wilt filteren op een Boolean-waarde **true** of **false** zonder aanhalingstekens te gebruiken.

In het volgende voorbeeld geeft als resultaat alle entiteiten waarvan de eigenschap IsActive is ingesteld op **true**:

    IsActive eq true

U kunt deze filterexpressie zonder de logische operator ook schrijven. In het volgende voorbeeld wordt geeft de tabel service ook alle entiteiten **waarvoor IsActive geldt**:

    IsActive

Als u weer alle entiteiten waarbij IsActive false is, kunt u het niet operator:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Filteren op datum/tijd-eigenschappen

Als u wilt filteren op een datum / tijdwaarde, geeft het sleutelwoord **datetime** , gevolgd door de datum en tijd constante tussen enkele aanhalingstekens. De datum/tijd-constante moet gecombineerde UTC-notatie, zoals beschreven in de [DateTime eigenschapswaarden opmaak](http://go.microsoft.com/fwlink/p/?LinkId=400449).

In het volgende voorbeeld retourneert de entiteiten waarvoor de eigenschap CustomerSince gelijk is aan 10 juli 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
