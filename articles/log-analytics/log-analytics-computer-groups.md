<properties
    pageTitle="Computergroepen in Analytics logboek logboek zoekopdrachten | Microsoft Azure"
    description="Computergroepen in Analytics logboek kunnen u de scope logboek zoeken naar een specifieke verzameling computers.  Dit artikel worden de verschillende methoden die u gebruiken kunt voor het maken van computergroepen en hoe ze te gebruiken in een logboek zoeken."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="bwren"/>

# <a name="computer-groups-in-log-analytics-log-searches"></a>Computergroepen in Analytics logboek logboek zoekopdrachten
Computergroepen in logboek Analytics kunnen u een bereik aan te [melden zoekopdrachten](log-analytics-log-searches.md) een bepaalde set computers.  Elke groep wordt gevuld met computers met behulp van een query die u definieert of door groepen uit verschillende bronnen importeren.  Wanneer de groep is opgenomen in een zoekactie van het logboek, zijn de resultaten beperkt tot records die overeenkomen met de computers in de groep.

## <a name="creating-a-computer-group"></a>Een computergroep maken
In logboek Analytics met behulp van de methoden in de volgende tabel kunt u een computergroep.  In de onderstaande secties vindt u meer informatie over elke methode. 

| Methode | Beschrijving |
|:---|:---|
| Logboek zoeken       | Maak een logboek zoeken geeft als resultaat een lijst met computers en de resultaten opslaan als een computergroep. |
| Logboek Search API   | Het logboek Zoek-API gebruiken via programmering een computergroep op basis van de resultaten van een zoekactie logboek maken. |
| Active Directory | Automatisch scannen het groepslidmaatschap van agentcomputers die lid zijn van een Active Directory-domein en een groep maken in het logboek voor Analytics voor elke beveiligingsgroep.
| WSUS              | Automatisch scannen WSUS-servers of -clients voor het afstemmen van de groepen en een groep maken in het logboek voor Analytics voor elk. |


### <a name="log-search"></a>Logboek zoeken

Computergroepen gemaakt op basis van een zoekopdracht voor logboek bevat alle computers die worden geretourneerd door een zoekopdracht die u definieert.  Deze query wordt uitgevoerd telkens wanneer de groep op de computer wordt gebruikt, zodat eventuele wijzigingen sinds de groep is gemaakt worden weergegeven.

Gebruik de volgende procedure een computergroep maken van een zoekfunctie.

1. [Maken van een zoekfunctie](log-analytics-log-searches.md) geeft als resultaat een lijst met computers.  De zoekactie moet een afzonderlijke set computers terug met behulp van iets als **Afzonderlijke Computer** of **count() maatregel door de Computer** in de query.  
2. Klik op de knop **Opslaan** boven in het scherm.
3. Selecteer **Ja** om te **deze query opslaan als een computergroep:**.
4. Typ een **naam** en een **categorie** voor de groep.  Als een zoekopdracht met dezelfde naam en dezelfde categorie al bestaat, wordt u gevraagd om te overschrijven.  U kunt meerdere zoekacties met dezelfde naam hebben in verschillende categorieën. 

Voorbeeld van de zoekopdrachten die u kunt opslaan als een computergroep volgen.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

### <a name="log-search-api"></a>Meld u Zoek-API

Gemaakt met de zoek-API logboek computergroepen zijn hetzelfde als de zoekacties die zijn gemaakt met een zoekfunctie.

