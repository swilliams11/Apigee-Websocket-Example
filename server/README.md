# WebSocket Server

You can deploy this WebSocket server to [CloudRun](https://cloud.google.com/run/docs/overview/what-is-cloud-run), which supports [WebSockets](https://cloud.google.com/run/docs/triggering/webhooks) natively, with the following command. 


```shell
gcloud auth login
gcloud config set project YOUR_PROJECT
gcloud run deploy websocket-server-demo --source . --region us-central1
```


### Test Connectivity to Cloud Run
You can use any Client to send a request to Cloud Run. I prefer to use Postman.  You must include an Authorization header, unless you deployed the Cloud Run service to allow all unauthenticated requests.

You can obtain an valid Identity Token to include in the Authorization header with the following `gcloud` command.

```shell
gcloud auth print-identity-token
```

Then you include the token in the HTTP Authorization header.
```
Authorization: Bearer ID_TOKEN
```

#### Curl
You can also test this with the following curl command.

 ```shell
export DOMAIN=YOUR_DOMAIN
curl -i -N --http1.1 -H "Connection: Upgrade" -H "Upgrade: websocket" -H "Sec-WebSocket-Key: $(openssl rand -base64 16)" -H "Sec-WebSocket-Version: 13" -H "Authorization: Bearer $(gcloud auth print-identity-token)" "https://$DOMAIN/" -w
```

###
Delete the Cloud Run server with the following command.
```shell
gcloud run services delete websocket-server-demo --region us-central1
```