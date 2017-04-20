
<properties 
    pageTitle="Beveiligen van de toegang tot Azure RemoteApp- en buiten | Microsoft Azure"
    description="Meer informatie over hoe veilig toegang tot RemoteApp Azure met behulp van voorwaardelijke toegang in Azure Active Directory"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="securing-access-to-azure-remoteapp-and-beyond"></a>Beveiligen van de toegang tot Azure RemoteApp- en buiten

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

In dit artikel geven wij een overzicht van hoe een beheerder een kanaal voor veilige toegang vanaf de eindgebruiker via Azure RemoteApp- en eindigend met een beveiligde bron, zoals een SQL-database of een andere toepassing back-end kunt instellen. Het doel is om ervoor te zorgen dat alleen gemachtigde gebruikers voldoen aan de gewenste voorwaarden toegang krijgen tot externe toepassingen, en dat de beveiligde back-end is alleen toegankelijk vanuit de testomgeving Azure RemoteApp- en niet vanuit andere locaties.

Er zijn 3 belangrijke gebieden die Admin nodig heeft om te bekijken:

![Overwegingen met betrekking tot Azure RemoteApp-voorwaardelijke toegang](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Lezen op voor informatie en antwoorden op deze vragen.

## <a name="who-can-access-the-collection"></a>Wie toegang heeft tot de collectie?
De beheerder wordt gekozen voor de gebruikers die toegang hebben tot externe toepassingen in de collectie. U kunt Azure Active Directory (AD Azure) werk of school (voorheen, 'organisatie accounts') of Microsoft grootboekrekeningen (bv. @outlook.com). De meeste scenario's voor enterprise gebruiken Azure AD rekeningen; Deze kunnen u voorwaardelijke toegang tot functies die verderop worden beschreven en zijn ook de enige keuze voor verzamelingen domein behoren. De rest van het artikel wordt ervan uitgegaan dat u accounts Azure AD in combinatie met Azure RemoteApp.

**Wat we hebben bereikt:**

Azure AD accounts met toegang tot RemoteApp Azure levert ons twee dingen:

1.  We altijd weten wie toegang heeft tot de toepassingen die we hebben gepubliceerd en toegang krijgen tot alle back-ends die toepassingen verbinding.
2.  We bepalen de onderliggende Azure AD zodat we maken kunnen en gebruikersaccounts verwijderen, wachtwoordbeleid instellen, meerledige verificatie, enz gebruiken. 

## <a name="how-is-the-collection-accessed-from-where"></a>Hoe is de collectie zijn geopend? Waarvandaan?
Beheerders willen vaak beleid voor toegang tot een openbare internetverbinding omgeving, zoals Azure RemoteApp definiëren. Ze willen bijvoorbeeld ervoor te zorgen dat gebruikers toegang hebben tot de omgeving van buiten het netwerk gebruiken meerledige verificatie (MVR gesloten moeten) toegang; of misschien ze helemaal niet moeten worden geblokkeerd.

Azure RemoteApp-beheerders kunnen de functionaliteit beschikbaar via Azure AD Premium gebruiken om voorwaardelijke toegang beleidsregels voor hun omgeving Azure RemoteApp. Ze kunnen ook rapporten met opmaak en signalering functies gebruiken om te controleren hoe de omgeving wordt geopend.

### <a name="how-to-set-up-conditional-access-for-azure-remoteapp"></a>Het instellen van voorwaardelijke toegang voor Azure RemoteApp
We gaan een voorbeeldscenario – de Azure RemoteApp-beheerder wil de toegang tot de omgeving blokkeren wanneer gebruikers buiten het bedrijfsnetwerk worden doorlopen.

>[AZURE.NOTE] Nemen we aan Azure AD upgrade hebt uitgevoerd naar de premie laag en dat u ten minste één Azure RemoteApp-collectie hebt gemaakt.

1.  Klik op het tabblad **Active Directory** in Azure portal. Klik vervolgens op de map die u wilt configureren.

    Denk eraan: Voorwaardelijke toegang is een eigenschap van de map en niet van RemoteApp-Azure, zodat de configuratie wordt verricht op het mapniveau van de. Dit betekent ook moet u de directory-beheerder deze wijzigingen aanbrengen.

2.  Klik op **toepassingen**en klik vervolgens op **Microsoft Azure RemoteApp** om voorwaardelijke toegang in te stellen. Houd er rekening mee dat u voorwaardelijke toegang voor elke toepassing "software als service" in de map afzonderlijk instellen kunt.
![Voorwaardelijke toegang instellen voor Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
 

3.  Stel op het tabblad **configureren** **Toegangsregels inschakelen** op ON.
![Toegangsregels voor Azure RemoteApp inschakelen](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
 

4.  U kunt nu verschillende regels configureren en kiezen die ze kunnen toepassen:

    1. Kies **blokkeren toegang wanneer niet op het werk** volledig te voorkomen dat gebruikers toegang krijgen tot RemoteApp Azure buiten de netwerkomgeving die u opgeeft.
    2. Klik hieronder voor het definiëren van de IP-adresbereiken de 'vertrouwde 'netwerk vormen. Alles wat buiten deze worden afgewezen.

5.  Uw configuratie testen door het starten van de RemoteApp-Azure-client een IP-adres buiten het bereik dat u hebt opgegeven. Nadat u zich met uw referenties Azure AD aanmelden ziet u een bericht als volgt:

![Toegang geweigerd tot Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessdenied.png)
 

### <a name="future-conditional-access-features"></a>Functies voor voorwaardelijke toegang 
Het team van Azure Active Directory werkt aan nieuwe mogelijkheden in voorwaardelijke toegang. Beheerders is het mogelijk nieuwe typen regels buiten het netwerk op basis van om locatieregels te maken. Een openbare preview van de nieuwe functionaliteit moet binnenkort beschikbaar.

### <a name="how-to-monitor-access-to-azure-remoteapp"></a>Het controleren van Azure RemoteApp
Een handige functie om te gebruiken naast voorwaardelijke toegang is Azure Active Directory Premium rapportagefuncties. U kunt rapporten controleren wie toegang heeft tot uw omgeving en verdachte activiteiten te detecteren.

Bijvoorbeeld, ziet u de namen van de gebruikers die toegang krijgen tot RemoteApp Azure, hoe deden ze dat vaak en wanneer.

1.  Klik op **Active Directory**in Azure portal en klik vervolgens op de map.

2.  Ga naar het tabblad **rapporten** .

3.  Selecteer in de lijst met rapporten **toepassingsgebruik** onder **geïntegreerde toepassingen**.

    Ziet u enkele statistische gegevens voor Azure RemoteApp. 
![Geaggregeerde statistieken van Azure RemoteApp-toegang](./media/remoteapp-secureaccess/ra-accessstats.png)
 
5.  Klik op de aanvraag om informatie over de gebruikers toegang hebben tot RemoteApp Azure zichtbaar te maken.
![Gebruiker toegang statistieken voor RemoteApp-Azure](./media/remoteapp-secureaccess/ra-userstats.png)
 
### <a name="summary"></a>Samenvatting
Met Azure Active Directory Premium kunt u toegangsregels instellen om tot RemoteApp-Azure (en andere software als een servicetoepassingen beschikbaar via Azure AD). Regels zijn momenteel alleen beschikbaar voor het netwerkbeleid op basis van locatie, maar in de toekomst zal worden uitgebreid tot andere aspecten van bedrijfsbeheer.

Azure AD Premium biedt ook melden en mogelijkheden die het besturingselement verder admin uitbreiden controle heeft over hun Azure RemoteApp-omgeving.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Hoe zorg ik ervoor dat mijn beveiligde bron is alleen toegankelijk vanuit Mijn omgeving Azure RemoteApp?
In de vorige secties van dit artikel gingen we gericht op het beveiligen van de toegang tot de RemoteApp-Azure-omgeving. Dat hebben we bereikt door te kiezen welke gebruikers toegang hebben en instellen van de toegangsregels om te bepalen hoe zij de service kunnen gebruiken.

Een algemeen scenario voor Azure RemoteApp-implementaties is dat externe toepassingen hoeft te communiceren met een back-end-bron, bijvoorbeeld een SQL-database. Deze bron wordt gehost op op-bedrijfsruimten (bv. in een bedrijfsnetwerk) of in de cloud (bv. in Azure IaaS). Beheerders willen vaak om ervoor te zorgen dat de back-end-resource is alleen toegankelijk door toepassingen geïmplementeerd via RemoteApp Azure en niet bijvoorbeeld een toepassing rechtstreeks op de PC van de gebruiker uitgevoerd en via het openbare Internet toegang hebben tot. Azure RemoteApp wordt vaak gezien als de centraal beheerde en beveiligde omgeving en dus het enige pad via welke gebruikers met de back-end-bron samenwerken moeten.

De oplossing is om zowel de Azure RemoteApp-omgeving en de beveiligde bron in de dezelfde Azure Virtual Network (VNET). Als de resource in een andere site, kunt u een site naar site VPN-verbinding, bijvoorbeeld voor het maken van een VNet spanning Azure datacenter en de klant op locatie omgeving.

Azure RemoteApp ondersteunt twee soorten collectie implementaties waarin u uw eigen VNET kunt opgeven:

-   Niet tot een domein behoren: de toepassingen hebben "onbelemmerd zicht" van de andere bronnen in de VNET. Bijvoorbeeld, dit kan worden gebruikt voor toepassingen met een SQL-database die gebruikmaakt van SQL-verificatie verbinding (de gebruiker rechtstreeks op de database toepassingen geverifieerd)

-   Een domein behoren: de virtuele machines die worden gebruikt door Azure RemoteApp lid zijn van een domeincontroller in de VNET. Dit is handig wanneer u de toepassingen moeten worden geverifieerd bij een Windows-domeincontroller om toegang tot een bron van de back-end.
![Een collectie domein behoren in Azure RemoteApp](./media/remoteapp-secureaccess/ra-domainjoined.png)
 
### <a name="how-to-create-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Het maken van een beveiligde verbinding tussen Azure en Mijn omgeving in gebouwen
Er zijn diverse configuratieopties voor het aansluiten van uw Azure en op gebouwen-omgevingen. Er is hier een goed overzicht van de opties beschikbaar.

Met RemoteApp-Azure, moet u eerst de VNet configureren en deze vervolgens gebruiken tijdens het maken van uw collectie. 

## <a name="the-complete-solution"></a>De complete oplossing
Het onderstaande diagram ziet u de volledige oplossing waar we een veilig kanaal van de gebruiker, via Azure RemoteApp (ARA), zijn ingebouwd in de backend-bron.
![Azure RemoteApp Secure](./media/remoteapp-secureaccess/ra-secureoverview.png) In fase 1 we de gebruikers geselecteerd en toegangsregels die bepalen hoe ARA toegankelijk gemaakt. In het volgende voorbeeld toestaan is alleen toegang voor gebruikers die werken met het bedrijfsnetwerk. Niet-compatibele gebruikers worden niet helemaal het ARA-omgeving toegang tot.
In 'Fase 2' hebben we de backend-bron alleen via de VNet/VPN-configuratie waarmee we zichtbaar. Azure RemoteApp is geplaatst in de dezelfde VNet. Het eindresultaat is dat de resource is alleen toegankelijk via de ARA-omgeving.


