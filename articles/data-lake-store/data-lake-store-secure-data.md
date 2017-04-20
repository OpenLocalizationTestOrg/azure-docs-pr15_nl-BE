<properties 
   pageTitle="Beveiligen van gegevens die zijn opgeslagen in de gegevensopslag Lake Azure | Microsoft Azure" 
   description="Informatie over het beveiligen van gegevens in Azure Lake gegevensopslag met groepen en toegangsbeheerlijsten" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/29/2016"
   ms.author="nitinme"/>

# <a name="securing-data-stored-in-azure-data-lake-store"></a>Beveiligen van gegevens die zijn opgeslagen in Azure Lake gegevensarchief

Beveiliging van gegevens in de gegevensopslag Lake Azure is een benadering van drie stappen.

1. Begin met het maken van beveiligingsgroepen in Azure Active Directory (AAD). Deze beveiligingsgroepen worden gebruikt voor het implementeren van op rollen gebaseerde toegangscontrole (RBAC) in Azure Portal. Voor meer informatie Zie [Toegangsbeheer op basis van functies in Microsoft Azure](../active-directory/role-based-access-control-configure.md).

2. De AAD beveiligingsgroepen toewijzen aan de account Azure Lake gegevensarchief. Dit bepaalt de toegang naar de rekening van het gegevensarchief Lake vanaf de portal- en bewerkingen van de portal of API's.

