<properties
pageTitle="Taalspecifieke huisstijlen aan uw pagina-in in de voorvertoning Azure Active Directory toevoegen | Microsoft Azure"
description="Informatie over het toevoegen van een bepaald bedrijf taal afbeeldingen en tekst op een Azure-aanmeldingspagina huismerk"
services="active-directory"
documentationCenter=""
authors="curtand"
manager="femila"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/12/2016"
ms.author="curtand"/>

# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Taalspecifieke huisstijlen aan uw pagina-in in de voorvertoning Azure Active Directory toevoegen

Om verwarring te voorkomen, willen veel bedrijven een consistente vormgeving toepassen op alle websites en services die ze beheren. Voorbeeld van Azure Active Directory voorziet in deze mogelijkheid doordat u het uiterlijk van de pagina aanmelden met uw bedrijfslogo en aangepaste kleurenschema's. [Wat is in het voorbeeld?](active-directory-preview-explainer.md) De pagina is de pagina die wordt weergegeven wanneer u zich aanmeldt voor Office 365 of andere web gebaseerde toepassingen die van AD Azure als leverancier van uw identiteit gebruikmaken. Werkt u met deze pagina uw referenties moet invoeren.

## <a name="customizing-the-sign-in-page-for-another-language"></a>Aanpassen van de pagina aanmelden voor een andere taal

U kunt taalspecifieke elementen toevoegen aan uw aangepaste pagina-in alleen als u al een aangepaste pagina als beschreven in [huisstijlen aan uw pagina toevoegen](active-directory-branding-custom-signon-azure-portal.md). U kunt een aanmeldingspagina per map met een standaardset elementen worden aangepast. Nadat u de reeks pagina-elementen hebt geconfigureerd, kunt u meer versies voor verschillende landinstellingen. U kunt ook mengen en overeenkomen met de verschillende elementen. U kunt bijvoorbeeld:

- Maak een standaard **-in pagina-afbeelding** die werkt voor alle culturen en vervolgens specifieke versies maken voor Nederlands en Frans. Wanneer u uw browser op een van deze twee talen instelt, de taal-specifieke afbeelding wordt weergegeven, terwijl de standaard-afbeelding wordt weergegeven voor alle andere talen.

- Configureer andere logo's voor uw organisatie (bijvoorbeeld Japanse of Hebreeuwse versies).

We aanbevolen dat u het nummer van de taal variaties laag, voor onderhoud en prestaties.

**Huisstijlen naar uw map toevoegen:**

1.  Log in om de [Azure portal](https://portal.azure.com) met een account die is een algemene admin voor de map.

2.  Selecteer **meer services**, **gebruikers en groepen** in het tekstvak invoeren en selecteer vervolgens **Enter**.

    ![Gebruikersbeheer openen](./media/active-directory-branding-localize-azure-portal/user-management.png)

3. Selecteer op de **gebruikers en groepen** -blade **huisstijl bedrijf**.

4. Op de **gebruikers en groepen - bedrijf huismerk** blade, selecteert u de opdracht **taal toevoegen** .

    ![Taal-specifieke huismerk elementen toevoegen](./media/active-directory-branding-localize-azure-portal/add-language.png)

5. Wijzig de elementen die u wilt aanpassen. Alle elementen zijn optioneel.

6. Klik op **Opslaan**.

Het kan duren een uur voor eventuele wijzigingen in de huisstijl weergegeven pagina.

## <a name="next-steps"></a>Volgende stappen

[Huisstijlen aan uw pagina toevoegen](active-directory-branding-custom-signon-azure-portal.md)
