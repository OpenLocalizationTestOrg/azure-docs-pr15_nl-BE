<properties
    pageTitle="Instellen van waarschuwingen voor uw abonnementen op Microsoft Azure facturering | Microsoft Azure"
    description="Hierin wordt beschreven hoe u kunt waarschuwingen instellen op uw rekening Azure zodat kunt u de facturering verrassingen vermijden."
    services=""
    documentationCenter=""
    authors="vikdesai"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/18/2016"
    ms.author="vikdesai"/>

# <a name="set-up-billing-alerts-for-your-microsoft-azure-subscriptions"></a>Factuuradres waarschuwingen instellen voor uw abonnementen op Microsoft Azure

Bent u bezorgd over hoeveel u elke maand voor uw abonnement op Azure uitgeeft? Als u de accountbeheerder voor een Azure-abonnement, kunt u de Azure facturering Alert-Service voor het maken van aangepaste facturatie waarschuwingen die u helpen bij het bewaken en beheren van facturering activiteit van uw accounts Azure.

Deze service is een voorbeeld, dus het eerste wat dat u moet doen is teken omhoog voor. Ga naar [de pagina onderdelen van de voorvertoning](https://account.windowsazure.com/PreviewFeatures) in de Azure account management portal deze functie inschakelen.

## <a name="set-the-alert-threshold-and-email-recipients"></a>De drempel- en e-ontvangers instellen

Nadat u de e-mailbevestiging die de betaalservices is ingeschakeld voor uw abonnement hebt ontvangen, gaat u naar [de pagina abonnementen](https://account.windowsazure.com/Subscriptions) in het portaal account. Klik op het abonnement dat u wilt controleren en klik vervolgens op **signalen**.

![][Image1]

Klik vervolgens op **Waarschuwing toevoegen** om zelf een maken - u kunt een totaal van vijf facturering meldingen per abonnement, met een verschillende drempel en maximaal twee geadresseerden per bericht instellen.

![][Image2]

Wanneer u een waarschuwing wilt toevoegen, u een unieke naam geven, kiest u een drempel voor uitgaven en u de e-mailadressen waarbij waarschuwingen worden verzonden. Bij het instellen van de drempel, kunt u een **Totale facturering** of een **Krediet voor monetaire** uit de lijst met **Waarschuwingen voor** . Voor een factuuradres totaal een waarschuwing verzonden wanneer het abonnement uitgaven hoger is dan de drempel. Voor een creditering van de monetaire, wordt een waarschuwing verzonden wanneer monetaire credits de limiet beneden. Gratis proefversies en die zijn gekoppeld aan accounts voor MSDN-abonnementen doorgaans monetaire verantwoording van toepassing.

![][Image3]

Azure biedt ondersteuning voor elk e-mailadres, maar niet verifiëren dat de e-mail adres werken, dus Controleer op typefouten.

## <a name="check-on-your-alerts"></a>Controleren op waarschuwingen

Na het instellen van waarschuwingen, de Account worden en ziet u hoeveel meer u kunt instellen. Voor elke waarschuwing ziet u de datum en tijdstip van verzending, of u nu een waarschuwing voor totale facturering of monetaire krediet en de limiet die u hebt ingesteld. De notatie voor datum en tijd is 24 uur Universal Time Coordinate (UTC) en de datum is jjjj-mm-dd-indeling. Klik op het plusteken (+) voor een waarschuwing in de lijst om deze te bewerken, of klik op de Prullenbak om het te verwijderen.

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png
