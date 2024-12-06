# Colors application
In this section we will create a web application that manages the RGB values of colors, including a Firestore Databases, Forms for editing and API documentation with Swagger. We define a Color DAO which is has the same structure as the former example that allows CRUD operations.

## WTForms
In order to manage and validate the form field we include the WTForms library, which allows us to define a form object and define each field as its properties.  In our case we are trying to bind our form to the Color DAO object.
```Python
from wtforms import Form, IntegerField,validators
class ColorForm(Form):
    red = IntegerField('Red', [validators.NumberRange(min=0,max=255)])
    green = IntegerField('Green', [validators.NumberRange(min=0,max=255)])
    blue = IntegerField('Blue', [validators.NumberRange(min=0,max=255)])
```
We need to reference them also in the HTML template.
```HTML
<form method="POST">
<table>
    <tr><td>{{form.red.label}}</td><td>{{form.red}}</td></tr>
    <tr><td>{{form.green.label}}</td><td>{{form.green}}</td></tr>
    <tr><td>{{form.blue.label}}</td><td>{{form.blue}}</td></tr>
    <tr><td></td><td><input type="submit" value="Edit"></td></tr>
</table>
</form>
```
In our route `/colors/<color>` we fetch the inputs from the `POST` request.
```Python
cform=Colorform(request.form)
colors_dao.add_color(color, cform.red.data, cform.green.data, cform.blue.data)
```
If the request is `GET` we create a we initialize the form bu using the class Struct.
```Python

## dictionary to object
class Struct:
    def __init__(self,**entries):
        self.__dict__.update(entries)
.....

c=colors_dao.get_color_by_name(color)
cform=Colorform(obj=Struct(**c)))
```

## API definition
First of all we do a recall about HTTP REST:
 - REST is a stateless paradigm
 - Each resource is identified by an URI
 - The types of access are mapped over HTTP methods `POST`,`GET`,`PUT`,`DELETE`...
 - The outcome of the request is defined by a status code
 - The URI path is `/api/version/resource/label/`

We describe our API in a YAML document, where we specify, endpoints, operations, parameters and return values on each operation, authentication methods and more. The first portion of our YAML document specifies the information metadata, base path, host and schemes(protocols).

```yaml
swagger: "2.0"
info:
    title: "Colors"
    description: "Manage color with RGB components"
    version: "1.0.0"
host: "api-dot-colors20221001.appspot.com"
basePath: "/api/v1"
schemes:
    - "https"
```


```yaml
paths:
    /colors/{colorName}:
        parameters: 
        -   name: colorName
            in: path
            description: 'Name of the color'
            type: string
            required: true
```












____________________
*example*
Color API
/api/v0.1/color/yellow
POST: create a new color given RGB values
Content-type: application/json{‘red’: 255, ‘green’: 255, ‘blue’: 0}
Form data:
Content-type: application/x-www-form-urlencodedred=255&green=255&blue=0
Rest compliant response
400 Bad request{‘message’: ‘invalid request parameters’}
200 OK{‘status’: ‘ko’,‘message’: ‘invalid request parameters’}

/api/v0.1/json/color/yellow
{“red”: 255,“green”: 255,“blue”: 0}

## OpenAPI
Is based on Swagger initiative the API describes:
 - Endpoints
 - Operations on each endpoints (HTTP methods)
 - Parameters and return values on each operation
 - Authetication methods
 - Contact information, license, terms of use, ...

An API can be described by a JSON or YAML document

consumes and produces are the mime types accepted and given in output
base path is the prefix for api
schemes are the accepted protocols
paths is the list of API entrypoints, which defines the method 
parametrers and responses

swagger: "2.0"
info:
 title: Sample API
 description: "API descr."
 version: 1.0.0
host: api.example.com
basePath: /v1
schemes:
 - https
consumes:
 - application/json
 - application/xml
produces:
 - application/json
 - application/xml
paths:
 /users/{userId}:
  get:
    summary: "Get user"
    parameters:
     - in: path
       name: userId
       required: true
       type: integer
       minimum: 1
       Description: "..."
    responses:
       200:
           description: OK
           schema:
               type: object
               properties:
                   id:
                      type: integer
                      example: 4
                   name:
                      type: string
                      example: "RL"
       400:
           description: "..."
definitions:
     User:
       properties:
              id:
                type: integer
              name:
                type: string
        # in response section
schema:
     $ref: '#/definitions/User'


securityDefinitions:BasicAuth:type: basicsecurity:- BasicAuth: []





