<properties
   pageTitle="Standaard tijdelijke map is te klein voor een rol | Microsoft Azure"
   description="De rol van een cloud-service heeft een beperkte hoeveelheid ruimte voor de map TEMP. Dit artikel bevat enkele tips voor het vermijden van ruimtegebrek."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/12/2016"
   ms.author="v-six" />

# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Standaard is tijdelijke map te klein voor een cloud service web/werknemer functie

De standaard tijdelijke map van een cloud service werknemer of web-rol heeft een maximale grootte van 100 MB, die op een bepaald moment volledig kan worden. In dit artikel wordt beschreven hoe om te voorkomen dat met de beschikbare ruimte voor de tijdelijke map.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Waarom kan ik geen ruimte meer uitvoeren?

De standaard Windows-omgevingsvariabelen TEMP en TMP zijn beschikbaar voor code die in uw toepassing wordt uitgevoerd. Zowel TEMP en TMP verwijzen naar één map een maximale grootte van 100 MB heeft. Alle gegevens die zijn opgeslagen in deze map wordt niet behouden in de levenscyclus van de cloud-dienst; Als de rol van exemplaren in een cloud-service gerecycled worden, wordt de map verwijderd.

## <a name="suggestion-to-fix-the-problem"></a>De suggestie om het probleem te verhelpen

Implementeren op een van de volgende alternatieven:

- Een bron van lokale opslag configureren en rechtstreeks in plaats van tijdelijk of TMP toegang. Voor toegang tot een bron van lokale opslag van code die wordt uitgevoerd binnen de toepassing, roept u de methode [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) . 

- Een bron van lokale opslag configureren en wijst u de mappen TEMP en TMP om te verwijzen naar het pad van de bron van de lokale opslag. Deze wijziging moet worden uitgevoerd binnen de methode [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) .

In het volgende voorbeeld ziet u hoe de doel mappen voor TEMP en TMP uit binnen de methode OnStart wijzigen:


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Lees een blog, waarin wordt beschreven [hoe u de grootte van de Azure rol ASP.NET tijdelijke map](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Bekijk meer [artikelen probleemoplossing](/?tag=top-support-issue&product=cloud-services) voor cloud services.

Als u wilt weten hoe u problemen met cloud service rol met Azure PaaS computer diagnostische gegevens, weergeven [van Kevin Williamson blog-serie](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
