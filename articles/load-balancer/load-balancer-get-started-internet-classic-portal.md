
<properties
   pageTitle="Aan de slag maken van een internetverbinding taakverdeling in het klassieke implementatiemodel met behulp van de klassieke Azure portal | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding taakverdeling in het klassieke implementatiemodel met behulp van de klassieke Azure portal"
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
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Een internetverbinding (klassiek)-taakverdeling in de klassieke Azure portal maken

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op de klassieke implementatiemodel. U kunt ook [informatie over het maken van een internetverbinding met behulp van bronbeheer Azure taakverdeling](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Instellen van een internetverbinding taakverdeling voor virtuele machines

Om te kunnen laden saldo netwerkverkeer van het Internet via de virtuele machines van een cloud-service, moet u een set verdeeld. Deze procedure wordt ervan uitgegaan dat u al de virtuele machines zijn gemaakt en dat ze allemaal in dezelfde cloud-service zijn.

**Een taakverdeling instellen voor virtuele machines configureren**

1. In de klassieke Azure portal op **virtuele Machines**en klik vervolgens op de naam van een virtuele machine in de taakverdeling.

2. **Eindpunten**op en klik vervolgens op **toevoegen**.

3. Klik op de pijl-rechts op de pagina **toevoegen een eindpunt op een virtuele machine** .

4. Op de pagina **Geef de details van het eindpunt** :

    * Typ een naam voor het eindpunt in het vak **naam**of Selecteer de naam van de lijst met vooraf gedefinieerde eindpunten voor gemeenschappelijke protocollen.
    * Selecteer in het **Protocol**, het protocol dat vereist is voor het type eindpunt, TCP of UDP, indien nodig.
    * Typ bij **poort openbare en particuliere poort**de poortnummers die u wilt dat de virtuele machine te gebruiken, indien nodig. U kunt privé poort en firewall-regels op de virtuele machine omgeleid verkeer op een manier die geschikt is voor uw toepassing. De private poort kan hetzelfde zijn als de openbare poort. Voor een eindpunt voor het webverkeer (HTTP), kunt u poort 80 kan toewijzen aan de openbare en particuliere poort.

5. Selecteer **een set met evenwichtige taakverdeling maken**en klik vervolgens op de pijl-rechts.

6. Op de pagina **de set taakverdeling configureren** , typ een naam voor de set verdeeld en vervolgens toewijzen de waarden voor het gedrag van de Load Balancer voor Azure sonde. De Load Balancer wordt sondes gebruikt om te bepalen of de virtuele machines in de reeks verdeeld beschikbaar zijn voor het binnenkomende verkeer wordt ontvangen.

7. Klik op het selectievakje om het eindpunt met evenwichtige taakverdeling maken. Ziet u **Ja** in de kolom **verdeeld de naam** van de pagina **Tunneleindpunten** voor de virtuele machine.

8. Klik op **virtuele Machines**, klik op de naam van een extra virtuele machine in de reeks verdeeld, klikt u op de **eindpunten**en klik op **toevoegen**in de portal.

9. **Eindpunt toevoegen aan een bestaande set van taakverdeling**klikt u op op de pagina **toevoegen een eindpunt op een virtuele machine** , selecteert u de naam van de set verdeeld en klikt u vervolgens op de pijl-rechts.

10. Typ een naam voor het eindpunt en klik op het vinkje op de pagina **Geef de details van het eindpunt** .

Herhaal stap 8-10 voor de aanvullende virtuele machines in de taakverdeling.



## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)

