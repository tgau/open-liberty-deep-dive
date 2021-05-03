# Module 4: Server Configuration

From your previous addition of the MicroProfile Metrics feature in the server.xml you should now see a message for a new metrics endpoint in the terminal that looks like:

```
[INFO] [AUDIT   ] CWWKT0016I: Web application available (default_host): http://localhost:9080/metrics/

```

Open the metrics endpoint in your browser. To do this select **Launch Application**, a box will appear where the port number is required. The application is running on port **9080**. The Open Liberty homepage will load. To access the **metrics** endpoint at the end of the URL type **/metrics**. The URL should look like: 
```
http://accountname-9080.theiadocker-1.proxy.cognitiveclass.ai/metrics
```
You should see a message like this:

```
Error 403: Resource must be accessed with a secure connection try again using an HTTPS connection.
```
or a **Username** and **Password** will be required

If you take a look at the server output, you should see the following error:

```
[INFO] [ERROR   ] CWWKS9113E: The SSL port is not active. The incoming http request cannot be redirected to a secure port. Check the server.xml file for configuration errors. The https port may be disabled. The keyStore element may be missing or incorrectly specified. The SSL feature may not be enabled.
```

It's one thing to configure the server to load a feature, but many Liberty features require additional configuration. You can view the complete set of Liberty features and their configuration here: https://openliberty.io/docs/ref/config/.

The error message suggests we need to add a `keyStore` and one route to solve this would be to add a `keyStore` and user registry (e.g. a `basicRegistry` for test purposes).  However, if we take a look at the configuration for [mpMetrics](https://openliberty.io/docs/ref/config/#mpMetrics.html) we can see that it has an option to turn the metrics endpoint authentication off.

Add the following below the `</featureManager>` in the `open-liberty-masterclass/start/coffee-shop/src/main/liberty/config/server.xml`.

```XML
    <mpMetrics authentication="false" />
```
{: codeblock}

Revisit the metrics endpoint with the `http://accountname-9445.theiadocker-1.proxy.cognitiveclass.ai/metrics` URL or run the following curl command by another terminal:

```
curl http://localhost:9080/metrics
```
{: codeblock}

You should see a number of metrics automatically generated by the JVM:
```
# TYPE base_classloader_loadedClasses_count gauge
# HELP base_classloader_loadedClasses_count Displays the number of classes that are currently loaded in the Java virtual machine.
base_classloader_loadedClasses_count 14053
...
```

This doesn't contain the metrics you added because the service hasn't been called and so no application metrics have been recorded. 

Open a new terminal and use curl to `POST` some json to the application in order to generate some metrics by making a coffee order.

```
curl -X POST "http://localhost:9080/coffee-shop/resources/orders" \
     -H  "accept: */*" -H  "Content-Type: application/json" \
     -d "{\"status\":\"FINISHED\",\"type\":\"ESPRESSO\"}"
```
{: codeblock}

Reload the metrics page or rerun the curl `/metric` endpoint command. At the bottom of the metrics results, you should see:

```
...
# TYPE application_com_sebastian_daschner_coffee_shop_boundary_OrdersResource_order_total counter
# HELP application_com_sebastian_daschner_coffee_shop_boundary_OrdersResource_order_total Number of times orders requested.
application_com_sebastian_daschner_coffee_shop_boundary_OrdersResource_order_total 1
```

# Next Steps

Congratulations on completing your next exercise. Don't stop now. Move on to the next module in the master class by simply closing this tab and clicking on the next module in the Open Liberty Masterclass landing page.