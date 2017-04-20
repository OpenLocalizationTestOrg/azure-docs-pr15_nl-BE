<properties 
    pageTitle="Emoji emoticons in Azure Mobile Engagement gebruiken" 
    description="Het gebruik van Emoji emoticons in de push-meldingen"     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="use-emoji-emoticon-within-push-notifications"></a>Emoji emoticons in Push-meldingen gebruiken

Kunt u opnemen Emoji emoticons in u push-bericht in een paar eenvoudige stappen: 

1. Ten eerste moet u zoeken naar de Emoji wilt u verzenden in het bericht. Zorg ervoor dat de Emoji die u selecteert wordt ondersteund door het doelapparaat als apparaat fabrikanten duren voordat nieuwe goedgekeurde Emojis naar het andere apparaat toevoegen. 

2. U kunt **Windows** - Ga naar deze [link](http://apps.timwhitlock.info/emoji/tables/unicode) en het pictogram 'Native' kopiëren.

    ![][7] 

3. Op **Mac** - vindt u dat de Emojis in toepassing van de woordenlijst bewerken -> Emoji en symbolen.

    ![][6] 

4. Nu, Ga naar het tabblad **bereiken** op het de betrokkenheid van Azure Mobile portal. Selecteer het type van de push-bericht (aankondiging, controleert enz). In dit voorbeeld kiezen we een push aankondiging.

5. Geef de verschillende velden van de melding totdat u bij de tekst van de melding. Dit is waar u uw Emoji emoticons toevoegen. U kunt in de titel of het bericht plaatsen. U moet slepen en neerzetten of kopiëren van de Emoji die u op de bovenstaande locaties vinden. 

    ![][1]

6. Vul de overige velden voor de melding en opslaan. 

7. Als u een test uitvoeren of activeren van de aankondiging ziet u een melding met de emoticon zoals is opgegeven.   

    ![][3] ![][4] ![][5]

<!-- Images. -->
[1]: ./media/mobile-engagement-use-emoji-with-push/notification_input.png
[3]: ./media/mobile-engagement-use-emoji-with-push/iOS_Emoji.png
[4]: ./media/mobile-engagement-use-emoji-with-push/Android_Emoji.png
[5]: ./media/mobile-engagement-use-emoji-with-push/WindowsPhone_Emoji.png
[6]: ./media/mobile-engagement-use-emoji-with-push/Mac_SelectEmoji.png
[7]: ./media/mobile-engagement-use-emoji-with-push/Windows_SelectEmoji.png

