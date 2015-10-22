---
Title: Integrate JBoss Messaging
Product: Gateway
Section: integration-jms
DocType: Regular
Enterprise: True
---

In this procedure, you will learn how to integrate KAAZING Gateway and JBoss Messaging, a high-performance JMS provider that is used in combination with JBoss Application Server.

To Integrate JBoss Messaging
----------------------------

JBoss Messaging (a rewrite of JBossMQ) is a high-performance JMS provider that is used in combination with JBoss Application Server. JBoss Messaging is part of JBoss Application Server version 5.x, but you can also install and configure JBoss Messaging to work with older versions of JBoss Application Server.

With JBoss, you will need to create topics and queues using JBoss APIs or tools before connecting to them. Consult JBoss documentation for the appropriate steps to create destinations.

This section covers the following JBoss Messaging scenarios:

-   [JBoss Application Server 4.x](#jboss-application-server-4x)
-   [JBoss Application Server 5.1.x](#jboss-application-server-51x)

#### JBoss Application Server 4.x

To configure the Gateway to work with JBoss Application Server 4.x and JBoss Messaging, perform the following steps:

1.  Download and install JBoss Application Server 4.x following the instructions in the JBoss documentation. The directory that contains the application server installation is referred to as `JBOSS_HOME`.
2.  Download and install JBoss Messaging following the instructions in the JBoss documentation. The directory where JBoss Messaging is installed is referred to as `JBOSS_MESSAGING_INSTALL_DIR`.
3.  Copy the file `JBOSS_MESSAGING_INSTALL_DIR/jboss-messaging-client.jar` to `GATEWAY_HOME/lib`.
4.  Copy the following JAR files from `JBOSS_HOME/client` to `GATEWAY_HOME/lib`.
    -   jbossall-client.jar
    -   jboss-aop-jdk50.jar (optional)
    -   javassist.jar
    -   trove.jar (optional)

**Note:** Instead of copying the required JBoss jar files to `GATEWAY_HOME`/lib`, you can also modify the `gateway.start` or `gateway.start.bat` startup file to add these `.JAR` files (in their original *`JBOSS_HOME`* and *`JBOSS_MESSAGING_INSTALL_DIR`* locations) to the `CLASSPATH` when the Gateway starts. 

5.  Open the file `GATEWAY_HOME/conf/gateway-config.xml` in a text editor and locate the `jms` service element (search for "jms").
6.  Modify the [`jms service environment properties`](../admin-reference/r_conf_jms.md#jms-service-environment-properties) with the following:

    ``` xml
      <properties>
        <connection.factory.name>ConnectionFactory</connection.factory.name>
        <context.lookup.topic.format>topic/%s</context.lookup.topic.format>
        <context.lookup.queue.format>queue/%s</context.lookup.queue.format>
        <env.java.naming.factory.initial>
          org.jnp.interfaces.NamingContextFactory
        </env.java.naming.factory.initial>
        <env.java.naming.provider.url>
          jnp://localhost:1099
        </env.java.naming.provider.url>
      </properties>
    ```

**Note:** You can also connect to your JBoss Messaging server using SSL. For more information, see [JBoss Messaging documentation](http://docs.jboss.org/jbossas/jboss4guide/r1/html/ch8.chapter.html#d0e21363) and [Securing JBoss Messaging](http://publib.boulder.ibm.com/infocenter/sfsf/v9r1/index.jsp?topic=%2Fcom.ibm.help.secure.deploy.doc%2FFND_SecuringJBossMsging.html).

JMS Integration Verification for JBoss Application Server 4.x
-------------------------------------------------------------

To verify that the JMS integration is working, perform the following steps:

1.  Start the JBoss Application Server (with JBoss Messaging).
2.  Start the Gateway as described in [Setting Up the Gateway](../about/setup-guide.md).
3.  In a browser, navigate to the out of the box demos at `http://localhost:8001/demo/`.
4.  Click the **JavaScript** demo.
5.  Change the value for **Location** to `ws://localhost:8001/jms`.
6.  Change the value of all **Destination** text boxes to an existing topic or queue, for example `/topic/testTopic`.
7.  Click **Connect**.
     The message "`CONNECT:  ws://localhost:8001/jms`" should appear in the log window followed by the message "`CONNECTED: undefined`."
8.  Click **Subscribe**.
9.  Click **Send**. You should see the message that you sent in the message log.

To learn how to secure your JMS configuration: see [Secure Your JMS Configuration](../security/o_jms_secure.md).

#### JBoss Application Server 5.1.x

To configure the Gateway to work with JBoss Application Server 5.x, which includes JBoss Messaging, perform the following steps:

1.  Download and install JBoss Application Server 5.x following the instructions in the JBoss documentation. The directory that contains the application server installation is referred to as *`JBOSS_HOME`*.
2.  Copy the following JAR files from `JBOSS_HOME/client` to `GATEWAY_HOME/lib`.

    -   concurrent.jar
    -   javassist.jar
    -   jbossall-client.jar
    -   jboss-aop-client.jar
    -   jboss-client.jar
    -   jboss-common-core.jar
    -   jboss-logging-spi.jar
    -   jboss-messaging-client.jar
    -   jboss-mdr.jar
    -   jboss-remoting.jar
    -   jboss-serialization.jar
    -   jnp-client.jar
    -   trove.jar

    **Note:** Instead of copying the required JBoss jar files to `GATEWAY_HOME/lib`, you can also modify the `gateway.start` or `gateway.start.bat` startup file to add these `.JAR` files (in their original `JBOSS_HOME` locations) to the CLASSPATH when the Gateway starts.

3.  Modify the JBoss profile.xml file.

    Change the constructor line of the `AttachmentStore` definition in `JBOSS_HOME/server/default/conf/bootstrap/profile.xml` (the original version of the file does not have the `class="java.io.File"` attribute):

    ``` xml
    <bean name="AttachmentStore"
        class="org.jboss.system.server.profileservice.repository.AbstractAttachmentStore">
    <constructor>
        <parameter class="java.io.File">
            <inject bean="BootstrapProfileFactory" property="attachmentStoreRoot" />
        </parameter>
    </constructor>
    ```

4.  Open the file `GATEWAY_HOME/conf/gateway-config.xml` in a text editor and locate the `jms` service element (search for "jms").
5.  Modify the [`jms service environment properties`](../admin-reference/r_conf_jms.md#jms-service-environment-properties) with the following:

    ``` xml
      <properties>
        <connection.factory.name>ConnectionFactory</connection.factory.name>
        <context.lookup.topic.format>topic/%s</context.lookup.topic.format>
        <context.lookup.queue.format>queue/%s</context.lookup.queue.format>
        <env.java.naming.factory.initial>
          org.jnp.interfaces.NamingContextFactory
        </env.java.naming.factory.initial>
        <env.java.naming.provider.url>
          jnp://localhost:1099
        </env.java.naming.provider.url>
        <!-- Static destinations support -->
        <destination.strategy>session</destination.strategy>
      </properties>
    ```

JMS Integration Verification for JBoss Application Server 5.1.x
---------------------------------------------------------------

To verify that the JMS integration is working, perform the following steps:

1.  Start the JBoss Application Server.
2.  Start the Gateway as described in [Setting Up the Gateway](../about/setup-guide.md).
3.  In a browser, navigate to the out of the box demos at `http://localhost:8001/demo/`.
4.  Click the **JavaScript** demo.
5.  Change the value for **Location** to `ws://localhost:8001/jms`.
6.  Change the value of all **Destination** text boxes to an existing topic or queue, for example `/topic/testTopic`.
7.  Click **Connect**.
     The message "`CONNECT:  ws://localhost:8001/jms`" should appear in the log window followed by the message "`CONNECTED: undefined`."
8.  Click **Subscribe**.
9.  Click **Send**. You should see the message that you sent in the message log.

Subscription and Destination Support
--------------------------------------------------------------

KAAZING Gateway integration with JBoss Messaging has the following subscription and destination support:

-   Durable subscriptions are not supported.
-   Dynamic destinations are not supported.
-   Static destinations are supported. Ensure that you configure the jms  [`destination.strategy`](../admin-reference/r_conf_jms.md#destinationstrategy) property with the value `session`.

Notes
-----

-   To learn how to secure your JMS configuration, see [Secure Your JMS Configuration](../security/o_jms_secure.md).
-   Because you are using a JMS provider that does not support individual message acknowledgement, you must ensure your client applications acknowledge each message received from a queue or durable subscriber. Otherwise, the client will receive only one message from that queue or durable subscriber.
-   With JBoss Application Server 5.1.x, all topics and queues have to be configured beforehand, and the default JBoss configuration file does not define any topics and queues. Therefore, you must explicitly create topics and queues, including the topic used in the out of the box demo (`testTopic` in `JBOSS_HOME/server/default/conf/jboss-service.xml`):

    ``` xml
    <mbean code="org.jboss.jms.server.destination.TopicService"
        name="jboss.messaging.destination:service=Topic,name=testTopic"
        xmbean-dd="xmdesc/Topic-xmbean.xml">
        <depends optional-attribute-name="ServerPeer">jboss.messaging:service=ServerPeer</depends>
        <depends>jboss.messaging:service=PostOffice</depends>
    </mbean>
    ```

See Also
--------

-   For information on troubleshooting JMS integration, see [Troubleshoot JMS Integration](../integration-jms/p_jms_integrate_tshoot.md).
-   For information on troubleshooting JMS clients, see [Troubleshoot Your Clients](../troubleshooting/p_dev_troubleshoot.md).
-   For general troubleshooting information, see [Troubleshoot KAAZING Gateway](../troubleshooting/o_troubleshoot.md).


