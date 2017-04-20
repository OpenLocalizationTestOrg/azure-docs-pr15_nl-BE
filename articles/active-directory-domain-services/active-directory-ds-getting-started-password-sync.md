<properties
    pageTitle="Azure AD Domain Services: Wachtwoordsynchronisatie inschakelen | Microsoft Azure"
    description="Aan de slag met Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Wachtwoordsynchronisatie Azure AD Domain Services inschakelen
In de voorafgaande taken u Azure AD Domain Services voor uw huurder Azure AD ingeschakeld. De volgende taak is het inschakelen van de referentie-hashes voor NTLM en Kerberos-verificatie te synchroniseren naar Azure AD Domain Services vereist. Zodra de synchronisatie van de referentie is ingesteld, zich gebruikers aanmelden bij de beheerde domein met hun zakelijke referenties.

De stappen zijn verschillende op basis van of uw organisatie een alleen-wolk Azure AD is pachters of is ingesteld om te synchroniseren met uw op ruimten directory Azure AD verbinden met.

<br>

> [AZURE.SELECTOR]
- [Alleen cloud Azure AD pachters](active-directory-ds-getting-started-password-sync.md)
- [Azure AD huurder gesynchroniseerd](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Taak 5: Wachtwoordsynchronisatie AAD Domain Services voor een Azure cloud alleen inschakelen AD huurder
Azure AD Domain Services moeten referentie-hashes in een indeling die geschikt is voor NTLM en Kerberos-verificatie, verificatie van gebruikers in het domein beheerd. Tenzij u AAD Domain Services voor uw huurder inschakelt, Azure AD niet genereren of referentie-hashes opslaan in de indeling voor NTLM of Kerberos-verificatie is vereist. Voor de hand liggende veiligheidsredenen slaat Azure AD ook geen referenties in leesbare vorm. Azure AD heeft daarom geen een manier voor het genereren van deze op basis van bestaande referenties van gebruikers NTLM of Kerberos-referenties-hashes.

> [AZURE.NOTE] Als uw organisatie een wolk alleen-lezen AD Azure huurder, gebruikers die willen gebruikmaken van domeinservices Azure AD hun wachtwoord moet wijzigen.

Dit proces wachtwoord wijzigen, wordt de referentie hashes die zijn vereist voor Azure AD Domain Services voor Kerberos en NTLM-verificatie in AD Azure worden gegenereerd. U kunt beide wachtwoorden voor alle gebruikers in de pachter die gebruikmaken van domeinservices Azure AD of opgeven dat deze gebruikers hun wachtwoorden te wijzigen moet verlopen.


### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>NTLM en Kerberos referentie hash genereren voor een Azure cloud alleen inschakelen AD huurder
Hier volgen instructies die u nodig hebt om eindgebruikers te bieden, zodat zij hun wachtwoord kunnen wijzigen:

1. Navigeer naar de pagina deelvenster Azure AD toegang voor uw organisatie op [http://myapps.microsoft.com](http://myapps.microsoft.com).

2. Selecteer het tabblad **profiel** op deze pagina.

3. Klik op de tegel **wachtwoord wijzigen** op deze pagina.

    ![Maak een virtueel netwerk voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password.png)

    > [AZURE.NOTE] Zorg ervoor dat uw organisatie [Wachtwoordbeheer in Azure AD](../active-directory/active-directory-passwords-getting-started.md)is geconfigureerd als de optie **wachtwoord wijzigen** op de pagina toegang tot Configuratiescherm niet wordt weergegeven.

4. Typ het bestaande (oude) wachtwoord op de pagina **wachtwoord wijzigen** en typ een nieuw wachtwoord en Bevestig het. Klik op **indienen**.

    ![Maak een virtueel netwerk voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Nadat u uw wachtwoord hebt gewijzigd, worden het nieuwe wachtwoord binnenkort in Azure AD Domain Services gebruikt. Na een paar minuten (meestal ongeveer 20 minuten), kunt u zich aanmelden op computers die deel uitmaken van het beheerde domein met de zojuist gewijzigde wachtwoord.

<br>

## <a name="related-content"></a>Verwante inhoud

- [Het bijwerken van uw eigen wachtwoord](../active-directory/active-directory-passwords-update-your-own-password.md)

- [Aan de slag met wachtwoordbeheer in Azure AD](../active-directory/active-directory-passwords-getting-started.md).

- [Inschakelen van Wachtwoordsynchronisatie AAD Domain Services voor een gesynchroniseerde Azure AD huurder](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Een beheerde Azure AD Domain Services-domein beheren](active-directory-ds-admin-guide-administer-domain.md)

- [Een virtuele Windows-computer aan een beheerde Azure AD Domain Services-domein toevoegen](active-directory-ds-admin-guide-join-windows-vm.md)

- [Een Red Hat Enterprise Linux virtuele machine aan een beheerde Azure AD Domain Services-domein toevoegen](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
