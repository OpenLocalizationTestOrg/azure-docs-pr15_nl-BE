<properties 
    pageTitle="App bedrijfslogica-functies | Microsoft Azure" 
    description="Informatie over het gebruik van de geavanceerde functies van apps logica." 
    authors="stepsic-microsoft-com" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/28/2016"
    ms.author="stepsic"/> 
    
# <a name="use-logic-apps-features"></a>Logica Apps-functies gebruiken

In het [vorige onderwerp](app-service-logic-create-a-logic-app.md), kunt u uw eerste logica app gemaakt. Nu leert we u hoe u een vollediger proces met App Services logica Apps. In dit onderwerp worden de volgende nieuwe logica Apps begrippen:

- Voorwaardelijke logica waarmee een actie wordt uitgevoerd wanneer een bepaalde voorwaarde is voldaan.
- De weergave van de code voor het bewerken van een bestaande logica app.
- Opties voor het starten van de werkstroom.

Voordat u dit onderwerp hebt voltooid, moet u de stappen in [een nieuwe logica app maken](app-service-logic-create-a-logic-app.md). Ga naar uw app logica in [Azure portal]en **Triggers en acties** in het overzicht voor het bewerken van de definitie van de logica app klikt u op.

## <a name="reference-material"></a>Referentiemateriaal

U nuttig kan zijn de volgende documenten:

