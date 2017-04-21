<properties
   pageTitle="De titel van de pagina die wordt weergegeven in de browser tab en zoek resultaten"
   description="Artikel beschrijving die wordt weergegeven op de landingspagina's en in de meeste zoekresultaten"
   services="service-name"
   documentationCenter="dev-center-name"
   authors="GitHub-alias-of-only-one-author"
   manager="manager-alias"
   editor=""/>

<tags
   ms.service="required"
   ms.devlang="may be required"
   ms.topic="article"
   ms.tgt_pltfrm="may be required"
   ms.workload="required"
   ms.date="mm/dd/yyyy"
   ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

# <a name="markdown-template-article-heading-1-or-h1-heading-at-the-top-of-the-article"></a>Prijsverlaging sjabloon (artikel Kop1 of H1): kop boven het artikel

De prijsverlaging van deze sjabloon kopiëren, kopieert het artikel in uw lokale repo of de Raw-knop in de GitHub UI en kopieer de prijsverlaging.

  ![ALT-tekst; laat niet leeg. Afbeelding wordt beschreven.][8]

Inleidende alinea: Lorem dolor amet, adipiscing elit. Phasellus interdum nulla risus, lacinia Portal nisl imperdiet sed. Mauris dolor mauris, tempus sed lacinia nec, niet felis euismod. Nunc semper Portal ultrices. Maecenas neque nulla, condimentum uitgebreid ipsum sit amet, dignissim aliquet nisi.

## <a name="heading-2-h2"></a>Rubriek 2 (H2)

Aenean sit amet leo nec purus placerat fermentum ac gravida odio. Aenean tellus lectus faucibus in rhoncus in faucibus sed urna.  volutpat mi-id purus ultrices iaculis nec niet neque. [Tekst voor koppeling buiten azure.microsoft.com koppeling](http://weblogs.asp.net/scottgu). Nullam dictum dolor op pharetra van aliquam. Vivamus ac hendrerit mauris [voorbeeld linktekst voor koppeling aan een artikel in een-map](../articles/expressroute/expressroute-bandwidth-upgrade.md).

Ik krijg 10 keer meer verkeer van [Google]  [ gog] dan van [Yahoo]  [ yah] of [MSN] [msn].

> [AZURE.NOTE] Ingesprongen tekst.  Het woord 'Opmerking' worden tijdens publicatie toegevoegd. UT eu pretium lacus. Nullam purus est, iaculis sed est vel, euismod vehicula odio. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi, EU-condimentum turpis nisi een purus.

1. Aenean sit amet leo nec **Purus** placerat fermentum ac gravida odio.

2. Aenean tellus lectus faucius in **Rhoncus** in faucibus sed urna. Suspendisse volutpat mi-id purus ultrices iaculis nec niet neque.

    ![ALT-tekst; laat niet leeg. Auto verzamelaar in racing rood.][5]

3. Nullam dictum dolor op pharetra van aliquam. Vivamus ac hendrerit mauris. Sed dolor dui, condimentum et varius een vehicula op nisl.

    ![ALT-tekst; niet leeg laten][6]


Suspendisse volutpat mi-id purus ultrices iaculis nec niet neque. Nullam dictum dolor op pharetra van aliquam. Vivamus ac hendrerit mauris. Otrus informatus: [1 koppeling naar een ander onderwerp dat azure.microsoft.com documentatie](virtual-machines-windows-hero-tutorial.md)

## <a name="heading-h2"></a>Kop (H2)

UT eu pretium lacus. Nullam purus est, iaculis sed est vel, euismod vehicula odio.

1. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi, EU-condimentum turpis nisi een purus.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:
        (NSDictionary *)launchOptions
        {
            // Register for remote notifications
            [[UIApplication sharedApplication] registerForRemoteNotificationTypes:
            UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
            return YES;
        }

2. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia.

        // Because toast alerts don't work when the app is running, the app handles them.
        // This uses the userInfo in the payload to display a UIAlertView.
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
        (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
            [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
            @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }


    > [AZURE.NOTE] Duis sed diam niet <i>nisl molestie</i> pharetra eget een est. [2 Link naar een ander onderwerp dat azure.microsoft.com documentatie](web-sites-custom-domain-name.md)


Quisque commodo eros vel lectus euismod auctor eget sit amet leo. Proin faucibus suscipit tellus dignissim ultrices.

## <a name="heading-2-h2"></a>Rubriek 2 (H2)

1. Maecenas sed condimentum nisi. Suspendisse potenti.

  + Fusce
  + Malesuada
  + SEM

2. Nullam in massa eu tellus tempus hendrerit.

    ![ALT-tekst; laat niet leeg. Voorbeeld van een video Channel 9.][7]

3. Quisque felis enim, fermentum ut aliquam nec, pellentesque hoog pulvinar magna.




<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Vestibul ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Nullam ultricies, ipsum uitgebreid volutpat hendrerit purus diam pretium eros, uitgebreid tincidunt nulla lorem sed turpis: [3 koppeling naar een ander onderwerp dat azure.microsoft.com documentatie](storage-whatis-account.md).

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/        
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/    
