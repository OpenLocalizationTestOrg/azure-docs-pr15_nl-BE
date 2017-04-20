<properties
    pageTitle="Een opslag-Account integreren met CDN | Microsoft Azure"
    description="Leren om Azure inhoud Delivery Network (CDN) gebruiken om inhoud met hoge bandbreedte door het in cache opslaan van BLOB's uit de opslag van Azure."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="integrate-a-storage-account-with-cdn"></a>Een opslag-Account integreren met CDN

CDN kan worden ingeschakeld om de inhoud van de cache van uw opslag Azure. Het biedt ontwikkelaars een globale oplossing voor het leveren van inhoud met hoge bandbreedte door het in cache opslaan van BLOB's en statische inhoud compute exemplaren bij fysieke knooppunten in de Verenigde Staten, Europa, Azië, Australië en Zuid-Amerika.


## <a name="step-1-create-a-storage-account"></a>Stap 1: Maak een account voor opslag

Gebruik de volgende procedure voor het maken van een nieuwe account opslag voor een abonnement op Azure. Een opslag account geeft toegang tot Azure opslagservices. De opslag account vertegenwoordigt het hoogste niveau van de naamruimte voor de toegang tot elk van de onderdelen van de Azure service: Blob, wachtrij, services en diensten van de tabel. Raadpleeg de [Inleiding tot Microsoft Azure opslag](../storage/storage-introduction.md)voor meer informatie.

Een opslag als account wilt maken, moet u de service-beheerder of beheerder van een collega voor het abonnement gekoppeld zijn.

> [AZURE.NOTE] Er zijn verschillende methoden die u gebruiken kunt voor het maken van een account voor opslag, met inbegrip van Powershell en Azure Portal.  Voor deze zelfstudie zullen we de Azure Portal gebruikt.  

**Voor een abonnement op Azure een opslag-account maken**

