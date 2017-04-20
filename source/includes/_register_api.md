# Enrolment Overview

<aside class="notice">

User Manager is available on the following domain names managed by Enigma Bridge. Whenever we use <strong>hut</strong>, it has to be replaced with one of these three domain names.
</aside>

User Manager domain names to replace **hut** in the text below are:

 1. hut3.enigmabridge.com
 1. hut6.enigmabridge.com
 1. hut8.enigmabridge.com

## Data Flow

The usual data-flow and sequence of actions is as follows:

 1. New client (computer, server, software package) will get an Enigma Bridge client type - this can be done automatically, e.g., for virtual machines configured by Enigma Bridge, or when you use a provided enrolment tool (script, application).
 1. This step requests creation of an Enigma Bridge client - i.e., a legal entity that will own API keys and will be authorized to use the Enigma Bridge service.
     1. The client process will use its EB type to request a new client account: this always involves a register client API call; optionally also `Get Client Auth Type` and `Get Client Auth`.
     1. The customer will request a new client account via our support system at: [https://enigmabridge.freshdesk.com](https://enigmabridge.freshdesk.com).
 1. Once the client ID and authentication details are available, the client process can request a new API key.
 1. The API key allows create of new user objects according to the client type definition.

# Client Endpoint - RESTful API

## Get Client Authentication

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"nonce",
    "version":1,
    "function":"getauth",
    "environment":"dev",
    "client":{
        "type":"test"
    }
}
```

This is a simple API call that returns the type of authentication required for a particular client type.

The request uses the following URL: 

`https://hut:8445/api/v1/client`

### Parameters

name | optional |type| note
---- |--------- |----|----
nonce| NO| string|random string that will be returned back
version|NO| integer| must be 1
function|NO|string| must be "create"
environment|NO|string| must be "dev", "test", or "prod"
<strong>client</strong>|NO|sequence|must contain "type" of the new client that will be registered

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
  "version": 1,
  "error": "success (ok)",
  "status": 36864,
  "nonce": "my nonce",
  "timestamp": 1475077216548,
  "response": {
    "type": "test",
    "authentication": [
      {
        "method": "authentication type1",
        "init": false,
        "initdata": []
      }, 
      {
        "method": "authentication type2",
        "init": true,
        "initdata": [ "data required in initialization" ]
      }
    ]
  }
}
```

### Response

Response shows allowed authentication methods and lists data items required for each of the authentication methods for the "Init Client Authentication" method.

name | optional|type   |description
---- |---------|-------|---------
method|NO|string|one of the values from the table below
init|NO|boolean|true if Init Client Authentication is required
initdata|NO|array of strings|values can be from the set of "email", "mobile", and "rsa"


The response will return an array of possible authentication types. It has to contain at least one element. Possible values are listed in the following table. Please note, that the requestor always has to know its type. 

name | description
---- |--------- 
type | client has to know its type and a token, which is fixed for all new requestors of the given type 
name | authentication is done via a correct name; the requestor has to know the client type it wants to be
token | the `type`, `name`, and a `token` are required; the `token` is unique to a particular requestor and it remains constant during repeated registrations
password | authentication requires a correct `type`, `name`, and `password`
challenge | authentication requires a correct one-time challenge, which is provided to the requestor via a separate channel
hotp | using an HOTP value, according to IETF RFC 4226
totp | using an TOTP value, according to IETF RFC 6238
ocra | using an OCRA value, according to IETF RFC 6287
signature | authentication with a digital signature 


## Init Client Authentication 

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"nonce",
    "version":1,
    "function":"initauth",
    "environment":"dev",
    "client":{
        "type": "test",
        "method": "name of authentication method",
        "email": "email@address",
        "mobile": "mobile phone number with country code",
        "rsa": "RSA public key"
    }
}
```

If registration of a new client requires the server to provide or initialize an authentication process, this API call will provide the server with all the necessary information.

name|optional|type|note
----|-------|----|-----
type|NO|string|a client type
method|NO|string|one of the names returned by "Get Client Authentication"
email|YES|string - email|email address for authentication
mobile|YES|string - number|mobile number with country code, only digits and whitespaces
rsa|YES|string|base64 public key



### Response

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
  "version": 1,
  "error": "success (ok)",
  "status": 36864,
  "nonce": "my nonce",
  "timestamp": 1475077234509,
  "response": {
    "authdata": {"if appropriate, otherwise an empty sequence"},
    "clientid": "fqwbev0cid5hxmgt90nk1it0osr98ama0ey1bc10"
  }
}
```

Response can be two-fold:

  - input for creating authentication data, if any;
  - requesting the server to use an alternative communication channel to provide the requestor with data out-of-band 

## Register Client

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"nonce",
    "version":1,
    "function":"create",
    "environment":"dev",
    "client":{
        "name":"my test name",
        "clientid":"fqwbev0cid5hxmgt90nk1it0osr98ama0ey1bc10 - from initauth API call",
        "authentication":"type",
        "type":"test",
        "token":"token-assigned-to-client-type"
    }
}
```




