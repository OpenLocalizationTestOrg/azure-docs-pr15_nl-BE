<properties
   pageTitle="Configureren van uw Azure project met meerdere configuraties | Microsoft Azure"
   description="Informatie over het configureren van een project Azure cloud service door het wijzigen van de bestanden ServiceDefinition.csdef en ServiceConfiguration.cscfg."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Uw Azure Project met meerdere configuraties configureren

Een project Azure cloud-service omvat twee configuratiebestanden: ServiceDefinition.csdef en ServiceConfiguration.cscfg. Deze bestanden zijn ingepakt met uw toepassing Azure cloud service en geïmplementeerd op Azure.

- Het bestand **ServiceDefinition.csdef** bevat de metagegevens die wordt vereist door de Azure-omgeving voor de vereisten van uw toepassing cloud-service, met inbegrip van welke rollen bevat. Dit bestand bevat ook de configuratie-instellingen die van toepassing op alle exemplaren. Deze configuratie-instellingen kunnen worden gelezen tijdens runtime Azure Service Hosting Runtime-API gebruiken. Dit bestand kan niet worden bijgewerkt terwijl de service wordt uitgevoerd in Azure.

- Het bestand **ServiceConfiguration.cscfg** stelt de waarden voor de configuratie-instellingen die zijn gedefinieerd in het bestand van de definitie en geeft u het aantal exemplaren uit te voeren voor elke rol. Dit bestand kan worden bijgewerkt terwijl de cloud-service wordt uitgevoerd in Azure.

De Azure-hulpprogramma's voor Microsoft Visual Studio biedt pagina's die u gebruiken kunt voor het instellen van configuratieinstellingen in deze bestanden. De verwijzing rol onder de Azure cloud service project in de Solution Explorer dubbelklikt op voor toegang tot de pagina's, of met de rechtermuisknop op de rol van verwijzing en kiest u **Eigenschappen**, zoals in de volgende afbeelding.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Zie het [Schema: referentie](https://msdn.microsoft.com/library/azure/dd179398.aspx)voor meer informatie over het onderliggende schema's voor de definitie van service en service-configuratiebestanden. Zie [How to Cloud Services configureren](./cloud-services/cloud-services-how-to-configure.md)voor meer informatie over de configuratie van de service.

## <a name="configuring-role-properties"></a>Functie-eigenschappen configureren

De eigenschappenpagina's voor een Webrol en de rol van een werknemer zijn vergelijkbaar, maar er enkele verschillen zijn, uiteengezet in de volgende secties.

Op de pagina **cache** kunt u de caching-services Azure.

### <a name="configuration-page"></a>De configuratiepagina

U kunt deze eigenschappen instellen op de pagina **configuratie** :

**Exemplaren**

Stel de **instantie** -eigenschap count om het aantal keren dat de service wordt uitgevoerd voor deze rol.

Stel de eigenschap **VM-grootte** op **Extra Small**, **Small**, **Medium**, **groot**of **Extra groot**.  Zie [formaten voor Cloud Services](./cloud-services/cloud-services-sizes-specs.md)voor meer informatie.

**Actie starten** (Webrol)

Stel deze eigenschap op te geven dat Visual Studio een webbrowser voor het HTTP-eindpunten of de eindpunten HTTPS of beide starten moet wanneer u foutopsporing start.

De HTTPS-eindpunt-optie is alleen beschikbaar als u al een HTTPS-eindpunt voor uw rol hebt gedefinieerd. Een HTTPS-eindpunt kunt u definiëren in het eigenschappenvenster van de **eindpunten** .

Als u een eindpunt HTTPS al hebt toegevoegd, de HTTPS-eindpunt optie is standaard ingeschakeld en Visual Studio een browser voor dit eindpunt wordt gestart bij het starten van foutopsporing, naast een browser voor uw HTTP-eindpunt. Hierbij wordt ervan uitgegaan dat beide opties voor het opstarten zijn ingeschakeld.

**Diagnostische gegevens**

