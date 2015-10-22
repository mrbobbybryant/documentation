---
Title: About Integrating KAAZING Gateway and JMS-Compliant Message Brokers
Product: Gateway
Section: integration-jms
DocType: Regular
Enterprise: True
---

KAAZING Gateway allows client applications to communicate directly with any JMS-compliant message broker (for example, TIBCO EMS, IBM WebSphere MQ (formerly known as MQSeries), and Informatica Ultra Messaging by way of the `jms` service. It is common for JMS implementations to be configured with existing entitlement systems that govern which users can access certain topics or queues.

The Gateway shares a small number of privileged connections to the JMS-compliant message broker while servicing a significantly higher number of independent clients. Because the Gateway's authentication is independent of the JMS-compliant message broker, it must manage fine-grained authorization for each of the connected clients. Messages may be originating from an untrusted source location and the Gateway offers the opportunity to track the originating user and application.

The following topics describe how to integrate the Gateway with a JMS-compliant message broker, and troubleshoot common problems:

-   [Integrate TIBCO Enterprise Message Service](p_jms_integrate_tibco.md)
-   [Integrate RabbitMQ Messaging](p_jms_integrate_rabbitmq.md)
-   [Integrate Informatica Ultra Messaging](p_jms_integrate_informatica.md)
-   [Integrate IBM WebSphere MQ](p_jms_integrate_ibm.md)
-   [Integrate JBoss Messaging](p_jms_integrate_jboss.md)
-   [Integrate Open MQ Messaging](p_jms_integrate_openmq.md)
-   [Integrate Oracle WebLogic JMS](p_jms_integrate_weblogic.md)
-   [Integrate Apache ActiveMQ](p_jms_integrate_activemq.md)
-   [Integrate Any JMS-Compliant Message Broker](p_jms_integrate_anybroker.md)
-   [Troubleshoot JMS Integration](p_jms_integrate_tshoot.md)

See Also
--------

-   For information on troubleshooting JMS clients, see [Troubleshoot Your Clients](../troubleshooting/p_dev_troubleshoot.md).
-   For general troubleshooting information, see [Troubleshoot KAAZING Gateway](../troubleshooting/o_troubleshoot.md).


