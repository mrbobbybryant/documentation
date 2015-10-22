Secure the Connection from Each Client to the Gateway
========================================================

In this procedure, you will learn how to configure JMS-specific authentication and authorization for JMS clients.

Before You Begin
----------------

This procedure is part of [Checklist: Secure Your JMS Configuration](o_jms_secure.md), that includes the following steps:

1.  [Secure the Connection from the Gateway to the Message Broker](p_broker_jms_secure.md)
2.  **Secure the Connection from Each Client to the Gateway**
3.  [Configure Auditing Messages Produced by Clients](p_auditing_jms_secure.md)
4.  [Configure the Gateway to Use Encrypted Credentials](p_encrypted_jms_secure.md)

To Secure the Connection from Each Client to the Gateway
-----------------------------------------------------------

There are two parts to securing connections from the client to the Gateway:

-   Authentication
-   Authorization

#### **Authentication**

The JMS client may choose to authenticate at the JMS layer, which is independent of any authentication performed at the WebSocket layer. To validate authenticated users at the JMS layer, you must specify the `authentication.realm` property for the service. Refer to [jms](https://github.com/kaazing/gateway/tree/develop/doc/admin-reference/r_conf_jms.md#jms) and [jms.proxy](https://github.com/kaazing/gateway/tree/develop/doc/admin-reference/r_conf_jms.md#jmsproxy) for more information about the configuration parameters.

#### **Authorization**

The shared broker connection has authenticated to the broker with elevated privileges so that it can perform actions from any client. However, not all clients necessarily have the same privileges. Therefore, you can use the Gateway's pluggable Java authorization API to let the Gateway enforce limited privileges for each client rather than letting each client adopt the full privileges of the shared broker connection. This Java authorization API allows you to integrate with your specific entitlement system.

The following example shows how you leverage the pluggable authorization.

``` java
package com.kaazing.gateway.jms.server.spi.demo.security;

import javax.security.auth.Subject;
import com.kaazing.gateway.jms.server.spi.security.JmsAuthorization;
import com.kaazing.gateway.jms.server.spi.security.JmsAuthorizationFactory;
import com.kaazing.gateway.jms.server.spi.JmsTopic;

public class TopicsOnlyAuthorizationFactory extends JmsAuthorizationFactory {
    // Factory class method
    public JmsAuthorization newAuthorization(Subject subject) {
        return new TopicsOnlyAuthorization();
    }
}

// Subclass that allows clients to subscribe to topics
class TopicsOnlyAuthorization extends JmsAuthorization {
    public boolean canSubscribeToTopic (JmsTopic topicKind, String topicName, String selector) {
    return true;
    }
}
```

In this example, clients can only subscribe to topics (not queues). If clients try to subscribe to a queue, the Gateway will forcibly close the connection. The default JMS Authorization behavior is to deny all behavior that requires authorization. The `TopicsOnlyAuthorization` subclass specifically allows subscriptions to topics to succeed while all other behavior is still denied. If the `authorization.factory` property is not specified in the service configuration then all authenticated clients connected to that service are implicitly fully authorized.

The following is an example of the `authorization.factory` property as specified in the service configuration.

``` xml
<authorization.factory>
  com.kaazing.gateway.jms.server.spi.demo.security.TopicsOnlyAuthorizationFactory
</authorization.factory>
```

If you are using the `kerberos5` and `gss` login-modules, you must retrieve the Kerberos principals from the Subject, which is passed to the `newAuthorization()` method of the `JmsAuthorizationFactory`. The following example shows how to use Kerberos5 with the `JmsAuthorizationFactory`.

``` java
import javax.security.auth.kerberos.KerberosPrincipal;
...
  public JmsAuthorization newAuthorization(Subject subject) {
     ....
     Set<KerberosPrincipal> krbPrincipals =
           subject.getPrincipals(KerberosPrincipal.class);
     ...
  }
...
```

See [Checklist: Configure Kerberos V5 Network Authentication](https://github.com/kaazing/gateway/tree/develop/doc/security/o_kerberos.md) for more information about the `kerberos5` and `gss` login-modules.

Refer to [jms](https://github.com/kaazing/gateway/tree/develop/doc/admin-reference/r_conf_jms.md#jms) and [jms.proxy](https://github.com/kaazing/gateway/tree/develop/doc/admin-reference/r_conf_jms.md#jmsproxy) for more information about the configuration parameters.

Next Steps
----------

[Configure Auditing Messages Produced by Clients](p_auditing_jms_secure.md)
