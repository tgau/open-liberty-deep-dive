# Module 2: Dev Mode

The Open Liberty Maven plug-in includes a dev goal that listens for any changes in the project, including application source code or configuration. The Open Liberty server automatically reloads the configuration without restarting. This goal allows for quicker turnarounds and an improved developer experience.

Lets start our server up in dev mode and make some changes to the configuration so that it will need to install new features while the server is still running:

```
mvn install liberty:dev
```
{: codeblock}

Take a look at the maven build file for the coffee-shop project: `open-liberty-masterclass/start/coffee-shop/pom.xml`

The Open Liberty Maven plugin must be version 3.x or above to use dev mode. We define the versions of our plugins at the top of our pom:

```XML
    <!-- Plugin Versions-->
       <version.liberty-maven-plugin>3.2</version.liberty-maven-plugin>
       <version.maven-compiler-plugin>3.5.1</version.maven-compiler-plugin>
       <version.maven-failsafe-plugin>3.0.0-M4</version.maven-failsafe-plugin>
       <version.maven-war-plugin>3.2.3</version.maven-war-plugin>
```
 

In the same `coffee-shop/pom.xml` locate the `<dependencies/>` section.  All the features we are using in this Masterclass are part of Jakarta EE and MicroProfile. By having the two dependencies below means that at build time these are available for Maven to use and then it will install any of the features you requests in your server.xml but we will get to that shortly.

``` XML
    <dependencies>
      <!--Open Liberty features -->
        <dependency>
            <groupId>jakarta.platform</groupId>
            <artifactId>jakarta.jakartaee-web-api</artifactId>
            <version>8.0.0</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.eclipse.microprofile</groupId>
            <artifactId>microprofile</artifactId>
            <version>3.3</version>
            <type>pom</type>
            <scope>provided</scope>
        </dependency> 
        ...
    </dependencies>
```

Let's add add dependency on the `MicroProfile OpenAPI` feature so we can try the `coffee-shop` service out.

We have already loaded the MicroProfile 3.3 feature in the pom that will include the latest version of MicroProfile OpenAPI so we just need to configure the Open Libetty server.

Open the file `open-liberty-masterclass/start/coffee-shop/src/main/liberty/config/server.xml`

This file is the configuration for the `coffee-shop` server.

Near the top of the file, you'll see the following `<featureManager/>` entry:

```XML
    <featureManager>
        <feature>jaxrs-2.1</feature>
        <feature>ejbLite-3.2</feature>
        <feature>cdi-2.0</feature>
        <feature>beanValidation-2.0</feature>
        <feature>mpHealth-2.2</feature>
        <feature>mpConfig-1.4</feature>
        <feature>mpRestClient-1.4</feature>
        <feature>jsonp-1.1</feature>
    </featureManager>
```
This entry lists all the features to be loaded by the server.  Add the following entry inside the `<featureManager/>` element:

```XML
        <feature>mpOpenAPI-1.1</feature>
```
{: codeblock}

If you now go back to your terminal you should notice Open Liberty installing the new features without shutting down. You can also re-run tests by simply pressing enter in the Terminal. 

For a full list of all the features available, see https://openliberty.io/docs/ref/feature/.

# Next Steps

Congratulations on completing your next excercise. Don't stop now. Move on to the next module in the master class by simply closing this tab and clicking on the next module in the Open Liberty Masterclass landing page.