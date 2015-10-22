---
Title: Integrate TIBCO Enterprise Message Service
Product: Gateway
Section: integration-jms
DocType: Regular
Enterprise: True
---

In this procedure, you will learn how to integrate KAAZING Gateway and TIBCO Enterprise Message Service (TIBCO EMS), a standards-based enterprise messaging platform.

To Integrate TIBCO Enterprise Message Service
---------------------------------------------

TIBCO Enterprise Message Service (TIBCO EMS) is a standards-based enterprise messaging platform. The Gateway supports integration with TIBCO EMS 5.x or higher, and 4.x versions.

To configure the Gateway to work with these versions of TIBCO EMS, perform the following steps:

1.  Install TIBCO EMS following the instructions in the TIBCO EMS documentation. The directory that contains the application server installation is referred to as *TIBCO\_HOME*.
2.  Copy the JAR files listed in the following table to `GATEWAY_HOME/lib` from the appropriate directory in your *TIBCO\_HOME* (for TIBCO EMS 5.x or higher: `TIBCO_HOME/ems/version/lib`; for TIBCO 4.x: `TIBCO_HOME/ems/clients/java`):

    **TIBCO EMS 5.x or higher**
    - jms.jar (jms-2.0.jar in EMS 8)
    - tibcrypt.jar
    - tibjms.jar
    - tibjmsadmin.jar
    - tibjmsapps.jar
    - tibrvjms.jar
    
    **TIBCO EMS 4.4.3**
    - jaxp.jar
    - jms.jar
    - jndi.jar
    - jta-spec1_0_1.jar
    - tibcrypt.jar
    - tibjms.jar
    - tibjmsadmin.jar
    - tibjmsapps.jar
    - tibrvjms.jar
    
    **TIBCO EMS 4.1**
    - crimson.jar
    - jaxp.jar
    - jcert.jar
    - jms.jar
    - jndi.jar
    - jnet.jar
    - jsse.jar
    - jta-spec1_0_1.jar
    - tibcrypt.jar
    - tibjms.jar
    - tibjmsadmin.jar
    - tibjmsapps.jar
    - tibrvjms.jar


    **Note**: Instead of copying the required TIBCO EMS jar files to `GATEWAY_HOME/lib`, you can also modify the `gateway.start` or `gateway.start.bat` startup file to add these `.JAR` files (in their original `TIBCO_HOME` locations) to the CLASSPATH when the Gateway starts.

3.  If you are using TIBCO EMS 5.x or higher, open the `TIBCO_HOME/ems/version/bin/factories.conf` file. Otherwise, skip to Step 7.
4.  Configure the file according to the TIBCO EMS 5.x or higher documentation. You can alternatively copy the contents of the file from the TIBCO EMS 5.x or higher samples (`TIBCO_HOME/ems/version/samples/config`) into the `factories.conf` file.
5.  Update the `factories.conf` to enable the Gateway to access TIBCO EMS. For a local KAAZING Gateway, you can use the following configuration:

    ``` txt
    [GenericConnectionFactory]
      type                  = generic
      url                   = tcp://7222

    [TopicConnectionFactory]
      type                  = topic
      url                   = tcp://7222

    [QueueConnectionFactory]
      type                  = queue
      url                   = tcp://7222
    ```

     For a remote KAAZING Gateway, you can use the following configuration, where `tibco.example.com` is the address of TIBCO EMS.

    ``` txt
    [GenericConnectionFactory]
      type                  = generic
      url                   = tcp://tibco.example.com:7222

    [TopicConnectionFactory]
      type                  = topic
      url                   = tcp://tibco.example.com:7222

    [QueueConnectionFactory]
      type                  = queue
      url                   = tcp://tibco.example.com:7222
    ```

