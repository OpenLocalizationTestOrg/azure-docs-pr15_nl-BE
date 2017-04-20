<properties
    pageTitle="Werken met verbindingslijnen Azure AD toepassingsproxy | Microsoft Azure"
    description="Wordt beschreven hoe u groepen van verbindingslijnen in Azure AD-toepassingsproxy maken en beheren."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups---public-preview"></a>Uitgeven van toepassingen op afzonderlijke netwerken en locaties met groepen connector - Public Preview

> [AZURE.SELECTOR]
- [Azure portal](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure klassieke portal](active-directory-application-proxy-connectors.md)


Connector-groepen zijn nuttig voor verschillende scenario's, met inbegrip van:

- Sites met meerdere onderling verbonden datacenters. In dit geval wilt u zo veel verkeer binnen het datacenter mogelijk houden omdat cross-datacenter koppelingen duur en traag zijn. U kunt verbindingslijnen in elk datacenter dienst alleen de toepassingen die zich binnen het datacenter bevinden te implementeren. Deze benadering wordt geminimaliseerd cross-datacenter koppelingen en biedt een volledig transparante ervaring voor uw gebruikers.
- Toepassingen die zijn geïnstalleerd op afzonderlijke netwerken die geen deel uitmaken van de belangrijkste bedrijfsnetwerk beheren. U kunt groepen connector speciale verbindingslijnen op afzonderlijke netwerken ook om toepassingen te isoleren op het netwerk installeren.
- Voor toepassingen die zijn geïnstalleerd op IaaS voor toegang tot de cloud, dienstverlening connector groepen gemeenschappelijke beveiliging van de toegang tot alle apps. Connector groepen niet meer afhankelijkheid maken op uw bedrijfsnetwerk of fragment van de app-ervaring. Verbindingslijnen kunnen worden geïnstalleerd op elke cloud datacenter en dienen alleen toepassingen die zijn opgeslagen in dit netwerk. U kunt verschillende connectoren voor maximale beschikbaarheid bereiken installeren.
- Ondersteuning voor omgevingen met meerdere forests waarin specifieke verbindingslijnen kunnen worden geïmplementeerd per forest en instellen voor specifieke toepassingen.
- Connector-groepen kunnen worden gebruikt als back-up of Disaster Recovery sites voor het detecteren van een failover voor de hoofdsite.
- Connector-groepen kunnen ook worden gebruikt voor meerdere bedrijven van een enkele huurder.

## <a name="prerequisite-create-your-connectors"></a>Let op: Uw verbindingslijnen maken
Als u wilt uw verbindingslijnen groeperen, die u hebt om te controleren of u [meerdere verbindingslijnen geïnstalleerd](active-directory-application-proxy-enable.md). Tijdens de installatie van een nieuwe verbindingslijn lid automatisch de **standaard** connector groep.

## <a name="step-1-create-connector-groups"></a>Stap 1: Groepen verbindingslijn maken
U kunt zoveel groepen verbindingslijn die u wilt maken. Verbindingslijn maken wordt in [Azure portal](https://portal.azure.com)uitgevoerd.

1. Selecteer **Azure Active Directory** naar het management dashboard voor de map te gaan. Selecteer vanuit **bedrijfstoepassingen** > **de toepassingsproxy**.

2. Klik op de knop **Verbindingslijn groepen** . De nieuwe Connector groep blade wordt weergegeven.

3. De nieuwe connector groep een naam geven en vervolgens de vervolgkeuzelijst gebruiken om te selecteren welke connectors in deze groep behoren.

4. Selecteer **Opslaan** als de verbindingslijn groep voltooid is.

## <a name="step-2-assign-applications-to-your-connector-groups"></a>Stap 2: Toepassingen toewijzen aan groepen connector
De laatste stap is in te stellen van elke aanvraag naar de connector-groep die zal worden gebruikt.

1. Het management dashboard voor de map, selecteer **Enterprise applications** > **alle toepassingen** > de toepassing die u wilt toewijzen aan een groep connector > **Toepassingsproxy**.
2. Gebruik het vervolgmenu te selecteren van de groep die u wilt dat de toepassing te gebruiken onder **Connector groep**.
3. Selecteer **Opslaan** om de wijziging door te voeren.


## <a name="see-also"></a>Zie ook

- [Toepassingsproxy inschakelen](active-directory-application-proxy-enable.md)
- [Eenmalige inschakelen](active-directory-application-proxy-sso-using-kcd.md)
- [Voorwaardelijke toegang](active-directory-application-proxy-conditional-access.md)
- [Oplossen van problemen met de toepassingsproxy](active-directory-application-proxy-troubleshoot.md)

Bekijk de [toepassingsproxy blog](http://blogs.technet.com/b/applicationproxyblog/) voor het laatste nieuws en updates
