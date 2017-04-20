
<properties
    pageTitle="Beveiligde toepassingen en bronnen in Azure RemoteApp | Microsoft Azure"
    description="Meer informatie over het vergrendelen van toepassingen en bronnen in Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Beveiligde toepassingen en bronnen in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp biedt gebruikers de toegang tot centraal beheerde Windows-toepassingen, kunt u bepalen wat de gebruikers en niet mogelijk.  Dit is vooral handig wanneer de gebruiker verbinding vanaf een apparaat zonder begeleiding (net als hun persoonlijke Macbook maakt) en u wilt de toegang van gebruikers of ervaring.

Bijvoorbeeld, als u Active Directory voor de verificatie van de gebruiker gebruikt en u wilt voorkomen dat gebruikers kopiëren van gegevens uit een app, kunt u een extern bureaublad-Groepsbeleid voorkomen dat gebruikers gegevens kopiëren.

Een ander voorbeeld is als u wilt blokkeren van toegang tot het internet voor een bepaalde toepassing in de collectie. U kunt een Windows Firewall regel maken die de toegang wordt geblokkeerd als u de afbeelding voor uw verzameling.

## <a name="implementation-options"></a>Implementatie-opties

  Hier volgen de belangrijkste implementatieopties, kunnen afzonderlijk of samen naar behoefte worden gebruikt:

1.  Als uw verzameling RemoteApp-domein gekoppeld is, kunt u [Groepsbeleid](https://technet.microsoft.com/library/cc725828.aspx) (met uitzondering van de niet-actief en verbinding verbreken-out beleid beschreven [hier](../azure-subscription-service-limits.md)) afdwingen.
2.  Als alternatief voor Groepsbeleid (als uw verzameling domein gekoppeld of als u niet de juiste bevoegdheden in AD hebt), kunt u [Lokaal beleid](https://technet.microsoft.com/library/cc775702.aspx) in de Sjabloonafbeelding.  Houd er rekening mee dat Groepsbeleid troef lokaal beleid wanneer er een conflict is.
3.  Sommige OS/app instellingen kunnen worden niet geconfigureerd via Groepsbeleid, maar via de registersleutel via het [hulpprogramma RegEdit](./remoteapp-hybridtrouble.md) tijdens het configureren van de Sjabloonafbeelding.
4.  U kunt [Windows Firewall](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) toegang van en naar de computer waarop de toepassing wordt uitgevoerd. Zorg ervoor dat u de URL's en poorten die hier zijn opgegeven niet blokkeren.
5.  [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) kunt u om te bepalen welke toepassingen en gebruikers bestanden kunnen worden uitgevoerd. Ervaren gebruikers kunnen bijvoorbeeld het uitvoeren van toepassingen die u niet heeft gepubliceerd maar die beschikbaar in de afbeelding die u gebruikt zijn voor het maken van de collectie - AppLocker dit blokkeren kunt uitzoeken.

## <a name="detailed-information"></a>Gedetailleerde informatie

- Het volgende beleid van RDS worden waarschijnlijk het meest nuttig zijn:
    - [Apparaat- en bronomleiding](https://technet.microsoft.com/library/ee791794.aspx)
    - [Printeromleiding](https://technet.microsoft.com/library/ee791784.aspx)
    - [Profielen](https://technet.microsoft.com/library/ee791865.aspx).
- Houd er rekening mee dat omleidingen via de RemoteApp PowerShell configureren module (zoals gezien [hier](./remoteapp-redirection.md)) is afhankelijk van de clientcomputer in het beleid wilt afdwingen, zodat als beveiliging de primaire doelstelling is moet u het beleid via de sjabloon afbeelding lokaal beleid of groepsbeleid afdwingen.
- [Beleid voor Windows Server 2012 R2](https://technet.microsoft.com/library/hh831791.aspx).
- [Beleid voor Office 2013](https://technet.microsoft.com/library/cc178969.aspx) (met inbegrip van [de Office werkbalk aanpassen](https://technet.microsoft.com/library/cc179143.aspx)).