The request uses the following URL: 

`https://hut:8445/api/v1/client`

### Parameters

name | optional |type| note
---- |--------- |----|----
nonce| NO| string|random string that will be returned back
version|NO| integer| must be 1
function|NO|string| must be "create"
environment|NO|string| must be "dev", "test", or "prod"
<strong>client</strong>|NO|sequence|contains information about the new client

### Structure of 'client'

name | optional |type| note
---- |--------- |----|----
name |NO|string|name provided by the user
clientid|YES|string|string provided by Init Client Authentication, if called
authentication|NO|string| must be "type"
type|NO|string| Name of EB client type (pre-defined)
token|YES|string|authentication token for the client type

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
  "version": 1,
  "error": "success (ok)",
  "status": 36864,
  "nonce": "my nonce",
  "timestamp": 1475077216548,
  "response": {
    "username": "dc5c2081-2a13-4566-ac8d-592ee955dc48",
    "password": "106..............571e0f53",
    "max_api_keys": 5,
    "authenticationType": "PASSWORD",
    "status": "ENABLED",
    "maxobjects": 5
  }
}
```

This is a typical response. The new items are:

name | optional |type| note
---- |--------- |----|----
status|NO|integer|status code - see Error codes
error|NO|string|text explaining the error code
timestamp|NO|long|timestamp of the response
<strong>response</strong>|NO|sequence|data returned to the request

### Structure of 'response'

name | optional |type| note
---- |--------- |----|----
username|NO|string|authentication username
password|YES|string|authentication password, depends on authenticationmethod
max_api_keys|NO|integer|max number of API keys the client can create
authenticationtype|NO|string|one of methods
status|NO|string|enabled, disabled, or removed
maxobjects|NO|integer|max number of objects this client can create


## Add API to Client

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"d",
    "version":1,
    "function": "addapi",
    "environment":"dev",
    "client":{
        "authentication": "password",
        "username": "dc5c2081-2a13-4566-ac8d-592ee955dc48",
        "password": "1067...................71e0f53"
    },
    "endpoint":{
        "country":"gb"
    }
}
```

The request uses the following URL:

`https://hut:8445/api/v1/client`


name | optional |type| note
---- |--------- |----|----
nonce|NO|string|
version|NO|integer|must be 1
function|NO| must be "addapi"
environment|NO|must be "dev", "test", or "prod"
client|NO|sequence|
endpoint|NO|sequence| properties of the client server to identify best Enigma Bridge servers


### Structure of 'client'

name | optional |type| note
---- |--------- |----|----
authentication|NO|string|at the moment must be "password"
username|NO|string|as provided during registration
password|NO|string|as provided during registration

### Structure of 'endpoint'

name | optional |type| note
---- |--------- |----|----
country|YES|string|ISO country code
email|YES|string|
productcode|YES|string|
instancetype|YES|string|
network|YES|string|
location|YES|pair of float|
ipv4|YES|string|
ipv6|YES|string|
hostid|YES|string|



```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
  "version": 1,
  "error": "success (ok)",
  "status": 36864,
  "nonce": "d",
  "timestamp": 1475078668177,
  "response": {
    "apikey": "0b75.................d057593b",
    "servers": [
      {
        "name": "gb_cambridge_damselfly",
        "domain": "default",
        "environment": "dev",
        "enrolEndpoints": [
          {
            "protocol": "tcp",
            "port": 11112
          },
          {
            "protocol": "https",
            "port": 11182
          }
        ],
        "useEndpoints": [
          {
            "protocol": "tcp",
            "port": 11110
          },
          {
            "protocol": "https",
            "port": 11180
          }
        ],
        "fqdn": "site2.enigmabridge.com"
      }
    ],
    "operations": [
      {
        "status": "enabled",
        "operation": 4,
        "operationname": "basic_aes128_encrypt"
      },
      {
        "status": "enabled",
        "operation": 5,
        "operationname": "basic_rsa1024_decrypt"
      },
      {
        "status": "enabled",
        "operation": 6,
        "operationname": "basic_rsa2048_decrypt"
      }
    ],
    "status": "enabled"
  }
}
```
A response example is on the right. The new items are:

