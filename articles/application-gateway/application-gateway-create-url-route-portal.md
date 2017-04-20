<properties
   pageTitle="Een regel op basis van het pad voor een toepassingsgateway maken met behulp van de portal | Microsoft Azure"
   description="Informatie over het maken van een regel op basis van het pad voor een toepassingsgateway met behulp van de portal"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Een regel op basis van het pad voor een toepassingsgateway maken met behulp van de portal

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-url-route-portal.md)
- [Azure PowerShell voor bronbeheer](application-gateway-create-url-route-arm-ps.md)

URL-pad gebaseerde routering kunt u routes op basis van het URL-pad van een HTTP-aanvraag koppelen. Het wordt gecontroleerd of er een route naar een back-end-toepassingen voor de lijsten URL in Application Gateway geconfigureerd is en het netwerkverkeer te verzenden naar de gedefinieerde groep met back-end. Vaak gebruikt voor routering op basis van een URL worden geladen aanvragen voor verschillende typen inhoud aan verschillende back-end-server-toepassingen.

Op URL gebaseerde routering wordt een nieuw type application gateway geïntroduceerd. Toepassingsgateway heeft twee soorten: basis en op basis van het pad. Basisregel type round robin-service voor de back-end-toepassingen terwijl de regels op basis van het pad naast de round-robin distributie, ook rekening pad patroon van de aanvraag-URL bij het kiezen van de back-end-toepassingen.

## <a name="scenario"></a>Scenario

In het volgende scenario gaat bij het maken van een regel op basis van het pad in een bestaande toepassingsgateway.
Het scenario wordt ervan uitgegaan dat u de stappen voor het [maken van een toepassingsgateway](application-gateway-create-gateway-portal.md)al hebt gevolgd.

![URL-route][scenario]

## <a name="createrule"></a>De regel op basis van een pad maken

Een regel op basis van het pad moet een eigen listener, voordat het maken van de regel, moet u controleren of er een listener beschikbaar om te gebruiken.

### <a name="step-1"></a>Stap 1

Ga naar http://portal.azure.com en selecteer een bestaande toepassingsgateway. Klik op **regels**

![Gateway: overzicht][1]

### <a name="step-2"></a>Stap 2

Klik op de knop **op basis van het pad naar** een nieuwe regel op basis van een pad toe.

### <a name="step-3"></a>Stap 3

Het **bestandspad gebaseerde regel toevoegen** blade bevat twee secties. Het eerste gedeelte is waar u de listener, de naam van de regel en de standaardinstellingen van het pad gedefinieerd. De standaardinstellingen van het pad zijn voor routes die niet onder de aangepaste route op basis van het pad vallen. Het tweede gedeelte van het blad **toevoegen op basis van het pad regel** is waarin u de regels op basis van het pad zelf definiëren.

**Basisinstellingen**

- **Naam** - dit is een beschrijvende naam voor de regel die in de portal toegankelijk is.
- **Listener** - dit is de listener die wordt gebruikt voor de regel.
- **Standaard back-end-toepassingen** - met deze instelling is de instelling die de back-end moet worden gebruikt voor de standaardregel definieert
- **Standaard-HTTP-instellingen** - deze instelling is de instelling die de HTTP-instellingen worden gebruikt voor de standaardregel definieert.

**Regels op basis van pad**

- **Naam** - dit is een beschrijvende naam voor de regel op basis van het pad.
- **Paden** - deze instelling bepaalt het pad naar dat de regel zoekt bij het doorsturen van verkeer
- **Groep met back-end** - met deze instelling is de instelling die de back-end moet worden gebruikt voor de regel definieert
- **Instelling voor HTTP** - met deze instelling is de instelling die de HTTP-instellingen worden gebruikt voor de regel definieert.

>[AZURE.IMPORTANT] Paden: De lijst met patronen pad aan. Elk moet beginnen met / en de enige plaats waar een "\*" mag zich aan het einde. Voorbeelden van geldige zijn/xyz, / XYZ* of /xyz/*.  

![Blade regel op basis van een pad met gegevens gevuld toevoegen][2]

Een regel op basis van een pad toe te voegen aan een bestaande toepassingsgateway is een eenvoudig proces via de portal. Wanneer u een regel op basis van het pad hebt gemaakt, kan het worden bewerkt om eenvoudig extra regels toevoegen. 

![aanvullende regels op basis van een pad toe te voegen][3]

## <a name="next-steps"></a>Volgende stappen

Zie informatie over het configureren van SSL-Offloading met Azure toepassingsgateway [Offload van SSL configureren](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png