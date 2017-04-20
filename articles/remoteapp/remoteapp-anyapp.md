<properties
   pageTitle="Elke Windows-toepassing uitgevoerd op elk apparaat met Azure RemoteApp | Microsoft Azure"
   description="Informatie over het delen van elke Windows-toepassing met uw gebruikers via RemoteApp Azure."
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Elke Windows-toepassing uitgevoerd op elk apparaat met Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

U kunt uitvoeren een Windows-toepassing overal op elk apparaat, nu, ernstig - NET via RemoteApp Azure. Of een aangepaste toepassing die 10 jaar geleden geschreven of een Office-toepassing, moeten de gebruikers niet meer worden gekoppeld aan een bepaald besturingssysteem (zoals Windows XP) voor die enkele toepassingen.

Met RemoteApp-Azure, uw gebruikers ook hun eigen Android of Apple-apparaten en de dezelfde ervaring die ze hebben gekregen in Windows (of op de Windows-telefoons). Dit gebeurt door het hosten van uw Windows-toepassing in een verzameling Windows virtuele machines op Azure - gebruikers toegang tot deze overal zij verbinding hebt met internet. 

Lees verder voor een voorbeeld van hoe u dit doet.

In dit artikel gaan we toegang delen met alle gebruikers. U kunt echter elke toepassing. Als u uw app op een computer met Windows Server 2012 R2 installeren kunt, kunt u delen met behulp van de volgende stappen uit. U kunt de [app voorschriften](remoteapp-appreqs.md) om ervoor te zorgen dat uw app werkt bekijken.

Houd er rekening mee dat omdat Access een database is en we die database willen nuttig, we een paar extra stappen doen gaan waarmee gebruikerstoegang de share toegang tot gegevens. Als uw app geen database of hoeft u niet uw gebruikers kunnen toegang krijgen tot gedeelde bestanden, kunt u de stappen in deze zelfstudie overslaan

> [AZURE.NOTE] <a name="note"></a>Een Azure-account voor het voltooien van deze zelfstudie hebt u nodig:
> - U kunt [gratis een Azure account openen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F): krijgt u credits kunt u uitproberen betaalde Azure services en zelfs nadat ze gebruikt omhoog kunt u de account en gebruik gratis Azure services, zoals Websites. Uw creditcard wordt nooit afgeschreven, tenzij u expliciet de instellingen wijzigen en ten laste van vragen.
> - Kunt u de [voordelen van MSDN subscriber activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): uw MSDN-abonnement biedt u credits elke maand die u voor betaalde Azure services gebruiken kunt.


## <a name="create-a-collection-in-remoteapp"></a>Een verzameling maken in RemoteApp

Begin met het maken van een collectie. De collectie fungeert als een container voor uw toepassingen en gebruikers. Elke collectie is gebaseerd op een afbeelding - u kunt uw eigen maken of gebruik een voorzien zijn van uw abonnement. We maken gebruik van de Office 2013 proefversie afbeelding voor deze zelfstudie - bevat de app die u wilt delen.

1. In het portal voor Azure schuiven in de structuur van de nav links totdat u RemoteApp. Open de pagina.
2. Klik op **maken van een RemoteApp-collectie**.
3. Klik op **snel maken** en typ een naam voor uw verzameling.
4. Selecteer het gebied dat u gebruiken wilt voor het maken van uw verzameling. Voor de beste ervaring, de regio selecteren die zich het dichtst bij de locatie waar uw gebruikers toegang de app tot geografisch. Bijvoorbeeld: in deze zelfstudie, de gebruikers, bevindt zich in Redmond, Washington. De dichtstbijzijnde Azure regio is **West VS**.
5. Selecteer het factuuradres plan dat u wilt gebruiken. Het factuuradres basisniveau geplaatst 16 gebruikers op een grote Azure VM, terwijl de standaard facturering plan 10 gebruikers op een grote Azure VM heeft. Een algemeen voorbeeld het basisniveau is een uitstekend voor data entry type workflow. Voor een app productiviteit, zoals Office, wilt u de standaard plan.
6. Ten slotte selecteert u de afbeelding Office 2013 Professional. Deze afbeelding bevat Office 2013 apps. Herinnering - deze afbeelding is alleen geschikt voor de evaluatieversie collecties en POCs. U ' deze afbeelding niet gebruiken in een productie-collectie.
7. Klik nu op **maken RemoteApp-collectie**.

