<properties 
    pageTitle="Noodherstel, back-up service implementeren en herstellen in Azure API Management | Microsoft Azure" 
    description="Informatie over het gebruik van back-up en terugzetten Noodherstel in Azure API beheer uitvoeren." 
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

# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Noodherstel, back-up service implementeren en herstellen in Azure API beheer

Door te kiezen voor het publiceren en beheren van uw API's via Azure API beheer je profiteert van vele fouttolerantie en infrastructuurmogelijkheden die u anders moet ontwerpen, implementeren en beheren. De Azure platform vermindert een groot deel van de mogelijke storingen in een fractie van de kosten.

Om te herstellen van de beschikbaarheid van moeten vraagstukken op het gebied van de regio waar uw API beheer service wordt gehost worden uw service in een ander gebied te reconstrueren op elk gewenst moment. Afhankelijk van uw doelstellingen voor beschikbaarheid en herstel tijd doelstelling wil u reserveren van een back-up service in een of meer regio's en probeer te houden met hun configuratie- en synchroon met de actieve service. De service back-up en herstelfunctie biedt de noodzakelijke bouwsteen voor de uitvoering van uw noodherstelplan.

Deze handleiding wordt beschreven hoe aanvragen Azure Resource Manager verifiëren, en back-up maken en terugzetten van uw exemplaren van de service Management API.

>[AZURE.NOTE] Het proces voor back-ups maken en terugzetten van een exemplaar van de beheer-API service voor noodherstel kan ook worden gebruikt voor het repliceren van exemplaren van de service Management API voor scenario's zoals klaarzetten.
>
>Houd er rekening mee dat elke back-up na 7 dagen is verstreken. Als u een back-up herstellen probeert nadat de verloopperiode 7 dagen is verlopen, mislukt het terugzetten met een `Cannot restore: backup expired` bericht.

## <a name="authenticating-azure-resource-manager-requests"></a>Aanvragen voor geverifieerde switchverbindingen Azure Resource Manager

>[AZURE.IMPORTANT] De REST-API voor het maken en terugzetten van back-up gebruikt Azure Resource Manager en heeft een ander verificatiemechanisme dan de REST API's voor het beheren van de beheersinstanties API. De stappen in deze sectie wordt beschreven hoe u verificatie aanvragen Azure Resource Manager. Zie [aanvragen voor verificatie van Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790557.aspx)voor meer informatie.

Alle taken die u met bronbeheer Azure bronnen moeten worden geverifieerd met Azure Active Directory met behulp van de volgende stappen uit.

-   Een toepassing toevoegen aan de huurder Azure Active Directory.
-   Machtigingen instellen voor de toepassing die u hebt toegevoegd.
-   Het token voor het verifiëren van aanvragen op Azure Resource Manager ophalen.

De eerste stap is een Azure Active Directory-toepassing maken. Log in op de [Azure klassieke Portal](http://manage.windowsazure.com/) met behulp van het abonnement met uw exemplaar van de service Management API en Ga naar het tabblad **toepassingen** voor uw standaard Azure Active Directory.

>[AZURE.NOTE] Als u de standaardmap Azure Active Directory is niet zichtbaar voor uw account, neem dan contact op met de beheerder van het abonnement Azure om de vereiste machtigingen toewijzen aan uw account. Zie voor meer informatie over de locatie van de standaardmap [Zoek de standaardadreslijst](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-portal).

![Azure Active Directory-toepassing maken][api-management-add-aad-application]

Klik op **toevoegen**, **het ontwikkelen van mijn organisatie toepassing toevoegen**en kies **Native client-toepassing**. Voer een beschrijvende naam en klik op de pijl volgende gemarkeerd. Voer de URL van een tijdelijke aanduiding, zoals `http://resources` die voor het **Omleiden van URI**is een verplicht veld, maar de waarde niet hoger wordt gebruikt. Schakel het selectievakje in als u wilt opslaan van de toepassing.

Zodra de toepassing is opgeslagen, klikt u op **configureren**, Ga naar de sectie **machtigingen voor andere toepassingen** en klik op **toepassing toevoegen**.

![Machtigingen toevoegen][api-management-aad-permissions-add]

Selecteer **Windows** **Azure Service Management API** en schakel het selectievakje in als de toepassing wilt toevoegen.

![Machtigingen toevoegen][api-management-aad-permissions]

**Overgedragen machtigingen** naast de nieuw toegevoegde **Windows** **Azure Service Management API** -toepassing op, schakel het selectievakje in voor het **Beheer van toegang tot Azure Service (voorbeeld)**en klikt u op **Opslaan**.

![Machtigingen toevoegen][api-management-aad-delegated-permissions]

Voor het aanroepen van API's die het genereren van de back-up en herstellen, is het nodig zijn om een token. Het volgende voorbeeld wordt het [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget pakket op te halen van het token.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace GetTokenResourceManagerRequests
    {
        class Program
        {
            static void Main(string[] args)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenant id}");
                var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

                if (result == null) {
                    throw new InvalidOperationException("Failed to obtain the JWT token");
                }

                Console.WriteLine(result.AccessToken);

                Console.ReadLine();
            }
        }
    }

Vervangen `{tentand id}`, `{application id}`, en `{redirect uri}` met behulp van de volgende instructies.

Vervangen `{tenant id}` met de id van de huurder van de Azure Active Directory-toepassing die u zojuist hebt gemaakt. U kunt de id openen door te klikken op **weergave eindpunten**.

![Eindpunten][api-management-aad-default-directory]

