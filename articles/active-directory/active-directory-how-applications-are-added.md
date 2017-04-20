<properties
   pageTitle="Hoe toepassingen naar Azure Active Directory worden toegevoegd."
   description="In dit artikel wordt beschreven hoe toepassingen worden toegevoegd aan een exemplaar van Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="shoatman"
   manager="kbrint"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="shoatman"/>

# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Hoe en waarom toepassingen worden toegevoegd aan AD Azure

Een van de in eerste instantie puzzling dingen bij het bekijken van een lijst met toepassingen in uw exemplaar van Azure Active Directory wordt informatie over waar de toepassingen vandaan komt en waarom zijn er.  Dit artikel biedt een hoog niveau overzicht van hoe toepassingen worden weergegeven in de map en u voorzien van context die u helpt bij het begrijpen hoe een toepassing zijn geworden in de map.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Welke services biedt Azure AD toepassingen?

Toepassingen worden toegevoegd aan de Azure AD gebruikmaken van een of meer van de services die biedt.  Deze diensten omvatten:

* App-verificatie en autorisatie
* Verificatie en autorisatie
* Eenmalige aanmelding (SSO) met behulp van federation of wachtwoord
* Gebruikersaanvragen & synchronisatie
* Op rollen gebaseerde toegangscontrole; Gebruik de map voor het rollen van de toepassing voor het uitvoeren van rollen definiëren op basis van toestemming controles in een app.
* oAuth machtigingsservices (gebruikt door Office 365 en andere Microsoft-toepassingen toegang tot API's / bronnen toe te staan.)
* Toepassing publiceren & proxy; Een app vanaf een particulier netwerk met het internet publiceren

## <a name="how-are-applications-represented-in-the-directory"></a>Hoe worden de toepassingen weergegeven in de map?

Toepassingen worden weergegeven in de Azure AD 2 objecten: een application-object en een service principal-object.  Er wordt een application-object, geregistreerd in een "thuis" / "eigenaar" of "publiceren" directory en een of meer service principal-objecten die de toepassing in elke map waarin het fungeert.  

Beschrijving van het application-object de app naar Azure AD (de huurder met meerdere service) en een van de volgende kan inhouden: (*Opmerking*: dit is geen uitputtende lijst.)

* Naam, Logo en Publisher
* Geheimen (symmetrische en/of asymmetrische sleutels gebruikt voor het verifiëren van de app)
* API-afhankelijkheden (oAuth)
* API's / resources/scopes gepubliceerd (oAuth)
* App rollen RBAC)
* SSO-metagegevens en configuratie (SSO)
* Metagegevens en de configuratie van gebruikers
* Configuratie- en proxy-metagegevens

De service principal is een record van de toepassing in elke map, waar de toepassing fungeert met inbegrip van de basismap.  De service principal:

* Verwijst naar een application-object via de eigenschap app-id
* Records lokale gebruikers- en groepstoewijzingen app-rol
* Lokale gebruiker en admin machtigingen records verleend App
    * Bijvoorbeeld: de machtiging voor een e-mailadres van bepaalde gebruikers toegang tot de app.
* Records lokaal beleid, met inbegrip van voorwaardelijke-beleid
* Records lokaal alternatieve lokale instellingen voor een app
    * Claims transformatie regels
    * Kenmerktoewijzingen (gebruikersaanvragen)
    * Pachters app voor specifieke rollen (als de app aangepaste rollen ondersteunt)
    * Naam/Logo

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Een diagram van toepassingsobjecten en service beveiligings-principals in mappen

![Een schematische weergave van hoe de toepassing objecten en beveiligings-principals met exemplaren van AD Azure bestaande service.][apps_service_principals_directory]

Zoals u in het bovenstaande diagram ziet.  Microsoft onderhoudt twee mappen (aan de linkerkant) intern wordt gebruikt voor het uitgeven van toepassingen.

* Voor Microsoft-Apps (Microsoft services directory)
* Voor vooraf geïntegreerde 3de partij Apps (galerie App directory)

Uitgevers/leveranciers van toepassingen die met AD Azure integreren moeten over een publicatiemap.  (Sommige SAAS Directory).

Er zijn toepassingen die u zelf toevoegen:

* Apps die u hebt ontwikkeld (geïntegreerd met AAD)
* Apps u verbonden voor eenmalige aanmelding
* Apps die u met behulp van de toepassingsproxy Azure AD gepubliceerd.

### <a name="a-couple-of-notes-and-exceptions"></a>Een aantal notities en uitzonderingen

* Niet alle service-principals verwijzen naar toepassingsobjecten.  Huh? Azure AD is oorspronkelijk gebouwd de diensten aan de toepassingen zijn veel meer beperkt als de service principal is voldoende voor de vaststelling van de identiteit van een app.  De oorspronkelijke service principal was dichter in de vorm bij de Windows Server Active Directory-account.  Om deze reden's het nog steeds mogelijk te maken van beveiligings-principals service met behulp van het AD Azure PowerShell zonder eerst te maken een application-object.  Een object app vereist de Graph API voordat u een service principal.
* Niet alle van de hierboven beschreven gegevens via programmacode momenteel zichtbaar is.  De volgende opties zijn alleen beschikbaar in de gebruikersinterface:
    * Claims transformatie regels
    * Kenmerktoewijzingen (gebruikersaanvragen)
