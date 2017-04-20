<properties 
    pageTitle="Het gebruik van eigenschappen in Azure API beleid" 
    description="Informatie over het gebruik van eigenschappen in Azure API beleid." 
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


# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Het gebruik van eigenschappen in Azure API beleid

Beleidsregels voor informatiebeheer API zijn een krachtige mogelijkheden van het systeem waarmee de uitgever van het gedrag van de API via configuratie wijzigen. Beleid is een verzameling instructies die worden uitgevoerd achter elkaar op de aanvraag of antwoord van een API. Beleidslijnen kunnen worden samengesteld met behulp van letterlijke waarden, beleid, expressies en eigenschappen. 

Elk exemplaar van de service Management API heeft een sleutel/waarde-paren die algemeen in het exemplaar van de zijn properties-collectie. Deze eigenschappen kunnen worden gebruikt voor het beheren van constante string-waarden voor alle API-configuratie en -beleid. Elke eigenschap heeft de volgende kenmerken.


| Kenmerk | Type            | Beschrijving                                                                                             |
|-----------|-----------------|---------------------------------------------------------------------------------------------------------|
| Naam      | tekenreeks          | De naam van de eigenschap. Mogelijk bevatten alleen letters, cijfers, periode, streepjes en onderstrepingstekens. |
| Waarde     | tekenreeks          | De waarde van de eigenschap. Niet kan leeg zijn of alleen uit spaties bestaan.                           |
| Geheim    | Boole-waarde         | Bepaalt of de waarde een geheim is, en dan niet moet worden gecodeerd.                                |
| Tags      | matrix van string | Optioneel tags die de geleverd kan worden gebruikt voor het filteren van de lijst met eigenschappen.                               |

Eigenschappen worden ingesteld in de publisher-portal op het tabblad **Eigenschappen** . In het volgende voorbeeld worden drie eigenschappen geconfigureerd.

![Eigenschappen][api-management-properties]

