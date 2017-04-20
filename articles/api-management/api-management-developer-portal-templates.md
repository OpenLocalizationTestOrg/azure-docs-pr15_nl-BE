<properties 
    pageTitle="Het aanpassen van het beheer van Azure API developer-portal met behulp van sjablonen | Microsoft Azure" 
    description="Informatie over het aanpassen van het beheer van Azure API developer-portal met behulp van sjablonen." 
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


# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Het aanpassen van het beheer van Azure API developer-portal met behulp van sjablonen

Azure API beheer biedt verschillende functies om te kunnen beheerders voor het [aanpassen van het uiterlijk van de portal voor ontwikkelaars](api-management-customize-portal.md), evenals aanpassen van de inhoud van de developer portal-pagina's met een verzameling sjablonen die de inhoud van de pagina's zelf te configureren. [DotLiquid](http://dotliquidmarkup.org/) -syntaxis en een opgegeven set bronnen met gelokaliseerde tekenreeksen, pictogrammen en paginabesturingselementen gebruikt, hebt u grote flexibiliteit voor het configureren van de inhoud van de pagina's naar wens met behulp van deze sjablonen.

## <a name="developer-portal-templates-overview"></a>Developer portal-sjablonen-overzicht

Developer portal-sjablonen worden beheerd in de developer-portal door beheerders van het exemplaar van de beheer-API. Ontwikkelaar sjablonen beheren, gaat u naar de API Management service-exemplaar in de klassieke Azure-Portal en klikt u op **Bladeren**.

![Developer-portal][api-management-browse]

Bent u al in de portal voor publisher, kunt u developer-portal kunt openen door te klikken op **Developer-portal**.

![Developer portal-menu][api-management-developer-portal-menu]

Klik op het pictogram aanpassen aan de linkerkant om het menu aanpassen weer te geven voor toegang tot de developer portal-sjablonen, en klikt u op **sjablonen**.

![Developer portal-sjablonen][api-management-customize-menu]

De lijst met sjablonen wordt opgedeeld in verschillende categorieën die betrekking hebben op de verschillende pagina's in de developer-portal-sjablonen weergegeven. Elke sjabloon is anders, maar de stappen voor het bewerken en publiceren van de wijzigingen zijn hetzelfde. Als u wilt bewerken in een sjabloon, klikt u op de naam van de sjabloon.

![Developer portal-sjablonen][api-management-templates-menu]

Een sjabloon te klikken gaat u naar de developer portal-pagina dat kan worden aangepast door die sjabloon. In dit voorbeeld is de **lijst met producten** van de wordt sjabloon weergegeven. De **productlijst** sjabloon bepaalt het gebied op het scherm aangegeven door de rode rechthoek. 

![Sjabloon voor lijst van producten][api-management-developer-portal-templates-overview]

Bepaalde sjablonen, zoals de sjablonen **Gebruikersprofiel** aanpassen voor verschillende delen van dezelfde pagina. 

![Profiel van gebruikerssjablonen][api-management-user-profile-templates]

De editor voor elke developer portal-sjabloon bevat twee secties weergegeven onder aan de pagina. De linkerkant weergegeven in het deelvenster bewerken van de sjabloon en de rechterkant wordt het gegevensmodel van de sjabloon. 

Het deelvenster bewerken sjabloon bevat de opmaak die u het uiterlijk en gedrag van de desbetreffende pagina in de portal voor ontwikkelaars bepaalt. De opmaak van de sjabloon gebruikt de syntaxis [DotLiquid](http://dotliquidmarkup.org/) . Een populaire editor voor DotLiquid is [DotLiquid voor ontwerpers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Eventuele wijzigingen in de sjabloon tijdens het bewerken in real-time worden weergegeven in de browser, maar zijn niet zichtbaar voor uw klanten totdat u [Opslaan](#to-save-a-template) en [publiceren](#to-publish-a-template) de sjabloon.

![Sjabloonopmaak][api-management-template]

Het deelvenster **van de sjabloon** biedt een handleiding voor het gegevensmodel van de entiteiten die beschikbaar voor gebruik in een bepaalde sjabloon zijn. Deze handleiding biedt door de dynamische gegevens die worden weergegeven in de developer-portal weer te geven. U kunt de deelvensters sjabloon uitvouwen door te klikken op de rechthoek in de rechterbovenhoek van het deelvenster **van de sjabloon** .

![Sjabloon-gegevensmodel][api-management-template-data]

In het vorige voorbeeld zijn er twee producten die worden weergegeven in de portal voor ontwikkelaars die zijn opgehaald uit de gegevens die worden weergegeven in het deelvenster **van de sjabloon** , zoals in het volgende voorbeeld wordt getoond.

    {
        "Paging": {
            "Page": 1,
            "PageSize": 10,
            "TotalItemCount": 2,
            "ShowAll": false,
            "PageCount": 1
        },
        "Filtering": {
            "Pattern": null,
            "Placeholder": "Search products"
        },
        "Products": [
            {
                "Id": "56ec64c380ed850042060001",
                "Title": "Starter",
                "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
                "Terms": "",
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            },
            {
                "Id": "56ec64c380ed850042060002",
                "Title": "Unlimited",
                "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
                "Terms": null,
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            }
        ]
    }

De opmaak van de sjabloon voor **lijst met producten** verwerkt de gegevens voor de gewenste uitvoer door het doorlopen van de collectie van de producten weer te geven informatie en een koppeling naar elk afzonderlijk product. Opmerking de `<search-control>` en `<page-control>` -elementen in de opmaak. Deze bepalen de weergave van de zoek- en besturingselementen op de pagina van het wisselbestand. `ProductsStrings|PageTitleProducts`verwijzing is een gelokaliseerde tekenreeks met de `h2` de koptekst voor de pagina. Voor een overzicht van de tekenreeks resources, paginabesturingselementen en pictogrammen voor gebruik in developer portal-sjablonen, Zie [API beheer portal-sjablonen: referentie voor ontwikkelaars](https://msdn.microsoft.com/library/azure/mt697540.aspx).

    <search-control></search-control>
    <div class="row">
        <div class="col-md-9">
            <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
        {% if products.size > 0 %}
        <ul class="list-unstyled">
        {% for product in products %}
            <li>
                <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
                {{product.description}}
            </li>   
        {% endfor %}
        </ul>
        <paging-control></paging-control>
        {% else %}
        {% localized "CommonResources|NoItemsToDisplay" %}
        {% endif %}
        </div>
    </div>

## <a name="to-save-a-template"></a>Een sjabloon opslaan

Als u een sjabloon wilt opslaan, klikt u op opslaan in de sjablooneditor.

![Sjabloon opslaan][api-management-save-template]

Opgeslagen wijzigingen zijn niet in de developer-portal leven nadat ze zijn gepubliceerd.

## <a name="to-publish-a-template"></a>Voor het publiceren van een sjabloon

Opgeslagen sjablonen kunnen worden gepubliceerd, hetzij afzonderlijk of allemaal tegelijk. Klik op publiceren als u wilt publiceren een afzonderlijke template, in de sjablooneditor.

![Sjabloon publiceren][api-management-publish-template]

Klik op **Ja** om te bevestigen en te zorgen dat de sjabloon live op de developer-portal.

![Bevestig publiceren][api-management-publish-template-confirm]

Klik op **publiceren** in de lijst met sjablonen voor het publiceren van alle momenteel niet-gepubliceerde versies. Niet-gepubliceerde sjablonen worden aangeduid met een sterretje achter de sjabloonnaam. In dit voorbeeld wordt worden de **lijst met producten** en **Product** -sjablonen gepubliceerd.

![Sjablonen publiceren][api-management-publish-templates]

Klik op **de aanpassingen publiceren** om te bevestigen.

![Bevestig publiceren][api-management-publish-customizations]

Nieuwe sjablonen zijn effectief in het developer-portal.

## <a name="to-revert-a-template-to-the-previous-version"></a>Een sjabloon naar de vorige versie herstellen

Als u een sjabloon naar de vorige gepubliceerde versie, klikt u op herstellen in de sjablooneditor.

![Sjabloon herstellen][api-management-revert-template]

Klik op **Ja** om te bevestigen.

![Bevestigen][api-management-revert-template-confirm]

De eerder gepubliceerde versie van een sjabloon is live in de developer-portal zodra de herstelbewerking voltooid is.

## <a name="to-restore-a-template-to-the-default-version"></a>Een sjabloon met de standaardversie herstellen

Sjablonen herstellen naar de standaardversie is een proces. Eerst worden de sjablonen moeten worden hersteld en vervolgens de herstelde versies moeten worden gepubliceerd.

Klik op herstellen in de sjablooneditor te herstellen van een bepaalde sjabloon naar de standaardversie.

![Sjabloon herstellen][api-management-reset-template]

Klik op **Ja** om te bevestigen.

![Bevestigen][api-management-reset-template-confirm]

Als alle sjablonen weer in hun standaardversies, klikt u op **herstellen standaardsjablonen** op de lijst met sjablonen.

![Sjablonen herstellen][api-management-restore-templates]

De herstelde sjablonen moeten vervolgens worden gepubliceerd afzonderlijk of allemaal tegelijk met de stappen in [een sjabloon publiceren](#to-publish-a-template).

## <a name="developer-portal-templates-reference"></a>Portal-sjablonen: referentie voor ontwikkelaars

Zie [API beheer portal-sjablonen: referentie voor ontwikkelaars](https://msdn.microsoft.com/library/azure/mt697540.aspx)voor de naslaginformatie voor ontwikkelaars portal-sjablonen, tekenreeksen, pictogrammen en paginabesturingselementen.

## <a name="watch-a-video-overview"></a>Bekijk een video-overzicht

Bekijk de volgende video om te zien hoe een discussiebord en beoordelingen toevoegen aan de API en werking pagina's in de portal voor ontwikkelaars met behulp van sjablonen.

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







