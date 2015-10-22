Secure the Connection from the Gateway to the Message Broker
===============================================================

In this procedure, you will learn how to use service properties to pass in the user name and password for connections made to the back-end message broker.

Before You Begin
----------------

This procedure is part of [Checklist: Secure Your JMS Configuration](o_jms_secure.md), that includes the following steps:

1.  **Secure the Connection from the Gateway to the Message Broker**
2.  [Secure the Connection from Each Client to the Gateway](p_client_jms_secure.md)
3.  [Configure Auditing Messages Produced by Clients](p_auditing_jms_secure.md)
4.  [Configure the Gateway to Use Encrypted Credentials](p_encrypted_jms_secure.md)

To Secure the Connection from the Gateway to the Message Broker
------------------------------------------------------------------

Use the following service properties to securely pass in the user name and password for connections made to the back-end message broker:

-   `connection.security.principal`
-   `connection.security.credentials`
-   `env.java.naming.security.principal`
-   `env.java.naming.security.credentials`

``` xml
<service>
 .
 .
 .
  <properties>
    <connection.security.principal>
      joe
    </connection.security.principal>
    <connection.security.credentials>
      welcome
    </connection.security.credentials>
    <env.java.naming.security.principal>
      joe
    </env.java.naming.security.principal>
    <env.java.naming.security.credentials>
      welcome
    </env.java.naming.security.credentials>
  </properties>
 .
 .
 .
</service>
```

Notes
-----

Refer to [jms](https://github.com/kaazing/gateway/tree/develop/doc/admin-reference/r_conf_jms.md#jms) and [jms.proxy](https://github.com/kaazing/gateway/tree/develop/doc/admin-reference/r_conf_jms.md#jmsproxy) for more information about the configuration parameters.

Next Steps
----------

[Secure the Connection from Each Client to the Gateway](p_client_jms_secure.md)
