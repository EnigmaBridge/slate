---
title: Enigma Bridge API Reference

language_tabs:
  - json: RESTful
  - java: Java
  - javascript: Javascript
  - python: Python
  - shell: cURL

toc_footers:
  - <a href='https://enigmabridge.freshdesk.com'>Ask a question, raise a support ticket</a>
  - <a href='https://enigmabridge.com'>Visit our main website</a>

includes:
  - errors
  - usermanager_client
  - usermanager_enrol

search: true
---

# Introduction

Welcome to Enigma Bridge API documentation and congratulations on taking steps to properly secure your systems from cyberattacks.

Enigma Bridge provides managed hardware encryption for a variety of applications and our proprietary hardware security platform ensures that your secrets can’t be extracted using any logical nor physical attacks. This protection is validated according to the FIPS 140-2 standard on **Level 3**. The **Level 3** means that the hardware device will erase secrets it stores when it detects an attack.

Side-channel, and logical attacks are also significantly limited. Assurance of this protection is provided by Common Criteria certifications on EAL5 ([EAL5 explained on Wikipedia](https://en.wikipedia.org/wiki/Evaluation_Assurance_Level#EAL5:_Semiformally_Designed_and_Tested)).

You can use our API to access Enigma Bridge encryption service directly or transparently via production integrations. The set of operations available via the API includes:

* **encryption** - just encrypt, MAC, sign, ...;
* **re-encryption** - atomic re-encryption from transport (data in transit) to storage (data at rest) keys;
* **key management** - generate, unwrap, manage keys; 
* **payment systems** - re-encryption between transport and database keys, tokenization;
* **cloud migration** - use any subset of supported operations or one of our services built on top of the Enigma Bridge platform;
* **user authentication** - create user context, verify passwords and one-time passwords, reset passwords; or
* **random data generation** - using FIPS140-2 certified generators.

The native API is REST web-service. We also provide language bindings for simple integration. We currently support Javascript (client and node.js), Python and Java. This site provides description of API functions, including code examples in the dark area to the right. Tabs in the top right allow  switching between the programming language of the examples.

# Basics

Use of Enigma Bridge consists of four phases, irrespective of operations you want to use:

1. **user enrolment** - creating user account;
2. **API key creation** - obtaining API key with authorizations and also network connection details to a suitable Enigma Bridge instance;
3. **operation enrolment** - introduction of a new use request into Enigma Bridge; and
4. **data processing** - operational use.

Depending on the use case and limits on usage, all steps can be completed fully automatically within seconds. If a particular use case requires more strict control, step 1 will require manual approval.

Each of the steps can be represented by a single API command regardless of the operation you request. The result of the enrolment is a configuration string that uniquely identifies a new user object.



# Getting Started

We provide several language bindings. 

```python
"sudo pip install ebclient.py"
```

```javascript
"npm install -g ebclient.js"
or
"git clone https://github.com/EnigmaBridge/client.js"
```

```shell
"nothing required"
```

```java
"git clone https://github.com/EnigmaBridge/client.java"
```

```json
"nothing required"
```


Our goal is to make any library installation as simple as possible, and using standard deployment mechanisms wherever possible. 

# Register as Enigma Client

The first step for using the EB service is to create a client account. The client account forms the basis for your authorization towards Enigma Bridge. Once you obtain an account, you will learn your authentication details you can use to obtain API keys.

```java
"coming soon"
```

```json
"coming soon"
```

```shell
curl -i \
  -H "Content-Type: application/json" \
  -H "X-Auth-Token: public" \
  -X POST
  -d '{
         "nonce":"arbitrary string that will be included in the response",
         "version":1,
         "function":"create",
         "environment":"dev",
         "client":{
             "name":"name of the client - it should be unique",
             "authentication":"type",
             "type":"pre-defined type of clients",
             "token":"initial type authentication token"
         }
    }'\
  https://hut3.enigmabridge.com:8445/api/v1/client
```

```javascript
cfg = Configuration()
cfg.endpoint_register = Endpoint.url('https://hut3.enigmabridge.com:8445')
cfg.retry = SimpleRetry()

client_data_reg = {
    'name': 'name of the client - it should be unique',
    'authentication': 'type',
    'type': 'pre-defined type of clients',
    'token': 'initial type authentication token'
}
regreq = RegistrationRequest(client_data=client_data_reg, env=ENVIRONMENT_DEVELOPMENT, config=cfg)
regresponse = regreq.call()
print(regresponse)
```

```python
"coming soon"
```
We have created three end-points for client registration. We named them in memory of wartime sections of the Government Code and Cypher School (GC&CS) at [Bletchley Park](https://en.wikipedia.org/wiki/Bletchley_Park)

 - **hut3.enigmabridge.com** - Hut 3 translated, interpretated and distributed German Army (Heer) and Air Force (Luftwaffe) messages deciphered by Hut 6.
 - **hut6.enigmabridge.com** - Hut 6's task was to break German Army and Air Force [Enigma machine](https://en.wikipedia.org/wiki/Enigma_machine) cyphers.
 - **hut8.enigmabridge.com** - Hut 8 was solving German naval (Kriegsmarine) Enigma messages. The section was led initially by Alan Turing.

Automated registration requires knowledge of your use-case. In certain cases, we pre-define types of clients for automated deployments. For example, [AMI](https://en.wikipedia.org/wiki/Amazon_Machine_Image) available from [Amazon AWS Marketplace](https://aws.amazon.com/marketplace) will be assigned such a client type. 

<aside class="notice">
Availability of automated execution of client registration depends on the use case and required operation.
</aside>
<aside class="notice">
In some cases, it is possible to use a default "TEST" user and "TEST_API" key - these credentials are only available for demonstration purposes.
</aside>


#  API Key Enrolment 

> To authorize, you first have to get client authentication data.


```shell
curl -i \
  -H "Content-Type: application/json" \
  -H "X-Auth-Token: public" \
  -X POST
  -d '{
         "nonce":"arbitrary string that will be included in the response",
         "version":1,
         "function":"addapi",
         "authentication":("name" | "type" | "password" | "hotp" | "totp" | "ocra" | "challenge" | "signature"),
         "client":{
             "username":"as provided by function create",
             "authentication":"type",
             "password":"as provided",
         },
         "endpoint":{
             "ipv4":"IPv4",
             "ipv6":"IPv6",
             "email":"contact@mail",
             "country":"2 letter code, e.g. gb, us",
             "network":"network or IP space",
             "location":[0.34,10],
             "datacenter":"name",
             "hostid":"hostid",
             "productcode":"product code",
             "instancetype":"instance type"
         },
         "operations":["name","name2","nameX"]
    }'\
  https://hut3.enigmabridge.com:8445/api/v1/client
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace default values with proper ones.

### Query Parameters

Parameter |Type | Optional | Description
--------- | --- | -------- | -----------
ipv4 | string| true |  IPv4 address provided by the client software
ipv6 | string|true | IPv6 address provided by the client software
email |string|true| contact email address
country| string|true|2 character ISO country code 
network|string|true|string - name or IP space
location|array|true|geographic location as an array of two float numbers
datacenter|string|true|name / identification of the datacenter
hostid|string|true|host ID - e.g. AMI name (Amazon), server fqdn
productcode|string|true|identification of the client host if a VM
instancetype|string|true|another identification 

Enigma Bridge servers require API keys to allow access to the API. You can register a new API key either programmaticaly or via our support portal at [support portal](https://enigmabridge.freshdesk.com).

<aside class="notice">
You must replace <code>default key</code> with your personal API key.
</aside>

# Use


```json
"coming soon"
```

```python
"coming soon"
```

```shell
"coming soon
```

```javascript
"coming soon"
```

```java
"coming soon"
```


> The above command returns JSON structured like this:

```json
"coming soon"
```

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

