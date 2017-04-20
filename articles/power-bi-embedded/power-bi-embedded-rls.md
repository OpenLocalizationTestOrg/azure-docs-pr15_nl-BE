<properties
   pageTitle="Beveiliging met Power BI ingesloten"
   description="Meer informatie over beveiliging op met Power BI ingesloten"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="row-level-security-with-power-bi-embedded"></a>Beveiliging op rijniveau met Power BI ingesloten

Beveiliging op rijniveau (RLS) kan worden gebruikt om de toegang van gebruikers beperken tot bepaalde gegevens in een rapport of een dataset, waardoor meerdere verschillende gebruikers gebruiken hetzelfde rapport tijdens alle zien verschillende gegevens. Power BI ingesloten ondersteunt nu geconfigureerd met RLS datasets.

![](media\power-bi-embedded-rls\pbi-embedded-rls-flow-1.png)

Om te kunnen profiteren van RLS, is het belangrijk dat u begrijpt dat drie belangrijkste concepten; Gebruikers, rollen en regels. Laten we dichter bij elkaar:

**Gebruikers** : dit zijn de werkelijke eindgebruikers bekijken van rapporten. In Power BI ingesloten worden gebruikers geïdentificeerd door de eigenschap username in een App Token.

**Rollen** : gebruikers aan rollen behoren. Een rol is een container voor regels en kan de naam iets als "Manager verkoop" of "Vertegenwoordiger". In Power BI ingesloten worden gebruikers geïdentificeerd door de eigenschap rollen in een App Token.

**Regels** – rollen beschikken over regels en deze regels worden de werkelijke filters die worden toegepast op de gegevens. Dit kan zo eenvoudig zijn ' land = USA ' of iets dynamischer.

### <a name="example"></a>Voorbeeld

Wij bieden een voorbeeld van RLS schrijven en die vervolgens binnen een ingesloten toepassing die voor de rest van dit artikel. Ons voorbeeld gebruikt de [Retail analyse monster](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX-bestand.

![](media\power-bi-embedded-rls\pbi-embedded-rls-scenario-2.png)

Onze Retail analyse voorbeeld toont de verkopen van alle winkels in een bepaalde winkelketen. Zonder RLS, ongeacht welke district manager zich aanmeldt en het rapport bekijkt ziet ze dezelfde gegevens. Directie heeft vastgesteld voor elk district manager ziet alleen de verkoopcijfers voor de winkels die ze beheren en hiervoor gebruiken we RLS.

RLS is geschreven in Power BI-Desktop. Wanneer de dataset en het rapport wordt geopend, kunnen we overschakelen op het diagram weer te geven van het schema:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-3.png)

Hier zijn een paar dingen merken met dit schema:

-   Alle maatregelen, zoals **Totale verkoop**, worden opgeslagen in de feitentabel van de **verkoop** .
-   Er zijn vier extra dimensie van gerelateerde tabellen: **artikel**, **tijd**, **winkel**en **District**.
-   De pijlen op de relatielijnen aangeven welke filters van de ene tabel naar een andere stromen kunnen manier. Bijvoorbeeld als een filter op **[datum] tijd**wordt geplaatst, filteren in het huidige schema het alleen door waarden in de tabel **verkoop** . Geen andere tabellen geldt dit filter omdat alle van de pijlen op de relatielijnen naar de tabel sales en niet opgeslagen wijst.
-   De tabel **District** geeft aan wie de manager is voor elk district:

    ![](media\power-bi-embedded-rls\pbi-embedded-rls-district-table-4.png)

Op basis van dit schema, als we een filter op de kolom **Manager District** in de tabel District toepassen en als die overeenkomt met de gebruiker die het rapport bekijkt filteren, dat filter ook naar de **winkel** en **verkopen** tabellen alleen gegevens worden weergegeven voor die bepaalde district manager worden gefilterd.

Hier ziet u hoe:

