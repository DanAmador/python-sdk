# MercadoLibre's Python SDK

This is the official Python SDK for MercadoLibre's Platform.

## How do I use it?
* Install it with pip
    * ```  $ pip install meli_client```
* Import it to your project
    * ```from meli_client import meli```
## How do I use it?

The first thing to do is to instance a ```Meli``` class. You'll need to give a ```clientId```, a ```clientSecret``` and a ```site_id``` depending on the country for which you wish to develop. 
You can obtain both after creating your own application. 
For more information on this please read: [creating an application](http://developers.mercadolibre.com/application-manager/)

You must use one of the following site_id's or it'll default to MLA


* **MLA** for Argentina
* **MLB** for Brazil
* **MCO** for Colombia
* **MCR** for Costa Rica
* **MEC** for Ecuador
* **MLC** for Chile
* **MLM** for Mexico
* **MLU** for Uruguay
* **MLV** for Venezuela
* **MPA** for Panama
* **MPE** for Peru
* **MPT** for Portugal
* **MRD** for the Dominican Republic
 

### Create an instance of Meli class
Simple like this
```python
meli_obj = meli.Meli(client_id=1234, client_secret="a secret",site_id="MLA")
```
With this instance you can start working on MercadoLibre's APIs.

There are some design considerations worth to mention.

1. This SDK is just a thin layer on top of an http client to handle all the OAuth WebServer flow for you.

2. There is JSON parsing. this SDK will include [json](http://docs.python.org/2/library/json.html) for internal usage.

3. If you already have the access_token and the refresh_token you can pass in the constructor

```python
meli_obj = meli.Meli(client_id=1234, client_secret"a secret", access_token="Access_Token", refresh_token="Refresh_Token",site_id="MLA")
```

## How do I redirect users to authorize my application?

This is a 2 step process.

First get the link to redirect the user. This is very easy!

```python
redirectUrl = meli_obj.auth_url(redirect_URI="http://somecallbackurl")
```

This will give you the url to redirect the user. You need to specify a callback url which will be the one that the user will redirected after a successfull authrization process.

Once the user is redirected to your callback url, you'll receive in the query string, a parameter named ```code```. You'll need this for the second part of the process.

```python
meli_obj.authorize(code="the received code", redirect_URI="http://somecallbackurl")
```

This will get an ```access_token``` and a ```refresh_token``` (is case your application has the ```offline_access```) for your application and your user.

At this stage your are ready to make call to the API on behalf of the user.

#### Making GET calls

```python
params = {'access_token' : meli_obj.access_token}
response = meli_obj.get(path="/users/me", params=params)
```

#### Making POST calls

```python
params = {'access_token' : meli_obj.access_token}

  #this body will be converted into json for you
body = {'foo'  : 'bar', 'bar' : 'foo'}

response = meli_obj.post(path="/items", body=body, params=params)
```

#### Making PUT calls

```python
params = {'access_token' : meli_obj.access_token}

  #this body will be converted into json for you
body = {'foo'  : 'bar', 'bar' : 'foo'}

response = meli_obj.put(path="/items/123", body=body, params=params)
```

#### Making DELETE calls
```python
params = {'access_token' : meli_obj.access_token}
response = meli_obj.delete(path="/questions/123", params=params)
```

## Examples

Don't forget to check out our examples codes in the folder [examples](https://github.com/mercadolibre/python-sdk/tree/master/examples)

## Community

You can contact us if you have questions using the standard communication channels described in the [developer's site](http://developers.mercadolibre.com/community/)

## I want to contribute!

That is great! Just fork the project in github. Create a topic branch, write some code, and add some tests for your new code.

Thanks for helping!
