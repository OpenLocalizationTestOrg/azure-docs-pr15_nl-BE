<properties
    pageTitle="Meld u aan bij een klassieke Azure VM | Microsoft Azure"
    description="De Azure klassieke portal gebruiken voor aanmelding bij een virtuele Windows-computer met het klassieke implementatiemodel gemaakt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>


# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-classic-portal"></a>Aanmelden bij een virtuele Windows-computer met behulp van de klassieke Azure portal

In de klassieke Azure portal kunt u de knop **verbinding maken met** een extern bureaublad-sessie starten en aanmelden bij een Windows VM.

Wilt u verbinding maken met een VM Linux? Zie [aanmelden bij een virtuele machine waarop Linux wordt uitgevoerd](virtual-machines-linux-mac-create-ssh-keys.md).

Meer informatie over hoe [u deze stappen uitvoert met behulp van nieuwe Azure portal](virtual-machines-windows-connect-logon.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] 

## <a name="video-walkthrough"></a>Video overzicht

Hier is een video-overzicht van de stappen in deze zelfstudie. Omvat tevens eindpunten en openbare en particuliere poorten gebruikt voor verbinding met een Windows VM in Azure.

[AZURE.VIDEO logging-on-to-vm-running-windows-server-on-azure]


## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

1. Log in om de klassieke Azure portal.

2. Klik op de **virtuele Machines**en selecteer vervolgens de virtuele machine.

3. In de werkbalk onder aan de pagina, klikt u op **verbinding maken**.

    ![Meld u aan bij de virtuele machine](./media/virtual-machines-windows-classic-connect-logon/connectwindows.png)
    
> [AZURE.TIP] Als de knop **verbinding maken** niet beschikbaar is, raadpleegt u de tips aan het einde van dit artikel.

## <a name="log-on-to-the-virtual-machine"></a>Meld u aan bij de virtuele machine

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Volgende stappen

-   Als de knop **verbinden** niet actief is of u andere met de verbinding met extern bureaublad problemen, probeer het opnieuw instellen van de configuratie. Uit het dashboard virtuele machine **Snel bekijken**, klik op **externe configuratie te herstellen**.
-   Voor problemen met uw wachtwoord, probeer het opnieuw instellen. Uit het dashboard virtuele machine **Snel bekijken**, klik op **wachtwoord opnieuw instellen**.

Als deze tips niet werkt of niet zijn wat u nodig hebt, raadpleegt u [problemen met extern bureaublad-verbindingen met een Windows Azure virtuele Machine](virtual-machines-windows-troubleshoot-rdp-connection.md). Dit artikel begeleidt u door het opsporen en oplossen van problemen.


