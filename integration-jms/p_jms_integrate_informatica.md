---
Title: Integrate Informatica Ultra Messaging
Product: Gateway
Section: integration-jms
DocType: Regular
Enterprise: True
---

In this procedure, you will learn how to integrate KAAZING Gateway and Informatica Ultra Messaging, a family of next-generation low latency messaging software products.

To Integrate Informatica Ultra Messaging
----------------------------------------

1.  Download and install the Informatica Ultra Messaging bundle by following the instructions in the Ultra Messaging documentation. The directory that contains the Informatica Ultra Messaging installation (for example, `C:\Program Files\UMQ_2.0` on Windows) is referred to as `Informatica_UM_HOME`.
2.  Locate the default directory (`*/jmsclient/bin`) created when you installed Ultra Messaging that contains the scripts for setting up environment variables, creating the `.bindings` files, and starting/stopping Ultra Messaging.
3.  Edit the `env.sh` file (`env_check.sh` in later versions) and update the path to Java to that on your machine.
4.  While still in the `env.sh` file, check the path to the license file (**\*.lic)**. You will need this path when you export the license file in a later step.
5.  Run the `config.sh` file to set up your environment variables and generate the necessary `.bindings` file. Take note of the location of the `.bindings` file, as you will need this to configure the Gateway with Ultra Messaging.
6.  Start Ultra Messaging by running `startJMS.sh`.
7.  Open a command prompt and navigate to `GATEWAY_HOME/bin`.
8.  Export the license information for Ultra Messaging. Ensure that `LBM_LICENSE_FILENAME` points to the **\*.lic** file path you noted in Step 4.
9.  In a text editor, open the `gateway-config.xml` file (located in `GATEWAY_HOME/conf`) for your KAAZING Gateway installation.
10. Modify the [`jms service environment properties`](../admin-reference/r_conf_jms.md#jms-service-environment-properties) with the following:

    ``` xml
    <properties>
        <connection.factory.name>uJMSConnectionFactory </connection.factory.name>
      <!-- Set destinations -->
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
    ```

    Replace `bindings_file_location` in the `env.java.naming.provider.url` element with the file path to the JNDI .bindings file **folder** for the connection to the broker. Do not include the .bindings file name in the file path. For example, `file:/C:/JNDI-Directory` (Windows) or `file:/Users/johndoe/Desktop/JNDI-Directory` (Mac and Linux).

    **Note**: For brokers that do not use dynamic destination lookups through JNDI (such as Ultra Messaging), you can add another property to the jms section in the `gateway-config.xml` file called `destination.strategy`, and set its value to `session`.

11. Locate the directory (`*/jmsclient/lib`) where you installed Ultra Messaging that contains the library files required for using Ultra Messaging with the Gateway.
12. Copy the contents of this directory into the Gateway's lib directory, located here: `GATEWAY_HOME/lib`.
13. Start the Gateway as described in [Setting Up the Gateway](../about/setup-guide.md).
14. In a browser, navigate to the out of the box demos at `http://localhost:8001/demo/`.
15. Click the **JavaScript** demo.
16. Change the value for **Location** to `ws://localhost:8001/jms`.
17. If you set the `destination.strategy` property to `session`, you can leave the value of all **Destination** fields as the default.

    If you did not set this property or include it in your `gateway-config.xml` file, you must set the value of the Destination fields to a topic or queue name that exists in the Ultra Messaging JNDI `.bindings` file.

18. Click **Connect**.

    The message "`CONNECT:  ws://localhost:8001/jms`" should appear in the log window followed by the message "`CONNECTED: ID:id_value.`"

19. Click **Subscribe**.
20. Click **Send**. You should see the message that you sent in the message log.

Subscription and Destination Support
--------------------------------------------------------------

KAAZING Gateway integration with Informatica Ultra Messaging has the following subscription and destination support:

-   Durable subscriptions are supported. Ensure that you use the [`anonymous.clientid.pattern`](../admin-reference/r_conf_jms.md#anonymousclientidpattern) [jms](../admin-reference/r_conf_jms.md#jms) service property. This property is required if the Informatica Ultra Messaging broker is configured to use a fixed client ID. For durable subscribers, set this property to the value of the fixed `clientID`.
-   Dynamic destinations are supported. Ensure that you configure the [`destination.strategy`](../admin-reference/r_conf_jms.md#destinationstrategy) [jms](../admin-reference/r_conf_jms.md#jms) service property with the value `session`.
-   Static destinations are supported.

Notes
-----

-   To learn how to secure your JMS configuration, see [Secure Your JMS Configuration](../security/o_jms_secure.md).
-   Because you are using a JMS provider that does not support individual message acknowledgement, you must ensure your client applications acknowledge each message received from a queue or durable subscriber. Otherwise, the client will receive only one message from that queue or durable subscriber.

See Also
--------

-   For information on troubleshooting JMS integration, see [Troubleshoot JMS Integration](../integration-jms/p_jms_integrate_tshoot.md).
-   For information on troubleshooting JMS clients, see [Troubleshoot Your Clients](../troubleshooting/p_dev_troubleshoot.md).
-   For general troubleshooting information, see [Troubleshoot KAAZING Gateway](../troubleshooting/o_troubleshoot.md).