Letterlijke tekenreeksen en [beleid expressies](https://msdn.microsoft.com/library/azure/dn910913.aspx)bevatten eigenschapwaarden. De volgende tabel ziet u de vorige drie monster eigenschappen en de bijbehorende kenmerken. De waarde van `ExpressionProperty` is een beleidsexpressie die een tekenreeks retourneert met de huidige datum en tijd. De eigenschap `ContosoHeaderValue` is gemarkeerd als een geheim, zodat de waarde ervan niet wordt weergegeven.

| Naam               | Waarde                      | Geheim | Tags    |
|--------------------|----------------------------|--------|---------|
| ContosoHeader      | TrackingId                 | False  | Contoso |
| ContosoHeaderValue | ••••••••••••••••••••••     | True   | Contoso |
| ExpressionProperty | @(DateTime.Now.ToString()) | False  |         |

## <a name="to-use-a-property"></a>Met behulp van een eigenschap.

U gebruikt een eigenschap in een beleid te plaatsen binnen een paar dubbele accolades, zoals de naam van de eigenschap `{{ContosoHeader}}`, zoals in het volgende voorbeeld wordt weergegeven.

    <set-header name="{{ContosoHeader}}" exists-action="override">
      <value>{{ContosoHeaderValue}}</value>
    </set-header>

In dit voorbeeld wordt `ContosoHeader` wordt gebruikt als de naam van een kop in een `set-header` beleid, en `ContosoHeaderValue` wordt gebruikt als de waarde van die header. Als dit beleid wordt geëvalueerd tijdens een aanvraag of antwoord op de gateway API Management `{{ContosoHeader}}` en `{{ContosoHeaderValue}}` worden vervangen door hun respectieve eigenschapswaarden.

Eigenschappen kunnen worden gebruikt als volledige kenmerk of de waarden van het element zoals in het vorige voorbeeld, maar ze kunnen ook worden ingevoegd in of in combinatie met een deel van de expressie van een letterlijke tekst zoals in het volgende voorbeeld wordt getoond:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Eigenschappen bevatten ook expressies beleid. In het volgende voorbeeld wordt de `ExpressionProperty` wordt gebruikt.

    <set-header name="CustomHeader" exists-action="override">
        <value>{{ExpressionProperty}}</value>
    </set-header>

Wanneer dit beleid wordt beoordeeld, `{{ExpressionProperty}}` wordt vervangen door de bijbehorende waarde: `@(DateTime.Now.ToString())`. Omdat de waarde een beleidsexpressie voor een is, wordt de expressie geëvalueerd en het beleid wordt doorgegaan met de uitvoering ervan.

Dit out kunt in de developer-portal u testen door het aanroepen van een bewerking met een beleid met de eigenschappen in het bereik. In het volgende voorbeeld wordt een bewerking wordt aangeroepen met de twee vorige voorbeeld `set-header` beleid met eigenschappen. Houd er rekening mee dat het antwoord bevat twee aangepaste headers die zijn geconfigureerd met een beleid met eigenschappen.

![Developer-portal][api-management-send-results]

Als u de [API-inspecteur traceren](api-management-howto-api-inspector.md) voor een gesprek met de twee vorige sample-beleid met de eigenschappen bekijkt, ziet u de twee `set-header` beleid met de waarden van de eigenschappen en de evaluatie van beleid de expressie voor de eigenschap die de beleidsexpressie ingevoegd.

![API-inspecteur traceren][api-management-api-inspector-trace]

Houd er rekening mee dat terwijl eigenschapswaarden beleid expressies bevatten kunnen, waarden geen andere eigenschappen. Als u tekst met een eigenschapverwijzing wordt gebruikt voor de waarde van een eigenschap, zoals `Property value text {{MyProperty}}`, die de eigenschapverwijzing won't worden vervangen en worden opgenomen als onderdeel van de waarde van de eigenschap.

## <a name="to-create-a-property"></a>Een eigenschap maken

Klik op **eigenschap toevoegen** om een eigenschap op het tabblad **Eigenschappen** .

![Eigenschap toevoegen][api-management-properties-add-property-menu]

**Naam** en **waarde** worden de vereiste waarden. Als de waarde van deze eigenschap is een geheim, schakel het selectievakje **Dit is een geheim** in. Voer een of meer optionele tags om te helpen bij het organiseren van uw eigenschappen en klik op **Opslaan**.

![Eigenschap toevoegen][api-management-properties-add-property]

Wanneer een nieuwe eigenschap wordt opgeslagen, **Zoeken eigenschap** textbox wordt gevuld met de naam van de nieuwe eigenschap en de nieuwe eigenschap wordt weergegeven. Om alle eigenschappen weergeven, schakelt u het tekstvak van de **eigenschap voor zoeken** en druk op enter.

![Eigenschappen][api-management-properties-property-saved]

Zie voor informatie over het maken van een eigenschap met de REST API's [maken een eigenschap met de REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Een eigenschap bewerken

Wilt u een eigenschap wilt bewerken, klikt u op **bewerken** naast de eigenschap die u wilt bewerken.

![Eigenschap bewerken][api-management-properties-edit]

Breng de gewenste wijzigingen aan en klik op **Opslaan**. Als u de naam van de eigenschap wijzigt, wordt beleid dat verwijst naar de eigenschap worden automatisch bijgewerkt voor het gebruik van de nieuwe naam.

![Eigenschap bewerken][api-management-properties-edit-property]

Zie [een eigenschap met de REST API bewerken](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch)voor meer informatie over het bewerken van een eigenschap met de REST API.

## <a name="to-delete-a-property"></a>Een eigenschap verwijderen

Als u een eigenschap wilt verwijderen, klikt u op **verwijderen** naast de eigenschap te verwijderen.

![Eigenschap verwijderen][api-management-properties-delete]

Klik op **Ja, het verwijderen** te bevestigen.

![Verwijderen bevestigen][api-management-delete-confirm]

>[AZURE.IMPORTANT] Als de eigenschap wordt verwezen met het beleid, kunt u zich niet met succes verwijderd totdat u de eigenschap verwijderen uit alle beleidsregels die gebruikmaken van deze.

Zie [een eigenschap met de REST API verwijderen](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete)voor meer informatie over het verwijderen van een eigenschap met de REST API.

## <a name="to-search-and-filter-properties"></a>Om te zoeken en filteren eigenschappen

Het tabblad **Eigenschappen** bevat de zoek- en filtermogelijkheden voor het beheren van de eigenschappen. Voer een zoekterm in het tekstvak **Zoeken-eigenschap** om de eigenschap naam van de eigenschap. Om alle eigenschappen weergeven, schakelt u het tekstvak van de **eigenschap voor zoeken** en druk op enter.

![Zoeken][api-management-properties-search]

Om de lijst met velden filteren op labelwaarden, voer een of meer tags in de textbox **filteren op tags** . Om alle eigenschappen weergeven, schakel het **filteren op tags** textbox en druk op enter.

![Filter][api-management-properties-filter]

## <a name="next-steps"></a>Volgende stappen

-   Meer informatie over het werken met beleid
    -   [Beleid in API-beheer](api-management-howto-policies.md)
    -   [Beleidsverwijzing](https://msdn.microsoft.com/library/azure/dn894081.aspx)
    -   [Beleid voor expressies](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Bekijk een video-overzicht

> [AZURE.VIDEO use-properties-in-policies]

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

