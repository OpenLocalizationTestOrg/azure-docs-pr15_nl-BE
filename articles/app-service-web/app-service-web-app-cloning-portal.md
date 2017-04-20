<properties
    pageTitle="Web App klonen met Azure Portal"
    description="Informatie over het kopiëren van uw Web Apps met nieuwe Web-Apps met Azure Portal."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/08/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Azure App Service App Azure Portal met behulp van klonen#

De functie voor het klonen in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) kunt u gemakkelijk klonen van bestaande web-apps aan een nieuwe app in dezelfde regio of in een andere regio. Hiermee schakelt u de klanten voor de implementatie van een aantal apps tussen de verschillende regio's snel en gemakkelijk.

Klonen van App wordt momenteel alleen ondersteund voor premium tier app serviceplannen. De nieuwe functie maakt gebruik van dezelfde beperkingen als Web Apps back-up functie, Zie [Back-up van een web app in Azure App-Service](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 


## <a name="cloning-an-existing-app"></a>Klonen van een bestaande App ##

De web app moet worden uitgevoerd in de modus van de **premie** voor u te maken voor het web app.

1. Open in de [Azure Portal](https://portal.azure.com/)van uw web app blade.
2. Klik op **Extra**. In het blad **Extra** Klik **Kloon App**.

    ![][1]

    > [AZURE.NOTE]
    > Als de web app nog niet in de **Premium** -modus, ontvangt u een bericht met de ondersteunde modi voor het klonen van app. Op dit punt hebt u de optie **Upgrade**selecteren.
    
3. Geef een naam op van het nieuwe web app, resourcegroep en App Service plannen in de **Kloon App** blade. Ook de gebruiker zal kunnen kiezen of u een aantal instellingen voor offlinegegevensbronnen web app klonen of niet.

    ![][2]

4. Nadat u op **maken** hebt zal het platform begint te werken voor het maken van een kloon van de bron web app.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonen van een bestaande App naar een App-omgeving##

In de **Kloon App** blade heeft de klant de optie voor het kiezen van een groep van toepassingen in een bestaande App-omgeving.

## <a name="current-restrictions"></a>Huidige beperkingen ##

Deze functie is momenteel in de voorvertoning, werken we aan nieuwe mogelijkheden toevoegen na verloop van tijd, in de volgende lijst worden de bekende beperkingen van de huidige ondersteuning van app klonen in Azure Portal:

- Azure verkeer Manager settings worden niet gekloond
- Instellingen voor automatisch schalen worden niet gekloond
- Back-upschema instellingen worden niet gekloond
- VNET-instellingen worden niet gekloond
- App inzichten worden niet automatisch ingesteld op de bestemming web app
- Eenvoudige verificatie-instellingen worden niet gekloond
- Kudu uitbreiding niet worden gekloond
- TiP-regels worden niet gekloond
- Database-inhoud niet worden gekloond


### <a name="references"></a>Verwijzingen ###
- [Web App klonen met PowerShell](app-service-web-app-cloning.md)
- [Back-up van een web app in Azure App-Service.](web-sites-backup.md)
- [Het maken van een App-omgeving](app-service-web-how-to-create-an-app-service-environment.md)
- [Maken van een web app in een App-omgeving](app-service-web-how-to-create-a-web-app-in-an-ase.md)
- [Inleiding tot de App-omgeving](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png