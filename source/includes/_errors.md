# Errors


## HTTP Status Codes

```
< HTTP/2 200
< date: Fri, 18 Sep 2020 19:32:04 GMT
< content-type: text/html
< set-cookie: __cfduid=d4c5a6dde91d9620ba4aac9380016f74a1600457524; expires=Sun, 18-Oct-20 19:32:04 GMT; path=/; domain=.honeypotdb.com; HttpOnly; SameSite=Lax; Secure
< last-modified: Sun, 31 May 2020 16:34:09 GMT
< vary: Accept-Encoding
< cf-cache-status: DYNAMIC
< cf-request-id: 05444cde180000cdd33f230200000001
< expect-ct: max-age=604800, report-uri="https://report-uri.cloudflare.com/cdn-cgi/beacon/expect-ct"
< server: cloudflare
< cf-ray: 5d4d7da9cb0bcdd3-CDG

```

The API will response with an appropriate HTTP status code, for example:

Error Code | Meaning
---------- | -------
200 | OK 🎉
201 | Created -- Your request to create something, has indeed been created!
202 | Accepted -- Your request to start a test as been scheduled.
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- Your request has been denied.
404 | Not Found -- The specified URL could not be found.
405 | Method Not Allowed -- You tried to access a URL with an invalid method.
406 | Not Acceptable -- Your request wasn't formatted properly.
418 | I'm a teapot.
429 | Too Many Requests -- Yooooo, chillout bro!
500 | Internal Server Error -- Something died on the server, please try again.
503 | Service Unavailable -- We're temporarily offline for maintenance, or something as died magnificently. Please try again later.

## Response error codes

```json
{
    "data": {},
    "error": 0,
    "message": "Pong!",
    "meta": {
        "items": 0,
        "meta": null
    },
    "success": true
}
```

The JSON response (If one is provided) will also contain an integer 'error' field at the JSON's array root with a corresponding error code.

The below table shows all possible API error codes. If you encounter an error code that is not in this table, please publicly shun my programming skills by tweeting the mysterious error code to @32bitbradley.

| Error Code | Meaning                                                                                               |
| ---------- | ----------------------------------------------------------------------------------------------------- |
| 0          | Success! 🎉                                                                                           |
| 1101       | The requested list is not ready yet, please try again soon.                                           |
| 1102       | The provided list type does not exist                                                                 |
| 1201       | Invalid size provided, must be between 0 and 10000                                                    |
| 1202       | The provided time range is out of bounds. Please provide a time range that is within the shown limit. |