<properties
    pageTitle="Beheren en controleren van uw verbindingslijnen en Apps in de App-Service API | Microsoft Azure"
    description="Weergave prestaties van uw verbindingslijnen en API Apps in Apps logica; microservices architectuur"
    services="app-service\logic"
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger"
    manager="anneta"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="mandia"/>

# <a name="manage-and-monitor-your-built-in-api-apps-and-connectors"></a>De ingebouwde API Apps en de verbindingen controleren en beheren

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op de schemaversie van logica apps 2014-12-01-voorbeeld.

U hebt gemaakt met een ingebouwde API App. Wat nu?

Elke App API is in Azure, een aparte website op Azure. U kunt daardoor eenvoudig zien hoeveel aanvragen worden gedaan en Zie hoeveel gegevens door de connector wordt gebruikt. U kunt ook back-up van uw App API, waarschuwingen maken folie beveiliging ingeschakeld en gebruikers en rollen toevoegen.

In dit onderwerp worden enkele van de verschillende opties voor het beheren van uw App API.

Open uw App API in [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)overzicht van deze ingebouwde functies. Als de API App op uw startboard, selecteert u deze om de eigenschappen te openen. U kunt ook selecteren **Bladeren**, **API Apps**selecteren en selecteert u uw App API:

![][browse]

## <a name="see-the-properties-you-entered"></a>Zie de eigenschappen die u hebt opgegeven

Wanneer u de API App opent, zijn er verschillende functies en taken beschikbaar:

![][settings]

U kunt:

- **Instellingen** bevat specifieke informatie van de API-App, met inbegrip van de details van uw abonnement en bevat de gebruikers die toegang tot uw app API hebben. Ook vergroten of verkleinen van het aantal exemplaren van de API-App met de functie schaal; onder andere functies.
- Gebruik de knoppen **starten** en **stoppen van** de API App beheren.
- Wanneer u updates van het product worden aangebracht in de onderliggende bestanden die door uw App API gebruikt, kunt u **bijwerken** om de nieuwste versies ophalen. Bijvoorbeeld als er een correctie of een beveiligingsupdate die is uitgebracht door Microsoft, werkt klikt u op **bijwerken** automatisch uw App API met deze correctie.
- Selecteer **Wijziging plannen** upgrade of downgrade op basis van het gebruik van de gegevens van de API-App. U kunt deze functie ook gebruiken om te zien van het gebruik van uw gegevens.
- Als u een connector tabellen bevat, net als de SQL-connector kunt u desgewenst verbinding maken met de naam van een tabel. Een schema op basis van de tabel wordt automatisch gemaakt en is beschikbaar wanneer u klikt op **Schema's downloaden**. Vervolgens kunt u dit schema gedownload een transformatie of een kaart maken.

## <a name="change-your-connector-or-api-configuration-values-you-entered"></a>De connector of de opgegeven waarden API-configuratie wijzigen

Nadat u een geconfigureerd of de ingebouwde connector gemaakt, kunt u de waarden die u hebt opgegeven. Bijvoorbeeld, als u de SQL-Connector geconfigureerd en u wilt de naam van SQL Server of de tabelnaam wijzigen, hiervoor in de blade-API App voor de verbindingslijn.

Stappen zijn:

1. Open de connector of API App. Wanneer u dit doet, wordt het blad API App geopend.
2. In de **Essentials**, klikt u op de hyperlink onder de eigenschap Host. De hyperlink de naam iets zoals *slackconnector* of *microsoftsqlconnector123*:

    ![][apiapphost]

3. Selecteer **Instellingen**in het blad API App Host. Selecteer **Instellingen voor toepassing**in de blade instellingen. De waarden van de systeemconfiguratie worden vermeld onder het **App-instellingen**:

    ![][hostsettings]

4. Klik op de instelling die u wilt wijzigen, typt u de nieuwe waarde en **Sla** de wijzigingen.


## <a name="install-the-hybrid-connection-manager---optional"></a>De hybride Connection Manager - optioneel installeren

![][hcsetup]

De hybride Connection Manager kunt u verbinding maken met een systeem van gebouwen, zoals SQL Server of SAP. Deze hybride connectiviteit wordt Azure Service Bus gebruikt om verbinding te maken en zo de besturing van de beveiliging tussen uw Azure resources en resources in ruimten.

Zie [werken met Verbindingsbeheer hybride in Azure App-Service](app-service-logic-hybrid-connection-manager.md).

> [AZURE.NOTE] Hybride Verbindingsbeheer is alleen vereist als u verbinding met een bron van lokalen achter uw firewall maakt. Als u niet een verbinding naar een systeem van gebouwen, kan de hybride Connection Manager niet worden weergegeven in de blade connector.

## <a name="monitor-the-performance"></a>De prestaties controleren
Prestatiegegevens worden ingebouwde functies en van elke API App u wilt maken. Deze gegevens zijn specifiek voor uw API App gehost in Azure. Monster metrics:

![][monitoring]

U kunt:

