To get the most out of Gatling integration with Flood IO you will need to `import flood._` packages in your test plan.

Complete examples are shown below.

__Gatling 1.5.x__
```scala
import com.excilys.ebi.gatling.core.Predef._
import com.excilys.ebi.gatling.http.Predef._
import com.excilys.ebi.gatling.jdbc.Predef._
import com.excilys.ebi.gatling.http.Headers.Names._
import akka.util.duration._
import bootstrap._

// Mandatory, you must import Flood libraries
import flood._

class Basic extends Simulation {

  // Optional, Flood IO will pass in threads, rampup and duration properties from UI
  val threads   = Integer.getInteger("threads",  10)
  val rampup    = Integer.getInteger("rampup",   10).toLong
  val duration  = Integer.getInteger("duration", 15).toLong

  // Mandatory, you must use httpConfigFlood
  val httpConf = httpConfigFlood
      .baseURL("http://google.com/")
      .acceptHeader("text/html,application/xhtml+xml,application/xml;")
      .acceptEncodingHeader("gzip, deflate")

   val scn = scenario("scenario")
      .during(duration seconds) {
        exec(
          http("home_page")
          .get("/"))
        .pause(1000 milliseconds, 5000 milliseconds)

      }

  setUp(scn.users(threads).ramp(rampup).protocolConfig(httpConf))
}

```

__Gatling 2.0.0-x__
```scala
import scala.concurrent.duration._

import io.gatling.core.Predef._
import io.gatling.http.Predef._
import io.gatling.jdbc.Predef._

// Mandatory, you must import Flood libraries
import flood._

class TestPlan extends Simulation {

  // Optional, Flood IO will pass in threads, rampup and duration properties from UI
  val threads   = Integer.getInteger("threads",  10)
  val rampup    = Integer.getInteger("rampup",   10).toLong
  val duration  = Integer.getInteger("duration", 15).toLong

  // Mandatory, you must use httpConfigFlood
  val httpProtocol = httpConfigFlood
    .baseURL("http://google.com/")
    .acceptHeader("text/html,application/xhtml+xml,application/xml;")
    .acceptEncodingHeader("gzip, deflate")

  val scn = scenario("Scenario Name")
    .exec(http("first_page")
    .get("/"))
    .pause(1000 milliseconds, 5000 milliseconds)

  setUp(scn.inject(ramp(10 users) over (60 seconds))).protocols(httpProtocol)
}
```