![Een collectie cloud maken in RemoteApp](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Hiermee start u uw verzameling te maken, maar het kan tot een uur in beslag.

U nu kunt uw gebruikers toe te voegen.

## <a name="share-the-app-with-users"></a>De app delen met gebruikers

Zodra uw verzameling is gemaakt, is het tijd om de toegang tot uitgeven aan gebruikers en de gebruikers die toegang tot deze hebben moeten.

Als u niet op het knooppunt Azure RemoteApp bent gegaan tijdens het maken van de collectie, start de weg terug naar Azure introductiepagina maken.

2. Klik op de verzameling die u eerder toegang tot aanvullende opties en configureren van de collectie gemaakt.
![Een nieuwe collectie van RemoteApp-wolk](./media/remoteapp-anyapp/ra-anyappcollection.png)
3. Op het tabblad **publiceren** , klikt u op **publiceren** op de onderkant van het scherm en klik vervolgens op **publiceren Start menu programma's**.
![Publiceren van een RemoteApp-programma](./media/remoteapp-anyapp/ra-anyapppublish.png)
4. Selecteer de apps die u wilt publiceren uit de lijst. Wij hebben gekozen voor toegang voor ons doel. Klik op **Voltooien**. Wacht totdat de apps te publiceren te voltooien.
![Publishing toegang in RemoteApp](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)


1. Zodra de app publishing, hoofd is voltooid via op het tabblad **Toegang** voor alle gebruikers die toegang tot uw toepassingen toevoegen. Voer gebruikersnamen (e-mailadres) voor uw gebruikers en klik vervolgens op **Opslaan**.

![Gebruikers toevoegen aan een RemoteApp](./media/remoteapp-anyapp/ra-anyappaddusers.png)


1. Nu is het tijd om uw gebruikers te vertellen over deze nieuwe apps en hoe ze. U doet dit door uw gebruikers een e-mail sturen aan te wijzen ze de URL van extern bureaublad-client downloaden.
![De client de URL voor RemoteApp downloaden](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-to-access"></a>Toegang tot Access configureren

Sommige apps hebt extra configuratie nodig nadat u ze hebt geïmplementeerd via RemoteApp. In het bijzonder om toegang te krijgen gaan we naar een bestandsshare op Azure die voor alle gebruikers toegankelijk maken. (Als u niet wilt doen, kunt u een [collectie van hybride](remoteapp-create-hybrid-deployment.md) [in plaats van onze collectie wolk] waarmee uw gebruikers toegang hebben tot bestanden en gegevens op uw lokale netwerk.) Vervolgens moet u vertellen onze gebruikers een lokaal station op hun computer toewijzen aan de Azure bestandssysteem.

Het eerste deel, u als de beheerder doet. Vervolgens hebben we een aantal stappen voor uw gebruikers.

1. Begin met het publiceren van de command line interface (cmd.exe). In het tabblad **publiceren** , selecteert u **cmd**en klik vervolgens op **publiceren > publiceren programma pad**.
2. Voer de naam van de app en het pad. Voor ons doel, 'File Explorer"als"% SYSTEMDRIVE%\windows\explorer.exe"en de naam als het pad te gebruiken.
![Publiceer het bestand cmd.exe.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. U moet nu een Azure [opslag account](../storage/storage-create-storage-account.md)maken. We onze 'accessstorage', met de naam dus kies een naam die voor u betekenis heeft. (Als u wilt misquote Highlander, kan er slechts één "accessstorage.") ![Onze Azure opslag account](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Ga nu terug naar uw dashboard zodat u kunt het pad naar uw opslag (eindpunt locatie). U kunt dit gebruiken in een bit, dus zorg ervoor dat u ergens kopiëren.
![Het pad opslag account](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Vervolgens moet u de primaire toegangstoets zodra de opslag-account is gemaakt. **Toegangstoetsen beheren**op en kopieer de primaire toegangstoets.
6. Nu de context van de opslag-account instellen en een nieuwe bestandsshare maken om toegang te krijgen. Voer de volgende cmdlets uit in een verhoogde Windows PowerShell-venster:

        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx

    Dus voor ons aandeel zijn de cmdlets die we uitvoeren:

        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx


Nu, is het inschakelen van de gebruiker. Ten eerste hebben uw gebruikers een [RemoteApp-client](remoteapp-clients.md)installeren. Vervolgens moeten de gebruikers toewijzen van een station van de account die Azure bestandsshare die u hebt gemaakt en hun toegang tot bestanden toevoegen. Dit is hoe zij dat doen:

1. In de RemoteApp-client toegang krijgen tot de gepubliceerde apps. Start het programma cmd.exe.
2. Voer de volgende opdracht om een station van de computer naar de bestandsshare toewijzen:

        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>

    Als u de parameter **/ persistent** op Ja instelt, wordt het toegewezen station blijven bewaard tussen sessies.
1. De toepassing File Explorer van RemoteApp-nu starten. Kopieer alle Access-bestanden die u wilt gebruiken in de gedeelde app naar de bestandsshare.
![Access-bestanden opslaan in een Azure-share](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
1. Ten slotte, open Access en open de database die u zojuist hebt gedeeld. Hier ziet u de gegevens in Access uitgevoerd vanuit de cloud.
![Een bestaande Access-database uitgevoerd vanuit de cloud](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Nu kunt u toegang op elk apparaat - Zorg ervoor dat u een RemoteApp-client installeert.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe het maken van een collectie, proberen een [collectie die gebruikmaakt van Office 365](remoteapp-tutorial-o365anywhere.md). Of u een [hybride-collectie ](remoteapp-create-hybrid-deployment.md)die de toegang uw lokale netwerk tot kunt maken.

<!--Image references-->
 
