<properties
    pageTitle="Cloud Services Veelgestelde vragen | Microsoft Azure"
    description="Veelgestelde vragen over Cloud diensten."
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
    ms.date="08/19/2016"
    ms.author="adegeo"/>

# <a name="cloud-services-faq"></a>Cloud Services Veelgestelde vragen
Dit artikel bevat antwoorden op enkele veelgestelde vragen over Microsoft Azure Cloud Services. U kunt ook de [Azure Support FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) voor algemene Azure prijzen en ondersteuning voor meer informatie. U kunt ook de [Cloud Services VM pagina](cloud-services-sizes-specs.md) raadplegen voor informatie over de grootte.

## <a name="certificates"></a>Certificaten

### <a name="where-should-i-install-my-certificate"></a>Waar moet ik mijn certificaat installeren?

- **Mijn**  
Toepassing-certificaat met de persoonlijke sleutel (\*.pfx, \*.p12).

- **CERTIFICERINGSINSTANTIE**  
Alle tussenliggende certificaten gaat in deze winkel (beleid en Sub-CA's).

- **HOOFDMAP**  
De basis-CA opslaan, zodat de belangrijkste basis-CA-certificaat moet hier gaan.

### <a name="i-cant-remove-expired-certificate"></a>Ik kan niet verlopen certificaat verwijderen

Azure voorkomt dat u een certificaat verwijderen terwijl deze gebruikt wordt. U moet verwijderen van de distributie die gebruikmaakt van het certificaat of de implementatie bijwerken met een ander of vernieuwde certificaat.

### <a name="delete-an-expired-certificate"></a>Een verlopen certificaat verwijderen

Als het certificaat niet in gebruik is, kunt u de PowerShell-cmdlet [Verwijderen AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) voor het verwijderen van een certificaat.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>Ik verlopen certificaten met de naam Windows Azure Service Management voor extensies

Deze certificaten worden gemaakt wanneer een verlenging wordt toegevoegd aan de cloud service zoals de extern bureaublad-extensie. Deze certificaten worden alleen gebruikt voor het coderen en decoderen van de persoonlijke configuratie van de uitbreiding. Het maakt niet uit als deze certificaten is verlopen. De vervaldatum is niet ingeschakeld.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Certificaten die hebt verwijderd blijven verschijnen.

Deze terugkomen waarschijnlijk omdat een programma dat u, zoals Visual Studio gebruikt. Wanneer u opnieuw verbinding met een hulpprogramma waarmee u een certificaat gebruikt maakt, wordt deze opnieuw geüpload naar Azure.

### <a name="my-certificates-keep-disappearing"></a>Mijn certificaten laten verdwijnen.

Wanneer het exemplaar van de virtuele machine wordt gerecycled, worden alle lokale wijzigingen verloren gaan. Een [taak starten](cloud-services-startup-tasks.md) gebruiken certificaten installeren op de virtuele machine telkens wanneer die de rol wordt gestart.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>Ik kan mijn certificaten beheren niet vinden in de portal

[Beheer van certificaten](..\azure-api-management-certs.md) zijn alleen beschikbaar in de klassieke Azure-Portal. De huidige Azure portal gebruikt geen certificaten beheren. 

### <a name="how-can-i-disable-a-management-certificate"></a>Hoe kan ik een certificaat beheer uitschakelen?

[Beheer van certificaten](..\azure-api-management-certs.md) kan niet worden uitgeschakeld. U verwijderen ze via de klassieke Azure-Portal wanneer u niet wilt dat ze niet meer worden gebruikt.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Hoe maak ik een SSL-certificaat voor een specifiek IP-adres

Volg de aanwijzingen in de [zelfstudie voor een certificaat te maken](cloud-services-certs-create.md). Het IP-adres als de DNS-naam gebruiken.

## <a name="security"></a>Beveiliging

### <a name="disable-ssl-30"></a>SSL 3.0 uitschakelen

Als u wilt uitschakelen van SSL 3.0 en TLS-beveiliging, opstarten op een taak maken die wordt beschreven in dit blogbericht: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

## <a name="scale-a-cloud-service"></a>Schaal een cloud-service

### <a name="i-cannot-scale-beyond-x-instances"></a>Ik kan geen schalen buiten X exemplaren

Uw abonnement Azure heeft een limiet op het aantal kernen die u kunt gebruiken. Schalen, werken niet als u de beschikbare cores hebt gebruikt. Bijvoorbeeld als er een limiet van 100 cores, hierdoor kan er exemplaren van de virtuele machine 100 A1 formaat voor uw service cloud of 50 A2 formaat exemplaren van de virtuele machine.

## <a name="troubleshooting"></a>Het oplossen van problemen

### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Ik kan niet een IP in een multi-VIP-service cloud reserveren

Controleer eerst of de virtuele machine-exemplaar dat u wilt reserveren, het onderzoektijdvak voor is ingeschakeld. Controleer vervolgens of dat u IP-adressen gereserveerd voor bAndere implementaties van de ontwikkel- en productiecomputers. **Niet** wijzigen de instellingen, terwijl het upgraden van de implementatie.

