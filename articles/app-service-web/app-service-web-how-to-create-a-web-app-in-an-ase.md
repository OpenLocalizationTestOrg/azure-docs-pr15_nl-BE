<properties
    pageTitle="Maken van een web app in een App-omgeving"
    description="Informatie over het maken van web apps en app-serviceplannen in een App-omgeving"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="create-a-web-app-in-an-app-service-environment"></a>Maken van een web app in een App-omgeving

## <a name="overview"></a>Overzicht

Deze zelfstudie laat zien hoe web apps en App serviceplannen maken in een [App-omgeving](app-service-app-service-environment-intro.md) (ASE). 

> [AZURE.NOTE] Als u meer informatie over het maken van een web app maar hoeft niet te doen in een App-omgeving, Zie [maken een .NET web app](web-sites-dotnet-get-started.md) of een van de bijbehorende zelfstudies voor andere talen en -kaders.

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt ervan uitgegaan dat u een App-omgeving hebt gemaakt. Als u dat nog niet hebt gedaan, Zie [maken van een App-omgeving](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Maken van een web app.

1. Klik in [Azure Portal](https://portal.azure.com/)op **Nieuw > Web + Mobile > Web App**. 

    ![][1]

2. Selecteer uw abonnement.  

    Als u meerdere abonnementen worden bewust te maken van een app in de App-omgeving, moet u het hetzelfde abonnement dat u gebruikt bij het maken van de omgeving te gebruiken. 

3. Selecteer of maak een resourcegroep.

    *Resourcegroepen* kunt u verwante Azure om bronnen te beheren als een eenheid en zijn handig bij het maken van regels voor *op rollen gebaseerde toegangscontrole* (RBAC) voor uw toepassingen. Zie voor meer informatie, [het beheer van bronnen Azure][ResourceGroups]. 

4. Selecteer of maak een plan voor App-Service.

    *App serviceplannen* worden beheerd sets van web apps.  Normaal wanneer u selecteert de prijzen, wordt de prijs toegepast op het abonnement App en niet op de individuele apps. In een ASE die u betaalt voor de compute exemplaren aan de ASE toegewezen in plaats van wat u hebt opgenomen met uw ASP.  Als u wilt schalen van het aantal exemplaren van een web app dat u de schaal van de exemplaren van uw App Service plan en het geldt voor alle van het web apps in dat plan.  Sommige functies, zoals site-sleuven of VNET integratie hebben ook beperkingen in de planning.  Zie [overzicht van Azure App Service plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) voor meer informatie

    Door te kijken naar de locatie die wordt vermeld onder de naam plan kunt u de App Service plannen in uw ASE identificeren.  

    ![][5]

    Als u een serviceplan App die al in uw omgeving App-Service gebruiken wilt, selecteer het desbetreffende schema. Als u een nieuw App Service plan maken, Zie het volgende gedeelte van deze zelfstudie, [maken een planning App Service in een App-omgeving](#createplan).

5. Voer de naam in voor uw web app en klik vervolgens op **maken**. 

    De URL van een app in een ASE is als de ASE maakt gebruik van een externe VIP: [*sitenaam*]. [*naam van uw App-omgeving*]. p.azurewebsites.net in plaats van [*sitenaam*]. azurewebsites.net
    
    Als uw ASE maakt gebruik van een interne VIP en vervolgens de URL van een app in dat ASE is: [*sitenaam*]. [*subdomein opgegeven tijdens het maken van ASE*]   
    Nadat u uw ASP tijdens het maken van ASE selecteren ziet u het subdomein werken onder **de naam**

## <a name="createplan"></a>Een App Service maken

Wanneer u een App Service in een App-omgeving maken zijn uw keuzes voor de werknemer anders als er geen gedeelde werknemers in een ASE zijn.  De werknemers die u moet gebruiken, zijn de velden die zijn toegewezen aan de ASE door de beheerder.  Dit betekent dat een nieuw plan maken, moet u hebben meer werknemers die zijn toegewezen aan uw groep ASE werknemer dan het totale aantal exemplaren over alle's al in die groep werknemer.  Als er geen voldoende werknemers in uw groep ASE werknemer om uw plan te maken, moet u werken met uw admin ASE aan toegevoegd.

Een ander verschil met de App serviceplannen gehost door een App-omgeving is het ontbreken van de prijzen van de selectie.  Wanneer u een Service App omgeving hebt u betaalt voor de compute-bronnen worden gebruikt door het systeem en hebben geen extra kosten voor de plannen in die omgeving.  Selecteer Normaal gesproken bij het maken van een App serviceplan een prijsstelling plan waarmee u uw factureringsaccount bepaalt.  Een App-omgeving is in feite een persoonlijke locatie waar u inhoud kunt maken.  U betaalt voor het milieu en niet als host voor uw inhoud.

De volgende instructies wordt aangegeven hoe u een App Service maken tijdens het maken van een web app zoals uiteengezet in het vorige gedeelte van de zelfstudie.

1. Klik op **Nieuw** in de selectie plan UI en geef een naam op voor uw plan zoals u gewend buiten een ASE bent.

2. Selecteer de ASE die u wilt gebruiken vanaf uw locatie kiezen.

    Omdat een App-omgeving in feite een particuliere implementatie locatie is, ziet u onder locatie. 

    ![][2]

    Na selectie van een ASE in de locatie kiezen van plan het maken van de App Service UI werkt.  De locatie wordt nu de naam van de ASE-systeem en de regio is en de kiezer voor prijsstelling plan wordt vervangen door een werknemer van toepassingen kiezen.  

    ![][3]

### <a name="selecting-a-worker-pool"></a>Een werknemer van toepassingen selecteren

Normaal gesproken zijn in Azure App Service en buiten een Service App omgeving, er 3 compute-formaten die beschikbaar bij de selectie van een plan voor de speciale prijs zijn.  Op een vergelijkbare manier voor een ASE definiëren van maximaal 3 toepassingen van de werknemers en de compute grootte die wordt gebruikt voor die werknemer groep opgeven.  Wat dat betekent voor huurders van de ASE is dat in plaats van het selecteren van een plan prijzen met compute-formaat voor uw serviceplan App, u selecteert wat een *werknemer groep*wordt genoemd.  

De selectie van de groep werknemer UI toont de compute-grootte gebruikt voor die werknemer groep onder de naam.  De beschikbare hoeveelheid verwijst naar hoeveel exemplaren zijn beschikbaar voor gebruik in die groep berekenen.  De totale groep kan eigenlijk meer exemplaren dan deze waarde, maar deze waarde verwijst naar een gewoon hoeveel niet in gebruik zijn.  Uw App-omgeving toe te voegen meer bronnen berekenen Zie [uw App-omgeving configureren](app-service-web-configure-an-app-service-environment.md)als u wilt aanpassen.

![][4]

In dit voorbeeld ziet u twee groepen van werknemer beschikbaar. Dat komt doordat de ASE beheerder toegewezen alleen hosts in de desbetreffende werknemer twee groepen.  De derde zou weergegeven wanneer er toegewezen aan het VMs zijn.  

## <a name="after-web-app-creation"></a>Na het maken van web app

Er zijn een paar overwegingen bij het uitvoeren van webtoepassingen en het beheren van serviceplannen App in een ASE die moet rekening worden gehouden.  

Zoals eerder opgemerkt, de eigenaar van de ASE verantwoordelijk is voor de grootte van het systeem en daardoor zijn ook verantwoordelijk voor dat er voldoende capaciteit om de gewenste App serviceplannen host is. Als er geen beschikbare werknemers, is het niet mogelijk om uw App Service plan te maken.  Dit is ook waar het schalen van uw web app.  Als u meer exemplaren nodig hebt u zou uw admin App-omgeving toe te voegen meer werknemers krijgen.

Na het maken van uw web app en de App serviceplan is het een goed idee om het schalen.  In een ASE moet u altijd ten minste 2 exemplaren van uw serviceplan App voor fouttolerantie voor uw toepassingen.  Een App serviceplan schaling in een ASE is hetzelfde als normaal via de App serviceplan UI.  Voor meer informatie over de schaal, [het schalen van een web app in een App-omgeving.](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
