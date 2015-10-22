Configure the Gateway to Use Encrypted Credentials
=====================================================

In this procedure, you will learn how to configure the Gateway to connect to the back-end JMS-compliant message broker using encrypted credentials.

Before You Begin
----------------

This procedure is part of [Checklist: Secure Your JMS Configuration](o_jms_secure.md), that includes the following steps:

1.  [Secure the Connection from the Gateway to the Message Broker](p_broker_jms_secure.md)
2.  [Secure the Connection from Each Client to the Gateway](p_client_jms_secure.md)
3.  [Configure Auditing Messages Produced by Clients](p_auditing_jms_secure.md)
4.  **Configure the Gateway to Use Encrypted Credentials**

To Configure the Gateway to Use Encrypted Credentials
--------------------------------------------------------

You can configure the Gateway to connect to the back-end JMS-compliant message broker using encrypted credentials. You can store this information in the `gateway-config.xml` as optional properties for the `jms.proxy` and `jms` services. To encrypt credentials for the Gateway to use to the back-end JMS-compliant message broker, you must first create the encrypted credentials, then configure the Gateway to use the encrypted credentials, as described in the following sections:

-   [Creating Encrypted Credentials](#creating-encrypted-credentials)
-   [Configuring the Gateway to Use Encrypted Credentials](#configuring-the-gateway-to-use-encrypted-credentials)

#### Creating Encrypted Credentials

The Gateway includes a tool you can use to generate the encrypted credentials using a 128-bit key and a password that you must provide. To create the key, perform the following steps:

1.  Open a command prompt.
2.  In your `GATEWAY_HOME`, navigate to the `bin` directory, then `tools`.
3.  Type the following command to use the `encrypt-credentials.jar`, the key you wish to use , and a sample password, which you should replace with the desired password:

    `    java -jar encrypt-credentials.jar <128-bit key> <my_password>`

    The following is an example of this command:

    `     java -jar encrypt-credentials.jar 0123456789012345 secretsauce`

**Note**  In this case, the resulting string is base64-encoded, and has been encrypted using an AES algorithm along with a multi-byte random salt argument and iterated multiple times. You can alternatively use a 256-bit key to generate the credentials. The Gateway can decrypt credentials generated from both types of keys. For more information about using these keys, see the [Java documentation](http://docs.oracle.com/javase/8/docs/technotes/guides/security/crypto/CryptoSpec.html).

4.  After you press **Enter**, the tool generates the credentials. Copy the generated credentials, as you must then add it to your `gateway-config.xml`, as described in the next section.

#### Configuring the Gateway to Use Encrypted Credentials

Once you generate the encryption credentials, you must configure the Gateway to use the credentials to access your back-end JMS-compliant message broker. To do so, perform the following steps:

1.  In a text editor, open `GATEWAY_HOME/conf/gateway-config.xml` and locate the `jms` service section.
2.  To use your newly generated encrypted credentials to access the back-end JMS-compliant message broker, you must set two properties on this service, one to point to the key you used to generate the credentials (`connection.security.credentials.key`), and one to point to the credentials you copied (`connection.security.credentials`). The following example shows the `jms` service with these two properties shown in lines 7-12:

    ``` xml
    <service>
      <accept>ws://localhost:8000/jms</accept>
      <accept>wss://localhost:9000/jms</accept>
      <type>jms</type>

      <properties>
        <connection.factory.name>ConnectionFactory</connection.factory.name>
        <connection.security.principal>guest</connection.security.principal>
        <connection.security.credentials.key>
          0123456789012345
        </connection.security.credentials.key>
        <connection.security.credentials>generated_credentials</connection.security.credentials>
        <context.lookup.topic.format>dynamicTopics/%s</context.lookup.topic.format>
        <context.lookup.queue.format>dynamicQueues/%s</context.lookup.queue.format>
        <env.java.naming.factory.initial>org.apache.activemq.jndi.ActiveMQInitialContextFactory
        </env.java.naming.factory.initial>
        <env.java.naming.provider.url>
          tcp://localhost:61616
        </env.java.naming.provider.url>
      </properties>
      ...
    </service>
    ```

    **Note**: You must set *both* properties to take advantage of using encrypted credentials to access the back-end JMS-compliant message broker. If you do not set the `connection.security.credentials.key` property, the Gateway considers the value of `connection.security.credentials` as a clear-text password.

3.  Save the `gateway-config.xml`.

Refer to [jms](https://github.com/kaazing/gateway/tree/develop/doc/admin-reference/r_conf_jms.md#jms) and [jms.proxy](https://github.com/kaazing/gateway/tree/develop/doc/admin-reference/r_conf_jms.md#jmsproxy) for more information about the configuration parameters.

Next Steps
----------

You have finished securing your JMS configuration. Learn more about securing KAAZING Gateway [Configure Authentication and Authorization](https://github.com/kaazing/gateway/blob/develop/doc/security/o_auth_configure.md).
