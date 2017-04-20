<properties
   pageTitle="Aan de slag met Azure logboek integratie | Microsoft Azure"
   description="Informatie over het installeren van de integratie van de Azure logboekservice en logboeken van Azure opslag, Azure controlelogboeken en meldingen van Beveiligingscentrum Azure integreren."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="get-started-with-azure-log-integration-preview"></a>Aan de slag met Azure logboek integratie (voorbeeld)

Integratie van de Azure logboek kunt u ruwe logboeken van uw resources Azure integreren in uw systemen van beveiligingsinformatie en gebeurtenis Management (SIEM) in ruimten. Deze integratie garandeert een consistente dashboard voor alle activa, in ruimten of in de cloud, zodat verzamelen, correleren, analyseren en voor beveiligingsgebeurtenissen die zijn gekoppeld aan uw toepassingen waarschuwen.

Deze zelfstudie begeleidt u bij de integratie van de Azure logboek installeren en integreren van Logboeken van Azure opslag, Azure controlelogboeken en meldingen van Beveiligingscentrum Azure. Geschatte tijd voor het voltooien van deze zelfstudie is één uur.

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie hebt u het volgende:

- Een machine (in ruimten of in de cloud) integratie van de Azure logboekservice installeren. Deze computer moet een 64-bits Windows-besturingssysteem worden uitgevoerd met .net 4.5.1 geïnstalleerd. Deze computer wordt de **Azlog Integrator**genoemd.
- Abonnement op Azure. Als u dit niet hebt nog, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
- Azure diagnostische gegevens is ingeschakeld voor uw Azure virtual machines (VMs). Zie diagnostische gegevens inschakelen voor Cloud Services [Azure Diagnostics inschakelen in Azure Cloud Services](../cloud-services/cloud-services-dotnet-diagnostics.md). Zie diagnostische gegevens inschakelen voor een Azure VM met Windows [PowerShell gebruiken om diagnostische in een virtuele Machine wordt uitgevoerd Windows Azure](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md).
- Verbinding hebben met de Azlog-Integrator naar Azure opslag te verifiëren en autoriseren met Azure abonnement.
- Voor Azure VM zich aanmeldt, moet de agent SIEM (bijvoorbeeld Splunk universele doorstuurserver, HP ArcSight Windows Event Collector-agent of IBM QRadar WinCollect) zijn geïnstalleerd op de Azlog-Integrator.

## <a name="deployment-considerations"></a>Aandachtspunten bij de implementatie

Als het volume van de gebeurtenis hoog is, kunt u meerdere exemplaren van de Integrator Azlog uitvoeren. Taakverdeling van Azure diagnostische gegevens opslag accounts voor Windows *(af)* en het aantal abonnementen voor de exemplaren moet worden gebaseerd op de capaciteit van de.

Op een computer (core) 8-processor kan slechts één exemplaar van een Integrator van Azlog gebeurtenissen ongeveer 24 miljoen per dag (~1M/hour) verwerken.

Op een computer 4-processor (core) kan slechts één exemplaar van een Integrator van Azlog gebeurtenissen ongeveer 1,5 miljoen per dag (~62.5K/hour) verwerkt.

## <a name="install-azure-log-integration"></a>Integratie van de Azure logboek installeren

