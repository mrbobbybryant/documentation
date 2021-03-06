---
Title: Configure Authentication and Authorization
Product: Gateway
Section: Security
DocType: Regular
---

The following checklist provides the steps necessary to configure KAAZING Gateway to perform authentication and authorization:

| #        | Step                                                                                                                  | Topic or Reference                                                                                                                                                                                             |
|:---------|:----------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1        | Learn about authentication and authorization.                                                                         | [About Security with KAAZING Gateway](c_security_about.md), [About Authentication and Authorization](c_auth_about.md), and [What's Involved in Secure Communication](u_secure_client_gateway_communication.md) |
| 2        | Learn how authentication and authorization work with the Gateway.                                                     | [What Happens During Authentication](u_authentication_gateway_client_interactions.md) and [How Authentication and Authorization Work with the Gateway](u_auth_how_it_works_with_the_gateway.md)                |
| 3        | Define the method the Gateway uses to secure back-end systems and respond to security challenges.                     | [Configure the HTTP Challenge Scheme](p_authentication_config_http_challenge_scheme.md)                                                                                                                        |
| 4        | Configure one or more login modules to handle the challenge/response authentication sequence of events with clients.  | [Configure a Chain of Login Modules](p_auth_configure_login_module.md)                                                                                                                                         |
| 5        | Code your client to respond to the Gateway's authentication challenge.                                                | [Configure a Challenge Handler on the Client](p_auth_configure_challenge_handler.md)                                                                                                                           |
| 6        | Configure the Gateway to specify the user roles that are authorized to perform operations for Gateway services.       | [Configure Authorization](p_authorization_configure.md)                                                                                                                                                        |
| 7        | Configure the Gateway to authorize or deny JMS operations performed by the client, using the JMSAuthorizationFactory. | [Secure the Connection from Each Client to the Gateway](p_client_jms_secure.md)                                                                                                                                |
| Optional | Inject bytes into a custom protocol or promote user credentials to the AMQP protocol.                                 | [Implement Protocol Injection](https://github.com/kaazing/enterprise.gateway/blob/develop/doc/security/p_auth_protocol_injection.md)                                                                           |
