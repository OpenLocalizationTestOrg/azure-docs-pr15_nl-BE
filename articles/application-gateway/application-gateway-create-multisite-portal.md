<properties
   pageTitle="Configureren van een bestaande toepassingsgateway voor het onderbrengen van meerdere sites in de portal Azure | Microsoft Azure"
   description="Deze pagina bevat instructies voor het configureren van een bestaande Azure application gateway voor het hosten van meerdere webtoepassingen op de dezelfde gateway met Azure portal."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Een bestaande toepassingsgateway voor het hosten van meerdere webtoepassingen configureren

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-multisite-portal.md)
- [Azure PowerShell voor bronbeheer](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Meerdere site hosting, kunt u meer dan één webtoepassing op de dezelfde toepassingsgateway implementeren. Dit is afhankelijk van de aanwezigheid van host-header in de binnenkomende HTTP-verzoek om te bepalen welke listener verkeer ontvangt. De listener stuurt vervolgens verkeer naar de juiste back-end-toepassingen zoals ingesteld in de definitie van de gateway. In webtoepassingen SSL is ingeschakeld, is de toepassingsgateway gebaseerd op uitbreiding van de Server naam aanduiding (SNI) kiest u de juiste listener voor de Internet-verkeer. Vaak gebruikt voor het hosten van meerdere site is om te laden voor verschillende webdomeinen aan groepen van verschillende back-end-server aanvragen. Op dezelfde manier kunnen meerdere domeinen van het hoofddomein hetzelfde ook worden gehost op de dezelfde toepassingsgateway.

## <a name="scenario"></a>Scenario

In het volgende voorbeeld fungeert toepassingsgateway verkeer voor contoso.com en fabrikam.com met groepen van twee back-end-server: contoso server-groep en de fabrikam server-groep. Soortgelijke setup kan worden gebruikt om host subdomeinen, zoals app.contoso.com en blog.contoso.com.

![scenario met meerdere locaties][multisite]

## <a name="before-you-begin"></a>Voordat u begint

Ondersteuning voor meerdere locaties in dit scenario toegevoegd aan een bestaande toepassingsgateway. Dit scenario voltooien moet scenario een toepassingsgateway bestaande beschikbaar zijn om te configureren. Ga naar [een toepassingsgateway via de portal maken](./application-gateway-create-gateway-portal.md) om informatie over het maken van een gateway basistoepassing in de portal.

## <a name="requirements"></a>Vereisten

- **Back-end server-groep:** De lijst met IP-adressen van de back-end servers. De IP-adressen ofwel moeten behoren tot het virtuele netwerk subnet of moeten een openbaar IP/VIP. FQDN-naam kan ook worden gebruikt.
- **Instellingen voor back-end server toepassingen:** Elke groep heeft als poort, protocol en affiniteit op basis van een cookie-instellingen. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front-poort:** Dit wordt de openbare poort op de toepassingsgateway wordt geopend. Verkeer treft deze poort en wordt vervolgens omgeleid naar een van de back-end servers.
- **Listener:** De listener is een front-end-poort, een protocol (Http of Https, deze waarden zijn hoofdlettergevoelig), en de SSL-certificaat (als het configureren van SSL-offload). Voor toepassing met meerdere locaties-gateways worden hostnaam en SNI indicatoren ook toegevoegd.
- **Regel:** De regel wordt de listener van de groep back-end-server gekoppeld en wordt gedefinieerd welke groep met back-end-server verkeer moet worden besteed aan wanneer deze een bepaalde listener.
- **Certificaten:** Elke listener moet een uniek certificaat, in dit voorbeeld 2 listeners worden gemaakt voor meerdere locaties. Twee .pfx-certificaten en de wachtwoorden voor deze moeten worden gemaakt.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

De vereiste stappen voor het bijwerken van de toepassingsgateway zijn:

1. Maak back-end-toepassingen te gebruiken voor elke site.
2. Een nieuwe listener maken voor elke site toepassingsgateway ondersteunt.
3. Regels maken om te wijzen elk listener met de juiste back-end.

## <a name="create-back-end-pools-for-each-site"></a>Back-end-toepassingen voor elke site maken

Een back-end-toepassingen voor elke site die toepassingsgateway zal ondersteuning nodig is, wordt in dit geval 2 gemaakt, één voor contoso11.com en één voor de fabrikam11.com.

### <a name="step-1"></a>Stap 1

Navigeer naar een bestaande toepassingsgateway in Azure portal (https://portal.azure.com). Selecteer **back-end-toepassingen** en klik op **toevoegen**

![back-end-toepassingen toevoegen][7]

### <a name="step-2"></a>Stap 2

Vul de gegevens voor de groep met back-end **pool1**, de FQDN-namen of IP-adressen voor de back-end servers toe te voegen en klik op **OK**

![instellingen voor back-end pool1][8]

### <a name="step-3"></a>Stap 3

Klik op **toevoegen** om een extra groep met back-end **pool2**, de FQDN-namen of IP-adressen voor de back-end servers toe te voegen op het blad van de back-end-toepassingen en klik op **OK**

![instellingen voor back-end pool2][9]

### <a name="create-listeners-for-each-back-end"></a>Listeners voor elke back-end maken

Application Gateway steunt op HTTP 1.1-host-headers als host voor websites op dezelfde openbare IP-adres en poort. De algemene listener is gemaakt in de portal bevat deze eigenschap niet.

### <a name="step-1"></a>Stap 1

**Listeners** op de bestaande toepassingsgateway en klik op **meerdere locaties** om toe te voegen van de eerste listener.

![listeners overzicht blade][1]

### <a name="step-2"></a>Stap 2

Vul de gegevens van de listener, In dit voorbeeld SSL-beëindiging is geconfigureerd, maakt u een nieuwe poort voor de frontend. Upload het .pfx-certificaat moet worden gebruikt voor SSL-beëindiging. Het enige verschil op deze blade ten opzichte van de standaard basic listener blade is de hostnaam.

![listener eigenschappen blade][2]

### <a name="step-3"></a>Stap 3

Op **meerdere locaties** en maken een andere listener zoals beschreven in de vorige stap voor de tweede site. Zorg ervoor dat u een ander certificaat gebruikt voor de tweede listener. Het enige verschil op deze blade ten opzichte van de standaard basic listener blade is de hostnaam. Vul de gegevens voor de listener en klik op **OK**.

![listener eigenschappen blade][3]

> [AZURE.NOTE] Maken van luisteraars in de Azure portal application gateway is een langdurige taak, duurt het enige tijd voor het maken van de twee listeners in dit scenario. Wanneer de listeners weergeven in de portal zoals weergegeven in de volgende afbeelding worden voltooid.

![listener-overzicht][4]

### <a name="create-rules-to-map-listeners-to-backend-pools"></a>Regels voor listeners worden toegewezen aan de back-end-toepassingen maken

### <a name="step-1"></a>Stap 1

Navigeer naar een bestaande toepassingsgateway in Azure portal (https://portal.azure.com). **Regels** selecteren en kiest u de bestaande standaard regel **rule1** en klik op **bewerken**.

### <a name="step-2"></a>Stap 2

Vul de blade regels zoals weergegeven in de volgende afbeelding. De eerste listener en de eerste groep en klikt u op **Opslaan** als voltooid.

![bestaande regel bewerken][6]

### <a name="step-3"></a>Stap 3

Klik op de **basisregel** als de 2e regel wilt maken. Vul het formulier met de tweede groep met back-end en de tweede listener en klik op **OK** om op te slaan.

![basisregel blade toevoegen][10]

Hiermee wordt een bestaande toepassingsgateway configureren met meerdere sites ondersteuning via de portal Azure.

## <a name="next-steps"></a>Volgende stappen

Informatie over het beveiligen van uw websites met [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png