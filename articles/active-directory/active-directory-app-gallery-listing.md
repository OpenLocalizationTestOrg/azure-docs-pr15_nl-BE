<properties
   pageTitle="Uw vermelding in de galerie met Azure Active Directory-toepassing"
   description="Hoe u een toepassing die eenmalige aanmelding in de galerie met Azure Active Directory ondersteunt | Microsoft Azure"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>


# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Uw vermelding in de galerie met Azure Active Directory-toepassing

Als u een toepassing die in de [galerie met Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/)single sign-on met Azure Active Directory ondersteunt, moet de toepassing eerst te implementeren op een van de volgende modi voor integratie:

* **OpenID verbinding** - directe integratie met Azure AD OpenID verbinding gebruiken voor verificatie en de API Azure AD toestemming voor configuratie. Als u een integratie net beginnen en SAML biedt geen ondersteuning voor uw toepassing, is dit de aanbevolen modus.

* **SAML** – uw toepassing al heeft de mogelijkheid om de van derden identiteitsproviders via de SAML-protocol configureren.

Vereisten voor vermelding voor elke modus staan hieronder.

##<a name="openid-connect-integration"></a>OpenID verbinding integratie

Uw toepassing integreren met Azure AD, volgens de [instructies van de ontwikkelaar](active-directory-authentication-scenarios.md). Vervolgens Vul de onderstaande vragen en verzenden naar waadpartners@microsoft.com.

* Referenties voor een account of huurder test met de toepassing die door het team Azure AD kan worden gebruikt voor het testen van de integratie.  

* Instructies over hoe het team Azure AD kunt melden en een instantie van Azure AD verbinden met uw toepassing met behulp van de [Azure AD framework toestemming](active-directory-integrating-applications.md#overview-of-the-consent-framework)geven. 

* Eventuele verdere instructies nodig voor het team Azure AD eenmalige aanmelding met uw toepassing testen bieden. 

* De volgende informatie bevatten:

> Bedrijfsnaam:
> 
> Website bedrijf:
> 
> Naam:
> 
> Beschrijving van de toepassing (maximaal 256 tekens):
> 
> Toepassing-Website (informatie):
> 
> Toepassing-Website voor technische ondersteuning of contactgegevens:
> 
> De client-ID van de toepassing, zoals wordt weergegeven in de Toepassingdetails op https://manage.windowsazure.com:
> 
> Aanmelden bij de URL waar klanten gaan voor het aanmelden bij en/of kopen de toepassing:
> 
> Kies drie categorieën voor uw toepassing te worden vermeld onder (voor de beschikbare categorieën Zie de Azure Active Directory-markt):
> 
> Kleine toepassingspictogram (PNG-bestand, 45px door 45px, de effen achtergrondkleur) bijvoegen:
> 
> Grote toepassingspictogram (PNG-bestand, 215px door 215px, de effen achtergrondkleur) bijvoegen:
> 
> Aanvraag Logo (PNG-bestand, 150px door 122px, doorzichtige achtergrondkleur) bijvoegen:

##<a name="saml-integration"></a>SAML-integratie

Elke toepassing die SAML 2.0 ondersteunt, kan rechtstreeks met een Azure AD huurder [deze](active-directory-saas-custom-apps.md)instructies voor het toevoegen van een aangepaste toepassing worden geïntegreerd. Zodra u hebt getest of de integratie van toepassingen met Azure Active Directory werkt, verzendt de volgende informatie naar <waadpartners@microsoft.com>.

* Referenties voor een account of huurder test met de toepassing die door het team Azure AD kan worden gebruikt voor het testen van de integratie.  

* Bevatten de SAML-On URL, URL van de uitgevende instelling (entiteit-ID) en antwoord URL (bevestiging consument service)-waarden voor uw toepassing, als beschreven [hier](active-directory-saas-custom-apps.md). Als u deze waarden meestal als onderdeel van een bestand met metagegevens SAML opgeeft, vervolgens Stuur dat ook.

* Geef een korte beschrijving van het configureren van AD Azure als een identiteitsprovider in uw toepassing met behulp van SAML 2.0. Als uw toepassing configureren Azure AD als een id-provider via een administratieve selfserviceportal ondersteunt, vervolgens Zorg ervoor dat de hierboven opgegeven referenties zijn de mogelijkheid om in te stellen dit.

* De volgende informatie bevatten:

> Bedrijfsnaam:
> 
> Website bedrijf:
> 
> Naam:
> 
> Beschrijving van de toepassing (maximaal 256 tekens):
> 
> Toepassing-Website (informatie):
> 
> Toepassing-Website voor technische ondersteuning of contactgegevens:
> 
> Aanmelden bij de URL waar klanten gaan voor het aanmelden bij en/of kopen de toepassing:
> 
> Kies drie categorieën voor uw toepassing te worden vermeld onder (voor de beschikbare categorieën Zie de [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):
> 
> Kleine toepassingspictogram (PNG-bestand, 45px door 45px, de effen achtergrondkleur) bijvoegen:
> 
> Grote toepassingspictogram (PNG-bestand, 215px door 215px, de effen achtergrondkleur) bijvoegen:
> 
> Aanvraag Logo (PNG-bestand, 150px door 122px, doorzichtige achtergrondkleur) bijvoegen:
