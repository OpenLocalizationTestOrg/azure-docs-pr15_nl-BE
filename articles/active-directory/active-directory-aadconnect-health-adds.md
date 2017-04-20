
<properties
    pageTitle="Met behulp van Azure AD gezondheid verbinding met AD DS | Microsoft Azure"
    description="Dit is de gezondheid verbinding Azure AD-pagina die wordt besproken hoe u AD DS te controleren."
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="arluca"/>

# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Met behulp van Azure AD gezondheid verbinding met AD DS
De volgende documentatie is specifiek voor het controleren van Active Directory Domain Services met Azure AD verbinding maken met de gezondheid. De ondersteunde versies van AD DS zijn: Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2.

Zie voor meer informatie over het controleren van AD FS met Azure AD verbinding maken met de gezondheid [Met behulp van Azure AD gezondheid verbinding met AD FS](active-directory-aadconnect-health-adfs.md). Zie daarnaast voor informatie over het controleren van Azure AD verbinding maken (synchroniseren) met Azure AD verbinding maken met de gezondheid van [Azure met behulp van AD gezondheid verbinden voor synchronisatie](active-directory-aadconnect-health-sync.md).

![Azure AD gezondheid verbinding voor AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>Waarschuwingen voor Azure AD verbinding gezondheid voor AD DS
De sectie waarschuwingen in Azure AD verbinding maken met de gezondheid voor AD DS, kunt u een lijst met actieve en opgeloste waarschuwingen, betrekking hebben op uw domeincontrollers. Selecteren van een actief of afgesloten waarschuwing met aanvullende informatie, samen met de stappen voor het oplossen, opent u een nieuwe blade en koppelingen naar ondersteunende documentatie. Elk type waarschuwing kan een of meer exemplaren die met elk van de domeincontrollers die zijn getroffen door deze bepaalde waarschuwing corresponderen te hebben. U kunt dubbelklikken op een desbetreffende domeincontroller een extra blade met meer details over die waarschuwing sessie geopend aan de onderkant van het blad alert.

Binnen deze blade, kunt u e-mailberichten voor waarschuwingen en in de weergave wijzigen van het tijdsbereik. Het bereik uit te breiden, kunt u vooraf opgelost waarschuwingen.

![Azure AD verbinden synchronisatiefout](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>Domain Controllers Dashboard
Dit dashboard biedt een topologische weergave van uw omgeving, samen met belangrijke operationele parameters en de status van elke gecontroleerde domeincontrollers. De aangeboden parameters helpen bij het snel identificeren alle domeincontrollers die mogelijk verder onderzoek. Standaard wordt alleen een subset van de kolommen weergegeven. U kunt echter de volledige set beschikbare kolommen vinden door te dubbelklikken op de opdracht kolommen. Selecteer de kolommen die u meest bent geïnteresseerd, schakelt dit dashboard in een enkele en eenvoudige plaats om de gezondheid van uw AD DS-omgeving weer te geven.

![Domeincontrollers](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Domeincontrollers kunnen worden gegroepeerd door hun respectieve domein of site handig is voor het begrijpen van de topologie van de omgeving. Ten slotte, als u dubbelklikt op de kop blade, het dashboard wordt gemaximaliseerd voor het gebruik van de beschikbare beeldruimte. Deze grotere weergave is handig als u meerdere kolommen worden weergegeven.

## <a name="replication-status-dashboard"></a>Replicatie Status Dashboard
Dit dashboard biedt een weergave van de replicatiestatus en de replicatietopologie van uw gecontroleerde domeincontrollers. De status van de meest recente replicatiepoging wordt vermeld, samen met nuttige documentatie voor elke fout die wordt gevonden. U kunt dubbelklikken op een domeincontroller met een fout, naar een nieuwe blade openen met informatie zoals: details over de fout, aanbevolen stappen voor het oplossen en koppelingen naar documentatie bij het oplossen van problemen.

![Replicatiestatus](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>Monitoring
Deze functie biedt een grafische trends van verschillende prestatiemeteritems die voortdurend worden verzameld uit elk van de gecontroleerde domeincontrollers. Prestaties van een domeincontroller kan gemakkelijk alle andere gecontroleerde domeincontrollers in het forest worden vergeleken. Bovendien ziet u verschillende prestatiemeteritems naast elkaar, die u kan helpen bij het oplossen van problemen in uw omgeving.

![Monitoring](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Standaard hebben wij vooraf geselecteerd vier prestatiemeteritems; u kunt anderen echter opnemen door te klikken op de filteropdracht en of als u alle gewenste prestatiemeteritems uitschakelt. Bovendien kunt u een grafiek performance counter opent een nieuwe blade gegevenspunten voor elk van de gecontroleerde domeincontrollers met dubbelklikken.

## <a name="related-links"></a>Verwante koppelingen

* [Azure AD verbinding gezondheid](active-directory-aadconnect-health.md)
* [Azure AD Health Agent-installatie aansluiten](active-directory-aadconnect-health-agent-install.md)
* [Azure AD verbinding gezondheid bewerkingen](active-directory-aadconnect-health-operations.md)
* [Met behulp van Azure AD gezondheid verbinding met AD FS](active-directory-aadconnect-health-adfs.md)
* [Met behulp van gezondheid Azure AD verbinden voor synchronisatie](active-directory-aadconnect-health-sync.md)
* [Azure AD gezondheid Veelgestelde vragen over verbinding](active-directory-aadconnect-health-faq.md)
* [Azure AD verbinding gezondheid versiegeschiedenis](active-directory-aadconnect-health-version-history.md)
