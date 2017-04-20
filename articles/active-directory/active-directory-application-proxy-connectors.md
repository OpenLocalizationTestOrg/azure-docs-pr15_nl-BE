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


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Uitgeven van toepassingen op afzonderlijke netwerken en locaties met connector-groepen

> [AZURE.SELECTOR]
- [Azure portal](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure klassieke portal](active-directory-application-proxy-connectors.md)


Connector-groepen zijn nuttig voor verschillende scenario's, met inbegrip van:

- Sites met meerdere onderling verbonden datacenters. In dit geval wilt u zo veel verkeer binnen het datacenter mogelijk houden omdat cross-datacenter koppelingen meestal duur en traag zijn. U kunt verbindingslijnen in elk datacenter dienst alleen de toepassingen die zich binnen het datacenter bevinden te implementeren. Deze benadering wordt geminimaliseerd cross-datacenter koppelingen en biedt een volledig transparante ervaring voor uw gebruikers.
- Toepassingen die zijn geïnstalleerd op afzonderlijke netwerken die geen deel uitmaken van de belangrijkste bedrijfsnetwerk beheren. U kunt groepen connector speciale verbindingslijnen op afzonderlijke netwerken ook om toepassingen te isoleren op het netwerk installeren.
- Voor toepassingen die zijn geïnstalleerd op IaaS voor toegang tot de cloud, dienstverlening connector groepen gemeenschappelijke beveiliging van de toegang tot alle apps. Connector groepen niet meer afhankelijkheid maken op uw bedrijfsnetwerk of fragment van de app-ervaring. Verbindingslijnen kunnen worden geïnstalleerd op elke cloud datacenter en dienen alleen toepassingen die zijn opgeslagen in dit netwerk. U kunt verschillende connectoren voor maximale beschikbaarheid bereiken installeren.
- Ondersteuning voor omgevingen met meerdere forests waarin specifieke verbindingslijnen kunnen worden geïmplementeerd per forest en instellen voor specifieke toepassingen.
- Connector-groepen kunnen worden gebruikt als back-up of Disaster Recovery sites voor het detecteren van een failover voor de hoofdsite.
- Connector-groepen kunnen ook worden gebruikt voor meerdere bedrijven van een enkele huurder.

## <a name="prerequisite-create-your-connectors"></a>Let op: Uw verbindingslijnen maken
Om uw verbindingslijnen, hebben ze Zorg ervoor dat u [meerdere verbindingslijnen geïnstalleerd](active-directory-application-proxy-enable.md)en die u naam en deze groeperen. Ten slotte moet u deze toewijzen aan bepaalde apps.

## <a name="step-1-create-connector-groups"></a>Stap 1: Groepen verbindingslijn maken
U kunt zoveel groepen verbindingslijn die u wilt maken. Verbindingslijn maken wordt in de klassieke Azure portal uitgevoerd.

1. Selecteer de map en klikt u op **configureren**.  
    ![Application-proxy configureren screenshot - Klik op de verbindingslijn groepen beheren](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)

2. Onder de toepassingsproxy, klikt u op de **Verbindingslijn groepen beheren** en een nieuwe verbindingslijn groep maken door de groep een naam.  
    ![Application proxy connector groepen screenshot - groep voor de nieuwe](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Stap 2: Connectors aan de groepen toewijzen
Zodra de verbindingslijn groepen zijn gemaakt, verplaatst u de verbindingslijnen aan de juiste groep.

1. Klik onder **De toepassingsproxy**, **Connectors beheren**.
2. Selecteer de groep die u voor elke verbindingslijn onder **groep**. Het kan de verbindingslijnen actief in de nieuwe groep van maximaal 10 minuten duren.  
    ![Application proxy verbindingslijnen screenshot - groep in vervolgkeuzelijst](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Stap 3: Toepassingen toewijzen aan groepen connector
De laatste stap is in te stellen van elke aanvraag naar de connector-groep die zal worden gebruikt.

1. Selecteer de toepassing die u wilt toewijzen aan de groep en klik op **configureren**in de Azure klassieke portal in uw directory.
2. Selecteer de groep waarop u de toepassing voor het gebruik onder **groep Connector**. Deze wijziging wordt onmiddellijk toegepast.  
    ![Application proxy connector groep screenshot - groep in vervolgkeuzelijst](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)


## <a name="see-also"></a>Zie ook

- [Toepassingsproxy inschakelen](active-directory-application-proxy-enable.md)
- [Eenmalige inschakelen](active-directory-application-proxy-sso-using-kcd.md)
- [Voorwaardelijke toegang](active-directory-application-proxy-conditional-access.md)
- [Oplossen van problemen met de toepassingsproxy](active-directory-application-proxy-troubleshoot.md)

Bekijk de [toepassingsproxy blog](http://blogs.technet.com/b/applicationproxyblog/) voor het laatste nieuws en updates
