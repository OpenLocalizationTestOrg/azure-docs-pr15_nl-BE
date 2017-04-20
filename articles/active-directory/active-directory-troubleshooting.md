<properties
   pageTitle="Problemen oplossen: Is 'Active Directory' item ontbreekt of is niet beschikbaar | Microsoft Azure "
   description="Wat te doen wanneer Active Directory menu-item niet wordt weergegeven in de Portal Azure Management."
   services="active-directory"
   documentationCenter="na"
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

# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Problemen oplossen: Is 'Active Directory' item ontbreekt of is niet beschikbaar

Veel van de instructies voor het gebruik van Azure Active Directory-functies en services die beginnen met "Ga naar de Azure Management Portal en klik op **Active Directory**." Maar wat doet u als het Active Directory-extensie of het menu-item niet wordt weergegeven of als deze is gemarkeerd als **Niet beschikbaar**? Dit onderwerp is bedoeld om te helpen. Hierin worden de voorwaarden waaronder **Active Directory** niet wordt weergegeven of niet beschikbaar is en wordt uitgelegd hoe verder te gaan.

## <a name="active-directory-is-missing"></a>Active Directory ontbreekt.

Normaal gesproken een **Active Directory** -item wordt weergegeven in het linkernavigatiemenu. De instructies in Azure Active Directory-procedures wordt ervan uitgegaan dat dit item in de weergave.

![Schermafdruk: Active Directory in Azure](./media/active-directory-troubleshooting/typical-view.png)

Het Active Directory-item wordt weergegeven in het linkernavigatiemenu als een van de volgende voorwaarden is voldaan. Anders wordt weergegeven het item niet.

* De huidige gebruiker aangemeld bij een Microsoft-account (voorheen Windows Live ID).

    OR

* De huurder Azure is een directory en de lopende rekening is een directory-beheerder.

    OR

* De huurder Azure heeft ten minste één naamruimte van Azure AD toegangsbeheer (ACS). Zie [Access Control Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx)voor meer informatie.

    OR

* De huurder Azure heeft ten minste één Azure meerledige verificatie. Zie [Azure meerledige Verificatieproviders beheren](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)voor meer informatie.

Klik op **+ Nieuw**om een Access Control-naamruimte of een meerledige verificatie provider > **App Services** > **Active Directory**.

Als u beheerdersrechten aan een map, hebt u een beheerder een beheerdersrol toewijzen aan uw account. Zie [beheerder-rollen toewijzen](active-directory-assign-admin-roles.md)voor meer informatie.

## <a name="active-directory-is-not-available"></a>Active Directory is niet beschikbaar

Wanneer u klikt op **+ Nieuw** > **App Services**, een **Active Directory** -item wordt weergegeven. Met name verschijnt de Active Directory als een van de Active Directory-functies, zoals Directory, Access Control of Provider meerledige verificatie, beschikbaar voor de huidige gebruiker zijn.

Echter, terwijl de pagina wordt geladen, het item wordt grijs weergegeven en is gemarkeerd met **Niet beschikbaar**. Dit is een tijdelijke status. Als u een paar seconden wachten, wordt het item beschikbaar. Als de vertraging wordt verlengd, vaak de webpagina vernieuwen wordt het probleem opgelost.

![Schermafdruk: Active Directory is niet beschikbaar](./media/active-directory-troubleshooting/not-available.png)
