---
Title: Configuration Skeleton
Product: Gateway
Section: admin-reference
DocType: Regular
---

You can view and link to all Gateway configuration elements and properties using these lists:

-   [Configuration Element Index](r_configure_gateway_element_index.md) for an alphabetical listing
-   **Configuration Skeleton** (this topic) for a bare bones Gateway configuration

-   [gateway-config](r_configure_gateway_gwconfig.md)
    -   [service](r_configure_gateway_service.md)
        -   [name](r_configure_gateway_service.md#service)
        -   [description](r_configure_gateway_service.md#service)
        -   [accept](r_configure_gateway_service.md#accept)
        -   [connect](r_configure_gateway_service.md#connect)
        -   [balance](r_configure_gateway_service.md#balance)
        -   [type](r_configure_gateway_service.md#type)
            -   [balancer](r_configure_gateway_service.md#balancer)
            -   [broadcast](r_configure_gateway_service.md#broadcast)

                Property:

                -   [accept](r_configure_gateway_service.md#broadcast)
            -   [directory](r_configure_gateway_service.md#directory)

                Properties:

                -   [directory](r_configure_gateway_service.md#directory)
                -   [options](r_configure_gateway_service.md#directory)
                -   [welcome-file](r_configure_gateway_service.md#directory)
                -   [error-pages-directory](r_configure_gateway_service.md#directory)
            -   [echo](r_configure_gateway_service.md#echo)
            -   [management.jmx](r_configure_gateway_service.md#managementjmx)
            -   [management.snmp](r_configure_gateway_service.md#managementsnmp)
            -   [kerberos5.proxy](r_configure_gateway_service.md#kerberos5proxy) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [proxy](r_configure_gateway_service.md#proxy-amqpproxy-and-jmsproxy)

                Properties:

                -   [maximum.pending.bytes](r_configure_gateway_service.md#maximumpendingbytes)
                -   [maximum.recovery.interval](r_configure_gateway_service.md#maximumrecoveryinterval)
                -   [prepared.connection.count](r_configure_gateway_service.md#preparedconnectioncount)
            -   [amqp.proxy](r_configure_gateway_service.md#proxy-amqpproxy-and-jmsproxy)

                Properties:

                -   [maximum.pending.bytes](r_configure_gateway_service.md#maximumpendingbytes)
                -   [maximum.recovery.interval](r_configure_gateway_service.md#maximumrecoveryinterval)
                -   [prepared.connection.count](r_configure_gateway_service.md#preparedconnectioncount)
                -   [virtual.host](r_configure_gateway_service.md#virtualhost)

            -   [jms](r_conf_jms.md#jms) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [jms.proxy](r_conf_jms.md#jmsproxy)  ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
			-   [http.proxy](r_configure_gateway_service.md#httpproxy)

        -   [properties](r_configure_gateway_service.md#properties)
        -   [accept-options](r_configure_gateway_service.md#accept-options-and-connect-options)
            -   [*protocol*.bind](r_configure_gateway_service.md#protocolbind), where *protocol* can be ws, wss, http, https, ssl, socks, tcp, or udp
            -   [*protocol*.transport](r_configure_gateway_service.md#protocoltransport), where *protocol* can be pipe, tcp, ssl, or http
            -   [ws.maximum.message.size](r_configure_gateway_service.md#wsmaximummessagesize)
            -   [http.keepalive.timeout](r_configure_gateway_service.md#httpkeepalivetimeout)
            -   [ssl.ciphers](r_configure_gateway_service.md#sslciphers)
            -   [ssl.protocols](r_configure_gateway_service.md#sslprotocols-and-sockssslprotocols)
            -   [ssl.encryption](r_configure_gateway_service.md#sslencryption)
            -   [ssl.verify-client](r_configure_gateway_service.md#sslverify-client)
            -   [socks.mode](r_configure_gateway_service.md#socksmode) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [socks.ssl.ciphers](r_configure_gateway_service.md#sockssslciphers) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [socks.ssl.protocols](r_configure_gateway_service.md#sslprotocols-and-sockssslprotocols) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [socks.ssl.verify-client](r_configure_gateway_service.md#sslverify-client) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [socks.retry.maximum.interval](r_configure_gateway_service.md#socksretrymaximuminterval) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [tcp.maximum.outbound.rate](r_configure_gateway_service.md#tcpmaximumoutboundrate) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [ws.inactivity.timeout](r_configure_gateway_service.md#wsinactivitytimeout)
            -   [http.server.header](r_configure_gateway_service.md#httpserverheader)
        -   [connect-options](r_configure_gateway_service.md#accept-options-and-connect-options)
            -   [*protocol*.transport](r_configure_gateway_service.md#protocoltransport), where *protocol* can be pipe, tcp, ssl, or http
            -   [ssl.ciphers](r_configure_gateway_service.md#sslciphers)
            -   [ssl.protocols](r_configure_gateway_service.md#sslprotocols-and-sockssslprotocols)
            -   [ssl.encryption](r_configure_gateway_service.md#sslencryption)
            -   [socks.mode](r_configure_gateway_service.md#socksmode) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [socks.timeout](r_configure_gateway_service.md#sockstimeout) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [socks.ssl.ciphers](r_configure_gateway_service.md#sockssslciphers) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [socks.ssl.protocols](r_configure_gateway_service.md#sslprotocols-and-sockssslprotocols) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [socks.ssl.verify-client](r_configure_gateway_service.md#sockssslverify-client) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [ws.inactivity.timeout](r_configure_gateway_service.md#wsinactivitytimeout)
            -   [ws.version](r_configure_gateway_service.md#wsversion-deprecated) (deprecated)
        -   [realm-name](r_configure_gateway_service.md#realm-name)
        -   [authorization-constraint](r_configure_gateway_service.md#authorization-constraint)
            -   [require-role](r_configure_gateway_service.md#authorization-constraint)
            -   [require-valid-user](r_configure_gateway_service.md#authorization-constraint)
        -   [mime-mapping](r_configure_gateway_service.md)
            -   [extension](r_configure_gateway_service_defaults.md)
            -   [mime-type](r_configure_gateway_service_defaults.md)
        -   [cross-site-constraint](r_configure_gateway_service.md#cross-site-constraint)
            -   [allow-origin](r_configure_gateway_service.md#cross-site-constraint)
            -   [allow-methods](r_configure_gateway_service.md#cross-site-constraint)
            -   [allow-headers](r_configure_gateway_service.md#cross-site-constraint)
            -   [maximum-age](r_configure_gateway_service.md#cross-site-constraint)
    -   [service-defaults](r_configure_gateway_service_defaults.md)
        -   [accept-options](r_configure_gateway_service_defaults.md)
            -   [*protocol*.bind](r_configure_gateway_service.md#protocolbind), where *protocol* can be ws, wss, http, https, ssl, socks, tcp, or udp
            -   [*protocol*.transport](r_configure_gateway_service.md#protocoltransport), where *protocol* can be pipe, tcp, ssl, or http
            -   [ws.maximum.message.size](r_configure_gateway_service.md#wsmaximummessagesize)
            -   [http.keepalive.timeout](r_configure_gateway_service.md#httpkeepalivetimeout)
            -   [ssl.ciphers](r_configure_gateway_service.md#sslciphers)
            -   [ssl.protocols](r_configure_gateway_service.md#sslprotocols-and-sockssslprotocols)
            -   [ssl.encryption](r_configure_gateway_service.md#sslencryption)
            -   [ssl.verify-client](r_configure_gateway_service.md#sslverify-client)
            -   [socks.mode](r_configure_gateway_service.md#socksmode) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [socks.ssl.ciphers](r_configure_gateway_service.md#sockssslciphers) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [socks.ssl.protocols](r_configure_gateway_service.md#sslprotocols-and-sockssslprotocols) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [socks.ssl.verify-client](r_configure_gateway_service.md#sockssslverify-client) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [socks.retry.maximum.interval](r_configure_gateway_service.md#socksretrymaximuminterval) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [tcp.maximum.outbound.rate](r_configure_gateway_service.md#tcpmaximumoutboundrate) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
            -   [ws.inactivity.timeout](r_configure_gateway_service.md#wsinactivitytimeout)
            -   [http.server.header](r_configure_gateway_service.md#httpserverheader)
        -   [mime-mapping](r_configure_gateway_service_defaults.md)
            -   [extension](r_configure_gateway_service_defaults.md)
            -   [mime-type](r_configure_gateway_service_defaults.md)
    -   [security](r_configure_gateway_security.md)
        -   [keystore](r_configure_gateway_security.md#keystore)
            -   [type](r_configure_gateway_security.md#keystore)
            -   [file](r_configure_gateway_security.md#keystore)
            -   [password-file](r_configure_gateway_security.md#keystore)
        -   [truststore](r_configure_gateway_security.md#truststore)
            -   [type](r_configure_gateway_security.md#truststore)
            -   [file](r_configure_gateway_security.md#truststore)
            -   [password-file](r_configure_gateway_security.md#truststore)
        -   [realm](r_configure_gateway_security.md#realm)
            -   [name](r_configure_gateway_security.md#realm)
            -   [description](r_configure_gateway_security.md#realm)
            -   [authentication](r_configure_gateway_security.md#authentication)
                -   [http-challenge-scheme](r_configure_gateway_security.md#authentication)
                -   [http-header](r_configure_gateway_security.md#authentication)
                -   [http-query-parameter](r_configure_gateway_security.md#authentication)
                -   [http-cookie](r_configure_gateway_security.md#authentication)
                -   [authorization-mode](r_configure_gateway_security.md#authentication)
                -   [authorization-timeout](r_configure_gateway_security.md#authentication) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
                -   [session-timeout](r_configure_gateway_security.md#authentication) ![This feature is available in KAAZING Gateway - Enterprise Edition](../images/enterprise-feature.png)
                -   [login-modules](r_configure_gateway_security.md#authentication)
                    -   [login-module](r_configure_gateway_security.md#login-module)
                        -   [type](r_configure_gateway_security.md#login-module)
                        -   [success](r_configure_gateway_security.md#login-module)
                        -   [options](r_configure_gateway_security.md#options-login-module)
                            -   [debug](r_configure_gateway_security.md#options-login-module)
                            -   [tryFirstToken](r_configure_gateway_security.md#options-login-module)
            -   [user-principal-class](r_configure_gateway_security.md#realm)
    -   [cluster](r_configure_gateway_cluster.md)
        -   [name](r_configure_gateway_cluster.md#cluster)
        -   [accept](r_configure_gateway_cluster.md#cluster)
        -   [connect](r_configure_gateway_cluster.md#cluster)
