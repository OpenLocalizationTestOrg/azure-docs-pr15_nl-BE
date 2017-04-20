<properties 
    pageTitle="Het maken en publiceren van een product in Azure API beheer" 
    description="Informatie over het maken en publiceren van producten in Azure API beheer." 
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

# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Het maken en publiceren van een product in Azure API beheer

Beheer van Azure API bevat een product API's voor een of meer, alsmede een quota voor en de gebruiksvoorwaarden. Als een product is gepubliceerd, kunnen ontwikkelaars abonneren op het product en beginnen met het gebruik van de API's van het product. Het onderwerp bevat richtlijnen voor het maken van een product, een API toe te voegen en te publiceren voor ontwikkelaars.

## <a name="create-product"> </a>Een product maken

Bewerkingen worden toegevoegd en geconfigureerd voor een API in de publisher-portal. Klik op **beheren** in de klassieke Azure-Portal voor uw service Management API voor toegang tot de portal publisher.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de zelfstudie [aan de slag met Azure API beheer][] .

Klik op **producten** in het menu aan de linkerkant op de pagina **producten** weer te geven en klik op **Product toevoegen**.

![Producten][api-management-products]

![Nieuw product][api-management-add-new-product]

Voer een beschrijvende naam voor het product in het veld **naam** en een beschrijving van het product in het veld **Omschrijving** .

Producten in de beheer-API kunnen worden **geopend** of **beveiligd**. Beschermde producten moeten geabonneerd op voordat ze kunnen worden gebruikt, terwijl deze was geopend producten kunnen worden gebruikt zonder een abonnement. Controleer de **abonnement nodig** om een beveiligde product waarvoor een abonnement te maken. Dit is de standaardinstelling.

**Abonnement goedkeuring vereist** als u wilt dat een beheerder om te bekijken en accepteren of afwijzen abonnement pogingen om dit product te controleren. Als het selectievakje uitgeschakeld is, worden pogingen van abonnement automatisch goedgekeurd. Zie voor meer informatie over abonnementen, [abonnees van een product bekijken][].

Als u wilt toestaan dat ontwikkelaars zich abonneert meerdere keren op het product, het selectievakje **toestaan dat meerdere abonnementen** . Als dit selectievakje niet is ingeschakeld, kan slechts één keer aan het product zich abonneren elke developer-account.

![Onbeperkt meerdere abonnementen][api-management-unlimited-multiple-subscriptions]

Alleen het aantal abonnementen voor meerdere gelijktijdige het selectievakje **Beperk het aantal gelijktijdige abonnementen op** en voer de limiet van het abonnement. In het volgende voorbeeld worden gelijktijdige abonnementen beperkt tot vier keer per developer-account.

![Vier verschillende abonnementen][api-management-four-multiple-subscriptions]

Nadat alle nieuwe productopties zijn geconfigureerd, klikt u op **Opslaan** om het nieuwe product te maken.

![Producten][api-management-products-page]

>Standaard worden nieuwe producten worden niet gepubliceerd en zijn alleen zichtbaar voor de groep **Administrators** .

Configureren van een product, klikt u op de naam van het product op het tabblad **producten** .

## <a name="add-apis"> </a>API's aan een product toevoegen

De pagina **producten** bevat vier koppelingen voor configuratie: **Overzicht**, **Instellingen**, **zichtbaarheid**en **abonnees**. Het tabblad **Overzicht** is waar u kunt API's toevoegen en publiceren of een product ongedaan.

![Samenvatting][api-management-new-product-summary]

Alvorens uw product moet u een of meer API's toevoegen. Hiertoe klikt u op **API toevoegen aan het product**.

![API's toevoegen][api-management-add-apis-to-product]

Selecteer de gewenste API's en klik op **Opslaan**.

## <a name="add-description"> </a>Beschrijvende informatie aan een product toevoegen

Het tabblad **Instellingen** kunt u gedetailleerde informatie over het product, zoals het doel ervan en de API's toegang tot biedt andere nuttige informatie. De inhoud is gericht op de ontwikkelaars die aanroepen van de API en kunnen worden geschreven in tekst zonder opmaak of HTML-opmaakcodes.

![Product-instellingen][api-management-product-settings]

Controleer de **abonnement nodig** om een beveiligde product waarvoor een abonnement moet worden gebruikt te maken of schakel het selectievakje uit om een open product dat kan worden aangeroepen zonder een abonnement te maken.

Selecteer **abonnement goedkeuring vereist** als u wilt dat alle aanvragen voor het abonnement van product handmatig goedkeuren. Standaard worden alle abonnementen van het product automatisch toegekend.

Als u wilt toestaan dat ontwikkelaars zich abonneert meerdere keren op het product, het selectievakje **toestaan dat meerdere abonnementen** en eventueel een limiet opgeven. Als dit selectievakje niet is ingeschakeld, kan slechts één keer aan het product zich abonneren elke developer-account.

Vul eventueel in de **voorwaarden voor het gebruik** met een beschrijving van de gebruiksvoorwaarden voor het product welke abonnees accepteren moeten, voordat het product gebruiken.

## <a name="publish-product"> </a>Een product publiceren

Voordat de API's in een product kan worden aangeroepen, moet het product worden gepubliceerd. Op het tabblad **Samenvatting** van het product, klikt u op **publiceren**en klik op **Ja, deze publiceren** om te bevestigen. Een eerder gepubliceerde product als privé wilt maken, klikt u op **publicatie ongedaan maken**.

![Product publiceren][api-management-publish-product]

## <a name="make-visible"> </a>Een product zichtbaar maken voor ontwikkelaars

Het tabblad **zichtbaarheid** kunt u kiezen welke functies het product zien op de portal voor ontwikkelaars en zich abonneren op het product zijn.

![Zichtbaarheid van product][api-management-product-visiblity]

Als u wilt in- of uitschakelen van de zichtbaarheid van een product voor de ontwikkelaars in een groep, of schakel het selectievakje naast de groep en klik op **Opslaan**.

>Zie voor meer informatie, [het maken en het gebruik van groepen voor het beheren van accounts in Azure API beheer developer][].

## <a name="view-subscribers"> </a>Abonnees van een product bekijken

Het tabblad **abonnees** worden de ontwikkelaars die zich hebben geabonneerd op het product. De details en de instellingen voor elke ontwikkelaar kunnen worden bekeken door te klikken op de naam van de ontwikkelaar. In dit voorbeeld hebben nog geen ontwikkelaars geabonneerd op het product.

![Ontwikkelaars][api-management-developer-list]

## <a name="next-steps"> </a>Volgende stappen

Nadat u de gewenste API's worden toegevoegd en het product is gepubliceerd, ontwikkelaars kunnen abonneren op het product en beginnen met de API's aanroepen. Zie voor een zelfstudie die deze items als de configuratie van geavanceerde product toont [hoe maken en configureren van instellingen voor geavanceerde product in Azure API beheer][].

Zie de volgende video voor meer informatie over het werken met producten.

> [AZURE.VIDEO using-products]

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[Abonnees van een product weergeven]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Aan de slag met Azure API beheer]: api-management-get-started.md
[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[Het maken en groepen gebruiken voor het beheren van accounts in Azure API beheer developer]: api-management-howto-create-groups.md
[Hoe maken en configureren van instellingen voor geavanceerde product in Azure API beheer]: api-management-howto-product-with-rules.md 