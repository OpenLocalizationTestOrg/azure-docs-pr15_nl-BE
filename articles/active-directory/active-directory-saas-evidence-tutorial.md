<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Evidence.com | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Evidence.com."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/23/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Zelfstudie: Azure Active Directory-integratie met Evidence.com

Het doel van deze zelfstudie is het instellen van eenmalige aanmelding tussen Azure Active Directory (AAD) en Evidence.com weergeven. Het scenario dat is beschreven in deze handleiding wordt ervan uitgegaan dat u al de volgende items:
    
* Een geldige licentie voor Microsoft Azure
* Een Evidence.com-abonnement met eenmalige aanmelding is ingeschakeld (e-mailadres earlyaccess@evidence.com als op basis van SAML eenmalige aanmelding niet is ingeschakeld)

Na het voltooien van deze zelfstudie, de AAD gebruikers aan wie u toegang Evidence.com hebt toegewezen kan worden aan één teken in de toepassing met het deelvenster AAD toegang.

## <a name="add-evidencecom-to-your-directory"></a>Evidence.com toevoegen aan de directory

In deze sectie worden Evidence.com als een geïntegreerde toepassing in Azure Active Directory toevoegen.

**De integratie van toepassingen voor bewijsmateriaal inschakelen:**

1.  Klik in de [Azure klassieke portal](https://manage.windowsazure.com)in het linkernavigatievenster op **Active Directory**.

2.  Selecteer in de lijst **map** de map die u wilt inschakelen van directory-integratie.

3.  De toepassingen in de weergave in de mapweergave, klikt u op **toepassingen** in het bovenste menu.

4.  U opent de galerie toepassing, klikt u op **toevoegen**en klik vervolgens op **toepassing uit de galerie toevoegen**.

5.  Typ **Evidence.com**in het zoekvak.

6.  **Evidence.com**selecteren in het deelvenster met resultaten en klik op **Voltooien** als de toepassing wilt toevoegen.


## <a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Deze sectie behandelt het inschakelen van gebruikers worden geverifieerd met hun account in Azure Active Directory, op basis van het protocol SAML federation met Evidence.com.

**Als u eenmalige aanmelding configureert, moet u de volgende stappen uitvoeren:**

1.  Na de Evidence.com in de klassieke Azure portal toe te voegen, klikt u op **Configureren van eenmalige aanmelding**. 
 
2.  In het volgende scherm Selecteer **Azure AD Single Sign-On**en klik vervolgens op **volgende**.

3.  In het scherm configureren App URL de URL opgeven waar gebruikers in uw URL Evidence.com huurder zal ondertekenen (voorbeeld: https://yourtenant.evidence.com en klik op **volgende**. 

4.  Klik op de koppeling **Certificaat downloaden** en opslaan op uw lokale schijf. Dit certificaat en de metagegevens van URL's (Afdelings-ID, SSO-teken In de URL en Sign Out URL) wordt gebruikt voor het instellen van eenmalige aanmelding op de website Evidence.com. 

5.  In een afzonderlijk webbrowservenster, meld u aan bij uw Evidence.com pachters als administrator en Ga naar tabblad **beheer**
      
6.  Klik op **Bureau-eenmalige aanmelding**
 
7.  Selecteer **SAML eenmalige op basis van**
 
8.  Kopieer de **URL van de uitgevende instelling**, **Single Sign On** en **Single Sign Out** waarden die worden weergegeven in de klassieke Azure portal en naar de bijbehorende velden in de Evidence.com.

9.  Open het certificaat gedownload in stap 4 een teksteditor zoals Notepad.exe, en kopieer en plak de inhoud in het **Certificaat van de beveiliging** . 

10. De configuratie opslaan in Evidence.com.
 
11. Controleer in het portal voor Azure klassieke **bevestigen dat u eenmalige aanmelding zoals hierboven beschreven hebt geconfigureerd**. Deze controle kan het huidige certificaat te werken voor dit selectievakje toepassing starten.
 
12. Klik op **Voltooien**op de bevestigingspagina van één aanmelding.  


## <a name="creating-an-evidencecom-test-user"></a>Het testgebruiker van een Evidence.com maken

Azure AD gebruikers kunnen inloggen, als ze worden ingericht om toegang te krijgen in de toepassing Evidence.com. In deze sectie wordt beschreven hoe u gebruikersaccounts Azure AD in Evidence.com

**Inrichten van een gebruikersaccount in Evidence.com:**

1.  Log in op uw site Evidence.com bedrijf als beheerder in een webbrowservenster.

2.  Ga naar het tabblad **beheer** .

3.  Klik op **gebruiker toevoegen**.

4.  Klik op de knop **toevoegen** .

5.  Het **E-mailadres** van de gebruiker toegevoegd moet overeenkomen met de gebruikersnaam van de gebruikers in Azure advertentie die u wenst te krijgen. Als de gebruikersnaam en e-mailadres niet dezelfde waarde in uw organisatie, kunt u de **Evidence.com > kenmerken > Single Sign-On** gedeelte van de Azure klassieke portal te wijzigen van de nameidenitifer naar Evidence.com naar het e-mailadres worden verzonden.


## <a name="assigning-users-to-evidencecom"></a>Gebruikers toewijzen aan Evidence.com

Ingerichte AAD gebruikers Evidence.com op hun toegang tot Configuratiescherm zien, als ze toegang binnen de klassieke Azure portal worden toegewezen.

**Gebruikers toewijzen aan Evidence.com:**

1.  Klik op de startpagina quick voor Evidence.com in de klassieke Azure portal **gebruikers toewijzen aan Evidence.com**.
 
2.  Selecteer in het menu **weergeven** , of u wilt dat een gebruiker of groep toewijzen aan Evidence.com, en klik op de knop ingeschakeld.
 
3.  Selecteer de gebruikers aan de groep aan wie u wilt toewijzen, Evidence.com in de lijst **gebruikers** .
 
4.  In de voettekst van de pagina, klikt u op de knop **toewijzen** .

