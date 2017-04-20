<properties
    pageTitle="Gegevens met Microsoft Avro Library serialiseren | Microsoft Azure"
    description="Informatie over hoe HDInsight Azure Avro serialiseren grote gegevens gebruikt."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Gegevens in Hadoop met Microsoft Avro Library serialiseren

Dit onderwerp wordt beschreven hoe u met de <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Bibliotheek van Microsoft Avro</a> serialisatie toepassen op objecten en andere gegevensstructuren in stromen om ze naar het geheugen, een database of een bestand behouden en hoe te converteren om de originele objecten hersteld.

[AZURE.INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

##<a name="apache-avro"></a>Apache Avro
De <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Bibliotheek van Microsoft Avro</a> implementeert de Apache Avro serialisatie systeem voor het milieu van Microsoft.NET. Apache Avro biedt een compact binaire data interchange-indeling voor serialisatie. <a href="http://www.json.org" target="_blank">JSON</a> wordt gebruikt voor het definiëren van een taal agnostic schema dat interoperabiliteit van talen beleggingsrendement. Gegevens in een bepaalde taal worden geserialiseerd kan worden gelezen in een andere. C, C++, C#, Java, PHP, Python en Ruby worden momenteel ondersteund. In de <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Specificatie van Apache Avro</a>vindt u gedetailleerde informatie over de indeling. Houd er rekening mee dat de huidige versie van de Microsoft Avro Library het onderdeel remote procedure calls (RPC's) van deze specificatie niet ondersteunt.

De geserialiseerde representatie van een object in het Avro-systeem bestaat uit twee delen: schema en de werkelijke waarde. Het schema van de Avro beschrijving van de taal-onafhankelijke gegevensmodel van de geserialiseerde gegevens met JSON. Het is aanwezig side-by-side met een binaire representatie van gegevens. Met het schema van de binaire weergave staat elk object worden geschreven met geen overheadkosten per waarde, waardoor snelle serialisatie en weergave van het kleine.

##<a name="the-hadoop-scenario"></a>Het Hadoop-scenario
De Apache Avro serialisatie-indeling wordt veel gebruikt in Azure, HDInsight en andere omgevingen Apache Hadoop. Avro biedt een handige manier om complexe gegevensstructuren binnen een project Hadoop MapReduce vertegenwoordigen. De indeling van de Avro-bestanden (Avro-object container-bestand) is ontworpen ter ondersteuning van het gedistribueerde MapReduce-programmeermodel. De belangrijkste functie waarmee u de verdeling is dat de bestanden zijn 'splitsbare"in de zin van een kunt elk punt in een bestand zoeken en lezen van een bepaald blok beginnen.

##<a name="serialization-in-avro-library"></a>Serialisatie in Avro-bibliotheek
De .NET-bibliotheek voor Avro ondersteunt twee manieren voor het serialiseren van objecten:

- **reflectie** - de JSON-schema voor de typen wordt automatisch samengesteld uit de gegevens contract kenmerken van de typen .NET moet worden toegepast.
- **algemene record** - schema A JSON expliciet wordt opgegeven in een record die wordt vertegenwoordigd door de klasse [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) wanneer geen .NET aanwezig zijn op het schema voor de gegevens worden beschreven.

Wanneer het schema van de bekend is dat de auteur en de lezer van de stream, kunnen de gegevens worden verzonden zonder het schema. In de gevallen wanneer een containerbestand van Avro-object wordt gebruikt, het schema opgeslagen in het bestand. Andere parameters, zoals de codec voor compressie van gegevens kunnen worden opgegeven. Deze scenario's worden in meer detail beschreven en geïllustreerd in de onderstaande codevoorbeelden.


## <a name="install-avro-library"></a>Avro-bibliotheek installeren

De volgende zijn vereist voor de installatie van de bibliotheek:

- <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a>
- <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 of hoger)

Houd er rekening mee dat de afhankelijkheid van de Newtonsoft.Json.dll wordt automatisch gedownload met de installatie van de Microsoft Avro Library. De procedure hiervoor wordt geleverd in de volgende sectie.


Microsoft Avro Library wordt gedistribueerd als een NuGet package die kan worden geïnstalleerd vanuit Visual Studio via de volgende procedure:

