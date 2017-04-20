<properties 
    pageTitle="Aan de slag met Azure Active Directory en Visual Studio verbonden services (MVC projecten) | Microsoft Azure" 
    description="Hoe aan de slag met Azure Active Directory in MVC na verbinding te maken met projecten of services maken een Azure AD met behulp van Visual Studio worden verbonden" 
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

# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Aan de slag met Azure Active Directory en Visual Studio verbonden services (MVC projecten)

> [AZURE.SELECTOR]
> - [Aan de slag](vs-active-directory-dotnet-getting-started.md)
> - [Wat is er gebeurd](vs-active-directory-dotnet-what-happened.md)
 
##<a name="requiring-authentication-to-access-controllers"></a>Toegang tot domeincontrollers-verificatie vereist 

Alle domeincontrollers in uw project zijn adorned met het kenmerk **machtigen** . Dit kenmerk moet de gebruiker worden geverifieerd voordat toegang tot deze controllers. Als u wilt dat de controller voor anonieme toegang, dit kenmerk te verwijderen uit de controller. Als u wilt dat de machtigingen ingesteld op een meer gedetailleerd niveau, van toepassing het kenmerk bij elke methode waarvoor de vergunning toepast op de controller-klasse.
 
##<a name="adding-signin--signout-controls"></a>Aanmelding toe te voegen / SignOut besturingselementen 

Toevoegen van een aanmelding/SignOut besturingselementen aan uw weergave, kunt u de **_LoginPartial.cshtml** gedeeltelijke weergave de functionaliteit toevoegen aan een van de weergaven. Hier volgt een voorbeeld van de functionaliteit die wordt toegevoegd aan de standaard **_Layout.cshtml** . (Let op het laatste element in de klasse navbar samenvouwen-div):

<pre>
    &lt;! DOCTYPE html&gt; 
&lt;html&gt; 
&lt;head&gt; 
&lt;meta charset="utf-8" /&gt; 
&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
&lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
@Styles.Render("~/Content/css") @Scripts.Render("~/bundles/modernizr") &lt;/head&gt; 
&lt;body&gt; 
&lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
&lt;div class="container"&gt; 
&lt;div class="navbar-header"&gt; 
&lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;/button&gt; 
@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) &lt;/div&gt; 
&lt;div class="navbar-collapse collapse"&gt; 
&lt;ul class="nav navbar-nav"&gt; 
&lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
&lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/ div&gt; 
&lt;/div&gt; 
&lt;/div&gt; 
&lt;div class="container body-content"&gt; 
@RenderBody() &lt;hr /&gt; 
&lt;footer&gt; 
&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
&lt;/footer&gt; 
&lt;/div&gt; 
@Scripts.Render("~/bundles/jquery") @Scripts.Render("~/bundles/bootstrap") @RenderSection("scripts", required: false) &lt;/body&gt; 
&lt;/html                                                                                                                                                                                                                                                                                                                                                                                                                                                           &gt;
</pre>

[Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 
