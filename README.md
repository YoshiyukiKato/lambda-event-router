# aws-lambda-event-router
URL-like inside routing for AWS lambda.

 ```js
const router = require("aws-lambda-event-router").createRouter();

router.get("/", (event, context, params) => {
  context.succeed("welcome");
});

router.post("/books/:id", (event, context, params) => {
  context.succeed("this is book " + params.id);
});

exports.handler = router;
 ```


## usage
Defining a router in lambda execution.

 ```js
router.get("/", (event, context, params) => {
  context.succeed("welcome");
});
```

The `router` requires `path` and `httpMethod` in an event object. `path` should be url-formed string. A minimum event object is:

```
{
  "path" : "/",
  "httpMethod" : "GET"
}
```

Finally, exports the router as handler: 

```js
exports.handler = router;
```

Then a response will be `welcome`.

### http methods
Now this library supports following methods.

- GET
- POST
- PUT
- DELETE
- HEAD
- PATCH
- OPTIONS

 To set a callback for a request, `router.get`, `router.post`, `router.put`, `router.delete`, `router.head`, `router.patch`, and `router.options` are provided.

```js
router.get("/", (event, context, params) => {
  context.succeed("get");
});

router.post("/", (event, context, params) => {
  context.succeed("post");
});

router.put("/", (event, context, params) => {
  context.succeed("put");
});

router.delete("/", (event, context, params) => {
  context.succeed("delete");
});

router.head("/", (event, context, params) => {
  context.succeed("head");
});

router.patch("/", (event, context, params) => {
  context.succeed("patch");
});

router.options("/", (event, context, params) => {
  context.succeed("options");
});
```

### params
`/path/:param_name` style parameter is supported. You can access a parameter by `req.params[param_name]`. For instance:

```js
router.get("/books/:id", (event, context, params) => {
  context.succeed("this is book " + params.id);
});
```

```
{
  "path" : "/books/1",
  "httpMethod" : "GET"
}
```

In this case, the response is `this is book 1`.

### use
A router imports sub-routers by `use` method.

```js
const router = Router.createRouter();
const subRouter = Router.createRouter();
subRouter.get("/hello", (event, context, params) => {
  context.succeed("hello from sub router");
});
router.use("/sub", subRouter);
```

```
{
  "path" : "/sub/hello",
  "httpMethod" : "GET"
}
```

## LICENSE
MIT