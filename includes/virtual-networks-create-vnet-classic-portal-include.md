## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Een VNet maken in de portal Azure

U maakt een VNet op basis van het bovenstaande scenario, de volgende stappen uit te voeren.

1. Vanuit een browser, Ga naar http://manage.windowsazure.com en meld u zonodig met uw Azure-account.
2. Klik op **Nieuw** > **NETWORK SERVICES** > **VIRTUEEL netwerk** > **Aangepaste maken** zoals in de onderstaande afbeelding.

    ![VNet maken in de portal](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure1.gif)

3. Typ de **naam** van de VNet op de pagina **Virtuele netwerkdetails** selecteren de **locatie**en klik op de pijl in de rechterbenedenhoek van de pagina om terug te gaan naar stap 2. In de volgende afbeelding ziet u de instellingen voor ons scenario.

    ![De detailpagina virtueel netwerk](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure2.png)

4. Geef de naam en het IP-adres voor maximaal 9 DNS-servers u wilt gebruiken op de pagina **DNS-Servers en VPN-verbindingen** . Als u een DNS-server niet opgeeft, gebruikt de VNet het interne naamgeving resolutie oplossing geleverd door Azure. In ons scenario zullen we geen DNS-servers configureren.
5. Als u point-to-site VPN-toegang geven tot uw VNet wilt, activeert u het vakje **met een punt-naar-site VPN configureren** . Als een point-to-site VPN niet is geconfigureerd, kunt u deze toevoegen aan uw VNet op elk gewenst moment na het maken. Voor ons scenario zullen we een point-to-site VPN niet configureren.
6. Als u wilt de site naar site VPN-connectiviteit tussen uw VNet en een andere VNet of het netwerk op gebouwen, activeert u het vakje **een VPN van site naar site configureren** en opgeven als u met **ExpressRoute** of notitie en de naam van het netwerk verbinding maken wilt met. Als een site naar site VPN niet is geconfigureerd, kunt u deze toevoegen aan uw VNet op elk gewenst moment na het maken. In ons scenario zullen we niet configureren van een VPN website.
7. Klik op de pijl in de rechterbenedenhoek van de pagina om terug te gaan naar stap 3 die in de volgende afbeelding ziet u de instellingen voor ons scenario.

    ![Pagina voor DNS-Servers en VPN-verbindingen](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure3.png)

8. Op de pagina **Virtuele netwerk adresruimten** onder **IP starten**, klik op *10.0.0.0* de adresruimte van de VNet wijzigen en typ de begin-adresruimte die u wilt gebruiken. In ons scenario typt *192.168.0.0*. 
9. Selecteer het aantal bits voor subnetmasker onder **CIDR (aantal ADRESSEN)** . Voor ons scenario selecteert u *16 (65536)*.
10. Onder de **SUBNETTEN**, klikt u op *Subnet 1* en de naam van het subnet indien nodig. Voor ons scenario, wijzig de *FrontEnd*.

    >[AZURE.NOTE] Als u buiten het tekstvak naam voor een subnet op u niet mogelijk de naam bewerken als het subnet opnieuw. Om op te lossen, moet u het subnet verwijderen door te klikken op de knop X rechts ervan, voegt u een nieuw subnet zoals beschreven in stap 13 hieronder.

11. Geef onder **Begint** voor het eerste subnet, het eerste IP-adres voor het subnet. In ons scenario typt *192.168.1.0*.
12. Selecteer het aantal bits van het subnetmasker voor het eerste subnet onder **CIDR (aantal ADRESSEN)** . Selecteer in ons scenario *24 (256)*.
13. Klik op toevoegen een nieuw subnet, **subnet toevoegen** indien nodig. Voor ons scenario een subnet toevoegen en Herhaal stap 10 tot en met 12 voor het configureren van de VNet zoals aangegeven in de onderstaande afbeelding.

    ![Virtueel netwerk adres spaties pagina](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure4.png)

14. Klik op het vinkje in de rechterbenedenhoek van de pagina voor het maken van de VNet. Na een paar seconden worden uw VNet weergegeven in de lijst met beschikbare VNets, zoals in de onderstaande afbeelding.

    ![Nieuw virtueel netwerk](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure5.png)