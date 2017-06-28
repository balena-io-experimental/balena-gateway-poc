### Resin Gateway POC

## Setup

## Add a few resin app environment variables

```
AWS_REGION = us-east-1
```
```
LAMBDA = lambda endpoint responsible for provisioning devices
```

## Deploying

* Clone this repository
* Add your resin applications endpoint
```
git remote add resin <username>@git.resin.io:<username>/<appName>.git
```

* Push your code
```
git push resin master
```

Once your code downloads to the device it will automatically run your app. If the certificates needed to communicate with AWS aren't present it will post it's RESIN_DEVICE_UUID to LAMBDA endpoint, if the certificates do exist it will run the app.

## API

Publish a message on topic

```
method: PUT
url: /api/publish/:topic
body: {
  value: '10',
  created_at: '1498588568783'
}
```

__Note__ The post body can be in whatever shape you like, it'll simply be merged with a gateway object and proxied straight to AWS IoT. So the final payload sent to AWS looks like:

```
{
  value: 10,
  created_at: 1498588568783,
  gateway: {
    uuid: process.env.RESIN_DEVICE_UUID,
    name: process.env.RESIN_DEVICE_NAME_AT_INIT
  }
}
```

## example request
```
curl -X POST -H "Content-Type: application/json" -d '{
	"value": "1",
	"created_at": "1498588568783"
}' "https://184021ab36fbf707583fcddef8d33961.resindevice.io/api/publish/temp"
```
