<properties 
    pageTitle="Polling-langdurige bewerkingen | Microsoft Azure" 
    description="In dit onderwerp wordt beschreven hoe langdurige bewerkingen te controleren." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="delivering-live-streaming-with-azure-media-services"></a>Levering van Live Streaming met Media Azure Services

##<a name="overview"></a>Overzicht

Microsoft Azure Media Services biedt API's die aanvragen verzenden naar Media Services activiteiten te starten (bijvoorbeeld: maken, starten, stoppen of verwijderen van een kanaal). Deze bewerkingen zijn langdurige.

De Media Services .NET SDK biedt API's die de aanvraag verzenden en wacht totdat de bewerking is voltooid (intern, de API's zijn polling voor bewerking uitgevoerd met bepaalde intervallen). Bijvoorbeeld wanneer u belt kanaal. Start(), retourneert de methode nadat het kanaal is gestart. U kunt ook de asynchrone versie: wachten op een kanaal. StartAsync() (Zie voor meer informatie over asynchrone patroon op basis van taken, [tikt u op](https://msdn.microsoft.com/library/hh873175(v=vs.110).aspx)). API's die een bewerking verzendt en vervolgens controleren van de status totdat de bewerking voltooid is, worden 'methoden polling' genoemd. Deze methoden (vooral de asynchrone versie) worden aanbevolen voor rijke toepassingen en statusafhankelijke diensten.

Er zijn scenario's waarbij een toepassing kan niet wachten op een langdurige HTTP-verzoek en wil worden handmatig gecontroleerd op de voortgang van de bewerking. Een typisch voorbeeld is een browser die interactie met een webservice stateless: als de browser vraagt om een kanaal te maken, de webservice initieert een langdurige bewerking en de bewerkings-ID naar de browser geretourneerd. Vraagt de browser kan de web-service de status van de bewerking op basis van de id ophalen De Media Services .NET SDK biedt API's die handig voor dit scenario zijn. Deze API's worden 'methoden niet polling' genoemd.
De "niet-polling 'methoden hebben de volgende naamgeving patroon: verzenden*OperationName*bewerking (bijvoorbeeld SendCreateOperation). Verzenden*OperationName*bewerking retourneert het object **IOperation** . het geretourneerde object bevat informatie die kan worden gebruikt voor het bijhouden van de bewerking. De verzenden*OperationName*OperationAsync retourneert **taak<IOperation>**.

Op dit moment de volgende klassen ondersteunen niet polling methoden: **kanaal**, **StreamingEndpoint**en **programma**.

Om te controleren van de bewerkingsstatus, gebruikt u de methode **GetOperation** voor de klasse **OperationBaseCollection** . Gebruik de volgende intervallen om de bewerkingsstatus: gebruiken voor **kanaal** - en **StreamingEndpoint** , 30 seconden; Gebruik 10 seconden voor de **programma** -activiteiten.


##<a name="example"></a>Voorbeeld

In het volgende voorbeeld definieert een klasse met de naam **ChannelOperations**. Deze klassedefinitie kan worden een beginpunt voor de klassedefinitie van web service. De volgende voorbeelden gebruiken voor eenvoud, de versies niet asynchrone methoden.

In het voorbeeld ziet hoe de client kunt gebruiken om deze klasse.

###<a name="channeloperations-class-definition"></a>Definitie van de klasse ChannelOperations

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;
    
        public ChannelOperations()
        {
                _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
                _context = new CloudMediaContext(_cachedCredentials);    }
    
        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });
    
            return operation.Id;
        }
    
        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;
    
            channelId = null;
    
            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }
    
    
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelInput001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelPreview001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

###<a name="the-client-code"></a>De clientcode

    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");
    
    string channelId = null;
    bool isCompleted = false;
    
    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }
    
    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);
 


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
