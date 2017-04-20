<properties
    pageTitle="Azure AD verbinden: Ondersteunde topologieën | Microsoft Azure"
    description="In dit onderwerp een gedetailleerd overzicht van ondersteunde en niet-ondersteunde topologieën voor Azure AD verbinden"
    services="active-directory"
    documentationCenter=""
    authors="AndKjell"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="billmath"/>

# <a name="topologies-for-azure-ad-connect"></a>Topologieën voor Azure AD verbinding
Het doel van dit onderwerp is voor het beschrijven van verschillende op gebouwen en Azure AD topologieën met Azure AD verbinden synchronisatie als de belangrijkste integratie oplossing. Ondersteunde en niet-ondersteunde configuraties worden beschreven.

Legenda voor afbeeldingen in het document:

Beschrijving | Pictogram
-----|-----
Active Directory-forest ruimten| ![AD](./media/active-directory-aadconnect-topologies/LegendAD1.png)
Active Directory met gefilterde importeren| ![AD](./media/active-directory-aadconnect-topologies/LegendAD2.png)
Azure AD verbinden synchronisatieserver| ![Sync](./media/active-directory-aadconnect-topologies/LegendSync1.png)
Azure AD verbinding maken met server "Staging Synchronisatiemodus"| ![Sync](./media/active-directory-aadconnect-topologies/LegendSync2.png)
GALSync met FIM2010 of MIM2016| ![Sync](./media/active-directory-aadconnect-topologies/LegendSync3.png)
Azure AD verbinden synchronisatieserver, gedetailleerde| ![Sync](./media/active-directory-aadconnect-topologies/LegendSync4.png)
Azure AD directory |![AAD](./media/active-directory-aadconnect-topologies/LegendAAD.png)
Niet-ondersteunde scenario | ![Niet-ondersteunde](./media/active-directory-aadconnect-topologies/LegendUnsupported.png)

