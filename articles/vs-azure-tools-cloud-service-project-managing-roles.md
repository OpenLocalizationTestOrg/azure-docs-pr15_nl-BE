<properties
   pageTitle="Beheren van rollen in de Azure cloud services-projecten met Visual Studio | Microsoft Azure"
   description="Informatie over nieuwe functies toevoegen aan uw project Azure cloud-service of het verwijderen van bestaande functies uit met behulp van Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-roles-in-the-azure-cloud-services-projects-with-visual-studio"></a>Beheren van rollen in de Azure cloud services-projecten met Visual Studio

Nadat u uw project Azure cloud-service hebt gemaakt, kunt u nieuwe functies aan toe te voegen of bestaande rollen verwijderen. U kunt ook een bestaand project importeren en converteren naar een rol. Zo kunt u een ASP.NET-webtoepassing importeren en opgeven dat het Webrol van een.

## <a name="adding-or-removing-roles"></a>Toevoegen of verwijderen van rollen

**Een rol toe te voegen**

Open het snelmenu voor het knooppunt **rollen** in de cloud service-project in de **Solution Explorer**en kies **toevoegen**. U kunt een bestaande Webrol of functie van de werknemer uit de huidige oplossing of maak een nieuw project voor web of werknemer rol. Of u kunt een passende project, zoals een ASP.NET web application-project selecteren en koppelen aan een project voor de rol.

**De rolkoppeling van een verwijderen**

Open het snelmenu voor de rol die u wilt verwijderen en kies **verwijderen**in het knooppunt **rollen** van de cloud service project in de Solution Explorer.

## <a name="removing-and-adding-roles-in-your-cloud-service"></a>Verwijderen en rollen in de cloud-service toevoegen

Als u een rol uit uw project cloud-service verwijderen, maar later besluit om de rol weer toevoegen aan het project, worden alleen de verklaring van de rol en de fundamentele kenmerken, zoals de eindpunten en diagnostische informatie toegevoegd. Geen extra bronnen of verwijzingen worden toegevoegd aan het bestand ServiceDefinition.csdef of het bestand ServiceConfiguration.cscfg. Als u deze informatie wilt, hebt u handmatig toe te voegen aan deze bestanden.

Bijvoorbeeld: u kunt een web service rol verwijderen en u later besluit rol toe te voegen dit terug in uw oplossing. Als u dit doet, treedt er een fout op. Als u wilt voorkomen dat deze fout, moet u toevoegen de `<LocalResources>` element weergegeven in de volgende XML-code in het bestand ServiceDefinition.csdef. De naam van de rol van de web service die u hebt toegevoegd aan het project als onderdeel van het kenmerk name van de **<LocalStorage>** element. In dit voorbeeld wordt de naam van de rol van web service **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het configureren van rollen in Visual Studio door het lezen van [de rollen voor een Azure Cloud Service met Visual Studio configureren](vs-azure-tools-configure-roles-for-cloud-service.md).
