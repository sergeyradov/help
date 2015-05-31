When you launch a test on Flood IO we tag each test with a unique identifier. This helps us identify and parse your results. We do this by setting a system property called `uuid` which is passed into your test plan at run time.

We then extract results using `InfoExtractors` to write the `uuid`, `Content-Length` and `StatusCode` headers to the simulation log file.

Please ensure that your test plan sets the `uuid` variable to the matching System property as follows.

```scala
val uuid = List(System.getProperty("uuid"))
```

Complete examples are shown below.

__Gatling 1.5.x__
```scala
import com.excilys.ebi.gatling.core.Predef._
import com.excilys.ebi.gatling.http.Predef._
import com.excilys.ebi.gatling.jdbc.Predef._
import com.excilys.ebi.gatling.http.Headers.Names._
import akka.util.duration._
import bootstrap._

class TestPlan extends Simulation {

  val uuid = List(System.getProperty("uuid"))

  val httpConf = httpConfig
      .baseURL("http://google.com/")
      .acceptHeader("text/html,application/xhtml+xml,application/xml;")
      .acceptEncodingHeader("gzip, deflate")
      .requestInfoExtractor((request: Request) => { uuid })
      .responseInfoExtractor(response => Option(response.getHeader("Content-Length")).getOrElse("0") :: List(response.getStatusCode.toString))

   val scn = scenario("scenario")
      .during(duration seconds) {
        exec(
          http("google.com")
          .get("/"))
        .pause(1000 milliseconds, 5000 milliseconds)

      }

  setUp(scn.users(10).ramp(60).protocolConfig(httpConf))
}
```

__Gatling 2.x__
```scala
import scala.concurrent.duration._

import io.gatling.core.Predef._
import io.gatling.http.Predef._
import io.gatling.jdbc.Predef._


class TestPlan extends Simulation {

  val uuid = List(System.getProperty("uuid"))

  val httpProtocol = http
    .baseURL("http://google.com")
    .acceptHeader("text/html,application/xhtml+xml,application/xml;")
    .acceptEncodingHeader("gzip, deflate")
    .extraInfoExtractor((status, session, request, response) =>
      List(uuid, Option(response.getHeader("Content-Length")).getOrElse("0"), response.getStatusCode.toString))

  val scn = scenario("Scenario Name")
    .exec(http("first_page")
    .get("/"))
    .pause(1000 milliseconds, 5000 milliseconds)

  setUp(scn.inject(ramp(10 users) over (60 seconds))).protocols(httpProtocol)
}
```