1. In Visual Studio **Solution Explorer**met de rechtermuisknop op het project en selecteer **toevoegen > Docker ondersteuning** in het contextmenu.

    ![Ondersteuning voor Docker contextmenu toevoegen](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. Docker ondersteuning toe te voegen aan de kern van een ASP.NET resulteert-webproject in de toevoeging van verschillende Docker gerelateerde bestanden die worden toegevoegd aan het project, inclusief bestanden Docker opstellen, implementeren Windows PowerShell-scripts en Docker eigenschap bestanden. 

    ![Docker bestanden toegevoegd aan project](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE]Als de [Docker voor Windows Beta](https://beta.docker.com), Properties\Docker.props openen, verwijderen de standaardwaarde en Visual Studio voor de waarde van kracht te laten starten.
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=='' "></DockerMachineName>
> ```
