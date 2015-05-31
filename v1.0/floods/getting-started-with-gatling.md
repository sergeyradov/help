# Getting Gatling

Gatling can be downloaded [here](http://gatling.io/download/) and typically you can start Gatling using the `gatling.bat` or `gatling.sh` scripts located in the downloaded archive `bin` directory. To prepare your first test plan you may find the Gatling recorder useful.

# Creating a Test Plan

Once you have created or downloaded our [basic test plan](https://flood.io/gatling-example.scala) and saved it locally as a `.scala` file, you can upload this to Flood IO to create your first Flood test.

To make sure your Gatling test plan is compatible with Flood IO you need to import the Flood IO libraries as follows:

```scala
// Mandatory, you must import Flood libraries
import flood._
```

You also need to make sure your `httpProtocol` uses the predefined  `httpConfigFlood` element as follows:

```scala
// Mandatory, you must use httpConfigFlood
val httpProtocol = httpConfigFlood
    .baseURL("http://google.com/")
    .acceptHeader("text/html,application/xhtml+xml,application/xml;")
    .acceptEncodingHeader("gzip, deflate")
```

This helps uniquely identify your tests on Flood IO and makes use of custom Flood IO libraries for detailed reporting and transaction tracing.

Note: If you want to develop scripts locally you can stub Flood IO libraries by downloading [flood.scala](/flood.scala). However, don't upload this file when using Flood IO.

The complete script should look like this:

```scala
import scala.concurrent.duration._

import io.gatling.core.Predef._
import io.gatling.http.Predef._
import io.gatling.jdbc.Predef._

// Mandatory, you must import Flood libraries
import flood._

// Redis related libraries
import com.redis._
import io.gatling.redis.feeder._

class TestPlan extends Simulation {
  // Optional, Flood IO will pass in threads, rampup and duration properties from UI
  val threads   = Integer.getInteger("threads",  10)
  val rampup    = Integer.getInteger("rampup",   10).toLong
  val duration  = Integer.getInteger("duration", 120).toLong

  // Mandatory, you must use httpConfigFlood
  val httpProtocol = httpConfigFlood
    .baseURL("http://google.com/")
    .acceptHeader("text/html,application/xhtml+xml,application/xml;")
    .acceptEncodingHeader("gzip, deflate")

  val redisPool = new RedisClientPool("172.31.28.223", 6379)

  val scn = scenario("Scenario Name")
    .exec(http("first_page")
    .get("/"))
    .pause(1000 milliseconds, 5000 milliseconds)
    .repeat(2) {
      feed(RedisFeeder(redisPool, "postcodes_sorted", RedisFeeder.LPOP))
      .exec(
        http("search_page")
          .get("/?q=${postcodes_sorted}")
          .check(status.is(200))
      )
      .pause(0 milliseconds, 100 milliseconds)
    }

  setUp(scn.inject(rampUsers(10) over (30 seconds))).protocols(httpProtocol)
}
```

### Creating a Flood Test

After you login to Flood IO you can click the "+" icon or from your [Floods dashboard](/dashboard/floods) you can click the "Create Flood" button to create a new Flood.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-06_17-36-50.jpg)

In the new Flood form, add the `.scala` test plan you created using JMeter to the Files input and select a Grid to run your Flood test on. You can optionally set the "Name" or "Tags" to help identify the test in your dashboard.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-06_21-28-54.jpg)

### Start your Flood Test

Read about more [advanced settings] or simply click the "Start Flood" button to launch your test.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-06_17-53-22.jpg)

You will be redirected to a Flood report which will show you results in real time from the test you just executed on the chosen Grid.

## Feeders

A common requirement of test plans is to access static test data from another source. Whilst most people start out with [CSV feeders](https://github.com/excilys/gatling/wiki/Feeders) this quickly becomes complex to manage for distributed load testing scenarios where you have many load generators (flood nodes) potentially needing access to a single source of data. Instead of carving up your test data into discrete chunks, we built a custom Redis backed solution which lets you effortlessly [share data](https://flood.io/blog/9-sharing-test-data) between flood nodes.

If you just want to access random data, which does not need to be unique per user, you can upload a standard CSV file to your grid and access it via our high performance test data API:

```
exec(http("get_test_data")
  .get("http://172.31.10.132/data/SRANDMEMBER/postcodes?type=text")
  .check(regex("""(\d+)""").saveAs("postcode"))
```

Alternatively, if you'd like to use the Gatling provided Redis feeder you can also upload a standard CSV file to your grid, but make sure it is a __Sorted__ list, the default queue strategy for Gatling. Each time the feed is called, the first record of the feeder is removed from the queue and injected into the session.

Then import the relevant package:

```scala
import com.redis._
import io.gatling.redis.feeder._
```

Define a Redis client pool within your test plan:

`val redisPool = new RedisClientPool("172.31.28.223", 6379)`

You will then have access to the redis feeder within your scenario:

`.feed(RedisFeeder(redisPool, "key_name", RedisFeeder.LPOP))`

## Example Script with Redis Feeder

Putting it all together, your script should look like this:

```scala
import scala.concurrent.duration._

import io.gatling.core.Predef._
import io.gatling.http.Predef._
import io.gatling.jdbc.Predef._

// Mandatory, you must import Flood libraries
import flood._

// Redis related libraries
import com.redis._
import io.gatling.redis.feeder._

class TestPlan extends Simulation {
  // Optional, Flood IO will pass in threads, rampup and duration properties from UI
  val threads   = Integer.getInteger("threads",  10)
  val rampup    = Integer.getInteger("rampup",   10).toLong
  val duration  = Integer.getInteger("duration", 120).toLong

  // Mandatory, you must use httpConfigFlood
  val httpProtocol = httpConfigFlood
    .baseURL("http://google.com/")
    .acceptHeader("text/html,application/xhtml+xml,application/xml;")
    .acceptEncodingHeader("gzip, deflate")

  val redisPool = new RedisClientPool("172.31.28.223", 6379)

  val scn = scenario("Scenario Name")
    .exec(http("first_page")
    .get("/"))
    .pause(1000 milliseconds, 5000 milliseconds)
    .repeat(2) {
      feed(RedisFeeder(redisPool, "postcodes_sorted", RedisFeeder.LPOP))
      .exec(
        http("search_page")
          .get("/?q=${postcodes_sorted}")
          .check(status.is(200))
      )
      .pause(0 milliseconds, 100 milliseconds)
    }

  setUp(scn.inject(rampUsers(10) over (30 seconds))).protocols(httpProtocol)
}
```