<properties
    pageTitle="Logica apps migreren naar schema versie 2015-08-01-preview | Microsoft Azure App Service"
    description="Eenvoudig kunt u uw apps logica migreren naar de meest recente versie. Volg deze stappen."
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="deonhe"/>

# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Logica apps migreren naar schema versie 2015-08-01-voorbeeld

Als u wilt uw bestaande logica apps verplaatst naar het nieuwe schema, het volgende doen:  
1. Open uw app logica in Azure portal  
2. Klik op Update Schema:

 ![API-pictogram][step1]   
De pagina Schema-Update wordt weergegeven en bevat een koppeling naar een document met details over de verbeteringen in het nieuwe schema: ![API-pictogram][step2]

>[AZURE.NOTE] Wanneer u **Update Schema**selecteert, we automatisch migratiestappen uitvoert en de uitvoer van code voorzien. U kunt dit echter uw definitie, bijwerken, controleren dat u goede coding praktijken zoals beschreven in de sectie **aanbevelingen** Hieronder volgen.

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Aanbevolen procedures bij het migreren van uw apps logica om de meest recente versie:  

- Kopieer het script gemigreerd naar een nieuwe logica App - niet overschrijven de oude totdat u klaar bent met testen en bevestigd dat de gemigreerde app werkt zoals verwacht.
- Test uw logica app **voordat** deze in de productie
- Nadat de migratie is voltooid, start u het bijwerken van uw apps logica voor het gebruik van de [beheerde API's](./apis-list.md) waar mogelijk. U kunt bijvoorbeeld starten met behulp van Dropbox v2, gebruikt u DropBox v1 ziekte.


## <a name="whats-next"></a>Wat is het volgende
-  [Meer informatie over het handmatig migreren uw logica apps](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






