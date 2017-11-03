# KeyChest 

<aside class="notice">
We are working on the first version of the API documentation and more functions will be added.
</aside>

Calls of the KeyChest API require an API key. Each client has to create its API key themselves but it can also request the 
KeyChest to generate one. We do not require this request to be authenticated to simplify automated enrolments.

## Register/check API key - GET

This call requests KeyChest to generate a suitable API key for a client. This key will be associated with an email address provided.

```java
"no data required"
```
```javascript
"no data required"
```
```python
"no data required"
```
```shell
curl https://keychest.net/api/v1.0/access/claim/me@myemail.com
```
```json
https://keychest.net/api/v1.0/access/claim/me@myemail.com
```

The endpoint of this call is:

`https://keychest.net/api/v1.0/access/claim/{email}`

### Parameters

name | optional |type| note
---- |--------- |----|----
email| NO| string|it must be a valid email address

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
{
    "status": "created",
    "user": "your@email.com",
    "api_key": "5b9b6aceb95011e7bb9f7fca73a26228"
}
```
```json
{
    "status": "created",
    "user": "your@email.com",
    "api_key": "5b9b6aceb95011e7bb9f7fca73a26228"
}
```
### Response

Response provides an api_key the client shall store and use for subsequent use of the KeyChest's API.

name | optional|type   |description
---- |---------|-------|---------
status | NO | string | the result of processing - "created" if successful
user|NO|string|email address as provided by the client
api_key|NO|string|a new API key - the length is between 16 and 64 characters and can contain digits, letters, and '-'


## Register/check API key - POST

This call requests KeyChest to check a provided API key, and register it if it is a new API key. This key will be associated with an email address provided.

```java
{
    "email":"your@email.com",
    "api_key":"5b9b6aceb95011e7bb9f7fca73a26228"
}
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
curl -s -X POST -H "Content-Type: application/json" -d '{"email":"your@email.com", "api_key":"5b9b6aceb95011e7bb9f7fca73a26228"}' https://keychest.net/api/v1.0/access/claim
```
```json
{
    "email":"your@email.com",
    "api_key":"5b9b6aceb95011e7bb9f7fca73a26228"
}
```

The endpoint of this call is:

`https://keychest.net/api/v1.0/access/claim`

### Parameters

name | optional |type| note
---- |--------- |----|----
email| NO| string|it must be a valid email address
api_key|NO|string|it must only contain letters, digits, and '-' and the length to be between 16 and 64 characters

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
{
    "status": "created",
    "user": "your@email.com",
    "api_key": "5b9b6aceb95011e7bb9f7fca73a26228"
}
```
```json
{
    "status": "created",
    "user": "your@email.com",
    "api_key": "5b9b6aceb95011e7bb9f7fca73a26228"
}
```
### Response

Response provides an api_key the client shall store and use for subsequent use of the KeyChest's API.

name | optional|type   |description
---- |---------|-------|---------
status | NO | string | the result of processing - "created" if successful
user|NO|string|email address as provided by the client
api_key|NO|string|a new API key - the length is between 16 and 64 characters and can contain digits, letters, and '-'

## Add domain name 

This API call adds a new domain name for monitoring. It can be used repeatedly for the same domain name and email address without any side-effects.

```java
{
    "domain":"fish.enigmabridge.com",
    "api_key":"5b9b6aceb95011e7bb9f7fca73a26228"
}
```
```javascript
see the RESTful column
```
```python
see the RESTful column
```
```shell
curl -s -X POST -H "Content-Type: application/json" -d '{"domain":"fish.enigmabridge.com", "api_key":"5b9b6aceb95011e7bb9f7fca73a26228"}' https://keychest.net/api/v1.0/servers/add
```
```json
{   
    "domain":"fish.enigmabridge.com",
    "api_key":"5b9b6aceb95011e7bb9f7fca73a26228"
}
```

The POST endpoint of this call is:

`https://keychest.net/api/v1.0/servers/add`

### Parameters

name | optional |type| note
---- |--------- |----|----
domain| NO| string|it must be a valid domain name
api_key|NO|string|a valid and registered API key



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
{
    "status": "success",
    "id": "671d1b10-bb1d-11e7-bae1-571d93cf1c53",
    "key": "da70d5e4864f57e910e66bd21c685b26",
    "domain": "fish.enigmabridge.com
}
```
```json
{
    "status": "success",
    "id": "671d1b10-bb1d-11e7-bae1-571d93cf1c53",
    "key": "da70d5e4864f57e910e66bd21c685b26",
    "domain": "fish.enigmabridge.com
}
```

### Response

Response returns the result of the processing and an additional information.

name | optional|type   |description
---- |---------|-------|---------
status|NO|string|"success" means a successful processing
id|NO|string|a handle for repeated calls if asynchronous integration is implemented
key|NO|string|internal representation of the domain name
domain|NO|string|cleaned domain name that was registered


## Check certificate expiration 

This API call returns information about certificate expiration for a given domain name. It will return detailed information for all IP addresses known for the given
domain name.

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
curl -s https://keychest.net/api/v1.0/servers/expiration/{domain}&api_key={api-key}
```
```json
see the RESTful column
```

The GET endpoint of this call is:

`https://keychest.net/api/v1.0/servers/expiration/{domain}`

### Parameters


name | optional |type| note
---- |--------- |----|----
domain| NO| string|it must be a valid domain name
api_key|NO|string|a valid and registered API key



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
{
    "domain": "fish.enigmabridge.com",
    "certificate_found":true,
    "renewal_due":false,
    "expired_found":false,
    "results": [
        {
            "ip": "2001:41c9:1:41d::131",
            "certificate_found": false,
            "certificate_sha256": null,
            "renewal_due": null,
            "expired": null,
            "renewal_utc": null,
            "last_scan_utc": 1509094412
        },
        {
            "ip": "46.43.0.131",
            "certificate_found": true,
            "certificate_sha256": "1aa7cda60ba61810321bedc4793fadc80f7ca6a3e328d484b0d5eb8a9ef230de",
            "renewal_due": true,
            "expired": false,
            "renewal_utc": 1510216500,
            "last_scan_utc": 1509099907
        }
    ],
    "status": "success"
}
```
```json
{
    "domain": "fish.enigmabridge.com",
    "certificate_found":true,
    "renewal_due":false,
    "expired_found":false,
    "results": [
        {
            "ip": "2001:41c9:1:41d::131",
            "certificate_found": false,
            "certificate_sha256": null,
            "renewal_due": null,
            "expired": null,
            "renewal_utc": null,
            "last_scan_utc": 1509094412
        },
        {
            "ip": "46.43.0.131",
            "certificate_found": true,
            "certificate_sha256": "1aa7cda60ba61810321bedc4793fadc80f7ca6a3e328d484b0d5eb8a9ef230de",
            "renewal_due": true,
            "expired": false,
            "renewal_utc": 1510216500,
            "last_scan_utc": 1509099907
        }
    ],
    "status": "success"
}
```

### Response

Response returns the result of the processing and detailed information for all IP addresses known for a given domain name.

name | optional|type   |description
---- |---------|-------|---------
domain|NO|string|requested domain name
certificate_found|NO|boolean|true if at least one certificate found for the domain name
renewal_due|NO|boolean|true if at least one certificate expires within 28 days
expired_found|NO|booean|true if at least one certificate expired
results|NO|JSON array|an array of records for each IP address
status|NO|string|"success" when processing was successful






