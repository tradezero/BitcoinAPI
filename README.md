# BitcoinAPI

Before using our API, you must enable API usage from within the application.  Once you provide your IP address and accept the terms you will be given a one-time secret key and the web socket URL for you to connect to.  You are responsible for protecting this key.  If you lose this key, you will have to generate a new one.

The API works over web socket only.  All API messages (requests and responses/notification) contain a message type property "mt" which is of type string.

### Authentication

Before using any of the services, you must have authenticated your connection.  Authentication requires two requests:

1. Request a nonce word.
```json
{"mt":"Authenticate","mode":"Nonce"}
```

You tell the service that you're requesting a nonce by specifying "Nonce" as the authentication mode.  You will receive a message containing the nonce word with the status "OK".

```json
{ "status":"OK", "nonce": "<base64 encoded nonce word>" ,"mt":"Authenticate"}
```

2. Send an HMAC SHA-256 signature of the nonce (represented as hex) using your secret key.

```json
{"mt": "Authenticate", "mode": "Signature", "nonce": "<base64 encoded nonce word>", 
"signature": "<signature hex>" }
```

If your signature matches, your connection will be authenticated and you will receive an Authenticate message with the status "Authenticated".

```json
{ "status":"Authenticated", "mt":"Authenticate" }
```

Once you are authenticated you can begin to send messages for the various services offered by the API.  Every message you send can have a "tag" property which will be given back as a "tag" property to allow you to pair a response or notification with your request.

### PlaceOrder

At this time we only support orders with counter "XBT" and base "USD", but that will change in the future.

The place order command takes the following attributes:

| Name       | Type       | Description       | Required |
| ---------- | ---------- | ----------------- | -------- |
| tag        | string     | An identifier for the request.  The tag will be returned in the response that corresponds to the request. | No |
| side       | string     | "buy" or "sell"   | Yes      |
| counter    | string     | The counter currency (only "XBT" is valid and is default | No  |
| counterUnit | string    | The unit of the counter.  Valid values are "Satoshi", "bXBT", "XBT", "BTC", "kXBT", "mBTC", "MXBT", "uBTC".  Note: "Satoshi" is equivalent to "bXBT" and "XBT" is equivalent to "BTC". | Yes |
| counterQuantity | integer | The amount of the counter in counterUnit units | Yes |
| base       | string     | The base currency (only "USD" is valid and is default)   | No      |
| baseUnit   | string     | Unit of the base (only "Cents" is valid and is default) | No |
| baseQuantity | integer | The quantity of base in baseUnit units (meaning depends on baseQuantityType) | Yes |
| baseQuantityType | string | baseQuantity semantics - "Unit" specifies the price of 1 counter, "Total" specifies the actual amount of base you want for the amount of counter specified | Yes |
| partialFill | boolean | partial fill or all or nothing (defaults to true) | No |


