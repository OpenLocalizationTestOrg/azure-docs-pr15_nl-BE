<properties
    pageTitle="Bouwen en implementeren van een Java API in Azure App Service app"
    description="Informatie over het maken van een pakket Java API app en deze implementeren in Azure App-Service."
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/31/2016"
    ms.author="rachelap"/>

# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Bouwen en implementeren van een Java API in Azure App Service app

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Deze zelfstudie laat zien hoe een Java-toepassing maken en deze implementeren in Azure App Service API Apps met behulp van [Git]. De instructies in deze handleiding kunnen op elk besturingssysteem dat Java waarop kan worden gevolgd. De code in deze zelfstudie is gebouwd met behulp van [Maven]. [Jax RS] wordt gebruikt voor het maken van de Service RESTful en wordt gegenereerd op basis van de metagegevens [Swagger] specificatie met de [Editor Swagger].

## <a name="prerequisites"></a>Vereisten

1. [Java Developer's Kit 8] \(of hoger)
1. [Maven] is geïnstalleerd op de ontwikkelcomputer
1. [Git] geïnstalleerd op de ontwikkelcomputer
1. Een betaalde of [gratis] abonnement op [Microsoft Azure]
1. Een test HTTP-toepassing als [Postman]

## <a name="scaffold-the-api-using-swaggerio"></a>De API Swagger.IO met scaffold

Met de online swagger.io-editor kunt u JSON Swagger of YAML-code die de structuur van de API. Zodra u de API-oppervlak ontworpen hebt, kunt u de code voor diverse platforms en -kaders exporteren. In de volgende sectie, wordt de code scaffolded worden aangepast model functionaliteit. 

Deze demonstratie begint met een Swagger JSON-instantie die u in de swagger.io-editor die vervolgens worden gebruikt voor het genereren van code maakt gebruik van de JAX RS toegang te krijgen tot het eindpunt van een REST API zal plakken. Vervolgens bewerkt u de scaffolded code als model gegevens, wilt simuleren van een REST API gebaseerd op een mechanisme van de permanente gegevens.  

1. De volgende Swagger JSON-code naar het Klembord kopiëren:

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. Ga naar de [Online Editor Swagger]. Eenmaal, klikt u op de menuopdracht **File -> JSON plakken** .

    ![JSON menu-item plakken][paste-json]

1. In de contacten lijst API Swagger JSON u eerder hebt gekopieerd te plakken. 

    ![JSON-code plakken in Swagger][pasted-swagger]

1. De pagina's van documentatie en API samenvatting weergegeven in de editor weergeven. 

    ![Weergave Swagger gegenereerde documenten][view-swagger-generated-docs]

1. Selecteer de optie **Server genereren JAX RS->** naar de server-side code die u later bewerken om toe te voegen model implementatie scaffold. 

    ![Menu-Item Code genereren][generate-code-menu-item]

    Nadat de code is gegenereerd, die u zult een ZIP-bestand te downloaden. Dit bestand bevat de code van de codegenerator Swagger scaffolded en alle bijbehorende scripts bouwen. Pak de gehele bibliotheek naar een map op uw werkstation ontwikkeling. 

## <a name="edit-the-code-to-add-api-implementation"></a>De Code voor het toevoegen van API-implementatie te bewerken

In deze sectie, zult u de Swagger gegenereerde code aan de serverzijde uitvoering met uw aangepaste code vervangen. De nieuwe code retourneert een ArrayList van contactpersonen naar de aanvragende client. 

1. Open het bestand *Contact.java* model, dat zich bevindt in de map *gen/src/java/swagger/i/o/model* met behulp van [Visual Studio Code] of uw favoriete tekstverwerker. 

    ![Bestand met contactpersonen Model openen][open-contact-model-file]

1. De volgende constructor toevoegen aan de klasse **contactpersoon** . 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. *ContactsApiServiceImpl.java* -service implementatiebestand, dat zich bevindt in de map *src/main/java/i/o/swagger/api/impl* [Visual Studio Code] of uw favoriete teksteditor openen.

    ![Open Contact Service codebestand][open-contact-service-code-file]

1. De code in het bestand overschrijven met een model implementatie toevoegen aan de code van deze code. 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. Open een opdrachtprompt en map naar de hoofdmap van de toepassing wijzigen.

1. De volgende opdracht Maven code bouwen en uitvoeren met behulp van de app Jetty server lokaal wordt uitgevoerd. 

        mvn package jetty:run

1. U ziet het venster geven dat de code de Jetty is gestart op poort 8080. 

    ![Open Contact Service codebestand][run-jetty-war]

1. [Postman] een aanvraag te doen om de' alle contactpersonen' methode voor api-http://localhost:8080/contacten API gebruiken.

    ![De contacten API aanroepen][calling-contacts-api]

1. [Postman] gebruiken om een aanvraag naar de ' contactpersoon ' API methode get te vinden op http://localhost:8080/api/contactpersonen/2.

    ![De contacten API aanroepen][calling-specific-contact-api]

1. Tot slot het Java WAR (webarchief)-bestand door het uitvoeren van de volgende Maven-opdracht in de console. 

        mvn package war:war

1. Zodra u het WAR-bestand is gebaseerd, wordt het in **de doelmap** geplaatst. Navigeer naar **de doelmap** en wijzig het WAR-bestand in **ROOT.war**. (Zorg ervoor dat het hoofdlettergebruik overeenkomt met deze indeling).

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. Ten slotte de volgende opdrachten worden uitgevoerd vanuit de hoofdmap van de toepassing voor het maken van een map **implementeren** met behulp van het WAR-bestand implementeren op Azure. 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>De uitvoer naar Azure App Service publiceren

In deze sectie zult u informatie over het maken van een nieuwe API-App via de Portal Azure, voorbereiden die App API voor Java-toepassingen die als host fungeert en de gemaakte WAR-bestand implementeren op Azure App-Service voor het uitvoeren van uw nieuwe API App. 

1. Een nieuwe API-app in de [Azure portal]maken door te klikken op de opdracht **Nieuw Web -> + Mobile -> API app** , de details van uw app en vervolgens te klikken op **maken**.

    ![Maak een nieuwe API App][create-api-app]

1. Zodra uw app API is gemaakt van uw app **Instellingen** blade openen en klik vervolgens op de menuopdracht **instellingen van de toepassing** . De meest recente versie van Java uit de beschikbare opties te selecteren en vervolgens de nieuwste Tomcat selecteren in het menu **Web container** en klik op **Opslaan**.

    ![Java in de blade-API App instellen][set-up-java]

1. Klik op de menuopdracht van de **referenties van de implementatie** -instellingen en geef een gebruikersnaam en wachtwoord die u gebruiken wilt voor het publiceren van bestanden naar uw App API. 

    ![Set implementatie referenties][deployment-credentials]

1. Klik op de menuopdracht **bron implementatie** -instellingen. Eenmaal, klikt u op de knop **bron kiezen** , selecteert u de optie **Lokale Git Repository** en klik vervolgens op **OK**. Hiermee maakt u een Git repository in Azure, die een koppeling met uw App API uitgevoerd. Telkens wanneer die u code op de *master* -vertakking van de opslagplaats Git commit zal uw code in uw live sessie API App worden gepubliceerd. 

    ![Een nieuwe lokale Git repository instellen][select-git-repo]

1. De URL van de nieuwe Git bibliotheek naar het Klembord kopiëren. Sla dit is even belangrijk. 

    ![Een nieuw Git repository voor uw app instellen][copy-git-repo-url]

1. GIT push het WAR-bestand naar de online-opslagplaats. U doet dit door navigeert u naar de map **distribueren** is die u eerder hebt gemaakt, zodat u eenvoudig verbindt zich ertoe de code op de opslagplaats in de App-Service wordt uitgevoerd. Zodra u in het consolevenster en navigeren naar de map waarin zich de map webapps bevindt, geeft de volgende opdrachten Git te starten tijdens het starten een implementatie. 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    Als u de **push** -aanvraag verzendt, wordt u gevraagd het wachtwoord dat u eerder voor de referentie implementatie gemaakt. Als u uw referenties hebt ingevoerd, ziet u de portal weergeven dat de update is toegepast.

1. Als u nog een keer met een Postman treffers in de API nieuwe geïmplementeerd App in Azure App-Service wordt uitgevoerd, ziet u dat het gedrag consistent is en dat nu het contact gegevens retourneert zoals verwacht, en Java-code met behulp van eenvoudige codewijzigingen in de Swagger.io scaffolded. 

    ![Gebruik van de API Java contactpersonen REST live in Azure][postman-calling-azure-contacts]

## <a name="next-steps"></a>Volgende stappen

In dit artikel, het is om te beginnen met een Swagger JSON-bestand en sommige Java-code afkomstig van de Swagger.io-editor scaffolded. Daar uw wijzigingen eenvoudig en een Git implementeren proces heeft geresulteerd in een functionele API-app in Java geschreven hebben. In de volgende zelfstudie ziet hoe [verbruiken API apps van JavaScript-clients met behulp van CORS][App Service API CORS]. Zelfstudies voor later in de serie hoe implementeren van verificatie en machtiging.

Bouwen van dit voorbeeld, kunt u meer informatie over de [SDK voor Java opslag] om de JSON-BLOB's. Of u kunt het [Document DB Java SDK] Azure Document DB uw contactpersoongegevens opslaan. 

Zie voor meer informatie over het gebruik van Java in Azure [Java Developer Center].

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure portal]: https://portal.azure.com/
[Document DB Java SDK]: ../documentdb/documentdb-java-application.md
[gratis proefversie]: https://azure.microsoft.com/pricing/free-trial/
[GIT]: http://www.git-scm.com/
[Java Developer Center]: /develop/java/
[Java Developer's Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Swagger online Editor]: http://editor.swagger.io/
[Postman]: https://www.getpostman.com/
[Opslag SDK voor Java]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Swagger-Editor]: http://editor.swagger.io/
[Visual Studio Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
