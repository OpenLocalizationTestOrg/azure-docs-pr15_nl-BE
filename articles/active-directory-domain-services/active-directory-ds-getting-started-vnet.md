<properties
    pageTitle="Azure AD Domain Services: Maak of Selecteer een virtueel netwerk | Microsoft Azure"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Maak of Selecteer een virtueel netwerk voor Azure AD Domain Services

## <a name="guidelines-to-select-an-azure-virtual-network"></a>Richtlijnen voor het selecteren van een virtueel netwerk van Azure
> [AZURE.NOTE] **Voordat u begint**: verwijzen naar [Overwegingen voor het AD-Domain Azure Services toegang](active-directory-ds-networking.md).


## <a name="task-2-create-an-azure-virtual-network"></a>Taak 2: Een Azure virtueel netwerk maken
De volgende configuratietaak is een Azure virtual network en een subnet binnen dit maken. U inschakelen Azure AD Domain Services in dit subnet binnen het virtuele netwerk. Als u al een bestaande virtuele netwerk wilt gebruiken, kunt u deze stap overslaan.

> [AZURE.NOTE] Zorg ervoor dat de Azure virtueel netwerk u maken of ervoor kiezen om te gebruiken in combinatie met Azure AD Domain Services deel uitmaakt van een Azure regio die wordt ondersteund door Azure AD Domain Services. Zie de pagina [Azure services per regio](https://azure.microsoft.com/regions/#services/) te weten de Azure gebieden waarin Azure AD Domain Services beschikbaar is.

Opmerking onder de naam van het virtuele netwerk zodat u het recht virtuele netwerk wanneer Azure AD Domain Services in een configuratiestap voor verdere inschakelen.

Voer de volgende configuratiestappen voor het maken van een Azure virtueel netwerk waar u graag wilt Azure AD Domain Services inschakelen.

1. Ga naar de **Azure klassieke portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Selecteer het knooppunt **netwerken** in het linkerdeelvenster.

    ![Knooppunt van netwerken](./media/active-directory-domain-services-getting-started/networks-node.png)

3. Klik op **Nieuw** in het taakvenster onder aan de pagina.

    ![Knooppunt virtuele netwerken](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. Selecteer in het knooppunt **Services netwerk** **Virtueel netwerk**.

5. Klik op de **Snelle invoer** om een virtueel netwerk te maken.

    ![Virtueel netwerk - snel maken](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Geef een **naam** voor het virtuele netwerk. U kunt ook de **adresruimte** of **VM Maximum aantal** voor dit netwerk configureren. Nu kunt u de **DNS-server** ingesteld op 'Geen'. U kunt de DNS-server instellen na het inschakelen van Azure AD Domain Services bijwerken.

7. Zorg ervoor dat u een ondersteunde Azure regio in de vervolgkeuzelijst **locatie selecteert** . Zie de pagina [Azure services per regio](https://azure.microsoft.com/regions/#services/) te weten de Azure gebieden waarin Azure AD Domain Services beschikbaar is.

8. Klik op de knop **een virtueel netwerk maken** om het virtuele netwerk.

    ![Maak een virtueel netwerk voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/create-vnet.png)

9. Nadat het virtuele netwerk is gemaakt, selecteert u het virtuele netwerk en klik op het tabblad **configureren** .

    ![Een subnet maken](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)

10. Ga naar de sectie **virtueel netwerk adresruimten** . Klik op **subnet toevoegen** en een subnetmasker opgeven met de naam **AaddsSubnet**. Klik op **Opslaan** om het subnet maken.

    ![Een subnet maken voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)


<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>Taak 3 - Enable Azure AD Domain Services
De volgende configuratietaak is [Azure AD Domain Services](active-directory-ds-getting-started-enableaadds.md)inschakelen.
