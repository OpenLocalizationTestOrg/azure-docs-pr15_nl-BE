<properties
    pageTitle="Bescherm uw API met Azure API Management | Microsoft Azure"
    description="Informatie over het beveiligen van uw API met quota's en beleid (snelheidsbeperking) beperken."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Bescherm uw API met limieten voor de snelheid met Azure API Management

Deze handleiding wordt uitgelegd hoe gemakkelijk het is bescherming voor uw back-end API om toe te voegen tarief limiet en quota-beleid configureren met Azure API beheer.

In deze zelfstudie maakt u een 'Gratis' API-product waarmee ontwikkelaars voor maximaal 10 gesprekken per minuut en met een maximum van 200 gesprekken per week aan de API met behulp van de [limiet oproep tarief per abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) en het beleid van de [quota voor Set per abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) . U vervolgens de API publiceren en testen van het tarief beleid.

Zie voor meer geavanceerde bandbreedteregeling scenario's met het [tarief limiet door sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) en [quota door sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) beleid, [Geavanceerde aanvraag met Azure API beheer beperken](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>Voor het maken van een product

In deze stap maakt u een gratis product dat geen abonnement goedkeuring vereist.

>[AZURE.NOTE] Als u al een product hebt geconfigureerd en u wilt gebruiken voor deze zelfstudie, kunt u gaan [configureren][] aanroepen tarief limiet en quota en volgt u de zelfstudie van daaruit met behulp van uw product in plaats van de gratis evaluatieversie.

Klik op **beheren** in het klassieke Azure voor uw service Management API om te beginnen. Hiermee gaat u naar de portal Management API publisher.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service Management API nog niet hebt gemaakt, ziet u [een API Management service-exemplaar maken][] in de handleiding van [uw eerste API in Azure API beheer beheren][] .

Klik op **producten** in de **API beheer** menu aan de linkerkant om de pagina **producten** weer te geven.

![Product toevoegen][api-management-add-product]

Klik op **product toevoegen** om het dialoogvenster **Nieuw product toevoegen** .

![Nieuw product toevoegen][api-management-new-product-window]

Typ in het vak **titel** **Gratis proefperiode**.

Typ de volgende tekst in het vak **Beschrijving** :  **abonnees mogelijk uitvoeren 10 gesprekken per minuut tot een maximum van 200 gesprekken per week, waarna de toegang is geweigerd.**

Producten in de beheer-API kunnen worden beveiligd of openen. Beschermde producten bent geabonneerd op voordat ze kunnen worden gebruikt. Open producten kunnen worden gebruikt zonder een abonnement. Zorg ervoor dat **abonnement is vereist** voor het maken van een beveiligde product waarvoor een abonnement is geselecteerd. Dit is de standaardinstelling.

Als u een beheerder om te bekijken en accepteren of afwijzen pogingen abonnement op dit product, selecteer **abonnement goedkeuring vereisen**. Als het selectievakje niet is ingeschakeld, worden pogingen van abonnement automatisch goedgekeurd. In dit voorbeeld worden abonnementen automatisch goedgekeurd, zodat niet het vak selecteert.

Als u wilt toestaan dat ontwikkelaars zich abonneert meerdere keren op het nieuwe product, selecteert u het selectievakje **meerdere gelijktijdige abonnementen** . Deze zelfstudie niet gebruikmaken van meerdere gelijktijdige abonnementen, dus laat het uitgeschakeld.

Nadat alle waarden zijn ingevoerd, klikt u op **Opslaan** om het product te maken.

![Product toegevoegd][api-management-product-added]

Nieuwe producten zijn standaard zichtbaar voor gebruikers in de groep **Administrators** . We gaan de groep **ontwikkelaars** toe te voegen. **Gratis proefversie**op en klik vervolgens op het tabblad **zichtbaarheid** .

>Groepen worden in beheer-API gebruikt voor het beheren van de zichtbaarheid van producten voor ontwikkelaars. Producten zichtbaarheid toekennen aan groepen en ontwikkelaars kunnen bekijken en zich abonneren op de producten die zichtbaar zijn voor de groepen waarin ze behoren. Zie voor meer informatie, [het maken en gebruiken van groepen in Azure API beheer][].

![Groep ontwikkelaars toevoegen][api-management-add-developers-group]

Schakel het selectievakje **ontwikkelaars** in en klik vervolgens op **Opslaan**.

