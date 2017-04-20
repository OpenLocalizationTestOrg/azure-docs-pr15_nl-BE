<properties
   pageTitle="Azure analyseservices beheren | Microsoft Azure"
   description="Informatie over het beheren van een Analysis Services-server in Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="manage-analysis-services"></a>Analyseservices beheren

Als u een Analysis Services-server hebt gemaakt in Azure, worden bepaalde beheers- en taken u moet meteen of ergens onder de weg. Uitvoeren bijvoorbeeld het verwerken van de gegevens vernieuwen, bepalen wie kan toegang krijgen tot de modellen die op de server of de status van de server kunt controleren. Sommige taken kunnen alleen worden uitgevoerd in Azure portal voor anderen in SQL Server Management Studio (SSMS), en sommige taken kunnen worden uitgevoerd in een.

## <a name="azure-portal"></a>Azure portal
De [portal Azure](http://portal.azure.com/) is waar u kunt maken en verwijderen van servers, bewaken serverbronnen, formaat wijzigen en beheren wie toegang heeft tot uw servers.  Als u problemen ondervindt, kunt u ook een verzoek om ondersteuning indienen.

![Naam van de server ophalen in Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Verbinding maken met uw server in Azure is net als de verbinding te maken met een exemplaar van de server in uw eigen organisatie. Van SSMS, kunt u veel van de taken zoals het verwerken van gegevens uitvoeren of maakt u een verwerkingsscript, rollen beheren en PowerShell gebruiken.

![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

 Een van de grotere verschillen is de verificatie die u kunt verbinding maken met uw server. Voor verbinding met uw Azure Analysis Services-server, moet u **Active Directory-wachtwoordverificatie**selecteren.

### <a name="to-connect-with-ssms"></a>Verbinding maken met SSMS
1. Voordat u verbinding maakt, moet u de naam van de server ophalen. In **Azure portal** > server > **Overzicht** > de**naam van de Server**, de naam van de server te kopiëren.

    ![Naam van de server ophalen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

2. In SSMS > **Object Explorer**, klikt u op **verbinden** > **Analysis Services**.

3. Plakken in naam van de server en klik vervolgens in bij de **verificatie**in het dialoogvenster **verbinding maken met Server** , kies een van de volgende:

    **Geïntegreerde verificatie van active Directory** met eenmalige aanmelding met Active Directory Azure Active Directory federation.

    **Verificatie van active Directory-wachtwoord** aan een organisatie-account gebruiken. Bijvoorbeeld wanneer een verbinding van een niet-domein lid van computer.

    Opmerking: Als er geen Active Directory-verificatie, moet u mogelijk [Azure Active Directory-verificatie inschakelen](#enable-azure-active-directory-authentication) in SSMS.

    ![Verbinding in SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

Omdat uw server in Azure beheren met behulp van SSMS grotendeels hetzelfde is als het beheer van een server op locatie, gaan we niet in de details hier. Alle hulp u nodig hebt vindt u in de [Analysis Services-instantie Management](https://msdn.microsoft.com/library/hh230806.aspx) op MSDN.

## <a name="server-administrators"></a>Server-beheerders
Kunt u **Analysis Services-beheerders** in het besturingselement blade voor uw server in Azure portal of SSMS voor het beheren van server-beheerders. Analysis Services-beheerders zijn database serverbeheerders met rechten voor de database beheertaken zoals het toevoegen en verwijderen van databases en gebruikers beheren. De gebruiker die de server in Azure portal maakt wordt standaard automatisch toegevoegd als een Analysis Services-beheerder.

U moet ook weten:

-   Windows Live ID is geen ondersteunde identiteitstype voor Azure Analysis Services.  
-   Analysis Services-beheerders moeten geldige Azure Active Directory-gebruikers.
-   Als een Azure Analysis Services-server via Azure Resource Manager sjablonen maakt, neemt de Analysis Services-beheerders een JSON-matrix van gebruikers die moeten worden toegevoegd als beheerders.

Analysis Services-beheerders kunnen afwijken van Azure resource beheerders die bronnen voor Azure abonnementen kunnen beheren. Dit houdt in database management-Services als u wilt scheiden van rechten tussen Azure bronbeheer en -analyse en compatibiliteit met bestaande XMLA- en TSML gedrag in Analysis Services beheren.

Alle rollen weergeven en toegang tot typen voor de resource Azure Analysis Services, Access control (IAM) op het blad van het besturingselement te gebruiken.

## <a name="database-users"></a>Databasegebruikers
Azure Analysis Services model gebruikers moeten zich in uw Azure Active Directory. Gebruikersnamen opgegeven voor de modeldatabase moet door organisatie-e-mailadres of de UPN. Dit is anders dan voor bedrijfsruimten model databases die ondersteuning bieden voor gebruikers met een Windows-domein-gebruikersnamen.

U kunt gebruikers toevoegen met behulp van [roltoewijzingen in Azure Active Directory](../active-directory/role-based-access-control-configure.md) of via [Scripting taal in tabelvorm Model](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) in SQL Server Management Studio.

**Voorbeeldscript voor TMSL**

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Users"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@contoso.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="enable-azure-active-directory-authentication"></a>Azure Active Directory-verificatie inschakelen
Met de functie Azure Active Directory-verificatie inschakelen voor SSMS in het register, maakt u een tekstbestand met de naam EnableAAD.reg, en vervolgens kopiëren en plakken van het volgende:


```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\Microsoft Analysis Services\Settings]
"AS AAD Enabled"="True"
```

Opslaan en voer vervolgens het bestand.



## <a name="next-steps"></a>Volgende stappen
Als u nog niet al een model in tabelvorm naar uw nieuwe server geïmplementeerd, is een goed moment. Zie voor meer informatie kunt [distribueren naar Azure Analysis Services](analysis-services-deploy.md).

Als u een model hebt geïmplementeerd op de server, kunt u zich via een client of de browser verbinding. Voor meer informatie, Zie [Azure Analysis Services-server gegevens ophaalt](analysis-services-connect.md).