- Selecteer de **verzoeken en fouten** toe te voegen andere prestatiegegevens met inbegrip van de algemeen bekende HTTP-foutcodes, zoals 200, 400 of 500 HTTP-statuscodes. U kunt ook Zie responstijden, zien hoeveel aanvragen in de App-API worden aangebracht, en hoeveel gegevens afkomstig zijn en hoeveel gegevens gaat uit. Op basis van de prestatiegegevens, kunt u e-mail waarschuwingen als de metric-waarde groter is dan een drempel van uw keuze.
- In het **Gebruik**, kunt u zien hoeveel **CPU** wordt gebruikt door de API App, bekijk de huidige **Gebruiksgegevens** in MB en het gebruik van uw gegevens op basis van de kosten laag. **Geschatte uitgaven** kunt u bepalen de potentiële kosten van het uitvoeren van uw App API.
- Selecteer de **processen** voor het openen van Process Explorer. Dit geeft de exemplaren van uw website en de bijbehorende eigenschappen, met inbegrip van de thread count en geheugengebruik bekijken.

Met deze hulpprogramma's, kunt u bepalen als de App Service planning moet worden vergroot of verkleind, op basis van uw bedrijfsbehoeften. Deze functies zijn ingebouwd in de portal met geen extra hulpprogramma's.

## <a name="control-the-security"></a>De beveiliging

API Apps op rollen gebaseerde beveiliging gebruiken. Deze rollen zijn van toepassing op de hele Azure ervaring, waaronder API Apps en andere Azure bronnen. De rollen zijn:

Rol | Beschrijving
--- | ---
Eigenaar | Volledige toegang tot de management ervaring en aan andere gebruikers of groepen kunt krijgen.
Inzender | Volledige toegang hebben tot de management-ervaring. Kan geen toegang verlenen aan andere gebruikers of groepen.
Reader | Ziet alle bronnen met uitzondering van geheimen.
Beheerder toegang | Alle bronnen, rollen maken/beheren kunt bekijken en tickets maken/beheren ondersteunen.

Zie [op rollen gebaseerde toegangscontrole in de portal voor Microsoft Azure](../active-directory/role-based-access-control-configure.md).

U kunt gemakkelijk gebruikers toevoegen en deze specifieke rollen toewijzen aan uw App API. De portal ziet u de gebruikers die toegang hun toegewezen rol en:

![][access]  

- Selecteer **gebruikers** , gebruikers toevoegen en verwijderen van een gebruiker een rol toewijzen.
- Selecteer de **rollen** voor een overzicht van alle gebruikers in een specifieke rol van een gebruiker aan een rol toevoegen en een gebruiker uit een rol verwijderen.


## <a name="more-good-stuff"></a>Meer goede dingen
- **Definitie van API** voor het openen van het Swagger-bestand automatisch gemaakt voor uw specifieke API app selecteren.
- **Afhankelijkheden** weergeven van de bestanden die nodig zijn door de API App selecteren. Installeren, bijvoorbeeld als u de SAP-connector, extra bestanden op op ruimten hybride Connection Manager. Deze afhankelijkheden worden weergegeven in uw app API blade.

>[AZURE.IMPORTANT] Wanneer u de eigenschappen van de API-app openen en zoek onder **Essentials**, zijn er **Host** en **Gateway** koppelingen nieuwe bladen openen:
>
> ![][host]
>
>Deze eigenschappen zijn specifiek voor de website waarop uw App API. Wanneer u een ingebouwde API App of connector, de meeste van deze eigenschappen zijn niet echt van toepassing en wordt aangeraden dat u deze eigenschappen niet bijwerken. Als u uw eigen API App gemaakt in Visual Studio geïnstalleerd op uw abonnement Azure, kunt u de Host- en Gateway-blades. <br/><br/>


>[AZURE.NOTE] Ga naar [Probeer logica App](https://tryappservice.azure.com/?appservice=logic)aan de slag met Apps logica voor het aanmelden voor een account met Azure. U kunt een tijdelijk starter logica app maken. Geen creditcard vereist en geen verplichtingen.

## <a name="read-more"></a>Lees meer

[De logica Apps controleren](app-service-logic-monitor-your-logic-apps.md)<br/>
[Verbindingslijnen en API Apps lijst in App-Service](app-service-logic-connectors-list.md)<br/>
[Toegangsbeheer op basis van rollen in de portal voor Microsoft Azure](../active-directory/role-based-access-control-configure.md)<br/>
[Met behulp van de hybride Connection Manager in Azure App-Service](app-service-logic-hybrid-connection-manager.md)


<!--Image references-->
[browse]: ./media/app-service-logic-monitor-your-connectors/browse.png
[settings]: ./media/app-service-logic-monitor-your-connectors/settings.png
[hcsetup]: ./media/app-service-logic-monitor-your-connectors/hcsetup.png
[monitoring]: ./media/app-service-logic-monitor-your-connectors/monitoring.png
[access]: ./media/app-service-logic-monitor-your-connectors/access.png
[host]: ./media/app-service-logic-monitor-your-connectors/host.png
[hostsettings]: ./media/app-service-logic-monitor-your-connectors/hostsettings.png
[apiapphost]: ./media/app-service-logic-monitor-your-connectors/apiapphost.png
