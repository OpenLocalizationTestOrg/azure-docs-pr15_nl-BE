####<a name="configuring-the-ios-project-in-xamarin-studio"></a>Het iOS-project configureren in Xamarin Studio

1. Open **Info.plist**in Xamarin.Studio, en het bijwerken van de **Bundel-id** met de bundel-ID die u eerder hebt gemaakt met de nieuwe App-ID.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)

2. Blader naar de **Achtergrond modi** en controleer het vak **Achtergrond modi inschakelen** en het vak **externe meldingen** . 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)

3. Dubbelklikt u op het project in het deelvenster Solution **Projectopties**te openen.

4.  Kies **bundel ondertekening iOS** onder **maken**en selecteert u de bijbehorende **identiteit** en **Provisioning profiel** dat u zojuist hebt ingesteld voor dit project. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

    Dit zorgt ervoor dat het project wordt gebruikt voor het nieuwe profiel voor ondertekening van programmacode. Zie [Xamarin apparaat ingericht]voor de officiële Xamarin apparaat documentatie wordt ingericht.

####<a name="configuring-the-ios-project-in-visual-studio"></a>Configureren van de iOS-project in Visual Studio

1. Klik met de rechtermuisknop op het project in Visual Studio, en klik vervolgens op **Eigenschappen**.

2. Klik op het tabblad **iOS toepassing** in de pagina eigenschappen en de **id** bijwerken met de ID die u eerder hebt gemaakt.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)

3. Selecteer op het tabblad **ondertekening bundel iOS** de **Provisioning profiel** dat u zojuist hebt ingesteld voor dit project en de bijbehorende **identiteit** . 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Dit zorgt ervoor dat het project wordt gebruikt voor het nieuwe profiel voor ondertekening van programmacode. Zie [Xamarin apparaat ingericht]voor de officiële Xamarin apparaat documentatie wordt ingericht.

4. Dubbel klik op Info.plist te openen en vervolgens de **RemoteNotifications** onder achtergrond modi inschakelen. 



[Xamarin apparaat inrichten]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/