Voor meer informatie over het maken van een computergroep met behulp van het logboek Zoek-API Zie [computergroepen in Analytics logboek logboek zoeken REST API](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory

Wanneer u een logboek Analytics groepslidmaatschappen van Active Directory importeren configureert, wordt het het groepslidmaatschap van domein gekoppeld met de agent OMS computers analyseren.  Een computergroep in Analytics logboek gemaakt voor elke beveiligingsgroep in Active Directory en elke computer wordt toegevoegd aan de computergroepen overeenkomt met de ze lid van zijn beveiligingsgroepen.  Dit lidmaatschap wordt voortdurend bijgewerkt elke 4 uur.  

U configureren Analytics logboek voor Active Directory-beveiligingsgroepen te importeren in het menu **Computergroepen** in logboek Analytics- **Instellingen**.  Selecteer **automatisering** en **groepslidmaatschappen van Active Directory importeren van computers**.  Er is geen verdere configuratie nodig.

![Computergroepen uit Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Wanneer de groepen zijn geïmporteerd, wordt het menu een lijst het aantal computers met groepslidmaatschap ontdekt en het aantal groepen dat is geïmporteerd.  U kunt klikken op een van deze koppelingen naar de records **ComputerGroup** met deze gegevens.

### <a name="windows-server-update-service"></a>Windows Server updateservice

Wanneer u een logboek Analytics voor het importeren van groepslidmaatschappen WSUS configureert, wordt het het doelitem groepslidmaatschap van computers met de agent OMS analyseren.  Als u client-side gericht, op elke computer die is verbonden met OMS en maakt deel uit van een WSUS gericht op groepen de groep lid zijn geïmporteerd in het logboek voor Analytics. Als u server-side moet gericht, de OMS agent worden geïnstalleerd op de WSUS-server om de gegevens over het lidmaatschap worden geïmporteerd op OMS.  Dit lidmaatschap wordt voortdurend bijgewerkt elke 4 uur. 

U configureren Analytics logboek voor Active Directory-beveiligingsgroepen te importeren in het menu **Computergroepen** in logboek Analytics- **Instellingen**.  Selecteer **Active Directory** en **Active Directory importeren groepslidmaatschappen van computers**.  Er is geen verdere configuratie nodig.

![Computergroepen uit Active Directory](media/log-analytics-computer-groups/configure-wsus.png)

Wanneer de groepen zijn geïmporteerd, wordt het menu een lijst het aantal computers met groepslidmaatschap ontdekt en het aantal groepen dat is geïmporteerd.  U kunt klikken op een van deze koppelingen naar de records **ComputerGroup** met deze gegevens.

## <a name="managing-computer-groups"></a>Computergroepen beheren

U kunt computergroepen die zijn gemaakt met een logboek of het logboek Zoek-API in het menu **Computergroepen** in logboek Analytics- **Instellingen**weergeven.  Klik op de **x** in de kolom **verwijderen** groep op de computer te verwijderen.  Klik op het pictogram **weergave leden** voor een groep voor het uitvoeren van de zoekfunctie van de groep die de leden als resultaat geeft. 

![Opgeslagen computergroepen](media/log-analytics-computer-groups/configure-saved.png)

Als u wilt de groep wijzigt, moet u een nieuwe groep maken met dezelfde **categorie** en **naam van** de oorspronkelijke groep overschrijven.

## <a name="using-a-computer-group-in-a-log-search"></a>Met behulp van een groep van de computer in een logboek zoeken
U gebruikt de volgende syntaxis om te verwijzen naar een computergroep in een logboek zoeken.  Geven dat de **categorie** is optioneel en is alleen vereist als er computergroepen met dezelfde naam in verschillende categorieën. 

    $ComputerGroups[Category: Name]

Wanneer u een zoekopdracht uitvoert, worden de leden van computergroepen die in de zoekopdracht opgenomen eerst opgelost.  Als de groep is gebaseerd op een zoekfunctie, is dat met de zoekactie uitgevoerd om terug te keren de leden van de groep voor het bovenste niveau logboek zoeken.

Computergroepen worden meestal gebruikt met **de component in het logboek zoeken zoals in het volgende voorbeeld** .

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

## <a name="computer-group-records"></a>Computer groepsrecords

Een record wordt gemaakt in de opslagplaats OMS voor lidmaatschap van elke computer gemaakt op basis van Active Directory of WSUS.  Deze records hebben een soort **ComputerGroup** en hebben de eigenschappen in de volgende tabel.  Records worden niet gemaakt voor de computergroepen op basis van zoekopdrachten logboek.

| Eigenschap | Beschrijving |
|:--|:--|
| Type                | *ComputerGroup* |
| SourceSystem        | *SourceSystem*  |
| Computer            | Naam van de lidcomputer is. |
| Groep               | De naam van de groep. |
| GroupFullName       | Het volledige pad naar de groep met inbegrip van de bron en de naam van de gegevensbron.
| GroupSource         | Bron van die groep is verzameld uit. <br><br>ActiveDirectory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName     | De naam van de bron die de groepen is verzameld uit.  Dit is de FQDN-naam voor Active Directory. |
| ManagementGroupName | De naam van de groep management voor SCOM agenten.  Voor andere functionarissen, is dit AOI -\<-ID voor werkruimte\> |
| TimeGenerated       | Datum en tijd-groep op de computer is gemaakt of bijgewerkt. |



## <a name="next-steps"></a>Volgende stappen

- Informatie over het [logboek zoeken](log-analytics-log-searches.md) voor het analyseren van de gegevens die worden verzameld uit gegevensbronnen en oplossingen.  