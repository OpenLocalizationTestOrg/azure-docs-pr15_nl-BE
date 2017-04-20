<properties 
    pageTitle="Fout tijdens verificatie-detectie" 
    description="De wizard active directory-verbinding een incompatibele verificatietype gevonden" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="error-during-authentication-detection"></a>Fout tijdens verificatie-detectie

De wizard gedetecteerd tijdens het opsporen van vorige verificatiecode een verificatietype niet compatibel.   

##<a name="what-is-being-checked"></a>Wat wordt gecontroleerd?

**Opmerking:** Om het correct vaststellen van vorige verificatiecode in een project, moet het project worden opgebouwd.  Als u deze fout is opgetreden en u niet een eerdere verificatiecode in uw project hoeft, opnieuw en probeer het opnieuw.

###<a name="project-types"></a>Projecttypen

De wizard controleert welk type project ontwikkelt zodat deze de juiste verificatie-logica in het project invoeren kunt.  Als er een domeincontroller die is afgeleid van `ApiController` in het project, het project zal worden beschouwd als een project WebAPI.  Als er alleen domeincontrollers die zijn afgeleid van `MVC.Controller` in het project, het project zal worden beschouwd als een MVC-project.  Iets anders wordt niet ondersteund door de wizard.  Webformulieren projecten worden momenteel niet ondersteund.

###<a name="compatible-authentication-code"></a>Compatibele verificatiecode

De wizard controleert ook of er verificatie-instellingen die eerder zijn geconfigureerd met de wizard of compatibel zijn met de wizard.  Als alle instellingen aanwezig zijn, wordt geacht een herhaalde aanvraag en de wizard openen en de instellingen worden weergegeven.  Als er slechts enkele instellingen aanwezig zijn, wordt geacht het geval van een fout.

De wizard controleert in een MVC-project voor een van de volgende instellingen die het gevolg van vroeger gebruik van de wizard:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

De wizard controleert bovendien voor een van de volgende instellingen in een project Web API die het gevolg van vroeger gebruik van de wizard:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

###<a name="incompatible-authentication-code"></a>Incompatibele verificatiecode

Ten slotte probeert de wizard voor het detecteren van versies van verificatiecode die met eerdere versies van Visual Studio zijn geconfigureerd. Als u dit foutbericht hebt ontvangen, betekent dit dat uw project bevat een verificatietype niet compatibel. De wizard detecteert de volgende typen verificatie uit vorige versies van Visual Studio:

* Windows-verificatie 
* Afzonderlijke gebruikersaccounts 
* Organisatie-Accounts 
 

Windows-verificatie in een project MVC detecteren, de wizard zoekt de `authentication` -element van het bestand **web.config** .

<pre>
    &lt;configuratie&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode = "Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/Configuration&gt;
</pre>

Windows-verificatie in een project Web API detecteren, de wizard zoekt de `IISExpressWindowsAuthentication` element uit uw projectbestand **.csproj** :

<pre>
    &lt;Project&gt;
&lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;ingeschakeld&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup > &lt;/Project        &gt;
</pre>

Verificatie van afzonderlijke gebruikersaccounts detecteren, zoekt de wizard naar het element package uit het bestand **Packages.config** .

<pre>
    &lt;pakketten&gt;
        <span style="background-color: yellow">&lt;id="Microsoft.AspNet.Identity.EntityFramework pakket ' versie"2.1.0"= targetFramework ="net45"/&gt;</span>
    &lt;/pakketten&gt;
</pre>

Een oude vorm van verificatie van de organisatie-Account detecteren, zoekt de wizard naar het volgende element van **web.config**:

<pre>
    &lt;configuratie&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;sleutel = "ida: Realm" value = "***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/Configuration&gt;
</pre>

Het verificatietype wijzigen, verwijdert u het verificatietype niet compatibel en de wizard opnieuw uitvoeren.

Zie [Verificatie-scenario's voor Azure advertentie](active-directory-authentication-scenarios.md)voor meer informatie.