1.  Klik op het tabblad modelleren op **Rollen beheren**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-modeling-tab-5.png)

2.  Maak een nieuwe rol **Manager**genoemd.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-6.png)

3.  Voer de volgende expressie in de DAX in de tabel **District** : **[District Manager] = USERNAME()**  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-7.png)

4.  Klik op **weergeven als rollen**om ervoor te zorgen dat de regels werkt, op het tabblad **Vormgeving** en voer vervolgens de volgende:  
![](media\power-bi-embedded-rls\pbi-embedded-rls-view-as-roles-8.png)

    De rapporten worden nu gegevens weergegeven als u bent aangemeld als **Andrew Ma**.

Als u het filter toepast, worden de manier waarop we hier, heeft u alle records in de tabel **District**, **winkel**en **verkoop** gefilterd. Echter door de filter-richting van de relaties tussen de ** **verkoop** - en** **verkoop** en **artikel**, en **artikel** en **tijd** tabellen niet gefilterd naar beneden.

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-9.png)

Dat kan ok voor deze vereiste, echter als we niet willen zien waarvoor ze verkopen geen artikelen, we kunnen inschakelen bidirectionele cross-filtering voor de relatie en het beveiligingsfilter in beide richtingen stromen. Dit kan worden gedaan door de relatie tussen **verkoop** en **Item**als volgt bewerken:

![](media\power-bi-embedded-rls\pbi-embedded-rls-edit-relationship-10.png)

Filters kunnen nu ook stromen uit de tabel verkoop in de tabel **artikel** :

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-11.png)

**Opmerking** Als u de modus DirectQuery voor uw gegevens, moet u inschakelen bidirectionele cross filteren op deze twee opties selecteren:

1.  **Bestand** -> **Opties en instellingen** -> **Voorbeeld functies** -> **inschakelen tussen filteren in beide richtingen voor DirectQuery**.
2.  **Bestand** -> **Opties en instellingen** -> **DirectQuery** -> **onbeperkte maatregel in de modus DirectQuery toestaan**.


Voor meer informatie over het bidirectionele cross filteren, download het witboek [bidirectionele cross-filtering in SQL Server Analysis Services 2016 en Power BI bureaublad](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) .

Dit loopt van het werk dat moet worden gedaan in Power BI-Desktop, maar er is één meer stuk werk dat worden gedaan moet om de RLS regels dat hebben we werk gedefinieerd in Power BI ingesloten. Gebruikers worden geverifieerd en geautoriseerd door de toepassing en App-verificatietokens worden gebruikt voor die gebruikerstoegang geven tot een bepaald rapport Power BI ingesloten. Power BI ingesloten heeft geen specifieke gegevens die op die de gebruiker is. Voor RLS te werken, moet u enkele extra context doorgeven als onderdeel van uw token app:
-   **gebruikersnaam** (optioneel) – met RLS gebruikt dit is een tekenreeks die kan worden gebruikt ter identificatie van de gebruiker bij het toepassen van regels voor RLS. Zie beveiliging met Power BI ingesloten rij gebruiken
-   **rollen** : een tekenreeks met de rollen die bij de toepassing van de regels voor de beveiliging op rij selecteren. Als meer dan één functie doorgeeft, moet het worden doorgegeven als een tekenreeks.

Als de eigenschap username aanwezig is, moet u ten minste één waarde ook doorgeven in rollen.

De volledige app-token wordt als volgt uitzien:

![](media\power-bi-embedded-rls\pbi-embedded-rls-app-token-string-12.png)

Nu, met alle onderdelen samen, wanneer iemand zich aanmeldt bij de toepassing weer te geven in deze lijst, zij alleen zult kunnen zien van de gegevens die ze zien, kunnen zoals gedefinieerd door de beveiliging op.

![](media\power-bi-embedded-rls\pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Zie ook
[Beveiliging (RLS) met de kracht van](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)