* Voor meer gedetailleerde informatie over de service principal en application-objecten raadpleegt u de documentatie van Azure AD Graph REST API reference.  *Tip*: de Azure AD Graph API-documentatie is het dichtst dat schema verwijst voor Azure advertentie die momenteel beschikbaar is.  
    * [Toepassing](https://msdn.microsoft.com/library/azure/dn151677.aspx)
    * [Service Principal](https://msdn.microsoft.com/library/azure/dn194452.aspx)


## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Hoe apps toegevoegd aan mijn exemplaar Azure AD?
Er zijn veel manieren die een app kan worden toegevoegd aan AD Azure:

* Een app toevoegen uit de [Galerie met Azure Active Directory App](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Teken omhoog/naar een 3de partij App geïntegreerd met Azure Active Directory (bijvoorbeeld: [Smartsheet](https://app.smartsheet.com/b/home) of [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
    * Tijdens de aanmelding omhoog/gebruikers gevraagd om machtiging App voor toegang tot hun profiel en andere machtigingen te geven.  De eerste persoon toestemming geven zorgt ervoor dat een service principal de toepassing die moet worden toegevoegd aan de map.
* Omhoog/naar Microsoft online services, zoals [Office 365](http://products.office.com/) ondertekenen
    * Wanneer u zich abonneren op Office 365 of beginnen met een evaluatieversie van een of meer service beveiligings-principals worden gemaakt in de map die de verschillende services die worden gebruikt voor het leveren van alle van de functionaliteit van Office 365.
    * Service-principals maken sommige services, Office 365 zoals SharePoint op basis van voortdurende veilige communicatie tussen componenten, met inbegrip van werkstromen toestaan.
* Een toepassing ontwikkelen in de Azure Management Portal zien toevoegen: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Voeg een app ontwikkelt met behulp van Visual Studio Zie:
    * [ASP.Net-verificatiemethoden](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
    * [Connected Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Een app met behulp van de [AD-toepassingsproxy Azure](https://msdn.microsoft.com/library/azure/dn768219.aspx) toevoegen
* Verbinding maken met een app voor eenmalige aanmelding met SAML of een SSO-wachtwoord
* Vele anderen met inbegrip van ervaringen met verschillende ontwikkelaars in Azure en/in explorer API ervaringen over developer centers

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Wie heeft de machtiging voor het toevoegen van toepassingen aan mijn exemplaar Azure AD?

Alleen globale beheerders de volgende mogelijkheden:

* Apps toevoegen uit de galerie met Azure AD app (vooraf geïntegreerde 3de partij Apps)
* Een app met de Azure AD toepassingsproxy publiceren

Alle gebruikers in de directory hebben op toepassingen die ze ontwikkelen en naar keuze via welke toepassingen zij delen/geven toegang tot hun bedrijfsgegevens toevoegen.  *Denk eraan dat gebruiker teken omhoog in een app en machtigingen kan ertoe leiden dat een service principal wordt gemaakt.*

Dit kan in eerste instantie goed betreffende, maar houden het volgende in gedachten:

* Apps zijn Windows Server Active Directory gebruikmaken voor de verificatie voor vele jaren zonder de toepassing geregistreerd geregistreerd in Active directory.  De organisatie zal hebben Verbeterde zichtbaarheid precies hoeveel apps directory en what voor zijn gebruikt.
* Geen behoefte aan admin app publiceren/registratieproces gereden.  Met Active Directory Federation Services is het waarschijnlijk dat een beheerder had een app toevoegen als een relying party voor ontwikkelaars.  Ontwikkelaars kunnen nu self-service.
* Gebruikers melden in/tot toepassingen met behulp van hun rekeningen organisatie voor zakelijke doeleinden is goed.  Als ze vervolgens laat u de organisatie dat ze geen toegang meer tot hun account in de toepassing was.
* Dat wordt vastgelegd welke gegevens u met die toepassing is dat goed werd gedeeld.  Gegevens is meer dan ooit vervoerbare en duidelijk wordt vastgelegd wie welke gegevens met toepassingen delen die nuttig is.
* Apps die met Azure AD oAuth besluiten precies welke machtigingen die gebruikers kunnen toewijzen aan toepassingen en die machtigingen moet akkoord gaan met een beheerder om hulp.  Deze vanzelf dat alleen beheerders kunnen instemming met groter bereik en meer belangrijke machtigingen.
* Gebruikers toevoegen en waarmee toepassingen toegang krijgen tot hun gegevens zijn gecontroleerd gebeurtenissen zodat u de auditverslagen binnen de portal Azure Management om te bepalen hoe een app is toegevoegd aan de map kunt bekijken.

**Opmerking:** *Microsoft zelf zijn werking met behulp van de standaardconfiguratie voor vele maanden nu.*

Die zei is het mogelijk om te voorkomen dat gebruikers in de map toepassingen toe te voegen en uit te oefenen naar keuze over welke gegevens ze delen met toepassingen door aanpassing van de mapconfiguratie in de portal Management Azure.  De volgende configuratie kan worden geopend binnen de Azure Management portal op van de map 'Configureren' tabblad.

![Een schermafbeelding van de gebruikersinterface voor het configureren van instellingen voor geïntegreerde app][app_settings]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Meer informatie over het toevoegen van Azure AD-toepassingen en het configureren van toepassingen.

* Ontwikkelaars: [informatie over het integreren van een toepassing met AAD](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Ontwikkelaars: [revisie voorbeeldcode voor apps geïntegreerd met Azure Active Directory op Github](https://github.com/AzureADSamples)
* Ontwikkelaars en IT-professionals: [de REST API-documentatie voor de Azure Active Directory Graph API controleren](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* IT-professionals: [informatie over het gebruik van vooraf geïntegreerde Azure Active Directory-toepassingen uit de galerie App](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* IT-professionals: [zelfstudies voor het configureren van specifieke vooraf geïntegreerde apps](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* IT-professionals: [informatie over het publiceren van een app met de toepassingsproxy Azure Active Directory](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Zie ook

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
