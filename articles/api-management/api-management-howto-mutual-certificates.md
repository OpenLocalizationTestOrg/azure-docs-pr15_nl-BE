<properties 
    pageTitle="Het beveiligen van back-end services client met certificaatverificatie in Azure API beheer" 
    description="Informatie over het beveiligen van back-end services met behulp van verificatie van clientcertificaten in Azure API beheer." 
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

# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Het beveiligen van back-end services client met certificaatverificatie in Azure API beheer

Beheer-API biedt de mogelijkheid om veilige toegang tot de back-end-service van een API met behulp van clientcertificaten. Deze handleiding bevat het beheren van certificaten in de API uitgever portal en het configureren van een API voor het gebruik van een certificaat voor de back-end-service.

Voor meer informatie over het beheren van certificaten met behulp van de API beheer REST API Zie [API Azure REST API certificaat entiteit][].

## <a name="prerequisites"> </a>Voorwaarden

Deze handleiding wordt beschreven hoe u uw exemplaar van de service Management API voor het gebruik van verificatie van clientcertificaten voor de back-end-service voor een API configureren. Voordat u de stappen in dit onderwerp, moet u uw back-end-service is geconfigureerd voor verificatie van clientcertificaten ([configureren van certificaatverificatie in Azure WebSites verwijzen naar dit artikel][]) en beschikken over het certificaat en het wachtwoord voor het certificaat voor het uploaden in de portal Management API publisher.

## <a name="step1"> </a>Een clientcertificaat uploaden

Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API om te beginnen. Hiermee gaat u naar de portal Management API publisher.

![API uitgever portal][api-management-management-console]

>Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de zelfstudie [aan de slag met Azure API beheer][] .

Klik op **beveiliging** in het menu **API beheer** aan de linkerkant en klik op **clientcertificaten**.

![Clientcertificaten][api-management-security-client-certificates]

Als u wilt een nieuw certificaat uploaden, klikt u op **certificaat uploaden**.

![Certificaat uploaden][api-management-upload-certificate]

Ga naar uw certificaat en voer vervolgens het wachtwoord voor het certificaat.

>Het certificaat moet in de **PFX** -indeling. Zelf-ondertekende certificaten zijn toegestaan.

![Certificaat uploaden][api-management-upload-certificate-form]

Klik op **uploaden** om het certificaat te uploaden.

>Het wachtwoord voor het certificaat wordt gevalideerd op dit moment. Als het niet juist wordt een foutbericht weergegeven.

![Certificaat geüpload][api-management-certificate-uploaded]

Nadat het certificaat is geüpload, deze wordt weergegeven op het tabblad **certificaten** . Als er meerdere certificaten, moet u een notitie van het onderwerp of de laatste vier tekens van de vingerafdruk, die worden gebruikt voor het certificaat te selecteren bij het configureren van een API voor het gebruik van certificaten, zoals besproken in de volgende sectie voor [een API te gebruiken van een clientcertificaat voor verificatie gateway configureren][] .

>Volg de stappen in deze Veelgestelde vragen over het [item](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)uit te schakelen keten certificaatverificatie gebruikt, bijvoorbeeld een zelf-ondertekend certificaat.

## <a name="step1a"> </a>Een certificaat verwijderen

Als u wilt een certificaat wilt verwijderen, klikt u op **verwijderen** naast het gewenste certificaat.

![Certificaat verwijderen][api-management-certificate-delete]

Klik op **Ja, het verwijderen** te bevestigen.

![Verwijderen bevestigen][api-management-confirm-delete]

Als het certificaat gebruikt door een API, is wordt een waarschuwing wordt weergegeven. Als het certificaat wilt verwijderen moet u het certificaat eerst verwijderen uit alle API's die zijn geconfigureerd voor het gebruik.

![Verwijderen bevestigen][api-management-confirm-delete-policy]

## <a name="step2"> </a>Een API voor het gebruik van een clientcertificaat voor verificatie gateway configureren

Klik op **API's** in het menu **API beheer** aan de linkerkant en klik op het tabblad **beveiliging** klikt u op de naam van de gewenste API.

![API-beveiliging][api-management-api-security]

**Clientcertificaten** selecteert in de vervolgkeuzelijst **met referenties** .

![Clientcertificaten][api-management-mutual-certificates]

Selecteer het gewenste certificaat in de vervolgkeuzelijst van het **clientcertificaat** . Als er meerdere certificaten kunt u het onderwerp of de laatste vier tekens van de vingerafdruk zoals beschreven in de vorige sectie om te bepalen van het juiste certificaat kijken.

![Certificaat selecteren][api-management-select-certificate]

Klik op **Opslaan** om de wijziging in de configuratie opslaan de API.

>Deze wijziging is onmiddellijk van kracht en bewerkingen van deze API aanroepen van het certificaat verifiëren op de back-end-server wordt gebruikt.

![API wijzigingen opslaan][api-management-save-api]

>Wanneer een certificaat voor de verificatie van de gateway voor de back-end-service van een API is opgegeven, wordt het onderdeel van het beleid voor deze API en kunnen worden weergegeven in de Groepsbeleid-editor.

![Certificaatbeleid][api-management-certificate-policy]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over andere manieren voor het beveiligen van uw back-end-service, zoals HTTP basic of het gedeelde geheim verificatie, de volgende video.

> [AZURE.VIDEO last-mile-security]

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Aan de slag met Azure API beheer]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance

[Azure API beheer REST API certificaat entiteit]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[configureren van certificaatverificatie in Azure WebSites verwijzen naar dit artikel]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Een API voor het gebruik van een clientcertificaat voor verificatie gateway configureren]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps


 