![Eindpunten][api-management-endpoint]

Vervangen `{application id}` en `{redirect uri}` met de **Client-Id** en de URL van het **Omleiden van URI's** sectie van uw toepassing Azure Active Directory **configureren** op het tabblad. 

![Bronnen][api-management-aad-resources]

Wanneer de waarden worden opgegeven, weer in het voorbeeld een token zoals in het volgende voorbeeld.

![Token][api-management-arm-token]

Instellen voordat u de back-up en terugzetten van bewerkingen in de volgende secties wordt beschreven, de kop vergunning aanvragen voor het overige bellen.

    request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);

## <a name="step1"> </a>Back-up maken van een API-Management-service
Back-up maken van een API Management service probleem het volgende HTTP-aanvraag:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

waar:

* `subscriptionId`-id van het abonnement met de API-Management-service u probeert aan back-up
* `resourceGroupName`-een tekenreeks in de vorm van 'Api - standaard-{service regio}' waar `service-region` identificeert de Azure regio waar de API-Management-service dat u wilt back-up wordt gehost, bv.`North-Central-US`
* `serviceName`-de naam van de API-Management-service maken van een back-up van de opgegeven op het moment van de creatie
* `api-version`-vervangen`2014-02-14`

Geef de doelaccountnaam Azure opslag toegangstoets, blob containernaam en naam van de reservekopie in het hoofdgedeelte van de aanvraag:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Stel de waarde van de `Content-Type` op verzoek-header `application/json`.

Back-up is een langdurige bewerking die meerdere minuten in beslag kan nemen.  Als de aanvraag gelukt is en het back-upproces is geïnitieerd ontvangt u een `202 Accepted` statuscode van antwoord met een `Location` kop.  Controleer GET-aanvragen naar de URL in de `Location` kop om de status van de bewerking. Tijdens de back-up blijven u een statuscode '202 geaccepteerde' ontvangen. De reactiecode voor een van `200 OK` wordt gemeld dat de back-up is voltooid.

**Opmerking**:

- **Container** is opgegeven in de aanvraag instantie **moet bestaan**.
* Tijdens back-up u **beter niet proberen een service management-activiteiten** zoals SKU upgraden of downgraden, wijziging van de domeinnaam, enz. 
* Terugzetten van een **back-up is gegarandeerd alleen voor 7 dagen** sinds het moment dat het werd gemaakt. 
* **Gebruiksgegevens** worden gebruikt voor het maken van analytics-rapporten **is niet opgenomen** in de back-up. [Azure REST API beheer-API][] gebruiken regelmatig analytics-Rapporten voor de bewaring ervan ophalen.
* Uw doelstelling herstel punt is van invloed op de frequentie waarmee u back-ups service uitvoeren. Als u wilt minimaliseren u wordt geadviseerd regelmatig back-ups implementeert, alsmede op verzoek back-ups uitvoeren nadat u belangrijke wijzigingen in uw service Management API.
* **Wijzigingen** aangebracht in de configuratie van de service (bijvoorbeeld API's, beleid, developer portal uiterlijk) tijdens de back-upbewerking is in het proces **mogelijk niet zijn opgenomen in de back-up en dus verloren**.

## <a name="step2"> </a>Een API-Management-service herstellen
Service van een eerder gemaakte back-up maken als u wilt herstellen van een beheer-API de volgende HTTP-aanvraag:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

waar:

* `subscriptionId`-id van het abonnement met de API-Management-service die u bij het terugzetten van een back-up in
* `resourceGroupName`-een tekenreeks in de vorm van 'Api - standaard-{service regio}' waar `service-region` identificeert de Azure regio waar de API-Management-service die u bij het terugzetten van een back-up in wordt gehost, bv.`North-Central-US`
* `serviceName`-de naam van de API beheer service wordt teruggezet in de opgegeven op het moment van de creatie
* `api-version`-vervangen`2014-02-14`

Geef de locatie van back-upbestand, d.w.z. Azure opslag accountnaam, toegangstoets containernaam blob en naam van de reservekopie in het hoofdgedeelte van de aanvraag:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Stel de waarde van de `Content-Type` op verzoek-header `application/json`.

Terugzetten is een langdurige bewerking die op meer dan 30 minuten duren kan om te voltooien.  Als de aanvraag gelukt is en het herstelproces gestart werd ontvangt u een `202 Accepted` statuscode van antwoord met een `Location` kop.  Controleer GET-aanvragen naar de URL in de `Location` kop om de status van de bewerking. Tijdens het terugzetten wordt nog steeds wordt '202 geaccepteerde' statuscode. De reactiecode voor een van `200 OK` wordt gemeld dat de bewerking voor terugzetten is voltooid.

>[AZURE.IMPORTANT] **De SKU** van de service wordt teruggezet in de **moet overeenkomen met** de SKU van de back-up-service wordt teruggezet.
>
>**Wijzigingen** aangebracht in de configuratie van de service (bijvoorbeeld API's, beleid, developer portal uiterlijk) tijdens de bewerking voor terugzetten wordt uitgevoerd **, worden overschreven**.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende Microsoft-blogs voor twee verschillende scenario's van het proces van het maken en terugzetten.

-   [Azure API bedrijfsboekhouding repliceren](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/) 
    -   Bedankt voor de Gisela voor haar bijdrage aan dit artikel.
-   [Azure API beheer: Back-Up en terugzetten van de configuratie](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   De gedetailleerde door Stuart aanpak komt niet overeen met de officiële richtlijnen, maar het is heel interessant.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API beheer REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
 