1. Selecteer het tabblad **Project** **Beheren NuGet colli...** ->
2. Zoek naar 'Microsoft.Hadoop.Avro' in het vak **Zoeken Online** .
3. Klik op de knop **installeren** naast **Microsoft Azure HDInsight Avro Library**.

Houd er rekening mee dat de Newtonsoft.Json.dll (> = 6.0.4) afhankelijkheid wordt ook automatisch gedownload met de Microsoft Avro Library.

U wilt gaat u naar de <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">introductiepagina van Microsoft Avro-bibliotheek</a> om te lezen van de huidige release.


De broncode voor Microsoft Avro Library is beschikbaar op de <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">startpagina van Microsoft Avro-bibliotheek</a>.

##<a name="compile-schemas-using-avro-library"></a>Schema's met behulp van Avro bibliotheek compileren

Microsoft Avro Library bevat een code generatie hulpprogramma waarmee de C#-typen automatisch op basis van de eerder gedefinieerde JSON schema maken. Het hulpprogramma code generatie wordt niet gedistribueerd als een binair uitvoerbaar bestand, maar kan gemakkelijk worden gemaakt via de volgende procedure:

1. Download het ZIP-bestand met de nieuwste versie van de broncode HDInsight SDK van <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK voor Hadoop</a>. (Klik op het pictogram **downloaden** , niet de tab **Downloads** ).

2. Pak de HDInsight SDK naar een map op de computer met .NET Framework 4 geïnstalleerd en aangesloten op het Internet voor het downloaden van vereiste afhankelijkheid NuGet pakketten. Hieronder wordt ervan uitgegaan dat de broncode wordt uitgepakt in de C:\SDK.

3. Ga naar de map C:\SDK\src\Microsoft.Hadoop.Avro.Tools en build.bat worden uitgevoerd. (Het bestand belt MSBuild van de 32-bits distributie van .NET Framework. Als u wilt gebruiken, de 64-bits versie bewerken build.bat, na de opmerkingen in het bestand.) Zorg ervoor dat het opbouwen voltooid is. (Op sommige systemen MSBuild kan leiden tot waarschuwingen. Deze waarschuwingen hebben geen invloed op het hulpprogramma als er geen fouten maken zijn.)

4. Het gecompileerde hulpprogramma bevindt zich in C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.


Als u bekend bent met de syntaxis voor de opdrachtregel, met de volgende opdracht worden uitgevoerd vanuit de map met het hulpprogramma code genereren:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

U kunt het hulpprogramma testen, kunt u C#-klassen uit het monster JSON schema-bestand met de broncode te genereren. Voer de volgende opdracht uit:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Dit moet resulteren in twee C#-bestanden in de huidige map: SensorData.cs en Location.cs.

Zie het bestand GenerationVerification.feature in C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc de logica die wordt gebruikt door het hulpprogramma code genereren tijdens het converteren van de JSON-schema aan de C#-typen.

Houd er rekening mee dat naamruimten worden geëxtraheerd uit het schema met de logica in het bestand in de vorige alinea genoemde JSON. Naamruimten geëxtraheerd uit het schema, hebben voorrang op hetgeen wordt geleverd met de parameter /n in het hulpprogramma vanaf de opdrachtregel. Als u de naamruimten die zijn opgenomen in het schema te overschrijven wilt, moet u de parameter /nf gebruiken. Bijvoorbeeld, u alle naamruimten van de SampleJSONSchema.avsc naar my.own.nspace, de volgende opdracht uitgevoerd:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="samples"></a>Voorbeelden
Zes voorbeelden die in dit onderwerp ziet u verschillende scenario's worden ondersteund door Microsoft Avro Library. Microsoft Avro Library is ontworpen voor gebruik met de stroom. Gegevens wordt in deze voorbeelden wordt gemanipuleerd via geheugen stromen in plaats van bestand streams of databases voor eenvoud en consistentie. De aanpak in een productieomgeving implementeert, is afhankelijk van de vereisten van het precieze scenario te reproduceren, gegevensbron en volume beperkingen prestaties en andere factoren.

