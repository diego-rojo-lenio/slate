# Errors

In order to simplify the error handling all Web API responses return <code>CODE 200</code> and contain a JSON object. This object contains a top-level property <code>data</code> in most of cases.

For failure results or invalid request, data property is not present. Instead the JSON object contains an <code>error</code> property setted. This error property is an object with attributes <code>code</code> and <code>message</code>.

> A valid request returns a JSON structured like this:

```json
{
  "data": []
}
```

> A not valid request returns a JSON structured like this:

```json
{
  "error" : { "code" :  "not_found" , "message": "Requested Object not found"},
}
```

<aside class="warning">
    In case of error, don't forget to include the error property. Otherwise, MiLenio will determine that the request was valid.
</aside>

The Custom App API uses the following codes:

Http Code | Meaning
---------- | -------
200 | Normal Response Code -- Both valid and not valid request.
401 | Unauthorized -- API credentials are wrong.
5xx | Other error -- An Unknown or unhandled error.
4xx | Other error -- An Unknown or unhandled internal error in MyLenio.

