<properties
    pageTitle="Azure meerledige verificatie rapporten"
    description="Dit wordt beschreven hoe u de functie Azure meerledige verificatie - rapporten."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="reports-in-azure-multi-factor-authentication"></a>Rapporten in Azure meerledige verificatie

Azure meerledige verificatie biedt verschillende rapporten die door u en uw organisatie kunnen worden gebruikt. Deze rapporten zijn toegankelijk via het meerledige verificatie Management Portal, die vereist dat een Provider van de MVR gesloten Azure, of een Azure MVR gesloten, Azure AD Premium of Enterprise Mobility Suite-licentie. Hier volgt een lijst met beschikbare rapporten.

Toegang tot rapporten via de portal Management Azure.

Naam| Beschrijving
:------------- | :------------- |
Gebruik | De gebruiksrapporten informatie weergeven op het totale verbruik, gebruiker overzicht en Gebruikerdetails.
Status van de server|Deze lijst bevat de status van een meerledige verificatie-Servers die zijn gekoppeld aan uw account.
Geblokkeerde gebruikersgeschiedenis|Deze rapporten tonen de geschiedenis van verzoeken te blokkeren of deblokkeren van gebruikers.
Overgeslagen gebruikersgeschiedenis|De geschiedenis van de aanvragen tot meerledige verificatie overslaan voor het telefoonnummer van de gebruiker bevat.
Fraudewaarschuwing|Bevat een geschiedenis van Fraudewaarschuwingen ingediend tijdens het datumbereik dat u hebt opgegeven.
In de wachtrij geplaatst|Lijsten, rapporten in de wachtrij voor verwerking en hun status. Een koppeling naar het downloaden of bekijken van het rapport is beschikbaar wanneer het rapport voltooid is.

## <a name="to-view-reports"></a>Rapporten weergeven

1.  Meld u aan bij http://azure.microsoft.com
2.  Selecteer Active Directory aan de linkerkant.
3.  Selecteer een van de volgende opties:
    - **Optie 1**: klik op het tabblad meerledige verificatie Providers. Selecteer uw provider MVR gesloten en klik op de knop beheren onder.
    - **Optie 2**: Selecteer de map en klik op het tabblad configureren. Selecteer in de sectie meerledige verificatie service-instellingen beheren. Klik op Ga naar de portal koppeling onderaan de pagina instellingen van de MVR gesloten.
4.  In Azure meerledige verificatie Management Portal ziet u de weergave een sectie van een rapport in de linkernavigatiebalk. Hier kunt u de hierboven beschreven rapporten.

<center>![Wolk](./media/multi-factor-authentication-manage-reports/report.png)</center>


**Aanvullende bronnen**

* [Voor gebruikers](./end-user/multi-factor-authentication-end-user.md)
* [Azure meerledige verificatie op MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)
