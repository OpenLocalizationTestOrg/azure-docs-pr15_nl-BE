<properties
pageTitle="Pas de pagina-in in de voorvertoning Azure Active Directory | Microsoft Azure"
description="Informatie over het toevoegen van een eigen stempel drukken op de pagina voor Azure-in bedrijf"
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
ms.date="09/30/2016"
ms.author="curtand"/>

# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Toevoegen aan uw pagina-in in de voorvertoning Azure Active Directory huisstijlen

Om verwarring te voorkomen, willen veel bedrijven een consistente vormgeving toepassen op alle websites en services die ze beheren. Voorbeeld van Azure Active Directory voorziet in deze mogelijkheid doordat u het uiterlijk van de pagina aanmelden met uw bedrijfslogo en aangepaste kleurenschema's. [Wat is in het voorbeeld?](active-directory-preview-explainer.md) De pagina is de pagina die wordt weergegeven wanneer u zich aanmeldt voor Office 365 of andere web gebaseerde toepassingen die van AD Azure als leverancier van uw identiteit gebruikmaken. Werkt u met deze pagina uw referenties moet invoeren.

Als u weergeven van het merk van uw bedrijf, kleuren en andere elementen worden aangepast op deze pagina wilt, Zie de volgende afbeeldingen te begrijpen het verschil tussen de twee ervaringen.

De volgende schermafbeelding ziet en voorbeeld voor de Office 365-aanmeldingspagina op een pc **voordat u** een aanpassing:

![Office 365-aanmeldingspagina voor aanpassing](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

De volgende schermafbeelding ziet en voorbeeld voor de Office 365-aanmeldingspagina op een desktopcomputer **als** een aanpassing:

![Office 365-aanmeldingspagina na aanpassing](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)


## <a name="customizing-the-sign-in-page"></a>De pagina aanpassen

Normaal, als u browser gebaseerde toegang tot uw cloud-toepassingen en services die uw organisatie op geabonneerd, gebruikt u de aanmeldingspagina.

Als u wijzigingen op uw pagina hebt toegepast, kan het duren tot een uur voordat de wijzigingen worden weergegeven.

Een huisstijl-aanmeldingspagina wordt alleen weergegeven wanneer u een service met een huurder-specifieke URL zoals https://outlook.com/**contoso**.com of https://mail bezoekt. **Contoso**. com.

Wanneer u een service met een specifieke URL's niet huurder bezoekt (bijvoorbeeld: https://mail.office365.com), een niet-merk-in de pagina wordt weergegeven. in dit geval uw huismerk wordt weergegeven nadat u uw gebruikers-ID hebt ingevoerd of u hebt een gebruikerstegel geselecteerd.

> [AZURE.NOTE]
>
- Uw domeinnaam moet worden weergegeven als 'Actief' in het gedeelte van de **domeinen** van de Azure portal die u hebt geconfigureerd huisstijl. Zie [namen van aangepaste domein toevoegen](active-directory-domains-add-azure-portal.md)voor meer informatie.
- Huismerk-aanmeldingspagina niet over te dragen aan de consument aanmeldingspagina van Microsoft. Als u zich met een Microsoft-account aanmelden, wordt er een huisstijl lijst van gebruiker tegels weergegeven door Azure AD, maar de huisstijl van uw organisatie niet van toepassing op de Microsoft-account aanmelden pagina.

Het selectievakje **laat me ingelogd blijven** kan de aanmeldingspagina, een gebruiker ingelogd blijven als ze sluiten en opnieuw openen van de browser. 

   ![Laat me ingelogd blijven](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Dit heeft geen invloed op de levensduur van de sessie. U kunt verbergen het selectievakje op de aanmeldingspagina van Azure Active Directory.
Of het selectievakje wordt weergegeven, is afhankelijk van de instelling van **laat me ingelogd uitgeschakeld blijven**.

   ![Laat me ingelogd blijven](./media/active-directory-branding-custom-signon-azure-portal/02.png)


Als u het selectievakje verbergen, dit instellen op **Ja**. 

> [AZURE.NOTE] Gebruikers kunnen dit vakje is afhankelijk van sommige functies van SharePoint Online en Office 2010. Als u deze instelling verborgen configureert, kunnen uw gebruikers te zien meer en onverwachte vragen om aan te melden.




**Huisstijlen naar uw map toevoegen:**

1.  Log in om de [Azure portal](https://portal.azure.com) met een account die is een algemene admin voor de map.

2.  Selecteer **meer services**, **gebruikers en groepen** in het tekstvak invoeren en selecteer vervolgens **Enter**.

    ![Gebruikersbeheer openen](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)

3. Selecteer op de **gebruikers en groepen** -blade **huisstijl bedrijf**.

4. Op de **gebruikers en groepen - bedrijf huismerk** blade, selecteert u de opdracht **bewerken** .

    ![Aangepaste huisstijl bewerken](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)

5. Wijzig de elementen die u wilt aanpassen. Alle elementen zijn optioneel.

6. Klik op **Opslaan**.

Het kan duren een uur voor eventuele wijzigingen in de huisstijl weergegeven pagina.

## <a name="next-steps"></a>Volgende stappen

[Taalspecifieke huisstijlen toevoegen](active-directory-branding-localize-azure-portal.md)
