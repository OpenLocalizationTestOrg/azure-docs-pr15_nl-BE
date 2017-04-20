<properties
   pageTitle="Sleutel kluis Developer's Guide | Microsoft Azure"
   description="Ontwikkelaars kunnen Azure sleutel kluis gebruiken voor het beheren van cryptografische sleutels binnen de Microsoft Azure-omgeving. "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/03/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-developers-guide"></a>Azure sleutel kluis Developer's Guide
Met de sleutel kluis, kun je veilig toegang krijgen tot vertrouwelijke informatie in uw toepassingen zodanig zijn dat:

- Sleutels en geheimen worden beschermd zonder de code zelf schrijven en kunt u gemakkelijk kunt ze uit uw toepassingen te gebruiken.
- U zult uw eigen klanten en hun eigen sleutels beheren zodat u zich concentreren kunt op het aanbieden van de belangrijkste software-functies. Op deze manier wordt de verantwoordelijkheid of potentiële aansprakelijkheid voor uw klanten huurder sleutels en geheimen niet eigenaar van uw toepassingen.
- Uw toepassing kan toetsen gebruiken voor het ondertekenen en codering nog blijft de Sleutelbeheer externe vanuit uw toepassing zodanig zijn dat de oplossing geschikt is voor een toepassing die is geografisch verspreid.

