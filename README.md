# BitcoinAPI

Before using our API, you must enable API usage from within the application.  Once you provide your IP address and accept the terms you will be given a one-time secret key and the web socket URL for you to connect to.  You are responsible for protecting this key.  If you lose this key, you will have to generate a new one.

The API works over web socket only.  All API messages (requests and responses/notification) contain a message type property "mt" which is of type string.

### Authentication

Before using any of the services, you must have authenticated your connection.  Authentication requires two requests:

1. Request a nonce word.
```json
{"mt":"Authenticate","mode":"Nonce"}
```

You will receive a message containing the nonce word with the status "OK".

```json
{"status":"OK","nonce":"<base64 encoded nonce word>","mt":"Authenticate"}
```
