
# User Manager - Client Role

<aside class="notice">

User Manager is available on the following domain names managed by Enigma Bridge. Whenever we use <strong>hut</strong>, it has to be replased with one of these three domain names.
</aside>

User Manager domain names to replace **hut** in the text below are:

 1. hut3.enigmabridge.com
 1. hut6.enigmabridge.com
 1. hut8.enigmabridge.com

## Data Flow

The usual data-flow and sequence of actions is as follows:

 1. New client process (computer, server, software package) will get an Enigma Bridge client type - this can be done automatically for virtual machines configured by Enigma Bridge.
 1. This step requests creation of an Enigma Bridge client - i.e., a legal entity that will own APIkeys and will be authorized to use the Enigma Bridge service.
     1. The client process will use its EB type to request a new client account.
     1. The customer will request a new client account via our support system at: [https://enigmabridge.freshdesk.com](https://enigmabridge.freshdesk.com).
 1. Once the client ID and authentication details are available, the client process can request a new API key.
 1. The API key allows create of new user objects according to the client type definition.

## Register Client (client)

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
        "authentication":"type",
        "type":"test",
        "token":"token-assigned-to-client-type"
    }
}
```


The request uses the following URL: 

`https://hut:8445/api/v1/client`

###Parameters

name | optional |type| note
---- |--------- |----|----
nonce| NO| string|random string that will be returned back
version|NO| integer| must be 1
function|NO|string| must be "create"
environment|NO|string| must be "dev", "test", or "prod"
<strong>client</strong>|NO|sequence|contains information about the new client

###Structure of 'client'

name | optional |type| note
---- |--------- |----|----
name |NO|string|name provided by the user
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

###Structure of 'response'

name | optional |type| note
---- |--------- |----|----
username|NO|string|authentication username
password|YES|string|authentication password, depends on authenticationmethod
max_api_keys|NO|integer|max number of API keys the client can create
authenticationtype|NO|string|one of methods
status|NO|string|enabled, disabled, or removed
maxobjects|NO|integer|max number of objects this client can create


## Add API for Client (client)

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


###Structure of 'client'

name | optional |type| note
---- |--------- |----|----
authentication|NO|string|at the moment must be "password"
username|NO|string|as provided during registration
password|NO|string|as provided during registration

###Structure of 'endpoint'

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

###Structure of 'servers' each item

name | optional |type| note
---- |--------- |----|----
name|NO|string|
domain|NO|string|name of Enigma Bridge security domain
environment|NO|string|"dev" or "test" or "prod"
enrolendpoints|NO|array|array of {protocol,port}
useendpoints|NO|array|array of {protocol,port}

## View API for Client (client)

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

###Structure of 'client'

name | optional |type| note
---- |--------- |----|----
authentication|NO|string|at the moment must be "password"
username|NO|string|as provided during registration
password|NO|string|as provided during registration


Responses have the same format as for "addapi" requests.

## Enrol for DNS Domain (api)

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


###Structure of 'apidata'

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

## Get Challenge (api)

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

## Update Domain (api)

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
dnsdata|YES|array| an array of new DNS records that will be added to the domain, each record is defined by name, type, and value
authentication|NO|string|as requested with a challenge
response|NO|string|either the challenge itself or a derived value (e.g., OCRA)

### Structure of 'dnsdata'

name | optional |type| note
---- |--------- |----|----
type|NO|string|type of DNS record to create, currently on "TXT" supported
name|NO|string|name of the new record
value|NO|string|value of the new record
domain|YES|string|a particular domain to be updated, if not present, all relevant domains will be udpated, if none matches -> error



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
        "addresses": ["registered IP addresses "]
    }
}
```

## Install Status (api)

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
email|YES|email| contact details of whoever will be contacted

## Unlock Domain (api)

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


