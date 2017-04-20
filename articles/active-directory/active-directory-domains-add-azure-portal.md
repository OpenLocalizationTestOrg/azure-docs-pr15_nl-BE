<properties
    pageTitle="Uw aangepaste domeinnaam toevoegen aan Azure Active Directory preview | Microsoft Azure"
    description="Het toevoegen van uw bedrijf domeinnamen aan Azure Active Directory en het controleren van de domeinnaam."
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
    ms.date="10/17/2016"
    ms.author="curtand"/>

# <a name="add-a-custom-domain-name-to-azure-active-directory-preview"></a>Een aangepaste domeinnaam toevoegen aan Azure Active Directory-voorbeeld

> [AZURE.SELECTOR]
- [Azure portal](active-directory-domains-add-azure-portal.md)
- [Azure klassieke portal](active-directory-add-domain.md)

U hebt een of meer domeinnamen die uw organisatie gebruikt om zaken te doen en uw gebruikers zich aanmelden met uw bedrijfsnetwerk met behulp van de domeinnaam van uw bedrijf. Azure Active Directory (AD Azure) voorbeeld kunt u uw zakelijke domeinnaam toevoegen aan Azure AD ook. [Wat is in het voorbeeld?](active-directory-preview-explainer.md) Hiermee kunt u het toewijzen van namen in de map die uw gebruikers bekend, zoals zijn ‘alice@contoso.com.’ het proces is eenvoudig:

1. Naam van het aangepaste domein toevoegen aan de directory
2. Een DNS-vermelding voor de naam van het domein op het domeinnamenregister toevoegen
3. Controleer of de aangepaste domeinnaam in Azure AD

## <a name="how-do-i-add-a-domain-name"></a>Hoe voeg ik een domeinnaam?

1.  Log in om de [Azure portal](https://portal.azure.com) met een account die is een algemene admin voor de map.

2.  Selecteer **meer services**en selecteer vervolgens **Enter** **Azure Active Directory** in het tekstvak invoeren.

    ![Gebruikersbeheer openen](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Selecteer op de ***naam van de map*** -blade **domeinnamen**.

4. Selecteer de opdracht **toevoegen** op de ** *naam van de directory* - domeinnamen** -blade.

  ![Selectie van de opdracht toevoegen](./media/active-directory-domains-add-azure-portal/add-command.png)

5. Voer de naam van uw aangepaste domein in het vak, zoals 'contoso.com' op de blade **-naam** en selecteer **Domein toevoegen**. Zorg ervoor dat u de .com, .net of andere uitbreiding op het hoogste niveau.

6. Op de ***domeinnaam*** blade (dat wil zeggen, het blad dat wordt geopend met uw nieuwe domeinnaam in de titel), krijgen de DNS-vermelding informatie Azure AD wordt gebruikt om te controleren of uw organisatie eigenaar is van de aangepaste domeinnaam.

  ![DNS-gegevens ophalen](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

U kunt de naam van het domein hebt toegevoegd, moet uw organisatie eigenaar is van de domeinnaam Azure AD controleren. Voordat u AD Azure kan deze controle uitvoert, moet u een DNS-vermelding toevoegen in het DNS-zonebestand voor de domeinnaam. Deze taak wordt uitgevoerd op de website voor domeinnamenregister voor de domeinnaam.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>De DNS-vermelding bij het domeinnamenregister van voor het domein toevoegen

De volgende stap in uw aangepaste domeinnaam gebruiken met AD Azure is voor het bijwerken van het bestand van de DNS-zone voor het domein. Hierdoor Azure AD om te controleren of uw organisatie eigenaar is van de aangepaste domeinnaam.

1.  Aanmelden bij het domeinnamenregister van het domein. Als u geen toegang tot het bijwerken van de DNS-vermelding, vraagt u de persoon of het team die deze toegang hebben tot het uitvoeren van stap 2 en laten u weten wanneer deze is voltooid.

2.  Het bestand DNS-zone voor het domein bijwerken door de DNS-vermelding die u van Azure AD toe te voegen. Deze DNS-vermelding kunt Azure AD om te controleren of uw eigendom van het domein. De DNS-vermelding verandert niet alle gedragingen zoals routering van e-mailberichten of webpagina die als host fungeert.

Hulp bij deze DNS-naam toevoegen, lees de [instructies voor het toevoegen van een DNS-vermelding op populaire DNS-registrators](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Controleer of de naam van het domein met Azure Active Directory

Als u de DNS-vermelding hebt toegevoegd, bent u klaar om te controleren of de naam van het domein met Azure Active Directory.

De naam van een domein kan worden geverifieerd nadat de DNS-records zijn doorgegeven. Deze doorgifte vaak duurt slechts seconden, maar het kan soms een uur of langer duren. Als de verificatie niet de eerste keer niet werkt, probeer het later opnieuw.

1.  Log in om de [Azure portal](https://portal.azure.com) met een account die is een algemene admin voor de map.

2.  Selecteer **Bladeren**en selecteert u **Enter**User Management invoeren in het tekstvak.

    ![Gebruikersbeheer openen](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Selecteer de niet-geverifieerde domeinnaam die u wilt controleren of op de bladeserver **User management - domeinnamen** .

4. Selecteer **controleren** voor het voltooien van de controle op de bladeserver ***domeinnaam*** (dat wil zeggen, het blad dat wordt geopend met uw nieuwe domeinnaam in de titel).

Nu kunt u [die uw aangepaste domeinnaam bevatten gebruikersnamen toewijzen](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Het oplossen van problemen

Als u de naam van een aangepaste domein kan worden geverifieerd, probeert u het volgende. We beginnen met de meest voorkomende en tot de kleinste gemene werken.

1.  **Wacht een uur**. DNS-records moeten worden doorgegeven voordat Azure AD kunt controleren of het domein. Dit kan een uur of langer duren.

2.  **Zorg ervoor dat de DNS-record is ingevoerd, en juist is**. Voltooi deze stap op de website van de registratieservice domein naam voor het domein. Azure AD kan de domeinnaam niet als de DNS-vermelding niet aanwezig in het DNS-zonebestand is of als het niet exact overeen met de DNS-vermelding die Azure AD geleverd, kunt u controleren. Als u geen toegang tot het bijwerken van DNS-records voor het domein bij het domeinnamenregister, delen van de DNS-vermelding met de persoon of het team in uw organisatie die deze toegang heeft en vragen of deze de DNS-vermelding toevoegen.

3.  **De domeinnaam van een andere adreslijst in Azure AD verwijderen**. Een domeinnaam kan worden gecontroleerd in één map. Als u een domeinnaam gecontroleerd werd eerder in een andere map, moet deze worden verwijderd er voordat deze kan worden gecontroleerd in de nieuwe map. Lees meer over het verwijderen van domeinnamen, [de aangepaste domeinnamen beheren](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Aangepaste domeinnamen toevoegen

Als uw organisatie gebruikmaakt van meerdere aangepaste domeinnamen, zoals 'contoso.com' en 'contosobank.com', kunt u deze toevoegen met een maximum van 900 domeinnamen. Gebruik dezelfde stappen in dit artikel toe te voegen elk van uw domeinnamen.

## <a name="next-steps"></a>Volgende stappen

[Aangepaste domeinnamen beheren](active-directory-domains-manage-azure-portal.md)
