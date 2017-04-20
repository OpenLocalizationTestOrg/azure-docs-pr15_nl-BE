<properties
    pageTitle="Implementeren van een VM met behulp van een wachtwoord opgeslagen in Azure Stack sleutel kluis | Microsoft Azure"
    description="Informatie over het implementeren van een VM met behulp van een wachtwoord opgeslagen in Azure Stack sleutel kluis"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="deploy-a-vm-by-retrieving-the-password-stored-in-key-vault"></a>Implementatie van een VM ophalen van het wachtwoord opgeslagen in een kluis met sleutel

Wanneer moet u een veilige waarde (zoals een wachtwoord) als parameter doorgeven tijdens de implementatie kunt u waarde opslaan als een geheim in een sleutel kluis Azure Stack en verwijst naar de waarde in het andere sjablonen Azure Resource Manager. Neem alleen een verwijzing naar het geheim in de sjabloon zodat het geheim is nooit zichtbaar. U hoeft niet de waarde handmatig invoeren voor het geheim elke keer dat u de resources implementeren. U opgeven welke gebruikers of service beveiligings-principals toegang krijgen het geheim tot.

## <a name="reference-a-secret-with-static-id"></a>Verwijst naar een geheim met statische-ID

U verwijst naar het geheim van binnen een parameterbestand, die waarden aan uw sjabloon. U verwijst naar het geheim door de bron-id van de sleutel kluis en de naam van het geheim. In dit voorbeeld wordt moet de geheime sleutel kluis aanwezig zijn. Een statische waarde u voor de resource-ID.

    "parameters": {
    "adminPassword": {
    "reference": {
    "keyVault": {
    "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
    },
    "secretName": "sqlAdminPassword"


>[AZURE.NOTE]De parameter die het geheim accepteert, moet een *securestring*.

## <a name="next-steps"></a>Volgende stappen
[Implementeren van een monster app met sleutel kluis](azure-stack-kv-sample-app.md)

[Implementeren van een VM met een certificaat sleutel kluis](azure-stack-kv-push-secret-into-vm.md)

