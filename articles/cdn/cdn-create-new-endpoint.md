<properties
     pageTitle="Met behulp van Azure CDN | Microsoft Azure"
     description="Dit onderwerp bevat een Content Delivery Network (CDN) voor Azure inschakelen. De zelfstudie helpt bij het maken van een nieuwe CDN-profiel en een eindpunt."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="07/28/2016" 
     ms.author="casoper"/>

# <a name="using-azure-cdn"></a>Met behulp van Azure CDN  

In dit onderwerp doorloopt Azure CDN inschakelen door een nieuw profiel CDN en eindpunt te maken.

>[AZURE.IMPORTANT] Zie de [CDN-overzicht](./cdn-overview.md)voor een inleiding tot CDN werking, alsmede een lijst van alle functies.

## <a name="create-a-new-cdn-profile"></a>Maak een nieuw profiel voor CDN

Een CDN-profiel is een verzameling van CDN eindpunten.  Elk profiel bevat een of meer CDN-eindpunten.  U kunt meerdere profielen worden gebruikt voor het ordenen van uw eindpunten CDN door internet-domein, webtoepassing of andere criteria.

> [AZURE.NOTE] Een abonnement op één Azure is standaard beperkt tot acht CDN-profielen. Elk profiel CDN is beperkt tot tien CDN eindpunten.
>
> CDN prijzen wordt toegepast op het niveau van de CDN-profiel. Als u een mengeling van Azure CDN prijzen lagen gebruiken wilt, moet u meerdere profielen voor CDN.

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Maak een nieuwe CDN eindpunt

**Voor het maken van een nieuw CDN eindpunt**

1. Ga naar uw profiel CDN in [Azure Portal](https://portal.azure.com).  U kan zijn vastgemaakt dat aan het dashboard in de vorige stap.  Als u niet, u vinden kunt door te klikken op **Bladeren**en vervolgens **CDN profielen**en op het profiel dat u van plan bent uw eindpunt wilt toevoegen.

    De blade CDN profiel verschijnt.

    ![CDN-profiel][cdn-profile-settings]

2. Klik op **Toevoegen eindpunt** .

    ![Eindpunt knop toevoegen][cdn-new-endpoint-button]

    Het blad **een eindpunt toevoegen** wordt weergegeven.

    ![Eindpunt blade toevoegen][cdn-add-endpoint]

3. Voer een **naam** voor dit eindpunt CDN.  Deze naam wordt gebruikt voor toegang tot de bronnen in de cache op het domein `<endpointname>.azureedge.net`.

4. Selecteer uw type van oorsprong in de vervolgkeuzelijst **type oorsprong** .  **Opslag** voor een account voor opslag in Azure, **Cloud service** voor een Azure Cloud-Service, **Web App** voor een Azure Web App of **aangepaste oorsprong** selecteren voor elke andere publiek toegankelijke web server oorsprong (Perfmon.msc in Azure of elders).

    ![CDN oorsprong type](./media/cdn-create-new-endpoint/cdn-origin-type.png)
        
5. In de vervolgkeuzelijst **hostnaam oorsprong** Selecteer of typ het domein van oorsprong.  De vervolgkeuzelijst wordt een lijst met alle beschikbare oorsprongen van het type dat u hebt opgegeven in stap 4.  Als u *aangepaste oorsprong* geselecteerd als de **oorsprong typt**, wordt u in het oorspronkelijke domein van uw aangepaste typen.

6. Geef het pad naar de bronnen die u wilt cache of niets cache zodat elke bron in het domein dat u in stap 5 hebt opgegeven in het tekstvak **pad van oorsprong** .

7. In de **host-header van oorsprong**, de host-header die u wilt dat de CDN voor het verzenden van elke aanvraag, of laat de standaardwaarde.

    > [AZURE.WARNING] Sommige soorten oorsprong zoals Azure opslag en Web-Apps moeten aan het domein van oorsprong overeen met de host-header. Tenzij u een oorsprong waarvoor een host-header verschillen van het domein hebt, laat u de standaardwaarde.

8. Geef de protocollen en poorten die worden gebruikt voor toegang tot de bronnen die aan de oorsprong voor **Protocol** en **poort van oorsprong**.  Ten minste één protocol (HTTP of HTTPS) moet worden geselecteerd.
    
    > [AZURE.NOTE] De **haven van oorsprong** heeft alleen invloed op welke poort het eindpunt gebruikt voor het ophalen van informatie van de oorsprong.  Het eindpunt zelf alleen worden gebruikt op clients afsluiten op de standaard HTTP en HTTPS-poorten (80 en 443), ongeacht de **oorsprong poort**.  
    >
    > Het volledige bereik van TCP-poort voor oorsprong toegestaan **Azure CDN van Akamai** eindpunten niet.  Zie voor een lijst met poorten van oorsprong die niet zijn toegestaan, [Azure CDN van Akamai toegestane oorsprong poorten](https://msdn.microsoft.com/library/mt757337.aspx).  
    >
    > CDN toegang tot heeft inhoud via HTTPS de volgende beperkingen:
    > 
    > - U moet het SSL certificaat van de CDN. Certificaten van derden worden niet ondersteund.
    > - Moet u het domein opgegeven CDN (`<endpointname>.azureedge.net`) aan toegang HTTPS-inhoud. HTTPS-ondersteuning is niet beschikbaar voor aangepaste domeinnamen (CNAME-records) omdat de CDN aangepaste certificaten op dit moment niet ondersteunt.

9. Klik op de knop **toevoegen** als u wilt maken van het nieuwe endpoint.

10. Als het eindpunt is gemaakt, wordt deze weergegeven in een lijst van eindpunten voor het profiel. De lijstweergave toont de URL moet worden gebruikt voor toegang tot inhoud in cache, als het oorspronkelijke domein.

    ![CDN eindpunt][cdn-endpoint-success]

    > [AZURE.IMPORTANT] Het eindpunt wordt niet onmiddellijk worden gebruikt, als het duurt om de inschrijving doorgeven via de CDN.  Doorgifte wordt voor <b>Azure CDN van Akamai</b> profielen, meestal binnen één minuut voltooid.  <b>Azure CDN van Verizon</b> profielen, het doorgeven van meestal voltooit binnen 90 minuten, maar in sommige gevallen kan langer duren.
    >    
    > HTTP 404 responscodes ontvangen gebruikers die proberen de CDN domeinnaam gebruiken voordat de endpoint-configuratie is doorgegeven aan de POP's.  Als het al enkele uren nadat u uw eindpunt gemaakt en u nog steeds 404 antwoorden ontvangt, Zie [Probleemoplossing CDN eindpunten 404 statussen te retourneren](cdn-troubleshoot-endpoint.md).


##<a name="see-also"></a>Zie ook
- [Beheren van caching gedrag van aanvragen met querytekenreeksen](cdn-query-string.md)
- [CDN inhoud toewijzen aan een aangepast domein](cdn-map-content-to-custom-domain.md)
- [Activa op een eindpunt Azure CDN vooraf laden](cdn-preload-endpoint.md)
- [Een eindpunt Azure CDN leegmaken](cdn-purge-endpoint.md)
- [Het oplossen van CDN eindpunten 404 status opvragen](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
