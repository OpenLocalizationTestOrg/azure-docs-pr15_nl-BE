<properties
    pageTitle="Beheren, toegang tot het logboek Analytics | Microsoft Azure"
    description="Toegang tot het logboek Analytics met tal van administratieve taken op gebruikers, rekeningen, OMS werkruimten en Azure accounts beheren."
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
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="banders"/>

# <a name="manage-access-to-log-analytics"></a>Toegang tot Analytics logboek beheren

Voor het beheren van toegang tot Analytics logboek kunt u allerlei administratieve taken op gebruikers, rekeningen, OMS werkruimten en Azure accounts. Om een nieuwe werkruimte maken in het Operations Management Suite (OMS), kiest u de naam van een werkruimte te koppelen aan uw account, en u Kies een geografische locatie. Een werkruimte is in wezen een container met accountgegevens en eenvoudige configuratie-informatie voor de account. U of de andere leden van uw organisatie kunnen meerdere OMS-werkruimten gebruiken om verschillende sets met gegevens die worden verzameld uit alle of gedeelten van uw IT-infrastructuur te beheren.

Het artikel [aan de slag met Analytics logboek](log-analytics-get-started.md) ziet u hoe snel u omhoog en uitgevoerd en de rest van dit artikel wordt beschreven in meer detail met enkele acties moet u de toegang beheren tot OMS.

Hoewel u mogelijk niet nodig voor het uitvoeren van elke taak in eerste instantie in beheer, komen alle veelgebruikte taken die u in de volgende secties gebruiken kunt:

- Bepaal het aantal werkruimten die u nodig hebt
- Accounts en gebruikers beheren
- Een groep toevoegen aan een bestaande werkruimte
- Een bestaande werkruimte koppelen aan een Azure-abonnement
- Een werkruimte voor een upgrade naar een betaalde data-abonnement
- Plan het gegevenstype wijzigen
- Een organisatie van Azure Active Directory toevoegen aan een bestaande werkruimte
- Sluit uw werkruimte OMS

## <a name="determine-the-number-of-workspaces-you-need"></a>Bepaal het aantal werkruimten die u nodig hebt

Een werkruimte is een Azure-bron en een container waar de gegevens is verzameld, samengevoegd, geanalyseerd en gepresenteerd in de OMS-portal.

Het is mogelijk om meerdere OMS logboek Analytics werkruimten te maken en voor gebruikers die toegang hebben tot een of meer werkruimten. U wilt in het algemeen tot een minimum beperken van het aantal werkruimten zodat u query's en over de meeste gegevens correleren. In deze sectie wordt beschreven wanneer kan het handig zijn om meer dan een werkruimte te maken.

Vandaag de dag biedt een logboek Analytics werkruimte:

- Een geografische locatie voor opslag van gegevens
- Granulatie voor facturering
- Gegevensisolatie

Op basis van de bovenstaande kenmerken, kunt u meerdere werkruimten maken als:

- U bent een mondiaal bedrijf en moet u de gegevens in specifieke regio's omwille van de gegevens soevereiniteit of conformiteit.
- Azure u gebruikt en u wilt voorkomen dat uitgaande gegevens overdracht kosten door een logboek Analytics werkruimte op hetzelfde gebied, als de Azure door haar beheerde middelen.
- Wilt u toeslagen toewijzen aan verschillende afdelingen of zakelijke groepen op basis van hun gebruik. Wanneer u een werkruimte maken voor elke afdeling of bedrijfsgroep, uw Azure factureren en het gebruik van de instructie geeft de toeslagen voor elke werkruimte afzonderlijk.
- U bent een beheerde serviceprovider en moet houden de logboekgegevens analytics voor elke klant u beheren geïsoleerd van andere gegevens.
- Beheren van meerdere klanten en u wilt dat elke klant of afdeling of bedrijfsgroep zien hun eigen gegevens, maar niet de gegevens voor andere klanten, afdelingen of bedrijven.

Wanneer u de agenten om gegevens te verzamelen, kunt u elke agent aan de werkruimte nodig.

Als u met behulp van System Center Operations manager, kan elke groep Operations Manager beheer worden verbonden met slechts één werkruimte. U kunt Microsoft Monitoring Agent installeren op computers die worden beheerd door Operations Manager en Operations Manager en een andere werkruimte met Analytics logboek in de lijst agent.

