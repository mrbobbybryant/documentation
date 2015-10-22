Configure Auditing Messages Produced by Clients
===============================================

In this procedure, you will learn how to stamp messages flowing from each client connected to the Gateway with an ID for auditing purposes.

Before You Begin
----------------

This procedure is part of [Checklist: Secure Your JMS Configuration](o_jms_secure.md), that includes the following steps:

1.  [Secure the Connection from the Gateway to the Message Broker](p_broker_jms_secure.md)
2.  [Secure the Connection from Each Client to the Gateway](p_client_jms_secure.md)
3.  **Configure Auditing Messages Produced by Clients**
4.  [Configure the Gateway to Use Encrypted Credentials](p_encrypted_jms_secure.md)

To Configure Auditing Messages Produced by Clients
--------------------------------------------------

For auditing purposes, you can configure messages flowing from each connected client to the Gateway to be stamped with an ID for auditing purposes.

There are two types of auditing you can configure:

1.  Application auditing (applies unique IDs per application)
2.  User auditing (applies unique IDs per user)

#### Configuring Application Auditing

To configure application Auditing, you must specify the value you would like to be appended to you messages in the `application.id` property of the service in the file `gateway-config.xml` as shown in the following example:

``` xml
<service>
  .
  .
  .
    <application.id>demo_app</application.id>
  .
  .
  .
</service>
```

With the gateway configuration shown in the previous example, messages that flow from the gateway to the JMS message broker will be stamped with the application ID `demo_app` for auditing purposes.

#### Configuring User Auditing

You can use the Gateway's pluggable Java API to produce an auditable user identifier for each authenticated client. To do this, you can leverage the `JMSUserIdentityResolver` class as shown in the following example:

``` java
package com.kaazing.gateway.jms.server.spi.demo.security;

  import java.util.Set;
  import javax.security.auth.Subject;
  import javax.security.auth.kerberos.KerberosPrincipal;
  import com.kaazing.gateway.jms.server.spi.security.JmsUserIdentityResolver;

  public class KerberosUserIdentityResolver extends JmsUserIdentityResolver {

  public String resolve(Subject subject) {

    Set<KerberosPrincipal> principals = subject.getPrincipals(KerberosPrincipal.class);

      if (principals != null && !principals.isEmpty()) {
        KerberosPrincipal principal = principals.iterator().next();
        return principal.getName();
      }
      return null;
    }
  }
```

In this example, users are authenticated by validating a Kerberos ticket. After successfully validating the ticket, the Kerberos principal is attached to the subject. The `KerberosUserIdentityResolver` then resolves the name of the Kerberos principal that is attached to the subject. The Kerberos principal name acts as the auditable user identifier that is attached to each message as it flows through the gateway to the JMS message broker.

The following is an example of the `user.id.resolver` property as specified in the service configuration.

``` xml
  <user.id.resolver>
    com.kaazing.gateway.jms.server.spi.demo.security.KerberosUserIdentityResolver
  </user.id.resolver>
```

Refer to [jms](../admin-reference/r_conf_jms.md#jms) and [jms.proxy](../admin-reference/r_conf_jms.md#jmsproxy) for more information about the configuration parameters.

Next Steps
----------

[Configure the Gateway to Use Encrypted Credentials](p_encrypted_jms_secure.md)
