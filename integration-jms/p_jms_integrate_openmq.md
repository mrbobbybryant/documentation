Integrate Open MQ Messaging  ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
===========================

In this procedure, you will learn how to integrate KAAZING Gateway and Open Message Queue (Open MQ), a community sponsored, messaging middleware product that implements and extends the Java Message Service (JMS) standard.

To Integrate Open MQ Messaging
------------------------------

1.  Download and install Open MQ following the instructions in the Open MQ documentation. The directory that contains the Open MQ installation (for example, `C:\Program Files\Sun\MessageQueue` on Windows) is referred to as `OPENMQ_HOME`.
2.  Using the Open MQ administration console, add an object store and create a connection factory named `ConnectionFactory` in it. Refer to the Open MQ documentation for more information.
3.  Copy the following JAR files from `OPENMQ_HOME/mq/lib` to `GATEWAY_HOME/lib`.
    -   imq.jar
    -   fscontext.jar

4.  Open the file `GATEWAY_HOME/conf/gateway-config.xml` in a text editor and locate the `jms` service element (search for jms).
5.  Update the `jms` service element properties section as shown in the following example (changes shown in lines 6-14). Note that the naming provider URL (the value of the `env.java.naming.provider.url` property in line 13) can be determined from the imqadmin console of OpenMQ</span>.

    ``` xml
    <service>
      <accept>ws://localhost:8001/jms</accept>
      <accept>wss://localhost:9001/jms</accept>
      <type>jms</type>

      <properties>
        <connection.factory.name>ConnectionFactory</connection.factory.name>
        <context.lookup.topic.format>%s</context.lookup.topic.format>
        <context.lookup.queue.format>%s</context.lookup.queue.format>
        <env.java.naming.factory.initial>
          com.sun.jndi.fscontext.RefFSContextFactory
        </env.java.naming.factory.initial>
        <env.java.naming.provider.url>file://tmp</env.java.naming.provider.url>
      </properties>

      <cross-site-constraint>
        <allow-origin>http://localhost:8001</allow-origin>
      </cross-site-constraint>
      <cross-site-constraint>
        <allow-origin>https://localhost:9001</allow-origin>
      </cross-site-constraint>
    </service>
    ```

    **Notes:**

    -   On UNIX or Linux, use `file:///tmp` as the value for ` env.java.naming.provider.url`.
    -   You can also connect to your Open MQ Messaging server using SSL and the `env.java.naming.provider.url` element. For more information, see [Open MQ Messaging documentation](http://mq.java.net/features.html "Open MQ: Feature MatrixAbout Open MQ — Java.net").

JMS Integration Verification
----------------------------

To verify that the JMS integration is working, perform the following steps:

1.  Start Open MQ.
2.  Start the Gateway as described in [Setting Up the Gateway](../about/setup-guide.md).
3.  In a browser, navigate to the out of the box demos at `http://localhost:8001/demo/`.
4.  Click the **JavaScript** demo.
5.  Change the value for **Location** to `ws://localhost:8001/jms`.
6.  Provide a valid user name and password for the values of **User Name** and **Password**. By default, Open MQ will have a `guest` user account with the password `guest`.
7.  Click **Connect**.

    The message "`CONNECT:  ws://localhost:8001/jms`" should appear in the log window followed by the message "`CONNECTED: undefined`"

8.  Enter a destination to an existing topic or queue (for example, `/topic/destination`) and click **Subscribe**.
9.  Click **Send**. You should see the message that you sent in the message log.

Notes
-----

-   To learn how to secure your JMS configuration, see [Secure Your JMS Configuration](../security/o_jms_secure.md).
-   Because you are using a JMS provider that does not support individual message acknowledgement, you must ensure your client applications acknowledge each message received from a queue or durable subscriber. Otherwise, the client will receive only one message from that queue or durable subscriber.

See Also
--------

-   For information on troubleshooting JMS integration, see [Troubleshoot JMS Integration](../integration-jms/p_jms_integrate_tshoot.md).
-   For information on troubleshooting JMS clients, see [Troubleshoot Your Clients](../troubleshooting/p_dev_troubleshoot.md).
-   For general troubleshooting information, see [Troubleshoot KAAZING Gateway](../troubleshooting/o_troubleshoot.md).


