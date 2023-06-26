# Come creare manualmente un'app Servlet d'esempio in Windows e prompt dei comandi con Java 17, VsCode, Maven e Tomcat

## Prerequisiti:

    1) Windows 10/11 
    2) Java JDK: 17 https://adoptium.net/download/
    3) Visual Studio Code: https://code.visualstudio.com/docs/?dv=win
    4) Apache Maven 3.9.3: https://dlcdn.apache.org/maven/maven-3/3.9.2/source/apache-maven-3.9.2-src.zip
    5) Apache Tomcat 10.1: https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.10/bin/apache-tomcat-10.1.10.zip

        
## Configurazione:
        
        
    1) Configurare Java:
        Installare Java avviando l'eseguibile dal link della sezione prerequisti. 
        Verificare la versione corretta da terminale con il comando "java --version" 
        
        Questo dovrebbe essere il risultato:
        "openjdk 17.0.6 2023-01-17
        OpenJDK Runtime Environment Temurin-17.0.6+10 (build 17.0.6+10)
        OpenJDK 64-Bit Server VM Temurin-17.0.6+10 (build 17.0.6+10, mixed mode, sharing)"
        
        Se non appare la versione corretta eliminare versioni ridondanti di java 
        dal pannello di configurazoine delle variabili d'ambiente"Modifica Varibili di
        Windows"
    
    
    2) Configurare VSCode: 
        Installare VSCode dall'eseguibile scaricato. 
        
        Dalla barra in alta cliccare "View -> Exstension", 
        
        cercare la parola chiave "java" e 
        installare l'estensione "Extension Pack for Java"
        cercare la parola chiave "maven" e 
        installare l'estensione "Maven for Java" 
       
    3) Configurare Maven
        Estrarre il pachetto Maven dal file zip dal link della sezione prerequisiti e estrarlo in una cartella a piacere.
        Aggiungere la cartella "bin" delle cartella appena estratta a alla variabile d'ambiente "PATH"
        Verficare la corretta installazione con il comando da terminale "mvn -v"
    
        Il risultato sarà simile a :
        "Apache Maven 3.9.2 (c9616018c7a021c1c39be70fb2843d6f5f9b8a1c)
        Maven home: /opt/apache-maven-3.9.2
        Java version: 1.8.0_45, vendor: Oracle Corporation
        Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home/jre
        Default locale: en_US, platform encoding: UTF-8
        OS name: "mac os x", version: "10.8.5", arch: "x86_64", family: "mac""
    
    4)Configurare Tomcat
        Estrarre il pachetto Tomcat dal file zip dal link della sezione prerequisiti e 
        estrarlo in una cartella a piacere.
        

## Creare un progetto webapp con Maven

    1) Aprire il terminale e posizionarsi in una directory a piacere. 
      Inserire il comando: 
      mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-webapp -DarchetypeVersion=1.4
    
      (nota: se si usa Powershell il comando corretto è: 
      mvn archetype:generate -DarchetypeGroupId="org.apache.maven.archetypes" -DarchetypeArtifactId="maven-archetype-  webapp" -DarchetypeVersion="1.4")
      
    2) Compilare i campi richiesti. 
        Ad esempio:
        'groupId': com.test.webapp
        'artifactId': webapp
        'version': 0.1
        'package': war
      
        Inserire Y per confermare
        
    3) Verficare la presenza della nuova cartella appena creata chiamata "webapp".
        
## Configurare il progetto in Visual Studio Code
    1) Avviare Visual Studio Code
    2) Aprire cartella "webapp" creata con Maven cliccando dalla barra in alto "File -> Open Folder"
    3) Nella scheda "EXPLORER" di destra avrete la struttura della directory
    4) Aprire il file "pom.xml" e aggiungere le righe seguenti fra il tag <dependecies> e salvare il file.
    
        <dependency>
            <groupId>jakarta.servlet</groupId>
            <artifactId>jakarta.servlet-api</artifactId>
            <version>6.0.0</version>
            <scope>provided</scope>
        </dependency>
    
    5) Aprire il file "web.xml" nella cartella src/webapp/WEB-INF e aggiungere le righe seguenti dopo il tag <display-name>
    
        <servlet>
            <servlet-name>HelloServlet</servlet-name>
            <servlet-class>HelloServlet</servlet-class>
        </servlet>

        <servlet-mapping>
            <servlet-name>HelloServlet</servlet-name>
            <url-pattern>/helloservlet</url-pattern>
        </servlet-mapping>
        
    5) Nella cartella "src/main" creare nuova cartella con il nome java e creare nuovo file "HelloServlet.java"
    
    6) Aprire il file appena creato "HelloServlet.java" e inserire le seguenti righe:
    
        import jakarta.servlet.*;
        import java.io.*;


        public class HelloServlet extends GenericServlet {
          
            @Override
            public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException{

                response.setContentType("text/html");
                PrintWriter pw = response.getWriter();
                pw.println("<B>HELLO!");
                pw.close();
                
            }

        }
        
    7) Aprire file "index.jsp" e aggiunge nel tag <body> la seguente riga:

      <a href="helloservlet">Hello Servlet Test</a>  


## Creazione pacchetto war e deploy sul server
    1) Da terminale posizionarsi nella cartella "webapp" e digitare il comando: mvn package
    2) Copiare il file "webapp.war" contenuto nella cartella "target" 
        all'interno della cartella "webapps" contenuta nella cartella di tomcat 
        che abbiamo estratto precedentemente nella sezione configurazione

## Avvio del Server
    1) Apriamo il terminale e  posizioniamoci all'interno della cartella "bin" 
      della nostra cartella di tomcat
    2) Inserire il comando "startup.bat"    

## Avvio e test della web app  
    1) Avviare il nostro browser
    2) Inserire l'indirizzo http://localhost:8080/webapp/
    3) Cliccare sul link "Hello Servlet Test" e apparirà la nostra pagina servlet corrispondente alla logica del file "HelloServlet.java"
    
## Shutdown del server
    1) Per spegnere il server Tomcat, da terminale posizionarsi all'interno della cartella "bin" della nostra cartella di tomcat
    2) Inserire il comando "shutdown.bat"
