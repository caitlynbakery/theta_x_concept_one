# THETA X Button Controls

![screenshot](docs/layout.png)

This application does not use state management, but uses the [http](https://pub.dev/packages/http) package from dart to connect to the camera. Using the http commands, such as get and post, the application receives information and executes commands. Below is a list of this project's button controls using the [THETA API](https://api.ricoh/docs/theta-web-api-v2.1/). 

## Button Controls

* [Info](https://api.ricoh/docs/theta-web-api-v2.1/protocols/info/)
* [State](https://api.ricoh/docs/theta-web-api-v2.1/protocols/state/)
* [camera.takePicture](https://api.ricoh/docs/theta-web-api-v2.1/commands/camera.take_picture/)

## HTTP Package

In order to use the HTTP package, I first needed to download it and import it at the top of my file. All HTTP requests require a url. For example, this is the url for getting the info from the camera. 

```
var url = Uri.parse('http://192.168.1.1/osc/info');
```

Next, I created a header using a key-value pair. 

```
var header = {
    'Content-Type': 'application/json;charset=utf-8'}
```

Finally, the http method is sent out and assigned to a variable called `response`.

```
var response = await http.get(url, headers: header);
```

## Take Picture

The command for taking a picture differs slightly from the info and state buttons as we need to run `jsonEncode`. The url for taking the picture uses `/osc/commands/execute` as shown below.

```
var url = Uri.parse('http://192.168.1.1/osc/commands/execute');
```

I created a map called `bodyMap` that stores the key-value pair from the camera. Next, I had to encode the map into Json, so I could pass it into my response. 

```
var bodyMap = {'name': 'camera.takePicture'};
var bodyJson = jsonEncode(bodyMap);
```

The response requires the url, but we also passed in the bodyJson.

```
  var response = await http.post(url, headers: header, body: bodyJson);
```