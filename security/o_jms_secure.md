---
Title: Secure Your JMS Configuration
Product: Gateway
Section: security
DocType: Regular
---

The following checklist provides the steps necessary to secure KAAZING Gateway integration with JMS-compliant message brokers.

| \#  | Step                                                                                                              | Topic or Reference                                                                          |
|-----|-------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| 1   | Use service properties to pass in the user name and password for connections made to the back-end message broker. | [Secure the Connection from the Gateway to the Message Broker](p_broker_jms_secure.md) |
| 2   | Configure JMS-specific authentication and authorization for JMS clients.                                          | [Secure the Connection from Each Client to the Gateway](p_client_jms_secure.md)        |
| 3   | Stamp messages flowing from each client connected to the Gateway with an ID for auditing purposes.                | [Configure Auditing Messages Produced by Clients](p_auditing_jms_secure.md)               |
| 4   | Configure the Gateway to connect to the JMS-compliant message broker using encrypted credentials.                 | [Configure the Gateway to Use Encrypted Credentials](p_encrypted_jms_secure.md)        |

<a name="overview"></a>Overview
-------------------------------

KAAZING Gateway allows client applications to communicate directly with any JMS-compliant message broker. You can secure the connection between the Gateway and the JMS-compliant message broker.

It is common for JMS implementations to be configured with existing entitlement systems that govern which users can access certain topics or queues. The Gateway provides a set of configuration properties and a pluggable Java authorization API that allows you to integrate with your specific entitlement system.

Notes
-----

Before you secure your JMS configuration, review [Configure Authentication and Authorization](https://github.com/kaazing/gateway/blob/develop/doc/security/o_auth_configure.md).