- [Beheer en de runtime-API's REST](https://msdn.microsoft.com/library/azure/mt643787.aspx) - met inbegrip van het rechtstreeks aanroepen van logica apps
- [Language reference](https://msdn.microsoft.com/library/azure/mt643789.aspx) - een uitgebreide lijst van alle ondersteunde functies expressies
- [Trigger- en type](https://msdn.microsoft.com/library/azure/mt643939.aspx) - de verschillende soorten acties en de "inputs" die zij nemen
- [Overzicht van App-Service](../app-service/app-service-value-prop-what-is.md) - beschrijving van wat onderdelen instellen wanneer een oplossing bouwen

## <a name="adding-conditional-logic"></a>Voorwaardelijke logica toe te voegen

Hoewel de oorspronkelijke stroom werkt, zijn er enkele gebieden die kunnen worden verbeterd. 


### <a name="conditional"></a>Voorwaardelijke
Deze logica app kan leiden tot u een groot aantal e-mailberichten ophalen. De volgende stappen toevoegen instructies om ervoor te zorgen dat u alleen een e-mail ontvangt wanneer de tweet van iemand met een bepaald aantal hobbyisten komt. 

1. Klik op het plusteken en de actie van *Gebruiker ophalen* voor Twitter vinden.

2. Geef in het veld **Tweeted door** van de trigger kunt u de informatie over de Twitter-gebruiker.

    ![Gebruiker ophalen](./media/app-service-logic-use-logic-app-features/getuser.png)

3. Klik nogmaals op de plusknop, maar deze keer selecteren **Voorwaarde toevoegen**

4. Klik op de **...** onder de **Gebruiker krijgt** het veld **aantal hobbyisten** vinden in het eerste vak.

5. Selecteer in de vervolgkeuzelijst, **groter is dan**

6. Typ het aantal hobbyisten die u wilt dat gebruikers in het tweede vak.

    ![Voorwaardelijke](./media/app-service-logic-use-logic-app-features/conditional.png)

7.  Ten slotte vak slepen en neerzetten het e-mailbericht in het vak **Indien Ja** . Dit betekent dat u krijgt alleen e-mails wanneer het aantal volger is voldaan.

## <a name="repeating-over-a-list-with-foreach"></a>Over een lijst met forEach herhalen

De forEach lus geeft een matrix als u wilt herhalen op. Als het niet een matrix die de stroom uitvalt. Als u bijvoorbeeld als u hebt action1 die een matrix van berichten wordt de uitvoer en u wilt dat elk bericht te verzenden kunt u opnemen deze forEach-instructie in de eigenschappen van de actie: forEach:"@action('action1').outputs.messages"
 

## <a name="using-the-code-view-to-edit-a-logic-app"></a>De codeweergave gebruiken voor het bewerken van een App logica

Naast de designer, kunt u de code die wordt als volgt gedefinieerd een app logica rechtstreeks bewerken. 

1. Klik op de knop **codeweergave** op de opdrachtbalk. 

    Hiermee opent u een volledige editor waarin de definitie die u zojuist hebt bewerkt.

    ![Codeweergave](./media/app-service-logic-use-logic-app-features/codeview.png)

    Met behulp van de editor, kunt u kopiëren en plakken van een willekeurig aantal acties binnen de app met dezelfde logica of tussen apps logica. U kunt ook gemakkelijk toevoegen of volledige secties verwijderen uit de definitie en u kunt definities ook delen met anderen.

2. Nadat u uw wijzigingen in de codeweergave aanbrengt, klikt u op **Opslaan**. 

### <a name="parameters"></a>Parameters
Er zijn enkele mogelijkheden van Logic-Apps die alleen kunnen worden gebruikt in de codeweergave. Een voorbeeld hiervan is de parameters. Deze kunnen parameters u gemakkelijk waarden opnieuw gebruiken in uw app logica. Bijvoorbeeld als u een e-mailadres hebt dat u gebruiken in diverse acties wilt, moet u definiëren het als een parameter.

De volgende updates uw bestaande app logica voor het gebruik van parameters voor de queryterm geplaatst.

1. Zoek in de codeweergave, de `parameters : {}` -object en wordt het volgende onderwerp object invoegen:

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }
    
2. Ga naar de `twitterconnector` actie, zoek naar de querywaarde en vervang ze door `#@{parameters('topic')}`.
    U kunt ook de functie **concat** deelnemen aan elkaar twee of meer reeksen, bijvoorbeeld: `@concat('#',parameters('topic'))` is gelijk aan het bovenstaande. 
 
Parameters zijn een goede manier om de waarden die u waarschijnlijk veel veranderen. Ze zijn vooral handig wanneer u wilt negeren de parameters in verschillende omgevingen. Zie onze [REST API-documentatie](https://msdn.microsoft.com/library/mt643787.aspx)voor meer informatie over het overschrijven van de parameters die zijn gebaseerd op de omgeving.

Nu, als u op **Opslaan**klikt, elk uur krijgt u een nieuwe tweets met meer dan 5 retweets bezorgd bij de map **tweets** in uw Dropbox.

Voor meer informatie over de logica App definities, Zie [definities logica App ontwerpen](app-service-logic-author-definitions.md).

## <a name="starting-a-logic-app-workflow"></a>Starten van de werkstroom logica app
Er zijn verschillende opties voor het starten van de werkstroom die is gedefinieerd in u app logica. Een werkstroom kan altijd worden gestart op verzoek in [Azure portal].

### <a name="recurrence-triggers"></a>Triggers voor terugkeerpatroon
Een terugkeerpatroon trigger wordt uitgevoerd met een interval dat u opgeeft. Wanneer de trigger voorwaardelijke logica heeft, bepaalt de trigger of de werkstroom moet worden uitgevoerd. Een trigger geeft aan dat deze moet worden uitgevoerd door een `200` statuscode. Wanneer u niet hoeft te worden uitgevoerd, wordt een `202` statuscode.

### <a name="callback-using-rest-apis"></a>Terugbellen met REST API 's
Services een eindpunt logica app u start een werkstroom kunnen worden aangeroepen. Zie [logica apps als callable eindpunten](app-service-logic-connector-http.md) voor meer informatie. Om dat soort logica app bellen op start, klik op **nu uitvoeren** op de opdrachtbalk. 

<!-- Shared links -->
[Azure portal]: https://portal.azure.com 