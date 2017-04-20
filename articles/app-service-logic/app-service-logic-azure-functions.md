<properties
   pageTitle="Gebruik van Azure functies met logica Apps | Microsoft Azure"
   description="Zie Azure functies gebruiken met logica Apps"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="using-azure-functions-with-logic-apps"></a>Gebruik van Azure functies met logica Apps

U kunt aangepaste fragmenten van C# of node.js uitvoeren via Azure functies uit binnen een logica app.  [Azure functies](../azure-functions/functions-overview.md) biedt de server gratis computergebruik in Microsoft Azure. Dit is handig voor het uitvoeren van de volgende taken:

* Geavanceerde opmaak of berekenen van velden in een logische App
* Berekeningen in een werkstroom
* De functionaliteit van Apps logica met functies die worden ondersteund in C# of node.js uitbreiden

## <a name="create-a-function-for-logic-apps"></a>Functies voor bedrijfslogica Apps maken

Het is raadzaam om een nieuwe functie te maken in de portal Azure functies met behulp van de **Algemene Webhook - knooppunt** of **Algemene Webhook - C#** -sjablonen. Deze auto-vult een sjabloon die u accepteert `application/json` vanuit een app logica.  Als u het tabblad **integratie** in Azure functies moet deze **modus** ingesteld op **Webhook** en **Webhook type** **Generieke JSON**hebben.  Functies die gebruikmaken van deze sjablonen worden automatisch ontdekt en vermeld in de ontwerpfunctie voor logica Apps onder **functies in mijn regio Azure.**

Webhook functies een verzoek accepteren en deze doorgeven aan de methode via een `data` variabele. U kunt toegang tot de eigenschappen van de nettolading met puntnotatie zoals `data.foo`.  Een eenvoudige JavaScript-functie die een DateTime-waarde naar een datumreeks converteert, ziet er bijvoorbeeld als volgt uit:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-a-logic-app"></a>Azure-functies aanroepen vanuit een app logica

Als u klikt op het menu **Acties** kunt u in de ontwerpfunctie voor **Azure functies in mijn regio**selecteren.  Dit geeft een overzicht van de containers in uw abonnement en kunt u de functie die u wilt bellen.  

Nadat u de functie hebt geselecteerd, wordt u gevraagd een invoer-nettolading object opgeven. Dit is het bericht dat de logica app naar de functie verzendt en moet een JSON-object. Bijvoorbeeld, als u doorgeven in de **Laatste** wijzigingsdatum van een trigger televergaderingen wilt, de nettolading van de functie als volgt uitzien:

![Datum van laatste modfied][1]

## <a name="trigger-logic-apps-from-a-function"></a>Trigger logica apps van een functie

Het kan ook een app logica uit binnen een functie te activeren.  Hiervoor gewoon een app logica met een handmatige trigger te maken. Voor meer informatie Zie [logica apps als callable eindpunten](app-service-logic-http-endpoint.md).  Vervolgens binnen uw functie, genereert een HTTP POST naar de URL handmatig trigger met de nettolading die u wilt verzenden naar de app logica.

### <a name="create-a-function-from-the-designer"></a>Maak een functie van de designer

U kunt ook een webhook functie node.js in de ontwerpfunctie van maken. Eerst selecteert u **Azure functies in mijn regio,** en kies vervolgens een container voor uw functie.  Als u een container nog niet hebt, moet u maken vanuit de [portal Azure functies](https://functions.azure.com/signin). Selecteer vervolgens **Nieuw**.  

Geef de contextobject dat u wilt doorgeven aan een functie voor het genereren van een sjabloon op basis van de gegevens die u wilt berekenen. Dit moet een JSON-object. Bijvoorbeeld als u in de inhoud van het bestand van een FTP-actie, eruit de nettolading context als volgt:

![Context-nettolading][2]

>[AZURE.NOTE] Omdat dit object niet is geconverteerd als een tekenreeks, wordt de inhoud rechtstreeks naar de JSON-nettolading toegevoegd. Het wordt echter fout uit als dit nog niet een JSON-token (dat wil zeggen een tekenreeks of een JSON object-matrix). Om de cast deze als tekenreeks, toe te voegen aanhalingstekens zoals weergegeven in de eerste afbeelding in dit artikel.

De ontwerper genereert vervolgens een sjabloon dat u kunt in line functie. Variabelen worden vooraf gemaakt op basis van de context die u wilt doorgeven aan de functie.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png
