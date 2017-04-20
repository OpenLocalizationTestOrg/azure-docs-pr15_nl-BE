<properties 
    pageTitle="Hoe developer accounts met Azure Active Directory in Azure API beheer toe te staan" 
    description="Informatie over het machtigen van gebruikers met Azure Active Directory in de beheer-API." 
    services="api-management" 
    documentationCenter="API Management" 
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

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Hoe developer accounts met Azure Active Directory in Azure API beheer toe te staan


## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u toegang tot de developer-portal voor alle gebruikers in één of meer Azure Active Directory. Deze handleiding ook wordt beschreven hoe u groepen met Azure Active Directory-gebruikers beheren door externe groepen met de gebruikers van een Azure Active Directory toe te voegen.

>Als u de stappen in deze handleiding moet u eerst een Azure Active Directory voor het maken van een toepassing hebben.

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Hoe developer accounts met Azure Active Directory machtigen

Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API om te beginnen. Hiermee gaat u naar de portal Management API publisher.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de zelfstudie [aan de slag met Azure API beheer][] .

Klik op **beveiliging** in het menu **API beheer** aan de linkerkant en klik op **Externe identiteiten**.

![Externe identiteiten][api-management-security-external-identities]

Klik op **Azure Active Directory**. Maak een notitie van de **Omleidings-URL** en switch via aan Azure Active Directory in de klassieke Azure-Portal.

![Externe identiteiten][api-management-security-aad-new]

Klik op de knop **toevoegen** als een nieuwe Azure Active Directory-toepassing wilt maken en kies **Mijn organisatie ontwikkelt toepassing toevoegen**.

![Nieuwe Azure Active Directory-toepassing toevoegen][api-management-new-aad-application-menu]

Voer een naam voor de toepassing en klik op de knop volgende Selecteer **webtoepassing en/of Web-API**.

![Nieuwe Azure Active Directory-toepassing][api-management-new-aad-application-1]

Voer de URL aanmelding van de developer-portal voor **aanmelding URL**. In dit voorbeeld wordt de **aanmelding URL** is `https://aad03.portal.current.int-azure-api.net/signin`. 

Geef het standaarddomein of een aangepast domein voor Azure Active Directory voor de **App ID URL**en een unieke tekenreeks toegevoegd aan. In dit voorbeeld wordt het standaarddomein van **https://contoso5api.onmicrosoft.com** gebruikt met het achtervoegsel van **/api** opgegeven.

![Nieuwe eigenschappen voor Azure Active Directory-toepassingen][api-management-new-aad-application-2]

Klik op de knop controleren als u wilt opslaan en de nieuwe toepassing maken en Ga naar het tabblad **configureren** voor het configureren van de nieuwe toepassing.

![Nieuwe Azure Active Directory-toepassing gemaakt][api-management-new-aad-app-created]

Als meerdere Azure Active Directory's voor deze toepassing moet worden gebruikt, klikt u op **Ja** voor een **toepassing met meerdere huurder is**. De standaardinstelling is **Nee**.

![Toepassing is met meerdere huurder][api-management-aad-app-multi-tenant]

De **Omleidings-URL** uit de **Azure Active Directory** -gedeelte van het tabblad **Externe identiteiten** in de portal voor publisher kopiëren en plakken in het tekstvak **URL antwoord** . 

![Antwoord-URL][api-management-aad-reply-url]

Schuif naar de onderkant van het tabblad configureren en selecteer de keuzelijst **Machtigingen** **lezen Active directory-gegevens**te controleren.

![Machtigingen voor een toepassing][api-management-aad-app-permissions]

Selecteer de keuzelijst **Machtigingen voor gemachtigde** en **aanmelding inschakelen**en profielen van gebruikers lezen.

![Overgedragen machtigingen][api-management-aad-delegated-permissions]

>Zie [toegang krijgen tot de Graph API][]voor meer informatie over de toepassing en de overgedragen machtigingen.

De **Client-Id** naar het Klembord kopiëren.

![Client-Id][api-management-aad-app-client-id]

Ga terug naar de uitgever portal en plakken in de **Client-Id** van de toepassingsconfiguratie Azure Active Directory gekopieerd.

![Client-Id][api-management-client-id]

Ga terug naar de Azure Active Directory-configuratie en klik op de **Selecteer duur** vervolgkeuzelijst in de sectie **sleutels** en een interval opgeven. In dit voorbeeld wordt **1 jaar** gebruikt.

![Sleutel][api-management-aad-key-before-save]

Klik op **Opslaan** om de configuratie op te slaan en de sleutel weer te geven. De sleutel naar het Klembord kopiëren.

>Noteer deze sleutel. Zodra u het venster Azure Active Directory-configuratie sluit, kan niet de sleutel worden weergegeven.

![Sleutel][api-management-aad-key-after-save]

Ga terug naar de uitgever portal en de sleutel in het **Geheim van de Client** tekstvak plakken.

![Het geheim van de client][api-management-client-secret]

