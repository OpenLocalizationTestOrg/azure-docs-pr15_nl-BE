<properties
   pageTitle="Verificatie met Lake gegevensarchief met Active Directory | Microsoft Azure"
   description="Meer informatie over het verifiëren met Lake gegevensarchief met Active Directory"
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
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="service-to-serivce-authentication-with-data-lake-store-using-azure-active-directory"></a>Verificatie met Lake gegevensarchief met Azure Active Directory service aan serivce

> [AZURE.SELECTOR]
- [Verificatie Services](data-lake-store-authenticate-using-active-directory.md)
- [Verificatie van de eindgebruiker](data-lake-store-end-user-authenticate-using-active-directory.md)

Azure Active Directory-gegevensarchief Lake Azure gebruikt voor verificatie. Voordat u een toepassing die met Azure Lake gegevensarchief of Azure gegevens Lake Analytics werkt ontwerpen, moet u eerst bepalen hoe u wilt dat voor de verificatie van uw toepassing met Azure Active Directory (AD Azure). De twee belangrijkste opties zijn beschikbaar:

* Verificatie van de eindgebruiker, en 
* Verificatie Services. 

Deze beide opties ertoe leiden dat uw toepassing wordt geleverd met een 2.0 OAuth-token die wordt gekoppeld aan elk verzoek Azure Lake gegevensarchief of Azure gegevens Lake Analytics.

Dit artikel spreekt over hoe u maken een webtoepassing Azure AD voor verificatie Services. Zie voor instructies voor de configuratie van Azure AD toepassing voor de verificatie van de eindgebruiker [eindgebruiker verificatie met Lake gegevensarchief Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).


## <a name="prerequisites"></a>Vereisten

* Een abonnement op Azure. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
* Uw abonnement-ID. U kunt deze ophalen vanaf de Portal Azure. Het is bijvoorbeeld verkrijgbaar op de bladeserver Lake gegevensarchief account.

    ![Abonnement-ID ophalen](./media/data-lake-store-authenticate-using-active-directory/get-subscription-id.png)

* Uw domeinnaam Azure AD. U kunt deze ophalen door de muis in de rechterbovenhoek van de Portal Azure. Het screenshot hieronder de domeinnaam is **contoso.microsoft.com**en de GUID tussen haakjes is de id van de huurder. 

    ![AAD domein ophalen](./media/data-lake-store-authenticate-using-active-directory/get-aad-domain.png)

## <a name="service-to-service-authentication"></a>Verificatie Services

Dit is de aanbevolen werkwijze als u wilt dat uw toepassing worden automatisch geverifieerd met Azure AD, zonder de noodzaak van een eindgebruiker hun referenties op te geven. Uw toepassing kan zichzelf verifiëren voor zo lang als de referenties geldig zijn, die kan worden aangepast om worden in volgorde van het jaar worden.

### <a name="what-do-i-need-to-use-this-approach"></a>Wat moet ik deze aanpak gebruiken?

* Azure AD-domeinnaam. Dit wordt al vermeld in de vereiste van dit artikel.

* Azure AD- **webtoepassing**.

* Client-ID voor de webtoepassing Azure AD.

* Het geheim van de client voor de webtoepassing Azure AD.

* Het eindpunt voor de webtoepassing Azure AD token.

* Toegang voor de webtoepassing Azure AD inschakelen op de het gegevensarchief Lake bestand of de map of de gegevens Lake Analytics-account die u wilt werken.

