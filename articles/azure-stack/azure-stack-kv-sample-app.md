<properties
    pageTitle="Toepassing revtrieve Azure Stack sleutel kluis geheimen | Microsoft Azure"
    description="Een monster app gebruiken om te werken met Azure Stack sleutel kluis"
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

# <a name="run-the-sample-application-for-key-vault"></a>De voorbeeldtoepassing voor sleutel kluis uitvoeren 

In deze handleiding, kunt u een voorbeeldtoepassing geheimen en wachtwoorden ophalen van de sleutel kluis.

## <a name="download-the-samples-and-prepare"></a>Downloaden van de monsters en voorbereiden

De monsters Azure sleutel kluis client downloaden vanaf de [pagina voorbeelden van Azure sleutel kluis client](https://www.microsoft.com/en-us/download/details.aspx?id=45343).

Pak de inhoud van het ZIP-bestand naar uw lokale computer.

Lees het **README.md** -bestand (dit is een tekstbestand) en volg de instructies.

## <a name="run-sample-1--hellokeyvault"></a>Voorbeeld #1--HelloKeyVault uitvoeren
HelloKeyVault is een consoletoepassing die belangrijke scenario's die worden ondersteund door de sleutel kluis doorloopt:

  1. Een sleutel voor HSM of software maken/importeren
  2. Coderen met behulp van een sleutel geheim
  3. De sleutel met behulp van een sleutel sleutel kluis laten teruglopen
  4. Pak de sleutel
  5. Het geheim decoderen
  6. Stel een geheim

Die consoletoepassing wordt uitgevoerd zonder wijzigingen, met dien verstande dat de juiste configuratie-instellingen in App.Config worden bijgewerkt volgens de volgende stappen uit:

1. De configuratie-instellingen van de app in HelloKeyVault\App.config bijwerken met uw URL kluis, principal toepassings-ID en geheim. De informatie kan eventueel worden gegenereerd met behulp van **scripts\GetAppConfigSettings.ps1**.
2. De waarden van de verplichte variabelen in GetAppConfigSettings.ps1 bijwerken.
3. Start de Windows PowerShell-venster.
4. Het script GetAppConfigSettings.ps1 uitvoeren in het venster PowerShell.
5. De resultaten van het script in het bestand HelloKeyVault\App.config kopiëren.


## <a name="next-steps"></a>Volgende stappen

[Implementeren van een VM met een wachtwoord sleutel kluis](azure-stack-kv-deploy-vm-with-secret.md)

[Implementeren van een VM met een certificaat sleutel kluis](azure-stack-kv-push-secret-into-vm.md)