<properties
   pageTitle="Een Azure-project maken met Visual Studio | Microsoft Azure"
   description="Een Azure-project maken met Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="creating-an-azure-project-with-visual-studio"></a>Een Azure-Project maken met Visual Studio

De Azure Tools voor Visual Studio bieden een sjabloon die u kunt maken van een cloud service voor Azure. De hulpmiddelen helpen u bij het configureren en implementeren van de cloud-service op Azure debug.

Een oplossing voor Azure cloud-service bevat de volgende soorten projecten:

- **Azure project**

    Het project Azure heeft koppelingen met de rol van projecten in de oplossing. Bevat ook de definitie van service en service-configuratiebestanden. Het definitiebestand definieert de runtime-instellingen voor uw toepassing met inbegrip van welke rollen zijn vereist, eindpunten en grootte van de virtuele machine. Het configuratiebestand van de service wordt geconfigureerd voor hoeveel exemplaren van een functie worden uitgevoerd en de waarden van de instellingen die zijn gedefinieerd voor een rol. Zie voor meer informatie over deze instellingen [hoe: de rollen configureren voor een Azure Cloud Service met Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

- **Webproject rol**

    De rol van een werknemer uitvoert achtergrondverwerking. De rol van een werknemer kan communiceren met storage-services en andere internetservices. De rol van een werknemer kan een willekeurig aantal HTTP-, HTTPS- of TCP-eindpunten hebben.

    - **ASP.NET Webrol**voor het bouwen van een ASP.NET-toepassing met een web-front-end
    - **Rol van MVC5 ASP.NET Web**
    - **Rol van MVC4 ASP.NET Web**
    - **Rol van MVC3 ASP.NET Web**
    - **De rol van de Web Service WCF**voor het bouwen van een WCF-service
    - **Silverlight Business Application Webrol** (vereist Visual Studio 2012)

- **Functie van de werknemer cache**

    Een rol met een speciale cache voor uw toepassing.

- **De rol van de werknemer met Bus-mailwachtrij Service**

    Een service bus wachtrij functionaliteit voor message queuing biedt te communiceren met het werkproces. Zie [wachtrijen Bus gebruiken](http://go.microsoft.com/fwlink/?LinkId=260560)voor meer informatie.

## <a name="to-create-an-azure-cloud-service-project-in-visual-studio"></a>Voor het maken van een Azure cloud service-project in Visual Studio

1. Microsoft Visual Studio starten als administrator.

1. Kies in het menu **bestand**, **Nieuw** **Project**.

1. Kies **wolk** in de Visual C# of Visual Basic-project sjabloon knooppunten in het deelvenster **Project Types** .

1. Kies in het deelvenster **sjablonen** **Azure Cloud-Service**.

1. Opgeven welke versie van het .NET Framework die u gebruiken wilt voor het ontwikkelen van uw project.

1. Voer een naam en locatie voor uw project en een naam voor de oplossing. Klik op de knop **OK** .

1. Klik in het dialoogvenster **Nieuw Azure Project** kiest u de rollen die u wilt toevoegen en klik op de knop pijl-rechts te voegen aan uw oplossing. U kunt zo veel rollen toevoegen.

1. Plaats de muisaanwijzer op de rol in het dialoogvenster **Nieuwe Azure Project** wilt wijzigen in een rol die u hebt toegevoegd aan uw project, en kiest u de **naam van** pictogram aan de rechterkant van de rol. U kunt ook een rol in uw oplossing wijzigen nadat deze is toegevoegd.