De eerste twee voorbeelden hoe serialiseren en terugconverteren van gegevens in de geheugenbuffers stroom met behulp van reflectie en algemene records. Wordt uitgegaan van het schema in deze twee gevallen worden gedeeld tussen de lezers en de schrijvers van de out-of-band.

De derde en vierde voorbeelden hoe serialiseren en terugconverteren van gegevens met behulp van de Avro-object container-bestanden. Wanneer gegevens worden opgeslagen in een bestand Avro, wordt het schema altijd opgeslagen met het omdat het schema moet worden gedeeld voor deserialisatie.

Het monster met de eerste vier voorbeelden kan worden gedownload van de site <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-86055923" target="_blank">Azure codevoorbeelden</a> .

Het vijfde voorbeeld wordt getoond hoe u een aangepaste compressie-codec gebruiken voor bestanden van Avro-object container. Een voorbeeld van de code in dit voorbeeld kan worden gedownload van de site <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111" target="_blank">Azure codevoorbeelden</a> .

De zesde voorbeeld ziet u hoe gegevens uploaden naar Azure Blob-opslag en vervolgens analyseren met behulp van de component met een HDInsight (Hadoop)-cluster met Avro serialisatie. Worden kan gedownload van de site <a href="https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure codevoorbeelden</a> .

Hier vindt koppelingen naar de zes steekproeven die worden beschreven in het onderwerp:

 * <a href="#Scenario1">**Serialisatie met reflectie**</a> - schema moet worden toegepast voor de JSON wordt automatisch samengesteld uit de gegevens contract kenmerken.
 * <a href="#Scenario2">**Serialisatie met algemene record**</a> - schema van de JSON expliciet wordt opgegeven in een record als er geen type .NET beschikbaar voor reflectie is.
 * <a href="#Scenario3">**Serialisatie object container bestanden met reflectie**</a> - de JSON-schema is automatisch gemaakt en gedeeld met de geserialiseerde gegevens via een bestand Avro-object container.
 * <a href="#Scenario4">**Object container bestanden met algemene record serialisatie**</a> - de JSON-schema is expliciet opgegeven voordat de serialisatie en gedeeld met de gegevens via een bestand Avro-object container.
 * <a href="#Scenario5">**Object container bestanden met een aangepaste compressiecodec serialisatie**</a> - in het voorbeeld ziet u hoe een Avro-object container-bestand maken met een aangepaste .NET implementatie van de compressiecodec Deflate gegevens.
 * <a href="#Scenario6">**Avro gebruiken voor het uploaden van gegevens voor de service Microsoft Azure HDInsight**</a> - in het voorbeeld ziet u hoe de Avro serialisatie samenwerkt met de service HDInsight. Een actieve Azure abonnement en toegang tot een cluster Azure HDInsight moet dit voorbeeld uitvoert.

###<a name="Scenario1"></a>Voorbeeld 1: Serialisatie met reflectie

De JSON-schema voor de typen kan worden automatisch ingebouwd door de Avro Microsoft Library via reflectie van de gegevens contract kenmerken van de C#-objecten moet worden toegepast. Microsoft Avro Library maakt een [**IAvroSeralizer<T> **](http://msdn.microsoft.com/library/dn627341.aspx) voor het identificeren van de velden moet worden toegepast.

In dit voorbeeld objecten (een **SensorData** klasse met een **locatie** lid struct) naar een geheugenstream zijn geserialiseerd en deze stroom wordt op zijn beurt gedeserialiseerd. Het resultaat wordt vervolgens vergeleken met het eerste exemplaar te bevestigen dat de herstelde **SensorData** -object identiek aan het origineel is.

Het schema in het volgende voorbeeld wordt aangenomen dat moet worden gedeeld tussen de lezers en schrijvers, zodat de Avro-object containerindeling niet vereist is. Zie voor een voorbeeld van het serialiseren en gegevens in de geheugenbuffers deserialize via reflectie met de indeling van de container object wanneer het schema moet worden gedeeld met de gegevens <a href="#Scenario3">met behulp van object containerbestanden met reflectie serialisatie</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-2-serialization-with-a-generic-record"></a>Voorbeeld 2: Serialisatie met een generieke record

