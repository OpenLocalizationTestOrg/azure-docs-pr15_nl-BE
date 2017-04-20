<properties
   pageTitle="Inschakelen of uitschakelen van Azure VM Monitoring"
   description="Beschrijving van het in- of uitschakelen van Azure VM controleren"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="enable-or-disable-azure-vm-monitoring"></a>In- of uitschakelen van Azure VM controleren

In deze sectie wordt beschreven hoe in- of uitschakelen van controle op virtuele machines waarop Azure. Controle is standaard ingeschakeld op Azure Virtual machines als vanuit de [portal Azure](https://portal.azure.com) geïmplementeerd en monitoring grafieken worden geleverd met een periode van 1 minuut standaard. U kunt inschakelen of uitschakelen, controle in de portal Azure opdrachtregelinterface voor Mac, Linux en Windows (de Azure CLI). 

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Inschakelen / uitschakelen, controle via de Portal Azure
 
U kunt controle van uw Azure VM, waarmee gegevens over uw exemplaar in periodes van 1 minuut. (wijzigingen van de opslag van toepassing). Gedetailleerde diagnostische gegevens zijn toegankelijk voor de VM in de grafieken portal of via de API. Azure portal kan controleren, maar u kunt deze uitschakelen zoals hieronder beschreven. U kunt inschakelen terwijl de VM is gestart of in staat gestopt te controleren.

- Open de Azure portal op ** [https://portal.azure.com](https://portal.azure.com)**

- Klik in de linkernavigatiebalk op virtuele machines.

- Selecteer in de lijst met virtuele machines, een instantie uitgevoerd of is gestopt. Blad van de virtuele machine wordt geopend.

- Klik op 'Alle instellingen'.

- Klik op 'Diagnostische gegevens'.

- Status wijzigen om in- of uitschakelen. U kunt ook kiezen in deze blade toezicht details die u wilt inschakelen voor uw virtuele machine.

[Azure.Note] De schakeloptie diagnostiek op is de standaardinstelling wanneer u een nieuwe virtuele machine maken

![Inschakelen / uitschakelen, controle via de Portal Azure.][1]


## <a name="enable--disable-monitoring-with-azure-cli"></a>Inschakelen / uitschakelen, controle met Azure CLI
 
Inschakelen van controle voor een Azure VM.

- Maak een bestand met de naam bijvoorbeeld PrivateConfig.json met de volgende inhoud.
        {"storageAccountName": "de opslag account om gegevens te ontvangen", "storageAccountKey": "de sleutel van de account"}
- Voer de volgende Azure CLI-opdracht.

        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] Kunt u van versie 2.0 naar een nieuwere versie, indien beschikbaar. 

Voor meer informatie over het configureren van controle maatstaven en monsters, gaat u naar het document - **[Met behulp van Linux diagnostische uitbreiding van prestaties en diagnostische gegevens van Monitor Linux VM](virtual-machines-linux-classic-diagnostic-extension.md).

<!--Image references-->
[1]: ./media/virtual-machines-linux-vm-monitoring/portal-enable-disable.png
 

