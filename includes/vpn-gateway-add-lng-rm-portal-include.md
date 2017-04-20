1. Klik op **+ toevoegen**in de portal, uit **alle bronnen**. In het **Alles** blade zoekvak, typ **LAN gateway**en klik vervolgens op zoeken. Dit resulteert in een lijst. **LAN gateway** om te openen van het blad, klik op **maken** om te openen van het **lokale netwerk-gateway maken** blade.

    ![LAN gateway maken](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. Geef een **naam** op voor uw lokale netwerk gateway-object op de **maken LAN gateway blade**.
 
3. Geef een geldig openbaar **IP-adres** voor het VPN-apparaat of de netwerkgateway virtuele waarmee u verbinding wilt maken.<br>Als dit het lokale netwerk een locatie op de gebouwen vertegenwoordigt, is dit het openbare IP-adres van de VPN-apparaat waarmee u verbinding wilt maken. Het kan niet achter NAT en zijn bereikbaar voor Azure.<br>Als dit het lokale netwerk een ander VNet, geeft u het openbare IP-adres dat is toegewezen aan de virtuele netwerkgateway voor die VNet.<br>

4. **Address Space** verwijst naar de adresbereiken van de voor het netwerk waarmee deze lokale netwerk. U kunt meerdere adresbereiken ruimte toevoegen. Zorg ervoor dat de hier opgegeven bereiken elkaar niet overlappen met het bereiken van andere netwerken waarmee u verbinding wilt maken.
 
5. Voor **abonnement**, moet u controleren of het juiste abonnement zichtbaar is.

6. Selecteer de resourcegroep die u wilt gebruiken voor de **Resourcegroep**. U kunt een nieuwe resourcegroep maken of selecteren die u al hebt gemaakt.

7. Voor de **locatie**, selecteert u de locatie die in dit object worden gemaakt. U kunt selecteren die de VNet bevindt zich in dezelfde locatie, maar u bent niet verplicht om dit te doen.

8. Klik op **maken** als u wilt maken van de gateway voor het lokale netwerk.
