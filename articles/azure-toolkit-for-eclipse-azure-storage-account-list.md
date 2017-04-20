<properties
    pageTitle="Lijst met Azure opslag"
    description="Met de Toolkit Azure voor Eclips Accountinstellingen van de opslag beheren"
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->

# <a name="azure-storage-account-list"></a>Lijst met Azure opslag #

Azure opslag rekeningen inschakelen downloadlocaties die moet worden gebruikt voor de JDK, toepassingsserver en willekeurige onderdelen en staat op te slaan wanneer u in cache opslaan. Eclips houdt een lijst van bekende opslag accounts die beschikbaar voor uw projecten in uw werkruimte Eclips zijn. U opent het dialoogvenster **Accounts van de opslag** die wordt gebruikt voor het beheren van die lijst, binnen de Eclips, klikt u op **venster**, klikt u op **Voorkeuren**, **Azure**uitbreiden en klik op **Opslag rekeningen**.

Hieronder ziet u het dialoogvenster **Accounts van de opslag** .

![][ic719496]

In dit dialoogvenster kan ook worden geopend via een koppeling **Accounts** in dialoogvensters met accounts voor opslag, zoals de volgende:

* De **JDK** -tabblad van het dialoogvenster **Configuratie van de Server** .
* Ga naar het tabblad **Server** van het dialoogvenster **Configuratie van de Server** .
* Het dialoogvenster **Component toevoegen** .
* Het dialoogvenster met eigenschappen voor **opslaan in cache** .

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>Voor het importeren van uw opslag rekeningen met behulp van een bestand publiceren ##

1. Klik op **importeren uit bestand publiceren-instellingen**in het dialoogvenster **Accounts van de opslag** .
2. (Sla deze stap over als u al een bestand publiceren naar uw lokale computer hebt opgeslagen.) Klik in het dialoogvenster **Gegevens importeren-abonnement** , **Publicatie-instellingen bestand downloaden**. Als u nog niet aangemeld bij uw account Azure, wordt u gevraagd aan te melden. Vervolgens wordt u gevraagd te publiceren instellingenbestand opslaan een Azure. (Kunt u de resulterende instructies op de pagina logon - negeren ze worden geleverd door de portal Azure en zijn bedoeld voor gebruikers van Visual Studio.) Opslaan op uw lokale computer.
3. **Nog steeds in het dialoogvenster **Gegevens importeren-abonnement** , klikt u op de knop **Bladeren** en selecteer het bestand publiceren met instellingen die u eerder lokaal opgeslagen.**
4. Klik op **OK** om de **Importgegevens abonnement** dialoogvenster te sluiten.

## <a name="to-create-a-new-storage-account"></a>Een nieuwe opslag-account maken ##

1. Klik op **toevoegen**in het dialoogvenster **Accounts van de opslag** .
2. Klik op **Nieuw**in het dialoogvenster **Opslag Account toevoegen** .
3. Geef waarden voor de volgende opties in het dialoogvenster **Nieuwe opslag-Account** :
    * De naam van opslag.
    * De locatie van de opslag-account.
    * Beschrijving van de opslag-account.
    * Het abonnement waarvan de opslag account deel uitmaakt.
4. Klik op **OK** om het dialoogvenster **Nieuwe opslag Account** te sluiten.

Het kan enkele minuten duren voordat uw account opslag moet worden gemaakt. Nadat deze is gemaakt, klikt u op **OK** om het dialoogvenster **Opslag Account toevoegen** te sluiten en de nieuwe opslag-account wordt toegevoegd aan de lijst met beschikbare opslagruimte accounts.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Een bestaande account voor opslag toevoegen aan de lijst ##

1. Als u nog een account maken door de stappen die in het **maken van een nieuwe opslag account sectie** boven Azure opslag. (Ook kunt u een nieuwe account voor opslag op de [Azure beheerportal][].)
2. Klik op **toevoegen**in het dialoogvenster **Accounts van de opslag** .
3. Voer de waarden voor **naam** en **Toegangstoets**van **Opslag Account toevoegen** in het dialoogvenster. De account naam en toegang tot de sleutel moet voor een bestaande account voor Azure opslag zijn. Met de sectie **opslag** van [Azure Management Portal][] kunt u de namen van opslag en sleutels. Uw **Opslag Account toevoegen** dialoogvenster ziet er ongeveer als volgt.

    ![][ic719497]

4. Klik op **OK** om het dialoogvenster **Opslag Account toevoegen** te sluiten.

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>Voor het wijzigen van een opslag-account als u wilt een nieuwe sleutel van access gebruiken ##

1. Klik op de account van de opslag die u wilt bewerken en klik vervolgens op **bewerken**in het dialoogvenster **Accounts van de opslag** .
2. In het dialoogvenster **Bewerken Storage Account Access Key** de **Toegangstoets** -waarde te wijzigen.
3. Klik op **OK** om het dialoogvenster **Opslag Account toegangstoets bewerken** te sluiten.

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>Een opslag account verwijderen uit de lijst die wordt bijgehouden in een Eclips ##

1. Klik op de opslag-account die u wilt bewerken en klik vervolgens op **verwijderen**in het dialoogvenster **Accounts van de opslag** .
2. Klik op **OK** wanneer u wordt gevraagd om de opslag-account te verwijderen.

>[AZURE.NOTE] Verwijderen van de account opslag via het dialoogvenster **Opslag rekeningen** alleen verwijderd uit de lijst met accounts voor opslag binnen Eclips zichtbaar. Het wordt de opslag-account niet verwijderd uit uw abonnement Azure. Bovendien de opslag account kan opnieuw worden weergegeven in uw lijst nadat Eclips de details van uw abonnement laadt.

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Installatie van de Toolkit Azure voor Eclips][] 

[Een toepassing Hallo wereld maakt voor Azure in Eclips][]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Een toepassing Hallo wereld maakt voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Toolkit Azure voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png
