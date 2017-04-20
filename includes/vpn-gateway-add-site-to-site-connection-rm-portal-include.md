1. Zoek uw gateway virtueel netwerk en op **alle instellingen** de blade **Instellingen** te openen.

2. Op het blad **Instellingen** klikt u op **verbindingen**en klik vervolgens op **toevoegen** aan de bovenkant van het blad om de bladeserver **verbinding toevoegen** .

    ![Verbinding van Site naar Site maken](./media/vpn-gateway-add-site-to-site-connection-rm-portal-include/addconnection250.png)

3. Op het blad **verbinding toevoegen** , **de naam** van de verbinding. 

4. Selecteer **het type verbinding** **Site-to-site(IPSec)**.

5. De waarde is voor **virtuele netwerkgateway**, omdat deze gateway verbinding vaste.

6. Voor het **lokale netwerk, gateway**, klikt u op **een lokaal netwerkgateway kiezen** en selecteert u het lokale netwerk-gateway die u wilt gebruiken. 

7. Voor **Gedeelde sleutel**, hier de waarde moet overeenkomen met de waarde die u voor uw lokale VPN-apparaat gebruikt. Als het VPN-apparaat op uw lokale netwerk een gedeelde sleutel niet, kunt u vormen een en deze invoer hier en op het lokale apparaat. Het belangrijkste is dat deze beide overeenkomen.

8. De resterende waarden voor het **abonnement**, **Resourcegroep**en **locatie** worden opgelost.

9. Klik op **OK** om de verbinding te maken. *Verbinding maken* met flash op het scherm wordt weergegeven.

10. Wanneer de verbinding voltooid is, ziet u dit weergegeven in de blade **verbindingen** voor de Gateway.

    ![Verbinding van Site naar Site maken](./media/vpn-gateway-add-site-to-site-connection-rm-portal-include/connectionstatus450.png)

