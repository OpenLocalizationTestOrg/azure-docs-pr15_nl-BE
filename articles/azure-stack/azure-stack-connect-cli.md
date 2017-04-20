<properties
    pageTitle="Verbinding maken met Azure Stack met CLI | Microsoft Azure"
    description="Informatie over het gebruik van de opdrachtregelinterface (CLI) van meerdere platforms beheren en implementeren van bronnen op Azure Stack"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-and-configure-azure-stack-cli"></a>Installeer en configureer Azure Stack CLI

In dit document leiden wij u door het proces van het beheren van bronnen op Linux en Mac clientplatforms Azure Stack met Azure opdrachtregelinterface (CLI).  

## <a name="install-azure-stack-cli"></a>Azure Stack CLI installeren

Als u op Mac of Linux, kunt u de CLI opvragen door de volgende opdracht:
  
    `npm install -g azure-cli@0.10.4`.


## <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack
In de volgende stappen kunt u Azure CLI verbinding maken met Azure-Stack configureren. Vervolgens aanmelden en ophalen van informatie over abonnementen.

1.  De waarde voor active directory-resource id ophalen door het uitvoeren van deze PowerShell:
        
          (Invoke-RestMethod -Uri https://api.azurestack.local/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]

2.  Gebruik de volgende CLI-opdracht toe te voegen de Azure Stack-omgeving, en zorg voor het bijwerken van *--active-directory-bron-id* met de gegevens URL opgehaald in de vorige stap:

           azure account env add AzureStack --resource-manager-endpoint-url "https://api.azurestack.local" --management-endpoint-url "https://api.azurestack.local" --active-directory-endpoint-url  "https://login.windows.net" --portal-url "https://portal.azurestack.local" --gallery-endpoint-url "https://portal.azurestack.local" --active-directory-resource-id "https://azurestack.local-api/" --active-directory-graph-resource-id "https://graph.windows.net/"

3.  Meld u aan met de volgende opdracht (Vervang *gebruikersnaam* met uw gebruikersnaam):

        azure login -e AzureStack -u “<username>”

    >[AZURE.NOTE]Als u een certificaat validatieproblemen krijgt, certificaatverificatie uitschakelen met de opdracht `set        NODE_TLS_REJECT_UNAUTHORIZED=0`.

4.  Stel de configuratiemodus Azure Azure Resource Manager met de volgende opdracht:

        azure config mode arm

5.  Ophalen van een lijst met abonnementen.

        azure account list     

## <a name="next-steps"></a>Volgende stappen

[Sjablonen met Azure CLI implementeren](azure-stack-deploy-template-command-line.md)

[Verbinding maken met PowerShell](azure-stack-connect-powershell.md)

[Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)