name | optional |type| note
---- |--------- |----|----
apikey|NO|string|apikey used to authenticate requests
<strong>servers</strong>|NO|array|connection details for Enigma Bridge servers
operations|NO|array|list of operations available for the API key

### Structure of 'servers' each item

name | optional |type| note
---- |--------- |----|----
name|NO|string|
domain|NO|string|name of Enigma Bridge security domain
environment|NO|string|"dev" or "test" or "prod"
enrolendpoints|NO|array|array of {protocol,port}
useendpoints|NO|array|array of {protocol,port}

## View API of Client

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"my nonce",
    "version":1,
    "function": "showapi",
    "environment":"dev",
    "client":{
        "authentication": "password",
        "username": "dc5c2081-2a13-4566-ac8d-592ee955dc48",
        "password": "1067....................1e0f53"
    },
    "apidata":{
        "apikey":"0b75f...............d057593b"
    }
}
```

The request uses the following URL:

`https://hut:8445/api/v1/client`

Clients can repeatedly request information about their API keys.

name | optional |type| note
---- |--------- |----|----
nonce|NO|string|
version|NO|integer|must be 1
function|NO| must be "showapi"
environment|NO|must be "dev", "test", or "prod"
client|NO|sequence|
apidata|NO|string|sequence of just "apikey" - string

### Structure of 'client'

name | optional |type| note
---- |--------- |----|----
authentication|NO|string|at the moment must be "password"
username|NO|string|as provided during registration
password|NO|string|as provided during registration


Responses have the same format as for "addapi" requests.

# APIkey Endpoint - RESTful API


## Client IP Address

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"nonce",
    "version":1,
    "function":"clientip"
}
```

The request uses the following URL:

`https://hut:8445/api/v1/apikey`

This function is generally available, it doesn't require any authentication of the client.


### Response

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
  "version": 1,
  "error": "success (ok)",
  "status": 36864,
  "nonce": "my nonce",
  "timestamp": 1475077216548,
  "response": {
      "ipv4":"11.11.11.11",
      "ipv6":"fe80::bae8:56ff:fe2e:ce68"
  }
}
```

This is a typical response. The new items are:

name | optional |type| note
---- |--------- |----|----
ipv4|NO|string|it can be NULL
ipv6|NO|string|it can be NULL


## List Operations

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"nonce",
    "version":1,
    "function":"listops",
    "environment":"dev",
    "apidata":{
        "username":"dc5c2081-2a13-4566-ac8d-592ee955dc48",
        "apikey":"0b75f...............d057593b"
    }
}
```

The request uses the following URL: 

`https://hut:8445/api/v1/apikey`

### Parameters

name | optional |type| note
---- |--------- |----|----
username| NO| string|client username as provided during registration
apikey|NO| string| apikey as provided during registration
function|NO|string| must be "listops"
environment|NO|string| must be "dev", "test", or "prod"

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
  "version": 1,
  "error": "success (ok)",
  "status": 36864,
  "nonce": "my nonce",
  "timestamp": 1475077216548,
  "response": {
      "operations":{
        "name": {
            #sequence with the operation definition
            #actual content is beyond the scope of
            #this documentation at the moment
        }
      }
  }
}
```

This is a typical response. The new items are:

name | optional |type| note
---- |--------- |----|----
status|NO|integer|status code - see Error codes
error|NO|string|text explaining the error code
timestamp|NO|long|timestamp of the response
response|NO|sequence|<strong>operations</strong>

### Structure of 'operations'

name | optional |type| note
---- |--------- |----|----
name|NO|sequence|definition for new operations


## Enrol for DNS Domain 

<aside class="notice">
<strong>api</strong> endpoint does not use client password. This password is not needed for APIkey related calls and should not be present in the client endpoint.
</aside>

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"my nonce",
    "version":1,
    "function": "enroldomain",
    "environment":"dev",
    "apidata":{
        "username":"dc5c2081-2a13-4566-ac8d-592ee955dc48",
        "apikey":"0b75f...............d057593b",
        "certificate":"base64 encoded X.509 certificate"
    }
}
```

The request uses the following URL:

`https://hut:8445/api/v1/apikey`


### Structure of 'apidata'

name | optional |type| note
---- |--------- |----|----
username|NO|string|client username as provided during registration
apikey|NO|string|apikey as provided during registration
certificate|NO|string|base64 encoded certificate - vehicle to provide public key

