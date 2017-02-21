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
  - background
  - register_api
  - authorizations_api

search: true
---

# Introduction

**Last update: 21 February**

> This column shows examples of API functions, or details that are language specific. Use the tab names above to switch to the language / syntax you are interested in.

Welcome to Enigma Bridge API documentation and congratulations on taking steps to properly secure your systems from cyberattacks.

Enigma Bridge provides managed hardware encryption for a variety of applications. Our service is built on top of our game-changing encryption system fir secure hardware processors. This design ensures that your secrets can’t be extracted using any logical nor physical attacks. This protection of your data is provided by processors validated according to the FIPS 140-2 standard on **Level 3**. The **Level 3** means that the hardware device will erase secrets it stores when it detects an attack.

Side-channel, and logical attacks are also significantly limited. Assurance of this protection is provided by Common Criteria certifications on EAL5 ([EAL5 explained on Wikipedia](https://en.wikipedia.org/wiki/Evaluation_Assurance_Level#EAL5:_Semiformally_Designed_and_Tested)).

You can use our API to access Enigma Bridge encryption service directly or transparently via production integrations. The set of operations available via the API includes:

* **encryption** - just encrypt, MAC, sign, ...;
* **re-encryption** - atomic re-encryption from transport (data in transit) to storage (data at rest) keys;
* **key management** - generate, unwrap, manage keys; 
* **payment systems** - re-encryption between transport and database keys, tokenization;
* **cloud migration** - use any subset of supported operations or one of our services built on top of the Enigma Bridge platform;
* **user authentication** - create user context, verify passwords and one-time passwords, reset passwords; or
* **random data generation** - using FIPS140-2 certified generators.

The native API is REST web-service. We also provide language bindings for simple integration. We currently support Javascript (client and node.js), Python and Java. This site provides a description of API functions, including code examples in the dark area to the right. Tabs in the top right allow  switching between the programming language of the examples.

# Basics

Use of Enigma Bridge consists of four phases, irrespective of operations you want to use:

1. **user enrolment** - creating user account;
2. **API key creation** - obtaining API key with authorizations and also network connection details to a suitable Enigma Bridge instance;
3. **operation enrolment** - introduction of a new use request into Enigma Bridge; and
4. **data processing** - operational use.

Depending on the use case and limits on usage, all steps can be completed fully automatically within seconds. If a particular use case requires more strict control, step 1 will require manual approval.

Each of the steps can be represented by a single API command, regardless of the operation you request. The result of the enrolment is a configuration string that uniquely identifies a new user object.



# Getting Started

```python
"sudo pip install ebclient.py"
```

```javascript
"npm install -g ebclient.js"
or
"git clone https://github.com/EnigmaBridge/client.js"
```

```shell
# nothing required
```

```java
"git clone https://github.com/EnigmaBridge/client.java"
```

```json
"nothing required"
```

We provide several language bindings. 

 - Java - [client.java at GitHub](https://github.com/EnigmaBridge/client.java)
 - Javascript - [client.js at GitHub](https://github.com/EnigmaBridge/client.js)
 - Python - [client.py at GitHub](https://github.com/EnigmaBridge/client.py)

We have also implemented a PKCS#11 wrapper that uses Enigma Bridge for hardware security:

 - SoftHSM - [SoftHSMv1 at GitHub](https://github.com/EnigmaBridge/SoftHSMv1)


Our goal is to make any library installation as simple as possible, and using standard deployment mechanisms wherever possible. 

# Register as Enigma Client


The first step in using the EB service is to create a client account. The client account forms the basis for your authorization towards Enigma Bridge. Once you obtain an account, you will learn your authentication details you can use to obtain API keys.

Unless you intend to use an automation tool that takes care of the enrolment process. Please raise a support ticket at:

[Enigma Bridge Support System](https://enigmabridge.freshdesk.com/helpdesk/tickets/new)

We always require manual approval (via [our support system](https://enigmabridge.freshdesk.com)) when you request access to higher transactions rates, or when a formal audit requirements are in place (e.g., payment applications subject to PCI audits).

## Automation

> This section can only be used in certain cases, when it is possible to automate enrolment to the Enigma Bridge service. 

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

regreq = RegistrationRequest(client_data=client_data_reg, \
                             env=ENVIRONMENT_DEVELOPMENT, config=cfg)
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


#  Get API Key 

Once you have a client ID and authentication data, you are ready to create your first API key (one client ID will typically have several API keys to separate software clients). All operational requests need a valid API key, even though it is usually hidden in client-side libraries.

> The automation code is only available if Enigma Bridge provides automation for particular use case or application. All operational requests need a valid ClientID, even though it is usually hidden in client-side libraries.


## Automation

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
"comming soon"
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

apireq = ApiKeyRequest(client_data=client_api_req, endpoint=endpoint, \
                       env=ENVIRONMENT_DEVELOPMENT, config=cfg)
apiresponse = apireq.call()

# Results
apikey = apiresponse['apikey']
servers = apiresponse['servers']
```

> Make sure to replace default values with proper ones.

Automation is enabled with the RESTful API of our user management server. It means that requests are still being sent to one of the huts.

A successful response to a request for an API key will also return network connection details for operational requests. The user management server will automatically select the most suitable location of Enigma Bridge servers to provide required functionality and also to optimize network performance.

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

```json
" POST request:"
"    https://hut:11180/1.0/TEST_API0000001dcb0000300006/ProcessData/f2044d875bdef29d"
"    parameters are an Enigma Bridge handle, command (ProcessData), and a nonce"
"    Data has to be encrypted, see the text to the left for details."
" POST data:"
{
  "data": "0000BCFAADB2FC41A300961AE8741463B676954515B07456681E1E180A9B5FCE7BC93F5A49328DD79CCD96F995F345E3E60674DCA06FE4B1B39E164E08E713C0DB58CBF3EDBF73BADB78BA116418704E52EEB75F575F032BD082AF2585A5F5D3119AF8ACB5C2BAB9C982A7068BD7CB6EA4B9EE84126E1B0E5FE58D6567F1595CCDB6D1B43338DF87E582C50C6A0A0479E40DD63A4E36BD7A1066E8E4293076BF513DD2AE450307A571B5D11D965910074511CCE9FEDBFDF646EFF69B42BAF6A0A8A995A46EEC223D3E595AF40BA4DB1CC0E3D29E06E05DC835A7EF4166BFC4D281F249607D6B913A46B5F69C44BFCA35D5F53AEB6A048E38C064B837C99E571A5A928B2C8C83BB467FDB76BC76F3BECB944F24EEF9B376D6A7D9977AAF59770287F2",
  "function": "ProcessData",
  "version": "1.0",
  "nonce": "06AEAEADE624C651",
  "objectid": "TEST_API0000001dcb0000300006"
}


"RESPONSE"
" Typical `ProcessData` response is a JSON object like the following one."
" The communication encryption is in place - decryption has to be performed" 
"  in order to get the response clear text. Status 0x9000 stands for OK - result completed successfully."
{
  "function": "ProcessData",
  "result": "0000d81cadea2e6499fdd1095988be916e0a3b3d706fd3a5b42b2ad6cdb751ac6a831fa030fc1f2885248388355ee5977cad",
  "nonce": "89c14e8802c42151",
  "status": "9000",
  "statusdetail": "success (ok)",
  "version":"1.0"
}
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
            configuration=cfg)
 
pd = ProcessData(uo=self.uo_aes, config=cfg)
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

// Define basic EB configuration - shared among many requests
var config = new eb.client.Configuration({
    endpointProcess: 'https://site2.enigmabridge.com:11180',
    endpointEnroll: 'https://site2.enigmabridge.com:11182',
    apiKey: 'TEST_API',

    timeout: 30000,
    retry: {
        maxAttempts: 2
    }
});

// Define UserObject to operate with, for particular operation.
var settings = {
    config: config,
    uoId: 'EE01',
    uoType: '4',
    aesKey: 'e134567890123456789012345678901234567890123456789012345678901234',
    macKey: 'e224262820223456789012345678901234567890123456789012345678901234'
};

// Input can be either hex-coded string or SJCL bitArray (array of 32bit words / integers).
var input = '6bc1bee22e409f96e93d7e117393172a';
var cl = new eb.client.processData(settings);
var promise = cl.call(input);

promise.then(function(data){
    console.log(eb.misc.inputToHex(data.data) == '95c6bb9b6a1c3835f98cc56087a03e82');
}).catch(function(error){
    console.log(error);
});
```

```java
// Coming soon 
```

`ProcessData` is the *operational call* of the Enigma Bridge service. It performs an operation that is identified by a handle, e.g., `TEST_API000000ee010000000004`. The only other parameter it takes is data for processing.

As the data should be typically protected between the client application and EB servers, it has to be encrypted with communication keys. The actual protocol is again defined by the handle.

For more details of the communication encryption, please refer to the *Communication encryption documentation - Link coming soon*, or use one of the client implementations at [Enigma Bridge GitHub](https://github.com/EnigmaBridge). 

" Typical `ProcessData` response is a JSON object like the following one."
" The communication encryption is in place - decryption has to be performed" 
"  in order to get the response clear text. Status 0x9000 stands for OK - result completed successfully."


`ProcessData` call can be tested also online in [ProcessData-demo](https://enigmabridge.com/test).

<aside class="notice">Data sent to Enigma Bridge for processing has to be formatted. It is usually taken care of by one of our clients.</aside>
See links to implementations in [Javascript, Python, and Java above](#getting-started).
<aside class="notice">Each response form the Enigma Bridge service will contain a numerical code, as well as a short text with explanation of the error.</aside>


## Create User Object

* The first step before working with EnigmaBridge is creation of User Object.
* User object can hold e.g., RSA-2048 private key or AES encryption/decryption key.
* Each UserObject has one operation associated with it. For RSA-2048 key it is typically a decryption.
* Symmetric encryption key - e.g. AES encryption key has also one operation thus you need two user objects, with the same AES key inside, to perform both encryption and decryption.
* You can either import a key into the User Object if you have it (e.g., AES key known to you, RSA key) or you can let EnigmaBridge generate a
new key in the secure hardware so you benefit from the secure RNG and the fact you don't really know the secret value
thus nobody can steal it from you - it never leaves the secure hardware.

```python
from ebclient.process_data import ProcessData
from ebclient.create_uo import TemplateFields, KeyTypes, Environment, Gen
from ebclient.eb_create_uo import *
from ebclient.uo import Configuration, Endpoint, SimpleRetry, UO
from ebclient.crypto_util import *
from ebclient.eb_consts import UOTypes

cfg = Configuration()
cfg.endpoint_process = Endpoint.url('https://site2.enigmabridge.com:11180')
cfg.endpoint_enroll = Endpoint.url('https://site2.enigmabridge.com:11182')
cfg.api_key = 'API_TEST'

# Prepare key template - settings
tpl = {
    TemplateFields.environment: Environment.DEV
}

# AES key - provided by the client
# It is also possible to let EB generate the key.
keys = {
    KeyTypes.APP_KEY: from_hex('0123456789123456789012345678901234567890123456789012345678901234')
}


# Create AES-128 key for decryption - UOTypes.PLAINAESDECRYPT
cuo = CreateUO(configuration=self.cfg, tpl=tpl, \
              obj_type=UOTypes.PLAINAESDECRYPT, \
              keys=keys)
uo = cuo.create_uo()

# Process data - prepare object to use the new AES key
pd = ProcessData(uo=uo, config=cfg)

# Call the operation
result = pd.call(from_hex('95c6bb9b6a1c3835f98cc56087a03e82'))
```

```javascript
"use strict";
var eb = require("ebclient.js");
var sjcl = eb.misc.sjcl;

// Define basic EB configuration - shared among many requests
var config = new eb.client.Configuration({
    endpointProcess: 'https://site2.enigmabridge.com:11180',
    endpointEnroll: 'https://site2.enigmabridge.com:11182',
    apiKey: 'TEST_API',

    timeout: 30000,
    retry: {
        maxAttempts: 2
    }
});

// Create randomly generated AES-128 key, with DECRYPT operation associated
// with it, in development environment.
var cfg = {
    config: config,
    tpl: {
        "environment": eb.comm.createUO.consts.environment.DEV
    },
    keys: {
        app: {
            key: sjcl.random.randomWords(4)
        }
    },
    objType: eb.comm.createUO.consts.uoType.PLAINAESDECRYPT
};

var cl = new eb.client.createUO(cfg);
var promise = cl.call();

promise.then(function(data){
    // Call ProcessData on the input data - here we verify
    // it works as we expect.
    var aes = new sjcl.cipher.aes(cfg.keys.app.key);
    var input2 = '6bc1bee22e409f96e93d7e117393172a';

    // Pass data to the process data function - contains UserObject
    // which ProcessData parses and uses.
    var cfg2 = eb.misc.extend(true, {}, data);
    var cl2 = new eb.client.processData(cfg2);
    var promise2 = cl2.call(input2);

    promise2.then(function(data){
        var desiredOutput = aes.decrypt(eb.misc.inputToBits(input2));
        console.log(eb.misc.inputToHex(data.data));
        console.log(eb.misc.inputToHex(desiredOutput));
    }).catch(function(error){
        console.log(error);
    });

}).catch(function(error){
    console.log(error);
});
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

## Create User Object - RSA

RSA user object is a bit more specific as it returns the public part.

```python
from ebclient.process_data import ProcessData
from ebclient.create_uo import TemplateFields, KeyTypes, Environment, Gen
from ebclient.eb_create_uo import *
from ebclient.uo import Configuration, Endpoint, SimpleRetry, UO
from ebclient.crypto_util import *

cfg = Configuration()
cfg.endpoint_process = Endpoint.url('https://site2.enigmabridge.com:11180')
cfg.endpoint_enroll = Endpoint.url('https://site2.enigmabridge.com:11182')
cfg.api_key = 'API_TEST'

# Prepare key template - settings
tpl = {
    TemplateFields.environment: Environment.DEV
}

cuo = CreateUO(configuration=self.cfg, tpl=tpl)

# Create RSA-2048 private key
# Communication keys are not specified here -> will be generated now, returned in the response.
rsa_key = cuo.create_rsa(2048)

# Process data - prepare object to use the new RSA-2048 key for decryption
pd = ProcessData(uo=rsa_key.uo, config=cfg)

# Prepare data - 000..1. RAW operation is performed - padding has to be done
# localy on the client side. You can use PKCS1.5 padding implemented in the package.
input = ("\x00"*255) + "\x01"
result = pd.call(input)
```

```javascript
"use strict";
var eb = require("ebclient.js");
var sjcl = eb.misc.sjcl;

// Define basic EB configuration - shared among many requests
var config = new eb.client.Configuration({
    endpointProcess: 'https://site2.enigmabridge.com:11180',
    endpointEnroll: 'https://site2.enigmabridge.com:11182',
    apiKey: 'TEST_API',

    timeout: 30000,
    retry: {
        maxAttempts: 2
    }
});

// Specification: Create a new RSA-1024 key in the hardware, devel environment.
var cfg = {
    config: config,
    tpl: {
        "environment": eb.comm.createUO.consts.environment.DEV
    },
    bits: 1024
};

var cl = new eb.client.createUO(cfg);
var promise = cl.createRSA();

promise.then(function(data){
    // ProcessData demo: RSA decryption of the 00000..1
    // For RSA it holds: 1 ^ e mod N = 1
    var input2 = '0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001';
    var cfg2 = eb.misc.extend(true, {}, data);
    var cl2 = new eb.client.processData(cfg2);

    // rsaPrivateKey object contains also the public part of
    // the key: modulus and public exponent
    console.log(data.rsaPrivateKey);

    var promise2 = cl2.call(input2);
    promise2.then(function(data){
        console.log(eb.misc.inputToHex(data.data));
    }).catch(function(error){
        console.log(error);
    });

}).catch(function(error){
    console.log(error);
});
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


[ProcessData-demo]: https://expert.enigmabridge.com/test/demo.html
