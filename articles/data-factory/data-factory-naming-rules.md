<properties 
    pageTitle="Data Factory - regels voor naamgeving | Microsoft Azure" 
    description="Beschrijving van de naamgevingsregels voor Data Factory entiteiten." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - regels voor naamgeving 
De volgende tabel bevat de naamgevingsregels voor Data Factory artefacten.



Naam | De unieke naam | De validatie wordt gecontroleerd
:--- | :-------------- | :----------------
Data Factory | Uniek zijn voor Microsoft Azure. De namen zijn niet hoofdlettergevoelig, dat wil zeggen, MyDF en mydf verwijzen naar dezelfde gegevens fabriek. |<ul><li>Elke fabriek gegevens is gekoppeld aan precies één Azure abonnement.</li><li>Objectnamen moeten beginnen met een letter of een nummer en kunnen alleen letters, cijfers en het koppelteken (-)-teken bevatten.</li><li>Elk streepje (-) teken moet onmiddellijk worden voorafgegaan en gevolgd door een letter of een nummer. Opeenvolgende streepjes zijn niet toegestaan in namen van de container.</li><li>De naam kan 3-63 tekens lang zijn.</li></ul>
Gekoppelde tabellen/Services/pijpleidingen | De unieke met in een fabriek van gegevens. De namen zijn niet hoofdlettergevoelig. | <ul><li>Maximum aantal tekens in een tabelnaam: 260.</li><li>Namen moeten beginnen met het nummer van een letter of een onderstrepingsteken (_).</li><li>Volgende tekens zijn niet toegestaan: ".", "+", "?", "/" "<", ">","*", "%", "&", ":","\\"</li></ul>
Resourcegroep | Uniek zijn voor Microsoft Azure. De namen zijn niet hoofdlettergevoelig. | <ul><li>Maximum aantal tekens: 1000.</li><li>De naam kan letters, cijfers en de volgende tekens bevatten: "-", "_",""en".".</li></ul>