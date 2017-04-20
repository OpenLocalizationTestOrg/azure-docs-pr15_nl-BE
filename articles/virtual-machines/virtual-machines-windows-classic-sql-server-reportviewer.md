<properties 
    pageTitle="Rapportviewer gebruiken in een website | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe u een Microsoft Azure-website met de rapportviewer van Visual Studio-besturingselement dat wordt opgeslagen op een Microsoft Azure Virtual Machine."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management" />
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Rapportviewer gebruiken in een website die wordt gehost in Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


U kunt een Microsoft Azure-website met de rapportviewer van Visual Studio-besturingselement dat wordt opgeslagen op een Microsoft Azure Virtual Machine maken. Het besturingselement rapportviewer is in een webtoepassing dat u met behulp van de ASP.NET-toepassing websjabloon maken.

>[AZURE.IMPORTANT] ASP.NET MVC Web Application sjablonen bieden geen ondersteuning voor het besturingselement rapportviewer.

Rapportviewer opnemen in uw website Microsoft Azure, moet u de volgende taken uitvoeren.

- **Toevoegen** Het implementatiepakket assembly 's

- **Configureren** Verificatie en autorisatie

- **Publiceren** van de ASP.NET-webtoepassing naar Azure

## <a name="prerequisites"></a>Vereisten

Raadpleeg de sectie 'algemene aanbeveling en best practices' in [SQL Server Business Intelligence in Azure virtuele Machines](virtual-machines-windows-classic-ps-sql-bi.md).

