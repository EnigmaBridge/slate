---
title: API Reference

language_tabs:
  - Java
  - Javascript
  - Node.js
  - Python
  - REST API

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to Enigma Bridge API documentation! Enigma Bridge provides managed hardware encryption. The hardware security ensures that your secrets can't be extracted using any physical attacks. This protection is validated according to the FIPS 140-2 standard on **Level 3**. The **Level 3** means that the hardware device will erase secrets it stores when it detects an attack. 

Side-channel, and logical attacks are also significantly limited. Assurance of this protection is provided by Common Criteria certifications on EAL5 ([EAL5 explained on Wikipedia](https://en.wikipedia.org/wiki/Evaluation_Assurance_Level#EAL5:_Semiformally_Designed_and_Tested)).

You can use our API to access Enigma Bridge encryption service directly or transparently via production integrations. The set of operations available via the API includes:

* **authentication** - create user context, verify passwords and one-time passwords, reset passwords;
* **payment systems** - re-encryption between transport and database keys, tokenization;
* **key management** - generate, unwrap, manage keys; 
* **random data generation** - using FIPS140-2 certified generators;
* **encryption** - just encrypt, MAC, sign, ...; or
* **re-encryption** - atomic re-encryption from asymetric to symetric algorithms.

The native API is REST web-service. We also provide language bindings for simple integration. We currently support Javascript (client and node.js), Python and Java. This site provides description of API functions, including code examples in the dark area to the right. Tabs in the top right allow  switching between the programming language of the examples.

# Basics

Use of Enigma Bridge consists of four phases:

1. **user enrolment** - creating user account;
2. **API key creation** - obtaining API key with authorizations and also networkconnection details to a suitable Enigma Bridge instance;
3. **operation enrolment** - introduction of a new use request into Enigma Bridge; and
4. data processing - operational use.

Depending on the use case and limits on usage, all steps can be completed fully automatically within seconds. If a particular use case requires more strict control, step 1 will require manual approval.


Each of the steps can be represented by a single API command regardless of the operation you request. The result of the enrolment is a configuration string that uniquely identifies a new user object.


# Enrolment 

> To authorize, use this code:


```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