6.  Save the `factories.conf` file.
7.  Open the file `GATEWAY_HOME/conf/gateway-config.xml` in a text editor and locate the [jms](../admin-reference/r_conf_jms.md#jms) service element (search for "jms").
8.  Update the `jms` service element properties section as shown in the following example. This example is for use with TIBCO EMS 5.1 or higher, and uses the default credentials (username `admin` with no password).

    **Note**: The following example uses the default credentials (username `admin` with no password). If you change the credentials, you will need to update these credentials accordingly. See also [Secure the Connection from the Gateway to the Message Broker](../security/p_broker_jms_secure.md) for more information about accessing a secured message broker from the Gateway. 

    You can use `TopicConnectionFactory` or `QueueConnectionFactory` in the `connection.factory.name` element, as appropriate.

    ``` xml
    <service>
      <accept>ws://localhost:8001/jms</accept>
      <accept>wss://localhost:9001/jms</accept>
      <type>jms</type>

      <properties>
        <connection.security.principal>admin</connection.security.principal>
        <connection.security.credentials></connection.security.credentials>
        <env.java.naming.security.principal>admin</env.java.naming.security.principal>
        <env.java.naming.security.credentials></env.java.naming.security.credentials>

        <connection.factory.name>GenericConnectionFactory</connection.factory.name>
        <!-- Set destinations -->
        <context.lookup.topic.format>%s</context.lookup.topic.format>
        <context.lookup.queue.format>%s</context.lookup.queue.format>
        
        <env.java.naming.factory.initial>
          com.tibco.tibjms.naming.TibjmsInitialContextFactory
        </env.java.naming.factory.initial>
        <env.java.naming.provider.url>tcp://localhost:7222</env.java.naming.provider.url>
        <!-- Dynamic destinations support; omit for static destinations -->
        <destination.strategy>session</destination.strategy>
      </properties>

      <cross-site-constraint>
        <allow-origin>http://localhost:8001</allow-origin>
      </cross-site-constraint>
      <cross-site-constraint>
        <allow-origin>https://localhost:9001</allow-origin>
      </cross-site-constraint>
    </service>
    ```

**Notes:** 
-   The `connection.security.principal` and `connection.security.credentials` elements are used for all message brokers that require client authentication. The `env.java.naming.security.principal` and `env.java.naming.security.credentials` elements are only used for TIBCO EMS because of the implementation of TIBCO EMS connection factory. The TIBCO EMS connection factory does a remote connection to the message broker, and therefore must provide credentials. The `env.java.naming.security.principal` and `env.java.naming.security.credentials` elements instruct the TIBCO EMS connection factory on which credentials to use. For more information, see [jms Service Properties](../admin-reference/r_conf_jms.md#jms-service-properties).
-   If TIBCO EMS is unable to detect connection loss and free up durable subscribers when a Gateway instance shuts down unexpectedly, see the troubleshooting information in [TIBCO EMS Unable to Free Up Durable Subscribers](../integration-jms/p_jms_integrate_tshoot.md#tibco-ems-unable-to-free-up-durable-subscribers).

JMS Integration Verification
----------------------------

To verify that the JMS integration is working, perform the following steps:

1.  Start the TIBCO EMS Server.
2.  Start the Gateway as described in [Setting Up the Gateway](../about/setup-guide.md).
3.  In a browser, navigate to the out of the box demos at `http://localhost:8001/demo/`.
4.  Click the **JavaScript** demo.
5.  Change the value for **Location** to `ws://localhost:8001/jms`.
6.  Change the value of all **Destination** text boxes to an existing topic or queue, for example `/topic/testTopic`. This example assumes that the topic `testTopic` exists. For information on creating topics and queues, see the documentation for TIBCO EMS (search for "create topic" or "create queue" in the Command Listing.)
7.  Click **Connect**.
     The message "`CONNECT:  ws://localhost:8001/jms`" should appear in the log window followed by the message "`CONNECTED:`".
8.  Click **Subscribe**.
9.  Click **Send**. You should see the message that you sent in the message log.

Subscription and Destination Support
--------------------------------------------------------------

KAAZING Gateway integration with TIBCO Enterprise Message Service has the following subscription and destination support:

-   Durable subscriptions are supported
-   Dynamic destinations are supported. Add the [`destination.strategy`] (../admin-reference/r_conf_jms.md#destinationstrategy) property to the [jms](../admin-reference/r_conf_jms.md#jms) section and set its value to `session` to support destination lookups without using JNDI. This configuration instructs the Gateway to obtain Topic and Queue objects using the `createTopic()` and `createQueue()` JMS API methods instead of performing JNDI lookups.
-   Static destinations are supported

Notes
-----

-   To learn how to secure your JMS configuration, see [Secure Your JMS Configuration](../security/o_jms_secure.md).
-   You can also connect to your TIBCO EMS 4.4.3 or higher server using SSL. To do so, set the protocol of the `env.java.naming.provider.url` property to `ssl`. For example:

    `<env.java.naming.provider.url>ssl://localhost:7222</env.java.naming.provider.url>`

See Also
--------

-   For information on troubleshooting JMS integration, see [Troubleshoot JMS Integration](../integration-jms/p_jms_integrate_tshoot.md).
-   For information on troubleshooting JMS clients, see [Troubleshoot Your Clients](../troubleshooting/p_dev_troubleshoot.md).
-   For general troubleshooting information, see [Troubleshoot KAAZING Gateway](../troubleshooting/o_troubleshoot.md).


