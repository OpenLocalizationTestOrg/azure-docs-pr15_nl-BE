<properties
   pageTitle="Het beheren van gegevens activa | Microsoft Azure"
   description="Artikel markeren van de zichtbaarheid en de eigendom van de activa van de gegevens geregistreerd in Azure gegevenscatalogus."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-manage-data-assets"></a>Het beheren van gegevens activa

## <a name="introduction"></a>Inleiding

**Catalogus met Azure gegevens** biedt mogelijkheden voor gegevensbron te zoeken, zodat gebruikers gemakkelijk ontdekken en begrijpen van de gegevensbronnen die ze nodig hebben om analyse en beslissingen te nemen. Deze detectie-mogelijkheden maken de grootste impact wanneer alle gebruikers kunnen vinden en begrijpen van een breed scala aan beschikbare gegevensbronnen. Met dit in gedachten, het standaardgedrag van de catalogus gegevens is voor alle geregistreerde gegevensbronnen worden zichtbaar voor – en kan worden gevonden door – alle gebruikers de catalogus.

Gegevenscatalogus geeft gebruikers toegang tot de gegevens zelf. Toegang tot gegevens wordt bepaald door de eigenaar van de gegevensbron. Gegevenscatalogus kan gebruikers gegevensbronnen te ontdekken en de metagegevens die verband houden met de bronnen die zijn geregistreerd in de catalogus weergeven.

Er zijn situaties, maar welke gegevensbronnen alleen zichtbaar aan specifieke gebruikers of groepen zijn moeten. Voor deze scenario's catalogus gegevens kunnen gebruikers eigenaar van de activa van de geregistreerde gegevens in de catalogus en vervolgens bepalen de zichtbaarheid van de activa ze eigenaar.

> [AZURE.NOTE] De functionaliteit die in dit artikel worden beschreven zijn alleen beschikbaar in de Standard Edition van Azure catalogus gegevens. De gratis versie biedt geen mogelijkheden voor het eigendom en de zichtbaarheid van de activa die gegevens.

## <a name="managing-ownership-of-data-assets"></a>Eigendom van de activa gegevens beheren
Gegevens activa in catalogus gegevens geregistreerd worden standaard zonder eigenaar; elke gebruiker met een machtiging voor toegang tot de catalogus kunt ontdekken en aantekeningen van deze activa. Gebruikers eigenaar kunnen worden van activa zonder eigenaar gegevens en de zichtbaarheid van de activa die ze eigenaar kunnen beperken.

Wanneer het eigendom van een actief gegevens in de catalogus gegevens is alleen gebruikers die zijn gemachtigd door de eigenaren kunnen ontdekken van de activa en de metagegevens weergeven en alleen de eigenaren van de activa van de catalogus kunnen verwijderen.

> [AZURE.NOTE] Eigendom in catalogus gegevens is alleen van invloed op de opgeslagen metagegevens in de catalogus. Het verleent geen machtigingen op de onderliggende gegevensbron.

### <a name="taking-ownership"></a>Eigenaar worden
Gebruikers kunnen eigenaar van de activa van de gegevens worden door de "Eigenaar" optie in de portal gegevenscatalogus. Geen speciale machtigingen zijn vereist voor de eigenaar worden van een actief zonder eigenaar gegevens; elke gebruiker kan eigenaar worden van een actief zonder eigenaar gegevens.

### <a name="adding-owners-and-co-owners"></a>Eigenaren en eigenaren van collega toevoegen
Als een activum gegevens reeds eigendom is, kunnen geen gebruikers gewoon eigenaar: ze moeten worden toegevoegd als co-eigenaar door de eigenaar van een bestaande. Elke eigenaar kunt extra gebruikers of beveiligingsgroepen toevoegen als collega eigenaren.

> [AZURE.NOTE] Het is raadzaam om ten minste twee personen als eigenaren voor activa eigendom gegevens.

### <a name="removing-owners"></a>Eigenaren verwijderen
Net als de eigenaar van een activum collega eigenaars toevoegen kunt, kan de eigenaar van een activum mede-eigenaar verwijderen.

Als eigenaar van een activum zelf als een eigenaar verwijderd, kan hij niet langer de activa te beheren. Als eigenaar van een activum zelf als een eigenaar verwijderd en er geen collega eigenaars zijn, wordt het activum een zonder eigenaar staat hersteld.

## <a name="visibility"></a>Zichtbaarheid
Gegevens actief eigenaren kunnen bepalen de zichtbaarheid van de gegevens activa die ze eigenaar zijn. Zichtbaarheid van de standaard – beperken kunt waar alle gebruikers van de catalogus gegevens kunnen ontdekken en weergeven van de gegevens activa: de eigenaar van de activa schakelen de zichtbaarheidsinstelling van 'Iedereen' naar 'Eigenaren en deze gebruikers' in de eigenschappen voor het activum. Eigenaren kunnen vervolgens specifieke gebruikers en beveiligingsgroepen toevoegen.

> [AZURE.NOTE] Indien mogelijk, moeten activa eigendom en zichtbaarheid machtigingen worden toegewezen aan beveiligingsgroepen en niet aan afzonderlijke gebruikers.

## <a name="catalog-administrators"></a>Catalogus-beheerders
Catalogus Gegevensbeheerders zijn impliciet collega eigenaren van alle activa in de catalogus. Asset eigenaren zichtbaarheid niet verwijderen uit catalogus administrators en beheerders kunnen beheren, eigendom en de zichtbaarheid van alle activa van de gegevens in de catalogus.

## <a name="summary"></a>Samenvatting
Gegevenscatalogus crowdsourcing model gegevens en metagegevens asset discovery kunnen alle gebruikers van de catalogus te dragen en te ontdekken. De Standard Edition van catalogus gegevens biedt mogelijkheden voor eigendom en beheer van de zichtbaarheid en het gebruik van specifieke gegevens activa te beperken.
