Integrate IBM WebSphere MQ  ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
==========================

In this procedure, you will learn how to integrate KAAZING Gateway and IBM WebSphere MQ, an IBM standard for program-to-program messaging across multiple platforms.

To Integrate IBM WebSphere MQ
-----------------------------

1.  Download and install IBM WebSphere MQ following the instructions in the WebSphere MQ documentation. The directory that contains the WebSphere MQ installation (for example,` C:\Program Files\IBM\WebSphere MQ` on Windows) is referred to as `WEBSPHEREMQ_HOME`.
2.  Copy the following JAR files from `WEBSPHEREMQ_HOME/java/lib` to `GATEWAY_HOME/lib`.
    -   `com.ibm.mq.jar`
    -   `com.ibm.mq.jmqi.jar`
    -   `com.ibm.mqjms.jar`
    -   `com.ibm.mq.headers.jar` (required for WebSphere client library version 7.5.0.0)
    -   `connector.jar`
    -   `dhbcore.jar`
    -   `fscontext.jar`
    -   `jms.jar`
    -   `jndi.jar`
    -   `jta.jar`
    -   `providerutil.jar`

3.  Open the file `GATEWAY_HOME/conf/gateway-config.xml` in a text editor and locate the `jms` service element (search for "jms").
4.  Ensure that you have the `.bindings` file, which contains the queue manager, initial context, topic name, and queue name. You will need the location of this `.bindings` file for the next step.
5.  Modify the `properties` element of the [jms](../admin-reference/r_conf_jms.md#jms) service with the following:

    ``` xml
      <properties>
        <connection.factory.name>ConnectionFactory</connection.factory.name>
        <context.lookup.topic.format>%s</context.lookup.topic.format>
        <context.lookup.queue.format>%s</context.lookup.queue.format>
        <env.java.naming.factory.initial>
          com.sun.jndi.fscontext.RefFSContextFactory
        </env.java.naming.factory.initial>

        <!-- The location of the .bindings file -->
        <env.java.naming.provider.url>
          file:/bindings_file_location
        </env.java.naming.provider.url>
        <!-- Dynamic destinations support; omit for static destinations -->
        <destination.strategy>context</destination.strategy>
      </properties>
    ```

    Replace `bindings_file_location` in the `env.java.naming.provider.url` element with the file path to the JNDI .bindings file **folder** for the connection to the broker. Do not include the .bindings file name in the file path. For example, `file:/C:/JNDI-Directory` (Windows) or `file:/Users/johndoe/Desktop/JNDI-Directory` (Mac and Linux).

    **Notes:**

    -   You can also connect to your IBM WebSphere MQ server using SSL and the `env.java.naming.provider.url` element. For more information, see [IBM WebSphere MQ documentation](http://publib.boulder.ibm.com/infocenter/wmqv6/v6r0/index.jsp?topic=%2Fcom.ibm.mq.csqzas.doc%2Fsy10920_.htm).
    -   When the Gateway detects this is a connection to IBM WebSphere MQ, the Gateway dynamically sets the following wildcard properties that are required by WebSphere:

        ```
        wildcard.any.descendent=#
        wildcard.separator=/
        wildcard.any.child=+
        ```

        See [Using Wildcards with Last Value Cache or Delta Messaging](../admin-reference/r_conf_jms.md#using-wildcards-with-last-value-cache-or-delta-messaging) for more information about using wildcards in the Gateway configuration.

JMS Integration Verification
----------------------------

To verify that the JMS integration is working, perform the following steps:

1.  Start WebSphere MQ.
2.  Start the Gateway as described in [Setting Up the Gateway](../about/setup-guide.md).
3.  In a browser, navigate to the out of the box demos at `http://localhost:8001/demo/`.
4.  Click the **JavaScript** demo.
5.  Enter the value for **Location** to `ws://localhost:8001/jms`.
6.  Change the value of all **Destination** fields to `/topic/testTopic`.
7.  Click **Connect**.
     The message "`CONNECT:  ws://localhost:8001/jms`" should appear in the log window followed by the message "`CONNECTED: ID:id_value.`"
8.  Click **Subscribe**.
9.  Click **Send**. You should see the message that you sent in the message log.

Subscription and Destination Support
--------------------------------------------------------------

KAAZING Gateway integration with IBM WebSphere MQ has the following subscription and destination support:

-   Durable subscriptions are not supported.
-   Dynamic destinations are supported. Ensure that you configure the [`destination.strategy`](../admin-reference/r_conf_jms.md#destinationstrategy) property to `context`.
-   Static destinations are supported.

Notes
-----

-   To learn how to secure your JMS configuration, see [Secure Your JMS Configuration](../security/o_jms_secure.md).
-   Because you are using a JMS provider that does not support individual message acknowledgement, you must ensure your client applications acknowledge each message received from a queue or durable subscriber. Otherwise, the client will receive only one message from that queue or durable subscriber.

See Also
--------

-   For information on troubleshooting JMS integration, see [Troubleshoot JMS Integration](../integration-jms/p_jms_integrate_tshoot.md).
-   For information on troubleshooting JMS clients, see [Troubleshoot Your Clients](../troubleshooting/p_dev_troubleshoot.md).
-   For general troubleshooting information, see [Troubleshoot KAAZING Gateway](../troubleshooting/o_troubleshoot.md).


