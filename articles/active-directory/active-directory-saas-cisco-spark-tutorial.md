<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Cisco vonk | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cisco vonk."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Zelfstudie: Azure Active Directory-integratie met Cisco Spark

In deze zelfstudie leert u Cisco Spark integreren met Azure Active Directory (AD Azure).

Cisco Spark integreren met AD Azure biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot de Cisco Spark heeft
- U kunt gebruikers automatisch ophalen ondertekend bij Cisco Spark (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw rekeningen op één centrale locatie - de klassieke Azure portal beheren

Als u weten meer informatie over SaaS app integratie met AD Azure wilt, Zie [toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met Cisco-Spark, moet u de volgende items:

- Een abonnement op Azure AD
- Een **Cisco Spark** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Test de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen te volgen:

- U moet uw productieomgeving niet gebruiken tenzij dat noodzakelijk is.
- Als u een evaluatieversie AD Azure-omgeving niet hebt, kunt u een maand proef [hier](https://azure.microsoft.com/pricing/free-trial/)krijgen.


## <a name="scenario-description"></a>Beschrijving van het scenario
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat is beschreven in deze zelfstudie bestaat uit twee voornaamste bouwstenen:

1. Cisco Spark toevoegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco Spark toevoegen uit de galerie
De integratie van Cisco Spark in Azure AD configureren, moet u Cisco vonk uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Cisco Spark toevoegen uit de galerie, kunt u de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Toepassingen][2]

4. Klik op **toevoegen** onder aan de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toepassing uit de galerie toevoegen**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Cisco vonk**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_01.png)

7. In het resultatendeelvenster **Cisco Spark**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Cisco Spark op basis van een testgebruiker met de naam "Simon Britta".

Voor eenmalige aanmelding wilt werken, moet AD Azure weet wat de gebruiker tegenhanger in Cisco Spark Azure advertentie aan een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Cisco vonk worden vastgesteld.
Deze relatie koppeling wordt vastgesteld door de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in het Cisco Spark toewijzen. Als u wilt configureren en testen Azure AD single sign-on met Cisco Spark, moet u voor het voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat de gebruikers deze functie wilt gebruiken.
2. **[Gebruikers maken een Azure AD test](#creating-an-azure-ad-test-user)** - Azure AD single sign-on met Britta Simon testen.
4. **[Gebruikers maken een vonk Cisco testen](#creating-a-cisco-spark-test-user)** - hebben een tegenhanger van Britta Simon in Cisco vonk die is gekoppeld aan de weergave Azure AD van haar.
5. **[Gebruiker toewijzen de Azure AD test](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik van eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van Single Sign-On](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

Het doel van deze sectie is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren op in uw toepassing Cisco vonk.

Cisco Spark toepassing verwacht de SAML-assertions specifieke kenmerken bevatten. Configureer de volgende kenmerken voor deze toepassing. De waarden van deze kenmerken kunt u beheren via het tabblad **'Atrributes'** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld voor dit.

![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_03.png) 

**Azure AD eenmalige aanmelding configureren met Cisco-Spark, voert u de volgende stappen uit:**

1. In de Azure klassieke portal op de **Cisco Spark** application Integratiepagina in het menu aan de bovenkant, klikt u op **kenmerken**.
     
    ![Eenmalige aanmelding configureren][5]


2. Voer de volgende stappen uit in het dialoogvenster **SAML-token kenmerken** :

    een. Klik op **gebruikerskenmerk toevoegen** om te openen, het dialoogvenster **Gebruiker-Attribure toevoegen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_05.png)
    
    b. Typ in het tekstvak **Naam van kenmerk** **uid**.
    
    c. Selecteer in de lijst **Waarde van het kenmerk** **user.userprincipal**.
    
    d. Klik op **Voltooien**. Vervolgens **Wijzigingen toepassen** onder aan de pagina.

3. In het menu aan de bovenkant, klikt u op de **Werkbalk Snel starten**.

    ![Eenmalige aanmelding configureren][6]

4. Klik in de klassieke portal op de pagina **Cisco Spark** application integration **eenmalige aanmelding configureren** om het dialoogvenster **Configureren van eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][7] 

5. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Cisco Spark** **Azure AD Single Sign-On**selecteren en klik op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_06.png)

6. Voer de volgende stappen uit op de pagina van het dialoogvenster **Toepassingsinstellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_07.png)


    een. Typ in het tekstvak teken in de URL een URL met het volgende patroon: `https://web.ciscospark.com/#/signin`.

    b. Klik op **volgende**.


