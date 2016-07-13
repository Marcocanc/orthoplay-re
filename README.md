# Reversing Orthoplay
*Tested on OD-11 running V0.9.11*

![Intel Edison with Encoder for controlling Volume and Display](https://raw.githubusercontent.com/Marcocanc/orthoplay/master/images/edison_remote.jpg)
## Endpoints

Communication to the OD-11 goes through Websockets.
The OD-11 is running AutobahnPython 0.5.14 twice:

- PROD: `http://od-11.local/ws`
- DEV: `http://od-11.local:8081`

##Initiating communication
To start communicating with the device, send the `global_join` message along with the protocol version you are going to use.
At the time of writing this, 0.4 seems to be the newest.

The message will look like this: `{"protocol_major_version":0,"protocol_minor_version":4,"action":"global_join"}`

##Registering/configuring your remote
Next you will need to register your remote with the following message:
``{"color_index":0,"name":"guest","realtime_data":true,"uid":"YOUR_REMOTE_UID","action":"group_join"}``

In the js implementation, a unique ID for the remote is generated with the following code
```javascript
var uid = "uid-" + Math.floor(1e8 * Math.random());
```

##Staying Connected
In order to stay connected, it is required to send a ping with a timestamp every once in a while (The web client does this every 5 seconds)
```javascript
var createPing = function() {
	return {
		value: (new Date).getTime() % 1e6,
		action:"speaker_ping"
	}
}
```
The ping message will look like this `{"value":913577,"action":"speaker_ping"}`


###Changing the Volume
You can change the volume of your speaker group by sending the `group_change_volume` message along with the amount by which the volume should change

`{"amount":-1,"action":"group_change_volume"}`

