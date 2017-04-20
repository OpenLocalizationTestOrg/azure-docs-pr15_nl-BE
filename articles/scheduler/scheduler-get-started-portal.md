<properties
 pageTitle="Aan de slag met Azure Scheduler in Azure portal | Microsoft Azure"
 description="Aan de slag met Azure Scheduler in Azure portal"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/10/2016"
 ms.author="deli"/>

# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Aan de slag met Azure Scheduler in Azure portal

Het is eenvoudig met geplande taken in Taakplanner Azure te maken. In deze zelfstudie leert u hoe u een taak wilt maken. U leert ook de Scheduler energiebewaking en -mogelijkheden.

## <a name="create-a-job"></a>Een taak maken

1.  Aanmelden bij [Azure portal](https://portal.azure.com/).  

2.  Klik op **+ Nieuw** > _Scheduler_ typt in het zoekvak > selecteert u **Taakplanner** in resultaten > klikt u op **maken**.

     ![][marketplace-create]

3.  We maken een taak die gewoon http://www.microsoft.com/ met een GET-aanvraag. Voer de volgende gegevens in het venster **Job Scheduler** :

    1.  **Naam:**`getmicrosoft`  

    2.  **Abonnement:** Uw abonnement op Azure   

    3.  **Collectie job:** Selecteer een bestaande taak verzameling, of klik op **Nieuw** > Voer een naam in.

4.  Definieer vervolgens de volgende waarden van het **Actie-instellingen**:

    1.  **Actietype:**` HTTP`  

    2.  **Methode:**`GET`  

    3.  **URL:**` http://www.microsoft.com`  

      ![][action-settings]

5.  Tot slot laten we een schema definiëren. De taak kan worden gedefinieerd als een eenmalige taak, maar we kiezen een terugkerend schema:

    1. **Terugkeerpatroon**:`Recurring`

    2. **Start**: de datum van vandaag

    3. **Herhalen elke**:`12 Hours`

    4. **Einddatum**: twee dagen na de huidige systeemdatum  

      ![][recurrence-schedule]

6.  Klik op **maken**

## <a name="manage-and-monitor-jobs"></a>Beheren en bewaken van projecten

Zodra een taak is gemaakt, wordt deze weergegeven in het belangrijkste dashboard Azure. Klik op de taak en een nieuw venster geopend met de volgende tabbladen:

1.  Eigenschappen  

2.  Actie-instellingen  

3.  Planning  

4.  Geschiedenis

5.  Gebruikers

    ![][job-overview]

### <a name="properties"></a>Eigenschappen

Deze alleen-lezen eigenschappen beschrijven de metagegevens van het management voor de job Scheduler.

   ![][job-properties]


### <a name="action-settings"></a>Actie-instellingen

Als u op een taak in het venster **taken** kunt u die taak configureren. U kunt geavanceerde instellingen configureren als u niet in de wizard Snelle maken.

Voor alle actietypen, kunt u het beleid opnieuw en de actie van de fout te wijzigen.

U kunt de methode wijzigen naar eventuele toegestane HTTP-woord voor HTTP en HTTPS taak actietypen. U kunt ook toevoegen, verwijderen of wijzigen van kopteksten en basisverificatie gegevens.

U kunt voor opslag actie wachtrijtypen, de opslag account wachtrijnaam, SAS-token en hoofdtekst wijzigen.

U kunt voor actietypen service bus, de naamruimte, onderwerp/wachtrijpad verificatie-instellingen, type vervoer, berichteigenschappen en hoofdtekst van het bericht wijzigen.

   ![][job-action-settings]

### <a name="schedule"></a>Planning

Hiermee kunt u de planning configureren als u wijzigen van het schema dat u hebt gemaakt wilt in de wizard Snelle maken.

Dit is een kans die [complexe schema's en geavanceerde terugkeerpatroon in uw project](scheduler-advanced-complexity.md) maken

U kunt de begindatum wijzigen en de tijd, een terugkerend schema en het einde datum en tijd (als de taak terugkerend is.)

   ![][job-schedule]


### <a name="history"></a>Geschiedenis

Het tabblad **Geschiedenis** worden geselecteerde parameters voor de taakuitvoering van elke in het systeem voor de geselecteerde taak weergegeven. Deze gegevens bieden real-time waarden met betrekking tot de gezondheid van de Scheduler:

1.  Status  

2.  Details  

3.  Herhalingspogingen

4.  Exemplaar: 1ste, 2de, 3de, enz.

5.  De begintijd van de uitvoering  

6.  De eindtijd van de uitvoering

   ![][job-history]

U kunt klikken op een run voor **Details van**, met inbegrip van het volledige antwoord voor elke uitvoering. In dit dialoogvenster kunt u het antwoord naar het Klembord kopiëren.

   ![][job-history-details]

### <a name="users"></a>Gebruikers

Azure Role-Based Access Control RBAC () kunt fijnmazig toegangsbeheer Azure scheduler. Voor meer informatie over het gebruik van het tabblad gebruikers verwijzen naar [Azure_Role-Based Access Control](../active-directory/role-based-access-control-configure.md)


## <a name="see-also"></a>Zie ook

 [Wat is Taakplanner?](scheduler-intro.md)

 [Taakplanner-begrippen en terminologie entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Plannen en facturering in Azure planner](scheduler-plans-billing.md)

 [Het bouwen van complexe schema's en geavanceerde herhaling met Azure Scheduler](scheduler-advanced-complexity.md)

 [Scheduler REST API: naslag](https://msdn.microsoft.com/library/mt629143)

 [Scheduler PowerShell cmdlets verwijzing](scheduler-powershell-reference.md)

 [Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)

 [Scheduler limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Uitgaande verificatie Scheduler](scheduler-outbound-authentication.md)


[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