Een JSON-schema kan expliciet worden opgegeven in een algemene record als reflectie kan niet worden gebruikt omdat de gegevens via .NET klassen met een gegevenscontract kan niet worden weergegeven. Deze methode is in het algemeen langzamer dan met behulp van reflectie. In dergelijke gevallen wordt het schema voor de gegevens ook dynamisch, dat wil zeggen worden kunnen, niet bekend bij compilatie. Gegevens die worden vertegenwoordigd als door komma's gescheiden waarden (CSV) bestanden waarvan het schema onbekend is totdat deze wordt omgezet naar de indeling van de Avro tijdens runtime is een voorbeeld van dit soort dynamische scenario.

In dit voorbeeld ziet maken en gebruiken van een [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) een schema JSON expliciet opgeven, hoe u deze vult met de gegevens en het serialiseren en het deserialisatie. Het resultaat wordt vervolgens vergeleken met het eerste exemplaar te bevestigen dat de record hersteld identiek aan het origineel is.

Het schema in het volgende voorbeeld wordt aangenomen dat moet worden gedeeld tussen de lezers en schrijvers, zodat de Avro-object containerindeling niet vereist is. Zie voor een voorbeeld van het serialiseren en gegevens in de geheugenbuffers deserialize met behulp van een generieke record met het object containerindeling wanneer het schema toegevoegd aan de geserialiseerde gegevens worden moet in het voorbeeld van het <a href="#Scenario4">object container bestanden met algemene record serialisatie</a> .


    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn will be then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Voorbeeld 3: Serialisatie via object containerbestanden en serialisatie met reflectie

In dit voorbeeld is vergelijkbaar met het scenario in het <a href="#Scenario1">eerste voorbeeld</a>, waarbij het schema met reflectie impliciet wordt opgegeven. Het verschil is dat hier, wordt het schema niet uitgegaan bekend zijn dat de lezer die het deserializes. De **SensorData** objecten moet worden toegepast en hun impliciet opgegeven schema worden opgeslagen in een Avro-object containerbestand vertegenwoordigd door de klasse [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) .

