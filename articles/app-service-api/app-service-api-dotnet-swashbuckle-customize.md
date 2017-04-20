
<properties 
    pageTitle="Definities van de gegenereerde Swashbuckle API aanpassen" 
    description="Informatie over het aanpassen Swagger API definities die worden gegenereerd door het Swashbuckle voor een API-app in Azure App-Service." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/29/2016" 
    ms.author="rachelap"/>

# <a name="customize-swashbuckle-generated-api-definitions"></a>Definities van de gegenereerde Swashbuckle API aanpassen 

## <a name="overview"></a>Overzicht

In dit artikel wordt uitgelegd hoe u Swashbuckle voor het verwerken van algemene scenario's waar u kunt het standaardgedrag wijzigen aanpassen:

* Swashbuckle genereert dubbele bewerking-id's voor overbelastingen van controller methoden
* Swashbuckle wordt ervan uitgegaan dat het alleen geldig antwoord van een HTTP-200 (OK) 
 
## <a name="customize-operation-identifier-generation"></a>Bewerking-id genereren aanpassen

Bewerking-id's Swagger Swashbuckle gegenereerd door de naam van de domeincontroller en de naam van de methode aaneen. Dit patroon wordt een probleem wanneer er meerdere overbelastingen van een methode: Swashbuckle genereert dubbele bewerking-id is ongeldig Swagger JSON.

De volgende code in de controller wordt bijvoorbeeld Swashbuckle op drie Contact_Get bewerking-id's genereren.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

U kunt het probleem handmatig oplossen door middel van unieke namen, zoals in het volgende voorbeeld van de methoden:

* Toevoegen
* GetById
* GetPage

Het alternatief is om uit te breiden Swashbuckle om het automatisch genereren van unieke bewerking-id's te maken.

De volgende stappen laten zien hoe Swashbuckle aanpassen met behulp van het *SwaggerConfig.cs* -bestand dat deel uitmaakt van het project in de Visual Studio-API Apps Preview project-sjabloon.  U kunt ook de Swashbuckle aanpassen in project Web API die u kunt voor de implementatie van een API-app configureren.

1. Maak een aangepaste `IOperationFilter` uitvoering 

    De `IOperationFilter` -interface biedt een punt uitbreidbaarheid voor Swashbuckle gebruikers die verschillende aspecten van het proces van de metagegevens van Swagger aanpassen. De volgende code toont een methode van het wijzigen van het gedrag van de bewerking-id genereren. Parameternamen code toegevoegd aan de naam van de bewerking-id.  

        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
        
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }

2. In het bestand *App_Start\SwaggerConfig.cs* , belt de `OperationFilter` hierbij Swashbuckle gebruik van de nieuwe `IOperationFilter` uitvoering.

        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();

    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)

    Het bestand *SwaggerConfig.cs* dat in wordt neergezet door het pakket Swashbuckle NuGet bevat veel toegelicht out voorbeelden van uitbreidbare punten. Aanvullende opmerkingen worden hier niet weergegeven. 

    Nadat u deze wijziging, de `IOperationFilter` implementatie wordt gebruikt, en zorgt ervoor dat de bewerking van de unieke id's worden gegenereerd.
 
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>
    
## <a name="allow-response-codes-other-than-200"></a>Responscodes dan 200 toestaan

Standaard Swashbuckle wordt ervan uitgegaan dat het antwoord van een HTTP-200 (OK) het *alleen* legitiem antwoord van een methode voor Web-API is. In sommige gevallen kunt u andere responscodes terug zonder dat de client een uitzondering veroorzaken.  Bijvoorbeeld ziet de volgende Web API-code u een scenario waarin zou u wilt dat de client een 200 of een 404 als geldige antwoorden te aanvaarden.

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

In dit scenario geeft de Swagger die Swashbuckle standaard genereert slechts één geldige HTTP-statuscode, HTTP-200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Aangezien de definitie van de API Swagger Visual Studio gebruikt om code te genereren voor de client, clientcode die een uitzondering voor elk antwoord dan een HTTP 200 genereert gemaakt. De onderstaande code is afkomstig van een C#-client gegenereerd voor deze methode monster Web API.

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