### Response 

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "version": 1,
    "error": "success (ok)",
    "status": 36864,
    "nonce": "my nonce",
    "timestamp": 1475078668177,
    "response": {
        "domain": "pki.enigmabridge.com",
        "controllers": [
            {
                "name": "ns.enigmabridge.com"
            }
        ],
        "status":"enabled"
    }
}
```

## Get Challenge

<aside class="notice">
<strong>api</strong> endpoint does not use client password. This password is not needed for APIkey related calls and should not be present in the client endpoint.
</aside>

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"my nonce",
    "version":1,
    "function": "getdomainchallenge",
    "environment":"dev",
    "apidata":{
        "ipv4":"IPv4 provided by the client",
        "ipv6":"IPv6 provided by the client",
        "private": true,
        "username":"dc5c2081-2a13-4566-ac8d-592ee955dc48",
        "apikey":"0b75f...............d057593b"
    }
}
```

The request uses the following URL:

`https://hut:8445/api/v1/apikey`

name | optional |type| note
---- |--------- |----|----
username|NO|string|client username as provided during registration
ipv4|YES|string|ipv4 that will be tested
ipv6|YES|string|ipv6 that will be tested
private|NO|boolean|default value is "false"; if true, the supplied IP addresses are private and will not be used for any verification

If IP addresses are marked as private, we will check whether they are from the expected range. I.e., fc00::/7 for IPv6 and 10.0.0.0/8, 172.16.0.0/12, or 192.168.0.0/16 for IPv4.

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "version": 1,
    "error": "success (ok)",
    "status": 36864,
    "nonce": "my nonce",
    "timestamp": 1475078668177,
    "response": {
        "challenge": "a random string for authenticated requests",
        "authentication": "signature"
    }
}
```

The response to these requests provides a **challenge** that has to be used to authenticate requests like **Update Domain**.

## Update Domain

<aside class="notice">
<strong>api</strong> endpoint does not use client password. This password is not needed for APIkey related calls and should not be present in the client endpoint.
</aside>

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"my nonce",
    "version":1,
    "function": "updatedomain",
    "environment":"dev",
    "apidata":{
        "username":"dc5c2081-2a13-4566-ac8d-592ee955dc48",
        "apikey":"0b75f...............d057593b",
        "dnsdata": [{
                        "type":"txt",
                        "domain":"walsall1.pki.enigmabridge.com", 
                        "name":"_acme-challenge", 
                        "value":"random string"}
                   ],
        "authentication":"signature",
        "response":"value derived from previously requested challenge"
    },
    "signature":{
        "payload":"apidata-base64",
        "value":"string-base64-signature"
    }
}
```

The request uses the following URL:

`https://hut:8445/api/v1/apikey`

name | optional |type| note
---- |--------- |----|----
apidata|YES|string|not needed if signature is present
signature|YES|string|only if method is "signature"; 


### Structure of 'apidata'

name | optional |type| note
---- |--------- |----|----
username|NO|string|as assigned
apikey|NO|string|as assigned
dnsdata|YES|array| an array of new DNS records that will be added to the domain, each record is defined by a name, type, and value
authentication|NO|string|as requested with a challenge
response|NO|string|either the challenge itself or a derived value (e.g., OCRA)

### Structure of 'dnsdata'

We currently support "TXT" and "CNAME" DNS records only.

name | optional |type| note
---- |--------- |----|----
type|NO|string|"TXT"
name|NO|string|name of the new record
value|NO|string|value of the new record, 
domain|YES|string| a particular domain to be updated, if not present, all relevant domains will be udpated, if none matches -> error

name | optional |type| note
---- |--------- |----|----
type|NO|string|"CNAME"
name|NO|string|name of the new subdomain
domain|YES|string| a particular domain to be updated, if not present, all relevant domains will be udpated, if none matches -> error



```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "version": 1,
    "error": "success (ok)",
    "status": 36864,
    "nonce": "my nonce",
    "timestamp": 1475078668177,
    "response": {
        "domains": ["assigned domain name"],
        "addresses": ["registered IP addresses "],
        "privateIP": false,
        "status": "enabled"
    }
}
```

### Structure of 'response'