De gegevens in dit voorbeeld met serienummer [**SequentialWriter<SensorData> **](http://msdn.microsoft.com/library/dn627340.aspx) en gedeserialiseerd met [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Het resultaat wordt vervolgens vergeleken met de eerste exemplaren te garanderen van de identiteit.

De gegevens in het object containerbestand is gecomprimeerd via de standaard [**Deflate**] [ deflate-100] compressiecodec van .NET Framework 4. Zie het <a href="#Scenario5">vijfde voorbeeld</a> in dit onderwerp voor meer informatie over het gebruik van een meer recente en superieure versie van de [**Deflate**] [ deflate-110] compressiecodec beschikbaar zijn in .NET Framework 4.5.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will be compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Voorbeeld 4: Serialisatie via object containerbestanden en serialisatie met algemene record

In dit voorbeeld is vergelijkbaar met het scenario in het <a href="#Scenario2">tweede voorbeeld</a>waarin het schema met JSON expliciet is opgegeven. Het verschil is dat hier, wordt het schema niet uitgegaan bekend zijn dat de lezer die het deserializes.

De data test set is verzameld in een lijst van objecten [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) via een expliciet gedefinieerd JSON-schema en vervolgens opgeslagen in een containerbestand die wordt vertegenwoordigd door de klasse [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) . Dit bestand maakt een auteur die wordt gebruikt voor de gegevens, niet gecomprimeerd naar een geheugenstroom dat vervolgens in een bestand opgeslagen wordt. De [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) -parameter die wordt gebruikt voor het maken van de lezer geeft aan dat deze gegevens worden niet gecomprimeerd.

De gegevens worden vervolgens lezen van het bestand en gedeserialiseerd tot een collectie van objecten. Deze collectie wordt vergeleken met de oorspronkelijke lijst met Avro records te bevestigen dat ze identiek zijn.


    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will not be compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




###<a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Voorbeeld 5: Serialisatie object container-bestanden gebruiken in een aangepaste compressie-codec

Het vijfde voorbeeld wordt getoond hoe u een aangepaste compressie-codec gebruiken voor bestanden van Avro-object container. Een voorbeeld van de code in dit voorbeeld kan worden gedownload van de site [Azure codevoorbeelden](http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111) .

Gebruik van een optionele compressiecodec (in aanvulling op standaard **Null** en **Deflate** ) kunt u de [Specificatie van de Avro](http://avro.apache.org/docs/current/spec.html#Required+Codecs) . In dit voorbeeld is niet het implementeren van een volledig nieuwe codec zoals Snappy (wordt vermeld als optioneel ondersteunde codec in de [Specificatie van de Avro](http://avro.apache.org/docs/current/spec.html#snappy)). Het laat zien hoe met de .NET Framework 4.5-uitvoering van de [**Deflate**] [ deflate-110] -codec biedt een betere compressiealgoritme op basis van de compressiebibliotheek [zlib](http://zlib.net/) dan de standaard .NET Framework 4-versie.


    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It will catch the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it will rely on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

###<a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Voorbeeld 6: Avro gebruiken voor het uploaden van gegevens voor de service Microsoft Azure HDInsight

Het zesde voorbeeld illustreert bepaalde programmeertechnieken betrekking hebben op de interactie met de service HDInsight Azure. Een voorbeeld van de code in dit voorbeeld kan worden gedownload van de site [Azure codevoorbeelden](https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3) .

In het voorbeeld gebeurt het volgende:

* Maakt verbinding met een bestaand cluster van HDInsight service.
* Serialiseert meerdere CSV-bestanden en het resultaat kunnen uploaden naar Azure Blob-opslag. (CSV-bestanden samen met het monster worden gedistribueerd en vertegenwoordigen een uittreksel uit voorraad AMEX historische gegevens geleverd door de [Infochimps](http://www.infochimps.com/) voor de periode 1970-2010. Het monster leest gegevens uit een CSV-bestand converteert records naar exemplaren van de klasse van het **materieel** en ze vervolgens met behulp van reflectie serialiseert. Definitie van het slot wordt gemaakt van een JSON-schema via het hulpprogramma Microsoft Avro Library code genereren.
* Hiermee maakt u een nieuwe externe tabel met de naam van de **voorraden** in de component en koppelingen naar de gegevens in de vorige stap hebt geüpload.
* Wordt een query uitgevoerd met behulp van component via de tabel **voorraden** .

Bovendien voert het monster een procedure opschonen vóór en na grote operaties uitvoeren. Alle verwante Azure Blob-gegevens en -mappen zijn verwijderd tijdens het opruimen, en de component-tabel wordt verwijderd. U kunt ook de procedure opruimen vanaf de opdrachtregel aanroepen.

Het monster heeft de volgende vereisten:

* Een abonnement op Microsoft Azure active en de abonnement-ID.
* Een certificaat voor het abonnement met de bijbehorende persoonlijke sleutel. Het certificaat moet worden geïnstalleerd in de huidige gebruiker particuliere opslag op de computer gebruikt voor het uitvoeren van de steekproef.
* Een actieve HDInsight cluster.
* Een opslag Azure-account is gekoppeld aan het cluster HDInsight uit de vorige vereiste, samen met de bijbehorende primaire of secundaire sleutel.

Alle informatie uit de voorwaarden moeten worden ingevoerd in het configuratiebestand van het monster vóór het uitvoeren van de steekproef. Er zijn twee manieren te werk:

* Bewerk het bestand app.config in de hoofdmap van het monster en vervolgens het monster samenstellen
* Eerst het monster samenstellen en bewerk vervolgens de AvroHDISample.exe.config in de build-map

In beide gevallen moeten alle bewerkingen worden uitgevoerd in de **<appSettings>** sectie instellingen. Als u de opmerkingen in het bestand.
Het monster wordt uitgevoerd vanaf de opdrachtregel door de volgende opdracht wordt uitgevoerd (waarbij het ZIP-bestand met het voorbeeld is uitgegaan worden uitgepakt naar C:\AvroHDISample; als het pad van het relevante anders gebruiken):

    AvroHDISample run C:\AvroHDISample\Data

Als u wilt opschonen van het cluster, moet u de volgende opdracht uitvoeren:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
