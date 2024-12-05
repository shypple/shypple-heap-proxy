## Heap Proxy Demo

### How to use this demo

This demo contains a Dockerfile that runs an NGINX reverse-proxy on the url and port `http://localhost:8087`. The container creates a test page at the root with contents in the index.html file. That file contains a Heap Tag, which will send requests through the proxy to Heap's APIs when configured with your `ENV_ID`. 

Once the tag is properly configured with your `ENV_ID`, reloading the page should automatically start sending requests through the proxy, due to Heap's auto-tracking feature.

The network tab within the developer tools of your browser should show the requests being sent. If everything is working correctly, you should receive 
`200` responses for each request.

## Build and run the image
Setting up the local path as a volume allows you to see changes to the test page without having to rebuild the docker contaiers. 

To build
```text
docker build -t nginx-proxy .
```

Run the image
```text
docker run -it --rm -p 8087:80 -v $(pwd):/usr/share/nginx/html --name nginx-proxy nginx-proxy
```

## Manually testing the proxy

You can also send a manual request through the proxy by using the following cUrl command:

```text
curl -X POST 'http://localhost:8087/api/capture/v2/identify' \
-H 'Content-Type: application/json' \
-d '{
    "env_id": "1772910386",
    "user_id": "5338921732914045",
    "identity": "test-email@heap.io",
    "time": "2024-12-03T16:04:26.979Z"
}'
```