<properties
    pageTitle="De eenheid van het rolletje een bal-zelfstudie"
    description="Stappen voor het maken van de eenheid van de klassieke rol een bal spel spelen voor alle mobiele Engagement eenheid zelfstudies is"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a id="unity-roll-a-ball"></a>Maak de eenheid van vorig een bal spel

Deze zelfstudie doorloopt de belangrijkste stappen enigszins gewijzigde [Unity rol een bal zelfstudie](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). In dit voorbeeld van een game bestaat uit een bolvormige player-object dat wordt beheerd door de gebruiker van de app en het doel van het spel is om te 'verzamelen' verzamelobjecten: objecten door de player-object met deze objecten verzamelobjecten: tegenaan. Vertrouwd zijn met de eenheid van het editor-omgeving wordt verondersteld. Als u problemen ondervindt en vervolgens moet u de volledige zelfstudie verwijzen. 

### <a name="setting-up-the-game"></a>Instellen van het spel
De volgende stappen zijn van de [eenheid van de zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. **Eenheid van de Editor** te openen en klik op **Nieuw**. 
    
    ![][51] 
    
2. Geef een **naam van het Project** & **locatie**, selecteert u **3D** en klik op **project maken**.
    
    ![][52]

3. Sla de standaard scène zojuist hebt gemaakt als onderdeel van het nieuwe project net als bij de naam van de **MiniGame** in een nieuw ** \_scènes** map onder de map **elementen** :
    
    ![][53]

4. Maak een **3D-Object -> vlak** als op het speelveld en de naam van dit object vlak **terrein**

    ![][1]

5. De transform-component voor dit object **grond** opnieuw zodat deze op de oorsprong. 

    ![][3]

6. Schakel **Raster weergeven** in **menu Gizmos** voor de **grond** -object.

    ![][4]

7. Het onderdeel **schaal** voor de **grond** -object zijn bijgewerkt [X = 2, Y = 1, Z = 2]. 

    ![][5]

8. Een nieuwe **3D-Object bol ->** toevoegen aan het project en de naam van dit object sphere als **speler**. 

    ![][6]

9. Selecteer het object **speler** en klik op **Opnieuw transformeren** vergelijkbaar met het object vlak. 

10. Update **Transform -> positie -> Y-coördinaat** component voor de Y-speler als 0,5.  

    ![][7]

11. Maak een nieuwe map genoemd **materiaal** in het project waar we het materiaal maken aan de kleur van de speler. 

12. Maak een nieuw **materiaal** ( **achtergrond)** in deze map. 

    ![][8]

13. De kleur van het materiaal bijwerken door de eigenschap **Albedo** van deze bij te werken. Kunt u de RGB-waarden van [0,32,64]. 

    ![][9]

14. Sleep dit materiaal in de weergave scène kleur wilt toepassen op het object van de **grond** . 

    ![][10]

17. Tot slot werken de **Transform -> rotatie -> Y** 60 op het object gericht licht voor de duidelijkheid. 

    ![][12]

### <a name="moving-the-player"></a>Verplaatsen van de speler
De volgende stappen zijn van de [eenheid van de zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Een **RigidBody** component toevoegen aan het object **Player** . 

    ![][13]

2. Maak een nieuwe map met de naam **Scripts** in het Project. 

3. Klik op **-onderdeel toevoegen -> nieuw Script Script C# ->**. Geef deze de naam **PlayerController**en klik op **maken en toevoegen**. Dit maakt en een script toevoegen aan het object Player.  

    ![][14]

5. Dit script in de map **Scripts** in het project gaan. 

6. Het script voor het bewerken in uw favoriete scripteditor openen, de scriptcode bijwerken met de volgende code en sla het op. 

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
    
8. Houd er rekening mee dat de bovenstaande script maakt gebruik van een eigenschap van de **snelheid** . Werken in de editor de eenheid van de eigenschap speed tot en met 10.  

    ![][15]

9. Druk op **spelen** in de eenheid van de Editor. Nu moet mogelijk zijn de bal met het toetsenbord besturen en moet draaien en bewegen. 

### <a name="moving-the-camera"></a>Verplaatsen van de camera
De volgende stappen zijn van de [eenheid van de zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) en zoet zullen de **Belangrijkste Camera** op de **Player** -object. 

1. Bijwerken van de **Transform.Position** als X = 0, Y = 10.5, Z =-10.  
2. Bijwerken van de **Transform.Rotation** als X = 45, Y = 0, Z = 0.  

    ![][16]

2. Een nieuw script genaamd **CameraController** aan de **MainCamera** toevoegen en verplaatsen in de map Scripts. 

    ![][17]

3. Open het script voor het bewerken en voeg de volgende code in het:

        using UnityEngine;
        using System.Collections;
        
        public class CameraController : MonoBehaviour {
        
            public GameObject player;
        
            private Vector3 offset;
        
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
            
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
    
5. Sleep in de eenheid van het milieu - de Player-variabele in de sleuf van de speler voor de belangrijkste Camera-object zodat de twee aan elkaar gekoppeld zijn. 

    ![][18]

6. Nu als u druk op Play in de eenheid van de editor en de bal speler object roteren ziet vervolgens u de volgende tekst in het verkeer Camera.  

### <a name="setting-up-the-play-area"></a>Het afspeelgebied instellen
De volgende stappen zijn van de [eenheid van de zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). We gaan de wanden rond de grond zodat de bal speler-object niet op het afspeelgebied in de verplaatsing inleveren maken. 

1. Klik op **maken -> leeg maken -> Object spel** en noem deze **wanden**

    ![][19]

2. Onder dit object wanden - Maak een nieuwe **3D-Object -> Cube** en noem deze "West wall". 

    ![][20]

3. De **Transform -> positie** en de **Transform -> schaal** bijwerken voor dit object West Wall. 

    ![][21]

4. Dubbele wand van de West een **Oostelijke muur** maken met de bijgewerkte transformatie plaatsen en schalen. 

    ![][22]

5. De wand Oost **Noord muur** maken met de bijgewerkte transformatie positie en schaal dupliceren. 

    ![][23]

6. Dupliceer de wand Noord en een **Zuid-muur** maken met de bijgewerkte transformatie positie en schaal. 

    ![][24]

### <a name="creating-collectible-objects"></a>Verzamelobjecten: objecten maken
De volgende stappen zijn van de [eenheid van de zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). We gaan maken sommige aantrekkelijk ogende objecten die de set verzamelobjecten: objecten die het object bal speler verzamelen moet door met hen tegenaan zal vormen. 

1. Een nieuw **3D-object van de kubus** en noem deze afhalen. 

2. Pas de **transformatie rotatie ->** & **Transform -> schaal** van het object afhalen. 

    ![][25]

3. Maken en koppelen van een **Nieuwe C# Script** **Rotator** aangeroepen voor het object afhalen. Zorg ervoor dat het script in de map Scripts plaatsen. 

    ![][26]

4. Open dit script voor het bewerken en bijwerken als u het volgende: 

        using UnityEngine;
        using System.Collections;
        
        public class Rotator : MonoBehaviour {
        
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }

5. Raak nu worden de afspeelmodus in de eenheid van de Editor en de voorstelling Pickup-object op de as draaien.

6. Maak een nieuwe map genaamd **Prefabs** 

    ![][27]

7. Sleep het object **ophalen** en plaatsen in de map Prefabs.

    ![][28]

8. Maak een nieuw **spel leeg object** **overdracht op**genoemd. De positie opnieuw in de oorsprong en sleept u het object afhalen onder dit object spel.  

    ![][29]

9. Dupliceert het object **afhalen** en zich verspreiden op de **grond** -object om het object van de **speler** door de **Transform.Position van X en Z** -waarden op de juiste wijze bij te werken. 

    ![][30]

10. Een **nieuw materiaal** genoemd **Pickup** maken en bijwerken om de rode kleur worden door de **eigenschap Albedo** vergelijkbaar met wat we hebben gedaan voor het bijwerken van het object grond bij te werken. 

    ![][31]

11. Het materiaal van toepassing op alle 4 pickup-objecten.

    ![][32]

### <a name="collecting-the-pickup-objects"></a>De Pickup objecten verzamelen
De volgende stappen zijn van de [eenheid van de zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). De speler wordt bijgewerkt zodat deze ' verzamelen ' objecten ophalen door ze tegenaan. 

1. Open het **PlayerController** script dat is gekoppeld aan het object Player bewerken en bijwerken naar de volgende:  

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour {
        
            public float speed;
        
            private Rigidbody rb;
        
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
        
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
        
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
        
                rb.AddForce (movement * speed);
            }
        
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }

2. Maak een nieuwe **Tag** naam **Pick Up** (moet overeenkomen met wat er in het script)  

    ![][33]
    
    ![][34]

3. Deze **code** van toepassing op het object afhalen Prefab. 

    ![][35]

4. Selectievakje voor het object Prefab **IsTrigger** inschakelen.

    ![][36]

5. Een stijve instantie aan Prefab Pickup object toevoegen. Voor optimalisatie van prestaties wordt de statische collider waarmee we bijgewerkt tot een dynamische collider. 

    ![][37]
  
6. Controleer ten slotte de eigenschap **IsKinematic** voor de prefab-object. 

    ![][38]

7. Treffers **spelen** in de eenheid van de editor en u kunnen worden deze **rol een bal** om spel te spelen door het verplaatsen van de Player-object met behulp van de toetsen voor de invoer van richting. 

### <a name="updating-the-game-for-mobile-play"></a>Het spel spelen op mobiele bijwerken
De bovenstaande secties de eenvoudige zelfstudie van de eenheid van het gesloten. We zullen nu het spel op het mobiele apparaat maken beschrijvende wijzigen. Houd er rekening mee dat we voor het spel tot nu toe voor het testen van invoer van het toetsenbord gebruikt. Nu zullen we wijzigen zodat we de speler beheren kunt met behulp van de beweging van de telefoon, dat wil zeggen versnellingsmeter als invoer gebruikt. 

Open het **PlayerController** script voor het bewerken en bijwerken van de methode **FixedUpdate** om de beweging van de versnellingsmeter gebruiken om de speler-object te verplaatsen. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Deze zelfstudie concludeert maken van een eenvoudig spel met de eenheid en u deze kunt implementeren op een apparaat van uw keuze om het spel te spelen. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png  
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png  
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png

    
    
    
    
    
    
    
    
    
    
    
    
