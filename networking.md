## Networking

[**Go back to Summary**](README.md#summary);

- [CORS](#cors);
- [Requests](#requests);
- [JSONP](#jsonp);

---------

### CORS

- [CORS - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

By default, browsers reject resources comming from origins that are not the current one. For example, if the host website is `foo.com` and it requests a script from `moo.com`, this request is rejected, as they are not from the same origin.

**CORS**, or Cross-Origin Resource Sharing, is a mechanism that allow requests from different origins.

#### Requests
When performing a request, the browser sends headers with the origin to the target resource. It then, returns a header property of `Access-Control-Allow-Origin`. Its value has to match the origin or use the "all" value, `*`. If the value is different than the origin, then the request response is rejected.

For GET (HEAD and POST) requests, only one back-and-forth response is needed (they are "simple requests"). But for other methods and header properties that might change the server, an additional step is added upfront, which is called the _preflight request_.

A preflight send a OPTIONS request, with the origin and the `Access-Control-Request-Method`, whose value can be PUT, DELETE, POST etc. The response's values have to match the origin's ones.

If the response is successful, a subsequent request with the chosen method will be performed.

An important note here is that if the request has failed because of a CORS issue, then the **server** has to change its response headers.

### JSONP

- [JSONP - W3Schools](https://www.w3schools.com/js/js_json_jsonp.asp)

JSONP is a solution for GET request that does not need to handle with cross origin resources. It is a `.js` file that calls a function and returns an array of objects (similiar to a json output).

The developer has to create a function with the same name as the one called in the JSONP file.

in `index.html`
```html
<script>
function callbackFunction(json) {
  console.log(json);
}
</script>

<script src="jsonp.js"></script>
```

in `jsonp.js`:
```javascript
callbackFunction([
  {
    "id": 1,
    "firstName": "Mell",
    "lastName": "Yon"
  }
])
```

Because it is not an `XMLHttpRequest`, in the network Devtools tab, this does not appear under XHR category.
