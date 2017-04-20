<properties
    pageTitle="Azure AD Domain Services: Update DNS-instellingen voor het virtuele netwerk van Azure | Microsoft Azure"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Azure AD Domain Services - Update DNS-instellingen voor het virtuele netwerk van Azure

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Taak 4: Bijwerken DNS-instellingen voor het virtuele netwerk van Azure
In de voorgaande configuratietaken, u hebt Azure AD Domain Services voor uw map ingeschakeld. De volgende taak is om ervoor te zorgen dat computers binnen het virtuele netwerk kunnen aansluiten en gebruik van deze services. De DNS-instellingen voor het virtuele netwerk om te verwijzen naar de twee IP-adressen die Azure AD Domain Services beschikbaar op het virtuele netwerk is bijwerken.

> [AZURE.NOTE] De IP-adressen noteren voor Azure AD Domain Services op het tabblad **configureren** van de map wordt weergegeven nadat u de Azure AD Domain Services voor de map hebt ingeschakeld.

Voer de volgende configuratiestappen voor het bijwerken van de DNS-server de instelling voor het virtuele netwerk waarmee u Azure AD Domain Services hebt ingeschakeld.

1. Ga naar de **Azure klassieke portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Selecteer het knooppunt **netwerken** in het linkerdeelvenster.

    ![Knooppunt virtuele netwerken](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. Selecteer in het tabblad **Virtuele netwerken** het virtuele netwerk waarin u Azure AD Domain Services om de eigenschappen ervan weer te geven ingeschakeld.

4. Klik op het tabblad **configureren** .

    ![Knooppunt virtuele netwerken](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. Voer in de sectie **DNS-servers** de IP-adressen van Azure AD Domain Services.

6. Zorg ervoor dat u zowel de IP-adressen die werden weergegeven in de sectie **Domain Services** op het tabblad **configureren** van de map opgeven.

7. Als u wilt opslaan in de instellingen van de DNS-server voor dit virtuele netwerk, klikt u op **Opslaan** in het taakvenster onder aan de pagina.

   ![De DNS-instellingen voor het virtuele netwerk bijwerken.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] Na het bijwerken van de DNS-instellingen voor het virtuele netwerk, duurt het een tijdje voor virtuele machines op het netwerk om de bijgewerkte DNS-configuratie. Als een virtuele machine kan geen verbinding maken met het domein, kunt u (bv de DNS-cache leegmaken. ' ipconfig/flushdns') op de virtuele machine. Met deze opdracht zorgt ervoor dat de DNS-instellingen op de virtuele machine vernieuwen.


## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>Taak 5 - wachtwoordsynchronisatie inschakelen Azure AD Domain Services
De volgende configuratietaak is [Wachtwoordsynchronisatie Azure AD Domain Services inschakelen](active-directory-ds-getting-started-password-sync.md).
