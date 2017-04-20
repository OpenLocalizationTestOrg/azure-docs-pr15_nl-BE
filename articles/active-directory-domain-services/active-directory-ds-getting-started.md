<properties
    pageTitle="Azure AD Domain Services: De groep Administrators van AAD DC maken | Microsoft Azure"
    description="Aan de slag met Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="get-started-with-azure-ad-domain-services"></a>Aan de slag met Azure AD Domain Services

Dit artikel helpt bij de configuratietaken in te schakelen Azure AD Domain Services voor uw huurder Azure AD.

## <a name="task-1-create-the-aad-dc-administrators-group"></a>Taak 1: De groep 'Beheerders AAD DC' maken
De eerste taak is het maken van een beheergroep in uw huurder Azure Active Directory. Deze speciale administratieve groep heet **AAD DC beheerders**. Leden van deze groep krijgen beheerdersbevoegdheden op computers die het AD-Domain Azure Services beheerde domein domein behoren. Op computers met een domein behoren, wordt deze groep toegevoegd aan de beheerdersgroep. Leden van deze groep kunt ook extern bureaublad extern verbinding maken met computers die deel uitmaakt van een domein.  

> [AZURE.NOTE] Er geen domeinbeheerder of Ondernemingsadministrator-bevoegdheden op de beheerde domein gemaakt met Azure AD Domain Services. Op beheerde domeinen, deze bevoegdheden door de service worden gereserveerd en zijn niet beschikbaar gesteld aan gebruikers van de huurder. Echter, kunt u de van de speciale beheerdersgroep gemaakt in deze configuratietaak sommige beschermde bewerkingen uit te voeren. Deze bewerkingen zijn onder meer computers toevoegen aan het domein, die deel uitmaken van de groep Administrators op een domein behoren computers configureren van beleid, enzovoort.

In deze configuratietaak maakt de beheergroep en een of meer gebruikers in de directory toevoegen aan de groep. Voer de volgende stappen voor het maken van de beheergroep voor Azure AD Domain Services:

1. Ga naar de **Azure klassieke portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com))

2. Selecteer het knooppunt **Active Directory** in het linkerdeelvenster.

3. Selecteer de huurder Azure AD (map) dat u wilt inschakelen, Azure AD Domain Services. U kunt slechts één domein voor elke map Azure AD maken.

    ![Azure AD map selecteren](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klik op het tabblad **groepen** .

5. Een groep toevoegen aan uw huurder Azure AD, klikt u op **Groep toevoegen** in het taakvenster onder aan de pagina.

    ![De knop groep toevoegen](./media/active-directory-domain-services-getting-started/add-group-button.png)

6. Maak een groep met de naam **AAD DC beheerders**. **Het TYPE** **beveiliging**instellen.

    > [AZURE.WARNING] Voor toegang binnen uw Azure AD Domain Services beheerd domein, maakt u een groep met deze exacte naam.

    ![Maken van de groep Administrators](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. Een beschrijving voor deze groep toevoegen, zodat anderen begrijpen dat deze groep wordt gebruikt in Azure AD Domain Services Administrator-bevoegdheden toekennen.

8. Nadat u de groep hebt gemaakt, klikt u op de naam van de groep om de eigenschappen van deze groep. Als gebruikers wilt toevoegen als lid van deze groep, klikt u op de knop **leden toevoegen** in het onderste deelvenster.

    ![Groep leden knop toevoegen](./media/active-directory-domain-services-getting-started/add-group-members-button.png)

9. Selecteer in het dialoogvenster **leden toevoegen** , gebruikers moeten lid zijn van deze groep en schakel het selectievakje in als u klaar bent.

    ![Gebruikers toevoegen aan de groep Administrators](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## <a name="task-2-create-or-select-an-azure-virtual-network"></a>Taak 2: Maak of Selecteer een Azure virtueel netwerk
De volgende configuratietaak is [gemaakt](active-directory-ds-getting-started-vnet.md)of Selecteer een Azure virtueel netwerk.
