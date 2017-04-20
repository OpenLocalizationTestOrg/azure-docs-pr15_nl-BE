<properties
   pageTitle="Aan de slag met SQL Database dynamische gegevens maskeren (Azure Portal)"
   description="Om te beginnen met de SQL-Database dynamische gegevens maskeren in Azure Portal"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>


# <a name="get-started-with-sql-database-dynamic-data-masking-azure-portal"></a>Aan de slag met SQL Database dynamische gegevens maskeren (Azure Portal)

> [AZURE.SELECTOR]
- [Dynamische gegevens maskeren - Azure klassieke Portal](sql-database-dynamic-data-masking-get-started-portal.md)

## <a name="overview"></a>Overzicht

SQL Database dynamische gegevens-maskering blootstelling van gevoelige gegevens beperkt door het voor onbevoegde gebruikers te maskeren. Maskeren voor dynamische gegevens wordt voor de V12-versie van Azure SQL-Database ondersteund.

Maskeren van dynamische gegevens kunt onbevoegde toegang tot vertrouwelijke gegevens voorkomen doordat klanten om aan te geven hoeveel van de gevoelige gegevens blijkt dat er met een minimale impact op de toepassingslaag. Het is een beveiligingsfunctie op basis van beleid dat de gevoelige gegevens in de resultaatset van een query over aangewezen databasevelden, verbergt terwijl de gegevens in de database niet is gewijzigd.

Bijvoorbeeld een medewerker in een callcenter bellers kan identificeren door verschillende cijfers van hun sofi-nummer of creditcardnummer, maar deze gegevensitems niet volledig worden blootgesteld aan de klantenservice. Een maskerend regel kan worden gedefinieerd dat alle maskers, maar de laatste vier cijfers van een sofi-nummer of creditcardnummer in het resultaat van een query ingesteld. Bijvoorbeeld kan een masker voor de juiste gegevens worden gedefinieerd als persoonlijk identificeerbare informatie (PII)-gegevens te beschermen die een ontwikkelaar kan een query productieomgevingen voor het oplossen van problemen de regelgeving overtreden.

## <a name="sql-database-dynamic-data-masking-basics"></a>Grondbeginselen van SQL-Database dynamische gegevens-maskeren

U instellen kunt een dynamische gegevens beleid in de Portal Azure maskeren door de bewerking maskeren van dynamische gegevens in uw SQL-Database configuratie blade of blade instellingen.


### <a name="dynamic-data-masking-permissions"></a>Machtigingen voor het maskeren van dynamische gegevens

Maskeren van dynamische gegevens kan worden geconfigureerd door admin Azure Database server admin of officier beveiligingsrollen.

### <a name="dynamic-data-masking-policy"></a>Beleid voor dynamische gegevens maskeren

* **SQL-gebruikers uitgesloten van maskeren** - een reeks SQL-gebruikers- of AAD identiteiten die niet-gemaskeerde gegevens in de resultaten van de SQL-query krijgen. Opmerking: gebruikers met administrator-bevoegdheden worden altijd uitgesloten van maskeren en ziet de oorspronkelijke gegevens zonder een masker.

* **Regels maskeren** - een set regels die bepalen de daartoe aangewezen velden die moeten worden gemaskerd en de maskerende functie die wordt gebruikt. Bepaalde velden kunnen worden gedefinieerd met behulp van een database schemanaam, naam van de tabel en kolomnaam.

* **Maskeren functies** - een set methoden waarmee de blootstelling van gegevens voor verschillende scenario's.

