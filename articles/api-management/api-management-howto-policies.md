<properties 
    pageTitle="Beleid in Azure API Management | Microsoft Azure" 
    description="Informatie over het maken, bewerken en configureren van beleid in de beheer-API." 
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


#<a name="policies-in-azure-api-management"></a>Beleid in Azure API beheer

Beheer van Azure API is beleid een krachtige mogelijkheden van het systeem waarmee de uitgever van het gedrag van de API via configuratie wijzigen. Beleid is een verzameling instructies die worden uitgevoerd achter elkaar op de aanvraag of antwoord van een API. Populaire overzichten opgenomen Indelingsconversie van XML naar JSON en roep snelheidsbeperking beperkingen instellen voor het bedrag van de inkomende oproepen van een ontwikkelaar. Veel meer beleid zijn beschikbaar vanuit de doos.

Zie de [Verwijzing naar beleid][] voor een volledige lijst van beleidslijnen en de instellingen.

Het beleid wordt toegepast in de gateway die bevindt zich tussen de consument API en de beheerde API. De gateway ontvangt alle aanvragen en stuurt ze naar de onderliggende API ongewijzigd meestal door. Een beleid kan echter wijzigingen toepassen op zowel de binnenkomende aanvraag en het uitgaande antwoord.

Beleid-expressies kunnen worden gebruikt als het kenmerkwaarden of tekst in een van de API informatiebeheerbeleid tenzij u iets anders heeft het beleid. Sommige beleid zoals het beleid voor [Besturing][] en [variabele][] zijn gebaseerd op expressies van beleid. Zie [Geavanceerde beleid][] en [beleid voor expressies][]voor meer informatie.

## <a name="scopes"> </a>Het beleid configureren
Beleid kunnen globaal of het bereik van een [Product][], een [API][] of een [bewerking][]worden geconfigureerd. Een beleid wilt configureren, gaat u naar de beleid-editor in de publisher-portal.

![Menu beleid][policies-menu]

De beleid-editor bestaat uit drie gedeelten: het bereik van Groepsbeleid (boven), de definitie van het beleid waarin beleid (links) worden bewerkt en de overzichten lijst (rechts):

![Beleid-editor][policies-editor]

U moet de scope waarop het beleid moet toepassen selecteren om te beginnen met een beleid te configureren. Product is geselecteerd in het screenshot hieronder de **Starter** . Houd er rekening mee dat het vierkante pictogram naast de naam van het beleid geeft aan dat een beleid al is toegepast op dit niveau.

![Toepassingsgebied][policies-scope]

Aangezien er reeds een beleid is toegepast, wordt de configuratie in de definitie van weergave weergegeven.

![Configureren][policies-configure]

Het beleid alleen-lezen wordt weergegeven op het eerste. De definitie wilt bewerken klikt u op de **Configuratie van het computerbeleid** actie.

![Bewerken][policies-edit]

De definitie van het beleid is een eenvoudige XML-document waarin een reeks instructies voor inkomende en uitgaande. De XML-gegevens kan worden bewerkt in het definitievenster. Een lijst met overzichten wordt geleverd aan de rechterkant en instructies die van toepassing is op de huidige scope zijn ingeschakeld en geselecteerd. zoals blijkt uit de instructie **Call-tarief limiet** in het screenshot hierboven.

Te klikken op een ingeschakelde-instructie wordt de juiste XML toegevoegd op de locatie van de cursor in de weergave van de definitie. 

>[AZURE.NOTE] Als het beleid dat u wilt toevoegen niet is ingeschakeld, moet u ervoor zorgen dat u zich in het juiste bereik voor dat beleid. Elke beleidsverklaring is ontworpen voor gebruik in bepaalde bereiken en de beleid-secties. Om te controleren de beleid-secties en bereiken voor een beleid, Controleer de sectie **Gebruik** voor dat beleid in de [Verwijzing naar beleid][].

Een volledige lijst van beleidslijnen en de instellingen zijn beschikbaar in de [Verwijzing van beleid][].

