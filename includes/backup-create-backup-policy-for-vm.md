## <a name="defining-a-backup-policy"></a>Een back-up beleid definiëren

Een back-up beleid bepaalt een matrix van wanneer de gegevens worden momentopnamen en hoe lang deze momentopnamen blijven behouden. Wanneer u een beleid definieert voor back-up een VM, kunt u een reservekopietaak *eenmaal per dag*kunt activeren. Wanneer u een nieuw beleid maakt, wordt deze toegepast op de kluis. De back-up beleid interface ziet er zo uit:

![Back-beleid](./media/backup-create-policy-for-vms/backup-policy.png)

Een beleid maken:

1. Voer een naam voor de **naam van het beleid**.

2. Momentopnamen van uw gegevens kunnen dagelijks of wekelijks regelmatig worden genomen. Gebruik de vervolgkeuzelijst **Frequentie van de back-up** of gegevens momentopnamen worden dagelijks of wekelijks te kiezen.

    - Als u een dagelijks interval, met de gemarkeerde kunt de tijd van de dag van de momentopname selecteren. De als uur wilt wijzigen, schakelt u het uur en selecteer het nieuwe uur.

    ![Dagelijkse back-up beleid](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>

    - Als u een wekelijks interval, gebruik de gemarkeerde besturingselementen om de dag(en) van de week en het tijdstip van de dag aan de momentopname. Selecteer een of meerdere dagen in het menu van de dag. Selecteer in het menu uur één uur. Om het uur te wijzigen, schakelt u de geselecteerde uren en selecteert u het nieuwe uur.

    ![Wekelijkse back-beleid](./media/backup-create-policy-for-vms/backup-policy-weekly.png)

3. Standaard worden alle opties voor **Behoud bereik** geselecteerd. Schakel het selectievakje alle bewaarlimiet bereik die niet wilt gebruiken. Geef vervolgens de interval(s) te gebruiken.

    Maandelijkse en jaarlijkse inhouding bereiken kunnen u de momentopnamen die op basis van een wekelijks of dagelijks interval opgeven.

    >[AZURE.NOTE] Wanneer een VM beveiligt, voert een reservekopietaak eenmaal per dag. De tijd waarop de back-up wordt uitgevoerd, is hetzelfde voor elk bereik bewaren.

4. Klik op **Opslaan**nadat u alle opties voor het beleid, aan de bovenkant van het blad.

    Het nieuwe beleid wordt onmiddellijk toegepast op de kluis.
