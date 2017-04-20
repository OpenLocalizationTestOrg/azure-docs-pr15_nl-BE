<properties
   pageTitle="Diagnose van storingen in logica apps | Microsoft Azure"
   description="Gemeenschappelijke aanpak voor wat waar logica apps lukt niet"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="diagnosing-logic-app-failures"></a>Diagnose van storingen in app logica

Als u problemen of fouten met de functie voor Apps logica van Azure App-Service ondervindt, kunt een paar benaderingen u beter te begrijpen waar de problemen vandaan.  

## <a name="azure-portal-tools"></a>Azure portal-hulpprogramma 's

De Azure portal biedt vele hulpprogramma's voor het bepalen van elke app logica bij elke stap.

### <a name="trigger-history"></a>Trigger-geschiedenis

Elke app logica heeft ten minste één trigger. Als u merkt dat de apps niet worden geactiveerd, is de eerste plaats om te zoeken naar aanvullende informatie het trigger-overzicht. Toegang tot de geschiedenis van de trigger op de belangrijkste logica app-blade.

![De trigger geschiedenis zoeken][1]

Hierin worden alle van de trigger pogingen die uw logica app heeft gemaakt. Kunt u elke poging trigger om het volgende detailniveau (met name invoer of uitvoer die de trigger poging gegenereerd). Als u triggers ziet, klikt u op de trigger poging en naar de **uitgangen** -koppeling voor een overzicht van eventuele foutberichten die mogelijk zijn gegenereerd (bijvoorbeeld voor ongeldige FTP-referenties).

De verschillende statussen mogelijk zijn:

* **Overgeslagen**. Het heeft het eindpunt om de gegevens te controleren en een antwoord hebt ontvangen dat geen gegevens beschikbaar is.
* **Is voltooid**. De trigger ontvangen een antwoord dat de gegevens beschikbaar is. Dit kan zijn van een handmatige trigger, een terugkeerpatroon trigger of een trigger polling. Dit waarschijnlijk zal gepaard gaan met de status van **Fired**, maar mogelijk niet als u een voorwaarde of een opdracht voor SplitOn in de codeweergave die niet is voldaan.
* **Is mislukt**. Er is een fout gegenereerd.

#### <a name="starting-a-trigger-manually"></a>Een trigger starten handmatig

Als u wilt dat de toepassing van de logica om te controleren of een beschikbare trigger direct (zonder te wachten op het volgende terugkeerpatroon), kunt u **Trigger selecteren** op de belangrijkste blade te dwingen een cheque. Op deze link te klikken met een trigger Dropbox zal bijvoorbeeld de werkstroom worden onmiddellijk Dropbox gecontroleerd op nieuwe bestanden.

### <a name="run-history"></a>Geschiedenis uitvoeren

Alle triggers die resulteert in een reeks wordt gestart. Toegang tot informatie over van de belangrijkste blade, waarin veel van de informatie die nuttig zijn bij het begrijpen van wat er is gebeurd tijdens de workflow kan worden.

![Zoeken naar de geschiedenis van uitvoeren][2]

Een uitvoert, verschijnt een van de volgende statussen:

* **Is voltooid**. Alle acties zijn voltooid of als er een fout is, is verwerkt door een actie die wordt later in de werkstroom wordt uitgevoerd. Dat wil zeggen, is het afgehandeld door een actie die moet worden uitgevoerd na een mislukte actie is ingesteld.
* **Is mislukt**. Ten minste één actie had een fout die niet is uitgevoerd door een actie later in de werkstroom.
* **Geannuleerd**. De werkstroom is gestart maar een verzoek tot annuleren wordt ontvangen.
* **Uitgevoerd**. De werkstroom wordt uitgevoerd. Dit kan gebeuren voor werkstromen die zijn wordt beperkt of door het huidige plan voor App-Service. Zie limieten voor actie op de [prijzen pagina](https://azure.microsoft.com/pricing/details/app-service/plans/) voor meer informatie. Diagnostische gegevens configureren kunnen (de grafieken hieronder de geschiedenis van uitvoeren) ook informatie verschaffen over de throttle-gebeurtenissen die optreden.

Als u naar de geschiedenis uitvoeren kijkt, kunt u inzoomen voor meer informatie.  

#### <a name="trigger-outputs"></a>Trigger-uitgangen

Trigger-uitgangen de gegevens weergeven die is ontvangen van de trigger. Hierdoor kunt u bepalen of alle eigenschappen weergegeven zoals verwacht.

>[AZURE.NOTE] Handig het kan zijn om te begrijpen hoe de logica Apps zijn uitgerust met [verschillende typen inhoud worden verwerkt](app-service-logic-content-type.md) als u alle inhoud die u niet begrijpt.

![Voorbeelden van trigger-uitvoer][3]

#### <a name="action-inputs-and-outputs"></a>Actie-ingangen en uitgangen

U kunt inzoomen in- en uitgangen die een actie ontvangen. Dit is handig voor informatie over de grootte en vorm van de resultaten, alsmede over eventuele foutberichten die zijn gegenereerd Zie.

![Actie-ingangen en uitgangen][4]

## <a name="debugging-workflow-runtime"></a>Workflowruntime Debugging

Naast het controleren van de inputs en outputs en triggers van een reeks, kan het zinvol zijn om toe te voegen sommige stappen in een werkstroom om te helpen bij het opsporen van fouten. [RequestBin](http://requestb.in) is een krachtig hulpmiddel dat u als een stap in een werkstroom toevoegen kunt. Via RequestBin kunt u een HTTP-aanvraag inspecteur instellen bepalen de exacte grootte, vorm en indeling van een HTTP-aanvraag. U kunt een nieuwe RequestBin maken en plak de URL in een logica app HTTP POST actie samen met de instantie inhoud u wilt testen (bijvoorbeeld een expressie of een andere stap uitvoer). Nadat u de app logica hebt uitgevoerd, kunt u uw RequestBin om te zien hoe de aanvraag is gevormd vernieuwen als is gegenereerd vanuit de logica Apps engine.




<!-- image references -->
[1]: ./media/app-service-logic-diagnosing-failures/triggerHistory.PNG
[2]: ./media/app-service-logic-diagnosing-failures/runHistory.PNG
[3]: ./media/app-service-logic-diagnosing-failures/triggerOutputsLink.PNG
[4]: ./media/app-service-logic-diagnosing-failures/ActionOutputs.PNG
