<properties
   pageTitle="Service principal in een portal maken | Microsoft Azure"
   description="Beschrijft hoe u een nieuwe Active Directory-toepassing en service principal die toegang tot bronnen te beheren met de functie van toegangsbeheer op basis van Azure Resource Manager kan worden gebruikt."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/07/2016"
   ms.author="tomfitz"/>

# <a name="use-portal-to-create-active-directory-application-and-service-principal-that-can-access-resources"></a>Portal gebruiken om Active Directory-toepassing en service principal die toegang bronnen tot te maken

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Wanneer u een toepassing die u wilt openen of wijzigen van bronnen hebt, moet u de benodigde machtigingen toewijzen aan en een toepassing van Active Directory (AD) instellen. In dit onderwerp wordt beschreven hoe u deze stappen uitvoert via de portal. Op dit moment moet u de klassieke portal een nieuwe Active Directory-toepassing maken en vervolgens overschakelen naar de portal Azure een rol toekennen aan de toepassing. 

> [AZURE.NOTE] De stappen in dit onderwerp zijn alleen van toepassing wanneer de AD-toepassing maken met de **klassieke portal** . **Als u de Azure portal gebruikt voor het maken van de toepassing AD, mislukt deze stappen.** 
>
> U kan het handiger om in te stellen uw AD toepassing en service principal via [PowerShell](resource-group-authenticate-service-principal.md) of [Azure CLI](resource-group-authenticate-service-principal-cli.md), vooral als u wilt een certificaat gebruiken voor verificatie. In dit onderwerp wordt niet weergegeven voor het gebruik van een certificaat.

