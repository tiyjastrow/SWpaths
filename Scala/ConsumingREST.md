## Consuming RESTful services
(..continuing with our PlayScalaSeed project)

To use WS, first add `ws` to your build.sbt file:

libraryDependencies ++= Seq(
  ws
)

Create a new controller called `ApiConsumer` to consume a public Web Service. We'll have to declare a dependency on the WSClient.

```
  import javax.inject.Inject

  import play.api.libs.ws._
  import play.api.mvc._
  import play.api.Play.current

  import scala.concurrent.{Await, TimeoutException}
  import scala.concurrent.duration._

  import scala.concurrent.ExecutionContext.Implicits.global

  class ApiConsumer @Inject() (ws: WSClient) extends Controller {
  }
```

* Get the weather forecast from the Yahoo Weather service.
  * (this one also works from [accuweather.com](http://apidev.accuweather.com/currentconditions/v1/351090.json?language=en&apikey=hoArfRosT1215)
* Time out if a response is not received in five seconds.
```
 def getWeather = Action {
   val promiseOfString = WS.url("http://weather.yahooapis.com/forecastrss?p=80020&u=f").get().map {
     response => response.body
   }
   try {
     val res = Await.result(promiseOfString, 5.seconds)    // Blocking with `Await`
     Ok(res)
   } catch {
     case e: TimeoutException => InternalServerError("Request timed out.")
   }
 }
```

* Now add the route.
  * and test with:  `localhost:9000/weather`
  * or this: `http://apidev.accuweather.com/currentconditions/v1/351090.json?language=en&apikey=hoArfRosT1215`
* Next, we'll run our own service using Sinatra at Port :4567.
  * run: `ruby myapp.rb` with a "/hello" GET route
      * get '/hello' do
      * '{"field1" : "value of field1", "field2" : "value of field2"}'
      * end
  * and add this Action to ApiConsumer:

```
 def getHello = Action {
   val url = "http://localhost:4567/"
   val promiseOfString = WS.url(url).get().map { response => response.body }
   try {
     val res = Await.result(promiseOfString, 1.seconds)
     Ok(res)
   } catch {
     case e: TimeoutException => InternalServerError("Service unavailable")
   }
 }
```

  * test with localhost:4567