Download [Azure integratie melden](https://www.microsoft.com/download/details.aspx?id=53324).

Integratie van de Azure logboekservice verzamelt telemetriegegevens van de computer waarop Netwerkcontrole is geïnstalleerd.  Telemetriegegevens verzameld zijn:

- Uitzonderingen die zich tijdens het uitvoeren van de integratie van de Azure logboek voordoen
- Statistieken over het aantal query's en verwerkte gebeurtenissen
- Statistieken over welke Azlog.exe opdrachtregelopties worden gebruikt

> [AZURE.NOTE] U kunt verzameling telemetriegegevens door het uitschakelen van deze optie uitschakelen.

## <a name="integrate-azure-vm-logs-from-your-azure-diagnostics-storage-accounts"></a>Logboeken van uw accounts Azure diagnostische gegevens opslag Azure VM integreren

1. Controleer de hierboven om ervoor te zorgen dat uw account af opslag logboeken verzamelen voordat u doorgaat met de integratie van de Azure logboek vermelde voorwaarden. Voer de volgende stappen niet uit als je account af opslag niet van Logboeken verzamelen is.

2. Open de opdrachtprompt en **cd** in **c:\Program Files\Microsoft Azure logboek integratie**.

3. De opdracht uitvoeren

        azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>

      StorageAccountName vervangen door de naam van de account Azure opslag is geconfigureerd voor het ontvangen van diagnostische gebeurtenissen van uw VM.

        azlog source add azlogtest WAD azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==

      Als u de abonnement-id dat wilt moet worden weergegeven in het logboek XML, moet u de abonnement-ID toevoegen aan de beschrijvende naam:

        azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>

4. Wacht 30-60 minuten (dit kan duren een uur) en de gebeurtenissen weergeven die worden opgehaald uit de opslag-account. Als u wilt bekijken, opent u **Logboeken > Windows-Logboeken > doorgestuurde gebeurtenissen** op de Azlog-Integrator.

5. Zorg ervoor dat uw standaard SIEM connector geïnstalleerd op de computer is geconfigureerd voor de gebeurtenissen uit de map **Doorgestuurde gebeurtenissen** selecteren en ze aan uw exemplaar van SIEM pipe. Bekijk de specifieke configuratie van SIEM als u wilt configureren en het integreren van Logboeken.

## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Wat gebeurt er als gegevens, wordt niet weergegeven in de map doorgestuurde gebeurtenissen?

Als na een uur gegevens, niet in de map **Doorgestuurde gebeurtenissen** klikt weergegeven wordt:

1. De computer en controleer of deze toegang hebt tot Azure. Connectiviteit testen probeert de [Azure portal](http://portal.azure.com) openen vanuit de browser.

2. Zorg ervoor dat de gebruiker rekening **azlog** heeft schrijfmachtiging voor de map **users\azlog**.

3. Controleer of de opslag account toegevoegd aan de opdracht **bron azlog toevoegen** wordt weergegeven tijdens het uitvoeren van de opdracht **lijst azlog**.
4. Ga naar **Logboeken > Windows-Logboeken > toepassing** om te zien of er fouten zijn gerapporteerd door de integratie van de Azure logboek.

Als u nog steeds niet, vinden de gebeurtenissen vervolgens ziet:

1. Download [Microsoft Azure Opslagverkenner](http://storageexplorer.com/).

2. Verbinding maken met de opslag account toegevoegd aan de opdracht **bron azlog toevoegen**.

3. In Opslagverkenner Microsoft Azure, bladert u naar de tabel **WADWindowsEventLogsTable** om te zien of alle gegevens. Als dat niet het geval is, vervolgens diagnostics in VM is niet correct geconfigureerd.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integratie van Azure controlelogboeken en meldingen van Beveiligingscentrum

1. Open de opdrachtprompt en **cd** in **c:\Program Files\Microsoft Azure logboek integratie**.

2. De opdracht uitvoeren

        azlog createazureid

      Met deze opdracht vraagt u om uw aanmelding Azure. De opdracht maakt vervolgens een [Azure Active Directory Service Principal](../active-directory/active-directory-application-objects.md) in de Azure AD huurders die host zijn van de Azure abonnementen waarin de aangemelde gebruiker een beheerder of een CO-beheerder een eigenaar is. De opdracht werkt niet als de aangemelde gebruiker een gastgebruiker in de huurder Azure AD is. Verificatie van Azure is gedaan via Azure Active Directory (AD).  Een service principal voor integratie met Azlog maken maakt de Azure AD identiteit die toegang tot het lezen van Azure abonnementen wordt gegeven.

3. De opdracht uitvoeren

        azlog authorize <SubscriptionID>

      Dit wijst lezerstoegang voor het abonnement op de service principal gemaakt in stap 2. Als u een SubscriptionID, vervolgens probeert toe te wijzen, de service principal lezersrol alle abonnementen die u toegang hebt.

        azlog authorize 0ee9d577-9bc4-4a32-a4e8-c29981025328

      > [AZURE.NOTE] Mogelijk waarschuwingen weergegeven als u de opdracht **toestaan dat** onmiddellijk na de opdracht **createazureid** . Er is een vertraging tussen wanneer de Azure AD-account is gemaakt en wanneer de account is beschikbaar voor gebruik. Als u ongeveer 10 seconden wacht na het uitvoeren van de opdracht **createazureid** uitvoeren van de opdracht **toestaan** , moet u deze waarschuwingen niet ziet.

4. Controleer de volgende mappen om te bevestigen dat de controlelogboekbestanden voor JSON er zijn:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Controleer de volgende mappen om te bevestigen dat de meldingen van Beveiligingscentrum erin bestaat:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Wijs de standaard SIEM bestand doorstuurserver verbindingslijn naar de juiste map naar de gegevens naar het exemplaar SIEM pipe. Mogelijk moet u enkele veldtoewijzingen op basis van de SIEM product dat u gebruikt.

Als u vragen over Azure logboek integratie hebt, stuur een e-mail naar [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe Azure logboek integration installeert en logboeken van Azure opslag te integreren. Voor meer informatie, Zie de volgende:

- [Integratie van Microsoft Azure logboek voor Azure Logboeken (voorbeeld)](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center voor details, systeemvereisten en installatie-instructies voor de Azure logboek integratie.
- [Inleiding tot de integratie van de Azure log](security-azure-log-integration-overview.md) : dit document maakt u kennis met de integratie van de Azure logboek, de belangrijkste mogelijkheden en hoe het werkt.
- [Partner configuratiestappen](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – dit blogbericht wordt beschreven hoe u integratie werken met oplossingen van partners Splunk ArcSight HP en IBM QRadar Azure logboekbestand configureren.
- Antwoorden op vragen over de integratie van de Azure logboek [azure logboek integratie vaak gestelde vragen (FAQ)](security-azure-log-integration-faq.md) - deze Veelgestelde vragen.
- [Waarschuwingen van Beveiligingscentrum integratie met Azure melden integratie](../security-center/security-center-integrating-alerts-with-log-integration.md) – dit document wordt beschreven hoe u meldingen van Beveiligingscentrum, samen met de virtuele machine beveiligingsgebeurtenissen verzameld door Azure diagnostiek en Azure controlelogboeken, met uw analytics logboek of SIEM oplossing synchroniseren.
- [Nieuwe functies voor Azure diagnostiek en Azure controlelogboeken](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – dit blogbericht maakt u kennis met Azure controlelogboeken en andere functies die u helpen om inzicht te krijgen in de activiteiten van uw Azure bronnen krijgen.