name | optional |type| note
---- |--------- |----|----
domains|NO|array of DNS names | a list of domain names registered for the API key
addresses|NO|array of IP addresses | a list of IPv6 and IPv4 IP addresses associated with the API key
privateIP|NO|boolean|returns true if the API key host is inside a private network
status|NO|string|a status of the DNS registration (disabled, enabled, removed, created, unlocked, not_assigned

## Install Status

This function allows the API key announce its current status and communicate any additional context information that may be needed for support or communication requiring an independent channel for security.

<aside class="notice">
<strong>api</strong> endpoint does not use client password. This password is not needed for APIkey related calls and should not be present in the client endpoint.
</aside>


```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"my nonce",
    "version":1,
    "function": "installstatus",
    "environment":"dev",
    "apidata":{
        "username":"dc5c2081-2a13-4566-ac8d-592ee955dc48",
        "apikey":"0b75f...............d057593b"
    },
    "statusdata":{
        "password":"very secret password",
        "status":36864,
        "error":"success (ok)",
        "duration":1800,
        "detail":"logs, details as a JSON message",
        "key":"base64encoded key blob",
        "email":"important@top.world"
    }
}
```

The request uses the following URL:

`https://hut:8445/api/v1/apikey`

This function will update status of the APIkey consumer/host. This information can be used for further communication with clients or contact clients. 

The structure and particular meaning of items in "statusdata" depends on the client type.

name | optional |type| note
---- |--------- |----|----
username|NO|string|as assigned
apikey|NO|string|as assigned


### Structure of 'statusdata'

name | optional |type| note
---- |--------- |----|----
password|YES|string|generated on the apikey host
status|NO|integer| status code
error|NO|string|provided by the apikey
duration|YES|integer|duration of an operation in seconds
key|YES|string|any key that we need to pass on
detail|YES|string|JSON message with details we need to store
email|YES|email| contact details of whoever will be contacted

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "version":1,
    "error": "success (ok)",
    "status": 36864,
    "nonce":"my nonce",
    "timestamp": 1475078668177,
    "response": null
}
```

The response to this query is just a result of the processing.


## Unlock Domain

<aside class="notice">
<strong>api</strong> endpoint does not use client password. This password is not needed for APIkey related calls and should not be present in the client endpoint.
</aside>

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"my nonce",
    "version":1,
    "function": "unlockdomain",
    "environment":"dev",
    "apidata":{
        "username":"dc5c2081-2a13-4566-ac8d-592ee955dc48",
        "apikey":"0b75f...............d057593b"
    }
}
```

The request uses the following URL:

`https://hut:8445/api/v1/apikey`

This function will "unlock" the domain record for the given API key so it can be
assigned to a new IP address.

name | optional |type| note
---- |--------- |----|----
apidata|NO|string|authentication of the client


### Structure of 'apidata'

name | optional |type| note
---- |--------- |----|----
username|NO|string|as assigned
apikey|NO|string|as assigned


```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "version": 1,
    "error": "success (ok)",
    "status": 36864,
    "nonce": "my nonce",
    "timestamp": 1475078668177,
    "response": {
        "status": "status of the domain in EB backend"
    }
}
```





## Send Log Data

<aside class="notice">
<strong>api</strong> endpoint does not use client password. This password is not needed for APIkey related calls and should not be present in the client endpoint.
</aside>

```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "nonce":"my nonce",
    "version":1,
    "function": "sendlog",
    "environment":"dev",
    "apidata":{
        "username":"dc5c2081-2a13-4566-ac8d-592ee955dc48",
        "apikey":"0b75f...............d057593b"
    },
    "effort":{
        "preimage":"407696;1487584132;34.249.220.146;Trunk;854843",
        "secondpreimage":"hex value",
        "collision": 20
    },
    "log": "text data, with log events"
}
```

The request uses the following URL:

`https://hut:8445/api/v1/apikey`

This function will store log data from the endpoint. If the endpoint has its API key, it should be used as it may be used as a protection against DDoS attacks. However, its presence is optional.

If the "apidata" is not present or invalid, the "effort" item must be present. Otherwise the request will be unsuccessful. The "effort" is not required if valid "apidata" is provided. The effort's goal is to slow down DDoS attacks by requiring clients to comput SHA-1 function collisions.


name | optional |type| note
---- |--------- |----|----
apidata|YES|string|authentication of the client
effort|NO|sequence|


### Structure of 'apidata'

name | optional |type| note
---- |--------- |----|----
username|NO|string|as assigned
apikey|NO|string|as assigned


### Structure of 'effort'

name | optional |type| note
---- |--------- |----|----
preimage|NO|string|unique identifier of the request, the format is random-number ";" timestamp ";" IP_address ";" client_version ";" nonce. All numbers are in decadic form.
collision|YES|int|how many bits of collision have been computed


The server will compare the strength of the collision and if not satisfactory, the request will be declined.




```java
see the RESTful column
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
see the RESTful column
```
```json
{
    "version": 1,
    "error": "success (ok)",
    "status": 36864,
    "nonce": "my nonce",
    "timestamp": 1475078668177,
    "response": {
        "collision": integer,
        "reference": "reference number of the log record"
    }
}
```

name | optional |type| note
---- |--------- |----|----
collision|YES|string|the length of the collision required, only present if provided collision is not sufficient
reference|NO|string| it canbe empty string if errir



If the request is declined because of insufficent collision strength, the required length/strength is present in the result



