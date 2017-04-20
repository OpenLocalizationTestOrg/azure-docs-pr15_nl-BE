<properties
   pageTitle="QuickStart voor de Azure AD Graph API | Microsoft-Aure"
   description="Azure Active Directory Graph API biedt toegang op programmeerniveau tot Azure AD via eindpunten OData REST API. Toepassingen kunnen de Graph API gebruiken voor het uitvoeren van maken, lezen, bijwerken en verwijderen van bewerkingen van Active directory-gegevens en objecten (CRUD)."
   services="active-directory"
   documentationCenter="n/a"
   authors="PatAltimore"
   manager="mbaldwin"
   editor=""
   tags=""/>


   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="09/16/2016"
      ms.author="patricka"/>

# <a name="quickstart-for-the-azure-ad-graph-api"></a>QuickStart voor de Azure AD Graph API

De Azure Active Directory (AD) Graph API biedt toegang op programmeerniveau tot Azure AD via eindpunten OData REST API. Toepassingen kunnen de Graph API gebruiken voor het uitvoeren van maken, lezen, bijwerken en verwijderen van bewerkingen van Active directory-gegevens en objecten (CRUD). Bijvoorbeeld, kunt u de Graph API een nieuwe gebruiker maken, weergeven of bijwerken van de eigenschappen van de gebruiker, het wachtwoord van gebruiker wijzigen, controleren groepslidmaatschap voor toegang op basis van rollen, uitschakelen of verwijderen van de gebruiker. Zie voor meer informatie over de Graph API-functies en toepassingsscenario's [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) en [Azure AD Graph API vereisten](https://msdn.microsoft.com/library/hh974476.aspx). 

