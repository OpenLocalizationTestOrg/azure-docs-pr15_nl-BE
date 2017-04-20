<properties
    pageTitle="Een nieuwe account voor Azure Stack huurder in Azure Active Directory toevoegen | Microsoft Azure"
    description="Na de implementatie van Microsoft Azure Stack Implementatiemodel, moet u ten minste één huurder gebruikersaccount maken vindt u op de portal van de huurder."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Een nieuwe account voor Azure Stack huurder in Azure Active Directory toevoegen

Na de [implementatie van de Stack Azure Implementatiemodel](azure-stack-run-powershell-script.md)moet u een gebruikersaccount huurder zodat u kunt de huurder portal verkennen en testen van uw aanbiedingen en plannen. Kunt u een huurder rekening [met de Azure portal](#create-an-azure-stack-tenant-account-using-the-azure-portal) of via [PowerShell](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Een stapel Azure huurder account maken met de Azure portal

U hebt een abonnement Azure de Azure portal gebruiken.

1. Log in op [Azure](http://manage.windowsazure.com).

2.  Klik op **Active Directory**in Microsoft Azure linkernavigatiebalk.

3.  In de maplijst klikt u op de map die u wilt gebruiken voor Azure Stack of een nieuwe maken.

4.  Klik op de pagina van deze map **gebruikers**.

5.  Klik op **gebruiker toevoegen**.

6.  Kies in de wizard **gebruiker toevoegen** in de lijst **Type gebruiker** **nieuwe gebruiker in uw organisatie**.

7.  Typ in het vak **naam** een naam voor de gebruiker.

8.  In de **@** Kies de juiste post.

9.  Klik op de pijl volgende gemarkeerd.

10.  Typ een **Voornaam**, **Achternaam**en **weergavenaam**in het **gebruikersprofiel** van de pagina van de wizard.

11. Kies **gebruiker**in de lijst **functie** .

12. Klik op de pijl volgende gemarkeerd.

13. Klik op **maken**op de pagina **tijdelijke wachtwoord opvragen** .

14. Kopieer het **nieuwe wachtwoord**.

15. Aanmelden bij Microsoft Azure met de nieuwe account. Wijzig het wachtwoord wanneer daarom wordt gevraagd.

16. Log in op `https://portal.azurestack.local` met de nieuwe account voor een overzicht van de huurder-portal.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Een stapel Azure huurder account maken met PowerShell

Als u niet een Azure-abonnement hebt, kunt u de portal Azure een huurder gebruikersaccount toevoegen niet gebruiken. In dit geval kunt u in plaats daarvan de Azure Active Directory-Module voor Windows PowerShell.

> [AZURE.NOTE] Als u Microsoft-Account (Live ID) naar Azure Stack Haalbaarheidstest implementeren, kunt u PowerShell AAD huurder account maken niet gebruiken. 

1.  Installeer de [Microsoft Online Services Sign-In Assistant voor IT-Professionals RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).

2.  De [Azure Active Directory Module voor Windows PowerShell (64-bits versie)](http://go.microsoft.com/fwlink/p/?linkid=236297) te installeren en te openen.

3.  Voer de volgende cmdlets uit:




    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack PoC
   
            $msolcred = get-credential
    
    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".
    
            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId
    
    ```

4.  Aanmelden bij Microsoft Azure met de nieuwe account. Wijzig het wachtwoord wanneer daarom wordt gevraagd.

5.  Meld u aan bij `https://portal.azurestack.local` met de nieuwe account voor een overzicht van de huurder-portal.



