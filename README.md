# Websocket example for Apigee

This is a very simple example of an Apigee API proxy that handles Websocket.

Contents:
* example ws server implemented in nodejs
* example API "pass-through" proxy that proxies Websocket

## Disclaimer

This example is not an official Google product, nor is it part of an
official Google product.

## Screencast

I've made a [screencast](https://youtu.be/TrGftwcI-Ps) walking through all this.


## Pre-requisites

* npm 8.30 or later
* node 16 or later
* Apigee X or hybrid

## Instructions

### Start the server

1. Open a terminal window

2. cd into the directory for the server
   ```
   cd server
   ```

3. install the prerequisites
   ```
   npm install
   ```

4. start the server:
   ```
   node ./server
   ```


### Run ngrok to expose the server to the internet

1. Download the ngrok tool from [ngrok.com](https://https://ngrok.com/).  This
   is a local application that exposes a local system to the internet,
   temporarily.

2. open a separate terminal window

2. within that terminal window, run ngrok to expose port 5950
   ```
   ngrok http 5950
   ```
   You will see an ngrok DNS name. This is the name at which you can
   reach your local server, over the internet.  An _example_ might be
   http://7959-50-35-82-179.ngrok.io  Your URL will be different!

### Modify, then Import and Deploy the API Proxy

1. modify the apiproxy to specify the ngrok endpoint that your ngrok command provided.

2. Zip up the apiproxy directory, and import the proxy via the UI.
  OR, use your favorite command line tool to import and deploy the API proxy.


### Invoke the Endpoint with Postman

1. Direct:

   Specify the connection url as the ngrok.io URL, something like
   wss://7959-50-35-82-179.ngrok.io  Your URL will be different!

   Click the connect button.  Then,
   send messages. Switch to the terminal window to see your nodejs server.
   It should indicate that it has received messages and sent responses.

2. via Apigee proxy:

   Disconnect in postman.

   Now specify the connection url as wss://YOUR-DNS-NAME-For-APIGEE/ws-passthrough

   Click the connect button.  Again,
   send messages. Switch to the terminal window to see your nodejs server.
   Again you should see log messages indicating activity.


### Teardown

Stop the ngrok server, with ^C in the window. Do the same for the nodejs server.
Undeploy the websocket proxy from your org.  All done!


## Updates
I made a minor update to the WebSocket Server to send an message to all clients every 5 seconds. - SW

## Apigee Proxy
I included a Cloud Run WebSocket Server example, which is included in the `server` folder.  
Complete the steps below to allow the Apigee proxy to call the Cloud Run Websockets Server. 

1. Create a Service Account to attach to the Apigee proxy named `apigee-websocket-example` and assign it the permission below.
* Cloud Run Invoker

2. Deploy the WebSocket Server using the README located in teh `server` folder. 

3. Update the Apigee Target Endpoint to the domain name of the Cloud Run Websocket server.

4. Update the Apigee Target Endpoint to include the Google Authentication element and include your Cloud Run domain name.
Review [Apigee Authentication examples](https://cloud.google.com/apigee/docs/api-platform/reference/api-proxy-configuration-reference#authentication-element-examples) for more details.

```shell
 <Authentication>
          <GoogleIDToken>
             <Audience>https://cloudrun-hostname.a.run.app/test</Audience>
          </GoogleIDToken>
</Authentication>
```


5. Deploy the Apigee proxy and test it the result with Postman.  

```shell
wss://YOUR-DNS-NAME-For-APIGEE/ws-passthrough
```