> [AZURE.IMPORTANT] Azure AD Graph API-functionaliteit is ook beschikbaar via [Microsoft Graph](https://graph.microsoft.io/)een geünificeerde API die API's van andere Microsoft-services, zoals OneDrive, Outlook, OneNote, Planner en Office-grafiek, toegankelijk via een enkel eindpunt en met een één toegangstoken bevat.

## <a name="how-to-construct-a-graph-api-url"></a>Het maken van een Graph API-URL

In Graph API voor toegang tot Active directory-gegevens en objecten (met andere woorden, resources of diensten) die u wilt CRUD bewerkingen uitvoeren, kunt u URL's op basis van het gegevensbestand (OData)-Protocol. De URL's die worden gebruikt in Graph API bestaat uit vier hoofdonderdelen: service root, huurder id bronpad en query-Tekenreeksopties: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Neemt u het voorbeeld van de volgende URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

- **Service Root**: In Azure AD Graph API, de hoofdmap van de service is altijd https://graph.windows.net.
- **Huurder-id**: dit is een gecontroleerde domeinnaam (geregistreerd), in het voorbeeld hierboven contoso.com. Ook kan zijn een huurder object-ID of de 'myorganiztion' of 'mij' alias. Voor meer informatie Zie [entiteiten adressering en bewerkingen in de Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
- **Bronpad**: in dit gedeelte van een URL geeft de bron te worden gecommuniceerd met (gebruikers, groepen, een bepaalde gebruiker of een bepaalde groep, enz.) In het bovenstaande voorbeeld is het hoogste niveau 'groepen' adres die resource instellen. U kunt ook een specifieke entiteit, zoals adres ' gebruikers / {id} "of"gebruikers / userPrincipalName".
- **Query-parameters**:? de sectie bron pad van de sectie van de query parameters zijn gescheiden. De queryparameter "api versie" is vereist voor alle aanvragen in de Graph API. De Graph API ondersteunt ook de volgende query OData-opties: **$filter**, **$orderby**, **$**, **$top**en **$format**. De volgende query-opties worden niet ondersteund: **$count**, **$inlinecount**en **$skip**. Voor meer informatie, Zie [Opties van het wisselbestand in Azure AD Graph API, Filters en query's ondersteund](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Graph API-versies

U geeft de versie voor een aanvraag voor een Graph API in de queryparameter 'api versie'. Voor versie 1.5 en hoger gebruikt u een numerieke waarde; API versie 1.6 =. Voor eerdere versies gebruikt u een datumreeks die aan de notatie jjjj-MM-DD voldoet; bijvoorbeeld api versie = 2013-11-08. Gebruik voor preview-functies, de tekenreeks "beta"; bijvoorbeeld api versie = beta. Zie voor meer informatie over de verschillen tussen versies van Graph API [Azure AD Graph API versiebeheer](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Graph API-metagegevens

Het metagegevensbestand Graph API terug, het segment "$metadata" toevoegen na de huurder-id in de URL voor voorbeeld, retourneert de volgende URL metagegevens voor het voorbeeldbedrijf gebruikt door de grafiek Explorer: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. U kunt deze URL in de adresbalk van een webbrowser om te zien van de metagegevens. Beschrijving van het CSDL metagegevensdocument geretourneerd de entiteiten en complexe typen, hun eigenschappen en de functies en acties die door de versie van Graph API die u hebt aangevraagd. De parameter api versie weglaten als resultaat metagegevens voor de meest recente versie.

## <a name="common-queries"></a>Algemene query 's

[Algemene query's Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) bevat algemene query's die met Azure AD Graph, met inbegrip van query's die op het hoogste niveau van de bronnen in de map kunnen worden gebruikt en query's voor het uitvoeren van bewerkingen in de directory kunnen worden gebruikt.

Bijvoorbeeld `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` geeft de bedrijfsgegevens voor directory contoso.com.

Of `https://graph.windows.net/contoso.com/users?api-version=1.6` geeft een overzicht van alle user-objecten in de directory contoso.com.

## <a name="using-the-graph-explorer"></a>Met behulp van de grafiek Explorer

De Explorer grafiek voor de Azure AD Graph API kunt u de Active directory-gegevens opvragen bij het bouwen van uw toepassing.

> [AZURE.IMPORTANT] De grafiek Explorer biedt geen ondersteuning voor het schrijven of de gegevens te verwijderen uit een map. U kunt alleen leesbewerkingen op de Azure AD-map met de grafiek Explorer uitvoeren.

Hieronder ziet u de uitvoer die u zien zou als u navigeert u naar de grafiek Explorer, gebruik Demo bedrijf selecteren en voer was `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` voor alle gebruikers in de directory demo weergeven:

![Azure AD graph api explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**De grafiek Explorer laden**: laadt het programma, gaat u naar [https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/). Klik op **Het voorbeeldbedrijf gebruiken** om de grafiek Explorer uitvoeren op gegevens uit een huurder monster. U hoeft niet te gebruiken van het voorbeeldbedrijf referenties. U kunt ook op **aanmelden** en meld u aan met uw accountreferenties Azure AD de grafiek Explorer uitvoeren tegen de huurder. Als u Explorer grafiek tegen de huurder zelf uitvoert, moet u of uw beheerder toe te staan bij het aanmelden. Als u een Office 365-abonnement hebt, hebt u automatisch een huurder Azure AD. De referenties die u gebruikt voor aanmelding bij Office 365 zijn in feite Azure AD accounts, en kunt u deze referenties met Graph Explorer.

**Een query uitvoeren**: uitvoeren van een query, typt u de query in het tekstvak van het verzoek en **Klik op** of klik op sleutel **invoeren** . De resultaten worden weergegeven in het antwoord. Bijvoorbeeld, `https://graph.windows.net/graphdir1.onmicrosoft.com /groups?api-version=1.6` wordt een lijst met alle objecten groeperen in de map demo.

Houd rekening met de volgende functies en beperkingen van de grafiek Explorer:
- Functie AutoAanvullen op bron wordt ingesteld. Om dit te zien, klikt u op **Het voorbeeldbedrijf gebruiken** en klik vervolgens op op verzoek in het tekstvak (waarbij de bedrijfs-URL wordt weergegeven). U kunt een resource instellen in de vervolgkeuzelijst selecteren.

- Ondersteunt de 'mij' en 'MijnOrganisatie' aliassen. U kunt bijvoorbeeld `https://graph.windows.net/me?api-version=1.6` als resultaat het gebruikersobject van de aangemelde gebruiker of `https://graph.windows.net/myorganization/users?api-version=1.6` te retourneren van alle gebruikers in de huidige map. Houd er rekening mee dat de 'me' met behulp van alias wordt een fout geretourneerd voor het voorbeeldbedrijf omdat er geen aangemelde gebruiker die de aanvraag doet.

- Een sectie van de reactie headers. Dit kan worden gebruikt voor het oplossen van problemen die optreden tijdens het uitvoeren van query's.

- Een JSON-viewer voor de reactie met mogelijkheden voor uitvouwen en samenvouwen.

- Geen ondersteuning voor het weergeven van een thumbnail foto.

## <a name="using-fiddler-to-write-to-the-directory"></a>Schrijven naar de map met behulp van Fiddler

Voor de toepassing van deze handleiding, kunt u de Debugger Fiddler Web wilt uitvoeren 'schrijven' bewerkingen op u oefenen Azure AD directory. Zie [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler)voor meer informatie en Fiddler installeren.

In het volgende voorbeeld wordt Fiddler Web Debugger maakt u een nieuwe beveiligingsgroep 'MyTestGroup' in de map Azure AD.

**Een toegangstoken verkrijgen**: toegang tot Azure AD grafiek, clients worden geverifieerd Azure AD eerst zijn vereist. Zie [Verificatie-scenario's voor Azure advertentie](active-directory-authentication-scenarios.md)voor meer informatie.

**Opstellen en uitvoeren van een query**: de volgende stappen uit.

1. Fiddler Web Debugger openen en overschakelen naar het tabblad **Composer** .
2. Omdat u maken van een nieuwe beveiligingsgroep wilt, selecteert u de **boeken** als de HTTP-methode uit het meerkeuzemenu. Zie voor meer informatie over machtigingen en bewerkingen in een groepsobject [groep](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) binnen de [Azure AD Graph REST API Reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. Typ in het veld naast de **boeken**in als de aanvraag-URL: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.

    > [AZURE.NOTE] U moet de mytenantdomain met de naam van het domein van uw eigen map Azure AD vervangen.

4. In het veld direct onder Post pull-down, typt u het volgende:

    ```
Host: graph.windows.net
Authorization: your access token
Content-Type: application/json
```

    > [AZURE.NOTE] Vervangende uw &lt;het toegangstoken&gt; met het toegangstoken voor de Azure AD-map.

5. Typ het volgende in het veld **instantie verzoeken** :

    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
```

    Zie voor meer informatie over het maken van groepen, [Groep maken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Zie voor meer informatie over AD Azure entiteiten en typen die door de grafiek worden weergegeven en informatie over de bewerkingen die kunnen worden uitgevoerd met Graph, [Azure AD Graph REST API Reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
- Meer informatie over [Scopes Azure AD Graph API machtiging](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