7. Klik op de pagina **configureren eenmalige aanmelding bij Cisco Spark** **metagegevens worden gedownload**, en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_09.png)

8. Aanmelden bij [Cisco Cloud samenwerking Management](https://admin.ciscospark.com/) met de volledige beheerrechten.
9. Selecteer **Instellingen** en klik op **wijzigen**in de sectie **verificatie** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)

10. Selecteer **een 3de partij identiteitsprovider integreren. (Geavanceerd)** en Ga naar het volgende scherm.
11. Klik op **Bestand met metagegevens downloaden** en sla het bestand op uw computer.
12. Op de pagina **Idp-metagegevens importeren** sleept en neerzetten het bestand Azure AD metagegevens op de pagina of gebruik de optie bestand browser te vinden en upload het metagegevensbestand Azure AD. Vervolgens selecteert u **certificaat is ondertekend door een certificeringsinstantie in de metagegevens (veiliger) vereisen** en klik op **volgende**. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

13. Selecteer **Test SSO-verbinding**en als een nieuw tabblad in de browser wordt geopend, worden geverifieerd met Azure AD aanmeldt.
14. Terug naar het tabblad **Beheer van Cisco Cloud samenwerking** browser. Als de test voltooid is, selecteert u **deze test is geslaagd. Inschakelen van de optie op** en klik op **volgende**.

7. Op de klassieke Azure AD-portal, selecteert u de bevestiging van enkele aanmelding-configuratie en klik op **volgende**.
    
    ![Azure AD Single Sign-On][10]

8. Klik op **Voltooien**op de pagina **bevestiging van één aanmelding** .  
    
    ![Azure AD Single Sign-On][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure AD test gebruikers maken
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **Azure klassieke portal**in het linkernavigatievenster op **Active Directory**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_09.png) 

2. Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Azure AD test gebruikers maken](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. Het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, klikt u op **Gebruiker toevoegen**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

5. Klik op de pagina **Vertel ons over de gebruiker dit** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Azure AD test gebruikers maken](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_05.png) 

    een. Selecteer Type gebruiker, nieuwe gebruiker in uw organisatie.

    b. Typ **BrittaSimon**in de naam van het **tekstvak**.

    c. Klik op **volgende**.

6.  Voer de volgende stappen uit op de pagina van het dialoogvenster **Gebruikersprofiel** :

    ![Azure AD test gebruikers maken](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Laatste naam** textbox, type, **Simon**.

    c. Typ in het tekstvak **Weergegeven naam** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster **maken**.

    ![Azure AD test gebruikers maken](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_07.png) 

8. Klik op de pagina **tijdelijke wachtwoord ophalen** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD test gebruikers maken](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**.

    b. Klik op **Voltooien**.   



### <a name="creating-a-cisco-spark-test-user"></a>Maken van een testgebruiker Cisco Spark

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Cisco Spark. In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Cisco Spark.

1. Ga naar het [Beheer van Cisco Cloud samenwerking](https://admin.ciscospark.com/) met uw volledige administrator-referenties.
2. Klik op **gebruikers** en klik vervolgens op **gebruikers beheren**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. In het venster **Gebruiker beheren** **handmatig toevoegen of wijzigen van gebruikers** selecteren en klik op **volgende**.
4. Selecteer **namen en e-mailadres**. Vul vervolgens de textbox als volg:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**

    b. Typ in het tekstvak **Achternaam** **Simon**

    c. Typ in het tekstvak **e-mailadres****britta.simon@contoso.com**

5. Klik op het plusteken om Britta Simon toevoegen. Klik vervolgens op **volgende**.
6. In het venster **Services toevoegen voor gebruikers** , klikt u op **Opslaan** en vervolgens op **Voltooien**.

### <a name="assigning-the-azure-ad-test-user"></a>Het testgebruiker Azure AD toewijzen

In deze sectie kunt inschakelen u Britta Simon voor de Azure eenmalige aanmelding toegang verlenen aan Cisco vonk.

![Gebruiker toewijzen][200] 

**Britta Simon toewijzen aan Cisco vonk, voert u de volgende stappen uit:**

1. Op de klassieke portal de toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Cisco vonk**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_14.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst alle gebruikers **Britta Simon**.

2. Klik op **toewijzen**op de werkbalk aan de onderkant.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD één aanmelding configuratie testen met behulp van het Access-venster.

Wanneer u op de tegel Cisco Spark in het Access-venster, u moet krijgen automatisch ondertekend voor toegang tot uw toepassing Cisco Spark.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-toepassingen met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_205.png