>[AZURE.NOTE] Rapportviewer besturingselementen worden geleverd bij Visual Studio, Standard Edition of hoger. Als u de Web Developer Express Edition, moet u de [MICROSOFT rapport VIEWER 2012 RUNTIME](https://www.microsoft.com/download/details.aspx?id=35747) als de rapportviewer runtime-functies wilt gebruiken.
>
>Rapportviewer geconfigureerd in lokale verwerkingsmodus wordt niet ondersteund in Microsoft Azure.

Het [besturingselement voor de viewer van Reporting Services en Microsoft Azure virtual machine gebaseerde rapportservers](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)whitepaper bekijken.

## <a name="adding-assemblies-to-the-deployment-package"></a>Assembly's aan het pakket toe te voegen.

Wanneer u uw ASP.NET-toepassing lokale host, de rapportviewer assembly's meestal rechtstreeks in de global assemblycache (GAC) van de IIS-server worden geïnstalleerd tijdens de installatie van Visual Studio en rechtstreeks door de toepassing kunnen worden benaderd. Echter, als host fungeert voor uw ASP.NET-toepassing in de cloud, Microsoft Azure kunnen niet iets te worden geïnstalleerd in de GAC, dus moet u ervoor zorgen dat de rapportviewer assembly's voor uw toepassing lokaal beschikbaar zijn. U kunt dit doen door de verwijzingen naar deze in uw project toevoegen en configureren zodat ze kunnen lokaal worden gekopieerd.

Het besturingselement rapportviewer gebruikt in de modus voor externe verwerking de volgende verzamelingen:

- **Microsoft.ReportViewer.WebForms.dll**: de rapportviewer-code moet u rapportviewer gebruiken in uw pagina bevat. Een verwijzing naar deze assembly wordt toegevoegd aan uw project als u een besturingselement rapportviewer naar een ASP.NET-pagina in het project neerzet.

- **Microsoft.ReportViewer.Common.dll**: bevat klassen die worden gebruikt door het besturingselement rapportviewer tijdens runtime. Het is niet automatisch toegevoegd aan uw project.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Een verwijzing naar Microsoft.ReportViewer.Common toevoegen

- Met de rechtermuisknop op het knooppunt met **verwijzingen** uit van uw project en selecteer **Verwijzing toevoegen**, selecteert u de assembly in het tabblad .NET en klik op **OK**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>De assembly's lokaal toegankelijk maken door een ASP.NET-toepassing

1. Klik in de map met **verwijzingen naar** de assembly Microsoft.ReportViewer.Common zodat de eigenschappen worden weergegeven in het deelvenster Eigenschappen.

1. Stel **Lokale kopie** op True in het deelvenster Eigenschappen.

1. Herhaal stap 1 en 2 voor Microsoft.ReportViewer.WebForms.

### <a name="to-get-reportviewer-language-pack"></a>Rapportviewer Language Pack ophalen

1. Het juiste Microsoft rapport Viewer 2012 Runtime-pakket installeren vanaf het [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=317386).

1. Selecteer de taal in de vervolgkeuzelijst en de pagina wordt doorgestuurd naar de betreffende downloadpagina.

1. Klik op **downloaden** om het downloaden van ReportViewerLP.exe te starten.

1. Nadat u ReportViewerLP.exe hebt gedownload, klikt u op **uitvoeren** om onmiddellijk te installeren, of klik op **Opslaan** om het op uw computer opslaan. Als u op **Opslaan**klikt, moet u de naam van de map waarin u het bestand wilt opslaan.

1. Zoek de map waarin u het bestand hebt opgeslagen. Klik met de rechtermuisknop op ReportViewerLP.exe, klik op **Als administrator uitvoeren**en klik op **Ja**.

1. Na het uitvoeren van ReportViewerLP.exe, ziet u de c:\windows\assembly heeft de resource bestanden **Microsoft.ReportViewer.Webforms.Resources** en **Microsoft.ReportViewer.Common.Resources**.

### <a name="to-configure-for-localized-reportviewer-control"></a>Configureren voor het besturingselement gelokaliseerde rapportviewer

1. Download en installeer het distributiepakket van Microsoft rapport Viewer 2012 Runtime door de hierboven opgegeven instructies.

1. Maak <language> map in het project en de kopie de assembly gekoppelde bron bestanden bevat. De assembly-bronbestanden te kopiëren zijn: **Microsoft.ReportViewer.Webforms.Resources.dll** en **Microsoft.ReportViewer.Common.Resources.dll**. Selecteer de resource assembly-bestanden en instellen in het deelvenster Eigenschappen **kopiëren naar de uitvoermap** op '**altijd kopiëren**'.

1. De Culture en de UICulture voor het webproject instellen. Zie voor meer informatie over het instellen van de cultuur en de UI-cultuur voor een ASP.NET-webpagina [hoe: de cultuur en de UI-cultuur instellen voor globalisering van ASP.NET-webpagina](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Verificatie en autorisatie configureren

De rapportviewer moet de juiste referenties gebruikt voor de verificatie van de report server en de referenties van de report server moeten worden gemachtigd voor toegang tot de rapporten die u wilt. Zie het white paper [besturingselement voor de viewer van Reporting Services en Microsoft Azure virtual machine gebaseerde rapportservers](https://msdn.microsoft.com/library/azure/dn753698.aspx)voor informatie over verificatie.

## <a name="publish-the-aspnet-web-application-to-azure"></a>De ASP.NET-webtoepassing naar Azure publiceren

Zie voor meer informatie over het publiceren van een ASP.NET-webtoepassing naar Azure [procedure: migreren en publiceren van webtoepassingen naar Azure vanuit Visual Studio](../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) en [aan de slag met Web Apps en ASP.NET](../app-service-web/web-sites-dotnet-get-started.md).

>[AZURE.IMPORTANT] Als de opdracht toevoegen Azure implementatieproject of toevoegen Azure Cloud-Service niet wordt weergegeven in het snelmenu in de Solution Explorer, wellicht moet u .NET Framework 4 wijzigen in het kader van het doel van het project.
>
>De twee opdrachten bieden in principe dezelfde functionaliteit. Een of de andere opdracht verschijnt in het snelmenu afhankelijk van de versie van de Microsoft Azure SDK die u hebt geïnstalleerd.

## <a name="resources"></a>Bronnen

[Rapporten van Microsoft](http://go.microsoft.com/fwlink/?LinkId=205399)

[SQL Server Business Intelligence in Azure virtuele Machines](virtual-machines-windows-classic-ps-sql-bi.md)

[PowerShell gebruiken voor het maken van een Azure VM met een rapportserver Native modus](virtual-machines-windows-classic-ps-sql-report.md)

[Reporting Services-rapport viewer-besturingselement en Microsoft Azure virtuele machine op basis van rapportservers](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)
