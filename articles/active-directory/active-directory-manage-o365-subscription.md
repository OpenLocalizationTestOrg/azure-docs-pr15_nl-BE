<properties
   pageTitle="Beheren van de directory voor uw abonnement op Office 365 in Azure | Microsoft Azure"
   description="Een Office 365 abonnement directory met Azure Active Directory en de klassieke Azure portal beheren"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>De map voor uw abonnement op Office 365 in Azure beheren

In dit artikel wordt beschreven hoe een directory die is gemaakt voor een Office 365-abonnement met de Azure klassieke portal beheren. U moet de servicebeheerder of een beheerder van een collega van een Azure-abonnement aan te melden de klassieke Azure portal. Als u nog geen een Azure-abonnement hebt, kunt u aanmelden voor een [gratis proefperiode van 30 dagen](https://azure.microsoft.com/trial/get-started-active-directory/) en uw eerste cloud-oplossing distribueren onder 5 minuten met behulp van deze koppeling. Zorg ervoor dat het werk of school account waarmee u zich kunt aanmelden bij Office 365.

Nadat u het abonnement Azure, kunt u zich aanmelden bij de klassieke Azure portal en toegang tot Azure services. Klik op de uitbreiding voor Active Directory voor het beheren van de map die uw Office 365-gebruikers worden geverifieerd.

Als u al een Azure-abonnement hebt, is het proces voor het beheren van een extra map ook eenvoudig. Michael Smith kan bijvoorbeeld een Office 365-abonnement voor Contoso.com. Hij heeft ook een Azure-abonnement dat hij met zijn Microsoft-account aangemeld voor msmith@hotmail.com. In dit geval beheert hij twee mappen.

  Abonnement |  Office 365  |  Azure
  -------------- | ------------- | -------------------------------
  Weergavenaam |  Contoso  |     Standaardmap voor Azure Active Directory (AD Azure)
  Naam van het domein  |  Contoso.com  | msmithhotmail.onmicrosoft.com

Hij wil de identiteit van gebruikers in de directory Contoso beheren terwijl hij is ingelogd met zijn account Microsoft Azure zodat hij Azure AD functies zoals multifactor-verificatie kunt inschakelen. In het volgende diagram kan bijdragen tot het proces te illustreren.

![Diagram met twee onafhankelijke mappen beheren](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

In dit geval zijn de twee mappen onafhankelijk van elkaar.

## <a name="to-manage-two-independent-directories"></a>Twee onafhankelijke mappen beheren
Michael Smith beide mappen beheren terwijl hij wordt aangemeld bij Azure als om msmith@hotmail.com, hij moet de volgende stappen uitvoeren:

> [AZURE.NOTE]
> Deze stappen kunnen worden uitgevoerd wanneer een gebruiker is aangemeld met een account van Microsoft. **Bestaande directory gebruiken** niet beschikbaar is als de gebruiker is aangemeld met een werk of school account, de optie. Een werk of school-account kan worden geverifieerd door de basismap (dat wil zeggen, de map waar het werk of school-account is opgeslagen en die eigenaar is van het bedrijf of de school).

1.  Aanmelden bij de [Azure klassieke portal](https://manage.windowsazure.com) als msmith@hotmail.com.
2.  Klik op **Nieuw** > **App services** > **Active Directory** > **map** > **Aangepaste maken**.
3.  Klik op bestaande map gebruiken en schakel het selectievakje **ik ben klaar om te worden afgemeld** .
4.  Aanmelden bij de klassieke Azure portal als het globale beheer van Contoso.onmicrosoft.com (bijvoorbeeld msmith@contoso.com).
5.  Wanneer u wordt gevraagd om te **de Contoso-directory gebruiken met Azure?**, klikt u op **Doorgaan**.
6.  Klik op **nu afmelden**.
7.  Aanmelden bij de Azure klassieke portal als msmith@hotmail.com. De Contoso-map en de map standaard weergegeven in de Active Directory-extensie.

Nadat u deze stappen uitvoert, msmith@hotmail.com is een globale beheerder in de map Contoso.

## <a name="to-administer-resources-as-the-global-admin"></a>Voor het beheren van bronnen als globale beheerder
Nu gaan we ervan uit dat Jane Doe moet beheren, websites en database resources die zijn gekoppeld aan het abonnement Azure voor msmith@hotmail.com. Als zij dat wilt doen, moet Michael Smith deze extra stappen uitvoeren:

1.  Aanmelden bij de [Azure klassieke portal](https://manage.windowsazure.com) met behulp van de account Administrator van de Service voor het abonnement Azure (in dit voorbeeld wordt msmith@hotmail.com).
2.  Het abonnement overbrengen naar de map Contoso: klik op **Instellingen** > **abonnementen** > Selecteer het abonnement > **Map bewerken** > Selecteer **Contoso (Contoso.com)**. Accounts die collega van de beheerders van het abonnement zijn worden verwijderd als onderdeel van de overdracht, werk of school.
3.  Jane Doe als co-beheerder van het abonnement toevoegen: klik op **Instellingen** > **beheerders** > Selecteer het abonnement > **toevoegen** > type **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Volgende stappen
Zie [hoe een abonnement is gekoppeld aan een map](active-directory-how-subscriptions-associated-directory.md)voor meer informatie over de relatie tussen de abonnementen en mappen.
