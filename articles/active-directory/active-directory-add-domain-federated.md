<properties
    pageTitle="Uw aangepaste domeinnaam toevoegen en instellen van federatieve eenmalige aanmelding met Azure Active Directory | Microsoft Azure"
    description="Het domeinnamen van uw bedrijf toevoegen aan Azure Active Directory en het instellen van federatieve eenmalige aanmelding tussen Azure Active Directory en uw oplossing op ruimten federation."
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
    ms.topic="get-started-article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>Uw aangepaste domeinnaam aan Azure Active Directory toevoegen

U kunt een aangepaste domeinnaam, bijvoorbeeld 'contoso.com' zodat gebruikers in contoso.com kunnen een federatieve één aanmelding ervaring met uw bedrijfsnetwerk hebt. Als u Active Directory Federation Services (AD FS) al hebt of een andere federation-server op uw bedrijfsnetwerk, kunt u Azure AD voor het gebruik van uw aangepaste domeinnaam met het hulpprogramma Azure AD verbinden. U kunt ook Azure AD-verbinding gebruiken voor de implementatie van een nieuwe AD FS-omgeving en die voor de federatieve eenmalige aanmelding naar Azure AD configureren.

Als u geen hebt en niet van plan bent voor de implementatie van AD FS of een andere federatieserver, als volgt: [een aangepaste domeinnaam naar Azure Active Directory toevoegen](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-to-your-directory"></a>Een aangepaste domeinnaam toevoegen aan de directory

1. Log in om de [Azure klassieke portal](https://manage.windowsazure.com/) met een gebruikersaccount die is een globale beheerder van uw map Azure AD.

2. Open de map en klik op het tabblad **domeinen** in **Active Directory**.

3. Op de opdrachtbalk **toevoegen**te selecteren en voer vervolgens de naam van uw aangepaste domein, zoals 'contoso.com'. Zorg ervoor dat u de .com, .net of andere uitbreiding op het hoogste niveau.

4. Schakel het selectievakje **Ik ben van plan dit domein voor eenmalige aanmelding met mijn lokale Active Directory te configureren** .

5. Selecteer **toevoegen**.

Voer het programma Azure AD verbinding maken als u de DNS-vermelding die Azure AD gebruiken wilt om te controleren of het domein. Ziet u de DNS-vermelding in de **AD-domein Azure** stap in de wizard. U kunt zien wat die stap in de wizard ziet er [in deze instructies](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Als u het hulpprogramma Azure AD verbinden niet hebt, kunt u [hier downloaden](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>De DNS-vermelding bij het domeinnamenregister van voor het domein toevoegen

De volgende stap in uw aangepaste domeinnaam gebruiken met AD Azure is voor het bijwerken van het bestand van de DNS-zone voor het domein. Hierdoor Azure AD om te controleren of uw organisatie eigenaar is van de aangepaste domeinnaam.

1. Log in om de website van domein naam registratieservice voor uw domeinnaam. Als u geen toegang tot dit doet, vraagt u de persoon of het team in uw organisatie die deze toegang heeft tot het uitvoeren van stap 2 en zodat u weet wanneer deze is voltooid.

2. Het bestand DNS-zone voor het domein bijwerken door de DNS-vermelding die u van Azure AD toe te voegen. Deze DNS-vermelding kunt Azure AD om te controleren of uw eigendom van het domein. De DNS-vermelding verandert niet alle gedragingen zoals routering van e-mailberichten of webpagina die als host fungeert.

Hulp bij deze stap, lees de [instructies voor het toevoegen van een DNS-vermelding op populaire DNS-registrators](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Controleer of de naam van het domein met Azure Active Directory

Als u de DNS-vermelding hebt toegevoegd, bent u klaar om te controleren of de naam van het domein met Azure Active Directory.

Om te controleren of het domein, selecteert u **volgende** op de **Azure AD domein** stap van de wizard Azure AD verbinden. Azure AD zoekt naar de DNS-vermelding in het bestand van de DNS-zone voor het domein. De domeinnaam Azure AD alleen controleren als de DNS-records zijn doorgegeven. Vermeerdering vaak duurt slechts seconden, maar het kan soms een uur of langer duren. Als de verificatie niet de eerste keer niet werkt, probeer het later opnieuw.

Vervolgens gaat u verder met de overige stappen in de wizard Azure AD verbinden. Deze worden gebruikers AD server Windows Azure AD gesynchroniseerd. Gesynchroniseerde gebruikers in het domein dat u hebt geconfigureerd voor Federatie is mogelijk op een federatieve single sign-on ervaring weg van uw bedrijfsnetwerk naar Azure AD.

## <a name="troubleshooting"></a>Het oplossen van problemen

Als u de naam van een aangepaste domein kan worden geverifieerd, probeert u het volgende. We beginnen met de meest voorkomende en tot de kleinste gemene werken.

1.  **Wacht een uur**. DNS-records moeten worden doorgegeven voordat Azure AD kunt controleren of het domein. Dit kan een uur of langer duren.

2.  **Zorg ervoor dat de DNS-record is ingevoerd, en juist is**. Voltooi deze stap op de website van de registratieservice domein naam voor het domein. Azure AD kan de domeinnaam niet als de DNS-vermelding niet aanwezig in het DNS-zonebestand is of als het niet exact overeen met de DNS-vermelding die Azure AD geleverd, kunt u controleren. Als u geen toegang tot het bijwerken van DNS-records voor het domein bij het domeinnamenregister, delen van de DNS-vermelding met de persoon of het team in uw organisatie die deze toegang heeft en vragen of deze de DNS-vermelding toevoegen.

3.  **De domeinnaam van een andere adreslijst in Azure AD verwijderen**. Een domeinnaam kan worden gecontroleerd in één map. Als u een domeinnaam gecontroleerd werd eerder in een andere map, moet deze worden verwijderd er voordat deze kan worden gecontroleerd in de nieuwe map. Lees meer over het verwijderen van domeinnamen, [de aangepaste domeinnamen beheren](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Aangepaste domeinnamen toevoegen

Als uw organisatie gebruikmaakt van meerdere aangepaste domeinnamen, zoals 'contoso.com' en 'contosobank.com', kunt u deze toevoegen met een maximum van 900 domeinnamen. Gebruik dezelfde stappen in dit artikel toe te voegen elk van uw domeinnamen.

## <a name="next-steps"></a>Volgende stappen

-   [Aangepaste domeinnamen beheren](active-directory-add-manage-domain-names.md)
-   [Meer informatie over domein management concepten in Azure AD](active-directory-add-domain-concepts.md)
-   [Huisstijl van uw bedrijf bij het aanmelden van gebruikers weergeven](active-directory-add-company-branding.md)
-   [PowerShell gebruiken voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
