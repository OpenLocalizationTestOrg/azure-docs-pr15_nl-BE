<properties
   pageTitle="Het uitvoeren van een onderzoek toegang | Microsoft Azure"
   description="Nadat u een access-onderzoek in Azure AD beschermde Identity Management gestart, informatie over het voltooien en de resultaten bekijken"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Het uitvoeren van een onderzoek van de toegang in Azure AD beschermde Identity Management


Beheerders van bevoorrechte rol kunnen bekijken bevoorrechte toegang eenmaal een [onderzoek is gestart](active-directory-privileged-identity-management-how-to-start-security-review.md). Azure rechten AD Identity Management (PIM) stuurt automatisch een e-mail waarin gebruikers hun toegang controleren. Als een gebruiker niet een e-mailbericht hebt, kunt u ze de instructies verzenden in [het uitvoeren van een onderzoek](active-directory-privileged-identity-management-how-to-perform-security-review.md).

Nadat de periode beveiliging controleren of alle gebruikers hun eigen bekijken hebt voltooid, voert u de stappen in dit artikel voor het beheren van de evaluatie en de resultaten weer.

## <a name="manage-security-reviews"></a>Beoordelingen voor beveiliging beheren

1. Ga naar de [Azure portal](https://portal.azure.com/) en selecteer de toepassing **Azure AD beschermde Identity Management** op het dashboard.
2. Selecteer de sectie **beoordeelt de toegang** van het dashboard.
3. Selecteer de toegang controleren die u wilt beheren.

In de access-revisie detail blade zijn er een aantal opties voor het beheer van die evaluatie.

![PIM access controle knoppen - screenshot][1]

### <a name="remind"></a>Herinneren

Als u een access-onderzoek is ingesteld zodat gebruikers zelf bekijken, verzendt de knop **herinnering sturen** een bericht. 

### <a name="stop"></a>Stop

Alle beoordelingen van access een einddatum hebben, maar u kunt de knop **stoppen** voortijdig wilt voltooien. Als gebruikers die nog niet is gecontroleerd op dit moment, meer ze niet kunt nadat u de revisie beëindigen. U kunt een revisie niet opnieuw starten nadat deze gestopt.

### <a name="apply"></a>Toepassing

Als een access-onderzoek is voltooid, implementeert ofwel omdat u de einddatum is bereikt of het handmatig gestopt de knop **toepassen** de resultaten van het onderzoek. Als een gebruiker toegang is geweigerd bij de herziening, is dit de stap die de toewijzing wordt verwijderd.  

### <a name="export"></a>Exporteren

Als u wilt dat de resultaten van de evaluatie van de beveiligingsupdate handmatig toepassen, kunt u de controle kunt exporteren. De knop **exporteren** zal beginnen met het downloaden van een CSV-bestand. U kunt de resultaten in Excel of andere programma's die CSV-bestanden beheren.

### <a name="delete"></a>Verwijderen

Als u niet geïnteresseerd in het onderzoek verder bent, verwijderen. De knop **verwijderen** verwijdert de herziening van de toepassing PIM.

> [AZURE.IMPORTANT] U wordt niet wordt een waarschuwing weergegeven voordat de verwijdering plaatsvindt, dus zorg ervoor dat u wilt verwijderen van dit onderzoek.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
