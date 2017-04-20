<properties
    pageTitle="Onregelmatige activiteit aanmelden"
    description="Een rapport met modules die zijn geïdentificeerd als afwijkende door onze algoritmen leren computer aanmelden."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="gchander"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="irregular-sign-in-activity"></a>Onregelmatige activiteit aanmelden

Onregelmatige aanmeldingen zijn die welke zijn vastgesteld door onze machine learning algoritmen op basis van een "onmogelijk reizen"-voorwaarde gecombineerd met een afwijkende teken op de locatie en het apparaat. Dit kan betekenen dat een hacker met deze account heeft aangemeld.
We sturen een e-mailbericht aan de globale beheerders als we 10 of meer afwijkende-in gebeurtenissen binnen een periode van 30 dagen of minder. Zorg ervoor dat aad-alerts-noreply@mail.windowsazure.com in de lijst met veilige afzenders.
