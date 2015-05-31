It is possible to include custom libraries when running flood tests on your grid nodes.

This lets you bundle 3rd party external libraries for those with experimental or commercial plugins not already included by Flood IO. It also lets you modify default JMeter and/or Gatling settings and configuration on the fly.

Examples might include:

- [UBIK Load Pack Productivity extensions for JMeter](http://ubikloadpack.com/)
- [Atlassian JWT-signed request plugin for Gatling](https://bitbucket.org/atlassianlabs/gatling-jwt)
- [Web Sockets plugin for JMeter](https://github.com/kawasima/jmeter-websocket)
- [Web Sockets plugin for Gatling](https://github.com/gilt/gatling-websocket)

To install these plugins, simply upload a `zip` file with your flood test that includes your custom directories such as `bin`, `lib`, `lib/ext` or `conf`.

![](https://s3.amazonaws.com/flood-io-support/bin_2015-03-02_23-45-24.jpg)

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2015-03-02_23-50-07.jpg)

These will then be merged with your container when the flood test executes. Containers are unique to your test and discarded at completion.

Tip: you can save yourself having to upload the same archive each time by using the repeat functionality for your next flood test.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2015-03-02_23-53-38.jpg)