## <a name="add-api"> </a>Een API toevoegen aan het product

In deze stap van de zelfstudie zullen we de Echo-API toevoegen aan het nieuwe gratis product.

>Elk exemplaar van de service Management API is vooraf geconfigureerd met een Echo-API die kan worden gebruikt om te experimenteren met en informatie over het beheer van de API. Zie [de eerste API in Azure API beheer beheren][]voor meer informatie.

Klik op **producten** in het menu **API beheer** aan de linkerkant en klik vervolgens op **Gratis proefversie** om het product te configureren.

![Product configureren][api-management-configure-product]

Klik op **API toevoegen aan het product**.

![API voor een product toevoegen][api-management-add-api]

**Echo API**te selecteren en klik vervolgens op **Opslaan**.

![Toevoegen van Echo-API][api-management-add-echo-api]

## <a name="policies"> </a>Oproep tarief limiet en quota-beleid configureren

Limieten voor de snelheid en quota zijn geconfigureerd in de Groepsbeleid-editor. Klik op **beleid** onder het **Beheer van de API** -menu aan de linkerkant. Klik in de lijst met **producten** , **Gratis proefversie**.

![Productbeleid][api-management-product-policy]

Klik op **Beleid toevoegen** voor het importeren van de sjabloon en beginnen met het maken van de snelheid beperken en het quotum.

![Beleid toevoegen][api-management-add-policy]

Plaats de cursor in het **inkomende** **uitgaande** gedeelte of van de sjabloon wilt invoegen beleid. Tarief limiet en quota van inkomende beleid, zodat de cursorpositie in het inkomende element.

![Systeembeleid-editor][api-management-policy-editor-inbound]

De twee beleidsregels die we toe te in deze zelfstudie voegen zijn het beleid [limiet oproep tarief per abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) en het [instellen van quota voor per abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) .

![Beleidslijnen][api-management-limit-policies]

Nadat u de cursor in het **inkomende** beleidselement, klikt u op de pijl naast de **limiet oproep tarief per abonnement** de sjabloon invoegen.

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**Limiet oproep tarief per abonnement** op het productniveau van het kan worden gebruikt en kan ook worden gebruikt op het niveau van afzonderlijke bewerking-naam en API. In deze zelfstudie alleen productniveau beleid wordt gebruikt, dus de **api** en **werking** elementen verwijderen uit het element **limiet** , zodat alleen de buitenste **limiet** element blijft zoals in het volgende voorbeeld.

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

De aanroep van de maximaal toegestane snelheid is 10 gesprekken per minuut in het product gratis, dus **10** typt als de waarde voor het kenmerk **oproepen** en **60** voor de **vernieuwingsperiode** kenmerk.

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

Plaats de cursor direct onder de nieuw toegevoegde **limiet** element binnen het **inkomende** element het **gebruik van quota per abonnement instellen** als beleid wilt configureren, en klik vervolgens op de pijl links van de **quota voor per abonnement**.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

Omdat dit beleid is ook bedoeld om te worden op het productniveau van het, de naam **api** en **werking** elementen verwijderen zoals in het volgende voorbeeld.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

Quota's kunnen worden gebaseerd op het aantal aanroepen per interval of bandbreedte. In deze zelfstudie, we zijn niet op basis van de bandbreedte beperken, dus verwijdert u het kenmerk **bandbreedte** .

    <quota calls="number" renewal-period="seconds">
    </quota>

Het quotum is in de gratis evaluatieversie 200 gesprekken per week. **200** opgeven als waarde voor het kenmerk **oproepen** en geeft als de waarde voor het kenmerk **vernieuwingsperiode** **604800** .

    <quota calls="200" renewal-period="604800">
    </quota>

>Intervallen van beleid zijn opgegeven in seconden. Voor het berekenen van het interval voor een week, kunt u het aantal dagen (7) vermenigvuldigen met het aantal uren in een dag (24) het aantal minuten in een uur (60) het aantal seconden in een minuut (60): 7 *24* 60 * 60 = 604800.

Wanneer u klaar bent met het configureren van het beleid moet overeenkomen met het volgende voorbeeld.

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

Nadat het gewenste beleid zijn geconfigureerd, klikt u op **Opslaan**.

![Opslaan van beleid][api-management-policy-save]

