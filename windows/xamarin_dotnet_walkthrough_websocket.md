---
Title: Walkthrough: Deploy Microsoft .NET or Silverlight WebSocket Clients to iOS or Android Using Xamarin
Product: Gateway
Section: windows
DocType: Regular
Enterprise: True
---

In this walkthrough, you will learn how to deploy an existing Microsoft .NET WebSocket web client as a Xamarin app for iOS and Android. This procedure demonstrates how you can build a Microsoft .NET WebSocket client using the KAAZING Gateway Microsoft .NET WebSocket client library and then use Xamarin to deploy the client as an app on iOS or Android devices.

This topic walks you through the following subjects:

-  [What You Will Accomplish](#what-you-will-accomplish)
-  [Before You Begin](#before-you-begin)
-  [Install the Gateway and Xamarin 3](#install-the-gateway-and-xamarin-3)
-  [Configure Xamarin](#configure-xamarin)
-  [Reusing UIs with Xamarin.Forms](#reusing-uis-with-xamarinforms)
    -  [Classic and Unified APIs](#classic-and-unified-apis)
-  [Run the Gateway](#run-the-gateway)
-  [Build the Xamarin Apps](#build-the-xamarin-apps)
-  [Run and Test the Xamarin App in iOS Simulator or Android
    Emulator](#run-and-test-the-xamarin-app-in-ios-simulator-or-android-emulator)
    -  [Run the Xamarin App in the Android Emulator](#run-the-xamarin-app-in-the-android-emulator)
    -  [Run the Xamarin App in the iOS Simulator](#run-the-xamarin-app-in-the-ios-simulator)
    -  [Test the iOS or Android Demo](#test-the-ios-or-android-demo)
-  [WebSocket Event Handlers in Xamarin](#websocket-event-handlers-in-xamarin)

**Note:** For information on troubleshooting or displaying logs, see [Troubleshoot Your Microsoft .NET and Silverlight
Clients](p_dev_dotnet_tshoot.md) and [Display Logs for .NET and Silverlight Clients](p_clientlogging_dotnet.md).

What You Will Accomplish
------------------------

At the end of this walkthrough, an Xamarin .NET WebSocket demo created using the KAAZING Gateway Microsoft .NET and Silverlight WebSocket library runs as an app on iOS and Android, connects to the Gateway, and sends and receives Echo messages using an emulated WebSocket connection. Users can run the Xamarin app on any iOS or Android device.

This walkthrough uses the out of the box Xamarin WebSocket demo that is included with the Gateway as the example app, but the steps outlined in this walkthrough are the same for other Xamarin WebSocket client applications built with the Gateway.

Before You Begin
----------------

Before starting this walkthrough you need the following:

-   KAAZING Gateway. See [Setting Up KAAZING Gateway](../about/setup-guide.md).
-   [Xamarin Platform](http://xamarin.com/ "Mobile App Development & App Creation Software - Xamarin") 3.
-   An Xamarin account. To create an Xamarin account, see [https://store.xamarin.com/Login?from=%2faccount%2f](https://store.xamarin.com/Login?from=%2faccount%2f).
-   iOS SDK. The iOS SDK is required in order to use the iOS Simulator. For the iOS SDK, see [https://developer.apple.com/xcode/](https://developer.apple.com/xcode/). You can run the Xamarin iOS demo on iOS 5.1+, 6.0+, 7.0+, and 8.0+.
-   Android SDK. The Android SDK is required in order to use the Android Emulator. For the Android SDK, see [https://developer.android.com/sdk/index.html](https://developer.android.com/sdk/index.html).

Install the Gateway and Xamarin 3
---------------------------------

The following steps link to resources to take you through the installation of the software required for deploying an Xamarin app. If you already have this software installed, you can simply note the locations of the installed software for later use.

1.  Install the Gateway as described in [Setting Up KAAZING Gateway](../about/setup-guide.md).
2.  Download and install Xamarin 3 from [http://xamarin.com/platform](http://xamarin.com/platform "Mobile Application Development to Build Apps in C# - Xamarin").

Configure Xamarin
-----------------

The following steps configure Xamarin Studio for building the iOS and Android apps.

1.  Launch Xamarin Studio.
2.  Click the **Xamarin Studio** menu and click **Account**.
3.  Click **Log In**. Log in with your account information.
4.  Close the **Account** dialog.
5.  Add the KAAZING Gateway demo.
    1.  In Xamarin Studio, click **Open**.
    2.  Navigate to the location of the Xamarin demos in your distribution of the Gateway: *GATEWAY\_HOME*/demo/xamarin/src/gateway.
    3.  Click the file named **gateway.client.xamarin.demo.sln** and click **Open**. Xamarin Studio loads the demos for Android and iOS. The following demos are loaded:
        -   KaazingGatewayXamarinDemo.Android
        -   KaazingGatewayXamarinDemo.Common
        -   KaazingGatewayXamarinDemo.iOS.Classic
        -   KaazingGatewayXamarinDemo.iOS.Unified

**Notes:**

-   KaazingGatewayXamarinDemo.Android and KaazingGatewayXamarinDemo.iOS.Classic both share KaazingGatewayXamarinDemo.Common. The UI is defined once using Xamarin.Forms in KaazingGatewayXamarinDemo.Common and is reused by the iOS and Android projects. Similarly, the code that handles user events in KaazingGatewayXamarinDemo.Common is reused by the iOS and Android projects. This method demonstrates how to develop an app using C\# and deploy the same app to both iOS and Android.
-   The Xamarin.Forms package is not available for the Unified API. As a result, KaazingGatewayXamarinDemo.iOS.Unified defines its own UI using a storyboard and has code for dealing with user events. KaazingGatewayXamarinDemo.iOS.Unified is not sharing or reusing the code from KaazingGatewayXamarinDemo.Common like KaazingGatewayXamarinDemo.Android and KaazingGatewayXamarinDemo.iOS.Classic are. For more information, see [http://forums.xamarin.com/discussion/24616/xamarin-forms-and-ios-unified-api](http://forums.xamarin.com/discussion/24616/xamarin-forms-and-ios-unified-api "Xamarin.Forms and iOS Unified API - Xamarin Forums").
-   The KAAZING Gateway .NET client library, **Kaazing.Gateway.dll**, is picked up from *GATEWAY\_HOME*/demo/xamarin/src/gateway/KaazingLibs automatically.

Reusing UIs with Xamarin.Forms
------------------------------

[Xamarin.Forms](http://developer.xamarin.com/guides/cross-platform/xamarin-forms/introduction-to-xamarin-forms/ "An Introduction to Xamarin.Forms - Xamarin") is a cross-platform natively backed UI toolkit abstraction that allows developers to easily create user interfaces that can be shared across Android and iOS. The KAAZING Gateway Xamarin demo uses Xamarin.Forms to implement the UI once and reuse it across multiple platforms and thus avoid duplication. With Xamarin.Forms, the UI is built using C\# code instead of a storyboard for iOS and XAML for Android. The same C\# code is then reused for iOS and Android platforms.

Xamarin.Forms is used to share the UI in KaazingGatewayXamarinDemo.Common with KaazingGatewayXamarinDemo.Android and KaazingGatewayXamarinDemo.iOS.Classic, but not with KaazingGatewayXamarinDemo.iOS.Unified.

### Classic and Unified APIs

The KAAZING Gateway Xamarin includes projects that showcase how to build iOS apps under Xamarin using both Classic and Unified APIs. Xamarin recommends using the Unified API, but Xamarin.Forms works well with Classic API only, at this time. Xamarin.Forms for the Unified API is still in pre-release stage and has [issues](http://forums.xamarin.com/discussion/24616/xamarin-forms-and-ios-unified-api "Xamarin.Forms and iOS Unified API - Xamarin Forums").

While the project for KaazingGatewayXamarinDemo.iOS.Classic uses the Xamarin.Forms’ reusable UI that is shared with
KaazingGatewayXamarinDemo.Android, the KaazingGatewayXamarinDemo.iOS.Unified project does not use Xamarin.Forms. Instead, the UI for KaazingGatewayXamarinDemo.iOS.Unified is defined in a storyboard just as it is done in Xcode for a regular iOS project and has its own event-handling code written in C\#.

Run the Gateway
---------------

The following steps review the Echo service and start the Gateway.

1.  Open the *GATEWAY\_HOME*/conf/gateway-config.xml file in a text editor. Review the **echo** service to see the configuration of the connection to the Gateway. For example:

    ```
    <service>
      <name>echo</name>
      <description>Simple echo service</description>
      <accept>ws://${gateway.hostname}:${gateway.extras.port}/echo</accept>

      <type>echo</type>

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

    **Note:** By default, the Gateway uses [Property defaults](../admin-reference/c_configure_gateway_concepts.md#about-kaazing-gateway-configuration-file-elements-and-properties) for `ws://${gateway.hostname}:${gateway.extras.port}/echo` and the actual URI is `ws://localhost:8001/echo`. The Android Emulator or device will not allow the use of localhost. For Android, change the value of the **accept** tag to the local IP address of the Gateway server.

2.  Start the Gateway as described in ${setting.up.inline}. Invoke the `gateway.start` command by navigating to the *GATEWAY\_HOME*/bin directory where you installed the Gateway, and then enter the following to run the `gateway.start` script: `./gateway.start (gateway.start.bat in Windows)`

Build the Xamarin Apps
----------------------

The following steps build the Xamarin apps for iOS and Android using the Xamarin project you created.

1.  For Android, right-click the Android demo named **KaazingGatewayXamarinDemo.Android**, and then click **Build *Project\_Name***, where *Project\_Name* is the name of the project.
2.  For iOS, right-click either the iOS demo named **KaazingGatewayXamarinDemo.iOS.Unified**, **KaazingGatewayXamarinDemo.Common**, or **KaazingGatewayXamarinDemo.iOS.Classic**, and then click **Build** *Project_Name*. Xamarin Studio informs you that the project is built. If you encounter KAAZING Gateway .NET library errors, ensure that you have included the library in the **Reference** folder for the demo as described in [Configure Xamarin](#configure-xamarin).

Run and Test the Xamarin App in iOS Simulator or Android Emulator
-----------------------------------------------------------------

The following steps run the Xamarin app and test the connection to the Gateway.

**Note:** The iOS SDK is required in order to use the iOS Emulator. For the iOS SDK, see [https://developer.apple.com/xcode/](https://developer.apple.com/xcode/). You can run the Xamarin iOS demo on iOS 5.1+, 6.0+, 7.0+, and 8.0+. The Android SDK is required in order to use the Android Emulator. For the
Android SDK, see [https://developer.android.com/sdk/index.html](https://developer.android.com/sdk/index.html).

### Run the Xamarin App in the iOS Simulator

For iOS, see the iOS Simulator guide here: [https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS\_Simulator\_Guide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/Introduction/Introduction.html).

1.  Run the iOS demo.
    1.  In Xamarin, click the iOS demo.
    2.  In the top left of Xamarin Studio, right-click the iOS demo, click **Run With**, and then click an iOS Simulator (for example, iPhone 6 / iOS 8.1). The iOS Simulator opens and displays the demo.

**Note:** For running the demo on a device, configure the **iOS
Deployment** setting in **Project Options**.

### Run the Xamarin App in the Android Emulator

For information on using the Android Emulator in Xamarin Studio, see the Xamarin article here:
[http://developer.xamarin.com/guides/android/getting\_started/installation/configure\_emulator/](http://developer.xamarin.com/guides/android/getting_started/installation/configure_emulator/ "Configure the Emulator - Xamarin").

1.  Start the Android virtual device you will use for testing.
    1.  Click the Android demo.
    2.  In the top left of Xamarin Studio, click **Debug** in the dropdown menu.
    3.  Click **Select Device**, then click **Virtual Devices** and select the **Android Emulator** virtual device. If the Android Emulator is not running, click **Manage Devices** and then start the emulator. Wait for the emulator to start before running the demo.
    4.  For the demo, you need to run a virtual device with **Android 4.4.2 API 19** or later. If you do not have a virtual device with this requirement (in the Android Virtual Device Manager, you will see the message **No system images installed for this target**), then you need to install **Android 4.4.2 (API 19)** using the [Android SDK Manager](http://developer.android.com/tools/help/sdk-manager.html) (in the Android SDK Manager, click the **Tools** menu, and then click **Android SDK Manager**). Here is an example virtual device setup:
        -   **Device:** Nexus 5
        -   **Target:** Android 4.4.2 - API Level 19
        -   **CPU/ABI:** ARM (armeabi-v7a)

2.  Run the Android demo. In Xamarin Studio, click the Android demo, and then click the **Play** button. The virtual device displays the demo.

### Test the iOS or Android Demo

The following step test the Xamarin app on the iOS Simulator or Android Emulator.

1.  In the demo running in the iOS Simulator or Android Emulator, click **Connect**. The demo connects to the Gateway.
     **Note:** The Android Emulator or device will not allow the use of localhost. For the Android, change the value of the accept tag in the configuration file of the Gateway to the local IP address of the Gateway server, and enter the IP address in the **Location** field of the app.
2.  Click **Send**. The message is sent and the Echo message is received.

WebSocket Event Handlers in Xamarin
-----------------------------------

You can see the WebSocket event handlers in the file **KaazingGatewayDemoController.cs** in **KaazingGatewayXamarinDemo.Common**, shown below. The code demonstrates WebSocket open, close, and message events. In addition, the code demonstrates how to use a Login Handler and Challenge Handler to perform user authentication using the Gateway.

```
using System;
using Xamarin.Forms;
using System.Collections.Generic;
using Kaazing.Security;
using System.Threading.Tasks;
using Kaazing.HTML5;
using System.Threading;
using System.Diagnostics;

namespace KaazingGatewayXamarinDemo
{
    public class KaazingGatewayDemoController
    {
    protected KaazingGatewayDemoPage _demoPage;

        bool _isConnected = false;
        string _username = null;
        string _password = null;
        AutoResetEvent _userInputCompleted;

    WebSocket _webSocket = null;

    public KaazingGatewayDemoController(KaazingGatewayDemoPage demoPage)
        {
            _demoPage = demoPage;

      _webSocket = new WebSocket();

            BasicChallengeHandler basicHandler = ChallengeHandlers.Load<BasicChallengeHandler>(typeof(BasicChallengeHandler));
            basicHandler.LoginHandler = new KaazingDemoLoginHandler(this);
            ChallengeHandlers.Default = basicHandler;

      _webSocket.OpenEvent += new OpenEventHandler(OpenHandler);
      _webSocket.CloseEvent += new CloseEventHandler(CloseHandler);
      _webSocket.MessageEvent += new MessageEventHandler(MessageHandler);

            _userInputCompleted = new AutoResetEvent(false);
        }

        public void SetUsernameAndPassword(string username, string password)
        {
            _username = username;
            _password = password;

            // Once the member variables are set, now unblock the other
            // thread so that it can create credentials using the username/password.
            _userInputCompleted.Set();
        }

    public async void ConnectOrDisconnect(object sender, EventArgs eventArgs)
        {
            try {

                if (!_isConnected) {
                    string uriInput = _demoPage.URI;
                    _demoPage.Log("CONNECTING: " + uriInput);
                    await Task.Factory.StartNew(() => {
            _webSocket.Connect(uriInput);
                    });
                } else {
          _demoPage.Log("CLOSING");
          await Task.Factory.StartNew(() => {
            _webSocket.Close ();
          });
                }

            } catch(Exception exc) {
                _demoPage.Log("CONNECTION FAILED: " + exc.Message);
            }
        }

    ///
    /// HTML5 Event Handlers
    ///
    private void OpenHandler(object sender, OpenEventArgs args)
    {
      // Enable User Interface for Connected application
      _demoPage.EnableUI(true);
      _isConnected = true;
      _demoPage.Log("CONNECTED");
    }

    private void CloseHandler(object sender, CloseEventArgs args)
    {
      _demoPage.EnableUI(false);
      _isConnected = false;
      _demoPage.Log("DISCONNECTED");
    }

    private void MessageHandler(object sender, MessageEventArgs args)
    {
      _demoPage.Log("RECIEVED MESSAGE: " + args.Data);
    }
//
        async public void SendMessage(object sender, EventArgs eventArgs)
        {
            string messageInput = _demoPage.Message;

            await Task.Factory.StartNew(() => {
        _webSocket.Send (messageInput);
            });
            _demoPage.Log("SEND: " + messageInput);
        }

        public void ClearLog(object sender, EventArgs eventArgs)
    {
      Device.BeginInvokeOnMainThread(() => {
        _demoPage.LogView = "";
      });
    }
//
        public  PasswordAuthentication AuthenticationHandler()
        {
            PasswordAuthentication credentials = null;
            try {
                _userInputCompleted.Reset();
                Device.BeginInvokeOnMainThread(() => {
                    _demoPage.Navigation.PushModalAsync(new KaazingDemoLoginPage(this));
                });
                _userInputCompleted.WaitOne();
                credentials = new PasswordAuthentication(_username, _password.ToCharArray());
            } catch(Exception ex) {
                _demoPage.Log(ex.Message);
            }
            return credentials;
        }
    }
}
```

Summary
-------

As you have seen in this walkthrough, building an iOS or Android app using the KAAZING Gateway Microsoft .NET and Silverlight WebSocket library and Xamarin is easy to accomplish. Now that you have created the Xamarin .NET WebSocket demo using the KAAZING Gateway Microsoft .NET and Silverlight WebSocket library, try building your own custom iOS or Android app using the KAAZING Gateway Microsoft .NET and Silverlight WebSocket library and Xamarin.
