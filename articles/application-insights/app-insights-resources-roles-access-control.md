<properties
    pageTitle="Bronnen, rollen en toegangsbeheer in de inzichten van toepassing"
    description="Eigenaren, medewerkers en lezers van de inzichten van uw organisatie."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/07/2016"
    ms.author="awills"/>

# <a name="resources-roles-and-access-control-in-application-insights"></a>Bronnen, rollen en toegangsbeheer in de inzichten van toepassing

U kunt bepalen wie heeft gelezen en toegang tot uw gegevens in een Visual Studio- [Toepassing inzichten]bijwerken[start], met behulp van [toegangsbeheer op basis van functies in Microsoft Azure](../active-directory/role-based-access-control-configure.md).

> [AZURE.IMPORTANT] Toegang voor gebruikers in de **resourcegroep of een abonnement** waartoe uw toepassingsbron behoort - niet in de bron zelf toewijzen. De **toepassing inzichten onderdeel Inzender** rol toewijzen. Op deze manier een uniform beheer van toegang tot het web tests en signalen samen met de bron van uw toepassing. [Meer informatie](#access).


## <a name="resources-groups-and-subscriptions"></a>Bronnen, groepen en abonnementen

Eerst enkele definities:

* **Resource** - een exemplaar van een service van Microsoft Azure. Uw toepassing inzichten bron verzamelt, analyseert en de telemetriegegevens verzonden vanuit uw toepassing worden weergegeven.  Andere soorten Azure bronnen zijn webtoepassingen, databases en VMs.

    Alle resources, terecht op de [Azure Portal][portal], log in en klik op Bladeren.

    ![Kies Bladeren, klik vervolgens op Alles of filteren op de inzichten van toepassing](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Resourcegroep** ] [ group] -elke resource deel uitmaakt van een groep. Een groep is een handige manier voor het beheren van gerelateerde bronnen, met name voor toegangsbeheer. Bijvoorbeeld in één resourcegroep kan u plaats een Web App, een bron van de inzichten van toepassing voor het controleren van de app en een resource opslag als geëxporteerde gegevens wilt behouden.


    ![Kies Bladeren, resourcegroepen, klik op een groep](./media/app-insights-resources-roles-access-control/11-group.png)

* [**Abonnement**](https://manage.windowsazure.com) - toepassing inzichten of andere Azure bronnen gebruiken wanneer u zich aanmeldt een Azure-abonnement. Elke groep behoort tot één Azure abonnement, waar u uw pakket prijs kiezen en, als het een organisatie-abonnement de leden en hun toegangsrechten kiezen.
* [**Microsoft-account** ] [ account] -de gebruikersnaam en het wachtwoord waarmee u zich kunt aanmelden bij Microsoft Azure abonnementen, XBox Live, Outlook.com en andere Microsoft-diensten.


## <a name="access"></a>Toegangsbeheer in de resourcegroep

Het is belangrijk te begrijpen dat naast de resource die u hebt gemaakt voor uw toepassing, er ook afzonderlijke verborgen bronnen voor waarschuwingen en web-tests zijn. Ze zijn gekoppeld aan dezelfde [resourcegroep](#resource-group) als uw toepassing. Mogelijk hebt u andere Azure diensten er, zoals websites of opslag ook plaatsen.

![Bronnen in de inzichten van toepassing](./media/app-insights-resources-roles-access-control/00-resources.png)

Toegang tot deze bronnen die daarom te aangeraden wordt beheren:

* Toegang beheren op het niveau van de **resourcegroep of abonnement** .
* De rol van **Inzender Application inzichten Component** aan gebruikers toewijzen. Hierdoor kunnen ze web tests, waarschuwingen en inzichten toepassing bronnen, zonder dat u toegang tot andere diensten in de groep bewerken.

## <a name="to-provide-access-to-another-user"></a>Om toegang te bieden aan een andere gebruiker

U moet hebben eigenaar op het abonnement of de resourcegroep.

De gebruiker moet beschikken over een [Microsoft-Account][account], of toegang tot hun [Organisatie Microsoft-Account](..\active-directory\sign-up-organization.md). U kunt toegang geven aan personen en aan gebruikersgroepen die zijn gedefinieerd in Azure Active Directory.

#### <a name="navigate-to-the-resource-group"></a>Ga naar de resourcegroep

De gebruiker er toevoegen.

![In een van uw toepassing resource blade, Essentials openen, open de resourcegroep en er instellingen/gebruikers selecteren. Klik op toevoegen.](./media/app-insights-resources-roles-access-control/01-add-user.png)

Of u kunt de hoofdmap en de gebruiker toevoegen aan het abonnement.

#### <a name="select-a-role"></a>Selecteer een rol

![Selecteer een rol voor de nieuwe gebruiker](./media/app-insights-resources-roles-access-control/03-role.png)

Rol | In de resourcegroep
---|---
Eigenaar | Alles, met inbegrip van de toegang van gebruikers kunt wijzigen
Inzender | Alles, met inbegrip van alle resources kunt bewerken
Toepassing inzichten onderdeel Inzender | Toepassing inzichten resources, web-tests en waarschuwingen kunt bewerken
Reader | Kunnen weergeven maar niet niets wijzigen

'Bewerken' omvat het maken, verwijderen en bijwerken:

* Bronnen
* Web-tests
* Waarschuwingen
* Doorlopende exporteren

#### <a name="select-the-user"></a>Selecteer de gebruiker


![Typ het e-mailadres van een nieuwe gebruiker. Selecteer de gebruiker](./media/app-insights-resources-roles-access-control/04-user.png)

Als de gebruiker die niet in Active directory, kunt u iedereen met een Microsoft-account uitnodigen.
(Als deze services, zoals Outlook.com, OneDrive, Windows Phone of XBox Live gebruikt, beschikken over een Microsoft-account.)



## <a name="users-and-roles"></a>Gebruikers en rollen

* [Toegangsbeheer in Azure op basis van rollen](../active-directory/role-based-access-control-configure.md)



<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md
