Integrate RabbitMQ Messaging  ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
============================

In this procedure, you will learn how to integrate KAAZING Gateway and RabbitMQ, a highly reliable enterprise messaging system based on the AMQP standard.

**Note:** The instructions in this topic use RabbitMQ 3.1.5.

To Integrate RabbitMQ Messaging
-------------------------------

1.  Set up RabbitMQ.
    1.  Download and install RabbitMQ by following the instructions on the RabbitMQ [website](http://www.rabbitmq.com/download.html). The root folder for the RabbitMQ installation will be referred to as `RABBITMQ_HOME` in this procedure.
    2.  Download the **JMS client and server plug-in 1.0.5 for vFabric RabbitMQ 3.1.5** on the VMware [website](https://my.vmware.com/web/vmware/details?downloadGroup=VFRMQ_JMS_105&productId=349) (for Mac or Linux, **rabbitmq-jms-package-1.0.5-client-and-plugin.tar.gz**; for Windows, **rabbitmq-jms-package-1.0.5-client-and-plugin.zip**).

        **Note:** For Windows users, ensure that the downloaded files are not blocked. Before unzipping the download, right-click the zip file, click **Properties**, and click the **Unblock** button. For more information, see [Fix: Windows has blocked access to this file](http://www.thewindowsclub.com/fix-windows-blocked-access-file "Fix: Windows has blocked access to this file").

    3.  Extract the **JMS client and server plug-in 1.0.5 for vFabric RabbitMQ 3.1.5**.
    4.  Navigate to the **plugin** folder in the extracted directory and copy the file **rjms-topic-selector-1.0.5.ez**.
    5.  Navigate to the `RABBITMQ_HOME/plugins` folder and paste **rjms-topic-selector-1.0.5.ez** into that folder.
    6.  Open a shell or command prompt on the location `RABBITMQ_HOME/sbin`.
    7.  Enable the plugin by running the following command:

        For Windows:
         `rabbitmq-plugins.bat enable rabbitmq_jms_topic_exchange`

        For Mac and Linux:
         `./rabbitmq-plugins enable rabbitmq_jms_topic_exchange`

        You will see the following output:
         `The following plugins have been enabled: rabbitmq_jms_topic_exchange Plugin configuration has changed. Restart RabbitMQ for changes to take effect.`

    8.  Start RabbitMQ by running the following command:

        For Windows:
         `rabbitmq-server.bat`

        For Mac and Linux:
         `./rabbitmq-server`

        The output will contain the following:
         ` Starting broker... completed with 1 plugins.`

        The RabbitMQ broker is started along with the installed plugin.

2.  Set up the Gateway.

    1.  Download and install the Gateway as described in [Setting Up the Gateway](../about/setup-guide.md).
    2.  Navigate to the extracted folder for the **JMS client and server plug-in 1.0.5 for vFabric RabbitMQ 3.1.5**.
    3.  Copy the following files into `GATEWAY_HOME/lib`
        -   rabbitmq-jms-1.0.5.jar
        -   dependencies/amqp-client-3.1.5.jar

    4.  Next, you will need to add the **fscontext.jar** and **providerutil.jar** files from the JNDI File System Service Provider. Go to the Java Archive Downloads page here: <http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-java-plat-419418.html>
    5.  Search for **scontext-1\_2-beta3.zip** and download the file.
    6.  Extract the **scontext-1\_2-beta3.zip** file, and copy the following files from the extracted **lib** folder into `GATEWAY_HOME/lib`: **fscontext.jar**, **providerutil.jar**.
    7.  Create a JNDI .bindings file for the connection to the RabbitMQ broker. Here is an example of a .bindings file:

        ``` txt
        ConnectionFactory/ClassName=com.rabbitmq.jms.admin.RMQConnectionFactory
        ConnectionFactory/FactoryName=com.rabbitmq.jms.admin.RMQObjectFactory

        ConnectionFactory/RefAddr/0/Content=jms/ConnectionFactory
        ConnectionFactory/RefAddr/0/Type=name
        ConnectionFactory/RefAddr/0/Encoding=String

        ConnectionFactory/RefAddr/1/Content=javax.jms.ConnectionFactory
        ConnectionFactory/RefAddr/1/Type=type
        ConnectionFactory/RefAddr/1/Encoding=String

        ConnectionFactory/RefAddr/2/Content=com.rabbitmq.jms.admin.RMQObjectFactory
        ConnectionFactory/RefAddr/2/Type=factory
        ConnectionFactory/RefAddr/2/Encoding=String

        # Change this line accordingly if the broker is not at localhost
        ConnectionFactory/RefAddr/3/Content=localhost
        ConnectionFactory/RefAddr/3/Type=host
        ConnectionFactory/RefAddr/3/Encoding=String
        ```

        For static destinations, you need to add an additional set of entries for each static destination. The snippet below shows how to add a static topic named "MyTopic", and a queue named "MyQueue" in JNDI:

        ``` txt
        MyTopic/ClassName=com.rabbitmq.jms.admin.RMQDestination
        MyTopic/FactoryName=com.rabbitmq.jms.admin.RMQObjectFactory

        MyTopic/RefAddr/0/Content=jms/Topic
        MyTopic/RefAddr/0/Type=name
        MyTopic/RefAddr/0/Encoding=String

        MyTopic/RefAddr/1/Content=javax.jms.Topic
        MyTopic/RefAddr/1/Type=type
        MyTopic/RefAddr/1/Encoding=String

        MyTopic/RefAddr/2/Content=com.rabbitmq.jms.admin.RMQObjectFactory
        MyTopic/RefAddr/2/Type=factory
        MyTopic/RefAddr/2/Encoding=String

        MyTopic/RefAddr/3/Content=MyTopic
        MyTopic/RefAddr/3/Type=destinationName
        MyTopic/RefAddr/3/Encoding=String


        MyQueue/ClassName=com.rabbitmq.jms.admin.RMQDestination
        MyQueue/FactoryName=com.rabbitmq.jms.admin.RMQObjectFactory

        MyQueue/RefAddr/0/Content=jms/Queue
        MyQueue/RefAddr/0/Type=name
        MyQueue/RefAddr/0/Encoding=String

        MyQueue/RefAddr/1/Content=javax.jms.Queue
        MyQueue/RefAddr/1/Type=type
        MyQueue/RefAddr/1/Encoding=String

        MyQueue/RefAddr/2/Content=com.rabbitmq.jms.admin.RMQObjectFactory
        MyQueue/RefAddr/2/Type=factory
        MyQueue/RefAddr/2/Encoding=String

        MyQueue/RefAddr/3/Content=MyQueue
        MyQueue/RefAddr/3/Type=destinationName
        MyQueue/RefAddr/3/Encoding=String
        ```

         **Notes:**

        -   If you need to modify the .bindings file, please refer to the attributes at: [Installing and Configuring JMS Client for vFabric RabbitMQ](http://pubs.vmware.com/vfabric53/index.jsp?topic=/com.vmware.vfabric.rabbitmq.3.2/jms-client/install.html). For example, the value of `ConnectionFactory/RefAddr/<an-attribute-number>/Type` in this file corresponds to the attribute names of `jms/ConnectionFactory`, which are described in the table in the link above. The value of the attribute would then be `ConnectionFactory/RefAddr/<same-attribute-number>/Content` in this file.</span>
        -   If you are running RabbitMQ on a non-standard port or with authorization enabled, you might need to specify the port, username, and password to connect to RabbitMQ. For more information, see [Configure JMS Applications to Use JMS Client for vFabric RabbitMQ](http://pubs.vmware.com/vfabric53/index.jsp?topic=/com.vmware.vfabric.rabbitmq.3.2/jms-client/install-configure-client.html).


    8.  Note the location of your JNDI .bindings file for the connection to the RabbitMQ broker. You will use this file location when configuring the [jms](../admin-reference/r_conf_jms.md#jms) service on the Gateway. A file path without spaces is easier to configure.
    9.  Open the configuration file for the Gateway, located at `GATEWAY_HOME/conf/gateway-config.xml`.
    10. Modify the `properties` element of the [jms](../admin-reference/r_conf_jms.md#jms) service with the following:

        ``` xml
        <service>
          <accept>ws://example.com:8000/jms</accept>

          <type>jms</type>

          <properties>
            <connection.factory.name>ConnectionFactory</connection.factory.name>
            <!-- Set dynamic destinations -->
            <context.lookup.topic.format>%s</context.lookup.topic.format>
            <context.lookup.queue.format>%s</context.lookup.queue.format>

            <env.java.naming.factory.initial>
              com.sun.jndi.fscontext.RefFSContextFactory
            </env.java.naming.factory.initial>
            <env.java.naming.provider.url>
              file:/bindings_file_location
            </env.java.naming.provider.url>
            <!-- Dynamic destinations support; omit for static destinations -->
            <destination.strategy>session</destination.strategy>
          </properties>
          
          <realm-name>demo</realm-name>
          
          <cross-site-constraint>
            <allow-origin>http://example.com:8000</allow-origin>
          </cross-site-constraint>
        </service>
        ```

        Replace `bindings_file_location` in the `env.java.naming.provider.url` element with the file path to the JNDI .bindings file **folder** for the connection to the RabbitMQ broker. Do not include the .bindings file name in the file path. For example, `file:/C:/JNDI-Directory` (Windows) or `file:/Users/johndoe/Desktop/JNDI-Directory` (Mac and Linux).

    11. Start the Gateway as described in [Setting Up the Gateway](../about/setup-guide.md).

        When the Gateway detects the connection to RabbitMQ, the Gateway dynamically sets the following wildcard properties that are required by RabbitMQ:

        ```
        wildcard.any.descendent=#
        wildcard.separator=/
        wildcard.any.child=+
        ```

        See [Using Wildcards with Last Value Cache or Delta Messaging](../admin-reference/r_conf_jms.md#using-wildcards-with-last-value-cache-or-delta-messaging) for more information about using wildcards in the Gateway configuration.

JMS Integration Verification
----------------------------


1.  In a browser, navigate to the out of the box demos at `http://localhost:8001/demo/`.
2.  Click the **JavaScript** demo.
3.  Click **Connect**.
    The status message `CONNECT: ws://localhost:8001/jms` appears followed by `CONNECTED`.
4.  Click **Subscribe**.
    The status message `SUBSCRIBE: /topic/destination` appears.
5.  Click **Send**. The sent message appears in **Log messages** along with the received message from the subscription. For example:
    
    ```
    SEND TextMessage: Hello, message
    Destination: /topic/destination
    RECEIVED TextMessage: Hello, message
    Destination: /topic/destination [#3]
    ```

    **Note:** Transactions are not supported.

Notes
-----

-   Because you are using a JMS provider that does not support individual message acknowledgement, you must ensure your client applications acknowledge each message received from a queue or durable subscriber. Otherwise, the client will receive only one message from that queue or durable subscriber.

See Also
--------

-   For information on troubleshooting JMS integration, see [Troubleshoot JMS Integration](../integration-jms/p_jms_integrate_tshoot.md).
-   For information on troubleshooting JMS clients, see [Troubleshoot Your Clients](../troubleshooting/p_dev_troubleshoot.md).
-   For general troubleshooting information, see [Troubleshoot the Gateway](https://github.com/michaelcretzman/gateway/blob/develop/doc/troubleshooting/o_troubleshoot.md).
-   [Configure JMS Applications to Use JMS Client for vFabric RabbitMQ](http://pubs.vmware.com/vfabric53/index.jsp?topic=/com.vmware.vfabric.rabbitmq.3.2/jms-client/install-configure-client.html) from VMware.
-   [vFabric Suite 5.3 Documentation](http://pubs.vmware.com/vfabric53/index.jsp?topic=/com.vmware.vfabric.rabbitmq.3.2/rabbit-web-docs/plugins.html "vFabric Documentation Center") from VMware (RabbitMQ plugins guide).
-   For information about message selectors support in the vFabric JMS Client for vFabric RabbitMQ, see [About JMS Client for vFabric RabbitMQ](http://pubs.vmware.com/vfabric53/topic/com.vmware.vfabric.rabbitmq.3.2/jms-client/overview.html "About JMS Client for vFabric RabbitMQ") from VMware.
-   For information on the limitations of using the vFabric JMS Client for vFabric RabbitMQ, [Using the JMS Client for vFabric RabbitMQ](http://pubs.vmware.com/vfabric53/topic/com.vmware.vfabric.rabbitmq.3.2/jms-client/overview-limitations.html "Limitations") from VMWare.
-   For information about `Reffscontextfactory`, see the CapTech Consulting Blogs post [A Riff on the RefFSContextFactory](http://blogs.captechconsulting.com/blog/david-tiller/riff-the-reffscontextfactory).
-   For information on topic name syntax (wildcards, hierarchy), see the [RabbitMQ Tutorial - Topics from VMware](http://pubs.vmware.com/vfabric53/topic/com.vmware.vfabric.rabbitmq.3.2/rabbit-web-docs/tutorials/tutorial-five-java.html "RabbitMQ Tutorial - Topics").


