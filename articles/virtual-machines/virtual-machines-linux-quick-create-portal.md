<properties
    pageTitle="Maak een Linux VM met behulp van de Portal Azure | Microsoft Azure"
    description="Maak een Linux VM met behulp van de Portal Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Maak een Linux VM op Azure met behulp van de Portal


In dit artikel wordt beschreven hoe u met behulp van de [portal Azure](https://portal.azure.com/) een Linux virtuele Machine maken.

De eisen zijn:

- [een account Azure](https://azure.microsoft.com/pricing/free-trial/)

- [SSH openbare en persoonlijke bestanden](virtual-machines-linux-mac-create-ssh-keys.md)


1. De Azure portal met uw accountidentiteit Azure aangemeld, klikt u op **+ Nieuw** in de linkerbovenhoek wordt weergegeven:

    ![scherm1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. Klik op de **virtuele Machines** in de **Marketplace** vervolgens **Ubuntu Server 14.04 LTS** vanuit **Featured Apps** lijst met afbeeldingen.  Controleer of onder het implementatiemodel is `Resource Manager` en klik vervolgens op **maken**.

    ![--scherm2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. Voer op de pagina **Grondbeginselen** :
    - een naam voor de VM
    - een gebruikersnaam voor de beheerder
    - het Type verificatie ingesteld op **openbare SSH-sleutel**
    - de openbare SSH-sleutel als een tekenreeks (uit de `~/.ssh/` directory)
    - naam van de groep een bron, of Selecteer een bestaande groep

    en klik op **OK** om terug te gaan en kies de grootte van de VM; Deze moet er ongeveer als volgt uit:

    ![screen3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

4. Kies grootte **DS1** Ubuntu op een SSD Premium installeert, en klik op **selecteren** om instellingen te configureren.

    ![screen4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

5. Laat de standaardinstellingen voor opslag- en waarden- **Instellingen**en klik op **OK** om de samenvatting te geven.  Aankondiging van de schijven van het type is ingesteld op Premium SSD DS1 kiest, de **S** notates SSD-technologie.

    ![screen5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

6. Bevestig de instellingen voor uw nieuwe Ubuntu VM en klik op **OK**.

    ![screen6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

7. Open het Dashboard Portal en kies de NIC in **Netwerkinterfaces**

    ![screen7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

8. Open het menu van de openbare IP-adressen onder de NIC-instellingen

    ![screen8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

9. SSH in het openbare IP met uw openbare SSH-sleutel

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Volgende stappen

U hebt nu een Linux VM snel moet worden gebruikt voor tests of demonstratie-doeleinden gemaakt. Aangepast om een Linux VM maken voor uw infrastructuur, kunt u deze artikelen volgen.

- [Een Linux VM in Azure met behulp van sjablonen maken](virtual-machines-linux-cli-deploy-templates.md)
- [Maak een SSH beveiligd Linux VM op Azure met behulp van sjablonen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Maak een Linux VM met behulp van de CLI Azure](virtual-machines-linux-create-cli-complete.md)