Bijvoorbeeld, als u wilt een nieuwe instructie om binnenkomende aanvragen te beperken tot opgegeven IP-adressen toe te voegen, plaatst u de cursor net binnen de inhoud van de `inbound` XML-element en klikt u op de **beller Restrict IPs** -instructie.

![Softwarerestrictiebeleid][policies-restrict]

Kunt u hieraan een XML-fragment met de `inbound` element biedt u informatie over het configureren van de instructie.

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

Binnenkomende aanvragen te beperken en accepteren alleen die naar een IP-adres 1.2.3.4 de XML als volgt wijzigen:

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![Opslaan][policies-save]

Voltooid wanneer de instructies voor het beleid, configureren, klikt u op **Opslaan** en de wijzigingen volautomatisch naar de gateway voor de beheer-API.

##<a name="sections"> </a>Wat beleid configureren

Een beleid is een reeks instructies die worden uitgevoerd om een aanvraag en een antwoord. De configuratie op de juiste wijze is onderverdeeld in `inbound`, `backend`, `outbound`, en `on-error` zoals weergegeven in de volgende configuratie.

    <policies>
      <inbound>
        <!-- statements to be applied to the request go here -->
      </inbound>
      <backend>
        <!-- statements to be applied before the request is forwarded to 
             the backend service go here -->
      </backend>
      <outbound>
        <!-- statements to be applied to the response go here -->
      </outbound>
      <on-error>
        <!-- statements to be applied if there is an error condition go here -->
      </on-error>
    </policies> 

Als er een fout tijdens het verwerken van een aanvraag, alle overige stappen in de `inbound`, `backend`, of `outbound` secties worden overgeslagen en uitvoering springt naar de instructies in de `on-error` sectie. Door het beleid van instructies in de `on-error` sectie kunt u de fout bekijken met behulp van de `context.LastError` eigenschap, controleren en aanpassen van de fout antwoord met behulp van de `set-body` -beleid en configureren van wat er gebeurt als een fout optreedt. Er zijn foutcodes voor ingebouwde stappen en voor fouten die tijdens de verwerking van beleid voor instructies optreden kunnen. Zie [Fouten afhandelen in het beleid voor API](https://msdn.microsoft.com/library/azure/mt629506.aspx)voor meer informatie.

Aangezien het beleid op verschillende niveaus (algemene, product, api en werking) kunnen worden opgegeven kunt de configuratie u de volgorde waarin de beleid-definitie-instructies voor het bovenliggende beleid uitvoeren. 

In de volgende volgorde geëvalueerd beleid bereiken.

1. Globaal bereik
2. Scope producten
3. API-scope
4. Scope voor bewerking

De instructies daarin worden geëvalueerd op de locatie van de `base` -element, indien aanwezig.

Bijvoorbeeld als u een beleid op het globale niveau en een beleid dat is geconfigureerd voor een API, zal vervolgens wanneer dat bepaalde API gebruikt beleid worden toegepast. Beheer-API kunnen voor het bestellen van deterministic van gecombineerde beleidsverklaringen via het base-element. 

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

In het bovenstaande voorbeeld beleid definitie de `cross-domain` instructie zou worden uitgevoerd voordat een hogere beleid dat op zijn beurt zou worden gevolgd door de `find-and-replace` beleid.

Als hetzelfde beleid tweemaal in de Beleidsverklaring verschijnt, wordt het meest recent geëvalueerd beleid toegepast. U kunt dit beleid die zijn gedefinieerd op een hoger bereik negeren. Overzicht van het beleid in de huidige scope in de Systeembeleid-editor, klikt u op **herberekenen effectief beleid voor het geselecteerde bereik**.

Houd er rekening mee dat globaal beleid geen bovenliggende beleid heeft en het gebruik van de `<base>` -element in het heeft geen effect. 

## <a name="next-steps"></a>Volgende stappen

Bekijk video op expressies beleid te volgen.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Beleidsverwijzing]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Bewerking]: api-management-howto-add-operations.md

[Geavanceerde beleidsregels]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Controlestroom]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variabele]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Beleid voor expressies]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
