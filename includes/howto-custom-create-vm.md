#<a name="how-to-create-a-custom-virtual-machine"></a>Het maken van een aangepaste virtuele Machine

Een *aangepaste* virtuele machine verwijst naar een virtuele machine die u met behulp van de methode **Van galerie maakt** omdat kunt u meer opties dan de **Snelle invoer** methode configureren. Deze opties zijn beschikbaar:

- Meer opties voor de afbeelding moet worden gebruikt voor het maken van de virtuele machine (VM)
- De VM verbinding met een virtueel netwerk
- De VM toe te voegen aan een bestaande cloud-service
- De VM toe te voegen aan een set beschikbaarheid

> [AZURE.IMPORTANT] Als u wilt dat de virtuele machine te gebruiken van een virtueel netwerk zodat kunt u deze rechtstreeks door de hostnaam of het instellen van verbindingen tussen ruimten, Controleer of u het virtuele netwerk wanneer u de virtuele machine maakt. Een virtuele machine kan worden geconfigureerd als u wilt deelnemen aan een virtueel netwerk alleen wanneer u de virtuele machine maakt. Zie voor meer informatie over virtuele netwerken [Azure Virtual Network overzicht](http://go.microsoft.com/fwlink/p/?LinkID=294063).

1. Log in om de [Azure portal](http://manage.windowsazure.com).

2. Klik op **Nieuw**op de opdrachtbalk.

3. Klik op **berekenen**, klikt u op de **virtuele Machine**en klik op **Uit galerie**.

4. Kies de afbeelding die u wilt gebruiken en klik vervolgens op de pijl om door te gaan.

5. Als u meerdere versies van de afbeelding, in **Versie releasedatum**beschikbaar zijn, kiest u de versie die u wilt gebruiken.

6. Typ de naam die u wilt gebruiken voor de virtuele machine in **Virtual machinenaam**.

7. Met **lagen** en de **grootte** kunt u de juiste grootte voor de virtuele machine. De grootte die u selecteert is van invloed op de maximale configuratie van de virtuele machine, alsmede de prijzen. Configuratie Zie [virtuele Machine en Cloud Service formaten voor Azure](http://go.microsoft.com/fwlink/p/?LinkID=389844).

8. Typ een naam voor de Administrator-account die u gebruiken wilt voor het beheren van de server in het vak **Gebruikersnaam**.

9. Typ in het vak **Nieuw wachtwoord**een sterk wachtwoord voor de Administrator-account. Typ het wachtwoord in het vak **Wachtwoord bevestigen**.

10. Klik op de pijl om door te gaan.

11. In de **Cloud-Service**, doe het volgende:

    - Als dit de eerste of de enige virtuele machine in de cloud-service, selecteert u **een nieuwe wolk Service maken**. In **De naam van de DNS-Service Cloud**, typ een naam die wordt gebruikt tussen 3 en 24 kleine letters en cijfers. Deze naam wordt onderdeel van de URI die contact opnemen met de virtuele machine via de cloud-service wordt gebruikt.
    - Als deze virtuele machine wordt toegevoegd aan een cloud service, in de lijst selecteren.

    > [AZURE.NOTE] Zie [verbinding maken met virtuele machines in een cloud-service](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/)voor meer informatie over de virtuele machines in dezelfde cloud-service.

12. Selecteer in **Regio affiniteit/groep/Virtual Network**, regio, affiniteit groep of virtuele netwerk dat u wilt gebruiken voor de virtuele machine. Zie voor meer informatie over affiniteit groepen [over affiniteit groepen voor virtuele netwerk](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

13. Selecteer een bestaande account van de opslag voor het VHD-bestand in **Opslag-Account**of een automatisch gegenereerde opslag gebruikt. Slechts één account voor opslag per regio wordt automatisch gemaakt. Alle andere virtuele machines die u met deze instelling maakt bevinden zich in deze rekening opslag. U bent beperkt tot 20 opslag-accounts.

14. Als u wilt dat de virtuele machine te behoren tot een set van beschikbaarheid, in **Beschikbaar**, selecteer **beschikbaar maken** of toe te voegen aan een bestaande set van beschikbaarheid.

    **Opmerking**: virtuele machines in een set beschikbaarheid worden geïmplementeerd op een fout met betrekking tot andere domeinen. Het plaatsen van meerdere virtuele machines in de beschikbaarheid van een set kunt u ervoor zorgen dat de toepassing beschikbaar tijdens netwerkfouten, lokale schijf hardwarestoringen en elke geplande uitvaltijd is.

15.  Bekijk de nieuwe eindpunten die wordt gemaakt om de verbindingen met de virtuele machine, bijvoorbeeld via Extern bureaublad of een Secure Shell (SSH) client toestaan onder **eindpunten**. Ook kunnen nu eindpunten toevoegen of ze later maken. Zie voor instructies over het maken van deze later [het instellen van de eindpunten aan een virtuele Machine](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

16.  Onder **VM Agent**, beslissen of de Agent VM installeren. Deze agent biedt de omgeving als u extensies waarmee u kunnen communiceren met de virtuele machine installeren. Zie [Extensies beheren](http://go.microsoft.com/FWLink/p/?LinkID=390493)voor meer informatie.

17. Klik op de pijl om de virtuele machine maken.

    ![Aangepaste virtuele machine maken succesvol](./media/howto-custom-create-vm/VMSuccessWindows.png)

##<a name="next-steps"></a>Volgende stappen##
Nadat u de virtuele machine hebt gemaakt, wordt deze automatisch gestart. Wanneer u de portal geeft de status als voorlopig, kunt u zich aanmelden op de virtuele machine. Zie de volgende artikelen voor instructies:

- [Hoe u zich aanmeldt bij een virtuele Machine waarop Linux](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)
- [Hoe u zich aanmeldt bij een virtuele Machine met Windows Server](../articles/virtual-machines/virtual-machines-windows-classic-connect-logon.md)

