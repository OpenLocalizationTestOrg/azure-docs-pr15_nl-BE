<properties 
    pageTitle="Verificatie met mobiel betrokkenheid bij de REST API - handmatig instellen"
    description="Hierin wordt beschreven hoe u verificatie voor Mobile Engagement REST API's handmatig instellen" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Verificatie met mobiel betrokkenheid bij de REST API - handmatig instellen

Dit is een aanhangsel documentatie verificatie [bij Mobile Engagement REST API's](mobile-engagement-api-authentication.md). Zorg ervoor dat u deze eerst om de context te lezen. Beschrijving van een andere methode voor het eenmalig instellen voor het instellen van de verificatie voor de Mobile Engagement REST API's met behulp van de Portal Azure. 

>[AZURE.NOTE] De onderstaande instructies zijn gebaseerd op deze [Active Directory-handleiding](../resource-group-create-service-principal-portal.md) en aangepast voor wat vereist voor verificatie voor Mobile Engagement API's is. Zo verwijzen als u de stappen hieronder in detail te begrijpen. 

1. Aanmelden bij uw Account Azure via de [klassieke portal](https://manage.windowsazure.com/).

2. Selecteer **Active Directory** in het linkerdeelvenster.

     ![Selecteer Active Directory][1]

3. Kies de **Standaard Active Directory** in uw Azure-portal. 

     ![Kies map][2]

    >[AZURE.IMPORTANT] Deze methode werkt alleen als u in de standaard Active Directory van uw account werkt en niet werkt als u dit doet in een Active Directory die u hebt gemaakt in uw account. 

4. De toepassingen in de map, klik op **toepassingen**.

     ![toepassingen weergeven][3]

5. Klik op **toevoegen**. 

     ![toepassing toevoegen][4]

6. Klik op **het ontwikkelen van mijn organisatie toepassing toevoegen**

     ![nieuwe toepassing][5]

6. Vul de naam van de toepassing en selecteert u het type toepassing als **WEB APPLICATION en/of WEB API** en klik op de knop Volgende.

     ![naam toepassing][6]

7. U kunt dummy URL's opgeven voor **SIGN-ON-URL** en **APP-ID-URI**. Ze worden niet gebruikt voor ons scenario en de URL's zelf niet worden gevalideerd.  

     ![Eigenschappen van toepassingen][7]

8. Aan het eind van dit hebt u een app AAD met de naam die u eerder als volgt. Dit is uw **AD\_APP\_naam** en noteer deze.  

     ![Naam toep.][8]

9. Klik op de naam van de app en klik op **configureren**.

     ![app configureren][9]

10. Maak een notitie van de CLIENT-ID die wordt gebruikt als **CLIENT\_ID** voor de API aanroept. 

     ![app configureren][10]

11. Ga naar de sectie **sleutels** en voeg een sleutel met bij voorkeur een duur van 2 jaar (vervallen) en klik op **Opslaan**. 

     ![app configureren][11]


12. De waarde die wordt weergegeven voor de sleutel zoals deze nu alleen weergegeven wordt en niet opgeslagen wordt, zodat u niet steeds opnieuw weergegeven onmiddellijk kunt kopiëren. Als u deze verliest hebt u een nieuwe sleutel genereren. Dit is de **CLIENT_SECRET** voor de API-aanroepen. 

     ![app configureren][12]

    >[AZURE.IMPORTANT] Deze sleutel verloopt aan het einde van de duur die u hebt opgegeven, dus zorg ervoor dat deze vernieuwen wanneer u anders de API-verificatie niet meer werkt. U kunt ook verwijderen en opnieuw maken van deze sleutel als u denkt dat het is aangetast.
 
13. Klik op de knop **Weergave EINDPUNTEN** die wordt nu geopend in het dialoogvenster **App eindpunten** . 

    ![][13]

14. Kopiëren vanuit het dialoogvenster eindpunten App het **OAUTH 2.0 TOKEN EINDPUNT**. 

    ![][14]

15. Dit eindpunt is in de volgende vorm waarbij de GUID in de URL is de **TENANT_ID** dus noteer deze: 

        https://login.microsoftonline.com/<GUID>/oauth2/token

16. We zullen nu verder configureren de machtigingen op deze app. U moet dit de [Azure portal](https://portal.azure.com)te openen. 

17. Klik op **Resourcegroepen** en de resourcegroep **Mobile Engagement** vinden.  

    ![][15]

18. De resourcegroep **Mobile Engagement** op en Ga naar de **Instellingen** van de bladeserver hier. 

    ![][16]

19. Klik op **gebruikers** in het blad van de instellingen en klik op **toevoegen** om een gebruiker toevoegen. 

    ![][17]

20. Klik op **Selecteer een rol**

    ![][18]

21. Klik op **eigenaar**

    ![][19]

22. Zoeken naar de naam van uw toepassing **AD\_APP\_naam** in het vak Zoeken. U ziet niet dit hier standaard. Als u het hebt gevonden, selecteert u het en klik op het **selecteren** onder aan het blad. 

    ![][20]

23. Op het blad **Toevoegen toegang** weergegeven als **gebruiker 1, 0 groepen**. Klik op **OK** op deze blade de wijziging te bevestigen. 

    ![][21]

U hebt nu de vereiste AAD configuratie voltooid en worden alle stelt u de API's aanroepen. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



