<properties 
    pageTitle="Hoe beheer gebruikersaccounts in Azure API Management | Microsoft Azure" 
    description="Informatie over het maken of gebruikers in Azure API beheer uitnodigen" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Het beheren van gebruikersaccounts in Azure API beheer

Ontwikkelaars zijn API in beheer van de gebruikers van de API's die u met behulp van beheer-API. Deze handleiding bevat voor het maken en ontwikkelaars uitnodigen gebruik van de API's en producten te maken met uw exemplaar van de beheer-API voor hen beschikbaar. Zie de documentatie van de [entiteit gebruiker](https://msdn.microsoft.com/library/azure/dn776330.aspx) in de [REST van API-beheer](https://msdn.microsoft.com/library/azure/dn776326.aspx) voor informatie over het programmatisch beheer van gebruikersaccounts.

## <a name="create-developer"> </a>Maakt een nieuwe ontwikkelaar

Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API om een nieuwe ontwikkelaar. Hiermee gaat u naar de portal Management API publisher. Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de zelfstudie [aan de slag met Azure API beheer][] .

![Publisher-portal][api-management-management-console]

Klik op **gebruikers** in het menu **API beheer** aan de linkerkant en klik vervolgens op **gebruiker toevoegen**.

![Ontwikkelaars maken][api-management-create-developer]

Het **e-mailadres**, **wachtwoord**en de **naam** voor de nieuwe ontwikkelaar en klik op **Opslaan**.

![Ontwikkelaars maken][api-management-add-new-user]

Standaard accounts voor nieuwe ontwikkelaars **actief**zijn en die zijn gekoppeld aan de groep **ontwikkelaars** .

![Nieuwe developer][api-management-new-developer]

Developer-accounts met een **actieve** status kunnen worden gebruikt voor toegang tot alle van de API's waarvoor ze abonnementen hebben. Zie voor de ontwikkelaar van de zojuist gemaakte koppelen aan andere groepen, [het koppelen van groepen met ontwikkelaars][].

## <a name="invite-developer"> </a>Een ontwikkelaar uitnodigen

Als u wilt een ontwikkelaar uitnodigen, klikt u op **gebruikers** in het menu **API beheer** aan de linkerkant en klik vervolgens op **Gebruiker uitnodigen**.

![Ontwikkelaar uitnodigen][api-management-invite-developer]

Voer de naam en e-mailadres van de ontwikkelaar en klik op **uitnodigen**.

![Ontwikkelaar uitnodigen][api-management-invite-developer-window]

Een bevestigingsbericht wordt weergegeven, maar de ontwikkelaar van de nieuwe uitgenodigde niet in de lijst pas nadat ze de uitnodiging accepteren. 

![Bevestiging uitnodigen][api-management-invite-developer-confirmation]

Als een ontwikkelaar is uitgenodigd, wordt een e-mail verzonden naar de ontwikkelaar. Deze e-mail wordt gegenereerd op basis van een sjabloon, en kan worden aangepast. Zie [e-mailsjablonen voor configureren][]voor meer informatie.

Zodra de uitnodiging is geaccepteerd, wordt de account actief.

## <a name="block-developer"></a> Deactiveren of opnieuw activeren van een account developer

Nieuwe of uitgenodigde developer accounts zijn standaard **actief**. Als u wilt een developer account deactiveren, klikt u op **blokkeren**. Als u wilt een geblokkeerde developer-account activeren, klikt u op **activeren**. Een geblokkeerde developer-account kan geen toegang krijgen tot de portal voor ontwikkelaars en alle API's aanroepen. Als u een gebruikersaccount wilt verwijderen, klikt u op **verwijderen**.

![Blok-ontwikkelaar][api-management-new-developer]

## <a name="reset-a-user-password"></a>Een gebruikerswachtwoord opnieuw instellen

Als u het wachtwoord voor een gebruikersaccount opnieuw instellen, klikt u op de naam van de account.

![Wachtwoord opnieuw instellen][api-management-view-developer]

Klik op **wachtwoord opnieuw instellen** voor het verzenden van een koppeling aan de gebruiker het wachtwoord opnieuw instellen.

![Wachtwoord opnieuw instellen][api-management-reset-password]

Om programmatisch met gebruikersaccounts werkt, raadpleegt u de documentatie van de [entiteit gebruiker](https://msdn.microsoft.com/library/azure/dn776330.aspx) in de [Management-REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx) reference. U kunt opnieuw instellen wachtwoord voor een gebruikersaccount op een specifieke waarde, gebruikt u de bewerking van [een gebruiker bijwerken](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) en geef het gewenste wachtwoord.

## <a name="pending-verification"></a>In behandeling controleren

![In behandeling controleren][api-management-pending-verification]

## <a name="next-steps"> </a>Volgende stappen

Zodra een developer-account is gemaakt, kunt u koppelen aan rollen en het abonneren op producten en API's. Zie voor meer informatie, [het maken en gebruiken van groepen][].


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
[]: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[Het maken en gebruiken van groepen]: api-management-howto-create-groups.md
[Groepen koppelen aan ontwikkelaars]: api-management-howto-create-groups.md#associate-group-developer

[Aan de slag met Azure API beheer]: api-management-get-started.md
[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance
[E-mailsjablonen configureren]: api-management-howto-configure-notifications.md#email-templates