Zie voor meer informatie over Active Directory-begrippen, [toepassing en Service Principal-objecten](./active-directory/active-directory-application-objects.md). Voor meer informatie over Active Directory-verificatie Zie [Verificatie scenario's voor Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Zie [handleiding voor ontwikkelaars op toelating met de API Azure Resource Manager](resource-manager-api-authentication.md)voor gedetailleerde stappen over het integreren van een toepassing in Azure voor het beheren van bronnen.

## <a name="create-an-active-directory-application"></a>Een Active Directory-toepassing maken

1. Log in op uw Account Azure via de [klassieke portal](https://manage.windowsazure.com/). 

2. Zorg ervoor dat u weet dat de standaard Active Directory voor uw abonnement. U kunt alleen toegang voor toepassingen in dezelfde map als uw abonnement. Selecteer **Instellingen** en zoek de naam van de map die is gekoppeld aan uw abonnement.  Zie [hoe Azure abonnementen zijn gekoppeld aan Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)voor meer informatie.
   
     ![de standaardmap vinden](./media/resource-group-create-service-principal-portal/show-default-directory.png)

2. Selecteer **Active Directory** in het linkerdeelvenster.

     ![Selecteer Active Directory](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. Selecteer de Active Directory die u gebruiken wilt voor het maken van de toepassing. Als er meer dan één Active Directory, moet u de toepassing maken in de standaardmap voor uw abonnement.   

     ![Kies map](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
3. Als de toepassingen in uw directory weergeven, selecteer **toepassingen**.

     ![toepassingen weergeven](./media/resource-group-create-service-principal-portal/view-applications.png)

4. Als u een toepassing hebt gemaakt in de desbetreffende map voordat, ziet u iets dat lijkt op de volgende afbeelding. Selecteer **een toepassing toevoegen**

     ![toepassing toevoegen](./media/resource-group-create-service-principal-portal/create-application.png)

     Of klik op **toevoegen** in het onderste deelvenster.

     ![toevoegen](./media/resource-group-create-service-principal-portal/add-icon.png)

5. Selecteer het type toepassing dat u wilt maken. Selecteer **Mijn organisatie ontwikkelt toepassing toevoegen**voor deze zelfstudie. 

     ![nieuwe toepassing](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. Geef een naam op voor de toepassing en selecteert u het type toepassing dat u wilt maken. Voor deze zelfstudie, een **WEB toepassing en/of WEB API** maken en klik op de knop Volgende. Als u de **Oorspronkelijke CLIENTTOEPASSING**selecteert, worden de resterende stappen in dit artikel niet overeen met uw ervaring.

     ![naam toepassing](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. Vul de eigenschappen voor uw app. **SIGN-ON-URL**, bieden de URI naar een website waarop de toepassing wordt beschreven. Het bestaan van de website is niet gevalideerd. Bieden voor **APP-ID URI**, de URI die de toepassing aangeeft.

     ![Eigenschappen van toepassingen](./media/resource-group-create-service-principal-portal/app-properties.png)

U kunt uw toepassing hebt gemaakt.

## <a name="get-client-id-and-authentication-key"></a>Client-id en verificatie sleutel ophalen

Wanneer u via programmacode zich aanmeldt, moet u de id voor uw toepassing. Als de toepassing wordt uitgevoerd onder een eigen referenties, moet u ook een verificatiesleutel.

1. Selecteer in het tabblad **configureren** om het wachtwoord van uw toepassing configureren.

     ![toepassing configureren](./media/resource-group-create-service-principal-portal/application-configure.png)

2. Kopieer de **CLIENT-ID**.
  
     ![client-id](./media/resource-group-create-service-principal-portal/client-id.png)

3. Als de toepassing wordt uitgevoerd onder een eigen referenties, Ga naar de sectie **sleutels** en selecteer hoe lang u wilt dat uw wachtwoord geldig zijn.

     ![toetsen](./media/resource-group-create-service-principal-portal/create-key.png)

4. Selecteer **Opslaan** om de sleutel te maken.

     ![opslaan](./media/resource-group-create-service-principal-portal/save-icon.png)

     De opgeslagen sleutel wordt weergegeven en kunt u deze kopiëren. U bent niet ophalen van de sleutel later zo nu kopiëren.

     ![sleutel opgeslagen](./media/resource-group-create-service-principal-portal/save-key.png)

## <a name="get-tenant-id"></a>Huurder-id ophalen

Wanneer u via programmacode zich aanmeldt, moet u de id van de huurder met de verificatieaanvraag doorgeeft. Voor Web Apps en Apps voor Web-API, kunt u de huurder-id ophalen door de **eindpunten van de weergave** onder aan het scherm selecteren en ophalen van de id, zoals in de volgende afbeelding.  

   ![huurder-id](./media/resource-group-create-service-principal-portal/save-tenant.png)

U kunt ook de huurder id via PowerShell ophalen:

    Get-AzureRmSubscription

Of CLI Azure:

    azure account show --json

## <a name="set-delegated-permissions"></a>Overgedragen machtigingen

Als uw toepassing toegang heeft tot bronnen een gebruiker ingelogd, moet u de toepassing de machtiging gedelegeerde toegang krijgen tot andere toepassingen. U verleent deze toegang in de sectie **machtigingen voor andere toepassingen** van het tabblad **configureren** . Een gemachtigde toestemming is standaard al ingeschakeld voor Azure Active Directory. Deze gedelegeerde machtigingen ongewijzigd laat.

1. Selecteer **de toepassing toevoegen**.

2. Selecteer in de lijst, de **API van Windows Azure Service Management**. Selecteer het pictogram voltooid.

      ![Selecteer de app](./media/resource-group-create-service-principal-portal/select-app.png)

3. Selecteer in de vervolgkeuzelijst voor de overgedragen machtigingen **Toegang Azure Service Management organisatie**.

      ![Schakel het selectievakje machtiging](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. De wijziging opslaan.

## <a name="assign-application-to-role"></a>Toepassing toewijst aan een rol

Als uw toepassing wordt uitgevoerd onder een eigen referenties, moet u de toepassing aan een rol toewijzen. Bepalen welke rol de juiste machtigingen voor de toepassing vertegenwoordigt. Zie voor meer informatie over de beschikbare functies [RBAC: rollen ingebouwd](./active-directory/role-based-access-built-in-roles.md). 

Een rol toewijzen aan een toepassing, moet u de juiste machtigingen hebben. Met name moet u beschikken over `Microsoft.Authorization/*/Write` toegang tot en met de rol van de [eigenaar](./active-directory/role-based-access-built-in-roles.md#owner) of de rol [Beheerder toegang](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) wordt verleend. De rol van Inzender beschikt niet over de juiste toegang.

U kunt het bereik instellen op het niveau van het abonnement, resource of resourcegroep. Machtigingen worden overgenomen op lagere niveaus van de scope. Bijvoorbeeld, een toepassing aan de rol van lezer voor een resourcegroep toe te voegen dat kan gelezen de bronnengroep en bronnen bevat.

1. De toepassing aan een rol toewijzen, overschakelen van de klassieke portal de [Azure portal](https://portal.azure.com).

1. Controleer uw machtigingen om ervoor te zorgen dat u de service principal kunt toewijzen aan een rol. Selecteer **de machtigingen** voor uw account.

    ![Selecteer Mijn machtigingen](./media/resource-group-create-service-principal-portal/my-permissions.png)

1. De toegewezen machtigingen voor uw account bekijken. Zoals eerder is opgemerkt, moeten behoren tot de eigenaar of beheerder toegang rollen of een aangepaste rol die schrijftoegang voor Microsoft.Authorization verleent. De volgende afbeelding ziet u een account die is toegewezen aan de rol van inzender voor het abonnement, niet voldoende machtigingen is voor een toepassing toewijst aan een rol.

    ![Mijn machtigingen weergeven](./media/resource-group-create-service-principal-portal/show-permissions.png)

     Als u niet de juiste machtigingen om toegang te verlenen aan een toepassing, moet u een aanvraag die de beheerder van uw abonnement u toegevoegd aan de rol beheerder toegang hebben, of een aanvraag die verleent een beheerder toegang tot de toepassing.

1. Ga naar het niveau van de scope die u wilt toewijzen van de toepassing. Selecteer een rol op de scope abonnement toewijzen, **abonnementen**.

     ![Selecteer abonnement](./media/resource-group-create-service-principal-portal/select-subscription.png)

     Selecteer het abonnement bepaald voor het toewijzen van de toepassing.

     ![Selecteer abonnement voor toewijzing](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

     Selecteer het **Access** -pictogram in de rechterbovenhoek.

     ![Selecteer toegang](./media/resource-group-create-service-principal-portal/select-access.png)
     
     Of een rol op het groepsbereik resource toewijzen, gaat u naar een groep. Selecteer de resource groep blade, **Access control**.

     ![gebruikers selecteren](./media/resource-group-create-service-principal-portal/select-users.png)

     De volgende stappen zijn hetzelfde voor elke scope.

2. Selecteer **toevoegen**.

     ![Selecteer toevoegen](./media/resource-group-create-service-principal-portal/select-add.png)

3. Selecteer de rol van **lezer** (of welke rol wilt toewijzen van de toepassing).

     ![Selecteer de rol](./media/resource-group-create-service-principal-portal/select-role.png)

4. Wanneer u eerst de lijst met gebruikers die u aan de rol toevoegen kunt ziet, ziet u geen toepassingen. U ziet alleen groepen en gebruikers.

     ![gebruikers weergeven](./media/resource-group-create-service-principal-portal/show-users.png)

5. Uw toepassing vindt moet u zoeken. Typ de naam van de toepassing en de lijst van beschikbare opties wordt gewijzigd. Selecteer uw toepassing als u in de lijst.

     ![toewijzen aan een rol](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. Selecteer **OK** voltooien de rol toewijzen. U ziet nu de toepassing in de lijst van toepassingen die zijn toegewezen aan een rol voor de resourcegroep.


Zie voor meer informatie over het toewijzen van gebruikers en toepassingen via de portal [roltoewijzingen gebruiken voor het beheren van toegang tot de bronnen van uw abonnement op Azure](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal).

## <a name="sample-applications"></a>Voorbeeldtoepassingen

De volgende voorbeeldtoepassingen hoe aan te melden als de service principal.

**.NET**

- [Implementeren van een SSH VM met een sjabloon met .NET ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Azure resources en resourcegroepen met .NET beheren](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Aan de slag met - implementeren met behulp van Azure Resource Manager sjabloon - bronnen in Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Aan de slag met Resources - groep beheren - in Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Implementeren van een SSH VM met een sjabloon in Python ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Azure resources en resourcegroepen met Python beheren](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Implementeren van een SSH VM met een sjabloon in Node.js ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure resources en resourcegroepen met Node.js beheren](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Implementeren van een SSH VM met een sjabloon in Ruby ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Azure resources en resourcegroepen met Ruby beheren](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het opgeven van beveiligingsbeleid, [Azure op rollen gebaseerde toegangscontrole](./active-directory/role-based-access-control-configure.md).  
- Zie [Programmatisch beheer van een Resource Azure met Azure Active Directory inschakelen](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory)voor een video demonstratie van deze stappen.

