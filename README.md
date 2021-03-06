#DevOps with Nodepop

To use a complet API an deploy it on a server, we use the API Nodepop developed by Javier Miguel jamg44.

## links

If no need to have a specific knowlege of what can you do with this API, yaou can read the complete guide that you have in this file.

You can see some examples and fases that you need to do previosly the deploy...

this Api its hosted in: [http://ec2-23-22-13-95.compute-1.amazonaws.com/](http://ec2-23-22-13-95.compute-1.amazonaws.com/).

We use a elastic IP: [http://23.22.13.95/](http://23.22.13.95/) , where there is deployed a Bootstrap theme.

We use NGINX to served the static files, if you want to check it,you can see the info when you visit th nodepop API or you can use the chrome developers tools and visit these two links:

[http://ec2-23-22-13-95.compute-1.amazonaws.com/images/anuncios/bici.jpg](http://ec2-23-22-13-95.compute-1.amazonaws.com/images/anuncios/bici.jpg)

[http://ec2-23-22-13-95.compute-1.amazonaws.com/stylesheets/style.css](http://ec2-23-22-13-95.compute-1.amazonaws.com/stylesheets/style.css)

In both cases, we put a header with de X-owner.

This API have persistence that works with PM2 and works fine and protected with file2ban.

# NodePop

Api for the iOS/Android apps.

## Deploy

### Install dependencies  
    
    npm install

### Configure  

Review models/db.js to set database configuration

### Init database

    npm run installDB

## Start

To start a single instance:
    
    npm start

To start in cluster mode: 

    npm run cluster  

To start a single instance in debug mode:

    npm run debug (including nodemon & debug log)

## Test

    npm test (pending to create, the client specified not to do now)

## JSHint & JSCS

    npm run hints

## API v1 info


### Base Path

The API can be used with the path: 
[API V1](/apiv1/anuncios)

### Security

The API uses JSON Web Token to handle users. First you will need to call /usuarios/register to create a user.  

Then call /usuarios/authenticate to obtain a token.
  
Next calls will need to have the token in:  

- Header: x-access-token: eyJ0eXAiO...
- Body: { token: eyJ0eXAiO... }
- Query string: ?token=eyJ0eXAiO...

### Language

All requests that return error messages are localized to english, if you want to 
change language make the request with the header x-lang set to other language, 
i.e. x-lang: es 

### Error example

    {
      "ok": false,
      "error": {
        "code": 401,
        "message": "Authentication failed. Wrong password."
      }
    }

### POST /usuarios/register

**Input Body**: { nombre, email, clave}

**Result:** 

    {
      "ok": true, 
      "message": "user created!"
    }

### POST /usuarios/authenticate

**Input Body**: { email, clave}

**Result:** 

    {
      "ok": true, 
      "token": "..."
    }

### GET /anuncios

**Input Query**: 

start: {int} skip records  
limit: {int} limit to records  
sort: {string} field name to sort by  
includeTotal: {bool} whether to include the count of total records without filters  
tag: {string} tag name to filter  
venta: {bool} filter by venta or not  
precio: {range} filter by price range, examples 10-90, -90, 10-   
nombre: {string} filter names beginning with the string  

Input query example: ?start=0&limit=2&sort=precio&includeTotal=true&tag=mobile&venta=true&precio=-90&nombre=bi

**Result:** 

    {
      "ok": true,
      "result": {
        "rows": [
          {
            "_id": "55fd9abda8cd1d9a240c8230",
            "nombre": "iPhone 3GS",
            "venta": false,
            "precio": 50,
            "foto": "/images/anuncios/iphone.png",
            "__v": 0,
            "tags": [
              "lifestyle",
              "mobile"
            ]
          }
        ],
        "total": 1
      }
    }


### GET /anuncios/tags

Return the list of available tags for the resource anuncios.

**Result:** 

    {
      "ok": true,
      "allowed_tags": [
        "work",
        "lifestyle",
        "motor",
        "mobile"
      ]
    }

### POST /pushtokens

Save user pushtoken { pushtoken, plataforma, idusuario}

idusuario is optional.  
plataforma can be 'ios' or 'android'  

**Result:** 

    {
      "ok": true,
      "created": {
        "__v": 0,
        "token": "123456",
        "usuario": "560ad58ff82387259adbf26c",
        "plataforma": "android",
        "createdAt": "2015-09-30T21:01:19.955Z",
        "_id": "560c4b648b892ca73faac308"
      }
    }