## <a name="publish-product"></a> Voor het publiceren van het product

Nu de worden toegevoegd met de API's en het beleid zijn geconfigureerd, wordt het product moet worden gepubliceerd, zodat deze kan worden gebruikt door ontwikkelaars. Klik op **producten** in het menu **API beheer** aan de linkerkant en klik vervolgens op **Gratis proefversie** om het product te configureren.

![Product configureren][api-management-configure-product]

Klik op **publiceren**en klik vervolgens op **Ja, deze publiceren** om te bevestigen.

![Product publiceren][api-management-publish-product]

## <a name="subscribe-account"> </a>Een developer-account om het product te abonneren

Nu het product wordt gepubliceerd, is beschikbaar voor een abonnement op en gebruikt door ontwikkelaars.

>Beheerders van een exemplaar van de beheer-API worden automatisch geabonneerd op elk product. In deze zelfstudie stap zullen we een van de accounts ontwikkelaars geen beheerder voor de gratis evaluatieversie abonneren. Als uw developer-account deel van de rol Administrator uitmaakt, vervolgens voert u met deze stap, hoewel u al bent geabonneerd.

Klik op **gebruikers** in het menu **API beheer** aan de linkerkant en klik vervolgens op de naam van uw account developer. In dit voorbeeld gebruiken we de **Gragg Cool** developer-account.

![Ontwikkelaar configureren][api-management-configure-developer]

Klik op **abonnement toevoegen**.

![Abonnement toevoegen][api-management-add-subscription-menu]

**Gratis proefversie**te selecteren en klik op **abonneren**.

![Abonnement toevoegen][api-management-add-subscription]

>[AZURE.NOTE] In deze zelfstudie worden meerdere gelijktijdige abonnementen zijn niet ingeschakeld voor de gratis evaluatieversie. Als dat zo is, zou u worden gevraagd als naam voor het abonnement, zoals in het volgende voorbeeld.

![Abonnement toevoegen][api-management-add-subscription-multiple]

Wanneer u op **abonneren**, het product wordt weergegeven in de lijst met **abonnementen** voor de gebruiker.

![Abonnement toegevoegd][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Aanroepen van een bewerking en test de limiet

We kunnen de gratis evaluatieversie is geconfigureerd en uitgegeven, aanroepen van bepaalde bewerkingen en test het tarief beleid.
Overschakelen naar de developer-portal **Developer-portal** in het menu rechtsboven te klikken.

![Developer-portal][api-management-developer-portal-menu]

**API's** op in het bovenste menu, en klik op **Echo API**.

![Developer-portal][api-management-developer-portal-api-menu]

Klik op **Resource ophalen**en klik op **Probeer het**.

![Open console][api-management-open-console]

De standaard parameterwaarden behouden en selecteer uw sleutel abonnement voor de gratis evaluatieversie.

![Abonnement toets][api-management-select-key]

>[AZURE.NOTE] Als u meerdere abonnementen hebt, moet u de sleutel voor de **Gratis proefperiode**selecteren, anders de beleidsregels die zijn geconfigureerd in de voorgaande stappen niet meer van kracht.

Klik op **verzenden**en het antwoord vervolgens bekijken. Bekijk de **Response-status** van **200 OK**.

![Resultaat van de bewerking][api-management-http-get-results]

Klik op **verzenden** met een snelheid die groter is dan het tarief beleid 10 oproepen per minuut. Nadat het beleid van de snelheid wordt overschreden, wordt de status van een antwoord van **429 te veel aanvragen** geretourneerd.

![Resultaat van de bewerking][api-management-http-get-429]

De **inhoud van de reactie** geeft aan dat de resterende interval voordat de pogingen lukken.

Wanneer het tarief van 10 gesprekken per minuut beleid van kracht is, mislukken volgende aanroepen tot 60 seconden na de eerste van de 10 succesvolle aanroepen voor het product voordat de limiet is overschreden. In dit voorbeeld is de resterende interval 54 seconden.

## <a name="next-steps"> </a>Volgende stappen

-   Bekijk een demonstratie van het instellen van limieten voor de snelheid en de quota in de volgende video.

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Uw eerste API in Azure API beheer beheren]: api-management-get-started.md
[Het maken en het gebruik van groepen in Azure API beheer]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Een API Management service-exemplaar maken]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Oproep tarief limiet en quota-beleid configureren]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
