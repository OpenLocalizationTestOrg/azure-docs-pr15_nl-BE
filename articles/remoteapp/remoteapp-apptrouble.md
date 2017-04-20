<properties 
    pageTitle="Azure RemoteApp-probleemoplossing - toepassingsfouten starten en verbinding | Microsoft Azure" 
    description="Informatie over het oplossen van problemen met starten en verbinding te maken met toepassingen in Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



#<a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Problemen oplossen met Azure RemoteApp - toepassingsfouten starten en verbinding 

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Gehost in Azure RemoteApp-toepassingen mislukken te starten voor een aantal verschillende redenen. Dit artikel beschrijft verschillende oorzaken en foutberichten kunnen gebruikers ontvangen bij het starten van toepassingen. Deze gesprekken voert ook over de mislukte verbinding. (Maar in dit artikel worden problemen beschreven niet bij het ondertekenen van de Azure RemoteApp-client).  

Lees verder voor meer informatie over algemene foutberichten als gevolg van storingen in app starten en verbinding.

##<a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Onze beste instellen... Probeer het opnieuw in 10 minuten.

Deze fout betekent dat Azure RemoteApp is verticaal schalen om te voldoen aan de capaciteitsbehoefte van uw gebruikers. Op de achtergrond worden meer Azure RemoteApp-VM exemplaren gemaakt voor het verwerken van de capaciteitsbehoeften van uw gebruikers. Doorgaans dit duurt ongeveer vijf minuten, maar kan maximaal 10 minuten duren. Soms, dit gebeurt niet snel genoeg en bronnen direct nodig zijn. Bijvoorbeeld een 9 AM scenario waar veel gebruikers moeten uw app in Azure RemoteAppn tegelijk gebruiken. Als dit het geval u dat we **capaciteit-modus** op de back-end kunt inschakelen. Om dit te doen een Azure Support ticket openen en of een e-mail sturen naar [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com). Er zeker van uw abonnements-ID in de aanvraag opnemen.  

![We krijgen u instellen](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a>Kan niet auto-reconnect voor uw toepassingen, opnieuw start de toepassing  

Dit foutbericht wordt vaak gezien als gebruikte Azure RemoteApp en zet uw PC in de slaapstand meer dan 4 uur en vervolgens de PC woke up en de Azure RemoteApp-client probeert automatisch opnieuw verbinding maakt en de time-out is overschreden.  Geef aan gebruikers terug naar de toepassing en probeert te openen vanuit de Azure RemoteApp-client.

![Kan niet auto-reconnect voor uw toepassingen](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a>Problemen met het tijdelijke profiel 

Deze fout treedt op als uw gebruikersprofiel (schijf van de gebruiker profiel) kan niet worden gekoppeld en de gebruiker een tijdelijk profiel ontvangen.  Beheerders moeten navigeren naar de collectie in de portal Azure gaat u naar het tabblad **sessies** en proberen te **Afmelden** van de gebruiker. Dit zal ervoor zorgen dat een volledig logboek van de sessie van de gebruiker - en vervolgens een gebruiker probeert een app opnieuw starten. Als dat niet lukt contact op met ondersteuning van Azure en of een e-mail sturen naar [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

## <a name="azure-remoteapp-has-stopped-working"></a>Azure RemoteApp werkt niet

Dit foutbericht geeft de Azure RemoteApp-client heeft een probleem en moet opnieuw worden gestart. Geef aan gebruikers te sluiten: Selecteer het **programma sluit** en vervolgens opnieuw starten de Azure RemoteApp-client.  Als het probleem blijft open en Azure Support ticket en of een e-mail sturen naar [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

![Azure RemoteApp werkt niet](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a>Er is een fout opgetreden tijdens de verbinding met extern bureaublad is toegang tot deze bron. De verbinding opnieuw of neem contact op met uw systeembeheerder.

Dit is een algemeen foutbericht - Neem contact op met ondersteuning van Azure en of een e-mail sturen naar [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com) zodat we kunnen onderzoeken. 

![Algemeen Azure RemoteApp-bericht](./media/remoteapp-apptrouble/ra-apptrouble4.png) 