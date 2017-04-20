<properties
   pageTitle="Aan de slag met persoonlijke sjablonen | Microsoft Azure"
   description="Toevoegen, beheren en delen van uw persoonlijke sjablonen met de Azure portal de Azure CLI of PowerShell."
   services="marketplace-customer"
   documentationCenter=""
   authors="VybavaRamadoss"
   manager="asimm"
   editor=""
   tags="marketplace, azure-resource-manager"
   keywords=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/18/2016"
   ms.author="vybavar"/>

# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Aan de slag met persoonlijke sjablonen op de Azure-Portal

Een sjabloon [Azure Resource Manager](../resource-group-authoring-templates.md) is een declaratieve gebruikt voor het definiëren van uw implementatie. U kunt de bronnen voor de implementatie van een oplossing voor definiëren, en opgeven van parameters en variabelen waarmee u de invoer-waarden voor verschillende omgevingen. De sjabloon bestaat uit JSON en expressies waarmee u waarden voor de implementatie samenstellen.

Kunt u de nieuwe functie voor de **sjablonen** in de [Portal van Azure](https://portal.azure.com) samen met de provider van de resource **Microsoft.Gallery** als een extensie van de [Azure Marketplace](https://azure.microsoft.com/marketplace/) zodat gebruikers kunnen maken, beheren en implementeren van persoonlijke sjablonen uit een persoonlijke bibliotheek.

Dit document leidt u door het toevoegen, beheren en delen van een persoonlijke **sjabloon** met behulp van de Portal Azure.

## <a name="guidance"></a>Richtlijnen

De volgende suggesties kunt u volledig profiteren van **sjablonen** bij het werken met uw oplossingen:

- Een **sjabloon** is een encapsulating middel met een bronnenbeheerder sjabloon en extra metagegevens. Het gedraagt zich erg op dezelfde wijze een item in de markt. Het belangrijkste verschil is dat het een privé-item en de openbare Marketplace items niet is.
- De bibliotheek van **sjablonen** is geschikt voor gebruikers die willen hun distributie aanpassen.
- **Sjablonen** zijn geschikt voor gebruikers die een eenvoudige opslagplaats in Azure nodig hebben.
- Beginnen met een bestaande sjabloon voor Resource Manager. Sjablonen zoeken in [github](https://github.com/Azure/azure-quickstart-templates) of [sjabloon exporteren](../resource-manager-export-template.md) uit een bestaande brongroep.
- **Sjablonen** zijn gekoppeld aan de gebruiker die ze publiceert. Naam van de uitgever is zichtbaar voor iedereen die leestoegang heeft.
- **Sjablonen** zijn Resource Manager bronnen en eenmaal uitgegeven kunnen niet worden gewijzigd.

## <a name="add-a-template-resource"></a>De bron van een sjabloon toevoegen

Er zijn twee manieren een bron **sjabloon** maken in de portal Azure.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>Methode 1: Maak een nieuwe sjabloon resource uit een resourcegroep uitgevoerd

1. Navigeer naar een bestaande resourcegroep op de Azure-Portal. Selecteer de **sjabloon exporteren** in **Instellingen**.
2. Zodra de bronnenbeheerder sjabloon wordt geëxporteerd, gebruik de knop **Sjabloon opslaan** wilt opslaan in de bibliotheek van **sjablonen** . Alle details voor exportsjabloon vinden [hier](../resource-manager-export-template.md).
<br /><br />
![Resource groep exporteren](media/rg-export-portal1.PNG)  <br />

3. Selecteer de knop **sjabloon opslaan** .
<br /><br />

4. Voer de volgende gegevens:

    - Naam: de naam van de template-object (Opmerking: dit is een naam op basis van Azure Resource Manager. Alle beperkingen voor naamgeving van toepassing en kan niet worden gewijzigd nadat gemaakt).
    - Omschrijving: snelle samenvatting over de sjabloon.

    ![Sjabloon opslaan](media/save-template-portal1.PNG)  <br />

5. Klik op **Opslaan**.

    > [AZURE.NOTE] De sjabloon exporteren blade toont meldingen wanneer de geëxporteerde sjabloon die Resource Manager fouten bevat, maar is nog steeds deze Resource Manager-sjabloon opslaan in de sjablonen. Controleer of u controleren en bronnenbeheerders sjabloon problemen opgelost voordat u de geëxporteerde sjabloon voor bronbeheer opnieuw distribueren.

### <a name="b-method-2--add-a-new-template-resource-from-browse"></a>B. Methode 2: Een nieuwe sjabloon resource uit de bladeren toevoegen

U kunt ook een nieuwe **sjabloon** toevoegen vanuit scratch met de + knop toevoegen in **Bladeren > sjablonen**. U moet een naam, beschrijving en de Resource Manager sjabloon JSON.

![Sjabloon toevoegen](media/add-template-portal1.PNG)  <br />

> [AZURE.NOTE] Microsoft.Gallery is dat een huurder op basis van Azure resource provider. De bron van de sjabloon is gekoppeld aan de gebruiker die deze heeft gemaakt. Het is niet gebonden aan een bepaald abonnement. Een abonnement moet worden gekozen alleen bij het implementeren van een sjabloon.

## <a name="view-template-resources"></a>Sjabloon voor resources weergeven

Alle **sjablonen** beschikbaar zijn, kan worden gezien op **Bladeren > sjablonen**. Dit omvat **sjablonen** die u hebt gemaakt en die met u zijn gedeeld met verschillende niveaus van machtigingen. De onderstaande sectie voor [toegangsbeheer](#access-control-for-a-tenant-resource-provider) voor meer informatie.

![Sjabloon weergeven](media/view-template-portal1.PNG)  <br />

U kunt de details van een **sjabloon** weergeven door te klikken op in een item in de lijst.

![Sjabloon weergeven](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Een resource sjabloon bewerken

De stroom bewerken voor een **sjabloon** kan worden geïnitieerd door rechts te klikken op het item in de lijst of de knop bewerken te kiezen.

![Template bewerken](media/edit-template-portal1a.PNG)  <br />

U kunt de omschrijving of bronnenbeheerder sjabloontekst bewerken. U kunt de naam niet bewerken omdat het een resourcenaam Resource Manager. Wanneer u de Resource Manager sjabloon bewerken JSON we worden gevalideerd om ervoor te zorgen dat deze geldige JSON. Kies **OK** en vervolgens op **Opslaan** om de bijgewerkte sjabloon opslaan.

![Template bewerken](media/edit-template-portal2a.PNG)  <br />

Nadat de **sjabloon** is opgeslagen, ziet u een bevestiging melding.

![Template bewerken](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Een bron voor de sjabloon distribueren

U kunt een **sjabloon** die u **Leesmachtigingen** hebben op implementeren. De stroom voor de implementatie van de standaard Azure sjabloon implementatie blade gestart. Vul de waarden voor de sjabloonparameters Resource Manager om verder te gaan met de implementatie.

![Sjabloon distribueren](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Delen van een bron voor de sjabloon

De bron van een **sjabloon** kan worden gedeeld met andere gebruikers. Delen gedraagt zich op dezelfde manier [roltoewijzing voor elke bron in Azure](../active-directory/role-based-access-control-configure.md). De eigenaar van de **sjabloon** biedt machtigingen aan andere gebruikers die interactief kunnen werken met een bron voor de sjabloon. De persoon of groep personen die u delen van de **sjabloon** met is mogelijk voor een overzicht van de sjabloon Resource Manager en de eigenschappen van de galerie.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Toegangsbeheer voor de Microsoft.Gallery middelen

Rol | Machtigingen
---|----
Eigenaar | Hiermee kunt volledig beheer over de sjabloon bron aandeel
Reader | Lees- en Execute(Deploy) kunnen op de bron sjabloon
Inzender | Kan de machtiging bewerken en verwijderen op de bron sjabloon. Gebruiker delen niet de sjabloon met anderen

Door rechts te klikken of op het blad weergave van een bepaald artikel, selecteert u **delen** van het item bladeren. De ervaring van een aandeel wordt gestart.

![Sjabloon delen](media/share-template-portal1a.png)  <br />

 U kunt nu een rol en een gebruiker of groep voor toegang tot een bepaalde **sjabloon**. De beschikbare rollen zijn eigenaar, Reader en Inzender. Meer informatie in de bovenstaande sectie voor [toegangsbeheer](#access-control-for-a-tenant-resource-provider) .

![Sjabloon delen](media/share-template-portal2b.png)  <br />

![Sjabloon delen](media/share-template-portal3b.png)  <br />

Klik op **selecteren** en op **Ok**. Nu ziet u de gebruikers of groepen die u hebt toegevoegd aan de bron.

![Sjabloon delen](media/share-template-portal4b.png)  <br />

> [AZURE.NOTE] Een sjabloon kan alleen worden gedeeld met gebruikers en groepen in de pachter met dezelfde Azure Active Directory. Als u een sjabloon met een e-mailadres dat zich niet in de huurder deelt, een uitnodiging verzonden waarin de gebruiker wordt gevraagd deel te nemen aan de huurder als gast.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het maken van sjablonen voor bronbeheer [sjablonen ontwerpen](../resource-group-authoring-templates.md)
- Zie [functies van de sjabloon](../resource-group-template-functions.md) inzicht in de functies die u in een sjabloon Resource Manager gebruiken kunt
- Zie [Aanbevolen procedures voor het ontwerpen van Azure Resource Manager-sjablonen](../best-practices-resource-manager-design-templates.md) als leidraad voor het ontwerpen van uw sjablonen
