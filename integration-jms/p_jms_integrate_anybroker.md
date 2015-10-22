Integrate Any JMS-Compliant Message Broker  ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
==========================================

In this procedure, you will learn how the [jms](../admin-reference/r_conf_jms.md#jms) service allows you to configure KAAZING Gateway to connect to any back-end JMS-compliant message broker.

JMS broker implementations provide a factory class in their JAR files. (For example, the Apache ActiveMQ factory class that ships in the KAAZING Gateway bundle is `org.apache.activemq.jndi.ActiveMQInitialContextFactory` in the JAR file `activemq-client-5.10.0.jar`.) To use the `jms` service with a JMS-compliant message broker, you would copy the jar file to `GATEWAY_HOME/lib` and specify the name of the factory class in the service configuration element.

**Note**: Java Naming and Directory Interface (JNDI) is a Java API for looking up objects by name (a directory service). With the Gateway, you use JNDI in two ways: 1. to set up the initial context and call the KAAZING Gateway JMS Client Library from your JMS client or 2. to connect from the Gateway to your JMS-compliant message broker and lookup topics and queues. This section focuses on the latter, where you want to configure the Gateway to communicate with your message broker. For information about setting up JMS clients, see the appropriate client developer topics in [For Developers](../index.md).

For information on integrating the Gateway with specific JMS-Compliant Message Brokers, see:

-   [Integrate TIBCO Enterprise Message Service](p_jms_integrate_tibco.md)
-   [Integrate RabbitMQ Messaging](p_jms_integrate_rabbitmq.md)
-   [Integrate Informatica Ultra Messaging](p_jms_integrate_informatica.md)
-   [Integrate IBM WebSphere MQ](p_jms_integrate_ibm.md)
-   [Integrate JBoss Messaging](p_jms_integrate_jboss.md)
-   [Integrate Open MQ Messaging](p_jms_integrate_openmq.md)
-   [Integrate Oracle WebLogic JMS](p_jms_integrate_weblogic.md)
-   [Integrate Apache ActiveMQ](p_jms_integrate_activemq.md)

To Integrate Any JMS-Compliant Message Broker
---------------------------------------------

To use the `jms` service, you must perform the following steps:

1.  Copy the JMS-compliant message broker's client-specific JAR files to `GATEWAY_HOME/lib`.
2.  Configure a `jms` service on the Gateway with the properties and values for the JNDI initial context factory. The Gateway creates an initial context with a set of properties, which are specific to the JMS-compliant message broker. These properties may vary based on the configuration of your message broker, and may involve references to LDAP systems. These properties map to the `env.*` properties listed in [jms](../admin-reference/r_conf_jms.md#jms); when you use these properties, the Gateway uses their values to create the initial context.
3.  Configure the connection factory name to connect to your JMS-compliant message broker. The Gateway creates the initial context, then uses it to obtain a connection factory by looking up the name you enter here. The name you enter here should be one of the classes included in the broker JAR file that you placed into the `/lib` directory in Step 1.
4.  Configure the *context lookup topic format* and *context lookup queue format* to inform `jms` service what format to use when looking up topics and queues in the broker. The Gateway follows this format when it attempts to fetch the reference to a topic or queue from the initial context.

    **Note**: For brokers that do not use destination lookups through JNDI (such as Informatica Ultra Messaging), you can add another property to the jms section in the `gateway-config.xml` file called `destination.strategy`, and set its value to `session`. This configuration instructs the Gateway to obtain `Topic` and `Queue` objects using the `createTopic()` and `createQueue()` JMS API methods instead of performing JNDI lookups.

5.  Once you have configured the `jms` service for the Gateway with the appropriate initial context properties and the connection factory name, save the `gateway-config.xml` and restart the Gateway. The Gateway then creates the connection using the connection factory.

    **Note**: Additional `env.java.naming` properties can be specified. Refer to the field summary in `http://java.sun.com/j2se/1.3/docs/api/javax/naming/Context.html` for a complete list. The Gateway does not enforce any of these property values; it just passes them to the JMS API.

The following is an example of a `service` element of type `jms`.

``` xml
<service>
  <accept>ws://localhost:8000/jms</accept>
  <accept>wss://localhost:9000/jms</accept>

  <type>jms</type>

  <properties>
    <connection.factory.name>ConnectionFactory</connection.factory.name>

    <env.java.naming.factory.initial>
      org.apache.activemq.jndi.ActiveMQInitialContextFactory
    </env.java.naming.factory.initial>
    <env.java.naming.provider.url>
      tcp://localhost:61616
    </env.java.naming.provider.url>
    <destination.strategy>session</destination.strategy>
  </properties>

  <cross-site-constraint>
    <allow-origin>http://localhost:8000</allow-origin>
  </cross-site-constraint>
  <cross-site-constraint>
    <allow-origin>https://localhost:9000</allow-origin>
  </cross-site-constraint>
</service>
```

Notes
-----

-   If you use a JMS-compliant message broker that does not support individual message acknowledgement, you must ensure your client applications acknowledge each message received from a queue or durable subscriber. Otherwise, the client will receive only one message from that queue or durable subscriber.
-   To learn how to secure your JMS configuration, see [Secure Your JMS Configuration](../security/o_jms_secure.md).
-   Client applications (using the KAAZING Gateway JMS client library) can lookup/create their `Topic` and `Queue` objects using either the `session.createTopic()` method (for example, `Topic myTopic = session.createTopic("/topic/myTopic");`) or by performing a JNDI lookup using the `initialContext.lookup()` method (for example, `Topic myTopic = initialContext.lookup("/queue/myQueue");`). The `initialContext.lookup()` method is available to use after creating the JNDI `InitialContext` object when creating the JMS connection.

See Also
--------

-   For information on troubleshooting JMS integration, see [Troubleshoot JMS Integration](../integration-jms/p_jms_integrate_tshoot.md).
-   For information on troubleshooting JMS clients, see [Troubleshoot Your Clients](../troubleshooting/p_dev_troubleshoot.md).
-   For general troubleshooting information, see [Troubleshoot KAAZING Gateway](../troubleshooting/o_troubleshoot.md).


