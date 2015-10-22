---
Title: Build WebSocket C Clients
Product: Gateway
Section: c
DocType: Regular
---

**Note:** To use the KAAZING Gateway, a KAAZING Gateway client library, or a KAAZING Gateway demo, fork the repository from [kaazing.org](http://kaazing.org).
This checklist provides the steps necessary to build WebSocket C clients to communicate with the KAAZING Gateway or any RFC-6455 WebSocket endpoint:

| \# | Step                                                                                                                                                                                                                     | Topic or Reference                                                                        |
|:---|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------|
| 1  | Learn about the WebSocket C client.                                                                                                                                                                                      | [Overview of the WebSocket C Client Library](#overview-of-the-websocket-c-client-library) |
| 2  | Learn how to use the WebSocket C Client Library and the supported APIs. This step includes information on how to use OpenSSL to create a secure network connection with the KAAZING Gateway RFC-6455 WebSocket endpoint. | [Use the WebSocket C Client Library](#use-the-websocket-c-client-library)                 |
| 3  | Implement a challenge handler in your client to respond to authentication challenges from the the Gateway.                                                                                                               | [Secure Your C Client](#secure-your-c-client)                                             |
| 4  | Learn how to require that C clients connection to the KAAZING Gateway provide TLS/SSL digital certificates.                                                                                                              | [Require Clients to Provide Certificates to the Gateway](../security/p_tls_mutualauth.md) |

Overview of the WebSocket C Client Library
--------------------------------------------------

The WebSocket C client library provides support for the HTML5 Communication protocol, WebSocket. Using the WebSocket C client library, you can enable WebSocket in new C applications. For example, you can create an application that uses WebSocket to get streaming financial data from a back-end server. The following figure shows a high-level overview of the architecture:

![KAAZING Gateway C Client](images/f-websocket-c-overview-web.png)
**Figure: The Gateway WebSocket C Client Connection**

Taking a Look at the KAAZING Gateway WebSocket C Demo
-----------------------------------------------------

Before you start, take a look at a demo that was built with the WebSocket C client library at [kaazing.org](http://kaazing.org). To see the WebSocket C client in action, perform the following steps:

1.  Fork or download the KAAZING Gateway WebSocket C demo from [kaazing.org](http://kaazing.org).
2.  Using the demo, connect to <ws://echo.websocket.org>, send messages to the Echo service, and see an Echo reply message.

Use the WebSocket C Client Library
==================================

In this procedure, you will learn how to use the WebSocket C client library and the supported APIs:

-   Build the client using the Command Line Interface (CLI) as described in [To Use the WebSocket C Client Library](#to-use-the-websocket-c-client-library).
-   See code examples for open and closing WebSocket connections, and sending and receiving messages: [KAAZING Gateway WebSocket C Examples](#kaazing-gateway-websocket-c-examples).

Components and Tools
--------------------

Before you get started, review the components and tools used to build your KAAZING Gateway WebSocket C client, described in the following table:

| Component or Tool                                   | Description                                                                                                                                                                                                                                                                                                                                       | Location                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Live KAAZING Gateway                                | Instead of using a local KAAZING Gateway you can use the live KAAZING Gateway, hosted at [http://websocket.org](http://websocket.org).                                                                                                                                                                                                            | Point your KAAZING Gateway WebSocket C client at [ws://echo.websocket.org/](http://echo.websocket.org/).                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| KAAZING Gateway or any RFC-6455 WebSocket endpoint. | You can use the KAAZING Gateway or any RFC-6455 WebSocket endpoint that hosts an Echo service, such as [www.websocket.org](http://www.websocket.org).                                                                                                                                                                                             | The KAAZING Gateway is available at [kaazing.org](http://kaazing.org).                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| KAAZING Gateway WebSocket C Client library files    | The C header files (.h) and the shared object files (.so) that make up the WebSocket C Client library.                                                                                                                                                                                                                                            | The library is located at [kaazing.org](http://kaazing.org).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| GNU C++ compiler                                    | The GNU Compiler Collection includes front ends for C and other languages.                                                                                                                                                                                                                                                                        | [https://gcc.gnu.org/](https://gcc.gnu.org/)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| GNU Make                                            | GNU Make is a tool that controls the generation of executables and other non-source files of a program from the program's source files.                                                                                                                                                                                                           | [https://www.gnu.org/software/make/](https://www.gnu.org/software/make/)                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| OpenSSL                                             | OpenSSL is required for the C client to connect with the KAAZING Gateway or a RFC-6455 WebSocket endpoint securely over TLS/SSL. See the OpenSSL examples in [OpenSSL Connections](#openssl-authentication-and-authorization). A TLS/SSL connection ensures that intermediaries, such as older routers, do not drop unfamiliar WebSocket traffic. | OpenSSL is typically installed on most operating systems; however, if you are using a Linux distribution, you might need to install it. In most cases, you can simply update and upgrade your Linux distribution (`$ sudo apt-get update && sudo apt-get upgrade`), or install the OpenSSL development package (`$ sudo apt-get install openssl libssl-dev`). For more information, see [http://www.openssl.org/support/faq.html](http://www.openssl.org/support/faq.html) and [http://www.openssl.org/source/](http://www.openssl.org/source/). |
| Security with Challenge Handlers                    | Authenticating your C client with the KAAZING Gateway involves implementing a challenge handler to respond to authentication challenges from KAAZING Gateway. If your challenge handler is responsible for obtaining user credentials, then you will also need to implement a login handler.                                                      | For examples, see [Secure Your KAAZING Gateway C Client](p_dev_c_secure.md).                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

To Use the WebSocket C Client Library
-------------------------------------

Use the following steps to build and run a C client using the WebSocket C library against a local KAAZING Gateway, the live KAAZING Gateway and its Echo service hosted at [ws://echo.websocket.org/](ws://echo.websocket.org/), or a RFC-6455 WebSocket endpoint.

1. Setting Up Your Development Environment.

    To develop applications using the WebSocket C client libraries, you can use the live KAAZING Gateway and its Echo service hosted at [ws://echo.websocket.org/](ws://echo.websocket.org/).

    You can also choose to develop using the default Echo service configured on KAAZING Gateway (available at [kaazing.org](http://kaazing.org)). The following is an example of the default configuration element for the Echo service in the KAAZING Gateway bundle, as specified in the configuration file `GATEWAY_HOME/conf/gateway-config.xml`:

    ```
    <service>
      <name>echo</name>
      <description>Simple echo service</description>
      <accept>ws://${gateway.hostname}:${gateway.extras.port}/echo</accept>

      <type>echo</type>

      <realm-name>demo</realm-name>

      <cross-site-constraint>
        <allow-origin>http://${gateway.hostname}:${gateway.extras.port}</allow-origin>
      </cross-site-constraint>
    </service>
    ```

    In this case, the service is configured to accept WebSocket Echo requests at `ws://${gateway.hostname}:${gateway.extras.port}/echo` which translates to `ws://localhost:8001/echo`.

2. Review the common WebSocket C programming steps.

    You will build a single C application that sends Echo requests to the Gateway or any RFC-6455 WebSocket endpoint that provides an Echo service. The demo located at [kaazing.org](http://kaazing.org) shows a single C application that performs this action. Refer to the WebSocket C API documentation for the complete list of all the WebSocket command and callback functions.

    The common KAAZING Gateway WebSocket C programming steps are:

      1.  Clone or download the WebSocket C client library from [kaazing.org](http://kaazing.org).
      2.  Download the GNU C++ compiler, GNU Make.
      3.  Build the WebSocket C library.

3. Clone or download the WebSocket C client library from [kaazing.org](http://kaazing.org).
4. Build the WebSocket C project.

  1.  In your Command Line Interface (CLI), navigate to the location of your local copy of the WebSocket C client library, such as `~/Users/johnsmith/Desktop/websocket-c-develop`
  2.  Build the client library using GNU Make: `$ make install`

      The includes and library files are added to the location of your local user includes (`~/usr/local/include`) and libraries (`~/usr/local/lib`). You can choose to install the includes and library files elsewhere, but you will need to specify their location when you build the Echo demo.

5.  Build the Echo demo.

  1.  In your CLI, navigate to the location of the Echo demo that is included in the WebSocket C client library repo (**examples** folder).
  2.  Run the following command:
     `$ gcc -o echo-demo echo-demo.c -lwebsocket -lpthread -lcrypto -lssl`

      If the includes and library files were installed somewhere other than the default locations, enter the following command instead:

      `$ gcc -o echo-demo echo-demo.c -I path-to-includes -L path-to-libraries -lwebsocket -lpthread -lcrypto -lssl`

      Enter `ls` to verify that an **echo-demo** executable file was created.

6. Run the Echo demo. In the **examples** folder, run the executable file you created: `$ ./echo-demo`
  The Echo demo prompts you with the following options:

    `What's next? o - open, t - send text, b - send binary, c - close, x - quit`

    Enter `$ o`. You are prompted to enter a URL. Enter `$ ws://echo.websocket.org/` (or the location of the Echo service of your local KAAZING Gateway or RFC-6455 WebSocket endpoint) and press **ENTER**. You will see the message `connecting ...` and then `OPEN`.

    Enter `$ t` to enter message text. Enter some text, such as `Hello`.

    The message is echoed back from the KAAZING Gateway as `TEXT MESSAGE (1): Hello`.

KAAZING Gateway WebSocket C Examples
--------------------------------------------------------------------------

The following example demonstrates how to:

1.  Import the libraries.
2.  Create the WebSocket connection to the Gateway.
3.  Send and receive an Echo as text or binary.
4.  Close the connection.
5.  [OpenSSL Connections.](#openssl-authentication-and-authorization)
    1.  [Use OpenSSL Verification Functions](#use-openssl-verification-functions).
    2.  [Use a Callback Function to do Customized Verification](#use-a-callback-function-to-do-customized-verification).

### echo-demo.c

The following example demonstrates how to create a WebSocket connection, and send and receive Echo requests and responses, and close the connection.

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>
#include "websocket.h"
#include "kaazing_websocket.h"

credential_t* getcredential(const char* challenge) {
  char buf[80];
  credential_t* credential = (credential_t*)malloc(sizeof(credential_t));
  puts("Enter username:");
  scanf("%s", buf);
  credential->username = strdup(buf);
  puts("Enter password:");
  scanf("%s", buf);
  credential->password = strdup(buf);
  return credential;
}

kaazing_challenge_handler_t* setup_demo_challenge_handler() {
  return kaazing_basic_challenge_handler_new(&getcredential);
}

void* receive_frome_websocket(void* args) {
  websocket_t* ws = (websocket_t*)args;

  int cnt = 0;
  while(1) {
    websocket_message_event_t* evt = websocket_recv(ws);
    cnt++;
    printf("\033[0;34m"); //change color to blue
    if (evt->type == WEBSOCKET_MESSAGE_TEXT) {
      printf("TEXT MESSAGE (%d): %s\n", cnt, evt->data);
    }
    else if (evt->type == WEBSOCKET_MESSAGE_BINARY) {
      printf("BINARY MESSAGE (%d): %d bytes\n", cnt, evt->length);
      int i;
      for (i = 0; i < evt->length; i++) {
        printf("%x ", (unsigned char)evt->data[i]);
      }
      printf("\n");
    }else if (evt->type == WEBSOCKET_MESSAGE_CLOSED) {
      printf("CLOSED\n");
      printf("\033[0;0m"); //reset color
      //websocket_free(ws);
      break;
    } else {
      printf("UNKNOWN MESSAGE TYPE (%d): %d\n", cnt, evt->type);
    }
    printf("\033[0;0m"); //reset color
  }
  return EXIT_SUCCESS;
}


int main(void) {

  websocket_t *ws;
  kaazing_challenge_handler_t* demohandler = setup_demo_challenge_handler();
  int result;
  char buf[80];

  while (1) {

    puts("What's next? o - open, t - send text, b - send binary, c - close, x - quit");
    char ch;
    scanf("%c", &ch);

    if (ch == 'o') {
      puts("Enter url:");
      scanf("%s", buf);

      ws = websocket_new();
      websocket_set_challenge_handler(ws, demohandler);
      puts("connecting ...");
      result = websocket_connect(ws, buf, "", "");
      //websocket_connect(&ws, "ws://chao-mac.kaazing.me:8001/echo", "");

      if (result != EXIT_SUCCESS) {
        websocket_free(ws);
        puts("FAILED");

      }
      else {
        puts("OPEN");
        pthread_t receive_thread;
        pthread_create(&receive_thread, NULL, &receive_frome_websocket, (void*)ws);
      }
    }
    else if (ch == 't') {
      puts("Enter text:");
      scanf("%s", buf);
      result = websocket_send_text(ws, buf);
    }
    else if (ch == 'b') {
      unsigned char bytes[256];
      int i;
      for (i = 0; i < 256; i++)
        bytes[i] = i;
      puts("SEND BINARY (0x00 - 0xff)");
      result = websocket_send_binary(ws, (char*)bytes, 256);
    }
    else if (ch == 'c') {
      //result = websocket_close(ws);
      websocket_free(ws);
      puts("CLOSE");
    }else if (ch == 'x') {
      break;
    }
  }
  return EXIT_SUCCESS;
}
```

OpenSSL Authentication and Authorization
--------------------------------------------------------------

The C library allows you to use OpenSSL to create a secure network connection to the KAAZING Gateway or RFC-6544 WebSocket endpoint over TLS/SSL. You have two options:

-   Use OpenSSL verification functions, or
-   Use a callback function to do customized verification

### Use OpenSSL Verification Functions

The following examples use the OpenSSL functions to perform verification between the server and the client.

#### Client Verifying a Server Certificate

To verify a server’s certificate against a Certificate Authority (CA), the C client should have the CA certificate file in pem format. Next, the client calls the `websocket_ssl_set_cacert` function to set the CA certificate in the WebSocket connection before calling the `websocket_connect` function.

```
int websocket_ssl_set_cacert(websocket_t* ws, const char* cacert)
```

| Parameter or Return Value | Description                                                          |
|:--------------------------|:---------------------------------------------------------------------|
| `ws`                      | Pointer to WebSocket                                                 |
| `cacert`                  | String containing the path of the CA certificate file in pem format. |
| `0`                       | Successful verification                                              |
| Not `0`                   | Verification failure                                                 |

Example:

```
ret = websocket_ssl_set_casert(ws, “/home/user/cert/democa.pem”);
```

#### Server Verifying a Client Certificate

To have the server validate client certificates, you can call the `kws_ssl_set_clientkey` function to add the client certificate and key to the TLS/SSL connection. Both certificate and key files must in the pem format.

```
int websocket_ssl_set_clientkey(websocket_t* ws, const char* cert, const char* key)
```

| Parameter or Return Value | Description                                                             |
|:--------------------------|:------------------------------------------------------------------------|
| `ws`                      | Pointer to WebSocket                                                    |
| `cert`                    | String containing the path of the client certificate file in pem format |
| `key`                     | String containing the path of the client key file in pem format         |
| `0`                       | Successful verification                                                 |
| Not `0`                   | Verification failure                                                    |

Example:

```
ret = websocket_ssl_set_clientkey(ws, “/home/user/cert/client.pem”, “/home/user/cert/client.key”);
```

**Note:** If you select to use this method, then call the `websocket_ssl_set_cacert()` function to ensure that you are connected to the correct server. Without setting the CA certificate, the client will connect to the server without any certificate verification. This might cause security problems.

### Use a Callback Function to do Customized Verification

You can write your own verification function that uses a callback function for the TLS/SSL handshake. The function returns `1` to accept the SSL handshake and proceed, and `0` to reject the SSL handshake.

```
int websocket_ssl_set_verify_callback(websocket_t* ws, int (*verify_callback)(int, X509_STORE_CTX *))
```

| Parameter or Return Value | Description                                                                                                                                    |
|:--------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|
| `ws`                      | Pointer to WebSocket                                                                                                                           |
| `verifyfunc`              | The callback function. Please refer to [OpenSSL](https://www.openssl.org/docs/ssl/SSL_CTX_set_verify.html) documentation for more information. |
| `0`                       | Successful verification                                                                                                                        |
| Not `0`                   | Verification failure                                                                                                                           |

Example of callback function for the client to verify the server certificate:

```
static int verify_callback_pass(int preverify_ok, X509_STORE_CTX *ctx) {
    char    buf[256];
    X509   *err_cert;
    int     err, depth;
    err_cert = X509_STORE_CTX_get_current_cert(ctx);
    err = X509_STORE_CTX_get_error(ctx);
    depth = X509_STORE_CTX_get_error_depth(ctx);
    X509_NAME_oneline(X509_get_subject_name(err_cert), buf, 256);
   //printf("certificate: %s\n", buf);

    return 1;
 }

...
// in main function()
websocket_set_verify_callback(ws, &verify_callback_pass);
```

Example of callback function for server to verify the client certificate:

```
static int verify_callback_client_cert(int preverify_ok, X509_STORE_CTX *ctx)
 {
    /*
     * Retrieve the pointer to the SSL of the connection
     */
    SSL* ssl = (SSL*)X509_STORE_CTX_get_ex_data(ctx, SSL_get_ex_data_X509_STORE_CTX_idx());
    //load client certification and private key for client authentication
    int ret;
    ret = SSL_use_certificate_file(ssl, "/home/user/cert/client.crt", SSL_FILETYPE_PEM);
    ret = SSL_use_PrivateKey_file(ssl,"/home/user/cert/client.key", SSL_FILETYPE_PEM);
    return 1;
 }
```

Next Steps
==========

-   To authenticate your WebSocket C client against the KAAZING Gateway, see the information in [Secure Your KAAZING Gateway C Client](p_dev_c_secure.md)
-   If you are using the KAAZING Gateway as your RFC-6455 WebSocket endpoint, you can require that clients provide the KAAZING Gateway with certificates. For more information, see [Require Clients to Provide Certificates to KAAZING Gateway](../security/p_tls_mutualauth.md).

Secure Your C Client
====================

**Note:** This topic provides information for authenticating your WebSocket C client with the KAAZING Gateway. For information on TLS/SSL connections between WebSocket C clients and the KAAZING Gateway or other servers, see [Use the WebSocket C Client Library](p_dev_c_client_websocket.md#openssl-authentication-and-authorization).

Before you add security to your clients, follow the steps in [Secure Network Traffic with the Gateway](../security/o_tls.md "Kaazing Developer Network") and [Configure Authentication and Authorization](https://github.com/kaazing/gateway/blob/develop/doc/security/o_auth_configure.md) to set up security on KAAZING Gateway for your client. The authentication and authorization methods configured on the Gateway influence your client security implementation. In this procedure, we provide an example of the most common implementation.

To Secure Your C Client
-----------------------

This procedure contains the following topics:

-   [Add a Challenge Handler to a KAAZING Gateway C Connection](#add-a-challenge-handler-to-a-kaazing-gateway-c-connection)
-   [Creating a Basic Challenge Handler](#creating-a-basic-challenge-handler)
-   [Creating a Custom Challenge Handler](#creating-a-custom-challenge-handler)

Authenticating your client involves implementing a challenge handler to respond to authentication challenges from the KAAZING Gateway. If your challenge handler is responsible for obtaining user credentials, then you will also need to implement a login handler.

### Add a Challenge Handler to a KAAZING Gateway C Connection

A challenge handler is a constructor used in an application to respond to authentication challenges from the Gateway when the application attempts to access a protected resource. Each of the resources protected by the Gateway is configured with a different authentication scheme (for example, Basic, Application Basic, or Application Token), and your application requires a challenge handler for each of the schemes that it will encounter or a single challenge handler that will respond to all challenges. Also, you can add a dispatch challenge handler to route challenges to specific challenge handlers according to the URI of the requested resource.

For information about each authentication scheme type, see [Configure the HTTP Challenge Scheme](https://github.com/kaazing/gateway/blob/develop/doc/security/p_authentication_config_http_challenge_scheme.md).

KAAZING Gateway C client developers must add a challenge handler to the WebSocket connection object before calling the `websocket_connect()` function. The KAAZING Gateway C library provides both a Basic Challenge Handler and a Dispatch Challenge Handler. The functions for these challenge handlers are defined in the **kaazing\_websocket.h** file.

KAAZING Gateway C client developers should add the `#include “kaazing_websocket.h”` statement in code and call the `websocket_set_challenge_hander()` function to add the Challenge Handler to the WebSocket connection.

Clients with a single challenge handling strategy for all challenges can simply set a specific challenge handler as the default using `websocket_set_challenge_handler()`. The following is an example of how to implement a single challenge handler for all challenges:

``` c
int websocket_set_challenge_handler(websocket_t* ws, kaazing_challenge_handler_t* handler);
```

| Parameter or Return Value | Description                                       |
|:--------------------------|:--------------------------------------------------|
| `ws`                      | Pointer to WebSocket                              |
| `handler`                 | Pointer to the struct `kaazing_challenge_handler` |
| `0`                       | Success                                           |
| Not 0                     | Failure                                           |

The challenge handler contains two callback functions, `canhandle()` and `handle()`. The `canhandle()` function returns 1 if it can handle the challenge, and 0 if it fails to handle the challenge. The `handle()` function returns a token (string) to send to KAAZING Gateway authentication, or `handle()` returns a NULL to abort the authentication process.

The struct can also have a point to a customized handler object for KAAZING Gateway C client developers that wish to write their own functions. For example:

``` c
struct kaazing_challenge_handler_t {
    int (*canhandle_callback)(kaazing_challenge_handler_t* self, const char* challenge);
    char* (*handle_callback)(kaazing_challenge_handler_t* self, const char* challenge);
    void* parent;
};
```

### Creating a Basic Challenge Handler

The KAAZING Gateway C client libraries include default basic challenge handler function to handle the standard [HTTP Basic authentication](http://www.ietf.org/rfc/rfc2617.txt) scheme and the Application Basic scheme of the Gateway ([Configure the HTTP Challenge Scheme](https://github.com/kaazing/gateway/blob/develop/doc/security/p_authentication_config_http_challenge_scheme.md "Kaazing Developer Network")).

To create a Basic challenge handler, use `kaazing_basic_challenge_handler_new` function:

``` c
kaazing_challenge_handler_t* kaazing_basic_challenge_handler_new(credential* (*getcredential_callback)(const char* challenge);
```

| Parameter or Return Value                             | Description                                                                                                      |
|:------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| `getcredential_callback`                              | Pointer to a callback function that returns credentials when challenged, or returns NULL to abort authentication |
| Pointer to `kaazing_basic_challenge_handler_t` object | Callback to `kaazing_basic_challenge_handler_t` object                                                           |

Here is an example of the Basic challenge handler:

``` c
#include "kaazing_websocket.h"

credential_t* getcredential(const char* challenge) {
    char buf[80];
    credential_t* credential = (credential_t*)malloc(sizeof(credential_t));
    puts("Enter username:");
    scanf("%s", buf);
    credential->username = strdup(buf);
    puts("Enter password:");
    scanf("%s", buf);
    credential->password = strdup(buf);
    return credential;
}
…
int main() {
    websocket_t* ws = websocket_new();
    kaazing_challenge_handler_t* handler = kaazing_basic_challenge_handler_new(&getcredential);
    websocket_set_challenge_handler(ws, handler);
    ws.connect("ws://kaazing.gateway/echoAuth", "", "");
}
```

### Creating a Custom Challenge Handler

To build a custom challenge handler, write your callback functions for `canhandle()` and `handle()` then add it to the `kaazing_challenge_handler_t` struct. Set the struct to the WebSocket connection by calling the `websocket_set_challenge_handler()` function.

Here is a simple sample challenge handler with a retry counter:

``` c
#include "kaazing_websocket.h"

struct My_challenge_handler {
    int retry;
    kaazing_challenge_handler_t* kaazing_handler;
}

int canhandle_challenge(kaazing_challenge_handler_t* self, const char* challenge) {
    // non-zero if challenge contains "My Challenge"
    if (strncmp(challenge, "My Challenge", 12) >= 0)
        return 1;
    else
        return 0;
}

char* handle_challenge(kaazing_challenge_handler_t* self, const char* challenge) {
    char* response = NULL;
    My_challenge_handler* parent = (My_challenge_handler*)self->parent;
    if (parent) {
        if (parent->retry++ < 3) {
            return "Token sometoken";
        }
    }
    return NULL;
}
...

int main() {

    // create kaazing_challenge_handler
    kaazing_challenge_handler* kaazing = malloc(sizeof(kaazing_challenge_handler));
    kaazing->canhandle_callback = &canhandle_challenge;
    kaazing->handle_callback = &handle_challenge;

    // create my challenge handler object
    struct My_challenge_handler *myhandler = malloc(sizeof(struct My_challenge_handler));
    myhandler->retry = 0;

    // set my handler to parent of kaazing handler
    kaazing->parent = myhandler;

    // set challenge handler to websocket
    int result;
    result = websocket_t* ws = websocket_new();
    result = websocket_set_challenge_handler(ws, kaazing);
    result = websocket_connect("ws://mygateway", NULL, NULL);
    // reset retry counter
    myhandler->retry = 0;
...
}
```

Next Steps
----------

If you are using the KAAZING Gateway as your RFC-6455 WebSocket endpoint, you can require that clients provide the KAAZING Gateway with certificates. For more information, see [Require Clients to Provide Certificates to KAAZING Gateway](../security/p_tls_mutualauth.md).
