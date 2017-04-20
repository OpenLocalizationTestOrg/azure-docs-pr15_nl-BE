<properties
    pageTitle="Beheer van aangepaste domeinnamen in uw voorbeeld Azure Active Directory | Microsoft Azure"
    description="Begrippen voor Computerbeheer en uitleg voor het beheren van een domeinnaam in Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand;jeffsta"/>

# <a name="managing-custom-domain-names-in-your-azure-active-directory-preview"></a>Aangepaste domeinnamen in uw voorbeeld Azure Active Directory beheren

Een domeinnaam is een belangrijk onderdeel van de vele bronnen van directory-ID: deze deel uitmaakt van een gebruikersnaam of e-mailadres voor een deel van het adres van een groep, gebruiker en kan deel uitmaken van de app-ID-URI voor een toepassing. Een bron in Azure Active Directory (AD Azure) voorbeeld kan een domeinnaam die reeds is gecontroleerd en het eigendom zijn van de map waarin u de bron bevatten. [Wat is in het voorbeeld?](active-directory-preview-explainer.md) Alleen de beheerder van een globale kunt domein beheertaken uitvoeren in Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>De primaire domeinnaam voor uw Azure AD-map instellen

Wanneer de map wordt gemaakt, is de eerste domeinnaam, bijvoorbeeld 'contoso.onmicrosoft.com', ook de naam van het primaire domein. Het primaire domein is de naam van het standaarddomein voor een nieuwe gebruiker wanneer u een nieuwe gebruiker maken. Dit stroomlijnt het proces voor een beheerder nieuwe gebruikers kunnen maken in de portal. De primaire domeinnaam wijzigen:

1.  Log in om de [Azure portal](https://portal.azure.com) met een account die is een algemene admin voor de map.

2.  Selecteer **meer services**en selecteer vervolgens **Enter** **Azure Active Directory** in het tekstvak invoeren.

    ![Gebruikersbeheer openen](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Selecteer op de ***naam van de map*** -blade **domeinnamen**.

4. Selecteer de naam van het domein dat u wilt de naam van de primaire domeincontroller op de ** *naam van de directory* - domeinnamen** -blade.

5.  Op het blad ***domeinnaam*** (dat wil zeggen, het blad dat wordt geopend met uw nieuwe domeinnaam in de titel) **Primair maken** de opdracht te selecteren. Bevestig uw keuze wanneer daarom wordt gevraagd.

    ![Een domeinnaam primair maken](./media/active-directory-domains-manage-azure-portal/make-primary.png)

U kunt de primaire domeinnaam voor uw map voor elke gecontroleerde aangepaste domein dat federated is niet wijzigen. De gebruikersnamen voor alle gebruikers wordt niet gewijzigd als u het primaire domein voor de map wijzigen.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Aangepaste domeinnamen toevoegen aan je advertentie Azure

U kunt maximaal 900 aangepaste domeinnamen toevoegen aan elke map Azure AD. Tijdens het toevoegen van [een aanvullende aangepaste domeinnaam](active-directory-domains-add-azure-portal.md) is hetzelfde voor de eerste aangepaste domeinnaam.

## <a name="add-subdomains-of-a-custom-domain"></a>Subdomeinen van een aangepaste domein toevoegen

Als u wilt dat een derde niveau domeinnaam zoals 'europe.contoso.com' toevoegen aan de directory, moet u toevoegen en controleer of het domein van het tweede niveau, bijvoorbeeld contoso.com. Het subdomein wordt automatisch gecontroleerd door AD Azure. Overzicht van het subdomein dat u zojuist hebt toegevoegd is gecontroleerd, vernieuw de pagina in de browser die domeinen.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Wat te doen als u de DNS registratieservice voor uw aangepaste domeinnaam wijzigen

Als u de DNS registratieservice voor uw aangepaste domeinnaam wijzigt, kunt u uw aangepaste domeinnaam gebruiken met Azure AD zelf zonder onderbreking en zonder extra configuratietaken blijven. Als u uw aangepaste domeinnaam met Office 365, Raadpleeg Intune of andere services die van aangepaste domeinnamen in Azure AD gebruikmaken, de documentatie voor deze services.

## <a name="delete-a-custom-domain-name"></a>De naam van een aangepaste domein verwijderen

U kunt een aangepaste domeinnaam verwijderen uit uw Azure Active Directory als uw organisatie die domeinnaam niet meer gebruikt of als u wilt gebruiken die domeinnaam met een andere Azure Active Directory.

Een aangepaste domein als naam wilt verwijderen, moet u eerst controleren in uw directory geen resources is afhankelijk van de domeinnaam. U kunt de naam van een domein verwijderen uit de map als:

-   Elke gebruiker heeft een gebruikersnaam, e-mailadres of proxy-adres dat bestaat uit de domeinnaam.

-   Elke groep heeft een e-mailadres of de proxy-adres dat bestaat uit de domeinnaam.

-   Elke toepassing in uw advertentie Azure is een app-ID-URI die de domeinnaam bevat.

U moet wijzigen of verwijderen van alle bronnen die in uw map Azure AD voordat u de naam van het aangepaste domein kunt verwijderen.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>PowerShell of Graph API gebruiken voor het beheren van domeinnamen

De meeste beheertaken voor domeinnamen in Azure Active Directory kunnen ook worden uitgevoerd met behulp van Microsoft PowerShell of via programmacode met behulp van Azure AD Graph API (in public preview).

-   [PowerShell gebruiken voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Graph-API gebruiken voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Volgende stappen

-   [Namen van aangepaste domein toevoegen](active-directory-domains-add-azure-portal.md)