- Met de release in September 2016 van sleutel kluis, uw toepassingen kunnen nu gebruik maken van sleutel kluis certificaten. Voor meer informatie, Zie artikel **over certificaten en sleutels, geheimen,** in de [verwijzing naar de REST](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Voor meer algemene informatie over Azure sleutel kluis, Zie [Wat is sleutel kluis](key-vault-whatis.md).

## <a name="videos"></a>Video 's
Deze video wordt uitgelegd hoe u het maken van uw eigen sleutel kluis en het gebruik van de voorbeeldtoepassing 'Hallo sleutel kluis'.

[AZURE.VIDEO azure-key-vault-developer-quick-start]

Koppelingen naar bronnen die worden vermeld in de video:
- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Voorbeeldcode Azure sleutel kluis](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

Als u wilt weten kunt meer u de [Sleutel kluis Blog](http://aka.ms/kvblog) volgen en de [Sleutel kluis Forum](http://aka.ms/kvforum)deelnemen.

## <a name="creating-and-managing-key-vaults"></a>Maken en beheren van sleutels kluizen

Voordat u gaat werken met Azure sleutel kluis in de code, kunt u maken en beheren van kluizen tot en met de REST, Resource Manager-sjablonen, PowerShell of CLI, zoals beschreven in de volgende artikelen:

- [Maken en beheren van sleutels kluizen met REST](https://msdn.microsoft.com/library/azure/mt620024.aspx)
- [Maken en beheren van sleutels kluizen met PowerShell](key-vault-get-started.md)
- [Maken en beheren van sleutels kluizen met CLI](key-vault-manage-with-cli.md)
- [Maak een sleutel kluis en een geheim via een sjabloon Azure Resource Manager toevoegen](../resource-manager-template-keyvault.md)

>[AZURE.NOTE] Bewerkingen op de sleutel kluizen worden via AAD geverifieerd en geautoriseerd via sleutel kluis van eigen-beleid gedefinieerd per kluis.

## <a name="coding-with-key-vault"></a>Codering met een sleutel kluis

De sleutel kluis beheersysteem voor programmeurs bestaat uit verschillende interfaces voor de REST als basis, [Sleutel kluis REST API Reference](https://msdn.microsoft.com/library/azure/dn903609.aspx).

U kunt geslaagde toestemming het volgende doen:

- Cryptografische sleutels [maken](https://msdn.microsoft.com/library/azure/dn903634.aspx), [importeren](https://msdn.microsoft.com/library/azure/dn903626.aspx), [bijwerken](https://msdn.microsoft.com/library/azure/dn903616.aspx), [verwijderen](https://msdn.microsoft.com/library/azure/dn903611.aspx) en andere bewerkingen met beheren

- Geheimen met het [ophalen](https://msdn.microsoft.com/library/azure/dn903633.aspx), [bijwerken](https://msdn.microsoft.com/library/azure/dn986818.aspx), [verwijderen](https://msdn.microsoft.com/library/azure/dn903613.aspx) en andere bewerkingen beheren

- Cryptografische sleutels te gebruiken met het [teken](https://msdn.microsoft.com/library/azure/dn878096.aspx)/[controleren](https://msdn.microsoft.com/library/azure/dn878082.aspx), [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx) en [coderen](https://msdn.microsoft.com/library/azure/dn878060.aspx)/[decoderen](https://msdn.microsoft.com/library/azure/dn878097.aspx) bewerkingen

De volgende SDK's zijn beschikbaar voor het werken met sleutel kluis:

|[![.NET](./media/key-vault-developers-guide/msft.netlogo_purple.png)](https://msdn.microsoft.com/library/mt765854.aspx)|[![Node.js](./media/key-vault-developers-guide/nodejs.png)](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)
|:--:|:--:|
|[.NET SDK-documentatie](https://msdn.microsoft.com/library/mt765854.aspx)|[Node.js SDK-documentatie](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)|
|[.NET SDK pakket op Nuget](http://www.nuget.org/packages/Microsoft.Azure.KeyVault)|[Node.js SDK-pakket](https://www.npmjs.com/package/azure-keyvault)|

Zie de [Release-opmerkingen](key-vault-dotnet2api-release-notes.md)voor meer informatie over de versie 2.x van de SDK voor .NET.

## <a name="example-code"></a>Van voorbeeldcode
Zie voor volledige sleutel kluis met uw toepassingen met voorbeelden:

- De voorbeeldtoepassing .NET *HelloKeyVault* en een voorbeeld van Azure web service. [Azure sleutel kluis codevoorbeelden](http://www.microsoft.com/download/details.aspx?id=45343)
- Zelfstudie kunt u informatie over het gebruik van Azure sleutel kluis uit een webtoepassing in Azure. [Azure sleutel kluis uit een webtoepassing gebruiken](key-vault-use-from-web-application.md)

## <a name="how-tos"></a>Uitleg

De volgende artikelen en scenario's vindt specifieke richtlijnen voor het werken met Azure sleutel kluis taak:

- [Sleutel kluis huurder-ID wijzigen nadat het abonnement verplaatsen](key-vault-subscription-move-fix.md) - wanneer u naar uw Azure abonnement van de huurder een huurder B, de bestaande sleutel kluizen zijn niet toegankelijk door de beveiligings-principals (gebruikers en toepassingen) in een huurder B. Fix dit met behulp van deze handleiding.
- [Toegang tot sleutel kluis achter firewall](key-vault-access-behind-firewall.md) - toegang te krijgen tot een sleutel kluis die uw clienttoepassing sleutel kluis moet kunnen toegang krijgen tot meerdere parameters voor verschillende functies.

- [Het genereren en Transfer HSM-Protected toetsen voor Azure sleutel kluis](key-vault-hsm-protected-keys.md) - dit helpt u bij het plannen, genereren en brengt u uw eigen beveiligd met een HSM toetsen die met Azure sleutel kluis.
- [Doorgeven van veilige waarden (zoals wachtwoorden) tijdens de implementatie](../resource-manager-keyvault-parameter.md) - als u wilt een veilige waarde (zoals een wachtwoord) als parameter doorgeven tijdens de implementatie, kunt u die waarde opslaan als een geheim in een Azure sleutel kluis en een verwijzing naar de waarde in een andere Resource Manager-sjablonen.
- [Het gebruik van de sleutel kluis voor extensible Sleutelbeheer met SQL Server](https://msdn.microsoft.com/library/dn198405.aspx) - de SQL Server-Connector voor Azure sleutel kluis kunt u SQL Server en SQL in een VM gebruikmaken van de service Azure sleutel kluis als een provider Extensible Key Management (EKM) ter bescherming van de coderingssleutels voor koppeling van toepassingen; Transparante gegevenscodering, back-codering en codering van kolom niveau.
- [Het implementeren van certificaten aan VMs uit sleutel kluis](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - een wolk toepassing die wordt uitgevoerd in een VM in Azure moet een certificaat. Hoe krijgt u dit certificaat in deze VM vandaag?
- [Hoe setup sleutel kluis met sleutel rotatie van begin tot eind en controle](key-vault-key-rotation-log-monitoring.md) - dit helpt bij het instellen van controle met Azure sleutel kluis en key rotatie.

Zie voor verdere taak-specifieke informatie over het integreren en sleutel kluizen gebruiken met Azure [Ryan Jansen Azure Resource Manager sjabloon voorbeelden voor sleutel kluis](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

## <a name="integrated-with-key-vault"></a>Geïntegreerd met sleutel kluis

Deze artikelen worden over andere scenario's en diensten die ons van of integreren met sleutel kluis.

- [Azure schijf codering](../security/azure-security-disk-encryption.md) maakt gebruik van de industrie standaard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) -functie van Windows en de functie [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) van Linux voor codering van het volume voor het besturingssysteem en de gegevensschijven. De oplossing is geïntegreerd met Azure sleutel kluis om te bepalen en de schijf coderingssleutels en geheimen in je abonnement sleutel kluis, terwijl ervoor te zorgen dat alle gegevens in de virtuele machine-schijven worden gecodeerd in rust in Azure opslag beheren.


## <a name="supporting-libraries"></a>Ondersteunende bibliotheken

- [Microsoft Azure sleutel kluis Core Library](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) bevat `IKey` en `IKeyResolver` interfaces voor het zoeken naar sleutels van id's en het uitvoeren van bewerkingen met sleutels.

- [Microsoft Azure sleutel kluis Extensions](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) biedt uitgebreide mogelijkheden voor Azure sleutel kluis.

## <a name="other-key-vault-resources"></a>Andere bronnen sleutel kluis
- [Sleutel kluis Blog](http://aka.ms/kvblog)
- [Sleutel kluis Forum](http://aka.ms/kvforum)
