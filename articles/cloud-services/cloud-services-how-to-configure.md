<properties 
    pageTitle="Het configureren van een cloud-service (klassieke portal) | Microsoft Azure" 
    description="Informatie over het cloud-services configureren in Azure. Informatie over de configuratie van de service cloud bijwerken en configureren van externe toegang tot de rol van exemplaren." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-configure-cloud-services"></a>Cloud-Services configureren

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-configure-portal.md)
- [Azure klassieke portal](cloud-services-how-to-configure.md)

In de klassieke Azure portal kunt u de meestgebruikte instellingen voor een cloud-service. Of als u direct bijwerken van de configuratiebestanden, downloaden van een bestand van de service-configuratie wilt bijwerken, het bijgewerkte bestand uploaden en de service cloud bijwerken met wijzigingen in de configuratie. In beide gevallen is de configuratie-updates naar buiten wijzen voor alle exemplaren van de rol.

De klassieke Azure portal kunt u ook [Verbinding met extern bureaublad voor een rol in Azure Cloud Services](cloud-services-role-enable-remote-desktop.md) inschakelen

Azure alleen ervoor 99.95% beschikbaarheid van de service tijdens het bijwerken van de configuratie hebt u ten minste twee exemplaren van de rol voor elke rol. Waarmee een virtuele machine voor het verwerken van clientaanvragen terwijl het andere wordt bijgewerkt. Zie voor meer informatie, [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Een cloud-service wijzigen

1. In [Azure klassieke portal](http://manage.windowsazure.com/) **Cloud Services**klikt u op, klikt u op de naam van de cloud-service en klik vervolgens op **configureren**.

    ![De configuratiepagina](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
    
    Op de pagina **configureren** kunt u controle configureren, instellingen bijwerken en kiest u het gastbesturingssysteem en familie voor exemplaren van de rol. 

2. Het niveau van toezicht ingesteld op uitgebreid of minimale **controle**, en tekenreeksen voor de diagnostische gegevens die vereist voor een uitgebreide controle zijn configureren.

3. Voor service rollen (gegroepeerd per rol), kunt u de volgende instellingen bijwerken:
    
    >**Instellingen**  
    >Wijzig de waarden van diverse configuratie-instellingen die zijn opgegeven in de *ConfigurationSettings* elementen van het service-configuratiebestand (.cscfg).
    >
    >**Certificaten**  
    >De vingerafdruk van het certificaat voor SSL-versleuteling wordt gebruikt voor een rol wijzigen. Als u wilt een certificaat wijzigt, moet u eerst het nieuwe certificaat (op de pagina **certificaten** ) uploaden. De vingerafdruk in de certificaattekenreeks weergegeven in de functie-instellingen bijwerken.

4. U kunt in het **besturingssysteem**, de familie besturingssysteem of de versie voor rol exemplaren wijzigen of kiezen **automatisch** inschakelen van automatische updates van de huidige versie van het besturingssysteem. De instellingen van toepassing op het web-rollen en functies van de werknemer, maar hebben geen invloed op de virtuele Machines.

    De meest recente versie van het besturingssysteem is geïnstalleerd op alle exemplaren van de rol en het besturingssysteem standaard automatisch worden bijgewerkt tijdens de implementatie. 
    
    Als u nodig hebt voor uw cloud-service worden uitgevoerd op een ander besturingssysteemversie door compatibiliteitseisen in uw code, kunt u een familie van besturingssysteem en de versie. Wanneer u een bepaald besturingssysteemversie kiest, worden updates automatische besturingssysteem voor de service cloud geschorst. U moet ervoor zorgen dat de besturingssystemen updates ontvangen.
    
    Als u alle problemen met de compatibiliteit met uw apps met de meest recente versie van het besturingssysteem oplossen, kunt u automatische besturingssysteem-updates inschakelen door de versie van het besturingssysteem in te stellen op **automatisch**. 
    
    ![OS-instellingen](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)

5. Om de configuratie-instellingen opslaan en deze push naar de exemplaren van de rol, klikt u op **Opslaan**. (Klik op **Annuleren** om de wijzigingen te annuleren.) **Opslaan** en **verwijderen** worden aan de opdrachtbalk toegevoegd nadat u een instelling wijzigt.

## <a name="update-a-cloud-service-configuration-file"></a>Een configuratiebestand cloud service bijwerken

1. Download een cloud service configuratiebestand (.cscfg) met de huidige configuratie. Klik op de pagina **configureren** voor de service cloud op **downloaden**. Vervolgens klikt u op **Opslaan**, of klik op **OpslaanAls** u het bestand wilt opslaan.

2. Na het bijwerken van het configuratie-bestand uploaden en de configuratie-updates:

    1. Klik op de pagina **configureren** op **uploaden**.
    
        ![Configuratie uploaden](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
    
    2. Gebruik **Bladeren** de bijgewerkte .cscfg-bestand te selecteren in het **configuratiebestand**.
    
    3. Als uw service cloud alle functies die slechts één exemplaar bevat hebben, het selectievakje **configuratie toepassen, zelfs als een of meer rollen een enkel exemplaar bevatten** waarmee de configuratie-updates voor de rollen om door te gaan.
    
        Tenzij u ten minste twee exemplaren van elke rol definieert, kan niet garanderen Azure ten minste 99.95% beschikbaarheid van de service cloud tijdens het bijwerken van de configuratie van service. Zie voor meer informatie, [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).
    
    4. Klik op **OK** (vinkje). 


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [implementeren van een cloud-service](cloud-services-how-to-create-deploy.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name.md)configureren.
* [Uw cloud-service beheren](cloud-services-how-to-manage.md).
* [Verbinding met extern bureaublad voor een rol in Azure Cloud Services inschakelen](cloud-services-role-enable-remote-desktop.md)
* [Ssl-certificaten](cloud-services-configure-ssl-certificate.md)configureren.
