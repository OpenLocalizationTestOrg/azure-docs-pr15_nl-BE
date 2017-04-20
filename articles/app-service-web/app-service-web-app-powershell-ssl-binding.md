<properties
    pageTitle="SSL-certificaten binding met PowerShell"
    description="Informatie over het koppelen van SSL-certificaten aan uw web app met PowerShell."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure App Service SSL certificaat Binding met PowerShell #

Met de release van Microsoft Azure PowerShell versie 1.1.0 is een nieuwe cmdlet toegevoegd die de gebruiker de mogelijkheid om bestaande of nieuwe SSL-certificaten binden aan een bestaand Web App zou geven.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Als u wilt weten over het gebruik van Azure Resource Manager op basis van Azure PowerShell controleren cmdlets voor het beheren van uw Web-Apps [Azure Resource Manager op basis van PowerShell-opdrachten voor Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Uploaden en een nieuw certificaat voor SSL-Binding ##

Scenario: De gebruiker wil graag een SSL-certificaat koppelen aan een van zijn web apps.

Weet de naam van de resourcegroep die de web app, de naam van de web app, certificaat .pfx pad naar het bestand op de computer van de gebruiker, het wachtwoord voor het certificaat en de aangepaste hostnaam bevat, kunnen we u het volgende PowerShell dat SSL-binding maken:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Houd er rekening mee dat een host-naam (aangepaste domein) al geconfigureerd voordat u een SSL-binding aan een web app toevoegt, moeten hebben. Als de host-naam niet is geconfigureerd, dan u een foutmelding krijgt 'hostname' tijdens het uitvoeren van nieuwe AzureRmWebAppSSLBinding niet bestaat. Rechtstreeks vanaf de portal of met Azure PowerShell kunt u een host-naam toevoegen. In het volgende fragment van PowerShell is de hostnaam configureren voordat u de nieuwe AzureRmWebAppSSLBinding.   
  
    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   
  
Het is belangrijk te begrijpen dat de hostnamen voor het web app wordt overschreven door de cmdlet Set-AzureRmWebApp. Dus het bovenstaande fragment van PowerShell is toe te voegen aan de bestaande lijst van hostnamen voor de web app.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Uploaden en een bestaand certificaat voor SSL-Binding ##

Scenario: De gebruiker wil graag een SSL-certificaat van eerder geüploade binden aan een van zijn web apps.

Krijgen we de lijst met certificaten die al zijn geüpload naar een specifieke groep met behulp van de volgende opdracht

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Houd er rekening mee dat de certificaten die lokaal op een specifieke locatie en de resourcegroep, moet het certificaat opnieuw te uploaden als de geconfigureerde web app in een andere locatie en brongroep die de gebruiker van het certificaat nodig 

Weet de naam van de resourcegroep die de web app, de naam van de web app vingerafdruk van het certificaat en de aangepaste hostnaam bevat, kunnen we u het volgende PowerShell dat SSL-binding maken:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Een bestaande SSL-binding verwijderen  ##

Scenario: De gebruiker wil verwijderen van een bestaande SSL-binding.

Weet de naam van de resourcegroep die de web app, web app naam en de aangepaste hostnaam bevat, kunnen we gebruiken de volgende PowerShell-opdracht dat SSL-binding verwijderen:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Als de verwijderde SSL-binding is de laatste binding met dat certificaat in deze locatie standaard het certificaat wordt verwijderd, als de gebruiker bewaren het certificaat dat hij de optie DeleteCertificate kunt gebruiken wilt om het certificaat

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Verwijzingen ###
- [Azure Resource Manager op basis van PowerShell-opdrachten voor Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Inleiding tot de App-omgeving](app-service-app-service-environment-intro.md)
- [Met behulp van Azure PowerShell met Azure Resource Manager](../powershell-azure-resource-manager.md)
