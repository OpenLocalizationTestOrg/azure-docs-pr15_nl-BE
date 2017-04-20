<properties 
    pageTitle="Zelfstudie: Werkdag configureren voor inkomende synchronisatie | Microsoft Azure" 
    description="Meer informatie over het inkomende synchronisatie met Azure Active Directory gebruiken voor het inschakelen van eenmalige aanmelding, geautomatiseerde provisioning en meer!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="04/06/2016" 
    ms.author="jeedes" />

#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Zelfstudie: Werkdag configureren voor inkomende synchronisatie
>[AZURE.NOTE]Azure Active Directory (AD) Premium is beschikbaar voor klanten in China met behulp van het wereldwijde exemplaar van AD Azure.    
Azure AD Premium wordt momenteel niet ondersteund in het Microsoft Azure-service beheerd door 21Vianet in China.    

Het doel van deze zelfstudie is aangegeven, zodat u de stappen die u moet uitvoeren in een werkdag en Microsoft Azure AD mensen van werkdag importeren naar Microsoft Azure AD.    
 Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:  

-   Een geldig abonnement Azure  
-   Een huurder in een werkdag  

Het scenario dat is beschreven in deze zelfstudie bestaat uit de volgende elementen:  

1.  De integratie van toepassingen voor de werkdag inschakelen  
2.  Een integratie-systeemgebruiker maken  
3.  Een beveiligingsgroep maken  
4.  De integratiegebruiker systeem toe te wijzen aan de beveiligingsgroep  
5.  Groep Beveiligingsopties configureren  
6.  Wijzigingen in beveiligingsbeleid activeren  
7.  Configureren gebruiker importeren in Microsoft Azure AD  

##<a name="enabling-the-application-integration-for-workday"></a>De integratie van toepassingen voor de werkdag inschakelen

Het doel van deze sectie is aan de contour van het inschakelen van de integratie van toepassingen voor televergaderingen.    

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Als u wilt dat de integratie van toepassingen voor een werkdag, moet u de volgende stappen uitvoeren:

1.  Klik in de Portal Azure Management in het linkernavigatievenster op **Active Directory**.    

    ![Active Directory] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Active Directory")  

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.    

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.    

    ![Toepassingen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Toepassingen")  

4.  Om te openen in de **Galerie van toepassing**, klikt u op **Een App toevoegen**en klik op **toevoegen een toepassing voor mijn organisatie te gebruiken**.    

    ![Wat wilt u doen?] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "Wat wilt u doen?")  

5.  Typ in het **zoekvak**de **werkdag**.    

    ![Werkdag] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "Werkdag")  

6.  In het resultatendeelvenster **werkdag**selecteren en klik op **Voltooien** als de toepassing wilt toevoegen.    

    ![Werkdag] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "Werkdag")  

##<a name="creating-an-integration-system-user"></a>Een integratie-systeemgebruiker maken

1.  In de **Werkdag Workbench** **gebruiker** invoeren in het zoekvak en klik op de link, **Integratie-systeemgebruiker maken**.     

    ![gebruiker maken] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "gebruiker maken")  

2.  Voer de taak integratie-systeemgebruiker maken door een gebruikersnaam en wachtwoord opgeven voor een nieuwe gebruiker van de integratie-systeem.  Laat de vereist wachtwoord bij volgende aanmelding In optie uitgeschakeld, omdat deze gebruiker programmatisch aanmelden.    
    Laat de notulen van de time-out voor sessie met de standaardwaarde is 0, waardoor wordt voorkomen dat de sessies time-out voortijdig beëindigd.    

    ![Integratie-systeemgebruiker maken] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Integratie-systeemgebruiker maken")  

##<a name="creating-a-security-group"></a>Een beveiligingsgroep maken

Voor het scenario in deze zelfstudie worden beschreven, moet u een beveiligingsgroep onbeperkte integratie systeem maken en aan de gebruiker toewijzen.    

1.  Voer beveiligingsgroep maken in het zoekvak en klik op de koppeling, een beveiligingsgroep maken.     

    ![Groep CreateSecurity] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "Groep CreateSecurity")  

2.  De beveiligingsgroep maken taak voltooien.  Schakel integratie systeem groep, zonder beperkingen in de vervolgkeuzelijst Type van verpachte beveiligingsgroep voor het maken van een beveiligingsgroep waaraan leden expliciet worden toegevoegd.     

    ![Groep CreateSecurity] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "Groep CreateSecurity")  

##<a name="assigning-the-integration-system-user-to-the-security-group"></a>De integratiegebruiker systeem toe te wijzen aan de beveiligingsgroep

1.  Groep bewerken opgeven in het zoekvak en klik op de link, **Groep bewerken**.     

    ![Groep bewerken] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Groep bewerken")  

2.  Zoek en selecteer de nieuwe groep voor integratie met de naam    

    ![Groep bewerken] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Groep bewerken")  

3.  De integratie van nieuwe gebruiker toevoegen aan de nieuwe groep.       

    ![Systeem-beveiligingsgroep] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "Systeem-beveiligingsgroep")  

##<a name="configuring-security-group-options"></a>Groep Beveiligingsopties configureren

