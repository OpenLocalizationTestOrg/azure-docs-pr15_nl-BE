<properties
   pageTitle="Azure automatische beveiliging | Microsoft Azure"
   description="Dit artikel biedt een overzicht van automatisering, beveiliging en de verschillende verificatiemethoden beschikbaar voor Accounts in Azure automatisering automatisering."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="automatische beveiliging, veilige automatisering" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/29/2016"
   ms.author="magoedte" />

# <a name="azure-automation-security"></a>Azure automatische beveiliging
Azure automatisering kunt u voor het automatiseren van taken vergelijken met de resources in Azure, in gebouwen en met andere cloud providers zoals Amazon Web Services (AWS).  Om een runbook de vereiste acties uit te voeren, moet machtigingen u veilig toegang krijgen tot de bronnen met minimale rechten vereist binnen het abonnement hebben.  
In dit artikel wordt uitgelegd hoe de verificatie scenario's worden ondersteund door automatisering Azure en wordt beschreven hoe u aan de slag op basis van de omgeving of u nodig hebt voor het beheren van omgevingen.  

## <a name="automation-account-overview"></a>Overzicht automatisering
Als u Automation Azure voor de eerste keer start, moet u ten minste één rekening voor automatisering. Automatisering-accounts kunt u voor het isoleren van uw resources automatisering (runbooks, activa, configuraties) van de bronnen die zich bevinden in andere rekeningen automatisering. Automatisering-accounts kunt u resources verdelen in afzonderlijke logische omgevingen. Bijvoorbeeld, kunt u één account voor ontwikkeling, een andere voor de productie en een ander voor uw omgeving op gebouwen.  Een account Azure automatisering verschilt van uw Microsoft-account of accounts die zijn gemaakt in uw abonnement Azure.

Bronnen voor elke rekening automatisering automatisering zijn gekoppeld aan een bepaalde regio Azure, maar automatisering accounts bronnen in elke regio kunnen beheren. De belangrijkste reden voor het maken van accounts voor automatisering in verschillende regio's zou zijn als er beleid waarvoor gegevens en bronnen worden beperkt tot een bepaalde regio.

>[AZURE.NOTE]Automatisering-accounts en de bronnen die ze bevatten die zijn gemaakt in de portal Azure, kan niet worden geopend in de klassieke Azure portal. Als u wilt dat om deze accounts of hun resources met Windows PowerShell te beheren, moet u de modules voor Azure Resource Manager.

Alle taken die u uitvoert voor bronnen met Azure Resource Manager en de Azure cmdlets in Azure automatisering moeten worden geverifieerd naar Azure Azure Active Directory organisatie-identiteit referentie gebaseerde verificatie gebruikt.  Op certificaten gebaseerde verificatie is de oorspronkelijke verificatiemethode met Azure Service Management-modus, maar om de installatie te ingewikkeld was.  Met Azure AD gebruikers worden geverifieerd bij Azure is geïntroduceerd terug in 2014 niet alleen het proces vereenvoudigen met een verificatieaccount configureren, maar ook ondersteuning voor niet-interactief worden geverifieerd bij Azure met één gebruikersaccount die met Azure Resource Manager en klassieke bronnen werkten.   

Op dit moment als u een nieuwe account voor automatisering in Azure portal maakt, wordt automatisch gemaakt:

-  Uitgevoerd als de account die wordt gemaakt van een nieuwe service principal in Azure Active Directory, certificaat, en krijgt de Inzender op rollen gebaseerde toegangscontrole RBAC (), die worden gebruikt voor het beheren van resourcemanager resources met behulp van runbooks.
-  Klassieke uitvoeren als-account door een management-certificaat dat wordt gebruikt voor het beheren van Azure Service Management of klassieke bronnen met behulp van runbooks te uploaden.  

Op rollen gebaseerde toegangscontrole is beschikbaar met Azure Resource Manager toegestane acties een Azure AD-gebruikersaccount en uitvoeren als de account verlenen en verifiëren die service principal.  Lees [op rollen gebaseerde toegangscontrole in Azure automatisering artikel](../automation/automation-role-based-access-control.md) voor meer informatie om te helpen bij het ontwikkelen van een model voor het beheer van machtigingen voor automatisering.  

Uitgevoerd op een hybride Runbook werknemer in uw datacenter of tegen computing services (AWS) in de Runbooks niet dezelfde methode, dat meestal wordt gebruikt voor het verifiëren van runbooks naar Azure bronnen gebruiken.  Dit is omdat deze middelen buiten Azure worden uitgevoerd en moeten daarom hun eigen beveiligingsreferenties gedefinieerd in automatisering om ze lokale toegang tot bronnen te verifiëren.  

## <a name="authentication-methods"></a>Verificatiemethoden

De volgende tabel bevat een overzicht van de verschillende verificatiemethoden voor elke omgeving wordt ondersteund door Azure automatisering en het artikel waarin wordt beschreven hoe u verificatie voor uw runbooks instellen.

Methode  |  Omgeving  | Artikel
----------|----------|----------
Azure AD-gebruikersaccount | Azure Resource Manager en Azure Service Management | [Runbooks verifiëren met Azure AD-gebruikersaccount](../automation/automation-sec-configure-aduser-account.md)
Azure uitvoeren als een Account | Azure Resource Manager | [Runbooks met Azure uitvoeren als-account verifiëren](../automation/automation-sec-configure-azure-runas-account.md)
Azure klassieke uitvoeren als-Account | Azure Service Management | [Runbooks met Azure uitvoeren als-account verifiëren](../automation/automation-sec-configure-azure-runas-account.md)
Windows-verificatie | On-Premises Datacenter | [Runbooks voor werknemers van hybride Runbook verifiëren](../automation/automation-hybrid-runbook-worker.md)
Referenties (AWS) | Amazon webservices | [Runbooks met Amazon webservices (AWS) verifiëren](../automation/automation-sec-configure-aws-account.md)



