node-mogreet
============

* A Simple Node Client for the Mogreet API *
<https://developer.mogreet.com/>

> Please note that the author of this software has no connection 
> with Mogreet. We just needed this stuff for our own projects. YMMV.

> RestClient is heavily adapted from *twilio-node*
> <https://github.com/twilio/twilio-node.git>

> Depends on request:
> <http://github.com/mikeal/request.git>

> > N.B.: request has issues with SSL under node > 0.9.2 <= ~0.10.5.
> > https calls to the Mogreet API fail with ECONNRESET errors.
> > Discussion here: http://stackoverflow.com/questions/11091974/ssl-error-in-nodejs

> > The fix (already included in this module) is to include:

```javascript
var https = require('https');
https.globalAgent.options.secureProtocol = 'SSLv3_method';
```

Configuration
-------------

You can find/create your unique API credentials at:
<https://developer.mogreet.com/dashboard/settings> (requires Mogreet dev account).

You can either instantiate the Client using your API credentials directly:

```javascript
var accountCID 	  = Your Account CID,
	authToken     = Your Auth Token,
	smsID         = Your Campaign ID,
	mmsID         = Your MMS Campaign ID,
	options       = {},

var client = require('node-mogreet')( accountCID, authToken, smsID, mmsID, options );
```

OR, nicer:

You can include the API credentials in your environment:

```
MOGREET_ACCOUNT_CID     = Your Account CID
MOGREET_AUTH_TOKEN      = Your Auth Token
MOGREET_SMS_CAMPAIGN_ID = Your Campaign ID
MOGREET_MMS_CAMPAIGN_ID = Your MMS Campaign ID
```

And instantiate the Client with:

```javascript
var client = require('node-mogreet')();
```

Basic Usage
-----------

You can use the Client to make any Mogreet API call.
A full list of API calls and options is available at:
<https://developer.mogreet.com/docs>

The standard Client request signature is:

```javascript
var options = {
	url: 'transaction.send'
	qs: {
		to          : '2125551212',
		message     : 'Hello World!'
		campaign_id : [SMS or MMS Campaign ID]

	}
}
client.request(options, function (error, data, response) {
	if (error) {
		// error - contains error information, if any
	}
	// data     - contains the server response as a JSON object
	// response - is the raw Mogreet response
});
```

Convenience Methods
-------------------

### Ping (system.ping)

```javascript
var options = {}
client.ping(options, function (error, data, response) {} );
```

### SMS (transaction.send)

```javascript
var options = {
	to: '2125551212',
	message: 'Hello World!',
	callback: '(URL for optional callback to your server)'
}
client.sendSms(options, function (error, data, response) {} );
```

### MMS (transaction.send)

```javascript
var options = {
	to: '2125551212',
	message: 'Hello World!',
	content_id: '(optional - for content already on a mogreet server)',
	content_url: '(optional - for content at any accessible URL)',
	callback: '(URL for optional callback to your server)'
}
client.sendMms(options, function (error, data, response) {} );
```

Testing
-------

At the moment there are no unit tests included, but there is a simple
test script included. You can adapt the server file in test.js
to run simple tests against your credentials.
