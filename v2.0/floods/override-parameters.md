It is possible to override parameters in your JMeter and Gatling test plans when using Flood IO.

![](https://flood.io/images/blog/override_parameters.gif)

__System (Java) properties__ can generally be specified with the `-D` prefix. Examples might include:

To set the time to cache successful name resolutions for the InetAddress Cache,
edit the following to reflect the desired value:

`-Dnetworkaddress.cache.ttl=10`

To set the time to cache unsuccessful name resolutions for the InetAddress Cache,
edit the following to reflect the desired value:

`-Dnetworkaddress.cache.negative.ttl=10`

To set a custom keystore and password for client certificates as [described here](https://flood.io/blog/23-mutual-two-way-ssl-with-jmeter):

`-Djavax.net.ssl.keyStore=/var/log/flood/files/yourkeystore.jks`

`-Djavax.net.ssl.keyStorePassword=yourpassword`

You can also modify __JMeter properties__ with the `-J` prefix. Examples might include:

Change from the default TLS protocol to SSLv3 for HTTPS:

`-Jhttps.default.protocol=SSLv3`

Modify CookieManager behaviour to store Cookies as variables:

`-JCookieManager.save.cookies=true`

Change the number of TCP retries for HttpClient 4

`-Jhttpclient4.retrycount=2`

Override parameters with caution. Flood IO makes an effort to provide sensible defaults, changing parameters may affect the performance of your tests.