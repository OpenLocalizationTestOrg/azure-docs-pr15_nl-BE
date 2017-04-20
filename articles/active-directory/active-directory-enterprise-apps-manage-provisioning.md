<properties
    pageTitle="Beheer van enterprise apps in de voorvertoning Azure Active Directory van gebruikers | Microsoft Azure"
    description="Informatie over het account gebruikersaanvragen voor enterprise apps met behulp van de voorvertoning Azure Active Directory beheren"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/12/2016"
    ms.author="asmalser"/>

#<a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>Voorbeeld: Beheer van gebruikersaccount voor enterprise apps in de nieuwe portal Azure ingericht

In dit artikel wordt beschreven hoe de [Azure portal](https://portal.azure.com) gebruiken voor het beheer van gebruikersaccount automatisch inrichten en verwijderen van gegevens voor toepassingen die worden ondersteund, met name die uit de categorie "aanbevolen" van de [galerie met Azure Active Directory-toepassing](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)toegevoegd. Dit management ervaring in de nieuwe portal Azure is momenteel in openbare preview en in dit artikel beschrijft de nieuwe functies, alsmede een aantal tijdelijke beperkingen die tijdens de proefperiode in plaats. [Wat is in het voorbeeld?](active-directory-preview-explainer.md)

Zie voor meer informatie over automatische account gebruikersaanvragen en hoe het werkt, [Gebruikersaanvragen automatiseren en Deprovisioning voor SaaS-toepassingen met Azure Active Directory](active-directory-saas-app-provisioning.md).

##<a name="finding-your-apps-in-the-new-portal"></a>Zoeken naar uw toepassingen in de nieuwe portal

Vanaf September 2016, kunnen alle toepassingen die zijn geconfigureerd voor één teken op in een map, door een beheerder van een directory met behulp van de [galerie met Azure Active Directory-toepassing](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) binnen de [Azure klassieke portal](https://manage.windowsazure.com), nu worden bekeken en beheerd in de nieuwe portal Azure.

Deze toepassingen vindt u in de sectie **Zakelijke toepassingen** van de nieuwe Azure portal, die toegankelijk zijn via het menu **Meer diensten** op het gebied van de navigatiebalk aan de linkerkant. Enterprise apps zijn apps die zijn geïmplementeerd en worden gebruikt door gebruikers binnen uw organisatie.

![Bedrijfstoepassingen blade][0]

De **alle toepassingen** te klikken aan de linkerkant bevat een overzicht van alle toepassingen die zijn geconfigureerd, met inbegrip van toepassingen die vanuit de galerie is toegevoegd. Selecteren van een app laadt het blad van de resource voor die toepassing, waar rapporten kunnen worden weergegeven voor die toepassing en verschillende instellingen kan worden beheerd.

Instellingen voor het inrichten gebruikersaccount kan worden beheerd door **Provisioning** selecteren aan de linkerkant.

![Application resource blade][1]


##<a name="provisioning-modes"></a>Modi inrichten

De blade **Provisioning** begint met een menu **modus** , waarin wordt aangegeven welke inrichten voor een bedrijfstoepassing worden ondersteund, en kan ze worden geconfigureerd. De volgende opties zijn beschikbaar:

* **Automatische** - deze optie wordt weergegeven als Azure AD ondersteunt automatisch inrichten op basis van een API en/of verwijderen van gegevens van gebruikersaccounts op deze toepassing. Selecteren van deze modus wordt een interface dat netwerkbeheerders Azure AD verbinding maken met de toepassing van de gebruiker API voor beheer configureren, Accounttoewijzingen en werkstromen die definiëren hoe de gegevens van gebruikersaccounts moeten stromen tussen Azure AD maken en de toepassing en het beheer van de Azure AD service ingericht.

* **Handleiding** - deze optie wordt weergegeven als AD Azure biedt geen ondersteuning voor automatische inrichting van gebruikersaccounts op deze toepassing. Deze optie houdt in dat gebruiker accountrecords opgeslagen in de toepassing moeten worden beheerd met behulp van een extern proces, op basis van de gebruiker beheer en toewijzing van functies in die toepassing (zoals JIT SAML-aanbod).


##<a name="configuring-automatic-user-account-provisioning"></a>Automatische account gebruikersaanvragen configureren

Als u de **Automatische** optie verschijnt een scherm dat is onderverdeeld in vier secties:

###<a name="admin-credentials"></a>Admin-referenties
Dit is waar de referenties vereist voor Azure AD verbinding maken met de toepassing van de gebruiker management API worden ingevoerd. De invoer is vereist, varieert afhankelijk van de toepassing. Meer informatie over de vereisten voor specifieke toepassingen en referenties, Zie de [zelfstudie voor de configuratie voor de specifieke toepassing](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

De knop **Verbinding testen** kunt u de referenties testen door Azure AD poging tot verbinding met de app de app met de opgegeven referenties wordt ingericht.

###<a name="mappings"></a>Toewijzingen
Dit is waar beheerders kunt weergeven en bewerken van welke gebruiker kenmerken stroom tussen AD Azure en de doeltoepassing wanneer gebruikersaccounts worden ingericht of bijgewerkt.

Er is een verzameling vooraf geconfigureerde toewijzingen tussen AD Azure user-objecten en elke SaaS-app van gebruikersobjecten. Sommige apps beheren andere typen objecten, zoals groepen of contactpersonen. Als u een van deze toewijzingen in de tabel ziet u de editor voor toewijzing aan de rechterkant, waar ze kunnen worden bekeken en aangepast.

![Application resource blade][2]

Ondersteunde aanpassingen tijdens het eerste voorbeeld zijn:

* In- en uitschakelen van de toewijzingen voor specifieke objecten, zoals het object AD Azure user aan het gebruikersobject de SaaS-app.

* Bewerken welke kenmerken naar het gebruikersobject van de app vloeien uit het gebruikersobject Azure AD. Zie voor meer informatie over de toewijzing van kenmerken, [Wat toewijzing kenmerktypen](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).

* Welke provisioning filteracties Azure AD op de doeltoepassing uitvoeren moet, een nieuwe functie in de portal Azure is. In plaats van Azure AD volledig synchroniseren objecten, kunt u de acties beperken. Door alleen te selecteren **Update**alleen updates Azure AD bestaande gebruikers-accounts in een toepassing en maakt geen nieuwe. Als u alleen **maken**, Azure alleen nieuwe gebruikersaccounts gemaakt maar werkt geen bestaande. Met deze functie kunnen beheerders voor de verschillende toewijzingen voor het maken van accounts maken en bijwerken van werkstromen. De volledige mogelijkheid om meerdere toewijzingen per app is gepland voor later in de proefperiode.

###<a name="settings"></a>Instellingen
In deze sectie kan beheerders om te starten en stoppen de Azure advertentie-service voor de geselecteerde toepassing inrichten, alsmede eventueel provisioning cache wissen en start de service opnieuw.

Als ingericht is voor het eerst voor een toepassing wordt ingeschakeld, schakelt de service door het wijzigen van de **Status-aanbod** **aan**. Dit zorgt ervoor dat de Azure AD service ingericht voor het uitvoeren van een initiële synchronisatie waarbij de toegewezen in de sectie **gebruikers en groepen** gebruikers worden gelezen, de doeltoepassing voor deze query's en voert de provisioning gedefinieerde acties in de sectie Azure AD **toewijzingen** . Tijdens dit proces het inrichtingsproces worden opgeslagen in cache opgeslagen gegevens over welke gebruikersaccounts beheert, zodat niet-beheerde accounts binnen de doeltoepassingen die nooit in de scope voor de toewijzing niet worden beïnvloed door het verwijderen van gegevens van bewerkingen. Na de eerste synchronisatie synchroniseert de provisioning service automatisch gebruikers- en groepsobjecten op een interval van tien minuten.

Wijzigen van de **Status wordt ingericht** op **uit** onderbreken de provisioning service te. In deze toestand Azure niet maken, bijwerken of verwijderen van een gebruiker of groepsobjecten in de toepassing. Wijzigt de status naar op zorgt ervoor dat de service om op te halen waar deze gebleven was.

De **huidige staat en start opnieuw synchronisatie** checkbox selecteren en opslaan stopt de provisioning service dumpen gegevens over Azure AD-welke accounts in de cache is het beheren van de services opnieuw worden gestart en de initiële synchronisatie opnieuw wordt uitgevoerd. Met deze optie kan beheerders het inrichtingsproces implementatie opnieuw starten.

###<a name="synchronization-details"></a>Synchronisatiedetails
Deze sectie bevat informatie over de werking van de provisioning service, met inbegrip van de eerste en laatste keer dat de service provisioning uitgevoerd tegen de toepassing en het aantal gebruikers- en groepsobjecten worden beheerd toevoeging.

Koppelingen worden verstrekt op het **activiteitenverslag Provisioning**, die bevat een logboek van alle gebruikers en groepen gemaakt, bijgewerkt en verwijderd tussen Azure AD en het doel van toepassing, en aan het **foutenrapport Provisioning** meer biedt gedetailleerde foutberichten voor gebruikers- en groepsobjecten die niet worden gelezen hebt gemaakt konden, bijgewerkt of verwijderd. 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
