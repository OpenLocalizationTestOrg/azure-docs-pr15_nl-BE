<properties
    pageTitle="Azure verkeer profielen beheren | Microsoft Azure"
    description="Dit artikel helpt u bij het maken, uitschakelen, inschakelen, verwijderen en de geschiedenis van een Azure verkeer Manager profiel weergeven."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="manage-an-azure-traffic-manager-profile"></a>Een serviceprofiel Azure verkeer beheren

Routering van verkeer methoden verkeer profielen gebruiken om de verspreiding van verkeer naar de cloud-services of eindpunten website. Dit artikel wordt beschreven hoe u kunt maken en beheren van deze profielen.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Maak een verkeer Manager profiel met behulp van snelle invoer

Met behulp van snelle invoer in de klassieke Azure portal kunt u snel een verkeer Verbindingsbeheer-profiel maken. Snelle invoer kunt u profielen maken met de basisconfiguratie-instellingen. Echter, kunt u snel maken voor instellingen, zoals het instellen van de eindpunten (cloud services en websites), de volgorde voor failover voor de routeringsmethode voor failover-verkeer of controle-instellingen. Nadat u uw profiel hebt, kunt u deze instellingen configureren in de klassieke Azure portal. Beheer van verkeer ondersteunt maximaal 200 eindpunten per profiel. Meeste gebruiksscenario's vereisen echter een aantal eindpunten.

### <a name="to-create-a-traffic-manager-profile"></a>Voor het maken van een serviceprofiel verkeer