### <a name="workspace-information"></a>Werkruimte

U kunt uw werkruimte-informatie bekijken en kiezen of u wilt informatie ontvangen van Microsoft in de portal OMS.

#### <a name="view-workspace-information"></a>Werkruimte-informatie weergeven

1. Klik op de tegel **Instellingen** in OMS.
2. Klik op het tabblad **Accounts** .
3. Klik op het tabblad **Gegevens van de werkruimte** .  
  ![Werkruimte](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Accounts en gebruikers beheren

Elke werkruimte kan meerdere gebruikersaccounts zijn gekoppeld, en elke gebruikersaccount (Microsoft-account of organisatie-account) kan toegang hebben tot meerdere OMS werkruimten.

Standaard wordt het Microsoft-account of de organisatie-account die wordt gebruikt voor het maken van de werkruimte de beheerder van de werkruimte. De beheerder kan vervolgens aanvullende Microsoft-accounts uitnodigen of gebruikers kiezen uit Azure Active Directory.

Mensen toegang krijgen tot de werkruimte OMS geregeld op 2 plaatsen:

- In Azure, kunt u op rollen gebaseerde toegangscontrole voor toegang tot de Azure abonnement en de bijbehorende bronnen Azure. Dit wordt ook gebruikt voor PowerShell en REST API-toegang.
- Toegang tot alleen de OMS portal - niet het bijbehorende Azure-abonnement in de portal OMS.

Als u gebruikers toegang verlenen aan de portal OMS maar niet aan de Azure-abonnement is gekoppeld aan, vervolgens weergeven de tegels van de oplossing voor automatisering, back-up en herstel van de Site niet gegevens aan gebruikers wanneer ze aanmelden de OMS-portal.

Zie de gegevens in deze oplossingen, zorgen ze ten minste beschikken over alle gebruikers **reader** toegang heeft tot voor de kluis Account automatisering, kluis back-up en herstel van de Site die is gekoppeld aan de werkruimte OMS.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Beheren van toegang tot het logboek Analytics met behulp van de portal Azure

Als u gebruikers toegang tot de Log Analytics werkruimte machtigingen Azure geven, in Azure portal bijvoorbeeld vervolgens dezelfde gebruikers toegang tot de portal logboek Analytics. Als gebruikers in de portal Azure, kunnen ze navigeren naar de OMS portal door te klikken op de **Portal OMS** -taak bij het weergeven van de bron van de werkruimte logboek Analytics.

Sommige punten rekening houden over de Azure portal:

- Dit is niet *op rollen gebaseerde toegangscontrole*. Als u toegangsmachtigingen voor *Reader* in Azure portal voor de werkruimte logboek Analytics hebt, kunt u wijzigingen met de OMS-portal maken. De OMS-portal is een concept van de beheerder en Inzender gebruiker alleen-lezen. Als de account die u ingelogd met bent de Azure Active Directory die aan de werkruimte is gekoppeld wordt u een beheerder van de portal OMS, anders zult u een inzendergroep.

- Als u aanmelden bij de OMS portal met http://mms.microsoft.com, standaard, ziet u de lijst **Selecteer een werkruimte** . Het bevat alleen werkruimten die zijn toegevoegd met behulp van de portal OMS. Overzicht van de werkruimten hebt u toegang tot met Azure abonnementen, moet u een huurder als onderdeel van de URL opgeven. Bijvoorbeeld:

  `mms.microsoft.com/?tenant=contoso.com`De id van de huurder is vaak dat laatste deel van het e-mailadres dat u aanmelden met.

- Als de rekening u aanmelden met een account in de huurder Azure Active Directory, dat meestal het geval is als u aanmelden als een CSP bent, zult u een *beheerder* van de portal OMS. Als uw account niet in de huurder Azure Active Directory, kunt u een *gebruiker* in de portal OMS zal zijn.

- Als u gaan rechtstreeks naar een portal wilt dat u toegang hebt tot Azure machtigingen gebruikt, moet u de resource opgeven als onderdeel van de URL. Het is mogelijk om deze URL met PowerShell.

  Bijvoorbeeld `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  De URL wordt er als volgt uitzien:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`


### <a name="managing-users-in-the-oms-portal"></a>In de portal OMS-gebruikers beheren

Beheren van gebruikers en groepen op het tabblad **Gebruikers beheren** onder het tabblad **Accounts** in de pagina-instellingen. Er is, kunt u de taken uitvoeren in de volgende secties.  

![gebruikers beheren](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Een gebruiker toevoegen aan een bestaande werkruimte

Gebruik de volgende stappen uit om een gebruiker of groep toevoegen aan een werkruimte OMS. De gebruiker of groep is mogelijk te werken met alle waarschuwingen die gekoppeld aan deze werkruimte zijn.

>[AZURE.NOTE] Als u wilt dat een gebruiker of groep van uw organisatie-Azure Active Directory-account toevoegt, moet u eerst ervoor zorgen dat u uw account OMS hebt gekoppeld aan de Active Directory-domein. Zie [een Azure Active Directory-organisatie naar een bestaande werkruimte toevoegen](#add-an-azure-active-directory-organization-to-an-existing-workspace).

1. Klik op de tegel **Instellingen** in OMS.
2. Klik op het tabblad **Accounts** en klik vervolgens op het tabblad **Gebruikers beheren** .
3. Kies het accounttype toevoegen in de sectie **Gebruikers beheren** : **Organisatie-Account**, **Account van Microsoft**, **Microsoft Support**.
    - Als u Microsoft-Account, typ het e-mailadres van de gebruiker die is gekoppeld aan het Microsoft-Account.
    - Als u organisatie-Account, kunt u een deel van de gebruiker of van de groep of een e-mailalias invoeren en een lijst met gebruikers en groepen weergegeven. Selecteer een gebruiker of groep.
    - Microsoft Support gebruiken om een Microsoft Support engineering tijdelijk toegang krijgen tot de werkruimte om te helpen bij het oplossen van problemen.

    >[AZURE.NOTE] Beperk het aantal Active Directory-groepen die zijn gekoppeld aan één account OMS drie voor de beste prestatieresultaten, een voor beheerders, één voor medewerkers en een voor gebruikers van alleen-lezen. Meer groepen mogelijk van invloed op de prestaties van het logboek Analytics.

5. Kies het type van de gebruiker of groep toe te voegen: **beheerder**, **medewerker**of **Gebruiker alleen-lezen** .  
6. Klik op **toevoegen**.

  Als u een Microsoft-account toevoegt, wordt een uitnodiging voor deelname van de werkruimte verzonden naar het e-mailbericht dat u hebt opgegeven. De gebruiker kan de accountgegevens voor deze account OMS en waarschuwingen weergeven nadat de gebruiker volgt de aanwijzingen in de uitnodiging voor OMS, en is het mogelijk de om gebruikersinformatie te bekijken op het tabblad **Accounts** van de pagina **-Instellingen** .
  Als u een organisatie-account toevoegt, wordt de gebruiker logboek Analytics onmiddellijk toegang tot zijn.  
  ![uitnodiging via e-mail](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Een gebruikerstype van een bestaande bewerken

U kunt de functie account voor een gebruiker die is gekoppeld aan uw account OMS wijzigen. U hebt de volgende opties voor de functie:

 - *Beheerder*: kunt gebruikers beheren, bekijken en reageren op alle meldingen en toevoegen en verwijderen van servers

 - *Inzender*: kunt bekijken en reageren op alle meldingen en toevoegen en verwijderen van servers

 - *Alleen-lezen gebruiker*: gebruikers alleen-lezen niet mogelijk voor:
   1. Software oplossingen. De galerie van oplossingen is verborgen.
   2. Toevoegen/wijzigen/verwijderen tegels op **Mijn Dashboard**.
   3. De **Instellingen voor** pagina's bekijken. De pagina's worden verborgen.
   4. Taken worden verborgen in de zoekopdracht weergeven, PowerBI configuratie opgeslagen zoekacties en waarschuwingen.


#### <a name="to-edit-an-account"></a>Een account bewerken

1. Klik op de tegel **Instellingen** in OMS.
2. Klik op het tabblad **Accounts** en klik vervolgens op het tabblad **Gebruikers beheren** .
3. Selecteer de rol voor de gebruiker die u wilt wijzigen.
2. Klik op **Ja**in het bevestigingsvenster.

### <a name="remove-a-user-from-a-oms-workspace"></a>Een gebruiker verwijderen uit een werkruimte OMS

Gebruik de volgende stappen uit om een gebruiker verwijderen uit een werkruimte OMS. Houd er rekening mee dat dit niet de werkruimte van de gebruiker wordt gesloten. In plaats daarvan wordt de koppeling tussen de gebruiker en de werkruimte verwijderd. Als een gebruiker gekoppeld aan meerdere werkruimten is, kan die gebruiker nog steeds aanmelden bij OMS en de andere werkruimten bekijken.

1. Klik op de tegel **Instellingen** in OMS.
2. Klik op het tabblad **Accounts** en klik vervolgens op het tabblad **Gebruikers beheren** .
3. Klik op **verwijderen** naast de naam van de gebruiker die u wilt verwijderen.
4. Klik op **Ja**in het bevestigingsvenster.


### <a name="add-a-group-to-an-existing-workspace"></a>Een groep toevoegen aan een bestaande werkruimte

1.  Voer stap 1 -4 in "Om een gebruiker toevoegen aan een bestaande werkruimte", hierboven.
2.  Selecteer onder **Kies gebruiker/groep** **groep**.
    ![een groep toevoegen aan een bestaande werkruimte](./media/log-analytics-manage-access/add-group.png)
3.  Voer de weergavenaam of e-mailadres voor de groep wilt toevoegen.
4.  Selecteer de groep in de lijst met zoekresultaten en klik vervolgens op **toevoegen**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Een bestaande werkruimte koppelen aan een Azure-abonnement

Het is mogelijk om een werkruimte maken via de website [microsoft.com/oms](https://microsoft.com/oms) .  Echter, bepaalde grenzen bestaan voor deze werkruimten, de vermaarde een limiet van 500MB per dag van het uploaden van gegevens als u een gratis account. U wilt *uw bestaande werkruimte voor een Azure abonnement koppelen*om deze werkruimte te wijzigen.

>[AZURE.IMPORTANT] Om een werkruimte wilt koppelen, moet uw account Azure al toegang hebben tot de werkruimte die u wilt koppelen.  Met andere woorden, moeten de account die u gebruikt voor toegang tot de portal Azure **hetzelfde** als de account die u gebruikt voor toegang tot uw werkruimte OMS. Als dit niet het geval is, ziet u [een gebruiker toevoegen aan een bestaande werkruimte](#add-a-user-to-an-existing-workspace).

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Een werkruimte aan een Azure-abonnement in de OMS portal koppelen

De aangemelde gebruiker moet een werkruimte om aan te koppelen in de portal OMS een Azure-abonnement, reeds een betaalde account Azure. De werkruimte die u actief gebruikt wordt gekoppeld aan de Azure account.

1. Klik op de tegel **Instellingen** in OMS.
2. Klik op het tabblad **Accounts** en klik vervolgens op het tabblad **Azure abonnement en gegevens plannen** .
3. Klik op de data-abonnement dat u gebruiken wilt.
4. Klik op **Opslaan**.  
  ![abonnement en gegevens plannen](./media/log-analytics-manage-access/subscription-tab.png)

De nieuwe indeling van de gegevens wordt in het Kantoorbeheersysteem portal lint boven aan de webpagina weergegeven.

![OMS-lint](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Een werkruimte aan een Azure-abonnement in Azure portal koppelen

1.  Inloggen op de [portal Azure](http://portal.azure.com).
2.  Ga voor **Logboek Analytics (OMS)** en selecteer deze vervolgens.
3.  Hier ziet u de lijst met bestaande werkruimten. Klik op **toevoegen**.  
    ![lijst met werkruimten](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4.  Klik onder **OMS werkruimte**, **of een bestaande koppeling**.  
    ![koppeling met bestaande](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5.  Klik op **configureren, instellingen vereist**.  
    ![vereiste instellingen configureren](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6.  Hier ziet u de lijst met werkruimten die nog niet zijn gekoppeld aan uw account Azure. Selecteer een werkruimte.  
    ![werkruimten selecteren](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7.  Indien nodig kunt u de waarden voor de volgende items wijzigen:
    - Abonnement
    - Resourcegroep
    - Locatie
    - Prijzen laag  
        ![waarden wijzigen](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8.  Klik op **maken**. De werkruimte is nu gekoppeld aan uw account Azure.

>[AZURE.NOTE] Als u de werkruimte die u wilt koppelen niet ziet, heeft vervolgens uw Azure abonnement geen toegang tot de OMS werkruimte die u hebt gemaakt met behulp van de website OMS.  U moet toegang tot deze account uit in uw gebruik van de website OMS OMS-werkruimte. Zie [een gebruiker toevoegen aan een bestaande werkruimte](#add-a-user-to-an-existing-workspace)doen.



## <a name="upgrade-a-workspace-to-a-paid-data-plan"></a>Een werkruimte voor een upgrade naar een betaalde data-abonnement

Er zijn drie werkruimtegegevens typen plannen voor OMS: **vrij**, **Standard**en **Premium**.  Als u op een *vrije* plan, hebt u mogelijk uw gegevens cap van 500 MB geraakt.  U moet uw werkruimte bijwerken op een ***"pay-as-you-go" plan*** voor het verzamelen van gegevens buiten deze limiet. U kunt op elk gewenst moment uw plantype converteren.  Zie voor meer informatie over het Kantoorbeheersysteem prijzen, [Prijzen Details](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

>[AZURE.IMPORTANT] Plannen van de werkruimte kunnen alleen worden gewijzigd als ze *gekoppeld* aan een abonnement Azure zijn.  Als u uw werkruimte in Azure hebt gemaakt of als u hebt *al* uw werkruimte gekoppeld, kunt u dit bericht negeren.  Als u de werkruimte met de [OMS-website](http://www.microsoft.com/oms)hebt gemaakt, moet u de stappen op de [koppeling van een bestaande werkruimte voor een abonnement met Azure](#link-an-existing-workspace-to-an-azure-subscription).

### <a name="using-entitlements-from-the-oms-add-on-for-system-center"></a>Met behulp van toeslagrechten uit de OMS-invoegtoepassing voor System Center

De invoegtoepassing OMS voor System Center biedt een recht voor het speciale niveau van OMS logboek Analytics, aan de [Prijzen van OMS](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx)beschreven.

Bij aankoop van de invoegtoepassing OMS voor System Center, wordt de invoegtoepassing OMS toegevoegd als een recht voor de overeenkomst voor System Center. Een abonnement voor Azure die krachtens deze overeenkomst wordt gemaakt, kunt u het gebruik van het recht. Zo kunt u bijvoorbeeld meerdere OMS werkruimten die gebruikmaken van het recht van de invoegtoepassing OMS hebben.

Om ervoor te zorgen dat gebruik van een werkruimte OMS wordt toegepast op de rechten uit de OMS-invoegtoepassing, moet u naar:

1. Uw werkruimte OMS koppelen aan een Azure-abonnement dat deel uitmaakt van de Enterprise Agreement met het Kantoorbeheersysteem invoegtoepassing inkoop en de Azure abonnement gebruik
2. Selecteer het plan van de premie voor de werkruimte

Als u het gebruik in de portal Azure of OMS bekijkt, ziet u niet de rechten van de invoegtoepassing OMS. U ziet hier echter toeslagrechten in Enterprise Portal.  

Als u wijzigen de Azure abonnement die aan uw Kantoorbeheersysteem werkruimte is gekoppeld wilt, kunt u de cmdlet Azure PowerShell [Verplaatsen AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) .

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Met behulp van Azure betrokkenheid van een Enterprise Agreement

Als u zelfstandige prijzen voor OMS-onderdelen gebruikt, u betaalt voor elke component van OMS afzonderlijk en het gebruik op uw Azure rekening weergegeven.

Hebt u een Azure monetaire vastleggen voor de inschrijving van de onderneming waaraan de Azure abonnementen zijn gekoppeld, worden automatisch gebruik van Log Analytics debet tegen eventuele resterende monetaire doorvoeren.

Als u wilt wijzigen de Azure abonnement dat de OMS-werkruimte is gekoppeld aan u de cmdlet Azure PowerShell [Verplaatsen AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) gebruikt.  



### <a name="to-change-a-workspace-to-a-paid-data-plan"></a>Een werkruimte wijzigen in een betaalde data-abonnement

1.  Inloggen op de [portal Azure](http://portal.azure.com).
2.  Ga voor **Logboek Analytics (OMS)** en selecteer deze vervolgens.
3.  Hier ziet u de lijst met bestaande werkruimten. Selecteer een werkruimte.  
    ![lijst met werkruimten](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4.  Klik onder **Instellingen**op **prijzen laag**.  
    ![prijzen laag](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5.  Onder de **prijzen laag**, selecteert u een data-abonnement en klik op **selecteren**.  
    ![Selecteer plan](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6.  Als u de weergave in de portal Azure vernieuwt, ziet u **prijzen laag** voor het geselecteerde schema wordt bijgewerkt.  
    ![Update prijs laag](./media/log-analytics-manage-access/manage-access-change-plan04.png)

U kunt nu gegevens na de initiaal 'gratis' gegevens verzamelen.


## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Een organisatie van Azure Active Directory toevoegen aan een bestaande werkruimte

U kunt uw werkruimte logboek Analytics (OMS) koppelen aan een Azure Active Directory-domein. Hiermee kunt u gebruikers vanuit Active Directory rechtstreeks toevoegen aan uw werkruimte OMS zonder een afzonderlijke Microsoft-account.

Wanneer u de werkruimte vanaf de portal Azure maken of uw werkruimte aan een Azure-abonnement koppelen wordt uw Azure Active Directory als uw organisatie-account gekoppeld.

Wanneer u de werkruimte vanaf de portal OMS maken wordt u gevraagd om te koppelen aan een Azure-abonnement en een organisatie-account.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Een organisatie van Azure Active Directory toevoegen aan een bestaande werkruimte

1. Op de pagina instellingen in OMS op **Accounts** en klik vervolgens op het tabblad **Gegevens van de werkruimte** .  
2. Lees de informatie over de organisatie en klik vervolgens op **Organisatie toevoegen**.  
    ![organisatie toevoegen](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Voer de identiteitsgegevens voor de beheerder van uw Azure Active Directory-domein. Hierna ziet u een bevestigingsbericht waarin wordt verklaard dat de werkruimte is gekoppeld aan uw Azure Active Directory-domein.
    ![bevestiging van de gekoppelde werkruimte](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

>[AZURE.NOTE] Zodra uw account is gekoppeld aan een organisatie-Account, kan niet koppeling worden verwijderd of gewijzigd.

## <a name="close-your-oms-workspace"></a>Sluit uw werkruimte OMS

Wanneer u een werkruimte OMS sluit, wordt alle gegevens met betrekking tot uw werkruimte verwijderd uit de OMS-service binnen 30 dagen na het afsluiten van de werkruimte.

Als u beheerder bent en er meerdere gebruikers die aan de werkruimte is gekoppeld zijn, wordt de koppeling tussen de gebruikers en de werkruimte is verbroken. Als de gebruikers gekoppeld aan andere werkruimten zijn, kunnen ze vervolgens blijven gebruiken OMS met die andere werkruimten. Echter, als ze niet gekoppeld aan andere werkruimten zijn ze moet voor het maken van een nieuwe werkruimte wilt gebruiken OMS.

### <a name="to-close-an-oms-workspace"></a>Een werkruimte OMS sluiten

1. Klik op de tegel **Instellingen** in OMS.
2. Klik op het tabblad **Accounts** en klik vervolgens op het tabblad **Gegevens van de werkruimte** .
3. Klik op **sluiten werkruimte**.
4. Selecteer een van de redenen voor het afsluiten van uw werkruimte of een andere reden invoeren in het tekstvak.
5. Klik op **sluiten werkruimte**.

## <a name="next-steps"></a>Volgende stappen

- Zie [verbinding maken met Windows-computers aanmelden Analytics](log-analytics-windows-agents.md) agents toevoegen en gegevens verzamelen.
- [Oplossingen voor logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md) voor functionaliteit toevoegen en gegevens te verzamelen.
- [Proxy- en firewall instellingen configureren in logboek Analytics](log-analytics-proxy-firewall.md) als uw organisatie gebruikmaakt van een proxyserver of firewall zodat agenten met de logboek-Analytics-service communiceren kunnen.
