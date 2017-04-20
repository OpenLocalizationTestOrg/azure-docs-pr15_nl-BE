<properties
    pageTitle="System Update beoordeling-oplossing in logboek Analytics | Microsoft Azure"
    description="Kunt u de oplossing systeemupdates in Analytics logboek kunt u ontbrekende updates toepassen op servers in uw infrastructuur."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="system-update-assessment-solution-in-log-analytics"></a>In logboek Analytics System Update beoordeling-oplossing

Kunt u de oplossing systeemupdates in Analytics logboek kunt u ontbrekende updates toepassen op servers in uw infrastructuur. Nadat u de oplossing, kunt u de updates die in de gecontroleerde servers ontbreken met behulp van de **System Update beoordeling** naast elkaar op de pagina **Overzicht** van OMS weergeven.

Als u ontbrekende updates worden gevonden, worden gegevens weergegeven in het dashboard **Updates** . Het dashboard **Updates** kunt u werken met ontbrekende updates en een plan opstellen om te deze toepassen op de servers die ze nodig hebben.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie te installeren en configureren van de oplossing.

- De System Update Assessment-oplossing toevoegen aan uw Kantoorbeheersysteem werkruimte met behulp van het proces beschreven in [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie nodig.

## <a name="system-update-data-collection-details"></a>Systeem Update data collectie details

Beoordeling van System Update verzamelt metagegevens en staat gegevens met behulp van de agenten die u hebt ingeschakeld.

De volgende tabel bevat methoden van de collectie gegevens en andere informatie over de manier waarop gegevens worden verzameld voor de beoordeling van System Update.

| platform | Directe Agent | SCOM agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | frequentie van verzameling |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)|![Nee](./media/log-analytics-system-update/oms-bullet-red.png)|            ![Nee](./media/log-analytics-system-update/oms-bullet-red.png)|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)| Ten minste 2 maal per dag en 15 minuten na het installeren van een update|

In de volgende tabel ziet u voorbeelden van gegevenstypen zijn verzameld door het System Update beoordeling:

|**Gegevenstype**|**Velden**|
|---|---|
|Metagegevens|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, netwerknaam, IP-adres, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-adres, NetbiosDomainName, LogicalProcessors, DNS-naam, naam, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Staat|StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|


### <a name="to-work-with-updates"></a>Werken met updates

1. Klik op de tegel **Systeem Update beoordeling** op de pagina **Overzicht** .  
    ![Systeem Update beoordeling naast elkaar](./media/log-analytics-system-update/sys-update-tile.png)
2. Op het dashboard **Updates** de update-categorieën te bekijken.  
    ![Dashboard voor updates](./media/log-analytics-system-update/sys-updates02.png)
3. Ga naar de rechterkant van de pagina te bekijken van de bladeserver **Kritiek/beveiligingsupdates voor Windows** en klik vervolgens onder **indeling**, op **Beveiligingsupdates**.  
    ![Beveiligingsupdates](./media/log-analytics-system-update/sys-updates03.png)
4. Op de pagina logboek zoeken een verscheidenheid aan informatie weergegeven over de beveiligingsupdates die zijn gevonden op servers ontbreken in uw infrastructuur. Klik op de **lijst** om gedetailleerde informatie over de updates weer te geven.  
    ![Zoekresultaten - lijst](./media/log-analytics-system-update/sys-updates04.png)
5. Gedetailleerde informatie over elke update op de pagina logboek zoeken wordt weergegeven. Klik naast het nummer KBID **weergeven** om het desbetreffende artikel op de website Microsoft Support weer te geven.  
    ![Zoekresultaten - KB weergave logboek](./media/log-analytics-system-update/sys-updates05.png)
6. Uw webbrowser wordt de webpagina Microsoft ondersteuning voor het bijwerken in een nieuw tabblad geopend. Bekijk de informatie over de update die ontbreekt.  
    ![Microsoft Support-webpagina](./media/log-analytics-system-update/sys-updates06.png)
7. Met behulp van de hand van de gegevens u hebt gevonden, kunt u een plan om de ontbrekende update handmatig toepassen of u kunt doorgaan na de resterende stappen om de update automatisch toegepast.
8. Als u wilt dat automatisch de ontbrekende update toe te passen, gaat u terug naar het dashboard **Updates** en klikt u onder **Update wordt uitgevoerd**, **klikt u op als u wilt plannen, een update uitvoeren**.  
    ![Update wordt uitgevoerd - tabblad geplande](./media/log-analytics-system-update/sys-updates07.png)
9. Op de pagina **Bijwerken wordt uitgevoerd** op het tabblad **gepland** , klikt u op **toevoegen** om te maken van een nieuwe update uitvoeren.  
    ![Geplande tabblad - toevoegen](./media/log-analytics-system-update/sys-updates08.png)
10. Op de pagina **Nieuwe Update uitvoeren** , type een naam voor de update uitvoert, afzonderlijke computers of computergroepen toevoegen, een schema definiëren en klik op **Opslaan**.  
    ![Nieuwe Update uitvoeren](./media/log-analytics-system-update/sys-updates09.png)
11. Het tabblad **gepland** op de pagina ziet u **Update wordt uitgevoerd** de nieuwe update uitvoeren die u hebt gepland.  
    ![Tabblad geplande](./media/log-analytics-system-update/sys-updates10.png)
12. Bij het starten van de update uitvoert, ziet u informatie op het tabblad **uitvoeren** .  
    ![Actief tabblad](./media/log-analytics-system-update/sys-updates11.png)
13. Nadat de update uitgevoerd is voltooid, wordt het tabblad **voltooide** status.
14. Als updates zijn toegepast vanuit de uitvoeren in de blade **Kritiek/beveiligingsupdates voor Windows** update ziet u dat het aantal updates is verminderd.  
    ![Blade kritiek/beveiligingsupdates voor Windows - update telling verlaagd](./media/log-analytics-system-update/sys-updates12.png)



## <a name="next-steps"></a>Volgende stappen

- [Zoeken in Logboeken](log-analytics-log-searches.md) update van gedetailleerde systeemgegevens weergeven.