1.  Aanmelden bij de [Azure Portal](https://portal.azure.com).
2.  Selecteer **Nieuw**in de linkerbovenhoek. In het dialoogvenster **Nieuw** **Data- +**selecteren, klik op **opslag-account**.

    Het blad voor de **opslag-account maken** wordt weergegeven.

    ![Opslag-Account maken][create-new-storage-account]

4. Typ in het veld **naam** de subdomeinnaam van een. Deze vermelding kan 3-24 kleine letters en cijfers bevatten.

    Deze waarde wordt de hostnaam van de in de URI die wordt gebruikt om Blob, wachtrij of tabel resources voor het abonnement. Om de bron van een container in de Blob-service op te lossen, gebruikt u een URI in de volgende notatie, waarbij * &lt;StorageAccountLabel&gt; * verwijst naar de waarde die u hebt getypt bij **Geef een URL**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Belangrijk:** De URL-label vormt het subdomein van de opslag account URI en moet uniek zijn alle gehoste services in Azure.

    Deze waarde wordt ook gebruikt als de naam van deze account voor opslag in de portal of als u via programmering toegang krijgen tot deze account.

5. Laat de standaardinstellingen voor **implementatiemodel**, **type Account**, **prestaties**en **replicatie**. 

6. Selecteer het **abonnement** dat met de account van de opslag wordt gebruikt.

7. Selecteer of maak een **Resourcegroep**.  Zie voor meer informatie over resourcegroepen, [Azure Resource Manager-overzicht](azure-resource-manager/resource-group-overview.md#resource-groups).

8. Selecteer een locatie voor uw opslag-account.

8. Klik op **maken**. Het proces van het maken van de opslag-account kan enkele minuten in beslag nemen.


## <a name="step-2-create-a-new-cdn-profile"></a>Stap 2: Maak een nieuw profiel voor CDN

Een CDN-profiel is een verzameling van CDN eindpunten.  Elk profiel bevat een of meer CDN-eindpunten.  U kunt meerdere profielen worden gebruikt voor het ordenen van uw eindpunten CDN door internet-domein, webtoepassing of andere criteria.

> [AZURE.TIP] Als u al een CDN-profiel dat u wilt gebruiken voor deze zelfstudie hebt, gaat u verder met [stap 3](#step-3-create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="step-3-create-a-new-cdn-endpoint"></a>Stap 3: Maak een nieuwe CDN eindpunt

**Een nieuw eindpunt CDN voor uw opslag-account maken**

1. Ga naar uw profiel CDN in [Azure Management Portal](https://portal.azure.com).  U kan zijn vastgemaakt dat aan het dashboard in de vorige stap.  Als u niet, u vinden kunt door te klikken op **Bladeren**en vervolgens **CDN profielen**en op het profiel dat u van plan bent uw eindpunt wilt toevoegen.

    De blade CDN profiel verschijnt.

    ![CDN-profiel][cdn-profile-settings]

2. Klik op **Toevoegen eindpunt** .

    ![Eindpunt knop toevoegen][cdn-new-endpoint-button]

    Het blad **een eindpunt toevoegen** wordt weergegeven.

    ![Eindpunt blade toevoegen][cdn-add-endpoint]

3. Voer een **naam** voor dit eindpunt CDN.  Deze naam wordt gebruikt voor toegang tot de bronnen in de cache op het domein `<endpointname>.azureedge.net`.

4. Selecteer in de vervolgkeuzelijst **type oorsprong** *opslag*.  

5. Selecteer in de vervolgkeuzelijst **hostnaam oorsprong** uw opslag-account.

6. Laat de standaardinstellingen voor het **pad van oorsprong**, **herkomst host-header**en **Protocol/oorsprong poort**.  U moet ten minste één protocol (HTTP of HTTPS).

    > [AZURE.NOTE] Deze configuratie kan al uw openbaar zichtbaar containers in uw account opslag voor caching in de CDN.  Als u wilt om het te beperken tot een enkele container, gebruikt u de **oorsprong-pad**.  Opmerking de container moet de zichtbaarheid ervan openbaar.

7. Klik op de knop **toevoegen** als u wilt maken van het nieuwe endpoint.

8. Als het eindpunt is gemaakt, wordt deze weergegeven in een lijst van eindpunten voor het profiel. De lijstweergave toont de URL moet worden gebruikt voor toegang tot inhoud in cache, als het oorspronkelijke domein.

    ![CDN eindpunt][cdn-endpoint-success]

    > [AZURE.NOTE] Het eindpunt wordt niet onmiddellijk worden gebruikt.  Het kan tot 90 minuten voor de inschrijving doorgeven via het netwerk van CDN duren. Gebruikers die proberen te gebruiken de CDN-domeinnaam onmiddellijk wordt statuscode 404 totdat de inhoud beschikbaar via de CDN is.


## <a name="step-4-access-cdn-content"></a>Stap 4: Toegang tot inhoud CDN

Voor toegang tot inhoud in cache op de CDN, gebruikt u de URL CDN in de portal. Het adres voor een blob in de cache worden de volgende strekking:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [AZURE.NOTE] Zodra u CDN toegang te tot een account voor opslag krijgen of service gehoste, komen alle algemeen beschikbare objecten in aanmerking voor het cachen van CDN rand. Als u een object dat momenteel in de CDN wijzigen, nieuwe inhoud worden niet beschikbaar via de CDN totdat de CDN de inhoud vernieuwen wanneer de cache-inhoud time-to-live verstreken.

## <a name="step-5-remove-content-from-the-cdn"></a>Stap 5: De inhoud van de CDN verwijderen

Als u niet langer een object in de Azure Content Delivery Network (CDN) in de cache wilt, kunt u een van de volgende stappen uitvoeren:

-   U kunt de container privé in plaats van het publiek. Zie [anonieme leestoegang tot containers en BLOB's beheren](../storage/storage-manage-access-to-resources.md) voor meer informatie.
-   U kunt uitschakelen of verwijderen van het CDN-eindpunt met behulp van de Portal beheren.
-   U kunt uw gehoste service niet meer reageert op verzoeken voor het object wijzigen.

Een object dat al in de cache in het CDN zal blijven in het cachegeheugen totdat de time-to-live voor het object zijn verstreken of het eindpunt wordt verwijderd. Wanneer de time-to-live verstreken, wordt de CDN gecontroleerd of het eindpunt CDN nog geldig is en het object nog steeds anoniem toegankelijk. Als dat niet het geval is, vervolgens het object niet langer opgeslagen.


## <a name="additional-resources"></a>Aanvullende bronnen

-   [CDN inhoud toewijzen aan een aangepast domein](cdn-map-content-to-custom-domain.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png

[cdn-profile-settings]: ./media/cdn-create-a-storage-account-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-a-storage-account-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-a-storage-account-with-cdn/cdn-endpoint-success.png
