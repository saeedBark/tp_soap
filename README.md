# tp_soap
 # ex1
   1. Concepts de base des services Web SOAP
    Quelle est la fonction principale du protocole SOAP ?
    La fonction principale de SOAP (Simple Object Access Protocol) est de permettre la communication entre des applications via des messages XML standardisés sur des réseaux tels qu'Internet.
    
     1- Quelle partie d’un message SOAP contient les données principales à transmettre ?
      Les données principales à transmettre se trouvent dans la section <Body> d'un message SOAP.
      
    2-  Quel langage est utilisé pour décrire un service SOAP ?
      Le langage utilisé est WSDL (Web Services Description Language).
      
    3- Quel outil est le plus adapté pour tester un service SOAP ?
      L'outil le plus couramment utilisé est SoapUI.
      
    4- Dans une approche Top-Down, quelle est la première étape ?
      La première étape est de créer un fichier WSDL qui définit les opérations et les données du service.
      
    5- Quelle annotation est utilisée pour déclarer une méthode comme une opération SOAP en Jakarta ?
      L'annotation utilisée est @WebMethod.
# exo2
  2. Questions Vrai/Faux
    SOAP ne supporte que le protocole HTTP.
    Faux. SOAP peut être utilisé avec plusieurs protocoles comme HTTP, SMTP ou JMS.
    
    Dans l’approche Bottom-Up, le fichier WSDL est écrit manuellement.
    Faux. Dans l’approche Bottom-Up, le WSDL est généré automatiquement à partir du code.
    
    SoapUI permet uniquement de tester les services REST.
    Faux. SoapUI permet de tester à la fois les services SOAP et REST.
    
    Les services SOAP utilisent généralement XML pour structurer les messages.
    Vrai.
    
    WS-Security est utilisé pour protéger les messages SOAP.
    Vrai.  

 #exo3 
  Calculator.java

   package com.example.calculator;

import jakarta.jws.WebMethod;
import jakarta.jws.WebService;

@WebService
public class Calculator {

    @WebMethod
    public double add(double num1, double num2) {
        return num1 + num2;
    }

    @WebMethod
    public double subtract(double num1, double num2) {
        return num1 - num2;
    }

    @WebMethod
    public double multiply(double num1, double num2) {
        return num1 * num2;
    }

    @WebMethod
    public double divide(double num1, double num2) throws IllegalArgumentException {
        if (num2 == 0) {
            throw new IllegalArgumentException("Division by zero is not allowed.");
        }
        return num1 / num2;
    }
  }
    CalculatorPublisher.java


    package com.example.calculator;
    
    import jakarta.xml.ws.Endpoint;
    
    public class CalculatorPublisher {
    
        public static void main(String[] args) {
            String url = "http://localhost:8080/calculatorService";
            Endpoint.publish(url, new Calculator());
            System.out.println("Calculator Service is published at: " + url);
        }
    }
 CalculatorClient.java
    
       package com.example.calculator;
    
    import java.net.URL;
    
    import jakarta.xml.ws.Service;
    import jakarta.xml.namespace.QName;

    public class CalculatorClient {

    public static void main(String[] args) {
        try {
            // Access the WSDL URL
            URL wsdlURL = new URL("http://localhost:8080/calculatorService?wsdl");
            QName serviceName = new QName("http://calculator.example.com/", "CalculatorService");
            Service service = Service.create(wsdlURL, serviceName);

            // Get the proxy object for the Calculator service
            Calculator calculator = service.getPort(Calculator.class);

            // Test the methods
            System.out.println("Addition (5 + 3): " + calculator.add(5, 3));
            System.out.println("Subtraction (5 - 3): " + calculator.subtract(5, 3));
            System.out.println("Multiplication (5 * 3): " + calculator.multiply(5, 3));
            System.out.println("Division (10 / 2): " + calculator.divide(10, 2));

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
  }


   
