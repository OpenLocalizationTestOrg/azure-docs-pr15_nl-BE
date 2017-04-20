<properties
    pageTitle="Aan de slag met catalogus gegevens | Microsoft Azure"
    description="End-to-end-zelfstudie presenteert de scenario's en de mogelijkheden van Azure gegevenscatalogus."
    documentationCenter=""
    services="data-catalog"
    authors="steelanddata"
    manager="jhubbard"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/20/2016"
    ms.author="spelluru"/>

# <a name="get-started-with-azure-data-catalog"></a>Aan de slag met Azure gegevenscatalogus
Catalogus Azure gegevens is een volledig beheerde cloud service die als een systeem van registratie en het systeem van discovery voor enterprise data activa fungeert. Zie [Wat is catalogus Azure-gegevens](data-catalog-what-is-data-catalog.md)voor een gedetailleerd overzicht.

Deze zelfstudie kunt u aan de slag met Azure gegevenscatalogus. U kunt de volgende procedures uitvoeren in deze zelfstudie:

| Procedure | Beschrijving |
| :--- | :---------- |
| [De catalogus gegevens verstrekken](#provision-data-catalog) | In deze procedure, inrichten of Azure gegevenscatalogus instellen. U doen deze stap alleen als de catalogus is niet ingesteld voor. U kunt slechts één gegevenscatalogus per organisatie (Microsoft Azure Active Directory-domein) hebben, hoewel er meerdere abonnementen aan uw Azure account gekoppeld. |
| [Activa gegevens registreren](#register-data-assets) | In deze procedure kunt u elementen van de gegevens uit de voorbeelddatabase AdventureWorks2014 registreren met de catalogus met gegevens. Registratie is het proces van het ophalen van belangrijke structurele metagegevens zoals namen, typen en locaties uit de gegevensbron en het kopiëren van metagegevens in de catalogus. De gegevensbron en de gegevens activa blijven waar ze zijn, maar de metagegevens wordt gebruikt door de catalogus zodat u ze gemakkelijker kan worden gedetecteerd en te begrijpen. |
| [Ontdek de gegevens activa](#discover-data-assets) | In deze procedure kunt u de catalogus van Azure gegevens portal gegevens activa die zijn geregistreerd in de vorige stap ontdekken. Nadat u een gegevensbron met Azure gegevenscatalogus is geregistreerd, worden de metagegevens wordt geïndexeerd door de service zodat gebruikers gemakkelijk kunnen zoeken naar de gegevens die ze nodig hebben. |
| [Activa voor gegevens van aantekeningen voorzien](#annotate-data-assets) | In deze procedure kunt opgeven u aantekeningen (informatie zoals beschrijvingen, tags, documentatie of deskundigen) voor de gegevens activa. Deze informatie vormt een aanvulling op de metagegevens uit de gegevensbron en de gegevensbron begrijpelijker te maken voor meer mensen. |
| [Verbinding maken met gegevens activa](#connect-to-data-assets) | In deze procedure kunt openen u gegevens activa in geïntegreerde clienthulpprogramma's (zoals Excel en hulpmiddelen voor SQL Server-gegevens) en een niet-geïntegreerde hulpprogramma (SQL Server Management Studio). |
| [Gegevens beheren](#manage-data-assets) | In deze procedure kunt instellen u beveiliging voor de gegevens activa. Gegevenscatalogus geeft gebruikers toegang tot de gegevens zelf. De eigenaar van de gegevensbron bepaalt de toegang tot gegevens. <br/><br/> Met Data-catalogus ontdekt u gegevensbronnen en de **metagegevens** betrekking hebben op de bronnen die zijn geregistreerd in de catalogus weergeven. Er zijn situaties, echter waar gegevensbronnen weergegeven alleen bepaalde gebruikers of groepen worden moeten. Voor deze scenario's kunt u gegevenscatalogus eigenaar van de activa van de geregistreerde gegevens in de catalogus en de zichtbaarheid van de activa die u bezit. |
| [Gegevens-elementen verwijderen](#remove-data-assets) | In deze procedure leert u hoe gegevens activa verwijderen uit de catalogus met gegevens. |  

## <a name="tutorial-prerequisites"></a>Voorwaarden voor zelfstudie

### <a name="azure-subscription"></a>Azure-abonnement
Catalogus met Azure gegevens instellen, moet u de eigenaar of mede-eigenaar van een Azure-abonnement.

Azure-abonnementen kunnen u toegang tot bronnen van cloud service zoals catalogus met Azure gegevens indelen. Ze ook help u hoe de weergave Resourcegebruik wordt gemeld bepalen, in rekening gebracht en betaald. Elk abonnement kan een verschillende facturerings- en instellingen hebben zodat u beschikt over verschillende abonnementen en verschillende plannen per afdeling, project en regionaal kantoor. Elke cloud service deel uitmaakt van een abonnement en moet u een abonnement op voordat u de catalogus gegevens Azure instelt. Voor meer informatie, Zie [accounts beheren, abonnementen, en administratieve rollen](../active-directory/active-directory-how-subscriptions-associated-directory.md).

Als u niet een abonnement hebt, kunt u een gratis proefperiode account in een paar minuten. [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) Zie voor meer informatie.

### <a name="azure-active-directory"></a>Azure Active Directory
Catalogus met Azure gegevens stelt moet u zijn aangemeld met een gebruikersaccount Azure Active Directory (AD Azure). U moet de eigenaar of mede-eigenaar van een Azure-abonnement.  

AD Azure biedt een eenvoudige manier voor uw bedrijf voor het beheren van identiteits- en toegangsbeheer, zowel in de cloud en op gebouwen. Kunt u een enkel werk of school-account aan te melden een webtoepassing die cloud of op locatie. Azure AD catalogus Azure gegevens gebruikt voor verificatie-in. Voor meer informatie, Zie [Wat is Azure Active Directory](../active-directory/active-directory-whatis.md).

### <a name="azure-active-directory-policy-configuration"></a>Configuratie van beleid voor Azure Active Directory

Een situatie kunnen optreden wanneer u zich aanmelden bij de portal catalogus Azure-gegevens, maar wanneer u probeert aan te melden het hulpprogramma voor de registratie van gegevens, u ontvangt een foutmelding dat verhindert dat u zich aanmeldt. Deze fout kan optreden wanneer u op het netwerk of als u een verbinding van buiten het bedrijfsnetwerk.

Het hulpprogramma voor inschrijving gebruikt *verificatie van formulieren* valideren gebruiker aanmeldingen met Azure Active Directory. Azure Active Directory-beheerder moet forms-verificatie in het *beleid van globale verificatie*inschakelen voor succesvolle aanmelden.

Het globale verificatie-beleid kunt u de verificatie voor intranet- en extranet-verbindingen afzonderlijk zoals in de volgende afbeelding. Fouten kunnen optreden als verificatie van formulieren is niet ingeschakeld voor het netwerk van waaruit u verbinding maakt.

 ![Azure Active Directory globale verificatie-beleid](./media/data-catalog-prerequisites/global-auth-policy.png)

Zie [beleid voor verificatie configureren](https://technet.microsoft.com/library/dn486781.aspx)voor meer informatie.

## <a name="provision-data-catalog"></a>De catalogus gegevens verstrekken
U kunt slechts één gegevenscatalogus per organisatie (Azure Active Directory-domein) inrichten. Dus als de eigenaar of mede-eigenaar van een Azure-abonnement die deel uitmaakt van deze Azure Active Directory-domein, heeft een catalogus hebt gemaakt, is u niet mogelijk opnieuw een catalogus maken zelfs als er meerdere Azure abonnementen. Om te testen of een catalogus van de gegevens is gemaakt door een gebruiker in uw Azure Active Directory-domein, gaat u naar de [introductiepagina catalogus Azure-gegevens](http://azuredatacatalog.com) en controleer of u de catalogus bekijken. Als u al een catalogus is gemaakt, gaat u verder met de volgende procedure en Ga naar de volgende sectie.    

1. Ga naar de [catalogus gegevens servicepagina](https://azure.microsoft.com/services/data-catalog) en klik op **aan de slag**.

    ![Azure gegevenscatalogus--marketing landingspagina](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Meld u aan met een gebruikersaccount die is de eigenaar of mede-eigenaar van een Azure-abonnement. U ziet de volgende pagina na ondertekening.

    ![Azure gegevenscatalogus--verstrekken gegevenscatalogus](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Geef een **naam** voor de catalogus van de gegevens, het **abonnement** dat u wilt gebruiken en de **locatie** van de catalogus.
4. Vouw de **prijzen** en selecteert u een catalogus met Azure gegevens **edition** (gratis of standaard).
    ![Azure gegevenscatalogus--select edition](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Vouw **Gebruikers catalogus** en klik op **toevoegen** om gebruikers van de catalogus met gegevens. U wordt automatisch toegevoegd aan deze groep.
    ![Catalogus met Azure gegevens--gebruikers](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Vouw **Catalogus Administrators** en klik op **toevoegen** om extra beheerders voor de gegevenscatalogus. U wordt automatisch toegevoegd aan deze groep.
    ![Catalogus met Azure gegevens--beheerders](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Klik op **Catalogus maken** als u wilt maken van de catalogus van de gegevens voor uw organisatie. U ziet de introductiepagina voor de gegevenscatalogus nadat deze is gemaakt.
    ![Catalogus met Azure gegevens--gemaakt](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>Een gegevenscatalogus zoeken in de portal voor Azure
1. Ga naar de [Azure portal](https://portal.azure.com) op een apart tabblad in de webbrowser of in een afzonderlijk webbrowservenster en zich aanmelden met dezelfde account die u gebruikt voor het maken van de catalogus van de gegevens in de vorige stap.
2. Selecteer **Zoeken** en klik vervolgens op **Catalogus gegevens**.

    ![Azure gegevenscatalogus--Azure Bladeren](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) ziet u de gegevenscatalogus die u hebt gemaakt.

    ![Azure gegevenscatalogus--catalogus weergeven in de lijst](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
4.  Klik op de catalogus die u hebt gemaakt. U ziet de **Catalogus gegevens** blade in de portal.

    ![Azure gegevenscatalogus--blade in portal ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
5. U kunt eigenschappen van de gegevenscatalogus weergeven en bijwerken. Bijvoorbeeld, klik op **prijzen laag** en de editie.

    ![Azure gegevenscatalogus--prijzen laag](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Adventure Works-voorbeelddatabase
In deze zelfstudie, het registreren van gegevens activa (tabellen) uit de voorbeelddatabase AdventureWorks2014 van de SQL Server-Database-Engine, maar kunt u elke ondersteunde gegevensbron als u liever wilt werken met gegevens die bekend zijn en relevant zijn voor uw rol. Zie [ondersteunde gegevensbronnen](data-catalog-dsr.md)voor een lijst met ondersteunde gegevensbronnen.

### <a name="install-the-adventure-works-2014-oltp-database"></a>De Adventure Works 2014 OLTP-database installeren
Adventure Works database ondersteunt standaard online transaction processing-scenario's voor een fictieve fietsenfabrikant (Adventure Works Cycle), inclusief producten, verkoop- en inkoopdocumenten. In deze zelfstudie registreert u informatie over de producten in de catalogus gegevens Azure.

De voorbeelddatabase Adventure Works installeren:

1. Download [Adventure Works 2014 volledige Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) op CodePlex.
2. Als u de database op uw computer herstellen, volg de instructies in [een Database terugzetten met behulp van SQL Server Management Studio](http://msdn.microsoft.com/library/ms177429.aspx), of door de volgende stappen:
    1. Open SQL Server Management Studio en verbinding met de SQL Server-Database-Engine.
    2. Klik met de rechtermuisknop op **Databases** en klik op **Database terugzetten**.
    3. De optie **apparaat** voor **bron** en klik op **Bladeren**onder **Database terugzetten**.
    4. **Selecteer back-apparaten**en klik op **toevoegen**.
    5. Ga naar de map waar u het bestand **AdventureWorks2014.bak** hebt, selecteert u het bestand en klik op **OK** om de **Back-upbestand Zoek** -dialoogvenster te sluiten.
    6. Klik op **OK** om de **back-apparaten selecteren** dialoogvenster te sluiten.    
    7. Klik op **OK** om het dialoogvenster **Database terugzetten** te sluiten.

Activa van de gegevens uit de voorbeelddatabase Adventure Works kunt u nu registreren met behulp van de catalogus gegevens Azure.

## <a name="register-data-assets"></a>Activa gegevens registreren

In deze oefening gebruikt u het hulpprogramma voor inschrijving activa van Adventure Works database gegevens registreren met de catalogus. Registratie is het proces van belangrijke structurele metagegevens zoals namen, typen en locaties van de gegevensbron en de activa bevat uitpakken en kopiëren van metagegevens in de catalogus. De gegevensbron en de gegevens activa blijven waar ze zijn, maar de metagegevens wordt gebruikt door de catalogus zodat u ze gemakkelijker kan worden gedetecteerd en te begrijpen.

### <a name="register-a-data-source"></a>Een gegevensbron te registreren

1.  Ga naar de [introductiepagina Azure gegevenscatalogus](https://azuredatacatalog.com) en klik op **Gegevens publiceren**.

    ![Azure gegevens catalogus--gegevens publiceren knop](media/data-catalog-get-started/data-catalog-publish-data.png)

2.  Klik op **Toepassing starten** om te downloaden, installeren en uitvoeren van het hulpprogramma voor inschrijving op uw computer.

    ![Azure gegevenscatalogus--knop starten](media/data-catalog-get-started/data-catalog-launch-application.png)

3. Klik op **aanmelden** en voer uw referenties op de pagina **Welkom** .    

    ![Azure gegevenscatalogus--welkomstpagina](media/data-catalog-get-started/data-catalog-welcome-dialog.png)

4. Klik op **SQL Server** en **volgende**op de pagina **Microsoft Azure gegevenscatalogus** .

    ![Azure gegevenscatalogus--gegevensbronnen](media/data-catalog-get-started/data-catalog-data-sources.png)

5.  Voer de SQL Server connection-eigenschappen voor **AdventureWorks2014** (Zie het volgende voorbeeld) en klik op **VERBINDEN**.

    ![Azure gegevenscatalogus--verbindingsinstellingen voor SQL Server](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

6.  De metagegevens van de activa van uw gegevens registreren. In dit voorbeeld kunt u objecten uit de naamruimte AdventureWorks productie **Productie/Product** registreren:

    1. De **Server** hiërarchiestructuur Vouw **AdventureWorks2014** uit en klik op **productie**.
    2. Selecteer **Product**, **ProductCategory**, **ProductDescription**en **ProductPhoto** met behulp van Ctrl + klikken.
    3. Klik op de **geselecteerde pijl verplaatsen** (**>**). Deze actie verplaatst u alle geselecteerde objecten in de lijst met **objecten worden geregistreerd** .

        ![Azure catalogus gegevens zelfstudie--bladeren en objecten selecteren](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
    4. Selecteer **inclusief een voorbeeld** om een voorbeeld van de momentopname van de gegevens. De momentopname bevat maximaal 20 records uit elke tabel en wordt deze gekopieerd naar de catalogus.
    5. Selecteer **Gegevens-profiel opnemen** om op te nemen van een momentopname van de statistieken van het object voor het profiel van gegevens (bijvoorbeeld: minimale, maximale en gemiddelde waarden voor een kolom, het aantal rijen).
    6. Voer **adventure works, cycli**in het veld **tags toevoegen** . Deze actie wordt de zoek-tags voor de activa van deze gegevens toegevoegd. Tags zijn een uitstekende manier om te zoeken naar een gegevensbestand geregistreerde gebruikers.
    7. Geef de naam van een **deskundige** op deze gegevens (optioneel).

        ![Azure catalogus gegevens zelfstudie: objecten worden geregistreerd](media/data-catalog-get-started/data-catalog-objects-register.png)

    8. Klik op **REGISTREREN**. Catalogus Azure gegevens registreert de geselecteerde objecten. In deze oefening worden de geselecteerde objecten van Adventure Works zijn geregistreerd. Het hulpprogramma voor inschrijving metagegevens wordt geëxtraheerd uit de gegevens activa en die gegevens in de catalogus van de gegevens Azure service opgehaald. De gegevens blijft waar het zich momenteel bevindt, en blijft onder de controle van de groep administrators en het beleid van het huidige systeem.

        ![Azure gegevenscatalogus--geregistreerde objecten](media/data-catalog-get-started/data-catalog-registered-objects.png)

    9. Overzicht van de bronobjecten geregistreerde gegevens, klikt u op **De Portal weergeven**. Bevestigen dat u alle vier tabellen en de database in de rasterweergave Zie in de portal catalogus Azure-gegevens.

        ![Objecten in de catalogus van Azure gegevens portal ](media/data-catalog-get-started/data-catalog-view-portal.png)


In deze oefening u objecten uit de voorbeelddatabase Adventure Works geregistreerd zodat deze worden gemakkelijk door gebruikers in uw organisatie gevonden kunnen. In de volgende oefening leert u hoe om te ontdekken van de activa van de geregistreerde gegevens.

## <a name="discover-data-assets"></a>Ontdek de gegevens activa
Discovery in Azure gegevenscatalogus maakt gebruik van twee primaire mechanismen: zoeken en filteren.

Zoeken is ontworpen intuïtief en krachtig. Standaard worden zoektermen vergeleken met elke eigenschap in de catalogus, met inbegrip van de gebruiker opgegeven aantekeningen.

Filteren is ontworpen als aanvulling op zoeken. Kunt u specifieke kenmerken zoals deskundigen, gegevensbrontype objecttype en labels weergeven van gegevens met congruente activa en congruente activa zoekresultaten beperken.

Met behulp van een combinatie van zoeken en filteren, kunt u de gegevensbronnen die zijn geregistreerd in Azure gegevenscatalogus om te ontdekken de gegevens activa die u nodig hebt snel te navigeren.

In deze oefening kunt u de catalogus van Azure gegevens portal ontdekken gegevens activa die u in de vorige oefening hebt geregistreerd. Zie [Zoeken in de catalogus syntaxis reference](https://msdn.microsoft.com/library/azure/mt267594.aspx) voor meer informatie over de syntaxis van de zoekopdracht.

Hier volgen enkele voorbeelden voor het ontdekken van de activa van de gegevens in de catalogus.  

### <a name="discover-data-assets-with-basic-search"></a>Ontdek de gegevens activa met een standaardzoekopdracht
Standaardzoekopdracht kunt u zoeken in een catalogus met behulp van een of meer zoektermen. De resultaten zijn elementen die overeenkomen met alle eigenschappen met een of meer van de opgegeven voorwaarden.

1. Klik op **Start** in de catalogus van Azure gegevens portal. Als u de webbrowser hebt gesloten, gaat u naar de [introductiepagina catalogus Azure-gegevens](https://www.azuredatacatalog.com).
2. Voer in het zoekvak `cycles` en druk op **ENTER**.

    ![Azure gegevenscatalogus--basistekst zoeken](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Bevestigen dat u alle vier tabellen en de database (AdventureWorks2014) in de resultaten te zien. U kunt schakelen tussen **rasterweergave** en de **Lijstweergave** door te klikken op knoppen op de werkbalk, zoals in de volgende afbeelding. U ziet dat het zoekwoord in de zoekresultaten is geselecteerd, omdat de optie **markeren** **ingeschakeld is**. U kunt ook het aantal **resultaten per pagina** opgeven in de zoekresultaten.

    ![Azure gegevenscatalogus--basistekst zoekresultaten](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)

    Het deelvenster **Zoeken** aan de linkerkant en het deelvenster **Eigenschappen** aan de rechterkant. U kunt zoekcriteria wijzigen en filteren van de resultaten in het deelvenster **Zoeken** . Het deelvenster **Eigenschappen** worden de eigenschappen van een geselecteerd object in het raster of lijst.

4. Klik op **Product** in de zoekresultaten. Klik op het **voorbeeld**, **kolommen**, **Gegevens**en **documentatie** tabbladen of klik op de pijl om het onderste deelvenster weer te geven.  

    ![Azure gegevenscatalogus--onderste deelvenster](media/data-catalog-get-started/data-catalog-data-asset-preview.png)

    In het tabblad **voorbeeld** ziet u een voorbeeld van de gegevens in de tabel **producten** .  
5. Klik op het tabblad **kolommen** voor meer informatie over kolommen (zoals de **naam** en het **gegevenstype**) de gegevens actief in.
6. Klik op het tabblad **Profiel gegevens** voor de beoordeling van de gegevens (bijvoorbeeld: aantal rijen, de grootte van de gegevens of de minimumwaarde in een kolom) in de activa van de gegevens.
7. De resultaten filteren met behulp van de **Filters** aan de linkerkant. Klik bijvoorbeeld op **tabel** voor het **Objecttype**en ziet u alleen de vier tabellen, niet van de database.

    ![Azure gegevenscatalogus--filter zoekresultaten](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Gegevens activa met de eigenschap scoping ontdekken
Eigenschap scoping, kunt u de gegevens activa waarbij de zoekterm met de opgegeven eigenschap overeenkomt ontdekken.

1. De **tabel** filter onder **Objecttype** in **Filters**wissen.  
2. Voer in het zoekvak `tags:cycles` en druk op **ENTER**. Zie [Naslaginformatie voor zoeken in de catalogus-syntaxis](https://msdn.microsoft.com/library/azure/mt267594.aspx) voor alle eigenschappen die u gebruiken kunt voor het zoeken in de gegevenscatalogus.
3. Bevestigen dat u alle vier tabellen en de database (AdventureWorks2014) in de resultaten te zien.  

    ![Gegevenscatalogus--eigenschap scoping zoekresultaten](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>De zoekopdracht opslaan
1. Klik in het deelvenster **Zoeken** in de sectie **Huidige zoeken** een naam voor de zoekactie en klik op **Opslaan**.

    ![Azure gegevenscatalogus--opslaan zoeken](media/data-catalog-get-started/data-catalog-save-search.png)
2. Zorg ervoor dat de opgeslagen zoekopdracht wordt weergegeven onder **Opgeslagen zoekopdrachten**.

    ![Azure gegevenscatalogus--opgeslagen zoekopdrachten](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Selecteer een van de acties die u op de opgeslagen zoekopdracht (**hernoemen**, **verwijderen**, **Opslaan als standaard** zoeken uitvoeren kunt).

    ![Azure gegevenscatalogus--opgeslagen zoekopties](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Boole-operators
U kunt uitbreiden of beperken van uw zoekopdracht met Boole-operators.

1. Voer in het zoekvak `tags:cycles AND objectType:table`, en druk op **ENTER**.
2. Bevestig dat u alleen tabellen (niet de database) in de resultaten zien.  

    ![Azure gegevenscatalogus--Booleaanse operator in zoekopdracht](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Groeperen met haakjes
Groeperen met haakjes, kunt u onderdelen van de query voor een logische isolatie, vooral met Boole-operators groeperen.

1. Voer in het zoekvak `name:product AND (tags:cycles AND objectType:table)` en druk op **ENTER**.
2. Bevestig dat u **de producttabel in de zoekresultaten** zien.

    ![Azure gegevenscatalogus--groepering zoeken](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Vergelijkingsoperatoren
Met vergelijkingsoperatoren, kunt u vergelijkingen dan gelijkheid voor eigenschappen met datum- en numerieke gegevenstypen.

1. Voer in het zoekvak `lastRegisteredTime:>"06/09/2016"`.
2. Wis het filter **tabel** bij **Objecttype**.
3. Druk op **ENTER**.
4. Bevestig dat u de tabellen **producten**, **ProductCategory**, **ProductDescription**en **ProductPhoto** en de AdventureWorks2014-database die u hebt geregistreerd in de zoekresultaten zien.

    ![Azure gegevenscatalogus--vergelijking zoekresultaten](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Zie [gegevens activa ontdekken](data-catalog-how-to-discover.md) voor gedetailleerde informatie over de gegevens activa en [Zoeken in de catalogus syntaxis verwijst](https://msdn.microsoft.com/library/azure/mt267594.aspx) zoeken syntaxis ontdekken.

## <a name="annotate-data-assets"></a>Activa voor gegevens van aantekeningen voorzien
In deze oefening u de portal catalogus Azure-gegevens van aantekeningen voorzien (informatie, zoals beschrijvingen, tags of deskundigen toevoegen) gegevens activa die u eerder hebt geregistreerd in de catalogus. De aantekeningen aanvulling en verbetering van de structurele metagegevens opgehaald uit de gegevensbron tijdens de registratie en kunnen u de activa gegevens veel gemakkelijker te ontdekken en te begrijpen.

In deze oefening gaat u aantekeningen toevoegen aan een activum enkele gegevens (ProductPhoto). U toevoegen een beschrijvende naam en beschrijving aan de ProductPhoto gegevens actief.  

1.  Ga naar de [introductiepagina Azure gegevenscatalogus](https://www.azuredatacatalog.com) en zoek met `tags:cycles` om te zoeken naar de gegevens activa die u hebt geregistreerd.  
2. Klik op **ProductPhoto** in de zoekresultaten.  
3. **Productafbeeldingen** voor **Beschrijvende naam** en **Product foto's voor marketingmateriaal** voor de **Omschrijving**invoeren.

    ![Azure gegevenscatalogus--ProductPhoto omschrijving](media/data-catalog-get-started/data-catalog-productphoto-description.png)

    De **Beschrijving** helpt anderen ontdekken en begrijpen waarom en hoe u de geselecteerde gegevens activa. U kunt ook meer labels toevoegen en weergeven van kolommen. Nu kunt u proberen te zoeken en filteren om gegevens activa achterhalen via de beschrijvende metagegevens die u hebt toegevoegd aan de catalogus.

U kunt ook op deze pagina het volgende doen:

- Toevoegen van deskundigen voor de gegevens activa. Klik op **toevoegen** in het gebied van **deskundigen** .
- Tags toevoegen op het niveau van de dataset. Klik op **toevoegen** in het gebied van **labels** . Een tag is een gebruiker-tag of verklarende woordenlijst. De Standard Edition van catalogus gegevens bevat een woordenlijst business die kan beheerders een centrale zakelijke taxonomie definieert een catalogus. Catalogus gebruikers kunnen vervolgens gegevens activa met termen in de aantekeningen. Zie voor meer informatie, [het instellen van de Business-verklarende woordenlijst voor Tagging geregeld](data-catalog-how-to-business-glossary.md)
- Tags toevoegen op kolomniveau. Klik op **toevoegen** onder **Tags** voor de kolom die u wilt toevoegen.
- Beschrijving toevoegen op kolomniveau. **Omschrijving** invoeren voor een kolom. U kunt ook de beschrijving van metagegevens opgehaald uit de gegevensbron weergeven.
- **Aanvraag toegang krijgen tot** informatie die gebruikers zien hoe toegang tot de gegevens activa vragen toevoegen.

    ![Azure gegevenscatalogus--tags, beschrijvingen toevoegen](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)

- Kies het tabblad **documentatie** en documentatie voor het activum gegevens te verschaffen. Met Azure gegevenscatalogus documentatie, kunt u uw gegevenscatalogus als een opslagplaats voor inhoud voor het maken van een volledige verhaal van de activa van uw gegevens.

    ![Azure gegevenscatalogus--tabblad documentatie](media/data-catalog-get-started/data-catalog-documentation.png)


U kunt ook een aantekening toevoegen aan meerdere gegevens activa. Zo kunt u selecteren alle gegevens activa die u hebt geregistreerd en een deskundige voor hen opgeven.

![Azure gegevenscatalogus--meerdere activa voor gegevens van aantekeningen voorzien](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Catalogus Azure gegevens ondersteunt een Schare sourcing aanpak van aantekeningen. Elke gebruiker catalogus gegevens kunt toevoegen tags (gebruiker of woordenlijst), beschrijvingen en andere metagegevens, zodat elke gebruiker met een perspectief op een actief gegevens en het gebruik dat perspectief vastgelegd en beschikbaar voor andere gebruikers.

Zie [gegevens activa aantekeningen](data-catalog-how-to-annotate.md) voor gedetailleerde informatie over de gegevens activa aantekeningen maken.

## <a name="connect-to-data-assets"></a>Verbinding maken met gegevens activa
Open in deze oefening gegevens activa in een geïntegreerde client tool (Excel) en een niet-geïntegreerde hulpprogramma (SQL Server Management Studio) met behulp van de verbindingsgegevens.

> [AZURE.NOTE] Het is belangrijk te onthouden catalogus Azure-gegevens niet hebt u toegang tot de eigenlijke gegevensbron — het gewoon makkelijker om te ontdekken en begrijpt. Wanneer u verbinding met een gegevensbron maakt, wordt de clienttoepassing die u gebruikt uw Windows-referenties of u wordt gevraagd om referenties als u nodig hebt. Als u niet eerder toegang is verleend aan de gegevensbron, moet u toegang worden verleend voordat u verbinding kunt maken.

### <a name="connect-to-a-data-asset-from-excel"></a>Verbinding maken met een asset gegevens vanuit Excel

1. Selecteer **Product** uit de zoekresultaten. Klik op **Openen In** op de werkbalk en klikt u op **Excel**.

    ![Azure gegevenscatalogus--verbinding maken met gegevens activa](media/data-catalog-get-started/data-catalog-connect1.png)
2. Klik op **openen** in het pop-upvenster downloaden. Deze ervaring kan variëren afhankelijk van de browser.

    ![Azure gegevenscatalogus--Excel bestand gedownload](media/data-catalog-get-started/data-catalog-download-open.png)
3. Klik op **inschakelen**in het venster **Beveiligingskennisgeving van Microsoft Excel** .

    ![Azure gegevenscatalogus--Excel pop-ups over beveiliging](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Houd de standaardinstellingen in het dialoogvenster **Gegevens importeren** en klik op **OK**.

    ![Azure gegevenscatalogus--gegevens van Excel importeren](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. De gegevensbron in Excel bekijken.

    ![Azure gegevenscatalogus--producttabel in Excel](media/data-catalog-get-started/data-catalog-connect2.png)

In deze oefening gaat verbonden u met gegevens activa via Azure gegevenscatalogus ontdekt. Met de portal catalogus Azure-gegevens kunt u via de client-toepassingen geïntegreerd in het menu **openen in** . U kunt ook verbinding maken met elke toepassing die u met behulp van de verbindingsgegevens van de locatie in de metagegevens van de activa worden opgenomen. U kunt bijvoorbeeld SQL Server Management Studio verbinding maken met de database AdventureWorks2014 toegang tot de gegevens in de activa van de gegevens in deze zelfstudie wordt geregistreerd.

1. Open **SQL Server Management Studio**.
2. Geef de servernaam uit het deelvenster **Eigenschappen** in de portal Azure catalogus met gegevens in het dialoogvenster **verbinding maken met Server** .
3. De juiste verificatie gebruiken en referenties voor toegang tot de gegevens activa. Als u geen toegang hebt, gebruiken om deze informatie in het veld **Toegang aanvragen** .

    ![Azure gegevenscatalogus--toegang vragen](media/data-catalog-get-started/data-catalog-request-access.png)

Klik op **Weergave verbindingsreeksen** bekijken en ADF.NET, ODBC- én OLEDB verbindingsreeksen kopiëren naar het Klembord voor gebruik in uw toepassing.

## <a name="manage-data-assets"></a>Gegevens beheren
In deze stap ziet u het instellen van beveiliging voor de gegevens activa. Gegevenscatalogus geeft gebruikers toegang tot de gegevens zelf. De eigenaar van de gegevensbron bepaalt de toegang tot gegevens.

U kunt catalogus gegevens kunnen vinden, gegevensbronnen en de metagegevens die verband houden met de bronnen die zijn geregistreerd in de catalogus weergeven. Er zijn situaties, maar welke gegevensbronnen alleen zichtbaar aan specifieke gebruikers of aan groepen zijn moeten. Voor deze scenario's kunt u gegevenscatalogus eigenaar van de activa van de geregistreerde gegevens in de catalogus en vervolgens bepalen de zichtbaarheid van de activa van jou.

> [AZURE.NOTE] De mogelijkheden die worden beschreven in deze oefening zijn beschikbaar in de Standard Edition van Azure catalogus gegevens, niet in de gratis versie.
In catalogus Azure-gegevens, kunt u eigenaar van de activa van de gegevens, collega eigenaars toevoegen aan elementen van de gegevens en de zichtbaarheid van de activa van de gegevens.

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Eigenaar van de activa van de gegevens en de zichtbaarheid wilt beperken

1. Ga naar de [homepage catalogus Azure-gegevens](https://www.azuredatacatalog.com). Voer in het vak **zoekopdracht** `tags:cycles` en druk op **ENTER**.
2. Klik op een item in de lijst en klikt u op de werkbalk op **Eigenaar** .
3. In de sectie **beheer** van het paneel **Eigenschappen** , klikt u op **Eigenaar**.

    ![Azure gegevenscatalogus--eigenaar](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. **Eigenaren en gebruikers van deze** kiezen in het gedeelte **zichtbaarheid** wilt beperken zichtbaarheid, en klikt u op **toevoegen**. Gebruiker e-mailadressen invoeren in het tekstvak en druk op **ENTER**.

    ![Azure gegevenscatalogus--toegang beperken](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Gegevens-elementen verwijderen

In deze oefening kunt u de catalogus van Azure gegevens portal verwijderen gegevens bekijken van de geregistreerde gegevens activa en activa gegevens verwijderen uit de catalogus.

In catalogus Azure-gegevens, kunt u een afzonderlijk actief of meerdere activa te verwijderen.

1. Ga naar de [homepage catalogus Azure-gegevens](https://www.azuredatacatalog.com).
2. Voer in het vak **zoekopdracht** `tags:cycles` en druk op **ENTER**.
3. Selecteer een item in de lijst en klik op **verwijderen** op de werkbalk zoals weergegeven in de volgende afbeelding:

    ![Azure gegevenscatalogus--raster-item verwijderen](media/data-catalog-get-started/data-catalog-delete-grid-item.png)

    Als u de lijstweergave, wordt het selectievakje links van het item, zoals in de volgende afbeelding:

    ![Azure gegevenscatalogus--lijstitem verwijderen](media/data-catalog-get-started/data-catalog-delete-list-item.png)

    U kunt ook meerdere data-elementen selecteren en deze verwijderen, weergegeven in de volgende afbeelding:

    ![Azure gegevenscatalogus--meerdere data-elementen verwijderen](media/data-catalog-get-started/data-catalog-delete-assets.png)


> [AZURE.NOTE] Het standaardgedrag van de catalogus is dat elke gebruiker registreren elke gegevensbron en kan elke gebruiker verwijderen gegevens activa die is geregistreerd. De beheermogelijkheden die is opgenomen in de Standard Edition van Azure catalogus gegevens bieden aanvullende opties voor de eigenaar van de activa, beperken die van activa detecteren kunnen, en het beperken van die activa kunt verwijderen.


## <a name="summary"></a>Samenvatting

In deze zelfstudie verkend u essentiële mogelijkheden van Azure Data Catalog, met inbegrip van registratie, aantekeningen maken, ontdekken en enterprise data elementen beheren. Nu u de zelfstudie hebt voltooid, is het tijd om aan de slag. U kunt vandaag nog beginnen door het registreren van de gegevensbronnen die u en uw team zijn afhankelijk van en uitnodigen collega's aan het gebruik van de catalogus.

## <a name="references"></a>Verwijzingen

- [Het registreren van gegevens activa](data-catalog-how-to-register.md)
- [Het ontdekken van de activa van de gegevens](data-catalog-how-to-discover.md)
- [Hoe gegevens activa van aantekeningen voorzien](data-catalog-how-to-annotate.md)
- [Het vastleggen van gegevens activa](data-catalog-how-to-documentation.md)
- [Verbinding maken met gegevens activa](data-catalog-how-to-connect.md)
- [Het beheren van gegevens activa](data-catalog-how-to-manage.md)
