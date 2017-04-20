<properties
   pageTitle="Aan de slag maken van een interne taakverdeling in Resource Manager, met behulp van de portal Azure | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling in Resource Manager, met behulp van de portal Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a>Een interne taakverdeling in Azure portal maken

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Aan de slag met Azure portal-een interne taakverdeling maakt

Gebruik de volgende stappen voor het maken van een interne taakverdeling van de Portal Azure.

1. Een browser opent, gaat u de [Azure portal](http://portal.azure.com)en meld u aan met uw account Azure.
2. Klik op **Nieuw**in het bovenste linker kant van het scherm, > **netwerk** > **Load balancer**.
3. Voer een **naam** voor de verdeling van de belasting in de blade **maken de belasting voor documentconversies** .
4. Klik onder de **regeling** **intern**.
5. Klik op **virtuele netwerk**en selecteer het virtuele netwerk waar u de taakverdeling te maken.

    >[AZURE.NOTE] Als u het virtuele netwerk dat u wilt gebruiken niet ziet, Controleer de **locatie** u gebruikt voor de taakverdeling en dienovereenkomstig wijzigen.

6. Klik op **Subnet**, en selecteer vervolgens het subnet waar u de taakverdeling te maken.
7. Klik onder **IP-adrestoewijzing**, **dynamische** of **statische**, afhankelijk van of u het IP-adres voor de taakverdeling vast (statisch) of niet.

    >[AZURE.NOTE] Als u een statisch IP-adres gebruiken selecteert, moet u een adres opgeven voor de taakverdeling.

8. Onder **resourcegroep** de naam opgeven van een nieuwe bronnengroep voor de taakverdeling of klikt u op **bestaande selecteren** en selecteert u een bestaande brongroep.
9. Klik op **maken**.

## <a name="configure-load-balancing-rules"></a>Regels voor taakverdeling configureren

Na het maken van load balancer, Ga naar de resource load balancer te configureren.
U moet eerst een back-end-adresgroep en configureren een sonde voordat u een regel voor taakverdeling configureren.

### <a name="step-1-configure-a-back-end-pool"></a>Stap 1: Een back-end-toepassingen configureren

1. Klik op **Bladeren**in het portal voor Azure > **netwerktaakverdelers**, en klik vervolgens op de verdeling van de belasting die u hierboven hebt gemaakt.
2. Klik op **back-end-toepassingen**in de blade **Instellingen** .
3. Klik op **toevoegen**in het blad **Backend-adresgroepen** .
4. Voer een **naam** voor de back-end-toepassingen in de blade **backend-groep toevoegen** en klik op **OK**.

### <a name="step-2-configure-a-probe"></a>Stap 2: Een sonde configureren

1. Klik op **Bladeren**in het portal voor Azure > **netwerktaakverdelers**, en klik vervolgens op de verdeling van de belasting die u hierboven hebt gemaakt.
2. Klik in de blade **Instellingen** op **sondes**.
3. Klik op **toevoegen**in het blad **sondes** .
4. Voer een **naam** voor de sonde in de blade **sonde toevoegen** .
5. Selecteer onder **Protocol** **HTTP** (voor websites) of **TCP** (voor andere toepassingen op basis van TCP).
6. Geef onder **poort**, de poort moet worden gebruikt bij het benaderen van de sonde.
7. Geef het pad moet worden gebruikt als een sonde onder **pad** (voor HTTP-sondes).
8. Opgeven hoe vaak de toepassing probe onder **Interval** .
9. Geef onder **drempel beschadigd**, hoeveel pogingen mag mislukken voordat de back-end virtuele machine wordt gemarkeerd als beschadigd.
10. Klik op **OK** om het maken van de sonde.

### <a name="step-3-configure-load-balancing-rules"></a>Stap 3: Regels voor taakverdeling configureren

1. Klik op **Bladeren**in het portal voor Azure > **netwerktaakverdelers**, en klik vervolgens op de verdeling van de belasting die u hierboven hebt gemaakt.
2. Klik op **Load balancing regels**in de blade **Instellingen** .
3. Klik op **toevoegen**in het blad **Load balancing regels** .
4. Voer een **naam** voor de regel in het blad **toevoegen balancing regel geladen** .
5. Selecteer onder **Protocol** **HTTP** (voor websites) of **TCP** (voor andere toepassingen op basis van TCP).
6. Geef onder **poort**, dat de poort-clients verbinding maken met in de taakverdeling.
7. Geef onder **back-end-poort**, de poort moet worden gebruikt in de mediagroep Back-end (meestal de load balancer poort en de back-end-poort zijn gelijk).
8. Selecteer de back-end-toepassingen die u hiervoor hebt gemaakt onder de **back-end-toepassingen**.
9. Selecteer onder **sessie persistentie**, hoe sessies aanhouden.
10. Geef de time-out voor inactiviteit onder **time-out voor inactiviteit (minuten)**.
11. **Zwevende IP (directe server return)**, klik op **uitgeschakeld** of **ingeschakeld**.
12. Klik op **OK**.

## <a name="next-steps"></a>Volgende stappen

[Een load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)