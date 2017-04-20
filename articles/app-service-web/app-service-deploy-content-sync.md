<properties
    pageTitle="Inhoud synchroniseren vanuit een map cloud Azure App service"
    description="Informatie over het implementeren van uw app Azure App service via inhoud synchroniseren vanuit een map wolk."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Inhoud synchroniseren vanuit een map cloud Azure App service

In deze zelfstudie wordt beschreven hoe u met [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) implementeren door het synchroniseren van uw inhoud van populaire cloud storage-services zoals Dropbox en OneDrive. 

## <a name="overview"></a>Overzicht van de distributie van inhoud synchroniseren

De distributie van inhoud synchroniseren op aanvraag wordt aangedreven door de [Kudu implementatie engine](https://github.com/projectkudu/kudu/wiki) geïntegreerd met App-Service. In [Azure Portal](https://portal.azure.com)kunt u een map in de cloud opslag, werken met uw app en inhoud in de map en service App synchroniseren met het klikken op een knop. Inhoud sync maakt gebruik van de Kudu-proces voor het bouwen en uitvoeren. 
    
## <a name="contentsync"></a>Inhoud sync implementatie inschakelen
Zodat inhoud synchroniseren vanuit de [Portal Azure](https://portal.azure.com)als volgt te werk:

1. Klik op **Instellingen**in de blade van uw app in de Portal Azure, > **Distributiebron**. Klik op **Bron kiezen**en selecteer vervolgens **OneDrive** of **Dropbox** als bron voor de implementatie. 

    ![Inhoud Sync](./media/app-service-deploy-content-sync/deployment_source.png)

    >[AZURE.NOTE] Vanwege de onderliggende verschillen in de API's wordt **OneDrive voor Business** niet ondersteund op dit moment. 

2. De werkstroom voor autorisatie inschakelen App-Service toegang krijgen tot een bepaald pad voor vooraf gedefinieerde aangewezen voor OneDrive of Dropbox waar alle inhoud van uw App-Service worden opgeslagen worden voltooid.  
    Na toestemming van de App Service krijgt platform u de optie voor het maken van een map met inhoud in het opgegeven pad inhoud of een bestaande map inhoud onder deze aangewezen inhoud pad kiezen. De aangewezen inhoud onder uw cloud opslag rekeningen gebruikt voor synchronisatie App Service zijn de volgende:  
    * **OneDrive**:`Apps\Azure Web Apps` 
    * **Dropbox**:`Dropbox\Apps\Azure`

3. Na de eerste synchronisatie van inhoud kan de synchronisatie van inhoud op aanvraag vanaf de portal Azure worden gestart. Geschiedenis van de implementatie is beschikbaar bij de **implementaties** blade.

    ![Implementatie-overzicht](./media/app-service-deploy-content-sync/onedrive_sync.png)
 
Meer informatie voor implementatie van Dropbox is beschikbaar bij het [implementeren van Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 