## <a name="single-forest-single-azure-ad-tenant"></a>Eén forest, één huurder Azure AD
![Één huurder één Forest](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

De meest algemene topologie is één forest op-ruimten, met een of meerdere domeinen en een enkele Azure AD pachters. Voor de verificatie van AD Azure wordt Wachtwoordsynchronisatie gebruikt. De aangepaste installatie van Azure AD Connect ondersteunt alleen deze topologie.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Één forest meerdere synchronisatie servers naar één Azure AD pachters
![Één Forest gefilterd wordt niet ondersteund](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Het wordt niet ondersteund als u wilt dat meerdere Azure AD verbinden synchronisatie servers aangesloten op de dezelfde Azure AD huurder, met uitzondering van een [staging-server](#staging-server). Het wordt niet ondersteund, zelfs als deze zijn geconfigureerd voor het synchroniseren van elkaar uitsluitende set objecten. U zou hebben beschouwd als dit als u alle domeinen in het forest met een enkele server of verdelen over meerdere server niet kan bereiken.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Meerdere forests één huurder Azure AD
![Multi-Forest één huurder](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Veel hebben organisaties omgevingen met meerdere Active Directory-forests op gebouwen. Er zijn diverse redenen om met meer dan één Active Directory-forest op gebouwen. Typische voorbeelden zijn ontwerpen met account-resource forests en daardoor na een fusie of overname.

Wanneer er meerdere forests, alle forests moet kunnen worden bereikt door één Azure AD verbinding maken met de server synchroniseren. U hoeft niet de server toevoegen aan een domein. Indien nodig tot alle forest, kan de server in een DMZ-netwerk worden geplaatst.

De installatiewizard Azure AD Connect biedt verschillende opties om samen te voegen gebruikers in meerdere forests vertegenwoordigd. Het doel is dat een gebruiker slechts eenmaal in Azure AD wordt weergegeven. Er zijn enkele algemene topologieën die u in het pad voor aangepaste installatie van de installatiewizard configureren kunt. De corresponderende optie die uw topologie op de pagina **een unieke identificatie van uw gebruikers**. De consolidatie is alleen geconfigureerd voor gebruikers. Gedupliceerde groepen zijn niet samengevoegd met de standaardconfiguratie.

Algemene topologieën worden besproken in de volgende sectie: [verschillende topologieën](#multiple-forests-separate-topologies)en [Full mesh](#multiple-forests-full-mesh-with-optional-galsync) [Account Resource](#multiple-forests-account-resource-forest).

De standaardconfiguratie in Azure AD verbinden sync wordt ervan uitgegaan dat:

1. Gebruikers hebben alleen een ingeschakelde account en het forest waarin deze account zich bevindt, wordt gebruikt om de gebruiker te verifiëren. Deze aanname is voor beide wachtwoord synchronisatie en federation. UserPrincipalName en sourceAnchor/immutableID komen in dit forest.
2. Gebruikers hebben alleen een postvak.
3. Het forest dat als host fungeert voor de postbus van een gebruiker heeft de beste kwaliteit van de gegevens voor kenmerken die zichtbaar is in de Exchange algemene adreslijst (GAL). Er is geen postbus voor de gebruiker, kan een forest worden gebruikt om bij te dragen deze kenmerkwaarden.
4. Als u een gekoppelde postbus hebt, is ook een andere account in een ander forest die wordt gebruikt voor het aanmelden.

Als uw omgeving niet overeenkomt met deze veronderstellingen, gebeurt het volgende:

- Als er meer dan één actieve account of meer dan één postvak, wordt de synchronisatie-engine een picks en negeren de andere.
- Een gekoppelde postbus met geen actieve account wordt niet geëxporteerd naar Azure AD. De gebruikersaccount wordt niet weergegeven als lid in elke groep. Altijd wordt een gekoppelde postbus in DirSync aangeduid als een normale postvak. Deze wijziging is opzettelijk een ander gedrag aan een betere ondersteuning van scenario's met meerdere forests.

Meer informatie vindt u bij het [begrijpen van de standaard configuation](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Meerdere forests, meerdere synchronisatie servers aan een AD Azure huurder
![Multi Forest meerdere synchronisatie wordt niet ondersteund](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Het wordt niet ondersteund als u meer dan één verbinding Sync Azure AD server met een enkele verbonden huurder Azure AD. De uitzondering is het gebruik van een [staging-server](#staging-server).

### <a name="multiple-forests--separate-topologies"></a>Meerdere forests – verschillende topologieën
**Gebruikers worden slechts éénmaal weergegeven in alle mappen**

![Eenmaal Forest met meerdere gebruikers](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Multi-Forest gescheiden topologieën](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

In deze omgeving, alle forests op-ruimten worden behandeld als afzonderlijke entiteiten en geen gebruiker wordt gemaakt in een ander forest.
Elk forest heeft een eigen Exchange-organisatie en er is geen GALSync tussen de forests. Deze topologie kan de situatie na een fusie/overname of in een organisatie waar elke business unit wordt uitgevoerd geïsoleerd van elkaar zijn. Deze bossen worden in dezelfde organisatie in Azure AD en weergegeven met een uniforme GAL.
In deze afbeelding wordt elk object in elk forest éénmaal weergegeven in de metaverse en samengevoegd in de huurder doel Azure AD.

### <a name="multiple-forests--match-users"></a>Meerdere forests – gebruikers overeenkomst
**Gebruikersidentiteiten bestaan in meerdere mappen**

Voor deze scenario's is dat distributielijsten en beveiligingsgroepen kunnen bevatten een combinatie van gebruikers, contactpersonen en FSP's (afwijkende beveiligings-Principals)

FSP's worden gebruikt in ADDS voor leden uit andere forests in een beveiligingsgroep. Alle FSP's worden omgezet naar het juiste object in AD Azure.

### <a name="multiple-forests--full-mesh-with-optional-galsync"></a>Meerdere forests – volledige gaas met optionele GALSync
**Gebruikersidentiteiten aanwezig in meerdere mappen zijn. Werken met behulp van: E-mailkenmerk**

![Forest met meerdere gebruikers E-mail](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Multi-Forest Full Mesh](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Een full-meshtopologie kan gebruikers en bronnen in elk forest bevinden en vaak zou er vertrouwensrelaties in twee richtingen tussen de forests.

Als Exchange aanwezig in meer dan één forest is, kan eventueel er een GALSync-oplossing op locatie. Elke gebruiker wordt aangeduid als een contactpersoon in alle andere forests. GALSync is meestal geïmplementeerd met behulp van Forefront Identiteitenbeheer 2010 of Microsoft Identity Manager 2016. Azure AD-verbinding kan niet worden gebruikt voor GALSync voor gebouwen.

In dit scenario worden identiteit objecten gekoppeld door middel van het e-mailkenmerk. Een gebruiker met een postvak in het ene forest is gekoppeld met de contacten in de andere forests.

### <a name="multiple-forests--account-resource-forest"></a>Meerdere Forests – Account bronforest
**Gebruikersidentiteiten aanwezig in meerdere mappen zijn. Werken met behulp van: ObjectSID en msExchMasterAccountSID kenmerken**

![Forest met meerdere gebruikers ObjectSID](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Forest met meerdere AccountResource](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

U hebt een of meer forests account aan actieve gebruikersaccounts in een topologie rekening bron-forest. U hebt ook een of meer resources forests met uitgeschakelde accounts.

In dit scenario een (of meer) **bronforest** -vertrouwensrelaties alle **account-forests**. Het bronforest heeft meestal een AD-schema uitgebreid met Exchange en Lync. Alle Exchange en Lync diensten, alsmede andere gedeelde services bevinden zich in dit forest. Gebruikers hebben een uitgeschakelde gebruikersaccount in het forest en het postvak is gekoppeld aan het accountforest.

## <a name="office-365-and-topology-considerations"></a>Office 365 en topologie overwegingen
De werkbelasting van sommige Office 365 moeten bepaalde beperkingen ondersteunde topologieën. Als u van plan bent te gebruiken een van deze, lees het onderwerp ondersteunde topologieën voor de werklast.

Werkbelasting |  
--------- | ---------
Exchange Online | Als er meer dan één Exchange organisatie op de gebouwen (dat wil zeggen Exchange is geïmplementeerd in meer dan één forest), en vervolgens moet u Exchange 2013 SP1 of hoger. Meer informatie vindt u hier: [hybride implementaties met meerdere Active Directory-forests](https://technet.microsoft.com/library/jj873754.aspx)
Skype voor bedrijven | Als u meerdere forests op-ruimten, wordt alleen de account resource forest topologie ondersteund. Details voor ondersteunde topologieën u hier vindt: [milieu-eisen voor Skype voor Business Server 2015](https://technet.microsoft.com/library/dn933910.aspx)

## <a name="staging-server"></a>Staging-server
![Staging-Server](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD Connect ondersteunt het installeren van een tweede server in de **Staging-modus**. Een server in deze modus leest gegevens van alle gekoppelde mappen maar iets niet schrijven naar mappen verbonden. Het is met behulp van de synchronisatie van normale cyclus en heeft daarom een bijgewerkte kopie van de id-gegevens. In een noodsituatie wanneer de primaire server niet kunt failover uitvoeren naar de staging-server. Dit doet u in de wizard Azure AD verbinden. Deze tweede server kan bij voorkeur in een ander datacenter zich bevinden, omdat er geen infrastructuur wordt gedeeld met de primaire server. Elke configuratiewijziging in op de tweede server de primaire server, moet u handmatig kopiëren.

Een staging-server kan ook worden gebruikt voor het testen van een nieuwe aangepaste configuratie en het effect dat dit op uw gegevens heeft. U kunt de wijzigingen bekijken en aanpassen van de configuratie. Wanneer u tevreden over de nieuwe configuratie bent, kunt u de actieve server de staging-server maken en stelt de oude active server in staging-modus.

Deze methode kan ook worden gebruikt ter vervanging van de ActiveSync-server. Voorbereiding van de nieuwe server en stel deze in de staging-modus. Zorg ervoor dat het is in goede staat, staging-modus (waardoor dit actieve), uitschakelen en de actieve server afsluiten.

Het is mogelijk om meer dan een staging-server als u meerdere back-ups in verschillende datacenters.

## <a name="multiple-azure-ad-tenants"></a>Meerdere AD Azure huurders
Microsoft raadt een enkele huurder Azure advertentie voor een organisatie.
Voordat u van plan bent meerdere AD Azure huurders gebruiken deze onderwerpen betrekking hebben op algemene scenario's, zodat u kunt een enkele huurder gebruiken.

Onderwerp |  
--------- | ---------
Overdracht met administratieve eenheden | [Beheer van bestuurlijke eenheden in Azure AD](active-directory-administrative-units-management.md)

![Forest-Multi multi huurder](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Er is een 1:1 relatie tussen een sync Azure AD verbinding maken met server en een huurder Azure AD. U moet voor elke huurder Azure AD één Azure AD verbinden sync serverinstallatie. De huurder Azure AD exemplaren worden door alleenstaande ontwerp en gebruikers in een gebruikers in de andere huurder niet zien. Als deze scheiding is bestemd, en dit een ondersteunde configuratie is, maar anders moet u de ene Azure AD huurder model.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Elk object slechts één keer in een huurder Azure AD
![Één Forest gefilterd](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

In deze topologie is één Azure AD verbinden sync-server verbonden met elke huurder Azure AD. De Azure AD verbinden sync-servers moeten worden geconfigureerd voor het filteren, zodat elk hebben die elkaar wederzijds uitsluiten objecten te bewerken. U kunt bijvoorbeeld het bereik van elke server op een bepaald domein of organisatie-eenheid. Een DNS-domein, kan alleen worden geregistreerd in één AD Azure huurder. De UPN-namen van de gebruikers in de lokalen op AD ook een aparte naamruimten moet gebruiken. Bijvoorbeeld achtervoegsels in de afbeelding hierboven drie afzonderlijke UPN zijn geregistreerd in de lokalen op AD: contoso.com en fabrikam.com wingtiptoys.com. De gebruikers in elk op-premises AD-domein een andere naamruimte gebruiken.

Er is geen GALsync tussen de huurder Azure AD exemplaren. Het adresboek in Exchange Online en Skype voor Business alleen geeft gebruikers in de dezelfde huurder.

Deze topologie bevinden zich de volgende beperkingen op andere wijze ondersteund scenario's:

- Slechts één van de huurders Azure AD kunt Exchange hybride met Active Directory op gebouwen.
- Windows 10-apparaten kunnen alleen worden gekoppeld aan één AD Azure huurder.

De eis van elkaar uitsluitende set objecten geldt ook voor write-back. Sommige Write-back-functies worden niet ondersteund met deze topologie omdat deze functies wordt ervan uitgegaan een enkele configuratie dat op locatie:

-   Terugschrijven naar de groep met de standaardconfiguratie
-   Apparaat Write-back

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Elk object meerdere keren in een huurder Azure AD
![Multi-huurder één Forest wordt niet ondersteund](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Één Forest meerdere Connectors wordt niet ondersteund](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

- Het wordt niet ondersteund als u wilt synchroniseren dezelfde gebruiker aan meerdere AD Azure huurders.
- Is het maken van een configuratie wijzigen als u wilt dat gebruikers in een Azure advertentie weergegeven als contactpersonen in een ander AD Azure huurder wordt niet ondersteund.
- Deze synchronisatie Azure AD verbinden verbinding maken met meerdere AD Azure huurders wijzigen niet wordt ondersteund.

### <a name="galsync-by-using-writeback"></a>GALsync met behulp van Write-back
![MultiForestMultiDirectoryGALSync1Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![MultiForestMultiDirectoryGALSync2Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD huurders worden door alleenstaande ontwerp.

- Het wordt niet ondersteund als u de configuratie van synchronisatie voor het lezen van gegevens uit een andere Azure AD huurder Azure AD verbinden.
- Het wordt niet ondersteund voor gebruikers als contactpersonen die u wilt exporteren een andere op-premises Azure AD verbinden synchroniseren met AD.

### <a name="galsync-with-on-premises-sync-server"></a>GALsync met synchronisatieserver voor gebouwen
![MultiForestMultiDirectoryGALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

Het wordt gebruik van FIM2010/MIM2016 op-ruimten voor GALsync gebruikers tussen twee Exchange-organisaties ondersteund. De gebruikers in een organisatie weergegeven als externe gebruikers/contactpersonen in de andere organisatie. Deze advertenties andere op plaatsen kunnen vervolgens worden gesynchroniseerd naar hun eigen huurders Azure AD.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het installeren van Azure AD verbinden voor deze scenario's, Zie [aangepaste installatie van Azure AD verbinden](./connect/active-directory-aadconnect-get-started-custom.md).

Meer informatie over het configureren van [Azure AD Connect worden gesynchroniseerd](active-directory-aadconnectsync-whatis.md) .

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
