---
Title: Build Microsoft .NET and Silverlight WebSocket Clients
Product: Gateway
Section: windows
DocType: Regular
Enterprise: True
---

This checklist provides the steps to enable your .NET Framework or Silverlight application to communicate with KAAZING Gateway using the signed client libraries:

| \# | Step                                                                                                                                      | Topic or Reference                                                                                                                                       |
|:---|:------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1  | Learn how to use the WebSocket API provided by the KAAZING Gateway client library.                                                        | [Use the .NET and Silverlight WebSocket API](#use-the-microsoft-net-and-silverlight-websocket-api)                                                       |
| 2  | Learn how to use the EventSource API provided by the KAAZING Gateway client library.                                                      | [Use the .NET and Silverlight EventSource API](#use-the-microsoft-net-and-silverlight-eventsource-api)                                                   |
| 3  | Migrate your legacy KAAZING Gateway WebSocket or ByteSocket-based client to the WebSocket API-compliant libraries in KAAZING Gateway 5.0. | [Migrate Microsoft .NET and Silverlight Applications to KAAZING Gateway 5.0](#migrate-microsoft-net-and-silverlight-applications-to-kaazing-gateway-50x) |
| 4  | Learn how to authenticate your client with KAAZING Gateway.                                                                               | [Secure Your Microsoft .NET and Silverlight Client](#secure-your-microsoft-net-and-silverlight-client)                                                   |
| 5  | Set up logging for your client.                                                                                                           | [Display Logs for .NET and Silverlight Clients](#display-logs-for-microsoft-net-and-silverlight-clients)                                                 |
| 6  | Troubleshoot the most common issues that occur when using Microsoft .NET and Silverlight clients with KAAZING Gateway.                    | [Troubleshoot Your Microsoft .NET and Silverlight Clients](#troubleshoot-your-microsoft-net-and-silverlight-clients)                                     |

The KAAZING Gateway Microsoft .NET and Silverlight WebSocket API supports the following deployment scenarios:

- .NET 4.0 Frameworks
- .NET 4.5 (4.5.1) Frameworks, including Windows Surface RT
- Windows 8 (8.1) desktop and Surface Pro applications
- Windows Phone 8.1 Silverlight apps
- Windows Phone 8.1 native apps

**Note:** There is no support for Windows CE.

Overview of Microsoft Silverlight
---------------------------------

Microsoft Silverlight (Silverlight) is a browser plugin that enables rich Internet applications. It runs in browsers on the Microsoft Windows and Mac operating systems. Silverlight version 4.0 provides support for .NET languages and development tools.

For more information about Silverlight, visit `http://www.microsoft.com/silverlight/`.

Overview of Microsoft .NET Framework
------------------------------------

Microsoft .NET Framework (.NET) provides a common language runtime, base libraries, and development technologies to build applications for Microsoft Windows desktop, mobile and server platforms.

For more information about the .NET Framework, visit `http://www.microsoft.com/NET/`.

WebSocket and the Microsoft .NET and Silverlight Frameworks
-----------------------------------------------------------

KAAZING Gateway provides client libraries that enable you to use HTML5 Communication protocols (for example, WebSocket and Server-Sent Events) in new or existing Silverlight or .NET Framework applications. For example, you can create a .NET application that receives streaming news data through Server-Sent Events (SSE). The following figure shows a high-level overview of the architecture:

![Silverlight and .NET architecture overview](images/f-html5-.net-client2-web.jpg)

**Figure: Silverlight and .NET architecture overview**

KAAZING Gateway Has Full WebSocket API Compliance
-------------------------------------------------

Clients built using the KAAZING Gateway Microsoft .NET and Silverlight Framework libraries are fully-compliant with the WebSocket API standard ([RFC-6455](https://tools.ietf.org/html/rfc6455)). All of the standards and features of the API are supported.

The KAAZING Gateway Microsoft .NET and Silverlight library supports all platforms where Microsoft .NET Framework 4.5 and higher is supported. To use the RFC-6455 support in the KAAZING Gateway Microsoft .NET and Silverlight library requires Windows 8 and higher.

For earlier versions of Microsoft Windows, such as Microsoft Windows 7 and Microsoft Windows Server 2003, the KAAZING Gateway Microsoft .NET and Silverlight client library uses emulation to connect to the KAAZING Gateway. Emulation allow developers to build applications on earlier versions of Microsoft Windows with the same API as their Windows 8 applications.

Use the Microsoft .NET and Silverlight WebSocket API
====================================================

This section describes how you can use the WebSocket API provided by the KAAZING Gateway Microsoft .NET and Silverlight client library (Microsoft .NET and Silverlight Client API). This API allows you to take advantage of the WebSocket standard as described in the HTML5 specification and WebSocket API including the sending and receiving of string and binary messages.

The steps in this topic show you how to create a simple Microsoft .NET and Silverlight client using the WebSocket API as implemented in the KAAZING Gateway Microsoft .NET and Silverlight WebSocket library. The Microsoft .NET and Silverlight client library is fully-compliant with the WebSocket API standard and includes several enhancements.

This topic covers the following information:

1.  [Components and Tools](#components-and-tools)
2.  [Taking a Look at the Microsoft .NET and Silverlight Client Demo](#taking-a-look-at-the-microsoft-net-and-silverlight-client-demo)
3.  [Supported Data Types](#supported-data-types)
4.  [Primary WebSocket Microsoft .NET and Silverlight API Features](#primary-websocket-microsoft-net-and-silverlight-api-features)
5.  [Build the WebSocket Microsoft .NET and Silverlight Demo](#build-the-websocket-microsoft-net-and-silverlight-demo)
6.  [Demo Files](#demo-files)

The examples in this topic highlight some of the most commonly used WebSocket methods. Refer to the Microsoft .NET and Silverlight Client API for a complete description of all the available methods.

Components and Tools
--------------------

Before you get started, review the components and tools used to build the WebSocket Microsoft .NET and Silverlight client in this procedure.

| Component or Tool                         | Description                                                                                                                                                                                                                                                                                                                                                                        | Location                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| KAAZING Gateway                           | You can use a local KAAZING Gateway or the KAAZING Gateway publicly available at [www.websocket.org](http://www.websocket.org).                                                                                                                                                                                                                                                    | KAAZING Gateway is available at [kaazing.com](http://www.kaazing.com).                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Target Framework or Environment           | Microsoft .NET Framework 4.0 environments, Microsoft Windows 8 (8.1 and higher) desktop applications, Microsoft Windows Phone 8 (8.1 and higher) devices, Microsoft Windows 8 Silverlight environments - the Silverlight API can be used with all platforms supporting Microsoft .NET 4.5 (4.5.1 and higher))                                                                      | [Microsoft Download Center](https://www.microsoft.com/en-us/download/)                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Library for target deployment environment | There are separate DLLs for each deployment environment. In the KAAZING Gateway Microsoft .NET and Silverlight Client library repo on Github (see kaazing.org for a link to the repo), there are Visual Studio projects from each deployment environment. There is also a single Microsoft Visual Studio 2013 solution file to build the DLLs for for each deployment environment. | The Visual Studio project files: Kaazing.WebSocket.Net40.csproj - .NET Framework 4.0, Kaazing.WebSocket.Net45.csproj - Windows 8 (8.1) desktop and Surface Pro, Kaazing.WebSocket.Portable.csproj - Windows Surface RT (this DLL can be used on all platforms with .NET 4.5 (4.5.1), Kaazing.WebSocket.WindowsPhone.csproj - Windows Phone 8.1 native, Kaazing.WebSocket.WindowsPhoneSilverlight.csproj - Windows Phone 8.1 Silverlight application. There is no support for Windows CE. |
| Development Tool                          | Visual Studio                                                                                                                                                                                                                                                                                                                                                                      | [https://www.visualstudio.com](https://www.visualstudio.com)                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Package Installer                         | NuGet Package Manager                                                                                                                                                                                                                                                                                                                                                              | Starting with Visual Studio 2012, NuGet is included in every edition (except Team Foundation Server) by default. Updates to NuGet can be found through the Extension Manager. For Visual Studio 2010, NuGet is available through the Visual Studio Extension Manager.                                                                                                                                                                                                                    |
| NuGet Command-Line                        | NuGet Command-Line Utility                                                                                                                                                                                                                                                                                                                                                         | [https://docs.nuget.org/consume/installing-nuget](https://docs.nuget.org/consume/installing-nuget)                                                                                                                                                                                                                                                                                                                                                                                       |
| Secure Networking of TLS/SSL              | Microsoft .NET and Silverlight client are run on the desktop or mobile devices. These environments manage TLS/SSL connections, requesting TLS/SSL certificates from KAAZING Gateway or an RFC-6455 WebSocket endpoint.                                                                                                                                                             | For more information on securing network connections between KAAZING Gateway and a Microsoft .NET and Silverlight client, see Secure Your Microsoft .NET and Silverlight Client.                                                                                                                                                                                                                                                                                                         |


Taking a Look at the Microsoft .NET and Silverlight Client Demo
----------------------------------------------------------------------------------------------

Take a look at a demonstration that was built with the Microsoft .NET and Silverlight client library. To see this demo in action, perform the following steps:

1.  Go to [kaazing.org](http://kaazing.org) to fork or download the KAAZING Gateway Microsoft .NET or Silverlight WebSocket demo.
2.  Build the demo by following the steps in the repo.

The Silverlight demo shows a simple WebSocket Echo procedure.

Supported Data Types
-------------------------------------------------------

You can send a WebSocket message using one of the following data types:

-   **String** — A text WebSocket message (UTF-8).
-   **Binary (using the ByteBuffer class)** — A compact byte array representation for sending, receiving and processing binary data using WebSocket.

Primary WebSocket Microsoft .NET and Silverlight API Features
------------------------------------------------------------------------------------------------

The examples in this section will demonstrate how to open and close a WebSocket connect, send and receive message, and error handling.

### Connecting and Closing Connections

The following example demonstrates how to open and close a connection. The code includes authentication and updates the client UI with message describing the connection state.

```
public partial class HTML5DemoForm : Form
{
  private WebSocket webSocket = null;
  private WebSocketFactory factory = new WebSocketFactory();
  ...
  private void ConnectButton_Click(object sender, EventArgs e)
  {
    // Disable the connect button
    ConnectButton.Enabled = false;
    // Set up ChallengeHandler to handler Basic or Application Basic authentications
    BasicChallengeHandler basicHandler = BasicChallengeHandler.Create();
    basicHandler.LoginHandler = new LoginHandlerDemo(this);
    factory.ChallengeHandler = basicHandler;
    webSocket = factory.CreateWebSocket();
    Log("CONNECT:" + LocationText.Text);
    webSocket.OpenEvent += new OpenEventHandler(OpenHandler);
    webSocket.CloseEvent += new CloseEventHandler(CloseHandler);
    webSocket.MessageEvent += new MessageEventHandler(MessageHandler);
    webSocket.Connect(LocationText.Text);
  }
  ...
  private void DisconnectButton_Click(object sender, EventArgs e)
  {
    Log("DISCONNECT");
    webSocket.Close();
  }
  ...
```

### Sending and Receiving Messages

The following code demonstrates sending and receiving messages using all of the supported data types.

```
private void SendButton_Click(object sender, EventArgs e)
{
  if (checkBox_Binary.Checked)
  {
    ByteBuffer buf = new ByteBuffer();
    buf.PutString(MessageText.Text, Encoding.UTF8);
    buf.Flip();
    Log("SEND BINARY:" + buf.GetHexDump());
    webSocket.Send(buf);
  }
  else
  {
    Log("SEND TEXT:" + MessageText.Text);
    webSocket.Send(MessageText.Text);
  }
}
...
private void MessageHandler(object sender, MessageEventArgs args)
{
  this.BeginInvoke((InvokeDelegate)(() =>
  {
    switch(args.MessageType) {
      case EventType.BINARY:
        Log("BINARY MESSAGE: "+  BitConverter.ToString(args.Data.Array));
        break;
      case EventType.TEXT:
        Log("TEXT MESSAGE: " + Encoding.UTF8.GetString(args.Data.Array));
        break;
      case EventType.CLOSE:
        Log("CLOSED");
        break;
      }
  }));
}
```

Build the WebSocket Microsoft .NET and Silverlight Demo
-------------------------------------------------------

The following example demonstrates how to build a WebSocket Microsoft .NET demo using the KAAZING Gateway WebSocket Microsoft .NET and Silverlight library.

### To Set Up Your Development Environment

To develop applications for the .NET and Silverlight Frameworks, you must install a .NET Integrated Development Environment (IDE) such as Visual Studio.

**Note:** You can develop Silverlight and .NET Framework applications in any of the .NET programming languages. This procedure uses Microsoft Visual C\#.

### Silverlight Environment Setup

In addition to the Visual Studio IDE, Silverlight application development also requires:

-   A .NET Integrated Development Environment (IDE) such as Visual Studio
-   Microsoft Silverlight 4 SDK
-   The Silverlight 4 browser plugin

To use the KAAZING Gateway Microsoft .NET and Silverlight library, you must add a reference to the library located in the .DLL file to your project. For information on the location of the library, see [Components and Tools](#components-and-tools).

**Note:** You can develop Silverlight 4 applications in any of the .NET programming languages. This procedure uses Microsoft Visual C\#.

Silverlight applications primarily consist of Extensible Application Markup Language (XAML) files and their corresponding code-behind classes .XAML.CS (in C\#). The .XAML files typically contain the User Interface (UI) elements and the .XAML.CS files contain the page's code-behind classes.

A typical Silverlight application is deployed as a compressed .XAP file. Adding the reference to the .DLL file in your Silverlight application packages the .DLL file in your finished application. To deploy your Silverlight application .XAP file, you can host it on web server. KAAZING Gateway provides a directory service for hosting website and web applications (`GATEWAY_HOME/web/base`).

### .NET Framework Environment Setup

The .NET Framework application development also requires:

-   A .NET Integrated Development Environment (IDE), such as Visual Studio.
-   Microsoft .NET Framework 4 with Microsoft .NET Framework 4 Patch KB2468871 (download the patch from <http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=3556>).

To use the KAAZING Gateway client libraries for Microsoft .NET and Silverlight, you must add a reference to the KAAZING Gateway libraries located to your .NET project. For information on the location of the library, see [Components and Tools](#components-and-tools).

Applications based on the .NET Framework can use standard .NET Forms Controls or Windows Presentation Foundation (WPF). Note that there are slight differences in APIs for Controls, Threading and Dispatching behavior. The steps in this document illustrate event dispatching for WPF applications. WPF Applications, like Silverlight applications, consist of Extensible Application Markup Language (XAML) files and their corresponding code-behind classes .XAML.CS (in C\#).

.NET Framework applications are typically deployed as an executable .EXE file. This executable references the.DLL file and can be deployed in the same directory alongside the executable, or registered through an installation process.

### Build the WebSocket Microsoft .NET Demo

The following steps summarize the primary code elements used in the WebSocket Microsoft .NET Silverlight Demo available in the KAAZING Gateway distribution and at [kaazing.org](http://www.kaazing.org).

1. Install the KAAZING Gateway Microsoft .NET and Silverlight library:
    1.  For information on the repo location, see [Components and Tools](#components-and-tools).
2. There are three different options when adding the library to your Visual Studio project:
  - Open and build the Visual Studio project for your deployment environment. There are five options:
    - Kaazing.WebSocket.Net40.csproj - .NET Framework 4.0
    - Kaazing.WebSocket.Net45.csproj - Windows 8 (8.1) desktop and Surface Pro
    - Kaazing.WebSocket.Portable.csproj - Windows Surface RT (this DLL can be used on all platforms with .NET 4.5 (4.5.1))
    - Kaazing.WebSocket.WindowsPhone.csproj - Windows Phone 8.1 native
    - Kaazing.WebSocket.WindowsPhoneSilverlight.csproj - Windows Phone 8.1 Silverlight application

    Once built, the DLL for the environment will be located in the `bin/Debug` folder of the library:
      - bin\Debug\net4.0\Kaazing.WebSocket.dll - .NET Framework 4.0
      - bin\Debug\net4.5\Kaazing.WebSocket.dll - Windows 8 (8.1) desktop and Surface Pro
      - bin\Debug\wp81\Kaazing.WebSocket.dll - Windows Phone 8.1 Silverlight application
      - bin\Debug\wpa\Kaazing.WebSocket.dll - Windows Phone 8.1 native
      - bin\Debug\portable\Kaazing.WebSocket.dll - Windows Surface RT (this DLL can be used on all platforms with .NET 4.5 (4.5.1))

    1.  Create a new Visual Studio project or solution.
    2.  Click **PROJECT**, and then click **Add Reference**. For more information, see [How to: Add or Remove References By Using the Add Reference Dialog Box](https://msdn.microsoft.com/en-us/library/wkze6zky.aspx).
    3.  Click **Browse**.
    4.  Locate the KAAZING Gateway Microsoft .NET and Silverlight API DLL for your target deployment environment, and click **Add**.
    5.  Click **OK** to close the reference dialog.
  - Open and build the Visual Studio project for all deployment environments:
    - Open the Kaazing.WebSocket.sln in Visual Studio and built it. Once built, the DLLs for each of the deployment environments will be located in the `bin/Debug` folder of the library. Add the DLL you need to your Visual Studio project as a **Reference**, described above.
  - Install the library as a NuGet package (see [Components and Tools](#components-and-tools) for more information):
    1. Install the [NuGet Command-Line Utility](https://docs.nuget.org/consume/installing-nuget).
    2. From the command-line, navigate to the library folder containing **Kaazing.WebSocket.nuspec**.
    2. Run the command, `nuget pack Kaazing.WebSocket.nuspec`. This will create the file **Kaazing.WebSocket.nupack**. Note the location of this file.
    3. Create a new Visual Studio project or solution.
    4. Click **Tools**, and then click **NuGet Package Manager**, and then **Package Manager Settings**.
    5. Click **Package Sources**.
    6. Click the plus sign (+) to add a new source.
    7. For the **Name**, enter **KAAZING**, and for **Source**, click **...** and navigate to the folder containing the file **Kaazing.WebSocket.nupack** and click **OK**.
    8. Click **Update** and then click **OK**.
    9. Click **Tools**, and then click **NuGet Package Manager**, and then **Manage NuGet Packages for Solution**.
    10. Click **All**, then click **KAAZING**. The **Kaazing.WebSocket** library appears.
    11. Click **Install**.
    12. In **Select Project**, select your project and click **OK**. The KAAZING Gateway Microsoft .NET and Silverlight client library is added to the project.
    13. To see the library package, in **Solution Explorer**, expand the **References** section. **Kaazing.WebSocket** is listed. You can double-click the package to see it in the **Object Explorer**.
3. Create the main files for the program. For the files used to create the KAAZING Gateway Microsoft .NET and Silverlight API demo, see [Demo Files](#demo-files). The following steps demonstrate the primary programming steps involved in creating the demo or any client using the KAAZING Gateway Microsoft .NET and Silverlight API.
4. In your main file, add the required import statement.

    ```
    using Kaazing.HTML5;
    using Kaazing.Security;
    ```

4. Create a new WebSocket object, as shown in the following example.

    ```
    private WebSocket ws;
    private WebSocketFactory factory;
    factory = new WebSocketFactory();
    ```

5. Add event listeners to the WebSocket object. The following example shows how three event listeners are created and then registered with the WebSocket object. The OpenHandler listener is called when a WebSocket connection is established, the MessageHandler listener is called when messages are received, and the CloseHandler listener is called when the WebSocket connection is closed.

    ```
    public partial class HTML5DemoForm : Form
    {
      private WebSocket webSocket = null;
      private WebSocketFactory factory = new WebSocketFactory();
      ...
      private void ConnectButton_Click(object sender, EventArgs e)
      {
        // Immediately disable the connect button
        ConnectButton.Enabled = false;

        //setup ChallengeHandler to handler Basic/Application Basic authentications
        BasicChallengeHandler basicHandler = BasicChallengeHandler.Create();
        basicHandler.LoginHandler = new LoginHandlerDemo(this);

        factory.ChallengeHandler = basicHandler;
        webSocket = factory.CreateWebSocket();
        Log("CONNECT:" + LocationText.Text);

        webSocket.OpenEvent += new OpenEventHandler(OpenHandler);
        webSocket.CloseEvent += new CloseEventHandler(CloseHandler);
        webSocket.MessageEvent += new MessageEventHandler(MessageHandler);

        webSocket.Connect(LocationText.Text);
      }
      ...
      private void OpenHandler(object sender, OpenEventArgs args)
      {
        this.BeginInvoke((InvokeDelegate)(() =>
        {
          Log("CONNECTED");

          DisconnectButton.Enabled = true;
          SendButton.Enabled = true;
          }));
        }

        private void CloseHandler(object sender, CloseEventArgs args)
        {
          this.BeginInvoke((InvokeDelegate)(() =>
          {
            Log("DISCONNECTED");

            ConnectButton.Enabled = true;
            DisconnectButton.Enabled = false;
            SendButton.Enabled = false;
            }));
          }

          private void MessageHandler(object sender, MessageEventArgs args)
          {
            this.BeginInvoke((InvokeDelegate)(() =>
            {
              switch(args.MessageType) {
                case EventType.BINARY:
                Log("BINARY MESSAGE: "+  BitConverter.ToString(args.Data.Array));
                break;
                case EventType.TEXT:
                Log("TEXT MESSAGE: " + Encoding.UTF8.GetString(args.Data.Array));
                break;
                case EventType.CLOSE:
                Log("CLOSED");
                break;

              }

              }));
            }
            ...
    ```

6. Use the connect method to connect to a back-end service, as shown in the following example. Note that a WebSocket can only connect to one URI at a time.

    ```
    string defaultLocation = "ws://localhost:8001/echo";
    LocationText.Text = defaultLocation;
    ...
    webSocket.Connect(LocationText.Text);
    ```

7. While the socket is open (that is, after the OpenEvent listener is called and before the CloseEvent listener is called), you can use the Send method to send text-only messages as shown in the following example.

    ```
    private void SendButton_Click(object sender, EventArgs e)
    {
      if (checkBox_Binary.Checked)
      {
        ByteBuffer buf = new ByteBuffer();
        buf.PutString(MessageText.Text, Encoding.UTF8);
        buf.Flip();
        Log("SEND BINARY:" + buf.GetHexDump());
        webSocket.Send(buf);
      }
      else
      {
        Log("SEND TEXT:" + MessageText.Text);
        webSocket.Send(MessageText.Text);
      }
    }
    ```

8. Later, you can use the close method at any time to deliberately disconnect from the WebSocket server, as shown in the following example.

    ```
    private void DisconnectButton_Click(object sender, EventArgs e)
    {
      Log("DISCONNECT");
      webSocket.Close();
    }
    ```

Demo Files
-----------------------------------

The following C\# demo files are used to build the KAAZING Gateway Microsoft .NET WebSocket demo. The files can be found in the GitHub repo for the Microsoft .NET WebSocket demo. See [kaazing.org](http://kaazing.org) for more a link to the repo.

- [MainForm.Designer.cs](https://github.com/kaazing/enterprise.dotnet.client/blob/develop/ws/demo/WindowsDesktop/EchoDemo/MainForm.Designer.cs)
- [MainForm.cs](https://github.com/kaazing/enterprise.dotnet.client/blob/develop/ws/demo/WindowsDesktop/EchoDemo/MainForm.cs)
- [MainForm.resx](https://github.com/kaazing/enterprise.dotnet.client/blob/develop/ws/demo/WindowsDesktop/EchoDemo/MainForm.resx)
- [Program.cs](https://github.com/kaazing/enterprise.dotnet.client/blob/develop/ws/demo/WindowsDesktop/EchoDemo/Program.cs)
- Authentication-related files:
  - [LoginDemoForm.Designer.cs](https://github.com/kaazing/enterprise.dotnet.client/blob/develop/ws/demo/WindowsDesktop/EchoDemo/LoginDemoForm.Designer.cs)
  - [LoginDemoForm.cs](https://github.com/kaazing/enterprise.dotnet.client/blob/develop/ws/demo/WindowsDesktop/EchoDemo/LoginDemoForm.cs)
  - [LoginHandlerDemo.cs](https://github.com/kaazing/enterprise.dotnet.client/blob/develop/ws/demo/WindowsDesktop/EchoDemo/LoginHandlerDemo.cs)
  - [LoginHandlerDemo.cs](https://github.com/kaazing/enterprise.dotnet.client/blob/develop/ws/demo/WindowsDesktop/EchoDemo/LoginHandlerDemo.cs)
  - [LoginDemoForm.resx](https://github.com/kaazing/enterprise.dotnet.client/blob/develop/ws/demo/WindowsDesktop/EchoDemo/LoginDemoForm.resx)


Notes
-----

-   The Microsoft .NET 4.0 Framework has a maximum connection limit of two per domain, similar to the browser limitation. For any Microsoft .NET application that uses more than one WebSocket connection at a time, you must either ensure that any WebSocket connection is closed by using `WebSocket.Close()` before opening another WebSocket connection, or increase the connection limit on the application by updating the `maxconnection` attribute in the `app.config` file.
-   You can verify that Kaazing Corporation has signed the relevant .NET DLL by selecting the DLL in the File Browser, then right-clicking and opening the **Properties** dialog. On the **Digital Signatures** tab, you can view the **Name of Signer** value "Kaazing Corporation" and a timestamp of when the DLL was signed. The email address is "Not available." For more information, see [an example C program](http://msdn.microsoft.com/en-us/library/aa382384%28VS.85%29.aspx) that shows how to use the Microsoft mechanism to verify a signature (a DLL is one example of a Portable Executable, or PE, file). You can also learn more about [preventing DLL pre-loading attacks](http://support.microsoft.com/kb/2389418).

See Also
--------

[.NET and Silverlight Client API](http://developer.kaazing.com/documentation/5.0/apidoc/client/dotnet/gateway/html/N_Kaazing_HTML5.htm)

Use the Microsoft .NET and Silverlight EventSource API
======================================================

This procedure describes how you can use the EventSource API—provided by the KAAZING Gateway Silverlight or .NET client library—from your application. This API allows you to take advantage of the server-sent events standard as described in the [HTML5 specification](http://www.w3.org/html/wg/html5/#server-sent-events). For example, you can create an application that uses the KAAZING Gateway Silverlight client library to receive streaming data from a news feed or streaming financial data. The support for server-sent events is provided by the EventSource class and its supporting classes.

The following steps allow you to use the EventSource API in a Silverlight or .NET Framework application. This example highlights some of the most commonly used EventSource methods and is not meant to be an end-to-end tutorial.

To Use the EventSource API
--------------------------

1.  Add the required import statement:

    `using Kaazing.HTML5;`

    **Note:** The .NET and Silverlight client libraries are signed.

2.  Create a new `EventSource` object, as shown in the following example.

    `EventSource mySource = new EventSource();`

3.  Add event listeners to the `EventSource` object. The following example shows how three event listeners are added to the `EventSource` object. The *open* listener is called when a server-sent events connection is established, the *message* listener is called when messages are received, and the *error* listener is called when errors occur.

    ```
    mySource.OpenEvent += new OpenEventHandler((object sender,
      OpenEventArgs e) =>
      {
        // Update the UI to show that the Silverlight application
        //            is now receiving events.
      });

    mySource.MessageEvent += new MessageEventHandler((object sender,
    MessageEventArgs e) =>
      {
        string messageData = e.Data;
        string origin = e.Origin;
        string lastEvent = e.EventId;

        // Use the data and event info to update the UI.
        //            For example:
          this.Dispatcher.BeginInvoke(() =>
          {
            Message.TextBox.text = messageData;
          });
      });

    mySource.ErrorEvent += new ErrorEventHandler((object sender,
      ErrorEventArgs e) =>
        {
          // Handle errors by closing the application
          //            or attempting to reconnect.
        });
    ```

    **Note**: The KAAZING Gateway client libraries fire events on multiple threads for better performance. Therefore, events are delivered on a thread other than the UI thread. This requires you to explicitly transfer execution to the UI thread when updating the UI from dispatched events. This technique varies slightly between Silverlight and .NET Framework applications.

4.  Connect to an event source as shown in the following example. Once this method is called, the `EventSource` is activated and message can flow through at any time.

    ` mySource.Connect("http://localhost:8000/sse");`

5.  Later, you can call a method to disconnect the `EventSource` in case you want to stop listening to messages from a particular event source.

    `mySource.Disconnect();`

See Also
--------

[.NET and Silverlight Client API](http://developer.kaazing.com/documentation/5.0/apidoc/client/dotnet/gateway/html/N_Kaazing_HTML5.htm)


Migrate Microsoft .NET and Silverlight Applications to KAAZING Gateway 5.0.x
===============================================================================

This topic explains how to migrate KAAZING Gateway 4.x Microsoft .NET and Silverlight clients to KAAZING Gateway 5.0.x.

**Notes:**

  -   The KAAZING Gateway Microsoft .NET and Silverlight library supports all platforms where Microsoft .NET Framework 4.5 is supported. To use the full RFC-6455 support in the KAAZING Gateway Microsoft .NET and Silverlight library requires Windows 8 or above.

To Migrate ByteSocket Applications to KAAZING Gateway 5.0.x
--------------------------------------------------------------

There are two issues when migrating to the KAAZING Gateway 5.0.x Microsoft .NET and Silverlight library:

1. To use the KAAZING Gateway 5.0.x Microsoft .NET and Silverlight library, use the NuGet package that contains all of the DLLs needed for all of the supported deployment environments. Starting with Visual Studio 2012, NuGet is included in every edition (except Team Foundation Server) by default.
    1. Install the [NuGet Command-Line Utility](https://docs.nuget.org/consume/installing-nuget).
    2. From the command-line, navigate to the library folder containing **Kaazing.WebSocket.nuspec**.
    2. Run the command, `nuget pack Kaazing.WebSocket.nuspec`. This will create the file **Kaazing.WebSocket.nupack**. Note the location of this file.
    3. Open the Visual Studio project for your client application.
    4. Click **Tools**, and then click **NuGet Package Manager**, and then **Package Manager Settings**.
    5. Click **Package Sources**.
    6. Click the plus sign (+) to add a new source.
    7. For the **Name**, enter **KAAZING**, and for **Source**, click **...** and navigate to the folder containing the file **Kaazing.WebSocket.nupack** and click **OK**.
    8. Click **Update** and then click **OK**.
    9. Click **Tools**, and then click **NuGet Package Manager**, and then **Manage NuGet Packages for Solution**.
    10. Click **All**, then click **KAAZING**. The **Kaazing.WebSocket** library appears.
    11. Click **Install**.
    12. In **Select Project**, select your project and click **OK**. The KAAZING Gateway Microsoft .NET and Silverlight client library is added to the project.
    13. To see the library package, in **Solution Explorer**, expand the **References** section. **Kaazing.WebSocket** is listed. You can double-click the package to see it in the **Object Explorer**.

2. Change the scope of the client authentication Challenge Handler code from global to connection-level. In KAAZING Gateway 4.x, the Challenge Handler scope was set globally. For example:

```
//Set up ChallengeHandler to handle Basic/Application Basic authentications
BasicChallengeHandler basicHandler = ChallengeHandlers.Load<BasicChallengeHandler>(typeof(BasicChallengeHandler));
basicHandler.LoginHandler = new LoginHandlerDemo(this);
ChallengeHandlers.Default = basicHandler;
```

In KAAZING Gateway 5.0.x, the Challenge Handler scope is set at the connection level, using `WebSocketFactory()`. For example:

```
factory = new WebSocketFactory();
BasicChallengeHandler handler = BasicChallengeHandler.Create();
handler.LoginHandler = new LoginHandlerDemo(this);
factory.ChallengeHandler = handler;
```

See Also
--------

[.NET and Silverlight Client API](http://developer.kaazing.com/documentation/5.0/apidoc/client/dotnet/gateway/html/N_Kaazing_HTML5.htm)

Secure Your Microsoft .NET and Silverlight Client
=================================================

Before you add security to your clients, follow the steps in [Checklist: Configure Authentication and Authorization](https://github.com/kaazing/gateway/blob/develop/doc/security/o_auth_configure.md) to set up security on KAAZING Gateway for your client. The authentication and authorization methods configured on the Gateway influence your client's security implementation. For information on secure network connections between clients and the Gateway, see [Checklist: Secure Network Traffic with the Gateway](https://github.com/kaazing/gateway/blob/develop/doc/security/o_tls.md).

To Secure Your Microsoft .NET and Silverlight Client
----------------------------------------------------

This section contains the following topics:

-   [Creating a Basic Challenge Handler](#creating-a-basic-challenge-handler)
-   [Managing Log In Attempts](#managing-log-in-attempts)
-   [Negotiate and Register a Location-Specific Challenge Handler](#negotiate-and-register-a-location-specific-challenge-handler)
-   [Using Wildcards to Match Sub-domains and Paths](#using-wildcards-to-match-sub-domains-and-paths)

Authenticating your client involves implementing a challenge handler to respond to authentication challenges from the KAAZING Gateway. If your challenge handler is responsible for obtaining user credentials, then you will also need to implement a login handler. For more information, see the [.NET and Silverlight Client API](http://developer.kaazing.com/documentation/5.0/apidoc/client/dotnet/gateway/html/N_Kaazing_HTML5.htm).

### Creating a Basic Challenge Handler

A challenge handler is a constructor used in an application to respond to authentication challenges from the KAAZING Gateway when the application attempts to access a protected resource. Each of the resources protected by the KAAZING Gateway is configured with a different authentication scheme (for example, `Basic`, `Application Basic`, or `Application Token`), and your application requires a challenge handler for each of the schemes that it will encounter or a single challenge handler that will respond to all challenges. Also, you can add a dispatch challenge handler to route challenges to specific challenge handlers according to the URI of the requested resource.

For information about each authentication scheme type, see [Configure the HTTP Challenge Scheme](https://github.com/kaazing/gateway/blob/develop/doc/security/p_authentication_config_http_challenge_scheme.md).

Clients with a single challenge handling strategy for all 401 challenges can simply set a specific challenge handler as the default using `ChallengeHandlers.Default`. The following is an example of how to implement a single challenge handler for all challenges:

```

public partial class HTML5DemoForm : Form
{
  private WebSocket webSocket = null;
  private WebSocketFactory factory = new WebSocketFactory();
  ...
  BasicChallengeHandler basicHandler = BasicChallengeHandler.Create();
  basicHandler.LoginHandler = new LoginHandlerDemo(this);
  ...
  factory.ChallengeHandler = basicHandler;
  webSocket = factory.CreateWebSocket();
...
```

### Managing Log In Attempts

When it is not possible for the KAAZING Gateway client to create a challenge response, the client must return `null` to the Gateway to stop the Gateway from continuing to issue authentication challenges.

The following .NET example demonstrates how to stop the Gateway from issuing further challenges.

```
public delegate void InvokeDelegate();

private const int maxRetries = 2;    // max retry number on wrong credentials
private int retry = 0;               // number of retry

/// <summary>
/// .Net HTML5 Demo Form
/// </summary>

public PasswordAuthentication AuthenticationHandler()
{
  PasswordAuthentication credentials = null;
  if (retry++ >= maxRetries)
  {
    return null;          // stop authentication when max retries reached
  }
  AutoResetEvent userInputCompleted = new AutoResetEvent(false);
  this.BeginInvoke((InvokeDelegate)(() =>
  {
    LoginDemoForm loginForm = new LoginDemoForm();
    if (loginForm.ShowDialog() == System.Windows.Forms.DialogResult.OK)
    {
      credentials = new PasswordAuthentication(loginForm.Username, loginForm.Password.ToCharArray());
    }
    else
    {
      //user click cancel button to stop the authentication
      retry = 0;     // reset retry counter
      credentials = null;  // return null to stop authentication process
    }
    userInputCompleted.Set();
  }));
  // wait user click 'OK' or 'Cancel' on login window
  this.BeginInvoke((InvokeDelegate)(() =>
  {
    Log("CONNECTED");

    retry = 0;     // reset retry counter
    DisconnectButton.Enabled = true;
    SendButton.Enabled = true;
  }));
  {
    Log("DISCONNECTED");

    retry = 0;     // reset retry counter
    ConnectButton.Enabled = true;
    DisconnectButton.Enabled = false;
    SendButton.Enabled = false;
    userInputCompleted.WaitOne();
    return credentials;
```

### Negotiate and Register a Location-Specific Challenge Handler

Client applications with location-specific challenge handling strategies can register a `DispatchChallengeHandler` object, on which location-specific `ChallengeHandler` objects are then registered. The result is that whenever a request matching one of the specific locations encounters a 401 challenge from the server, the corresponding `ChallengeHandler` object is invoked to handle the challenge.

The following example creates a challenge handler to handle authentications and uses `DispatchChallengeHandler` to assign different challenge handler to each location:

```
DispatchChallengeHandler dispatchHandler = DispatchChallengeHandler.Create();
LoginHandler loginHandler = new LoginHandlerDemo(this);
```

Next, set a `loginHandler` for this location:

```
BasicChallengeHandler basicHandler = BasicChallengeHandler.Create();
basicHandler.LoginHandler = loginHandler;
dispatchHandler.Register("ws://myserver.com/*", basicHandler);
```

Add another challenge handler for a second location:

```
BasicChallengeHandler basicHandler2 = BasicChallengeHandler.Create();
basicHandler2.LoginHandler = loginHandler2;
dispatchHandler.Register("ws://otherserver.com/*", basicHandler2);
factory.ChallengeHandler = dispatchHandler;
```

### Using Wildcards to Match Sub-domains and Paths

You can use wildcards (“\*”) when registering locations using `DispatchChallengeHandler`. Some examples of `locationDescription` values with wildcards are:

-   `*/` matches all requests to any host on port 80 (default port), with no user information or path specified.
-   `*.hostname.com:8000` matches all requests to port 8000 on any sub-domain of hostname.com, but not hostname.com itself.
-   `server.hostname.com:*/*` matches all requests to a particular server on any port on any path but not the empty path.

See Also
--------

[.NET and Silverlight Client API](http://developer.kaazing.com/documentation/5.0/apidoc/client/dotnet/gateway/html/N_Kaazing_HTML5.htm)

Display Logs for Microsoft .NET and Silverlight Clients
=======================================================

You can configure logging to gather data on your Microsoft .NET and Silverlight clients.

To Display Logs for .NET and Silverlight Clients
------------------------------------------------

1.  Build your Kaazing .NET or Silverlight client, as described in [Checklist: Build Microsoft .NET and Silverlight WebSocket Clients](../windows/o_dev_dotnet.md).
2.  To produce WebSocket-level debug output, add the Debug version of the client library (see [Components and Tools](p_dev_dotnet_websocket.md#components-and-tools) for the location of the debug library).
3.  You can obtain additional debugging information by using a `Logger.LoggerCallback` statement and send the output to a log, as described in the following example:

    ```
    Logger.LoggerCallback = HandleLog;
         private void HandleLog(String message)
         {
              this.BeginInvoke((InvokeDelegate)(() =>
              {
                   Log("LOG: " + message);
              }));
         }
    ```

See Also
--------

[.NET and Silverlight Client API](http://developer.kaazing.com/documentation/5.0/apidoc/client/dotnet/gateway/html/N_Kaazing_HTML5.htm)

Troubleshoot Your Microsoft .NET and Silverlight Clients
========================================================

This procedure provides troubleshooting information for the most common issues that occur when using KAAZING Gateway Microsoft .NET and Silverlight clients.

To Troubleshoot Your Microsoft .NET and Silverlight Clients
-----------------------------------------------------------

The following are some things that can go wrong when using the .NET and Silverlight clients:

-   [Debug Log Produces an Error](#debug-log-produces-an-error)
-   [Delay to Initial Connection Request from the .NET Client](#delay-to-initial-connection-request-from-the-net-client)
-   [.NET client has reached connection limit](#net-client-has-reached-connection-limit)

### Debug Log Produces an Error

When debugging a Silverlight application from Visual Studio, the debug log gives you a disconnecting message or an error.

**Cause**

This message usually means that you are trying to debug a Silverlight application from Visual Studio and using the incorrect cross-site-constraint to connect to the Gateway services.

**Action**

A cross-site-constraint of \* is required to connect to the Gateway services when debugging Silverlight applications from Visual Studio using a `file:// `based URL.

### Delay to Initial Connection Request from the .NET Client

The initial WebSocket connection from the .NET client built using KAAZING Gateway client libraries for .NET and Silverlight is delayed by several seconds.

**Cause**

The delay may be caused by automatic proxy detection in Windows.

**Action**

To address this, if you are using Internet Explorer, choose **Options \> Connections tab \> LAN Settings**. Clear the Automatically Detect Settings checkbox and, if necessary, specify the proxy settings. In .NET applications, you can bypass the proxy settings by including the line`System.Net.WebRequest.DefaultWebProxy = null;` *before*the client initiates the WebSocket connection. If your application requires a proxy, change the null value in this line to the default proxy or follow [these instructions](http://msdn.microsoft.com/en-us/library/dkwyc043.aspx). For other browsers, see where they modify their LAN settings.

To get more troubleshooting information for your .NET and Silverlight clients, see [Display Logs for .NET and Silverlight Clients](../windows/p_clientlogging_dotnet.md).

### .NET client has reached connection limit

The .NET client has reached a maximum connection limit per domain of two connections.

**Cause**

The Microsoft .NET 4.0 Framework has a maximum connection limit of two per domain, similar to the browser limitation.

**Action**

For any Microsoft .NET application that uses more than one WebSocket connection at a time, you must either ensure that any WebSocket connection is closed by using `WebSocket.Close()` before opening another WebSocket connection, or increase the connection limit on the application by updating the `maxconnection` attribute in the `app.config` file.

Next Step
---------

You have completed the Microsoft .NET and Silverlight client procedures. For more information on client API development, see the [.NET and Silverlight Client API](http://developer.kaazing.com/documentation/5.0/apidoc/client/dotnet/gateway/html/N_Kaazing_HTML5.htm).