3. De beveiligingsgroepen AAD Access toegangsbeheerlijsten (ACL's) op het bestandssysteem Lake gegevensarchief toewijzen.

4. Daarnaast kunt u ook een IP-adresbereik instellen voor clients die toegang hebben tot de gegevens in het gegevensarchief Lake.

Dit artikel bevat instructies voor het gebruik van de portal Azure de bovenstaande taken uit te voeren. Zie [beveiliging in Azure Lake gegevensarchief](data-lake-store-security-overview.md)voor gedetailleerde informatie over hoe Lake gegevensarchief wordt geïmplementeerd met beveiliging op het niveau en de gegevens. Zie [Overzicht van toegangsbeheer in Lake gegevensarchief](data-lake-store-access-control.md)voor uitvoerige informatie over hoe ACL's zijn geïmplementeerd in Azure Lake gegevensarchief.

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
- **Een Azure Lake gegevensarchief account**. Zie voor instructies voor het maken van een [aan de slag met Azure Lake gegevensarchief](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Beveiligingsgroepen in Azure Active Directory maken

Zie voor instructies voor het maken van beveiligingsgroepen AAD en het toevoegen van gebruikers aan de groep, [beveiligingsgroepen beheren in Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Gebruikers of beveiligingsgroepen toe te wijzen aan Azure Lake gegevensarchief

Wanneer u gebruikers of beveiligingsgroepen aan accounts Azure Lake gegevensarchief toegewezen, bepaalt u de toegang tot de beheertaken uit te voeren op de rekening met de Azure portal en Azure Resource Manager API's. 

1. Open een account Azure Lake gegevensarchief. Klik op **Bladeren**in het linkerdeelvenster naar **Lake gegevensopslag**en klik vervolgens op de accountnaam die u wilt toewijzen een gebruikers- of groep uit de blade Lake gegevensarchief.

2. Klik op **Instellingen**in het gegevensarchief Lake account-blade. Klik op **gebruikers**van de bladeserver **Instellingen** .

    ![Beveiligingsgroep met Azure Lake gegevensarchief account toewijzen] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Beveiligingsgroep met Azure Lake gegevensarchief account toewijzen")

3. De blade **gebruiker** standaard worden beheerdersgroep **abonnement** als eigenaar. 

    ![Gebruikers toevoegen en rollen] (./media/data-lake-store-secure-data/adl.add.group.roles.png "Gebruikers toevoegen en rollen")
 
    Er zijn twee manieren aan een groep toevoegen en relevante rollen toewijzen.

    * Een gebruiker of groep toevoegen aan de account en vervolgens een rol toe te wijzen of
    * Een rol toevoegen en vervolgens gebruikers/groepen aan rollen toewijzen.

    In deze sectie bekijken we de eerste benadering, een groep toe te voegen en vervolgens rollen toewijzen. U kunt vergelijkbare stappen voor het eerst een rol te selecteren en vervolgens groepen toewijzen aan die rol uitvoeren.
    
4. Klik op **toevoegen** om de bladeserver **toegang toevoegen** in het blad **gebruikers** . Klikt u op **Selecteer een rol**in de blade **toegang toevoegen** en selecteer een rol voor de gebruikersgroep.

     ![Een rol voor de gebruiker toevoegen] (./media/data-lake-store-secure-data/adl.add.user.1.png "Een rol voor de gebruiker toevoegen")

    De **eigenaar** en de rol van **Inzender** bieden toegang tot een aantal functies voor beheer op de account gegevens lake. Voor gebruikers die met gegevens in het meer gegevens werken zullen, kunt u deze toevoegen aan de rol van **lezer **. Het toepassingsgebied van deze rollen is beperkt tot de beheerswerkzaamheden gekoppeld aan de account Azure Lake gegevensarchief.

    Voor definiëren afzonderlijke bestandssysteemmachtigingen bewerkingen wat gebruikers kunnen doen. Daarom een gebruiker met de rol van lezer kunt beheerinstellingen voor de account alleen bekijken, maar kan mogelijk gegevens lezen en schrijven op basis van de machtigingen die aan hen zijn toegewezen. Lake gegevensarchief bestandssysteemmachtigingen worden beschreven bij het [toewijzen van ACL's naar het bestandssysteem Azure Lake gegevensarchief beveiligingsgroep](#filepermissions).



5. Klik op **gebruikers toevoegen** als u wilt openen de blade **gebruikers toevoegen** in het blad **toegang toevoegen** . Zoek in deze blade, de beveiligingsgroep die u eerder in Azure Active Directory gemaakt. Gebruik het tekstvak boven hebt u veel groepen wilt zoeken, kunt u filteren op de naam van de groep. Klik op **selecteren**.

    ![Een beveiligingsgroep toevoegen] (./media/data-lake-store-secure-data/adl.add.user.2.png "Een beveiligingsgroep toevoegen")

    Als u toevoegen van een groep/gebruiker die niet wordt vermeld wilt, kunt u ze uitnodigen door het pictogram **uitnodigen** en geven het e-mailadres voor de gebruikersgroep.

6. Klik op **OK**. Hier ziet u de beveiligingsgroep toegevoegd zoals hieronder wordt weergegeven.

    ![Beveiligingsgroep toegevoegd] (./media/data-lake-store-secure-data/adl.add.user.3.png "Beveiligingsgroep toegevoegd")

7. De gebruiker/groep heeft nu toegang tot de account Azure Lake gegevensarchief. Als u toegang verlenen aan specifieke gebruikers wilt, kunt u deze toevoegen aan de beveiligingsgroep. Op dezelfde manier als u toegang voor een gebruiker intrekken wilt, kunt u ze uit de beveiligingsgroep. U kunt ook meerdere beveiligingsgroepen toewijzen aan een account. 

## <a name="filepermissions"></a>Gebruikers of beveiligingsgroep als ACL's toewijzen aan het bestandssysteem Azure Lake gegevensarchief

Gebruiker/beveiligingsgroepen aan het bestandssysteem Azure gegevens Lake toewijst, moet u toegangsbeheer voor de gegevens in de gegevensopslag Lake Azure instellen.

1. In uw account Lake gegevensarchief blade, klikt u op **Gegevens**.

    ![Mappen maken in het gegevensarchief Lake account] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Mappen maken in rekening voor meer gegevens")

2. Klik op het bestand of de map die u wilt configureren de ACL in de blade **Data Explorer** en klik op **toegang**. ACL toewijzen aan een bestand, klikt u op **toegang** van het blad **Bestandsvoorbeeld** .

    ![ACL's instellen voor meer gegevens] (./media/data-lake-store-secure-data/adl.acl.1.png "ACL's instellen voor meer gegevens")

3. De blade **toegang** geeft een overzicht van de standaard- en aangepaste toegang al is toegewezen aan de hoofdmap. Klik op het pictogram **toevoegen** om een aangepast niveau ACL's toevoegen.

    ![Lijst met standaard- en aangepaste toegang] (./media/data-lake-store-secure-data/adl.acl.2.png "Lijst met standaard- en aangepaste toegang")

    * **Standaard toegang** , is de toegang UNIX-stijl waar u lezen, schrijven, uitvoeren (rwx) naar drie verschillende gebruikersklassen: eigenaar, groep en anderen.
    * **Aangepaste toegang** correspondeert met de POSIX-ACL's waarmee u machtigingen kunt instellen voor een specifiek benoemde gebruikers of groepen, en niet alleen van het bestand eigenaar of groep. 
    
    Zie [HDFS ACL's](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists)voor meer informatie. Zie [Toegangsbeheer in Lake gegevensarchief](data-lake-store-access-control.md)voor meer informatie over hoe ACL's in Lake gegevensopslag worden geïmplementeerd.

4. Klik op het pictogram **toevoegen** om de bladeserver met **Aangepaste toegang toevoegen** . In deze blade, klikt u op **gebruiker of groep selecteren**en in de blade, **gebruiker of groep selecteren** , zoekt u de beveiligingsgroep die u eerder in Azure Active Directory gemaakt. Gebruik het tekstvak boven hebt u veel groepen wilt zoeken, kunt u filteren op de naam van de groep. Klik op de groep die u wilt toevoegen en klik vervolgens op **selecteren**.

    ![Een groep toevoegen] (./media/data-lake-store-secure-data/adl.acl.3.png "Een groep toevoegen")

5. Klik op **Machtigingen selecteren**en selecteer de machtigingen of u wilt dat de machtigingen toewijzen als een standaard ACL, access-ACL, of beide. Klik op **OK**.

    ![Machtigingen toewijzen aan de groep] (./media/data-lake-store-secure-data/adl.acl.4.png "Machtigingen toewijzen aan de groep")

    Voor meer informatie over machtigingen voor gegevensopslag Lake en ACL's / toegang Zie [Toegangsbeheer in Lake gegevensarchief](data-lake-store-access-control.md).


6. Klik op **OK**in het blad met **Aangepaste toegang toevoegen** . De nieuwe groep, met de bijbehorende machtigingen wordt nu in het blad **Access** worden weergegeven.

    ![Machtigingen toewijzen aan de groep] (./media/data-lake-store-secure-data/adl.acl.5.png "Machtigingen toewijzen aan de groep")

    > [AZURE.IMPORTANT] U kunt alleen 9 vermeldingen onder **Aangepaste toegang**hebben in de huidige versie. Als u wilt dat meer dan 9 gebruikers toe te voegen, moet u beveiligingsgroepen maken gebruikers toevoegen aan beveiligingsgroepen, toevoegen bieden toegang tot deze gegevens over beveiligingsgroepen voor het gegevensarchief Lake-account.

7. Indien nodig kunt u de toegangsmachtigingen ook wijzigen nadat u de groep hebt toegevoegd. Schakel het selectievakje in voor elk machtigingstype (lezen, schrijven, uitvoeren) op basis van de vraag of u wilt verwijderen of deze machtiging toewijzen aan de groep of uit. Klik op **Opslaan** om de wijzigingen opslaan of **verwijderen** om de wijzigingen ongedaan te maken.

## <a name="set-ip-address-range-for-data-access"></a>Set IP-adresbereik voor toegang tot gegevens

Azure Lake gegevensarchief kunt u verdere vergrendelen van toegang tot de gegevensopslag op netwerkniveau van het. U kunt firewall inschakelt, geeft u een IP-adres of een IP-adresbereik definiëren voor uw vertrouwde clients. Eenmaal is ingeschakeld, worden alleen clients de IP-adressen binnen het gedefinieerde bereik kunnen verbinden met de winkel.

![Firewall-instellingen en de IP-toegang] (./media/data-lake-store-secure-data/firewall-ip-access.png "Firewall-instellingen en het IP-adres")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Beveiligingsgroepen voor een gegevensarchief voor Lake Azure-account verwijderen

Wanneer u beveiligingsgroepen van Azure Lake gegevensarchief accounts verwijdert, wijzigt u alleen toegang tot de beheertaken uit te voeren op de rekening met de Portal Azure en Azure Resource Manager API's.

1. Klik op **Instellingen**in het gegevensarchief Lake account-blade. Klik op **gebruikers**van de bladeserver **Instellingen** .

    ![Beveiligingsgroep met Azure gegevens Lake account toewijzen] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Beveiligingsgroep met Azure gegevens Lake account toewijzen")

2. Klik in het blad van de **gebruikers** op de beveiligingsgroep die u wilt verwijderen.

    ![Groep verwijderen] (./media/data-lake-store-secure-data/adl.add.user.3.png "Groep verwijderen")

3. In het blad voor de beveiligingsgroep, klik op **verwijderen**.

    ![Groep verwijderen] (./media/data-lake-store-secure-data/adl.remove.group.png "Groep verwijderen")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Beveiligingsgroep ACL's van bestandssysteem Azure Lake gegevensarchief verwijderen

Wanneer u ACL's voor beveiligingsgroepen uit bestandssysteem Azure Lake gegevensarchief verwijderen, wijzigt u toegang tot de gegevens in het gegevensarchief Lake.

1. In uw account Lake gegevensarchief blade, klikt u op **Gegevens**.

    ![Mappen maken in rekening voor meer gegevens] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Mappen maken in rekening voor meer gegevens")

2. Klik op het bestand of de map die u wilt verwijderen van de ACL in de blade **Data Explorer** en klikt u vervolgens op het pictogram van **Access** in uw account-blade. Om de ACL voor een bestand wilt verwijderen, klikt u op **toegang** van het blad **Bestandsvoorbeeld** .

    ![ACL's instellen voor meer gegevens] (./media/data-lake-store-secure-data/adl.acl.1.png "ACL's instellen voor meer gegevens")

3. Klik op de beveiligingsgroep die u wilt verwijderen in de blade **toegang** vanuit de sectie **Aangepaste toegang** . In de **Aangepaste Access** -blade klikt u op **verwijderen** en klik vervolgens op **OK**.

    ![Machtigingen toewijzen aan de groep] (./media/data-lake-store-secure-data/adl.remove.acl.png "Machtigingen toewijzen aan de groep")


## <a name="see-also"></a>Zie ook

- [Overzicht van Azure Lake gegevensarchief](data-lake-store-overview.md)
- [Gegevens kopiëren van Azure Storage Blobs naar Lake gegevensarchief](data-lake-store-copy-data-azure-storage-blob.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensarchief](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensarchief](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Aan de slag met Lake gegevensarchief met PowerShell](data-lake-store-get-started-powershell.md)
- [Aan de slag met Lake gegevensarchief met .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Diagnostische logboeken voor gegevensopslag Lake](data-lake-store-diagnostic-logs.md)
