---
Title: Walkthrough: Deploy Microsoft .NET or Silverlight JMS Clients to iOS or Android Using Xamarin
Product: Gateway
Section: windows
DocType: Regular
Enterprise: True
---

In this walkthrough, you will learn how to deploy an existing Microsoft .NET JMS web client as a Xamarin app for iOS and Android. This procedure demonstrates how you can build a Microsoft .NET JMS client using the KAAZING Gateway Microsoft .NET JMS client library and then use Xamarin to deploy the client as an app on iOS or Android devices.

This topic walks you through the following subjects:

1.  [What You Will Accomplish](#what-you-will-accomplish)
2.  [Before You Begin](#before-you-begin)
3.  [Install the Gateway and Xamarin 3](#install-the-gateway-and-xamarin-3)
4.  [Configure Xamarin](#configure-xamarin)
5.  [Run the Gateway and Apache ActiveMQ](#run-the-gateway-and-apache-activemq)
6.  [Build the Xamarin Apps](#build-the-xamarin-apps)
7.  [Run and Test the Xamarin Apps in iOS Simulator or Android Emulator](#run-and-test-the-xamarin-apps-in-ios-simulator-or-android-emulator)
    1.  [Run the Xamarin App in the Android Emulator](#run-the-xamarin-app-in-the-android-emulator)
    2.  [Run the Xamarin App in the iOS Simulator](#run-the-xamarin-app-in-the-ios-simulator)
    3.  [Test the iOS or Android Demo](#test-the-ios-or-android-demo)

**Note:** For information on troubleshooting or displaying logs, see [Troubleshoot Your Microsoft .NET and Silverlight JMS Clients](p_dev_dotnet_tshoot_jms.md "Kaazing Developer Network") and [Display Logs for .NET and Silverlight JMS Clients](p_clientlogging_dotnet_jms.md).

What You Will Accomplish
------------------------

At the end of this walkthrough, an Xamarin .NET JMS demo created using the KAAZING Gateway Microsoft .NET and Silverlight JMS libraries runs as an app on iOS and Android, connects to the Apache ActiveMQ broker via the Gateway, and sends and receives JMS messages using a native or emulated WebSocket connection. Users can run the iOS or Android app on any iOS or Android device and connect via the Gateway to Apache ActiveMQ.

This walkthrough uses the out of the box Xamarin JMS demo that is included with the Gateway as the example app, but the steps outlined in this walkthrough are the same for other Xamarin JMS client applications built with the Gateway.

Before You Begin
----------------

Before starting this walkthrough you need the following:

-   KAAZING Gateway - Enterprise Edition. See [Setting Up KAAZING Gateway](../about/setup-guide.md).
-   Xamarin Platform 3.
-   An Xamarin account. To create an Xamarin account, see [https://store.xamarin.com/Login?from=%2faccount%2f](https://store.xamarin.com/Login?from=%2faccount%2f).
-   iOS SDK. The iOS SDK is required in order to use the iOS Simulator.
-   Android SDK. The Android SDK is required in order to use the Android Emulator. For the Android SDK, see [https://developer.android.com/sdk/index.html](https://developer.android.com/sdk/index.html).

**Note:** Learn about supported browsers, operating systems, and platform versions in the [Release Notes](../release-notes.html).

Install the Gateway and Xamarin 3
---------------------------------

The following steps link to resources to take you through the installation of the software required for deploying Xamarin apps. If you already have this software installed, you can simply note the locations of the installed software for later use.

1.  Install the Gateway as described in [Setting Up KAAZING Gateway](../about/setup-guide.md).
2.  Download and install Xamarin 3 from <http://xamarin.com/platform>.

Configure Xamarin
-----------------

The following steps configure Xamarin Studio for building the iOS and Android apps.

1.  Launch Xamarin Studio.
2.  Click the **Xamarin Studio** menu and click **Account**.
3.  Click **Log In**. Log in with your account information.
4.  Close the **Account** dialog.
5.  Add the the Gateway demo.

    1.  In Xamarin Studio, click **Open**.
    2.  Navigate to the location of the Xamarin demos in your distribution of the Gateway: `GATEWAY_HOME/demo/xamarin/jms`.
    3.  Click the file named **jms.client.xamarin.demo.sln** and click **Open**. Xamarin Studio loads the demos for Android and iOS. The following demos are loaded:

        -   KaazingJMSXamarinDemo.Android
        -   KaazingJMSXamarinDemo.iOS.Unified
        -   KaazingJMSXamarinDemo.Common
        -   KaazingJMSXamarinDemo.iOS.Classic

6.  Add the KAAZING Gateway .NET client libraries to the demos:

    1.  Expand the demo you want to build and run.
    2.  Right-click **References** and click **Edit References**.
    3.  In the **Edit References** dialog, click the **.Net Assembly** tab.
    4.  Navigate to the location of the KAAZING Gateway .NET client libraries: `GATEWAY_HOME/lib/client/dotnet/Release`.
    5.  Click both **Kaazing.Gateway.dll** and **Kaazing.JMS.dll**, and then click **Add**.
    6.  Click **OK**.

**Notes:**
-   `KaazingJMSXamarinDemo.Android and KaazingJMSXamarinDemo.iOS.Classic` both share `KaazingJMSXamarinDemo.Common`. The UI is defined once using Xamarin.Forms in `KaazingJMSXamarinDemo.Common` and is reused by the iOS and Android projects. Similarly, the code that handles user events in `KaazingJMSXamarinDemo.Common` is reused by the iOS and Android projects. This method demonstrates how to develop an app using C\# and deploy the same app to both iOS and Android.
-   The Xamarin.Forms package is not available for the Unified API. As a result, `KaazingJMSXamarinDemo.iOS.Unified` defines its own UI using a storyboard and has code for dealing with user events. `KaazingJMSXamarinDemo.iOS.Unified` is not sharing or reusing the code from `KaazingJMSXamarinDemo.Common` like `KaazingJMSXamarinDemo.Android` and `KaazingJMSXamarinDemo.iOS.Classic` are. For more information, see [http://forums.xamarin.com/discussion/24616/xamarin-forms-and-ios-unified-api](http://forums.xamarin.com/discussion/24616/xamarin-forms-and-ios-unified-api).
-   The KAAZING Gateway .NET client libraries, **Kaazing.Gateway.dll** and **Kaazing.JMS.dll**, are picked up from `GATEWAY_HOME/demo/xamarin/src/jms/KaazingLibs` automatically.

Run the Gateway and Apache ActiveMQ
-----------------------------------

The following steps start the Apache ActiveMQ service that is included with the Gateway and then run the Gateway.

1.  Start Apache ActiveMQ. For steps on starting Apache ActiveMQ, see the [Setting Up KAAZING Gateway](../about/setup-guide.md).
2.  Open the `GATEWAY_HOME/conf/gateway-config.xml` file in a text editor. Review the [jms](admin-reference/r_conf_jms.md) service to see the configuration of the connection to the Apache ActiveMQ. For example.

    ``` xml
    <service>
        <accept>ws://${gateway.hostname}:${gateway.extras.port}/jms</accept>
        <type>jms</type>
        <properties>
            <connection.factory.name>ConnectionFactory</connection.factory.name>
            <context.lookup.topic.format>dynamicTopics/%s</context.lookup.topic.format>
            <context.lookup.queue.format>dynamicQueues/%s</context.lookup.queue.format>
            <env.java.naming.factory.initial>
                org.apache.activemq.jndi.ActiveMQInitialContextFactory
            </env.java.naming.factory.initial>
            <env.java.naming.provider.url>tcp://localhost:61616</env.java.naming.provider.url>
        </properties>

        <realm-name>demo</realm-name>
        <!--
        <authorization-constraint>
            <require-role>AUTHORIZED</require-role>
        </authorization-constraint>
        -->
        <cross-site-constraint>
            <allow-origin>http://${gateway.hostname}:${gateway.extras.port}</allow-origin>
        </cross-site-constraint>
    </service>
    ```

    **Note:** The Android Emulator or device will not allow the use of `localhost`. For the Android, change the value of the `accept` tag to the local IP address of the Gateway.

3.  Start the Gateway as described in [Setting Up KAAZING Gateway](../about/setup-guide.md). Invoke the `gateway.start` command by navigating to the `GATEWAY_HOME/bin` directory where you installed the Gateway, and then enter the following to run the `gateway.start` script:

     `./gateway.start`

Build the Xamarin Apps
----------------------

The following steps build the Xamarin apps for iOS and Android using the Xamarin project you created.

1.  For Android, right-click the Android demo named **KaazingJMSXamarinDemo.Android**, and then click **Build *Project\_Name***, where ***Project\_Name*** is the name of the project.
2.  For iOS, right-click either the iOS demo named **KaazingJMSXamarinDemo.iOS.Unified**, **KaazingJMSXamarinDemo.Common**, or **KaazingJMSXamarinDemo.iOS.Classic**, and then click **Build *Project\_Name***.

Xamarin Studio informs you that the project is built. If you encounter KAAZING Gateway .NET library errors, ensure that you have included the libraries in the **Reference** folder for the demo as described in [Configure Xamarin](#configure-xamarin).

Run and Test the Xamarin Apps in iOS Simulator or Android Emulator
------------------------------------------------------------------

The following steps run the Xamarin app and test the connection to the Gateway and Apache ActiveMQ.

**Note:** The iOS SDK is required in order to use the iOS Simulator. For the iOS SDK, see <https://developer.apple.com/xcode/>. The Android SDK is required in order to use the Android Emulator. For the Android SDK, see <https://developer.android.com/sdk/index.html>. Learn about supported browsers, operating systems, and platform versions in the [Release Notes](../release-notes.html).

### Run the Xamarin App in the Android Emulator

For information on using the Android Emulator in Xamarin Studio, see the Xamarin article here: [http://developer.xamarin.com/guides/android/getting_started/installation/configure_emulator/](http://developer.xamarin.com/guides/android/getting_started/installation/configure_emulator/).

1.  Start the Android virtual device you will use for testing.

    1.  Click the Android demo.
    2.  In the top left of Xamarin Studio, click **Debug** in the dropdown menu.
    3.  Click **Select Device**, then click **Virtual Devices** and select the Android Emulator virtual device. If the Android Emulator is not running, click **Manage Devices** and then start the emulator. Wait for the emulator to start before running the demo.

        For the demo, you need to run a virtual device with Android 4.4.2 API 19 or later. In you do not have a virtual device with this requirement (in the Android Virtual Device Manager, you will see the message No system images installed for this target), then you need to install Android 4.4.2 (API 19) using the Android SDK Manager (in the Android SDK Manager, click the Tools menu, and then click Android SDK Manager). Here is an example virtual device setup:

        -   **Device:** Nexus 5
        -   **Target:** Android 4.4.2 - API Level 19
        -   **CPU/ABI:** ARM (armeabi-v7a)

2.  Run the Android demo. In Xamarin Studio, click the Android demo, and then click the **Play** button. The virtual device displays the demo.

### Run the Xamarin App in the iOS Simulator

For iOS, see the iOS Simulator guide here: [https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/Introduction/Introduction.html).

1.  Run the iOS demo. In Xamarin Studio, right-click the iOS demo, click **Run With**, and then click an iOS Simulator (for example, **iPhone 6 / iOS 8.1**). The iOS Simulator opens and displays the demo.

**Note:** For running the demo on a device, configure the **iOS Deployment** setting in **Project Options**.

### Test the iOS or Android Demo

The following step test the Xamarin app on the iOS Simulator or Android Emulator.

1.  In the demo running in the iOS Simulator or Android Emulator, click **Connect**. The demo connects to the Apache ActiveMQ via the Gateway.

    **Note:** The Android Emulator or device will not allow the use of `localhost`. For the Android, change the value of the `accept` tag in the configuration file of the Gateway to the local IP address of the Gateway, and enter the IP address in the **Location** field of the app.

2.  Click **Subscribe**. You are subscribed to the topic destination.
3.  Click **Send**. The message is sent to the Apache ActiveMQ and the subscription is updated with the received message.

Next Step
---------

[Troubleshoot Your Microsoft .NET and Silverlight JMS Clients](p_dev_dotnet_tshoot_jms.md)
