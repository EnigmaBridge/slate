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
// coming soon
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
// JS coming soon
```

```python
from ebclient.eb_configuration import *
from ebclient.eb_registration import *
from ebclient.registration import *

cfg = Configuration()
cfg.endpoint_register = Endpoint.url('https://hut3.enigmabridge.com:8445')

client_data_reg = {
    'name': 'name of the client - it should be unique',
    'authentication': 'type',
    'type': 'pre-defined type of clients',
    'token': 'initial type authentication token'
    'email': self.email
}

regreq = RegistrationRequest(client_data=client_data_reg, env=ENVIRONMENT_DEVELOPMENT, config=cfg)
regresponse = regreq.call()

# results
username = regresponse['username']
password = regresponse['password']

print(regresponse)
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

```python
from ebclient.eb_configuration import *
from ebclient.eb_registration import *
from ebclient.registration import *

cfg = Configuration()
cfg.endpoint_register = Endpoint.url('https://hut3.enigmabridge.com:8445')

client_api_req = {
    'authentication': 'password',
    'username': username,
    'password': password
}

endpoint = {
    "ipv4": "123.23.23.23",
    "ipv6": "fe80::2e0:4cff:fe68:bcc2/64",
    "country": "gb",
    "network": "plusnet",
    "location": [0.34,10]
}

apireq = ApiKeyRequest(client_data=client_api_req, endpoint=endpoint, env=ENVIRONMENT_DEVELOPMENT, config=self.eb_config)
apiresponse = apireq.call()

# Results
apikey = apiresponse['apikey']
servers = apiresponse['servers']
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

## ProcessData

`ProcessData` is the basic call for using the UserObject - performing an operation
on the input data with the registered object (e.g., RSA decryption, AES decryption). 

`ProcessData` call can be tested also online in [ProcessData-demo].

```json
" Process data uses a communication encryption preprocessing. "
" For more details please refer to the Communication encryption documentation. " 
{"data":"Packet0_PLAINAES_00000396233a255a52b0ac083fc6a983730e8b6b9c8e74dcdc85f1343bf9f86543acd3f5bf024dc9a5364f17638250867403"}

" Typical `ProcessData` response is a JSON object like the following one."
" The communication encryption is in place - decryption has to be performed" 
"  in order to get the response clear text. Status 0x9000 stands for OK - result completed successfully."
{"function":"ProcessData","result":"0000d81cadea2e6499fdd1095988be916e0a3b3d706fd3a5b42b2ad6cdb751ac6a831fa030fc1f2885248388355ee5977cad_Packet0_PLAINAES_","nonce":"89c14e8802c42151","status":"9000","statusdetail":"success (ok)","version":"1.0"}
```

```python
from ebclient.process_data import ProcessData
from ebclient.uo import Configuration, Endpoint, SimpleRetry, UO
from ebclient.crypto_util import *

cfg = Configuration()
cfg.endpoint_process = Endpoint.url('https://site2.enigmabridge.com:11180')
cfg.api_key = 'API_TEST'

uo_aes = UO(uo_id=0xee01,
                 uo_type=0x4,
                 enc_key=from_hex('e134567890123456789012345678901234567890123456789012345678901234'),
                 mac_key=from_hex('e224262820223456789012345678901234567890123456789012345678901234'),
                 configuration=self.cfg)
 
pd = ProcessData(uo=self.uo_aes, config=self.cfg)
result = pd.call(from_hex('6bc1bee22e409f96e93d7e117393172a'))
print(from_hex('95c6bb9b6a1c3835f98cc56087a03e82') == result)
```

```shell
# Process data uses a communication encryption preprocessing.
# For more details please refer to the Communication encryption documentation. 
# Pure shell client is not yet planned.
```

```javascript
"use strict";
var eb = require("ebclient.js");
var sjcl = eb.misc.sjcl;

var settings = {
    host: "https://site2.enigmabridge.com:11180",
    enrollHost: "https://site2.enigmabridge.com:11182",
    requestMethod: "POST",
    requestTimeout: 30000,
    apiKey: "TEST_API",
    uoId: 'EE01',
    uoType: '4',
    aesKey: 'e134567890123456789012345678901234567890123456789012345678901234',
    macKey: 'e224262820223456789012345678901234567890123456789012345678901234'
};

var input = '6bc1bee22e409f96e93d7e117393172a';
var cl = new eb.client.processData(settings);
var promise = cl.call(input);

promise.then(function(data){
    console.log(data.data == '95c6bb9b6a1c3835f98cc56087a03e82');
}).catch(function(data){
    console.log(data);
});
```

```java
// Coming soon 
```


## Create User Object

* The first step before working with EnigmaBridge is creation of User Object.
* User object can represent e.g., RSA-2048 private key or AES encryption/decryption key.
* Each UserObject has one operation associated with it. For RSA-2048 key it is typically a decryption.
* Symmetric encryption key - e.g. AES encryption key has also one operation thus you need two user objects, with the same AES key inside, to perform both encryption and decryption.

```python
# Coming soon
```

```javascript
// Coming soon
```

```java
// Coming soon
```

```json
" Not supported with RAW JSON. Please, take a look at clients" 
```

```shell
# Not supported in shell. Please, take a look at clients
```

## Test

Skip.

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>


[ProcessData-demo]: https://expert.enigmabridge.com/test/demo.html
