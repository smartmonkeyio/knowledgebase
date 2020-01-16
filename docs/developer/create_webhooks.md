!> This guide is **under construction**, our monkeys are working hard to bring it to you as soon as possible.

# Building a webhook endpoint

**Receive events on your local test environment to build your integration.**

!> **TIP** We are into the development of a **sandbox for webhooks** to improve the
integration experience

With Highway Webhooks you can have the events triggered on your Highway account sent to your application.
In this guide we will overview the lifecycle of events and we will create a new webhook for a
custom application.

If you want a detailed explaination of what are webhooks visit [the previous page](/developer/webhooks.md).

## 1. Configure Webhook events

## 2. Set up your Server

```javascript
// content of index.js
const http = require("http");
const port = 3001;

const requestHandler = (request, response) => {
  console.log(Object.keys(request));
  response.end("Hello Node.js Server!");
};

const server = http.createServer(requestHandler);

server.listen(port, err => {
  if (err) {
    return console.log("something bad happened", err);
  }
  console.log(`server is listening on ${port}`);
});
```

## 3. Testing events

Once we have our small ingesting system we can attempt to generate and read an `Event`. To raise a `plan.created` we will need to create a new plan through the API.

```bash
curl "http://localhost:8090/api/v1/plan\
?private_key=priv_zWBZEPmNc9phMQ9oQ-Q0eSuD~UY4pMcfTZ4rWZ4SWVAnTsiUkUsTiTvJmJHc.Joa" \
```

This action will internally generate an `plan.created` event that will be sent to our mini server that will print an output like this.

```json
{
  "id": "5e208446c97efa37d4830b19",
  "user": "5e208444c97efa37d4830b16",
  "type": "plan.created",
  "created_at": "2020-01-16T15:41:58.195Z",
  "data": {
    "id": "5e208446c97efa37d4830b18",
    "user_id": "5e208444c97efa37d4830b16",
    "_version": 1,
    "created_at": "2020-01-16T15:41:58.192Z",
    "updated_at": "2020-01-16T15:41:58.192Z",
    "start_date": "2020-01-16T15:41:53.271Z",
    "tags": [],
    "status": "planning",
    "services": [],
    "routes": []
  }
}
```
