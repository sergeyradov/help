The Flood UI allows you to specify parameters such as `threads`, `rampup` and `duration`.

To make sure your test plans receive these parameters, remove hard coded values and replace with the following. Note: we never over write actual values in your test plans with values entered in the UI.

### JMeter

```
${__P(threads, 50)}
${__P(rampup, 60)}
${__P(duration, 120)}
```

![](https://s3.amazonaws.com/flood-io-support/jmeter-example.jmx_UserstimkoopmansGitfloodflood-sitepublicjmeter-example.jmx_-_Apache_JMeter_2.12_r1636949_2015-02-04_17-09-27.jpg)

### Gatling

```
val threads   = Integer.getInteger("threads",  50)
val rampup    = Integer.getInteger("rampup",   60).toLong
val duration  = Integer.getInteger("duration", 120).toLong
```

![](https://s3.amazonaws.com/flood-io-support/DropboxGitfloodflood-sitepublicgatling-example.scala__flood_2015-02-04_17-12-19.jpg)

Note that `rampup` and `duration` should always be specified in seconds.