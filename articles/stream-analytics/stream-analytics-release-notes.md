<properties 
    pageTitle="Release-opmerkingen voor Analytics Stream | Microsoft Azure" 
    description="Stream Analytics Release-opmerkingen" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="stream-analytics-release-notes"></a>Stream Analytics release-opmerkingen

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Opmerkingen voor 15/04/2016 release van Stream Analytics ##

Deze versie bevat de volgende update.

Titel | Beschrijving
---|---
Algemene beschikbaarheid voor Power BI-uitgangen  | [Power BI wordt](stream-analytics-power-bi-dashboard.md) nu in het algemeen beschikbaar zijn. Het vervallen van de vergunning van 90 dagen voor Power BI is verwijderd. Zie de sectie [verlengen vergunning](stream-analytics-power-bi-dashboard.md#Renew-authorization) voor het maken van een dashboard Power BI voor meer informatie over scenario's waarin de vergunning moet worden vernieuwd.

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Notities voor de release van de Stream Analytics 03/03/2016 ##

Deze versie bevat de volgende update.

Titel | Beschrijving
---|---
Nieuwe Stream Analytics Query Language-objecten  | SAQL heeft nu [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "GetType MSDN-pagina"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "De MSDN-pagina TRY_CAST") en [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "REGEXMATCH MSDN-pagina").

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Notities voor de release van Stream Analytics 12/10/2015 ##

Deze versie bevat de volgende update.

Titel | Beschrijving
---|---
REST API versie-update | De REST API versie is bijgewerkt tot 10-01-2015. Details vindt u op MSDN op [Stroom Analytics beheer REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx) en [integratie van Machine Learning in Analytics Stream](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md).
Azure Machine Learning integratie | In deze versie bevat ondersteuning voor Azure Machine leren door gebruiker gedefinieerde functies. Zie de [handleiding](stream-analytics-machine-learning-integration-tutorial.md) voor meer informatie, alsmede de [algemene blog aankondiging](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx).

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Notities voor de release van de Stream Analytics 11/12/2015 ##

Deze versie bevat de volgende update.

Titel | Beschrijving
---|---
Nieuwe gedrag van selecteren | Selecteer in Analytics Stream is uitgebreid zodat * als de eigenschapsaccessor van een van een geneste record. Raadpleeg voor meer informatie [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "Complexe gegevenstypen").

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Notities voor de release van Stream Analytics 22-10/2015 ##

Deze versie bevat de volgende updates.

Titel | Beschrijving
---|---
Taalfuncties in extra query | Stream Analytics de querytaal is uitgebreid door de volgende functies: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx), [plafond](https://msdn.microsoft.com/library/azure/mt605286.aspx), [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [VLOER](https://msdn.microsoft.com/library/azure/mt605240.aspx), [voeding](https://msdn.microsoft.com/library/azure/mt605287.aspx), [teken](https://msdn.microsoft.com/library/azure/mt605290.aspx), [VIERKANT](https://msdn.microsoft.com/library/azure/mt605288.aspx)en [WORTEL](https://msdn.microsoft.com/library/azure/mt605238.aspx).
Statistische beperkingen verwijderd  | Deze release verwijdert de beperking van 15 geaggregeerd in een query. Nu is er geen limiet aan het aantal samengestelde waarden per query.
Extra functie van de groep door System.Timestamp | Nu kunt u de [GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) -functie voor window_type of [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx).
Extra verschuiving voor Tumbling en Hopping van windows | Standaard windows [Tumbling](https://msdn.microsoft.com/library/azure/dn835055.aspx) en [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) worden uitgelijnd ten opzichte van nul keer (1/1/0001-00:00:00 uur UTC). De nieuwe (optionele) parameter 'offsetsize' kunt een aangepaste verschuiving (of uitlijning).


## <a name="notes-for-09292015-release-of-stream-analytics"></a>Notities voor de release van de Stream Analytics 29-09/2015 ##

Deze versie bevat de volgende updates.

Titel | Beschrijving
---|---
Azure IoT Suite Public Preview | Stream Analytics is opgenomen in de Public Preview van Azure IoT Suite.
Azure Portal-integratie | Naast de blijvende aanwezigheid in de portal Management Azure is Stream Analytics nu geïntegreerd in de [Portal Azure](https://azure.microsoft.com/overview/preview-portal/). Opmerking Stream Analytics in het Preview portal-functionaliteit momenteel is een subset van de functionaliteit aangeboden in de portal zonder ondersteuning voor het testen van query's in de browser Azure Management Power BI uitvoer configuratie, en om te surfen of maken van nieuwe input en output resources in abonnementen u toegang tot hebt.
Ondersteuning voor de uitvoer van DocumentDB | Stream Analytics taken kunnen nu uitvoeren op [DocumentDB](https://azure.microsoft.com/services/documentdb/).
Ondersteuning voor invoer IoT Hub | Stream Analytics taken kunnen nu gegevens uit de IoT Hubs nemen.
TIJDSTEMPEL van heterogene gebeurtenissen | Wanneer een enkele gegevensstroom meerdere gebeurtenistypen met tijdstempels in verschillende velden bevat, nu kunt u [Door tijdstempel](http://msdn.microsoft.com/library/mt573293.aspx) met expressies verschillende tijdstempelvelden voor elk geval opgeven.

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Notities voor de release van de Stream Analytics 09/10/2015 ##

Deze versie bevat de volgende updates.

Titel|Beschrijving
---|---
Ondersteuning voor PowerBI-groepen|Zodat het delen van gegevens met andere gebruikers van de Power BI kunnen Analytics Stream taken nu schrijven naar [PowerBI groepen](stream-analytics-define-outputs.md#power-bi) binnen uw account Power BI.

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Notities voor de release van Stream Analytics 20/08/2015 ##

Deze versie bevat de volgende updates.

Titel|Beschrijving
---|---
LAATSTE functie toegevoegd |De [laatste](http://msdn.microsoft.com/library/mt421186.aspx) functie is nu beschikbaar in de Stream Analytics taken, zodat u kunt ophalen van de meest recente gebeurtenis in de stroom van een gebeurtenis binnen een bepaalde tijd.
Nieuwe functies van de matrix|Array functies [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) en [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) zijn nu beschikbaar.
Nieuwe functies van de Record|Record functies [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) en [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) zijn nu beschikbaar.

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Notities voor de release van Stream Analytics 30/07/2015 ##

Deze versie bevat de volgende updates.

Titel|Beschrijving
---|---
Power BI organisatie-Id ontkoppeld van Azure-Id|Met deze functie kunnen [output Power BI](stream-analytics-power-bi-dashboard.md) voor ASA taken onder elk type Azure rekening (Live-Id of de organisatie-Id). U kunt ook één organisatie-Id voor uw Azure-account hebt en een voor de toekenning van de Power BI-uitvoer gebruikt.
Ondersteuning voor Bus servicewachtrijen uitvoer|[Bus servicewachtrijen](stream-analytics-define-outputs.md#service-bus-queues) uitgangen zijn nu beschikbaar in de Stream Analytics taken.
Ondersteuning voor uitvoer Service Bus onderwerpen|[Service Bus onderwerpen](stream-analytics-define-outputs.md#service-bus-topics) uitgangen zijn nu beschikbaar in de Stream Analytics taken.

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Notities voor de release van de Stream Analytics 07/09/2015 ##

Deze versie bevat de volgende updates.


Titel|Beschrijving
---|---
Blob-aangepaste partitionering van de uitvoer|BLOB storage uitgangen kunnen nu een optie voor de frequentie die uitvoer BLOB's worden geschreven en de structuur en indeling van de output pad map gegevensstructuur. 

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Notities voor de release van de Stream Analytics 05/03/2015 ##

Deze versie bevat de volgende updates.


Titel|Beschrijving
---|---
Maximale waarde verhoogd voor Out van tolerantie venster|De maximale grootte van het venster van de tolerantie Out of Order is nu 59:59 (mm: SS)
JSON-uitvoerindeling: De regel gescheiden of matrix|Er is nu een optie bij het uitvoeren naar een Blob-opslag of gebeurtenis Hub moet worden uitgevoerd als een matrix van JSON-objecten of door de JSON-objecten van elkaar te scheiden met een nieuwe regel. 

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Notities voor de release van de Stream Analytics 16/04/2015 ##


Titel|Beschrijving
---|---
De wachttijd in Azure account opslagconfiguratie|Wanneer u een Stream Analytics taak in een regio voor de eerste keer, wordt u gevraagd om een nieuwe opslag-account maken of een bestaande account voor het controleren van taken in die regio Stream Analytics opgeven. Als gevolg van vertraging bij het configureren van controle, wordt een andere Stream Analytics-taak maken in dezelfde regio binnen 30 minuten gevraagd voor het opgeven van een tweede account opslag in plaats van met een onlangs hebt geconfigureerd in de vervolgkeuzelijst opslag-Account controleren. Als u wilt voorkomen dat een onnodige opslag-account maakt, wacht 30 minuten na het maken van een taak in een regio voor de eerste keer voor het inrichten van extra taken in die regio.
Upgrade van project|Stream Analytics ondersteunt op dit moment geen live wijzigingen in de definitie of de configuratie van een actieve taak. Zo wijzigt u de invoer, uitvoer, query, schaal of configuratie van een actieve taak, moet u eerst de taak stoppen.
Gegevenstypen die zijn afgeleid van invoerbron|Als een CREATE TABLE-instructie wordt niet gebruikt, het invoertype is afgeleid van een input-indeling, bijvoorbeeld alle velden uit CSV tekenreeks. Velden moeten expliciet worden geconverteerd naar het juiste type CAST-functie gebruiken om te voorkomen dat type probleem fouten.
Ontbrekende velden worden als null-waarden gegenereerd|Verwijzen naar een veld dat is niet aanwezig in de invoerbron resulteert in een null-waarden in het geval van uitvoer.
MET de jaarrekening moeten worden voorafgegaan door SELECT-instructies|SELECT-instructies moeten subquery gedefinieerd met de instructies volgen in uw query.
-Geheugen probleem|Streaming Analytics projecten met een grote tolerantie voor out-volgorde gebeurtenissen en/of complexe query's voor het beheren van dat een grote hoeveelheid staat mogelijk onvoldoende geheugen, wat resulteert in een taak van de taak opnieuw te starten. De start- en stoptijden bewerkingen zijn zichtbaar in de logboeken van de taak. U kunt dit probleem voorkomen door de query uit op verschillende partities worden geschaald. Deze beperking wordt in een toekomstige versie opgelost door tasten de prestaties op het risico van taken in plaats van ze opnieuw op te starten.
Grote blob invoer zonder payload timestamp kunnen-geheugen probleem veroorzaken|Consumptie van grote bestanden uit de Blob-opslag mogelijk Stream Analytics taken vastloopt als een tijdstempelveld is opgegeven via een tijdstempel door. Om te voorkomen dat dit probleem, Houd elke blob onder 10MB groot.
SQL-Database gebeurtenis volume-beperking|Wanneer u SQL Database als een uitvoerdoel, kunnen zeer hoge volumes van uitvoergegevens de taak Stream Analytics time-out veroorzaken. Verminder het volume van de uitvoer met behulp van statistische functies of operatoren filter dit probleem op te lossen of kies Azure Blob-opslag of gebeurtenis Hubs als het uitvoerdoel van een in plaats daarvan.
PowerBI datasets kan slechts één tabel bevatten.|PowerBI ondersteunt niet meer dan één tabel in een gegeven dataset.

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