**Huurder toegestaan** geeft aan welke mappen toegang hebben tot de API's van het exemplaar van de beheer-API. Geef de instanties waaraan u toegang wilt verlenen Azure Active Directory-domeinen. U kunt meerdere domeinen met de nieuwe regels, spaties of komma's van elkaar scheiden.

![Toegestane huurders][api-management-client-allowed-tenants]

Meerdere domeinen kunnen worden opgegeven in de sectie **Huurder toegestaan** . Voordat een gebruiker vanaf een ander domein dan het oorspronkelijke domein waar de aanvraag is geregistreerd aanmelden kunt, moet een globale beheerder van het andere domein toestemming voor de toepassing op de toegang tot Active directory-gegevens. Als u wilt machtigen, moet een globale beheerder aanmelden bij de toepassing en klikt u op **accepteren**. In het volgende voorbeeld `miaoaad.onmicrosoft.com` is toegevoegd aan de **Huurder toegestaan** en een globale beheerder van dat domein is voor de eerste keer aanmelden.

![Machtigingen][api-management-permissions-form]

>Als een niet-globale beheerder probeert in te loggen voordat u machtigingen worden verleend door een globale beheerder, de aanmeldingspoging mislukt en een fout wordt weergegeven.

Als de gewenste configuratie is opgegeven, klikt u op **Opslaan**.

![Opslaan][api-management-client-allowed-tenants-save]

Zodra de wijzigingen zijn opgeslagen, kunnen de gebruikers in het opgegeven Azure Active Directory door de stappen in het [aanmelden bij de Developer-portal met een Azure Active Directory-account][]zich aanmelden bij de Developer-portal.

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Het toevoegen van een externe Azure Active Directory-groep

Na het inschakelen van toegang voor gebruikers in een Azure Active Directory, kunt u de Azure Active Directory-groepen toevoegen in het beheer van de API voor het beheren van de associatie van de ontwikkelaars in de groep met de gewenste producten gemakkelijker.

> Voordat u een externe Azure Active Directory-groep configureren, worden Azure Active Directory eerst geconfigureerd op het tabblad identiteit door de procedure in de vorige sectie. 

Externe Azure Active Directory-groepen worden toegevoegd via het tabblad **zichtbaarheid** van het product waarvoor u toegang verleent aan de groep wilt maken. Klik op **producten**en klik op de naam van het gewenste product.

![Product configureren][api-management-configure-product]

Overschakelen naar het tabblad **zichtbaarheid** en klik op **Groepen toevoegen uit Azure Active Directory**.

![Groepen toevoegen][api-management-add-groups]

De **Huurder van Azure Active Directory** selecteert in de vervolgkeuzelijst en typ vervolgens de naam van de gewenste groep in de **groepen** worden toegevoegd in het tekstvak.

![Selecteer groep][api-management-select-group]

De naam van deze groep vindt u in de lijst **groepen** voor uw Azure Active Directory, zoals in het volgende voorbeeld wordt getoond.

![Lijst van Azure Active Directory-groepen][api-management-aad-groups-list]

Klik op **toevoegen** om te valideren de naam van de groep en voeg de groep. In dit voorbeeld de **Ontwikkelaars van Contoso 5** wordt externe groep toegevoegd. 

![Groep toegevoegd][api-management-aad-group-added]

Klik op **Opslaan** om de nieuwe groep selectie opslaan.

Zodra van de ene Azure Azure Active Directory-groep is geconfigureerd, is beschikbaar op het tabblad **zichtbaar** voor de andere producten in het exemplaar van de beheer-API worden gecontroleerd.

Als u wilt controleren en de eigenschappen voor externe groepen configureren nadat ze zijn toegevoegd, klikt u op de naam van de groep op het tabblad **groepen** .

![Groepen beheren][api-management-groups]

Hier kunt u de **naam** en de **Beschrijving** van de groep bewerken.

![Groep bewerken][api-management-edit-group]

Gebruikers uit de geconfigureerde Azure Active Directory kunnen zich aanmelden bij de Developer-portal en de weergave en zich abonneren op de groepen waarvoor ze zichtbaarheid hebben door de instructies in de volgende sectie.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Hoe kunt u zich aanmelden bij de Developer-portal met een Azure Active Directory-account

Als u wilt zich aanmelden bij de Developer-portal met een Azure Active Directory-account geconfigureerd in de voorgaande secties, open een nieuw browservenster met de **URL voor aanmelding** van de configuratie van de Active Directory-toepassing en **Azure Active Directory**op.

![Developer-Portal][api-management-dev-portal-signin]

Voer de referenties van een van de gebruikers in uw Azure Active Directory en klik op **aanmelden**.

![Aanmelden][api-management-aad-signin]

Wordt u mogelijk gevraagd met een registratieformulier als aanvullende informatie vereist is. Vul het registratieformulier en klik op **aanmelden**.

![Registratie][api-management-complete-registration]

De gebruiker is nu aangemeld bij de developer-portal voor uw exemplaar van de service Management API.

![Inschrijving voltooid][api-management-registration-complete]



[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Aan de slag met Azure API beheer]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Toegang tot de API van de grafiek]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in op de Developer-portal met een Azure Active Directory-account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

