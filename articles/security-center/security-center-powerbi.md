<properties
   pageTitle="Inzichten verkrijgen van gegevens met Power BI Azure Security Center | Microsoft Azure"
   description="De Azure Security Center Power BI-content pack kunt u gemakkelijk vinden beveiligingswaarschuwingen, aanbevelingen, resources aangevallen en trends, op basis van een dataset die is gemaakt voor het melden van."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>Inzichten verkrijgen van gegevens met Power BI Azure Security Center
Het [Dashboard voor Power BI](http://aka.ms/azure-security-center-power-bi) voor Azure Beveiligingscentrum kunt u visualiseren, analyseren en aanbevelingen en waarschuwingen vanaf elke locatie, met inbegrip van het mobiele apparaat te filteren. Gebruik het dashboard Power BI onthullen trends en patronen - waarschuwingen weergeven door de resource of de bron-IP-adres en niet geadresseerd beveiligingsrisico's door de resource of leeftijd vallen. 

U kunt ook verfijnen van Security Center aanbevelingen en waarschuwingen met andere gegevens op interessante manieren, bijvoorbeeld met behulp van gegevens uit [De controlelogboeken Azure](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) en [Azure SQL Database controleren](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/). Power BI Dashboards bieden en u kunt deze gegevens ook exporteren naar Excel voor eenvoudige rapportage over de beveiligingsstatus van uw resources wolk.

##<a name="using-azure-security-center-dashboard-to-access-power-bi"></a>Beveiligingscentrum Azure dashboard gebruiken voor toegang tot de Power BI
Ook kunt u het dashboard Azure Security Center te krijgen tot rapporten in Power BI. Volg de stappen voor het uitvoeren van deze taak: 

1. Klik in het dashboard **Azure Security Center** op **verkennen in Power BI** .

    ![Verbinding maken met Azure Security Center met Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new10.png) 

2. De blade **verkennen in Power BI** geopend aan de rechterkant, zoals in het volgende scherm:

    ![Verbinding maken met Azure Security Center met Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new2.png)

3. Als u het dashboard Power BI voor de eerste keer maakt, kunt u een van de volgende opties in de blade **verkennen in Power BI** : 

    - **Beveiliging inzichten dashboard**: Kies deze optie als u wilt maken van een dashboard waarin de beveiligingsstatus, threads en detectie. Deze optie is een meer algemeen voor DevOps rol die verantwoordelijk is voor het analyseren van hun beschermingsstatus en meldingen over abonnementen gevonden.
    - **Groepsbeleid management dashboard**: Kies deze optie als u wilt verkennen, beheer en de tenuitvoerlegging van beleid.  Deze optie is een meer algemeen voor de centrale IT die meer gericht zijn op de governance. Zichtbaarheid en inzichten op security policy hechting te krijgen in hun organisatie kunnen ze dit dashboard gebruiken.
    - Als u al een Power BI-dashboard, klikt u op **het huidige Power BI-dashboard**.

4. In dit voorbeeld klikt u op de optie **beveiliging inzichten dashboard** . Als dit de eerste keer is, maakt u een dashboard Power BI voor Beveiligingscentrum wordt gevraagd of u de content pack te installeren. Zoals in het volgende scherm, klikt u op de knop **ophalen** in het venster **inhoud packs voor Power BI** :

    ![Azure Security Center beveiliging inzichten dashboard](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)

5. Het venster **verbinding maken met inzichten Azure Security Center-beveiliging** weergegeven. Zorg ervoor dat **de verificatiemethode** wordt **oAuth2** zoals hieronder wordt weergegeven en klik op **aanmelden** .
    
    ![Verificatie](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)

6. U kan gevraagd opnieuw te identificeren met uw referenties Azure. Na verificatie van uw dashboard wordt gemaakt. Zodra het dashboard wordt gemaakt ziet u een rapport met een vergelijkbare structuur, zoals in het volgende scherm:

    ![Power BI-Dashboard](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)


> [AZURE.NOTE] Vernieuwen van het rapport zal plaatsvinden op een dagelijkse basis. In het geval er een fout op deze vernieuwing, [Vernieuwen problemen met de Azure Security Center Power BI](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/), voor meer informatie over het oplossen van problemen met lezen.

Hier ziet u het aantal waarschuwingen en aanbevelingen, alsmede het aantal VMs, Azure SQL-databases en netwerkbronnen wordt bewaakt door Beveiligingscentrum Azure.

Een koppeling naar Azure Security Center wordt u omgeleid naar de Azure portal. De grafieken maakt makkelijk informatie over aanbevelingen voor de beveiliging en signalen, met inbegrip van:

- Status van de beveiliging
- Aanbevelingen in behandeling
- VM-aanbevelingen
- Meldingen over de tijd
- Aangevallen Resources
- Aangevallen IP-adressen

Achter elke grafiek zijn er aanvullende inzichten. Selecteer een tegel voor meer informatie. Bijvoorbeeld ziet de **Status van de beveiliging** tegel u meer details over in afwachting van aanbevelingen door resources zoals in het volgende scherm:

![Aanbevelingen](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Als u op een willekeurige regel van deze grafiek klikt, gaat de andere grijs en focus u alleen op die u hebt geselecteerd. Als u wilt terugkeren naar het dashboard, klikt u op **Azure Security Center** onder de optie **Dashboards** in het linkerdeelvenster van deze pagina.

> [AZURE.NOTE] Als u uw rapporten aanpassen door extra velden toevoegen of wijzigen van bestaande beeldmateriaal wilt, kunt u het rapport bewerken. Lees [een rapport in de weergave bewerken in Power BI beheren](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) voor meer informatie.

De tegels **waarschuwingen na verloop van tijd, Resources aangevallen** en **Aanvaller IP -adressen** hebben de vergelijkbare uitvoer wanneer u op een van deze. Dit gebeurt omdat het rapport informatie met betrekking tot deze drie variabelen aggregaten en deze **bronnen onder aanval roept** zoals in het volgende scherm:

![Bronnen onder aanval](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

Nu u ook een kopie van dit rapport opslaan, afdrukken of publiceren op het web met behulp van de opties in het menu **bestand** .

![Bestandsmenu](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>Uw gegevens Azure Security Center met Power BI services verkennen

Verbinding maken met de [Power BI-inhoud Pack Services](https://msit.powerbi.com/groups/me/getdata/services) in Power BI en de volgende stappen uitvoeren:

1. In het venster **Inhoud Pack voor Power BI** ziet u twee opties zoals hieronder wordt weergegeven.

    ![Content pack voor Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

    >[AZURE.NOTE] Als het eerste deel van dit artikel al wordt uitgevoerd, ziet u slechts één optie beheer van beveiligingsbeleid Center Azure is.

2. **In dit voorbeeld klikt u op in het **Beheer van beveiligingsbeleid Center Azure** naast elkaar.**

3. In het venster **verbinding maken met het beheer van beveiligingsbeleid Center Azure** zorg **oAuth2** onder **Verificatiemethode** vervolgkeuzelijst zoals hieronder te selecteren en klik op **aanmelden** .

    ![Beleid voor Computerbeheer](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)

4. U wordt omgeleid naar een verificatiepagina u de referenties die u gebruikt typt moet om verbinding met Azure Security Center. Nadat de verificatie voltooid is, start Power BI importeren van gegevens voor het opbouwen van uw rapporten. Gedurende deze tijd ziet u soms het volgende bericht op de rechterhoek van de browser:

    ![Verbinding maken met Azure Security Center met Power BI](./media/security-center-powerbi/security-center-powerbi-fig4.png)

    >[AZURE.NOTE] Wanneer het dashboard wordt gemaakt voor de eerste keer kunt duurt langer dan gewoonlijk, hoofdzakelijk voor scenario's waarin u meerdere abonnementen hebt. 

5. Zodra het proces is voltooid, wordt uw dashboard Azure Security Center Power BI geladen met het **Beheer van** rapport lijkt op de hieronder weergegeven:

    ![Groepsbeleid Management dashboard](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Zie ook
In dit document, hebt u geleerd hoe Power BI in Azure Beveiligingscentrum gebruiken. Voor meer informatie over Azure Security Center, raadpleegt u het volgende:

- [Azure Security Center Planning en Operations Guide](security-center-planning-and-operations-guide.md) , informatie over het plan Azure Security Center vaststelling.
- [Het instellen van beveiligingsbeleid in Beveiligingscentrum Azure](security-center-policies.md) , informatie over het configureren van beveiligingsinstellingen in Azure Security Center
- [Beheren en reageren op de beveiligingswaarschuwingen in Beveiligingscentrum Azure](security-center-managing-and-responding-alerts.md) , informatie over het beheren van en reageren op de beveiligingswaarschuwingen
- [Azure Security Center FAQ](security-center-faq.md) , veelgestelde vragen over het gebruik van de service zoeken
- [Beveiligingsblog voor Azure](http://blogs.msdn.com/b/azuresecurity/) — vinden over Azure beveiliging en naleving van blogberichten
