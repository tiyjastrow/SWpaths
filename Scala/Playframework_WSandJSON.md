## Play WS and Json

**Requests and Responses**

Play supports HTTP requests and responses with a content type of JSON by using the HTTP API in combination with the JSON library.

Play uses the JSON library and the HTTP API to manage the HTTP requests and responses. The HTTP header will show Content-Type: application/json.

1. Open the PlayScalaSeed project and start SBT and run the app.
2. Let's make the GET request on the `/json` route. Run this curl command from the unix prompt.
```
$ curl --include http://localhost:9000/json?parm=curltest
```

The response should be "HTTP/1.1 200 OK  Content-Type: application/json"

#### POST requests
Let's add more to our Model to handle GETs and POSTs requests:
1. Add 2 more parameters to the case class Thing..

```
case class Thing(myThing: String, count: Int, active: Boolean)
```

2. Add a List to the companion object..

```
object Thing {
  ..

  var list: List[Thing] = {
    // default data to start
    List(
      Thing("strange thing", 1, false),
      Thing("odd thing", 26, true)
      )
  }

  def save(thing: Thing): Boolean = {
    val listCount = list.size
    list = list ::: List(thing)
    list.size - listCount == 1
  }
```

Now, we’ll need to define the more complicated Action to accept a POST and JSvalue content from the client in the Request object.

First, we'll need 

```
 def saveThing = Action(BodyParsers.parse.json) { request =>
    val res = request.body.validate[Thing].map{
      case thing => {
        if (Thing.save(thing)) {
          Ok(Json.obj("status" -> "OK", "message" -> ("Your Thing '" + thing.myThing + "' successfully saved.")))
        } else {
          UnprocessableEntity(Json.obj("status" -> "Unprocessable", "message" -> s"Failed to save: ${thing.myThing}"))
        }
      }
      case _ =>  BadRequest(Json.obj("status" ->"KO"))
      }
    res.get
    }
```

* The `Action` is expecting a request containing a Content-Type header of `text/json` or `application/json` and a body containing a JSON representation of the model we want to create.
* The JSON BodyParser will parse the request and give us `request.body` as a JsValue for our model.
* The validate method on the BodyParser will convert the JsValue to our model using our implicit Reads.
* The `Action` also sends a JSON response.
* Then, we need to define a route in conf/routes:

```
POST    /json                       controllers.Application.saveThing
```

* Now we're ready to test: run from the command line (the backslash is for multiline commands)

```
curl --include \
  --request POST \
  --header "Content-type: application/json" \
  --data '{"myThing":"little posted","count":22,"active":true}' \
  http://localhost:9000/json
```

The response should also be 200 json.

Try passing bad data and see the error returned.


