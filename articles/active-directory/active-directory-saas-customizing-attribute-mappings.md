<properties
    pageTitle="Kenmerktoewijzingen aanpassen | Microsoft Azure"
    description="Informatie over welke kenmerktoewijzingen voor SaaS-toepassingen in Azure Active Directory hoe kunt u ze aanpassen om de behoeften van uw bedrijf."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="customizing-attribute-mappings"></a>Kenmerktoewijzingen aanpassen


Microsoft Azure AD biedt ondersteuning voor gebruikersaanvragen voor SaaS-toepassingen van derden zoals televergaderingen, Google Apps en anderen. Als u gebruikersaanvragen voor derden SaaS-applicatie is ingeschakeld, bepaalt de Azure Management Portal de kenmerkwaarden in de vorm van een configuratie "kenmerktoewijzing" genoemd.

Er is een verzameling vooraf geconfigureerde kenmerktoewijzingen tussen AD Azure user-objecten en elke SaaS-app van gebruikersobjecten. Sommige apps beheren andere typen objecten, zoals groepen of contactpersonen. <br> 
U kunt de standaardtoewijzingen kenmerk aanpassen aan uw zakelijke behoeften. Dit betekent, kunt u wijzigen of nieuwe kenmerktoewijzingen maken of bestaande kenmerktoewijzingen verwijderen.

In de portal Azure AD u toegang tot deze functie door op kenmerken te klikken in de werkbalk van een SaaS-toepassing.

> [AZURE.NOTE] De **kenmerken** -koppeling is alleen beschikbaar als u ingeschakeld voor de toepassing van een SaaS gebruikersaanvragen hebt. 


![Televergaderingen][1] 


Wanneer u kenmerken klikt op de werkbalk, de lijst met huidige toewijzingen die zijn geconfigureerd voor een SaaS-toepassing.

De volgende schermafdruk toont een voorbeeld van deze:



![Televergaderingen][2]  


In het bovenstaande voorbeeld kunt u zien dat de **Voornaam** kenmerk van een beheerde objecten in de televergaderingen wordt gevuld met de **givenName** waarde van het gekoppelde object Azure AD.

Als u wilt uw kenmerktoewijzingen aanpassen of als u instellingen terug naar de standaardconfiguratie aangepaste, kunt u dit doen door te klikken op de bijbehorende knop op de werkbalk aan de onderkant van een toepassing.


![Televergaderingen][3]  


Er zijn kenmerktoewijzingen die vereist zijn voor een SaaS-toepassing goed te laten functioneren. In de tabel kenmerken hebben de gerelateerde kenmerktoewijzingen **Ja** als waarde voor het kenmerk **vereist** . Als een kenmerktoewijzing vereist is, kunt u deze niet verwijderen. In dit geval is de functie **verwijderen** niet beschikbaar.

Naar een bestaande toewijzing van kenmerken wijzigen, selecteert u de toewijzing en klik vervolgens op **bewerken**. Verschijnt een dialoogvenster pagina waarmee u de toewijzing van het geselecteerde kenmerk wijzigen.


![Toewijzing van kenmerken bewerken][4]  



## <a name="understanding-attribute-mapping-types"></a>Informatie over typen kenmerktoewijzing


Bepaal je hoe de kenmerken worden gevuld in een derde partij met kenmerktoewijzingen, SaaS-applicatie. Er zijn vier verschillende toewijzingstypen ondersteund:

- **Directe** – het doelkenmerk wordt gevuld met de waarde van een kenmerk van het gekoppelde object in AD Azure.


- **Constante** – het doelkenmerk wordt gevuld met een bepaalde tekenreeks die u hebt opgegeven.


- **Expressie** - het doelkenmerk wordt gevuld op basis van het resultaat van een script-achtige expressie. Zie voor meer details, [Het schrijven van expressies voor het toewijzen van kenmerken in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).


- **None** - het doelkenmerk links niet gewijzigd is. Echter, als het kenmerk target ooit leeg is, is het gevuld met de waarde die u opgeeft.



Naast deze vier eenvoudige toewijzing kenmerktypen ondersteunen aangepaste kenmerktoewijzingen het concept van de toewijzing van een **standaardwaarde** . De toewijzing van de standaard waarde zorgt ervoor dat een doelkenmerk wordt gevuld met een waarde als er geen waarde in Azure AD, noch op het doelobject.

Microsoft Azure AD biedt een zeer efficiënte implementatie van een synchronisatieproces. In een omgeving met geïnitialiseerd worden alleen objecten die moet worden bijgewerkt tijdens een synchronisatie-cyclus verwerkt. Heeft dit gevolgen voor de prestaties van een cyclus synchronisatie kenmerktoewijzingen bijwerken. Dit komt omdat een update van het kenmerk configuratie is vereist voor alle beheerde objecten om te worden reevaluated. Daarom is het aanbevolen beste houden uw kenmerktoewijzingen ten minste het aantal opeenvolgende wijzigingen.


##<a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Automatiseren van de gebruiker wordt ingericht/Deprovisioning voor SaaS-Apps](active-directory-saas-app-provisioning.md)
- [Het schrijven van expressies voor het toewijzen van kenmerken](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Filters voor gebruikersaanvragen scoping](active-directory-saas-scoping-filters.md)
- [Met behulp van SCIM automatisch inrichten van gebruikers en groepen van Azure Active Directory voor toepassingen inschakelen](active-directory-scim-provisioning.md)
- [Account meldingen inrichten](active-directory-saas-account-provisioning-notifications.md)
- [Lijst met zelfstudies over het integreren van SaaS-toepassingen](active-directory-saas-tutorial-list.md)


<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
