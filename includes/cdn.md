# <a name="using-cdn-for-azure"></a>Met behulp van CDN voor Azure

Azure inhoud Delivery Network (CDN) biedt ontwikkelaars een globale oplossing voor het leveren van inhoud met hoge bandbreedte door het in cache opslaan van BLOB's en statische inhoud compute exemplaren bij fysieke knooppunten in de Verenigde Staten, Europa, Azië, Australië en Zuid-Amerika. Zie voor een actuele lijst van CDN knooppunt locaties [Azure CDN knooppunt locaties].

Deze taak omvat de volgende stappen uit:

* [Stap 1: Maak een account voor opslag](#Step1)
* [Stap 2: Maak een nieuw eindpunt CDN voor uw opslag account](#Step2)
* [Stap 3: Toegang tot de inhoud van uw CDN](#Step3)
* [Stap 4: Verwijder de inhoud van uw CDN](#Step4)

De voordelen van het gebruik van CDN voor Azure gegevens in de cache zijn:

-   Betere prestaties en de gebruiker ervaring voor eindgebruikers die ver van een inhoudsbron en toepassingen worden gebruikt waar veel internet trips nodig zijn om inhoud te laden
-   Grote gedistribueerde schaal beter verwerken momentane hoge belasting, bijvoorbeeld aan het begin van een gebeurtenis, zoals een productlancering

Bestaande CDN klanten kunnen nu de Azure CDN in [Azure klassieke portal]gebruiken. De CDN is een functie van de invoegtoepassing op uw abonnement en heeft een apart [plan voor facturering].

<a id="Step1"> </a>
<h2>Stap 1: Maak een account voor opslag</h2>

Gebruik de volgende procedure voor het maken van een nieuwe account opslag voor een abonnement op Azure. Een opslag account geeft toegang tot Azure opslagservices. De opslag account vertegenwoordigt het hoogste niveau van de naamruimte voor de toegang tot elk van de onderdelen van de Azure service: Blob, wachtrij, services en diensten van de tabel. Zie voor meer informatie over de van Azure opslagservices [met behulp van de opslag Azure Services](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Een opslag als account wilt maken, moet u de service-beheerder of beheerder van een collega voor het abonnement gekoppeld zijn.

> [AZURE.NOTE] Zie het onderwerp [Opslag Account maken](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) voor meer informatie over het uitvoeren van deze bewerking met behulp van de API-Azure Service Management.

**Voor een abonnement op Azure een opslag-account maken**

1.  Log in op de [Azure klassieke portal].
2.  Klik op **Nieuw**in de linkerbenedenhoek. Klik in het dialoogvenster **Nieuw** **Data Services**selecteren, klik op **opslag**, **Snel maken**.

    Verschijnt het dialoogvenster **Opslag Account maken** .

    ![Opslag-Account maken][create-new-storage-account]

4. Typ in het veld **URL** een subdomeinnaam. Deze vermelding kan van 3-24 kleine letters en cijfers bevatten.

    Deze waarde wordt de hostnaam van de in de URI die wordt gebruikt om Blob, wachtrij of tabel resources voor het abonnement. Om de bron van een container in de Blob-service op te lossen, gebruikt u een URI in de volgende notatie, waarbij * &lt;StorageAccountLabel&gt; * verwijst naar de waarde die u hebt getypt bij **Geef een URL**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Belangrijk:** De URL-label vormt het subdomein van de opslag account URI en moet uniek zijn alle gehoste services in Azure.

    Deze waarde wordt ook gebruikt als de naam van deze account voor opslag in de portal of als u via programmering toegang krijgen tot deze account.

5.  Selecteer een regio of een groep affiniteit voor de opslag van **Regio/affiniteit groep** in de vervolgkeuzelijst. Selecteer een groep affiniteit in plaats van een regio als u wilt dat uw opslagservices niet in hetzelfde datacenter met andere Windows Azure services die u gebruikt. Dit kan de prestaties verbeteren en geen kosten verbonden zijn egress.  

    **Opmerking:** Een affiniteit als groep wilt maken, opent u het gebied **Instellingen** van de Portal voor beheer, klik op **affiniteit groepen**en klik op **een groep affiniteit toevoegen** of **toevoegen**. U kunt ook groepen maken en beheren affiniteit met behulp van de API van Windows Azure Service Management. Zie [bewerkingen op affiniteit groepen] voor meer informatie.

6. Selecteer uit de vervolgkeuzelijst **abonnement** het abonnement dat met de account van de opslag wordt gebruikt.
7.  Klik op **opslag-Account maken**. Het proces van het maken van de opslag-account kan enkele minuten in beslag nemen.
8.  Als u wilt controleren of de opslag-account is gemaakt, controleert u of dat de account wordt weergegeven in de items met de status **on line**voor **opslag** .

<a id="Step2"> </a>
<h2>Stap 2: Maak een nieuw eindpunt CDN voor uw opslag account</h2>

Zodra u CDN toegang te tot een account voor opslag krijgen of service gehoste, komen alle algemeen beschikbare objecten in aanmerking voor het cachen van CDN rand. Als u een object dat momenteel in de CDN wijzigen, nieuwe inhoud worden niet beschikbaar via de CDN totdat de CDN de inhoud vernieuwen wanneer de cache-inhoud time-to-live verstreken.

**Een nieuw eindpunt CDN voor uw opslag-account maken**

1. Klik in de [Azure klassieke portal]in het navigatiedeelvenster op **CDN**.

2. Klik op **Nieuw**op het lint. Selecteer in het dialoogvenster **Nieuw** **App Services**, vervolgens **CDN**en vervolgens **Snel maken**.

3. Selecteer in de vervolgkeuzelijst **Domein van oorsprong** de opslag-account die u hebt gemaakt in het vorige gedeelte in de lijst van de rekeningen beschikbare opslagruimte. 

4. Klik op de knop **maken** om het maken van het nieuwe endpoint.

5. Als het eindpunt is gemaakt, wordt deze weergegeven in een lijst van eindpunten voor het abonnement. De lijstweergave toont de URL moet worden gebruikt voor toegang tot inhoud in cache, als het oorspronkelijke domein. 

    Het domein van oorsprong is de locatie van waaruit de CDN inhoud in cache opgeslagen. Het domein van oorsprong is een opslag-account of een cloud-service; een account voor de opslag wordt gebruikt voor de toepassing van dit voorbeeld. Edge-servers op basis van een cache-instelling die u opgeeft, of de standaard-methodiek van het netwerk in het cachegeheugen opgeslagen inhoud voor opslag in. 


    > [AZURE.NOTE] De configuratie die is gemaakt voor het eindpunt zal niet onmiddellijk beschikbaar zijn; het kan 60 minuten duren voordat de inschrijving doorgeven via het netwerk van CDN duren. Totdat de inhoud beschikbaar via de CDN is, kunnen gebruikers die proberen de CDN-domeinnaam onmiddellijk gebruiken statuscode 400 (onjuiste aanvraag) ontvangen.

<a id="Step3"> </a>
<h2>Stap 3: Toegang tot inhoud CDN</h2> 

Voor toegang tot inhoud in cache op de CDN, gebruikt u de URL CDN in de portal. Het adres voor een blob in de cache worden de volgende strekking:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>
<h2>Stap 4: De inhoud van de CDN verwijderen</h2>

Als u niet langer een object in de Azure Content Delivery Network (CDN) in de cache wilt, kunt u een van de volgende stappen uitvoeren:

-   U kunt de blob verwijderen uit de openbare container voor een Azure blob.
-   U kunt de container privé in plaats van het publiek. Zie de [Toegang beperken tot Containers en BLOB's](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) voor meer informatie.
-   U kunt uitschakelen of verwijderen van het CDN-eindpunt met behulp van de Portal beheren.
-   U kunt uw gehoste service niet meer reageert op verzoeken voor het object wijzigen.

Een object dat al in de cache in het CDN zal blijven in het cachegeheugen totdat de time-to-live voor het object zijn verstreken. Wanneer de time-to-live verstreken, wordt de CDN gecontroleerd of het eindpunt CDN nog geldig is en het object nog steeds anoniem toegankelijk. Als dat niet het geval is, vervolgens het object niet langer opgeslagen.

De mogelijkheid om inhoud direct te verwijderen wordt momenteel niet ondersteund op Azure Management Portal. Neem contact op met [ondersteuning van Azure](https://azure.microsoft.com/support/options/) als u onmiddellijk inhoud te verwijderen. 

## <a name="additional-resources"></a>Aanvullende bronnen

-   [Het maken van een groep affiniteit in Azure]
-   [Hoe: opslag beheren voor een abonnement Azure]
-   [Over de API voor servicebeheer]
-   [CDN inhoud toewijzen aan een aangepast domein]

  [Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
  [Azure CDN knooppunt locaties]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
  [Azure klassieke portal]: https://manage.windowsazure.com/
  [factuuradres-plan]: /pricing/calculator/?scenario=full
  [Het maken van een groep affiniteit in Azure]: http://msdn.microsoft.com/library/azure/ee460798.aspx
  [Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
  [Over de API voor servicebeheer]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
  [CDN inhoud toewijzen aan een aangepast domein]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