Diagnostische gegevens is standaard ingeschakeld voor de rol van het Web. Azure cloud opslag project en de account voor de service op de emulator van de lokale opslag zijn ingesteld. Wanneer u klaar bent om te implementeren op Azure, kunt u de knop Opbouwfunctie (**...**) voor het bijwerken van de rekening van de opslag voor het gebruik van Azure opslag in de cloud. Op de vraag of regelmatig automatisch, kunt u de diagnostische gegevens overbrengen naar de rekening voor opslag. Zie voor meer informatie over diagnostische gegevens van Azure [Diagnostics inschakelen in Azure Cloud Services en virtuele Machines](./cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Pagina-instellingen

U kunt de configuratie-instellingen voor uw service toevoegen op de pagina **Instellingen** . Configuratie-instellingen zijn naam-waardeparen. Code die wordt uitgevoerd in de rol van de waarden van de configuratie-instellingen bij uitvoering met klassen beschikbaar zijn in de [Bibliotheek van Azure beheerd](http://go.microsoft.com/fwlink?LinkID=171026)kan worden gelezen. Met name retourneert de methode [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) de waarde van een benoemde configuratie-instelling bij uitvoering.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Een verbindingsreeks voor een opslag-account configureren

Een verbindingsreeks is een configuratie-instelling waarmee verbindings-en voor de emulator voor opslag of voor een rekening Azure opslag. Wanneer uw code Azure services opslaggegevens – dat wil zeggen, blob, wachtrij of gegevens in een tabel: vanuit code in een functie die wordt uitgevoerd openen moet, hebt u een verbindingsreeks voor de opslag account definiëren.

Een verbindingsreeks die naar een account Azure opslag verwijst kan een gedefinieerde indeling moet gebruiken. Zie voor meer informatie over het maken van databaseverbindingsreeksen [Azure opslag verbindingsreeksen configureren](./storage/storage-configure-connection-string.md).

Wanneer u klaar bent om te testen uw service tegen de Azure storage-services, of als u wilt uw cloud service implementeren op Azure, kunt u de waarde van eventuele verbindingstekenreeksen verwijzen naar uw account Azure opslag. (**...**) Selecteert, selecteert u **Enter opslag referenties**. Voer uw accountgegevens in met uw accountnaam en de sleutel van de account. In het dialoogvenster **Verbindingsreeks opslag-Account** kunt u ook aangeven of u wilt gebruiken de standaard-HTTPS-eindpunten (de standaardoptie), de standaard HTTP-eindpunten of aangepaste eindpunten. U kunt aangepaste eindpunten gebruiken als u een aangepaste domeinnaam voor uw service hebt geregistreerd, zoals wordt beschreven in [een aangepaste domeinnaam voor blob-gegevens in een opslag Azure-account configureren](./storage/storage-custom-domain-name.md).

>[AZURE.IMPORTANT] U moet uw verbindingsreeksen om te verwijzen naar een Azure opslag account voordat u uw service wijzigen. Niet te doen, is het mogelijk dat uw rol niet starten, of bladeren door de lidstaten bij het initialiseren, bezet en stoppen.

## <a name="endpoints-page"></a>Pagina Tunneleindpunten

De rol van een werknemer kan een willekeurig aantal HTTP-, HTTPS- of TCP-eindpunten hebben. Eindpunten zijn invoer-eindpunten die beschikbaar zijn voor externe clients, of interne eindpunten die beschikbaar zijn voor andere functies die in de service worden uitgevoerd.

- Als u een HTTP-eindpunt aan externe clients en webbrowsers, het eindpunt naar invoer wijzigen en geef een naam en een openbare poortnummer.

- Als u een eindpunt HTTPS aan externe clients en webbrowsers, eindpunt type **invoer**wijzigen en geef een naam, een openbare poortnummer en de naam van een management-certificaat.

    Opmerking voordat u een certificaat opgeven kunt, u het certificaat op het tabblad **certificaten definiëren moet** .

- Als u een eindpunt voor interne toegang door andere rollen in de cloud-service, het eindpunt naar interne wijzigen en geef een naam en een mogelijke particuliere poorten voor dit eindpunt.

## <a name="local-storage-page"></a>Pagina lokale opslag

Het eigenschappenvenster voor **Lokale opslag** kunt u een of meer lokale opslag resources reserveert een rol. Een bron van lokale opslag is een gereserveerde map in het bestandssysteem van Azure virtual machine een exemplaar van een rol wordt uitgevoerd.

## <a name="certificates-page"></a>Pagina certificaten

Klik op de pagina **certificaten** kunt u certificaten kunt koppelen aan uw rol. De certificaten die u toevoegt kunnen uw HTTPS-eindpunten configureren op de eigenschappenpagina **eindpunten** worden gebruikt.

Informatie over de certificaten de eigenschappenpagina **certificaten** toegevoegd aan de serviceconfiguratie van de. Houd er rekening mee dat de certificaten niet zijn verpakt met uw service. u moet uw certificaten afzonderlijk uploaden naar Azure via de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

Als u wilt een certificaat koppelen aan uw rol, Geef een naam voor het certificaat. U kunt deze naam gebruiken om te verwijzen naar het certificaat wanneer u een eindpunt HTTPS op de eigenschappenpagina van de **eindpunten configureren** . Vervolgens opgeven of het certificaatarchief **Lokale computer** of de **Huidige gebruiker** en de naam van de winkel. Voer ten slotte de blauwdruk van het certificaat. Als het certificaat in de huidige User\Personal (Mijn) winkel wordt, kunt u de vingerafdruk van het certificaat door het certificaat te selecteren uit een lege lijst. Als deze zich op een andere locatie, met de hand de vingerafdrukwaarde invoeren.

Wanneer u een certificaat uit het certificaatarchief toevoegt, worden alle tussenliggende certificaten automatisch toegevoegd aan de configuratie-instellingen voor u. Deze tussenliggende certificaten moeten ook worden geüpload naar Azure alleen goed configureren uw service voor SSL.

Beheer van certificaten die u aan uw service koppelt van toepassing op de service alleen wanneer deze wordt uitgevoerd in de cloud. De service wordt uitgevoerd in de omgeving van de plaatselijke ontwikkeling, wordt een standaardindeling voor certificaten die wordt beheerd door de compute-emulator gebruikt.

## <a name="configuring-the-azure-cloud-service-project"></a>Het project Azure cloud-service configureren

U het snelmenu van dat projectknooppunt voor het eerst openen om instellingen te configureren die betrekking hebben op een hele Azure cloud service-project, en kies Eigenschappen om de eigenschappenpagina's openen. De volgende tabel ziet de eigenschap pagina's.

|Eigenschappenpagina|Beschrijving|
|---|---|
|Toepassing|Op deze pagina kunt u informatie over de versie van Azure's die dit project cloud-service gebruikt en u kunt upgraden naar de huidige versie van de hulpprogramma's weergeven.|
|Gebeurtenissen maken|Op deze pagina kunt u vooraf gebouwde en na het samenstellen van gebeurtenissen instellen.|
|Ontwikkeling|Op deze pagina kunt u configuratie-instructies samenstellen en de voorwaarden waaronder de gebeurtenissen na het samenstellen zijn uitgevoerd.|
|Web|Op deze pagina kunt u instellingen die betrekking op de webserver hebben configureren.|
