## <a name="create-an-event-hub"></a>Een Hub gebeurtenis maken

1. Meld u aan bij de [Azure portal][]en klikt u op **Nieuw** op de bovenkant van het scherm links.

2. Klik op **gegevens + Analytics**en klik vervolgens op **Gebeurtenis Hubs**.

    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)

3. Typ de naamruimtenaam van een in de blade **-naamruimte maken** . Het systeem onmiddellijk gecontroleerd als de naam beschikbaar is.

    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)

4. Nadat en zorg dat de naam beschikbaar is, kiest u de prijzen laag (Basic of Standard). Kies ook een Azure-abonnement, de resourcegroep en de locatie waar de bron maakt. 

2. Klik op **maken** om de naamruimte te maken.

6. Klik op de zojuist gemaakte naamruimte in de lijst gebeurtenis Hubs-naamruimte.      

    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)

7. Klik in de naamruimte-blade **Gebeurtenis Hubs**.

    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)

8. Klik op **Gebeurtenis Hub toevoegen**aan de bovenkant van het blad.

    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)

3. Typ een naam voor de gebeurtenis Hub en klik vervolgens op **maken**.

    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)

4. In de lijst met Hubs gebeurtenis, klikt u op de naam van de zojuist gemaakte gebeurtenis Hub. 

    ![](./media/event-hubs-create-event-hub/create-event-hub6.png)

5. Klik op **beleid voor gedeelde toegang**in de naamruimte-blade (niet de specifieke blade gebeurtenis Hub) en klik op **RootManageSharedAccessKey**.

    ![](./media/event-hubs-create-event-hub/create-event-hub7.png)

5. Klik op de knop kopiëren om de verbindingsreeks van de **RootManageSharedAccessKey** naar het Klembord kopiëren. Sla deze verbindingsreeks verderop in de zelfstudie gebruiken.

    ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

De gebeurtenis Hub wordt gemaakt en u hebt de verbindingsreeksen die u wilt verzenden en ontvangen van gebeurtenissen.

[Azure portal]: https://portal.azure.com/