<properties 
   pageTitle="Azure opslag toevoegen met behulp van Connected Services in Visual Studio | Microsoft Azure"
   description="Azure opslag toevoegen aan uw app via het dialoogvenster Visual Studio verbonden Services toevoegen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Azure opslag toe te voegen met behulp van Visual Studio Connected Services

## <a name="overview"></a>Overzicht

Met Visual Studio 2015, kunt u een cloud service C#, .NET back-end mobiele service, ASP.NET-website of service, service ASP.NET 5 of Azure WebJob service Azure opslag met behulp van het dialoogvenster **Verbonden Services toevoegen** . De functie verbonden wordt alle benodigde verwijzingen en code van de verbinding en de configuratiebestanden op de juiste manier wordt aangepast. Het dialoogvenster gaat u documentatie kunt u zien wat de volgende stappen zijn blob storage, wachtrijen en tabellen.

## <a name="supported-project-types"></a>Ondersteunde projecttypen

Het dialoogvenster verbonden Services kunt u verbinding maken met Azure opslag in de volgende projecttypen.

- ASP.NET Web projecten

- ASP.NET 5 projecten

- .NET cloud Service Webrol en projecten met een rol werknemer

- .NET-projecten voor mobiele Services

- Azure WebJob projecten


## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Verbinding maken met Azure opslag met behulp van het dialoogvenster Connected Services

1. Zorg ervoor dat u een Azure-account hebt. Als u niet een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefperiode](http://go.microsoft.com/fwlink/?LinkId=518146). Zodra u een Azure-account hebt, kunt u accounts opslag maken, mobiele services maken en Azure Active Directory configureren.

1. Open het project in Visual Studio, opent u het contextmenu voor het knooppunt met **verwijzingen** in de Solution Explorer en kies vervolgens **Verbonden Service toevoegen**.

    ![Een verbonden service toe te voegen](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. **Azure een opslagmedium**kiezen in het dialoogvenster **Verbonden Service toevoegen** en kies vervolgens de knop **configureren** . U wordt mogelijk gevraagd om in te loggen Azure als u nog niet hebt gedaan.

    ![Het dialoogvenster verbonden Service - opslagruimte toevoegen](./media/vs-azure-tools-connected-services-storage/IC796703.png)

1. Selecteer een bestaande account voor opslag in **Azure opslag** van het dialoogvenster en selecteer **toevoegen**.

    Als u een nieuwe opslag-account te maken, gaat u naar de volgende stap. Ga anders verder met stap 6.

    ![Azure opslag, dialoogvenster](./media/vs-azure-tools-connected-services-storage/IC796704.png)

1. Een nieuwe opslag-account maken: 

    1. Klik op de knop **een nieuwe opslag-Account maken** onder in het dialoogvenster Azure opslag.

    1. Vul het dialoogvenster **Opslag Account maken** en kies vervolgens de knop **maken** .
    
        ![Azure opslag, dialoogvenster](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)

        Wanneer u in het dialoogvenster **Azure opslag** , wordt de nieuwe opslag weergegeven in de lijst.

    1. Nieuwe opslag selecteren in de lijst en selecteer **toevoegen**.

1. De aangesloten storage-service wordt weergegeven onder het knooppunt met verwijzingen naar de Service van het project WebJob.

    ![Azure opslag in taken webproject](./media/vs-azure-tools-connected-services-storage/IC796705.png)

1. Bekijk de pagina aan de slag en leer hoe het project is gewijzigd. Een pagina aan de slag wordt weergegeven in uw browser wanneer u een service verbonden toevoegt. U kunt de codevoorbeelden en mogelijke volgende stappen controleren of Ga naar de pagina Wat is er gebeurd om te zien welke verwijzingen zijn toegevoegd aan het project en hoe de code en configuratie-bestanden zijn gewijzigd.

## <a name="how-your-project-is-modified"></a>Hoe het project is gewijzigd

Wanneer u klaar bent met het dialoogvenster is Visual Studio voegt verwijzingen en bepaalde configuratiebestanden worden gewijzigd. De specifieke wijzigingen zijn afhankelijk van het projecttype. 

 - Zie [Wat is er gebeurd: ASP.NET-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513126)voor ASP.NET-projecten. 
 - Zie [Wat is er gebeurd: ASP.NET 5 projecten](http://go.microsoft.com/fwlink/p/?LinkId=513124)voor ASP.NET 5-projecten. 
 - Serviceprojecten (web-rollen en functies van de werknemer), Zie [Wat is er gebeurd – Cloud Service projecten](http://go.microsoft.com/fwlink/p/?LinkId=516965)voor wolk. 
 - Zie [Wat is er gebeurd - WebJob projecten](./storage/vs-storage-webjobs-what-happened.md)voor projecten met een WebJob.

## <a name="next-steps"></a>Volgende stappen

1. Aan de slag met codevoorbeelden als leidraad, de typen opslag die u wilt maken, en start schrijven van code voor toegang tot uw account opslag!

1. Vragen stellen en Help-informatie opvragen
     - [MSDN-Forum: Azure opslag](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)

     - [Azure opslag-teamblog](http://blogs.msdn.com/b/windowsazurestorage/)

     - [Opslag in azure.microsoft.com](https://azure.microsoft.com/services/storage/)

     - [Opslag-documentatie op azure.microsoft.com](https://azure.microsoft.com/documentation/services/storage/)