In deze stap verleent u aan de nieuwe groepsmachtigingen voor de bewerkingen ophalen en plaatsen op de objecten die zijn beveiligd door het volgende beveiligingsbeleid voor domein:  

-   Externe Account inrichten  
-   Werknemersgegevens: Werknemer openbare rapporten  
-   Werknemersgegevens: Alle posities  
-   Werknemersgegevens: Huidige personeel informatie  
-   Werknemersgegevens: Business titel op het werknemersprofiel  

&nbsp;  

1.  Beveiligingsbeleid voor domein invoeren in het zoekvak en klik op de link, beveiligingsbeleid voor domein voor functioneel gebied.     

    ![Beveiligingsbeleid voor domein] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Beveiligingsbeleid voor domein")  

2.  Systeem zoekt en selecteert u de module systeem.  Klik op de knop geëtiketteerd, OK.     

    ![Beveiligingsbeleid voor domein] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Beveiligingsbeleid voor domein")  

3.  In de lijst van beveiligingsbeleid voor het systeem functioneel gebied beveiligingsbeheer vouwen en selecteer het domeinbeveiligingsbeleid externe Account inrichten.     

    ![Beveiligingsbeleid voor domein] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Beveiligingsbeleid voor domein")  

4.  Klik op de knop Machtigingen bewerken en klik vervolgens op het scherm machtigingen bewerken de nieuwe beveiligingsgroep toevoegen aan de lijst met beveiligingsgroepen met machtigingen voor Get en Put integratie.     

    ![De machtiging bewerken] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "De machtiging bewerken")  

5.  Herhaal stap 1 hierboven, om terug te keren naar het scherm voor het selecteren van functionele gebieden, en deze keer zoeken naar personeel, selecteert u de module personeel en klik op de knop OK geëtiketteerd.    

    ![Beveiligingsbeleid voor domein] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Beveiligingsbeleid voor domein")  

6.  Vouw in de lijst met beveiligingsbeleid voor de module personeel, werknemersgegevens: personeel en herhaalt u stap 4 uit voor elk van deze resterende beveiligingsbeleid:    

    -   Werknemersgegevens: Werknemer openbare rapporten  
    -   Werknemersgegevens: Alle posities  
    -   Werknemersgegevens: Huidige personeel informatie  
    -   Werknemersgegevens: Business titel op het werknemersprofiel    

    ![Beveiligingsbeleid voor domein] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Beveiligingsbeleid voor domein")  

##<a name="activating-security-policy-changes"></a>Wijzigingen in beveiligingsbeleid activeren

1.  Voer in het zoekvak activeren en klik op de koppeling activeren in behandeling zijnde wijzigingen in beveiligingsbeleid.    

    ![Activeren] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Activeren")  

2.   De taak activeren in behandeling zijnde wijzigingen in beveiligingsbeleid eerst een opmerking invoeren voor controledoeleinden en vervolgens te klikken op de knop geëtiketteerd, OK.      

    ![In afwachting van beveiliging activeren] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "In afwachting van beveiliging activeren")  

3.  Voer de taak op het volgende scherm door te controleren, het selectievakje bevestigen en klik op de knop geëtiketteerd, OK geëtiketteerd.     

    ![In afwachting van beveiliging activeren] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "In afwachting van beveiliging activeren")  

##<a name="configuring-user-import-in-microsoft-azure-ad"></a>Configureren gebruiker importeren in Microsoft Azure AD

Het doel van deze sectie wordt aan de contour van het configureren van Microsoft Azure AD mensen van de werkdag te importeren.    

###<a name="to-configure-user-import-in-microsoft-azure-ad-perform-the-following-steps"></a>Gebruikers importeren in Microsoft Azure AD configureren, moet u de volgende stappen uitvoeren:

1.  Klik op de pagina **werkdag** application integration **importeren configureren gebruiker** opent het dialoogvenster **Configureren wordt ingericht** .    

2.  Voer de volgende stappen uit op de pagina **instellingen en referenties admin** en klik op volgende:    

    ![Instellingen en beheer referenties] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Instellingen en beheer referenties")    

    1.  Typ in het tekstvak **werkdag admin gebruikersnaam** de naam van de gebruiker die u hebt gemaakt in de sectie [een integratie-systeemgebruiker maken](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) .    
    2.  Typ in het tekstvak **werkdag admin-wachtwoord** het wachtwoord van de gebruiker die u hebt gemaakt in de sectie [een integratie-systeemgebruiker maken](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) .    
    3.  Typ de URL of de huurder werkdag in het tekstvak **URL van de huurder werkdag** .    

3.  Klik op de pagina **verbinding testen** Klik op **test starten** om te bevestigen de verbinding en klik op **volgende**.    

    ![Verbinding testen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "Verbinding testen")  

4.  Klik op **volgende**op de pagina **Opties voor creëren** .    

    ![Configuratieopties] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Configuratieopties")  

5.  Klik op **Voltooien**in het dialoogvenster **Start-aanbod** .    

    ![Inrichten van start] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Inrichten van start")  

U kunt nu gaat u naar de sectie **gebruikers** en controleer of de gebruiker van uw werkdag is geïmporteerd.    
