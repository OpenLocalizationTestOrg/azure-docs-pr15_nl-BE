<properties
    pageTitle="Weergave SAML die wordt geretourneerd door de Access Control-Service (Java)"
    description="Informatie weergeven die wordt geretourneerd door de toegangsbeheerservice in Java-toepassingen die worden gehost op Azure SAML."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>Het weergeven van SAML die wordt geretourneerd door de Azure Access Control-Service

Deze handleiding wordt beschreven hoe u weer te geven van de onderliggende Security Assertion Markup Language (SAML) terug naar de toepassing door de Azure Access Control Service (ACS). De gids is gebaseerd op het onderwerp [hoe verificatie van webgebruikers met Azure Access Control Service met behulp van Eclips][] door middel van code die de SAML-informatie wordt weergegeven. De voltooide toepassing ziet er ongeveer als volgt.

![Voorbeeld van de SAML-uitvoer][saml_output]

Zie de sectie [volgende stappen](#next_steps) voor meer informatie over de ACS.

> [AZURE.NOTE]
> Het besturingselement Azure Access Services-Filter is een voorbeeld van de technologie Gemeenschap. Als pre-release software, is het niet officieel ondersteund door Microsoft.

## <a name="prerequisites"></a>Vereisten

Als u de taken in deze handleiding wilt voltooien van het monster op [het Web-gebruikers worden geverifieerd met Azure Access Control Service met behulp van Eclips][] en gebruiken als uitgangspunt voor deze zelfstudie.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>De JspWriter-bibliotheek toevoegen aan de build en de implementatie-assembly

De bibliotheek met de klasse **javax.servlet.jsp.JspWriter** om de build en de implementatie-assembly toevoegen. Als u Tomcat gebruikt, is de bibliotheek **jsp-api.jar**, dat in de map Apache **lib bevindt zich** .

1. In de Projectverkenner van de Eclips met de rechtermuisknop op **MyACSHelloWorld**, klikt u op **Pad maken**, **Bouwen pad configureren**, klikt u op het tabblad **bibliotheken** en klikt u op **Externe potten toevoegen**.
2. **In het dialoogvenster **Selectie JAR** , navigeer naar het vereiste oppervlak en selecteert u het.**
3. Met het dialoogvenster **Eigenschappen voor MyACSHelloWorld** nog geopend is, klikt u op **Deployment Assembly**.
4. Klik op **toevoegen**in het dialoogvenster **Web implementatie Assembly** .
5. Klik op **Posten van Java pad maken** in het dialoogvenster **Nieuwe Assembly-richtlijn** , en klik op **volgende**.
6. Selecteer de gewenste bibliotheek en klikt u op **Voltooien**.
7. Klik op **OK** om te sluiten van het dialoogvenster **Eigenschappen voor MyACSHelloWorld** .

## <a name="modify-the-jsp-file-to-display-saml"></a>Het JSP-bestand zodat de SAML wijzigen

**Index.jsp** de volgende code gebruiken als u wilt wijzigen.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {
        
            try
            {
                String nodeName;
                int nChild, i;
                
                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");
                   
                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }
                   
                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                   {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    
    
                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        
                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }
                                            
                                     }
                                     out.println("<br>");
                                     
                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>
    
        <%
        try 
        {
            String data  = (String) request.getAttribute("ACSSAML");
            
            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();
            
            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA); 
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();
            
            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();
    
            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e) 
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }
        
        %>
    </body>
    </html>

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

1. Uw toepassing worden uitgevoerd in de emulator van de computer of distribueren naar Azure, op [het Web-gebruikers worden geverifieerd met Azure Access Control Service met behulp van Eclips][]gedocumenteerd.
2. Start een browser en uw webtoepassing te openen. Nadat u zich aanmeldt bij uw toepassing, ziet u de SAML-informatie, met inbegrip van de security assertion geleverd door de identiteitsprovider.

## <a name="next-steps"></a>Volgende stappen

Verder verkennen van ACS-functionaliteit en om te experimenteren met meer geavanceerde scenario's, Zie [Access Control Service 2.0][].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Hoe webgebruikers worden geverifieerd met Azure Access Control-Service met behulp van de Eclips]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 