Swashbuckle kan op twee manieren voor het aanpassen van de lijst van verwachte HTTP responscodes die wordt gegenereerd met behulp van XML-commentaar of de `SwaggerResponse` kenmerk. Het kenmerk is eenvoudiger, maar is alleen beschikbaar in 5.1.5 Swashbuckle of hoger. De API Apps Sjabloonvoorbeeld nieuw project in Visual Studio 2013 bevat Swashbuckle versie 5.0.0, dus als u de sjabloon gebruikt en niet wilt bijwerken, Swashbuckle, de enige optie is het gebruik van XML-commentaar. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>Verwachte reactiecodes met behulp van XML-commentaar aanpassen

Gebruik deze methode responscodes opgeven als uw versie van de Swashbuckle lager dan 5.1.5 is.

1. Eerst becommentarieerd XML-documentatie over de methoden die u wilt opgeven, HTTP responscodes voor. Nemen van het monster Web API zou actie hierboven en de XML-documentatie toe te passen resulteren in code, zoals in het volgende voorbeeld. 

        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
        
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
        
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

1. Voeg instructies toe die in het bestand *SwaggerConfig.cs* om Swashbuckle te maken gebruik van het XML-documentatiebestand.

    * *SwaggerConfig.cs* openen en maken van een methode in de klasse *SwaggerConfig* om het pad naar het bestand van de XML-documentatie. 

            private static string GetXmlCommentsPath()
            {
                return string.Format(@"{0}\XmlComments.xml", 
                    System.AppDomain.CurrentDomain.BaseDirectory);
            }

    * Schuif omlaag in het bestand *SwaggerConfig.cs* totdat u de regel toegelicht out code die lijkt op de onderstaande schermafdruk zien. 

        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
    
    * Verwijder de opmerking van de regel waarmee de XML-opmerkingen verwerken tijdens het genereren van Swagger. 
    
        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
    
1. Om de documentatie XML-bestand genereren, gaat u naar de eigenschappen van het project en de XML-documentatiebestand zoals in het screenshot hieronder inschakelen. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

Nadat u deze stappen hebt uitgevoerd, wordt de Swagger JSON gegenereerd door Swashbuckle de HTTP responscodes die u hebt opgegeven in het XML-opmerkingen weergegeven. Het screenshot hieronder toont dit nieuwe JSON-nettolading. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Als u Visual Studio te genereren van de clientcode voor de REST API gebruikt, accepteert de C#-code statuscodes in de HTTP-OK en niet gevonden zonder een uitzondering, waardoor uw code in beslag nemen om beslissingen te nemen over het afhandelen van de terugkeer van een contactpersoonrecord null. 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

De code voor deze demonstratie vindt u in [deze bibliotheek GitHub](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse). Met de Web-API is gemarkeerd met XML-documentatieopmerkingen project een consoletoepassing project met een client gegenereerd voor deze API. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Verwachte reactiecodes met behulp van het kenmerk SwaggerResponse aanpassen

Het kenmerk [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) is beschikbaar in Swashbuckle 5.1.5 en hoger. Als u een eerdere versie in uw project hebt, wordt in deze sectie begint met waarin wordt uitgelegd hoe u het pakket Swashbuckle NuGet bijwerken, zodat u dit kenmerk kunt.

1. In de **Solution Explorer**met de rechtermuisknop op het project Web-API en klik op **Beheren NuGet pakketten**. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)

1. Klik op de *Update* knop naast het pakket *Swashbuckle* NuGet. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)

1. De *SwaggerResponse* -kenmerken toevoegen aan het Web API actie methoden die u wilt opgeven van geldige HTTP responscodes. 

        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();

            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

2. Voeg toe een `using` -instructie voor de naamruimte van het kenmerk:

        using Swashbuckle.Swagger.Annotations;
        
1. Ga naar de URL van de */swagger/docs/v1* van uw project en de verschillende HTTP responscodes zichtbaar zal zijn in de Swagger JSON. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

De code voor deze demonstratie vindt u in [deze bibliotheek GitHub](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes). Met de Web-API is gedecoreerd met het kenmerk *SwaggerResponse* project een consoletoepassing project met een client gegenereerd voor deze API. 

## <a name="next-steps"></a>Volgende stappen

Dit artikel is het aanpassen van de manier waarop die swashbuckle bewerking-id's en codes geldig antwoord genereert gebleken. Zie voor meer informatie [Swashbuckle op GitHub](https://github.com/domaindrivendev/Swashbuckle).
 
