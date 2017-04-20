<properties 
    pageTitle="Mobiele Engagement Azure - back-end integratie" 
    description="Azure betrokkenheid bij de mobiele verbinding met een SharePoint-end maken van campagnes van SharePoint" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement---api-integration"></a>Mobiele Engagement Azure - API-integratie

In een geautomatiseerd systeem van de marketing maken en activeren van de marketingcampagnes ook automatisch uitgevoerd. Voor dit doel - Azure Mobile Engagement kunnen dergelijke geautomatiseerde marketingcampagnes met API's ook maken. 

Klanten gebruiken meestal de betrokkenheid bij de mobiele front-end interface aankondigingen/polls enz maken als onderdeel van hun marketingcampagnes. Maar zodra de marketingcampagnes volwassen worden, er is nodig om de gegevens in de back-end-systemen (zoals een CRM-systeem of CMS-systeem zoals SharePoint) worden vergrendeld, zodat een volledig geautomatiseerde pijpleiding worden gemaakt die campagnes maakt in Mobile Engagement dynamisch op basis van de gegevens die de back-end systemen. 

![][5]

In deze zelfstudie wordt door middel van een scenario waarbij een zakelijke SharePoint gebruiker vult een SharePoint-lijst met marketinggegevens en een geautomatiseerd proces items uit de lijst wordt opgehaald en verbinding maakt met een marketingcampagne maken vanuit de SharePoint-gegevens met behulp van de beschikbare REST API's Mobile Engagement system. 
 
> [AZURE.IMPORTANT] In het algemeen kunt u dit voorbeeld als uitgangspunt om te begrijpen hoe Mobile Engagement REST API's aanroepen als er een gedetailleerd overzicht van de twee belangrijkste aspecten van het aanroepen van de API - verificatie en doorgeven van parameters. 

## <a name="sharepoint-integration"></a>Integratie met SharePoint
1. Hier is de steekproef SharePoint-lijst ziet eruit als. **Titel**, **categorie**, **NotificationTitle**, **bericht** , en **URL** worden gebruikt voor het maken van de aankondiging. Er is een kolom met de naam **IsProcessed** wordt gebruikt door het proces van de automatisering monster in de vorm van een consoleprogramma. Kunt u deze console programma als een Azure-WebJob zodat u deze kunt plannen uitvoeren of u kunt direct de SharePoint-werkstroom te maken en de aankondiging wordt geactiveerd wanneer een item wordt ingevoegd in de SharePoint-lijst. In dit voorbeeld gebruiken we de consoleprogramma doorloopt de items in de SharePoint-lijst en maak aankondiging in Azure Mobile Engagement voor elk van hen en markeert u de vlag **IsProcessed** om te worden voldaan bij het maken van geslaagde aankondiging ten slotte.

    ![][1]

2. We gebruiken de code van de steekproef *externe verificatie in SharePoint Online met behulp van het objectmodel van Client* [hier](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) te verifiëren met de SharePoint-lijst.
 
3. Zodra u geverifieerd, we doorlopen om erachter te komen alle nieuwe items de items in de lijst (die hebben **IsProcessed** = false). 

        static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);

                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();

                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;

                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;

                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";

                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Betrokkenheid bij de mobiele integratie
1.  Wanneer we zoeken naar een item waarvoor u verwerking - pakken we de informatie die is vereist voor het maken van een aankondiging van het item in de lijst en de oproep `CreateAzMECampaign` aan te maken en vervolgens `ActivateAzMECampaign` om deze te activeren. Dit zijn in feite REST API-aanroepen zoals het aanroepen van de backend Mobile Engagement. 

2.  De Mobile Engagement REST API's vereisen een **elementaire auth scheme vergunning HTTP-header** bestaat uit de `ApplicationId` en de `ApiKey` die u krijgt van de portal Azure. Zorg ervoor dat u de sleutel uit de sectie **sleutels api** gebruikt en *niet* vanuit de sectie **sleutels sdk** . 

    ![][2]

        static string CreateAuthZHeader()
        {
            string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
            return BasicAuthzHeaderString;
        }

        static public string EncodeTo64(string toEncode)
        {
            byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
            string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
            return returnValue;
        }  

3. Voor het maken van de aankondiging type campagne - Raadpleeg de [documentatie](https://msdn.microsoft.com/library/azure/mt683750.aspx). U moet ervoor zorgen dat u de campagne opgeeft `kind` als de *aankondiging* en de [nettolading](https://msdn.microsoft.com/library/azure/mt683751.aspx) en door te geven als FormUrlEncodedContent. 

        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";

            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
                
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
                
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });

                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }

4. Zodra u de aankondiging gemaakt hebt, ziet u ongeveer het volgende op de portal Mobile Engagement (Houd er rekening mee dat de toestand = concept en geactiveerd = n/b)

    ![][3]

5. `CreateAzMECampaign`Hiermee maakt u een campagne aankondiging en de Id geretourneerd naar de aanroepende functie. `ActivateAzMECampaign`deze Id vereist als de parameter voor het activeren van de campagne. 

        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());

                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });

                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }

6. Wanneer de aankondiging is geactiveerd, ziet u de volgende op de portal Mobile Engagement:

    ![][4]

7. Als de campagne wordt geactiveerd, wordt alle apparaten die voldoen aan het criterium van de campagne gestart meldingen zien. 

8. Ook ziet u dat het item gemarkeerd met IsProcessed = false is ingesteld op True als de aankondiging campagne is gemaakt.  

In dit voorbeeld is een eenvoudige aankondiging campagne geven meestal de vereiste eigenschappen gemaakt. U kunt dit zo groot kunt u vanuit de portal met de informatie [hier](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png


 