Zie de sectie [een Active Directory-toepassing maken](#create-an-active-directory-application) hieronder voor instructies over het maken van een webtoepassing Azure AD en configureert voor de hierboven vermelde eisen.

>[AZURE.NOTE] De toepassing AD Azure is standaard geconfigureerd voor het geheim van de client, die u uit de toepassing van Azure AD ophalen kunt gebruiken. Echter, als u wilt dat de toepassing AD Azure een certificaat te gebruiken in plaats daarvan, moet u de webtoepassing van Azure AD met Azure PowerShell, zoals beschreven bij het [maken van een service principal met certificaat](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate).

## <a name="create-an-active-directory-application"></a>Een Active Directory-toepassing maken

In deze sectie we meer informatie over het maken en configureren van een webtoepassing Azure AD voor services verificatie met Azure Lake gegevensarchief Azure Active Directory wordt gebruikt. 


### <a name="step-1-create-an-azure-active-directory-application"></a>Stap 1: Een Azure Active Directory-toepassing maken

>[AZURE.NOTE] De onderstaande stappen via de Portal Azure. U kunt ook een Azure AD toepassing maken met [Azure PowerShell](../resource-group-authenticate-service-principal.md) of [Azure CLI](../resource-group-authenticate-service-principal-cli.md).

1. Log in op uw Account Azure via de [klassieke portal](https://manage.windowsazure.com/).

2. Selecteer **Active Directory** in het linkerdeelvenster.

     ![Selecteer Active Directory](./media/data-lake-store-authenticate-using-active-directory/active-directory.png)
     
3. Selecteer de Active Directory die u gebruiken wilt voor het maken van de nieuwe toepassing. Als er meer dan één Active Directory, wilt u meestal de toepassing maken in de map waarin uw abonnement staat. U kunt alleen toegang verlenen tot resources in uw abonnement voor toepassingen in dezelfde map als uw abonnement.  

     ![Kies map](./media/data-lake-store-authenticate-using-active-directory/active-directory-details.png)
    
    
3. De toepassingen in de map, klik op **toepassingen**.

     ![toepassingen weergeven](./media/data-lake-store-authenticate-using-active-directory/view-applications.png)

4. Als u nog niet hebt gemaakt een toepassing in deze map voordat u ziet iets dergelijks aan de volgende afbeelding. Klik op **een toepassing toevoegen**

     ![toepassing toevoegen](./media/data-lake-store-authenticate-using-active-directory/create-application.png)

     Of klik op **toevoegen** in het onderste deelvenster.

     ![toevoegen](./media/data-lake-store-authenticate-using-active-directory/add-icon.png)

6. Geef een naam op voor de toepassing en selecteert u het type toepassing dat u wilt maken. Voor deze zelfstudie, een **WEB toepassing en/of WEB API** maken en klik op de knop Volgende.

     ![naam toepassing](./media/data-lake-store-authenticate-using-active-directory/tell-us-about-your-application.png)

7. Vul de eigenschappen voor uw app. **SIGN-ON-URL**, bieden de URI naar een website waarop de toepassing wordt beschreven. Het bestaan van de website is niet gevalideerd. Bieden voor **APP-ID URI**, de URI die de toepassing aangeeft.

     ![Eigenschappen van toepassingen](./media/data-lake-store-authenticate-using-active-directory/app-properties.png)

    Klik op het selectievakje om de wizard hebt voltooid en de toepassing te maken.

### <a name="step-2-get-client-id-client-secret-and-token-endpoint"></a>Stap 2: De client-id, geheim client en eindpunt token ophalen

Wanneer u via programmacode zich aanmeldt, moet u de id voor uw toepassing. Als de toepassing wordt uitgevoerd onder een eigen referenties, moet u ook een verificatiesleutel.

1. Klik op het tabblad **configureren** om het wachtwoord van uw toepassing configureren.

     ![toepassing configureren](./media/data-lake-store-authenticate-using-active-directory/application-configure.png)

2. Kopieer de **CLIENT-ID**.
  
     ![client-id](./media/data-lake-store-authenticate-using-active-directory/client-id.png)

3. Als de toepassing wordt uitgevoerd onder een eigen referenties, Ga naar de sectie **sleutels** en selecteer hoe lang u wilt dat uw wachtwoord geldig zijn.

     ![toetsen](./media/data-lake-store-authenticate-using-active-directory/create-key.png)

4. Selecteer **Opslaan** om de sleutel te maken.

    ![opslaan](./media/data-lake-store-authenticate-using-active-directory/save-icon.png)

    De opgeslagen sleutel wordt weergegeven en kunt u deze kopiëren. Niet mogelijk om de sleutel later ophalen zodat het nu moet kopiëren.

    ![sleutel opgeslagen](./media/data-lake-store-authenticate-using-active-directory/save-key.png)

5. Ophalen het token eindpunt **eindpunten weergeven** onder aan het scherm selecteren en ophalen van de waarde voor het veld **OAuth 2.0 Token eindpunt** , zoals hieronder wordt weergegeven.  

    ![huurder-id](./media/data-lake-store-authenticate-using-active-directory/save-tenant.png)

### <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-store-account-file-or-folder-only-for-service-to-service-authentication"></a>Stap 3: De Azure AD toepassing toewijst aan de Azure Lake gegevensarchief accountbestand of map (alleen voor service-service-verificatie)

1. Aanmelden bij de nieuwe [Portal Azure](https://portal.azure.com) en open de Azure Lake gegevensarchief account die u wilt koppelen aan de Azure Active Directory-toepassing die u eerder hebt gemaakt.

1. In uw account Lake gegevensarchief blade, klikt u op **Gegevens**.

    ![Mappen maken in het gegevensarchief Lake account] (./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "Mappen maken in rekening voor meer gegevens")

2. Klik op het bestand of de map waarvoor u toegang te verlenen tot de toepassing Azure AD in de blade **Data Explorer** en klik op **toegang**. Configureren van de toegang tot een bestand, klikt u op **toegang** van het blad **Bestandsvoorbeeld** .

    ![ACL's instellen voor meer gegevens] (./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "ACL's instellen voor meer gegevens")

3. De blade **toegang** geeft een overzicht van de standaard- en aangepaste toegang al is toegewezen aan de hoofdmap. Klik op het pictogram **toevoegen** om een aangepast niveau ACL's toevoegen.

    ![Lijst met standaard- en aangepaste toegang] (./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "Lijst met standaard- en aangepaste toegang")

4. Klik op het pictogram **toevoegen** om de bladeserver met **Aangepaste toegang toevoegen** . In deze blade, klikt u op **gebruiker of groep selecteren**en in de blade, **gebruiker of groep selecteren** , zoekt u de beveiligingsgroep die u eerder in Azure Active Directory gemaakt. Gebruik het tekstvak boven hebt u veel groepen wilt zoeken, kunt u filteren op de naam van de groep. Klik op de groep die u wilt toevoegen en klik vervolgens op **selecteren**.

    ![Een groep toevoegen] (./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "Een groep toevoegen")

5. Klik op **Machtigingen selecteren**en selecteer de machtigingen of u wilt dat de machtigingen toewijzen als een standaard ACL, access-ACL, of beide. Klik op **OK**.

    ![Machtigingen toewijzen aan de groep] (./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "Machtigingen toewijzen aan de groep")

    Voor meer informatie over machtigingen voor gegevensopslag Lake en ACL's / toegang Zie [Toegangsbeheer in Lake gegevensarchief](data-lake-store-access-control.md).


6. Klik op **OK**in het blad met **Aangepaste toegang toevoegen** . De nieuwe groep, met de bijbehorende machtigingen wordt nu in het blad **Access** worden weergegeven.

    ![Machtigingen toewijzen aan de groep] (./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "Machtigingen toewijzen aan de groep") 


## <a name="next-steps"></a>Volgende stappen

In dit artikel een Azure AD-webtoepassing wordt gemaakt en die worden verzameld door de informatie die u nodig hebt in uw toepassingen te ontwerpen met behulp van de SDK voor .NET, Java SDK, enz. U kunt nu doorgaan met de volgende artikelen die praten over het gebruik van de webtoepassing Azure AD eerst worden geverifieerd met het gegevensarchief Lake en vervolgens andere bewerkingen uitvoeren op de winkel.

- [Aan de slag met Azure Lake gegevensarchief maken met .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Aan de slag met Azure Lake gegevensarchief maken met Java-SDK](data-lake-store-get-started-java-sdk.md)
