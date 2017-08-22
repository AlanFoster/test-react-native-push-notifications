This is a simple react-native test for google cloud post notifications.

### Required Setup

Create a new Firebase project within https://console.firebase.google.com/

The specific credentials don't matter, as we will rely on the old Cloud Messaging token approach

Go to the settings of your new project and select `Cloud Messaging`

You should now have access to your `Server key` and `Sender ID`

### Running Android

Ensure that the senderID has been configured for android within `application.js` from your firecloud console

```js
senderID: "...",
```

Install the required dependencies with either `yarn` or `npm`:

```shell
npm install
```

Connect your device, and run:

```shell
react-native run-android
```

This should now install the react-native application onto your phone.

### Triggering push notifications


For this particular example we require the token returned from Google's cloud messaging service.
When running the app you can receive the token via logcat with:

```shell
> adb logcat | grep "\[pushnotifications\]"
08-22 13:05:47.609  1828  3616 I ReactNativeJS: '[pushnotifications] TOKEN:', { token: 'your token',
```

Via the command line you should be able to trigger push notifications to your device

```shell
server_key=from_cloud_messaging_console

curl --header "Authorization: key=$server_key" \
       --header Content-Type:"application/json" \
       https://gcm-http.googleapis.com/gcm/send \
       -d '{"data":{"message": "Hello world!"},"to":"your_logcat_token"}'
```

If it is successful you should see a notification on your phone, as well as the command line response:

```json
{"multicast_id":7283445387059431224,"success":1,"failure":0,"canonical_ids":0,"results":[{"message_id":"0:1503403906561737%ff91e4e1f9fd7ecd"}]}%
```
