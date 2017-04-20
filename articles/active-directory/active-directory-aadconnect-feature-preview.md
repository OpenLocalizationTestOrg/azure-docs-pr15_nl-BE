<properties
   pageTitle="Azure AD verbinden: Functies in voorvertoning | Microsoft Azure"
   description="In dit onderwerp wordt beschreven in meer detail-functies die in het voorbeeld in Azure AD verbinden."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/27/2016"
   ms.author="billmath"/>

# <a name="more-details-about-features-in-preview"></a>Meer informatie over functies in voorvertoning
In dit onderwerp wordt beschreven hoe u op dit moment gebruik van functies in voorvertoning.

## <a name="group-writeback"></a>Groep Write-back
De optie voor het terugschrijven van de groep in de optionele onderdelen kunt u voor write-back **Office 365-groepen** aan een forest met Exchange is geïnstalleerd. Dit is een groep die altijd in de cloud is gemaakt. Als u Exchange op gebouwen en u deze groepen naar op gebouwen terugschrijven kunt zodat gebruikers met een Exchange-postbus in ruimten kunnen verzenden en ontvangen e-mails van deze groepen.

Meer informatie over Office 365 groepen en hoe u ze gebruikt vindt u [hier](http://aka.ms/O365g).

Deze groep wordt weergegeven als een distributiegroep op ruimten AD DS. De Exchange-server op het bedrijf moet zich op cumulatieve update voor Exchange 2013 8 (uitgebracht in maart 2015) of Exchange 2016 herkent dit nieuwe groepstype.

**Notities tijdens de voorvertoning**

- Het kenmerk adresboek is momenteel leeg in het voorbeeld. Zonder dit kenmerk, de groep worden niet weergegeven in de algemene Adreslijst. De eenvoudigste manier om dit kenmerk te vullen is met de Exchange-PowerShell-cmdlet `update-recipient`.
- Alleen forests met Exchange-schema zijn geldige doelen voor groepen. Als er geen Exchange is waargenomen, is vervolgens groep Write-back niet mogelijk om in te schakelen.
- Slechts één forest Exchange organisatie implementaties worden ondersteund. Als er meer dan één Exchange organisatie op de gebouwen, moet u een oplossing op locatie GALSync voor deze groepen worden weergegeven in de andere forests.
- De groep Write-back-functie verwerkt niet momenteel beveiligingsgroepen of distributiegroepen.

>[AZURE.NOTE] Een Azure AD Premium-abonnement is vereist voor het terugschrijven van de groep.

## <a name="user-writeback"></a>Gebruiker Write-back
> [AZURE.IMPORTANT] De functie Afdrukvoorbeeld Write-back is verwijderd in de update van augustus 2015 Azure AD verbinden. Als u deze hebt ingeschakeld, moet u deze functie uitschakelen.

## <a name="next-steps"></a>Volgende stappen
Doorgaan met de [aangepaste installatie van Azure AD verbinden](./connect/active-directory-aadconnect-get-started-custom.md).

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