1. **Cloud services en websites implementeren in uw productieomgeving.** Zie voor meer informatie over cloud services, [Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=314074). Zie voor meer informatie over websites, [Websites](http://go.microsoft.com/fwlink/p/?LinkId=393327).

2. **Log in op de klassieke Azure portal.** Klik op **Nieuw** op de linkerbenedenhoek van de portal, **Netwerkservices > verkeer Manager**, en klik vervolgens op **Snelle invoer** om te beginnen met het configureren van uw profiel.
3. **Configureer het DNS-voorvoegsel.** Geef uw verkeer manager profiel een unieke DNS voorvoegsel van de naam. U kunt alleen het voorvoegsel voor een domeinnaam Manager verkeer opgeven.
4. **Selecteer het abonnement.** Selecteer het juiste abonnement Azure. Elk profiel is gekoppeld aan een enkel abonnement. Als u alleen een abonnement hebt, wordt deze optie niet weergegeven.
5. **Selecteer de routeringsmethode verkeer.** Selecteer de routeringsmethode verkeer in het **verkeer routing beleid**. Zie voor meer informatie over methoden voor het doorsturen van verkeer [over het beheer van verkeer verkeer routeringsmethoden](traffic-manager-routing-methods.md).
6. **Klik op 'Maken' om het profiel te maken**. Wanneer de configuratie is voltooid, kunt u uw profiel vinden in het deelvenster Beheer van verkeer in de klassieke Azure portal.
7. **Eindpunten, controle en extra instellingen configureren in de klassieke Azure portal.** Met behulp van snelle invoer alleen elementaire instellingen geconfigureerd. Het is noodzakelijk voor het configureren van aanvullende instellingen, zoals de lijst met eindpunten en de volgorde van de failover-eindpunt.


## <a name="disable-enable-or-delete-a-profile"></a>Uitschakelen, inschakelen of verwijderen van een profiel

U kunt een bestaand profiel uitschakelen zodat deze Manager verkeer aanvragen van gebruikers niet naar de geconfigureerde eindpunten verwijst. Wanneer u een serviceprofiel verkeer uitschakelt, het profiel en de informatie in het profiel blijft intact en kunnen worden bewerkt in de interface van de Manager van verkeer.  Verwijzingen hervat wanneer u het profiel opnieuw in te schakelen. Wanneer u een serviceprofiel verkeer in de klassieke Azure portal maakt, wordt deze automatisch ingeschakeld. Als u dat een profiel nodig is, kunt u deze verwijderen.

### <a name="to-disable-a-profile"></a>Een profiel uitschakelen

1. Als u een aangepaste domeinnaam wijzigen de CNAME-record op de Internet-DNS-server zodanig dat deze niet langer aan uw profiel verkeer Manager wijst.
2. Verkeer gestopt wordt vervolgens naar de eindpunten via de instellingen van het beheer van verkeer.
3. Selecteer het profiel dat u wilt uitschakelen. Selecteer het profiel door te klikken op de kolom naast de naam van het profiel op de pagina beheer van verkeer. Notitie, de naam van het profiel of de pijl naast de naam te klikken opent de instellingenpagina voor het profiel.
4. Selecteer het profiel en klik op **uitschakelen** onder aan de pagina.

### <a name="to-enable-a-profile"></a>Om een profiel

1. Selecteer het profiel dat u wilt uitschakelen. Selecteer het profiel door te klikken op de kolom naast de naam van het profiel op de pagina beheer van verkeer. Notitie, de naam van het profiel of de pijl naast de naam te klikken opent de instellingenpagina voor het profiel.
2. Selecteer het profiel en klik op **inschakelen** onder aan de pagina.
3. Als u een aangepaste domeinnaam gebruikt, maakt u een CNAME-bronrecord op de Internet-DNS-server om te verwijzen naar de domeinnaam van uw serviceprofiel verkeer.
4. Verkeer wordt omgeleid naar de eindpunten opnieuw.

### <a name="to-delete-a-profile"></a>Een profiel verwijderen

1. Zorg ervoor dat de DNS-bronrecord op de Internet-DNS-server geen gebruik maakt van een CNAME-bronrecord die naar de domeinnaam van uw serviceprofiel verkeer verwijst.
2. Selecteer het profiel dat u wilt uitschakelen. Selecteer het profiel door te klikken op de kolom naast de naam van het profiel op de pagina beheer van verkeer. Notitie, de naam van het profiel of de pijl naast de naam te klikken opent de instellingenpagina voor het profiel.
3. Selecteer het profiel en klik op **verwijderen** onder aan de pagina.

## <a name="view-traffic-manager-profile-change-history"></a>Wijzigingsoverzicht verkeer Weergavebeheer-profiel

U kunt het wijzigingsoverzicht weergeven voor uw serviceprofiel verkeer in de Azure klassieke portal in Management Services.

### <a name="to-view-your-traffic-manager-change-history"></a>Uw Manager verkeer wijzigingsoverzicht weergeven

1. Klik in het linkerdeelvenster van de klassieke Azure portal **Management Services**.
2. Klik op de pagina Services voor **Logboeken**.
3. U kunt filteren op de pagina bewerking Logboeken om het wijzigingsoverzicht voor uw serviceprofiel verkeer weer te geven. Na het selecteren van opties voor het filter, klikt u op het vinkje om de resultaten te bekijken.

   - De wijzigingen wilt bekijken voor al uw profielen, selecteer uw abonnement en tijdsbereik en **Verkeer Manager** selecteert in het snelmenu van het **Type** .
   - Als u wilt filteren op naam, typ de naam van het profiel in het veld **Naam van de Service** of selecteren in het snelmenu.
   - Om details te bekijken voor elke afzonderlijke wijziging, selecteert u de rij met de wijziging die u wilt weergeven en klik op **Details** onder aan de pagina. U kunt de XML-representatie van de API-object dat is gemaakt of bijgewerkt als onderdeel van de bewerking weergeven in het venster **Details van de bewerking** .

## <a name="next-steps"></a>Volgende stappen

- [Een eindpunt toevoegen](traffic-manager-endpoints.md)
- [Failover-methode configureren](traffic-manager-configure-failover-routing-method.md)
- [Methode round robin configureren](traffic-manager-configure-round-robin-routing-method.md)
- [Prestaties routeringsmethode configureren](traffic-manager-configure-performance-routing-method.md)
- [Een bedrijf internetdomein wijst u een domeinnaam voor het beheer van verkeer](traffic-manager-point-internet-domain.md)
- [Probleemoplossing verkeer Manager verslechterd staat](traffic-manager-troubleshooting-degraded.md)