<properties
    pageTitle="Verificatie met niet-interactieve HDInsight .NET applciations maken | Microsoft Azure"
    description="Informatie over het maken van verificatie met niet-interactieve HDInsight .NET-toepassingen."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Verificatie met niet-interactieve HDInsight .NET-toepassingen maken

U kunt uw toepassing .NET Azure HDInsight onder de identiteit van de toepassing (niet-interactieve) of onder de identiteit van de aangemelde gebruiker van de toepassing (interactief) uitvoeren. Zie voor een voorbeeld van de interactieve toepassing [verzenden component/varkens/Sqoop taken HDInsight .NET SDK gebruiken](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). In dit artikel wordt beschreven hoe u verificatie voor niet-interactieve .NET-toepassing verbinding maken met Azure, HDInsight en een component-taak maken.

Vanuit uw toepassing .NET hebt u het volgende nodig:

- uw abonnement op Azure huurder-ID
- de Azure toepassing client-ID
- de Directory Azure toepassing geheime sleutel.  

Het belangrijkste proces omvat de volgende stappen uit:

2. Azure-Directory-toepassing maken.
2. Rollen toewijzen aan de AD-toepassing.
3. De clienttoepassing te ontwikkelen.


##<a name="prerequisites"></a>Vereisten

- HDInsight cluster. U kunt maken met behulp van de instructies in de [handleiding aan de slag](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). 




## <a name="create-azure-directory-application"></a>Azure-Directory-toepassing maken 
Wanneer u een Active Directory-toepassing maakt, maakt het zowel de toepassing als een service principal. U kunt de toepassing onder de identiteit van de toepassing uitvoeren.

Op dit moment moet u de klassieke Azure portal gebruiken voor het maken van een nieuwe Active Directory-toepassing. Deze mogelijkheid wordt toegevoegd aan de portal Azure in een latere versie. U kunt ook als volgt via Azure PowerShell of Azure CLI uitvoeren. Zie voor meer informatie over het gebruik van PowerShell of CLI met de service principal [verifiëren service principal Azure Resource Manager](../resource-group-authenticate-service-principal.md).

**Een Azure-Directory-toepassing maken**

1.  Log in om de [Azure klassieke portal]( https://manage.windowsazure.com/).
2.  Selecteer **Active Directory** in het linkerdeelvenster.

    ![Azure klassieke portal active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\active-directory.png)
    
3.  Selecteer de map die u gebruiken wilt voor het maken van de nieuwe toepassing. Het mag bestaande zijn.
4.  Klik op **toepassingen** van boven de bestaande toepassingen.
5.  Klik op **toevoegen** onder een nieuwe toepassing toevoegen.
6.  Voer **naam in**, selecteer de **webtoepassing en/of Web-API**en klik op **volgende**.

    ![nieuwe azure active directory-toepassing](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application.png)

7.  Voer **URL aanmelding** en **App -ID-URI**. **SIGN-ON-URL**, bieden de URI naar een website waarop de toepassing wordt beschreven. Het bestaan van de website is niet gevalideerd. Bieden voor APP-ID URI, de URI die de toepassing aangeeft. En klik vervolgens op **Voltooien**.
Duurt het enkele seconden duren voordat de toepassing maken.  Zodra de toepassing is gemaakt, ziet u de portal de snelle Glace pagina van de nieuwe toepassing. Sluit niet de portal. 

    ![nieuwe eigenschappen voor azure active directory-toepassingen](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application-properties.png)

**De toepassingsclient-ID en de geheime sleutel ophalen**

1.  Klik op **configureren** in het bovenste menu op de pagina nieuwe AD-toepassing.
2.  Maak een kopie van **De Client-ID**. Deze moet u in uw toepassing .NET.
3.  Klik op de vervolgkeuzelijst **Selecteer duur** onder **toetsen**, en selecteer **1 jaar** of **2 jaar**. De waarde van de sleutel wordt niet weergegeven totdat u de configuratie op te slaan.
4.  Klik op **Opslaan** op de onderkant van de pagina. Wanneer de geheime sleutel wordt weergegeven, moet u een kopie van de sleutel. Deze moet u in uw toepassing .NET.

##<a name="assign-ad-application-to-role"></a>AD toepassing toewijst aan rol

U moet de toepassing toewijst aan een [rol](../active-directory/role-based-access-built-in-roles.md) toe te kennen als de machtigingen voor het uitvoeren van acties. U kunt het bereik instellen op het niveau van het abonnement, resource of resourcegroep. De machtigingen worden overgenomen op lagere niveaus van het bereik (bijvoorbeeld het toevoegen van dat een toepassing aan de rol van lezer voor een bronnengroep houdt dat de resourcegroep kan worden gelezen en resources bevat). In deze zelfstudie wordt u het bereik instelt op het niveau van de resourcegroep.  Omdat de Azure klassieke portal geen resourcegroepen ondersteunt, heeft dit deel moet worden uitgevoerd vanaf de portal Azure. 

**De rol eigenaar toevoegen aan de AD-toepassing**

1.  Log in om de [Azure portal](https://portal.azure.com).
2.  Klik op **Resourcegroep** in het linkerdeelvenster.
3.  Klik op de resourcegroep met het HDInsight-cluster waarin u uw query component verderop in deze zelfstudie wordt uitgevoerd. Als er te veel resourcegroepen, kunt u het filter.
4.  **Klik op van het cluster blade.**

    ![pictogram wolk en thunderbolt = quickstart](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5.  Klik op **toevoegen** van de bladeserver **gebruikers** .
6.  Volg de aanwijzingen op de rol van **eigenaar** toevoegen aan de AD-toepassing die u hebt gemaakt in de laatste procedure. Wanneer u deze hebt voltooid is, ziet u de toepassing in de blade gebruikers met de rol van de eigenaar vermeld.


##<a name="develop-hdinsight-client-application"></a>HDInsight client-toepassingen ontwikkelen

C# .net-console een toepassing maken volgens de instructies in [Hadoop indienen projecten in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk)gevonden. Vervolgens de methode GetTokenCloudCredentials vervangen door het volgende:

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

De huurder ID via PowerShell ophalen:

    Get-AzureRmSubscription

Of CLI Azure:

    azure account show --json

      
## <a name="see-also"></a>Zie ook

- [Hadoop taken in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Active Directory-toepassing en service principal met portal maken](../resource-group-create-service-principal-portal.md)
- [Service principal Azure Resource Manager verifiëren](../resource-group-authenticate-service-principal.md)
- [Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md)
