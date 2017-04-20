<properties
    pageTitle="Azure overheidsdiensten | Microsoft Azure"
    description="Informatie over het beheren van uw abonnement in Azure overheid"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/21/2016"
    ms.author="zakramer" />


#  <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>Beheren en koppelen aan uw abonnement in Azure overheid

Azure regering heeft een unieke URL's en eindpunten voor het beheer van uw omgeving. Het is belangrijk de juiste verbindingen gebruiken voor het beheren van uw omgeving, via de portal of via PowerShell. Zodra u verbonden bent met de regering van Azure-omgeving, werkt de normale bewerkingen voor het beheren van een service als het onderdeel is geïmplementeerd.

## <a name="connecting-via-the-portal"></a>Verbinding maken via de portal
De portal is de primaire manier waarop de meeste mensen maken verbinding met Azure regering.  Als u wilt verbinden, Ga naar de portal op [https://portal.azure.us](https://portal.azure.us).  De oudere versie van de Azure portal is toegankelijk via [https://manage.windowsazure.us](https://manage.windowsazure.us).

Abonnementen kunnen worden gemaakt voor uw account door verbinding te maken met [https://account.windowsazure.us](https://account.windowsazure.us).

## <a name="connecting-via-powershell"></a>Verbinding maken via PowerShell

U gebruikt of Azure PowerShell grote abonnementen via een script of een access-functies momenteel niet beschikbaar in de Azure portal zijn, moet u verbinding maken met de overheid in plaats van Azure Azure.  Als u in openbare Azure PowerShell gebruikt, is grotendeels hetzelfde.  De verschillen in Azure overheid zijn:

+ Verbinding maken met uw account
+ Regionamen

>[AZURE.NOTE] Als u niet PowerShell gebruikt nog, Raadpleeg de [Inleiding tot Azure PowerShell](../powershell-install-configure.md).

Wanneer u PowerShell start, hebt u vertellen Azure PowerShell verbinding maken met Azure regering door een parameter omgeving te geven.  De parameter zorgt ervoor dat PowerShell maakt verbinding met de juiste eindpunten.  De collectie van eindpunten wordt bepaald wanneer u een logboek in verbinding met uw account.  Verschillende API's vereisen verschillende versies van de milieu-switch:

Verbindingstype | Opdracht
---|----
Opdrachten voor het [Beheer van de service](https://msdn.microsoft.com/library/dn708504.aspx) | `Add-AzureAccount -Environment AzureUSGovernment`
Opdrachten voor het [Beheer van de bron](https://msdn.microsoft.com/library/mt125356.aspx) | `Add-AzureRmAccount -EnvironmentName AzureUSGovernment`
[Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx) -opdrachten | `Connect-MsolService -AzureEnvironment UsGovernment`
[Azure Active Directory opdracht v2](https://msdn.microsoft.com/library/azure/mt757189.aspx) | `Connect-AzureAD -AzureEnvironmentName AzureUSGovernment`

U kunt ook de schakeloptie omgeving gebruiken wanneer u verbinding maakt met een nieuw AzureStorageContext met opslag-account en AzureUSGovernment opgeven.

### <a name="determining-region"></a>Bepaling van de regio

Zodra u verbonden bent, is er een extra verschil – de regio's gebruikt om een service.  Elke Azure cloud heeft verschillende regio's.  U kunt deze vermeld op de pagina beschikbaarheid zien.  Gebruikt het gebied in de parameter locatie voor een opdracht.

Er is één vangst.  De regering van Azure regio's moeten worden anders opgemaakt dan de algemene benamingen:

Algemene naam | Opdracht
---|----
Amerikaanse finan-Virginia | USGov-Virginia
Amerikaanse beurs Iowa | USGov-Iowa

>[AZURE.NOTE] Er is geen ruimte tussen de Verenigde Staten en bestudeerd wanneer de parameter locatie gebruikt.

Als u ooit wilt voor het valideren van de beschikbare gebieden in Azure, overheid, kunt u de volgende opdrachten uitvoeren en de huidige lijst afdrukken:

    Get-AzureLocation

Als u meer wilt weten over de beschikbare omgevingen in Azure, die u kunt uitvoeren:

    Get-AzureEnvironment

## <a name="next-steps"></a>Volgende stappen

Als u meer informatie zoekt, kunt u uitchecken:

+ [PowerShell docs op GitHub](https://github.com/Azure/azure-powershell)
+ [Stapsgewijze instructies voor het verbinden met Resource Management](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
+ [Azure PowerShell docs op MSDN](https://msdn.microsoft.com/library/mt619274.aspx)

Voor aanvullende informatie en updates abonneren op de [Microsoft Azure overheid Blog] (https://blogs.msdn.microsoft.com/azuregov/)
