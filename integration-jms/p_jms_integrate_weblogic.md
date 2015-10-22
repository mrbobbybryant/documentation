Integrate Oracle WebLogic JMS  ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
=============================

In this procedure, you will learn how to integrate KAAZING Gateway and [Oracle WebLogic JMS](http://docs.oracle.com/cd/E13222_01/wls/docs81/jms/intro.html), an enterprise-class messaging system that is tightly integrated into the WebLogic Server platform.

To Integrate Oracle WebLogic JMS
--------------------------------

1.  Download and install Oracle WebLogic Server following the instructions on the [Oracle website](http://www.oracle.com/technetwork/middleware/weblogic/overview/index.html). You might also want to consult the Oracle documentation, [Configuring and Managing JMS for Oracle WebLogic Server](http://docs.oracle.com/cd/E24329_01/web.1211/e24385/toc.htm).
2.  Determine the type of WebLogic stand-alone clients you want the Gateway to support:

    -   [WebLogic Thin T3 Client](http://docs.oracle.com/cd/E14571_01/web.1111/e13717/wlthint3client.htm#BABJBHIJ)
    -   [WebLogic Thin JMS Client](http://docs.oracle.com/cd/E14571_01/web.1111/e13717/jms_thin_client.htm#i1028796)
    -   [WebLogic Full Client](http://docs.oracle.com/cd/E14571_01/web.1111/e13717/t3.htm#i1079150)

    For information about WebLogic stand-alone clients for Oracle WebLogic Server, see the [Oracle documentation](http://docs.oracle.com/cd/E14571_01/web.1111/e13717/toc.htm).

3.  Copy the following JAR files to the library directory for the Gateway, `GATEWAY_HOME/lib`:

    -   WebLogic Thin T3 Client: `wlthint3client.jar`
    -   WebLogic Thin JMS Client: `wlclient.jar`, `wljmsclient.jar`
    -   WebLogic Full Client: `wlfullclient.jar`

4.  Open the file `GATEWAY_HOME/conf/gateway-config.xml` in a text editor and locate the `jms` service element (search for "jms").
5.  Update the jms service element properties section as shown in the following example:

    ``` xml
    <service>
        <accept>ws://localhost:8001/jmsWeblogic</accept>
        <type>jms</type>
        <properties>
            <connection.factory.name>weblogic.jms.ConnectionFactory</connection.factory.name>
            <!-- optional username & password locations -->
            <!-- <Context.SECURITY_PRINCIPAL></Context.SECURITY_PRINCIPAL> -->
            <!-- <Context.SECURITY_CREDENTIALS></Context.SECURITY_CREDENTIALS> -->
            <context.lookup.topic.format>%s</context.lookup.topic.format>
            <context.lookup.queue.format>%s</context.lookup.queue.format>
            <env.java.naming.factory.initial>
            weblogic.jndi.WLInitialContextFactory
            </env.java.naming.factory.initial>
            <env.java.naming.provider.url>t3://localhost:7001</env.java.naming.provider.url>
        </properties>
        <cross-site-constraint>
           <allow-origin>*</allow-origin>
        </cross-site-constraint>
    </service>
    ```

JMS Integration Verification
----------------------------

To verify that the JMS integration is working, perform the following steps:

1.  Start Oracle WebLogic Server.
2.  Start the Gateway as described in [Setting Up the Gateway](../about/setup-guide.md).
3.  In a browser, navigate to the out of the box demos at `http://localhost:8001/demo/`.
4.  Click the **JavaScript** demo.
5.  Change the value for **Location** to `ws://localhost:8001/jmsWeblogic`.
6.  Change the value of all **Destination** fields to `/topic/testTopic`. You must prefix the destination name with `/queue/` or `/topic/` depending on the destination type or the client will not connect.
7.  Click **Connect**.
8.  The message `"CONNECT: ws://localhost:8001/jmsWeblogic"` should appear in **Log messages** followed by the message `"CONNECTED: ID:id_value."`
9.  Click **Subscribe**.
10. Click **Send**. You should see the message that you sent in **Log messages**.


Subscription and Destination Support
------------------------------------

KAAZING Gateway integration with Oracle WebLogic JMS has the following subscription and destination support:

-   Durable subscriptions are not supported.
-   Dynamic destinations are not supported.
-   Static destinations supported. Ensure that you do not configure the [`destination.strategy`](../admin-reference/r_conf_jms.md#destinationstrategy) property in the [jms](../admin-reference/r_conf_jms.md#jms) service. Using that property will cause problems with static destinations. If you choose to retrieve destinations using session methods, each client connection to the Gateway will need to provide a destination syntax conforming to the WebLogic server requirements. See [Using Distributed Destinations](http://docs.oracle.com/cd/E13222_01/wls/docs90/jms/dds.html "Using Distributed Destinations") from Oracle for more details.

Notes
-----

-   To use the out of the box JavaScript JMS Demo with WebLogic, you must first create the queue `/queue/testQueue` and topic `/topic/testTopic` in WebLogic using the WebLogic console.
-   On UNIX or Linux, use `file:///tmp` as the value for `env.java.naming.provider.url`.
-   You can also connect to your Oracle WebLogic server using SSL and the [`env.java.naming.provider.url`](../admin-reference/r_conf_jms.md#envjavanamingproviderurl) element and the URI scheme `t3s://`. For more information, see [Additional Security Configuration for Stand-alone Clients](http://docs.oracle.com/cd/E24329_01/web.1211/e24385/aq_jms.htm#JMSAD625).
-   Because you are using a JMS provider that does not support individual message acknowledgement, you must ensure your client applications acknowledge each message received from a queue or durable subscriber. Otherwise, the client will receive only one message from that queue or durable subscriber.

See Also
--------

-   For information on troubleshooting JMS integration, see [Troubleshoot JMS Integration](../integration-jms/p_jms_integrate_tshoot.md).
-   For information on troubleshooting JMS clients, see [Troubleshoot Your Clients](../troubleshooting/p_dev_troubleshoot.md).
-   For general troubleshooting information, see [Troubleshoot KAAZING Gateway](../troubleshooting/o_troubleshoot.md).