| Maskeren, functie | Logica maskeren |
|----------|---------------|
| **Standaard**  |**Volledige maskeren volgens de gegevenstypen van de desbetreffende velden**<br/><br/>• Gebruik XXXX of minder Xs als het formaat van het veld minder dan 4 tekens voor de gegevenstypen tekenreeks (nchar, ntext, nvarchar is).<br/>• Gebruik de waarde nul voor numerieke gegevenstypen (bigint, bits, decimal, int, money, Numeriek, smallint, smallmoney, tinyint, float, real).<br/>• 01-01-1900 te gebruiken voor de gegevenstypen Datum/tijd (datum, datetime2, datetime, datetimeoffset, smalldatetime, tijd).<br/>• Voor SQL variant, de standaardwaarde van het huidige type wordt gebruikt.<br/>• Voor het document-XML <masked/> wordt gebruikt.<br/>• Gebruik een lege waarde voor speciale gegevenstypen (tijdstempel tafel, activiteitknooppunt, GUID, binair bestand, afbeelding, ruimtelijke typen varbinary).
| **Creditcard** |**Methode die de laatste vier cijfers van de daartoe aangewezen velden beschrijft maskeren** en voegt een constante tekenreeks als voorvoegsel in de vorm van een creditcard.<br/><br/>XXXX-XXXX-XXXX-1234|
| **Sofi-nummer** |**Methode die de laatste vier cijfers van de desbetreffende velden beschrijft te maskeren** en een constante tekenreeks als voorvoegsel in de vorm van een Amerikaans sofi-nummer toegevoegd.<br/><br/>XX-XXX-1234 |
| **E-mail** | **Methode die beschrijft de eerste letter en het domein met XXX.com vervangt maskeren** met een constant string voorvoegsel in de vorm van een e-mailadres.<br/><br/>aXX@XXXX.com |
| **Willekeurig getal** | **Methode die een aselect getal genereert maskeren** volgens de geselecteerde grenzen en de werkelijke gegevenstypen. Als de grenzen van de aangewezen gelijk zijn, worden de functie maskeren een constante waarde.<br/><br/>![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Aangepaste tekst** | **Methode die de eerste en laatste tekens beschrijft te maskeren** en een opvulling met aangepaste tekenreeks toegevoegd in het midden. Als de oorspronkelijke tekenreeks korter dan de blootgestelde prefix en suffix is, is alleen de tekenreeks opvulling wordt gebruikt. <br/>achtervoegsel voorvoegsel [opvulling]<br/><br/>![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>
### <a name="recommended-fields-to-mask"></a>Aanbevolen velden die u wilt maskeren

De engine DDM aanbevelingen vlaggen bepaalde velden uit de database als potentieel gevoelige velden die goede kandidaten voor maskeren zijn. In het blad maskeren van dynamische gegevens in de portal ziet u de aanbevolen kolommen voor uw database. U hoeft alleen **Masker toevoegen** voor een of meer kolommen en klik vervolgens op **Opslaan** klikt u op om een masker voor deze velden van toepassing is.

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Maskeren voor uw database met behulp van de Portal Azure dynamische gegevens instellen

1. Start de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

2. Ga naar het blad van de instellingen van de database met de gevoelige gegevens die u wilt maskeren.

3. Klik op de **Dynamische gegevens maskeren** tegel die de configuratie van **Dynamische gegevens maskeren** blade wordt gestart.

    * U kunt ook Ga naar de sectie **bewerkingen** en klikt u op **Dynamische gegevens maskeren**.

    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>


4. In de configuratie van **Dynamische gegevens maskeren** blade ziet u enkele kolommen dat de motor aanbevelingen voor maskeren is gemarkeerd. Om de aanbevelingen accepteert, klikt u op **Masker toevoegen** voor een of meer kolommen en masker wordt gemaakt op basis van het type voor deze kolom. U kunt de functie maskeren wijzigen door te klikken op de regel maskeren en bewerken van het maskerende veldindeling naar een andere indeling van uw keuze. Zorg ervoor dat klikt u op **Opslaan** om uw instellingen te slaan.

    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>


5. Als u een masker voor een kolom in uw database toevoegen, aan de bovenkant van de configuratie van **Dynamische gegevens maskeren** blade klikt u op **Masker toevoegen** om te openen de **Maskering regel toevoegen** configuratie blade

    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>

6. Selecteer het **Schema**, **tabel** - en **kolomnamen** voor het definiëren van het veld dat is opgegeven dat gemaskeerd.

7. Kies een **Veldindeling maskeren** in de lijst van gevoelige gegevens in categorieën te maskeren.

    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>     

8. Klik op **Opslaan** in de regel blade om bij te werken van de verzameling van regels in de dynamische gegevens beleid maskeren maskeren maskeren.

9. De SQL-gebruikers of AAD identiteiten die moeten worden uitgesloten van het maskeren en toegang hebben tot de niet-gemaskerde gevoelige gegevens typt. Dit moet een door puntkomma's gescheiden lijst met gebruikers. Houd er rekening mee dat gebruikers met administrator-bevoegdheden altijd toegang tot de oorspronkelijke gegevens zonder masker hebben.

    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)

    >[AZURE.TIP] Als u wilt ervoor zorgen dat gevoelige gegevens voor gebruikers van toepassingen die rechten kan worden weergegeven in de application-laag, de SQL-gebruiker toevoegen of AAD identiteit de toepassing wordt gebruikt om de query. Het is raadzaam dat deze lijst een minimum aantal geprivilegieerde gebruikers tot een minimum beperken van blootstelling van gevoelige gegevens bevatten.

10. Klik op **Opslaan** in het maskeren blade configureren om op te slaan van het beleid van het maskeren van nieuwe of bijgewerkte gegevens.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Dynamische gegevens voor de database met Powershell-cmdlets maskeren instellen

Zie [Cmdlets voor Azure SQL-Database](https://msdn.microsoft.com/library/azure/mt574084.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Maskeren voor uw database met behulp van REST API dynamische gegevens instellen

Zie [Operations voor Azure SQL-Databases](https://msdn.microsoft.com/library/dn505719.aspx).
